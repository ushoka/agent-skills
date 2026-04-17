---
name: create-pr
description: Create a GitHub pull request from the current branch. Use this skill whenever the user wants to open, create, draft, file, submit, or put up a pull request / PR from their branch — including casual phrasings like "ship this", "push this up for review", "let's PR this", "open a draft", or "create a pull request". Defaults to a draft PR; creates a normal (ready-for-review) PR only when the user explicitly asks. Handles branch push, base-branch detection, PR template population, issue linking, and pre-submission review.
---

# Create a PR

Open a pull request from the current branch using `gh`, filling the repo's PR template from the branch's commits and diff. Default to **draft** unless the user explicitly asks for a ready-for-review PR (e.g., "not a draft", "ready for review", "normal PR", "non-draft").

## Why draft by default

Drafts signal "work in progress, please don't review yet" and avoid pinging reviewers prematurely. They're cheap to promote to ready-for-review later (`gh pr ready`). Creating a non-draft PR by mistake generates noise — creating a draft by mistake does not. So the cost of over-drafting is near zero; the cost of under-drafting is real.

## High-level flow

1. **Preflight** — verify `gh` is authenticated and the branch is in a valid state to PR from.
2. **Gather context** — find the base branch, read the PR template, analyze commits and diff.
3. **Draft** — compose the title and body, filling the template sections from the actual changes.
4. **Review with user** — show the drafted title + body and let them approve or request edits.
5. **Submit** — push if needed, then `gh pr create`. Return the PR URL.

Each section below expands one of these steps.

---

## 1. Preflight checks

Run these **before** gathering context. If any fail, stop and tell the user clearly what's wrong — do not try to work around missing prerequisites.

**Check `gh` is installed and authenticated:**

```bash
gh auth status
```

If this fails, tell the user to install `gh` (`brew install gh`) or run `gh auth login`, and stop. Don't fall back to manual URL construction — the user asked for a PR, and without `gh` we can't create one.

**Check we're on a branch that makes sense to PR:**

- Get the current branch: `git branch --show-current`
- Get the repo default branch: `gh repo view --json defaultBranchRef --jq .defaultBranchRef.name`
- If the current branch **is** the default branch, refuse: tell the user to switch to a feature branch first. Do not offer to create one automatically — it's presumptuous and the user may have intended to stash or reset.

**Check for an existing PR:**

```bash
gh pr view --json url,state,isDraft 2>/dev/null
```

If a PR already exists for this branch, print its URL and stop. Do not try to update it — that can silently overwrite the user's edits. If the user wants to update, they can ask explicitly (and that's a different operation).

## 2. Gather context

### Base branch

Default to the repo's default branch (from `gh repo view` above). Only override if the user explicitly said something like "target develop" or "base off the release branch". Don't try to infer a different base from the branch name — stacked-PR detection is fiddly and wrong-guessing causes silent mistakes.

### PR template

Look for a template in this order:

1. `.github/PULL_REQUEST_TEMPLATE.md`
2. `.github/pull_request_template.md`
3. `docs/pull_request_template.md`
4. `PULL_REQUEST_TEMPLATE.md` (repo root)

If a template exists, read it and preserve its structure: section headers (`## Summary`, `## Test plan`, etc.), checkbox items, HTML comments, and ordering should all survive into the final body. The template's shape is a repo convention — respect it.

If no template exists, use this minimal default:

```markdown
## Summary

<one-paragraph or 2-3 bullets describing the change>

## Test plan

- [ ] <how this was verified>
```

### Git analysis

Gather the raw material for the body in parallel:

```bash
git fetch origin <base-branch> --quiet
git log origin/<base-branch>..HEAD --pretty=format:"%h %s%n%b" --no-merges
git diff origin/<base-branch>...HEAD --stat
git diff origin/<base-branch>...HEAD --name-only
```

The three-dot range (`base...HEAD`) shows what's on the branch that isn't on the base — the same diff GitHub will display. Don't just look at the most recent commit; a branch often has several commits that together tell a single story.

### Issue references

Scan the branch name **and** commit messages for issue identifiers:

- GitHub-style: `#123`
- JIRA-style: `ABC-456` (only if a JIRA key pattern appears — don't match random uppercase sequences)

If found, add a `Closes #123` line near the top of the body (below the Summary). For JIRA keys, include them as-is (`Relates to ABC-456`) — `Closes` only works for GitHub issues. If nothing is found, skip this — don't ask.

## 3. Draft the title and body

### Title

One line, imperative mood, under ~70 chars. Summarize what the branch accomplishes as a whole, not just the latest commit. If the branch has one commit, its subject is usually a fine starting point. If it has many, synthesize.

Good: `fix: avoid race in session refresh`
Bad: `Fixed the thing` · `WIP` · `Update auth.ts and 3 other files`

Follow the repo's existing commit/PR style (look at recent PRs with `gh pr list --state merged --limit 5` if unsure about convention — but only if it's genuinely ambiguous; don't belabor this).

### Body

Fill each section of the template from the git analysis:

- **Summary / Overview / What changed**: 2-4 bullets or a short paragraph grounded in the actual diff. Lead with the *why* (what problem does this solve), then the *what*.
- **Test plan / How was this tested**: Derive from test files changed, new test commands visible in CI config, or commit messages mentioning tests. If there's no evidence, write a checklist of things the user probably did (or should do) and leave it as unchecked boxes — don't fabricate a test plan.
- **Screenshots / Demo**: Leave placeholders like `<screenshot>` for the user to fill. Don't invent.
- **Unknown/custom sections**: Best-effort one-line draft based on the changes, or leave the section heading with a brief placeholder. Never delete a section.

### Checkboxes

Check an item only when there's concrete evidence in the diff:

- "Added tests" / "Tests added" → check if any test file was added or modified (`*test*`, `*spec*`, `__tests__/`, etc.)
- "Updated docs" / "Documentation" → check if any `.md` or `docs/` file changed
- "No breaking changes" → leave unchecked (you can't prove a negative from the diff)
- "Ran locally" / "CI passes" → leave unchecked (no evidence either way)

When in doubt, leave unchecked. False positives are worse than omissions — an unchecked box prompts the user to think; a wrongly-checked box misleads reviewers.

### Issue links

Place `Closes #N` or `Relates to ABC-456` on its own line directly under the Summary section (or at the top of the body if no Summary section exists). GitHub auto-links and auto-closes on merge.

## 4. Review with the user

Print the drafted title and body in the chat in a clearly delimited block:

```
---
Title: <title>

Body:
<body>
---

Ready to create this as a draft PR against <base-branch>? (Reply 'ready' for a non-draft PR.)
```

Wait for the user's response. If they ask for edits, apply them and re-show. If they say "ready" / "not a draft" / "normal PR", switch to non-draft mode. If they approve, proceed.

This review step is load-bearing — PR bodies are public-ish artifacts that teammates see, and it's much cheaper to edit in chat than on GitHub after the fact.

## 5. Submit

### Push if needed

Check upstream state:

```bash
git rev-parse --abbrev-ref --symbolic-full-name @{u} 2>/dev/null
git status -sb
```

If the branch has no upstream, or is ahead of its upstream, push:

```bash
git push -u origin <current-branch>
```

If the push fails (e.g., non-fast-forward), surface the error — don't force-push. The user might have rebased and need to decide.

### Create the PR

Pass the body via heredoc to preserve formatting:

```bash
gh pr create \
  --base <base-branch> \
  --title "<title>" \
  --draft \
  --body "$(cat <<'EOF'
<body>
EOF
)"
```

**Flags:**
- `--draft` — omit only when the user explicitly asked for a non-draft PR.
- `--base` — always pass explicitly, even if it's the default branch; makes the command self-documenting.
- `--reviewer`, `--assignee`, `--label`, `--milestone` — only if the user explicitly requested them. Don't read CODEOWNERS or invent reviewers.

Return the PR URL from the command output to the user. Done.

---

## Examples

### Example 1: Simple branch with template

**Situation:** Branch `fix/session-timeout` with 2 commits, base branch `main`, template has Summary / Test plan / Checklist sections. Changed files include `src/auth.ts` and `src/auth.test.ts`.

**Draft title:** `fix: handle session timeout during token refresh`

**Draft body:**
```markdown
## Summary

Closes #847

- Token refresh was racing with session expiry, causing intermittent 401s
- Added a mutex around the refresh path and a regression test

## Test plan

- [x] Added unit tests covering concurrent refresh
- [ ] Verified in staging

## Checklist

- [x] Tests added
- [ ] Docs updated
- [ ] Breaking changes noted
```

### Example 2: No template, single commit

**Situation:** Branch `chore/bump-deps`, one commit `chore: bump lodash to 4.17.21`, no template.

**Draft title:** `chore: bump lodash to 4.17.21`

**Draft body:**
```markdown
## Summary

Bumps lodash from 4.17.20 to 4.17.21 to pick up the prototype pollution fix (CVE-2021-23337).

## Test plan

- [ ] CI passes
```

### Example 3: User says "not a draft"

User: `ship this, not a draft`

→ Skip `--draft` in `gh pr create`. Everything else identical.

---

## Edge cases and what to do

- **Dirty working tree**: Don't commit on the user's behalf. Tell them there are uncommitted changes and ask if they want to stash, commit, or proceed anyway (PR will reflect only committed state).
- **Branch is behind base**: Proceed — `gh pr create` will work; reviewers will see the stale state. Mention it so the user can rebase if they care.
- **Multiple templates under `.github/PULL_REQUEST_TEMPLATE/`**: Pick one based on branch name prefix (`feature/` → feature template, `fix/`/`bug/` → bug template) if the names line up obviously; otherwise ask the user which template to use.
- **No remote on `origin`**: Error clearly. Don't try to guess another remote — the user likely has a multi-remote setup that needs explicit handling.
- **Fork workflow (PR from fork to upstream)**: `gh pr create` handles this natively when the repo is a fork; no extra flags needed. Just make sure `--base` is set to the upstream default branch.
