---
name: commit
description: Break working changes into smaller, semantic commits with Conventional Commits messages so the history reads cleanly. Use this skill whenever the user asks to commit working changes — "commit this", "commit these changes", "make a commit", "split this into commits", "commit my work" — even when they don't explicitly ask for a split. The default behavior is to propose a multi-commit plan, get approval, then execute; if there's only one sensible commit, say so and proceed automatically. Do not use for push, PR creation, rebase, or history rewriting beyond the initial commits.
---

# Commit

Turn a messy working tree into a tidy sequence of small, semantic commits that a reviewer can actually follow.

## Why this exists

One commit that touches ten things is unreviewable. `git bisect` can't find anything. The PR description ends up being a pile of bullets. Breaking the same diff into a handful of focused commits — each one a single idea — makes review, blame, and bisect all work the way they're supposed to.

The user wants the *output* (clean history), not a lecture on commit hygiene. Be decisive, show the plan, ship the commits.

## The flow

1. **Survey** the working tree.
2. **Group** changes by logical concern.
3. **Order** commits so each one makes sense on top of the last.
4. **Propose** multi-commit plans and wait for approval when needed.
5. **Execute** one commit at a time, using `git add -p` when a file spans concerns.
6. **Report** what happened.

### 1. Survey

Run these in parallel to understand the full state:

- `git status` — what's modified, added, untracked
- `git diff` — unstaged changes
- `git diff --staged` — already-staged changes
- `git log -n 10 --oneline` — recent commit style for reference

Read the diffs. You can't split what you don't understand.

### 2. Group by logical concern

A "concern" is one reviewable idea: a feature, a bug fix, a refactor, a test addition, a doc update, a dependency bump. Group changes that share a purpose and would reasonably be reverted together.

**A single file can contribute to multiple commits.** If `auth.ts` has both the new JWT feature and an unrelated whitespace cleanup, those go in different commits — use `git add -p` to stage hunks selectively.

**Generated files travel with their cause.** `package.json` + `package-lock.json` together. Generated types with the feature that regenerated them. Don't split mechanical byproducts from the change that produced them.

**When there's genuinely only one concern**, say so and make one well-formed commit. Don't invent splits.

### 3. Order the commits

Foundations first, so each commit ideally builds and makes sense standalone:

1. `chore` / dependency updates that later commits rely on
2. `refactor` — preparatory cleanup that doesn't change behavior
3. `fix` / `feat` — the behavior changes themselves
4. `test` — tests for the above
5. `docs` — documentation updates

This isn't a rigid template — respect actual code dependencies. If a test file imports something introduced in a `feat` commit, the test commit must come after. Mentally walk through: "if I checked out each commit in order, would the repo be in a sensible state?"

### 4. Propose the plan when needed

If you found **multiple commits**, before touching staging present a numbered plan:

```
Proposed commits:

1. refactor: extract token parser into util
   src/auth/token.ts, src/auth/index.ts

2. feat: add JWT-based authentication
   src/auth/jwt.ts, src/auth/index.ts, src/middleware.ts, package.json, package-lock.json

3. test: cover JWT auth flow
   tests/auth.test.ts

4. docs: document auth config
   README.md
```

Then ask: "Proceed with these commits, or adjust?"

Wait for approval. If the user asks for edits (merge two, rename a message, move a file), revise and re-show before proceeding.

If you found **exactly one sensible commit**, don't stop for approval just to ask permission for a no-op choice. Briefly state that the working tree is one coherent concern, show the commit you plan to make, and proceed directly to execution.

Example:

```
One coherent concern; proceeding with a single commit:

1. fix: handle timezone offset in date comparison
   src/date.ts, src/date-utils.ts, tests/date.test.ts
```

### 5. Execute

Once approved, or immediately for the single-commit path:

1. **Reset staging** so you start from a known state: `git reset` (not `--hard` — preserves working tree).
2. For each commit in order:
   - Stage only what belongs in this commit:
     - Whole-file staging: `git add <path>`
     - Partial-hunk staging (file spans concerns): `git add -p <path>` — select only the relevant hunks
     - New untracked files: `git add <path>` (or `git add -N` then `-p` if you need to split additions)
   - Verify staging matches intent: `git diff --staged --stat`
   - Commit with a HEREDOC message:
     ```
     git commit -m "$(cat <<'EOF'
     feat: add JWT-based authentication
     EOF
     )"
     ```
   - If the message genuinely needs a body (non-obvious *why* — a bug context, a constraint, a deliberate trade-off), include it. Default to subject-only.

Never use `--amend`, `--no-verify`, `--no-gpg-sign`, or force operations. Let hooks run normally.

### 6. If a pre-commit hook fails

Stop. The commit didn't happen.

- Do **not** `--amend` — that would modify the previous commit, not the one that just failed.
- Do **not** `--no-verify` — the hook is telling you something real.
- Do **not** continue to the next planned commit — subsequent commits probably depend on this one landing.

Report the failure, show the hook output, leave staging as-is (so the user can inspect what was about to be committed), and ask how to proceed. Commits already made stay — they're fine.

### 7. Report

When finished, summarize: how many commits made, which ones, and anything the user should know (e.g., "commits 2 and 3 touched the same file via partial hunks").

## Commit message format

Conventional Commits: `<type>: <description>`

Types: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`, `perf`, `ci`

- Imperative mood: "add X", not "added X" or "adds X"
- Lowercase after the colon
- No trailing period
- Subject under ~72 chars

**Subject-only by default.** Add a body only when the *why* would surprise a future reader:

```
fix: guard against null session in refresh handler

The refresh endpoint was 500'ing when the session cookie was
absent but the refresh cookie was present — a state only
reachable from the mobile app's background-refresh path.
```

If the diff speaks for itself, no body.

## Scope: what counts as "working changes"

Everything dirty: staged, unstaged, and untracked. The whole point is to produce a clean history from a messy tree, so take it all into account. The staging reset in step 5 handles this uniformly.

Do not touch files outside the working tree (stashes, other branches, remote state).

## Boundaries

- **Do not push.** Commits land locally. Pushing is the user's separate decision.
- **Do not open a PR.** Same reason.
- **Do not rebase, reorder existing commits, or rewrite history beyond the commits you're making now.**
- **Do not run tests, typecheck, or lint between commits.** Pre-commit hooks handle their own checks; intermediate commits that don't pass CI are fine — the series as a whole is what ships.
- **Do not add `Co-Authored-By` footers** unless the user's git config or project conventions already use them. (User has this disabled globally.)

## Examples

**Example 1 — mixed diff**

Working tree: new auth feature across 3 files, a typo fix in README, and a lockfile bump from adding `jsonwebtoken`.

Plan:
```
1. feat: add JWT authentication
   src/auth.ts, src/middleware.ts, package.json, package-lock.json
2. docs: fix typo in setup instructions
   README.md
```

The typo is unrelated to auth → its own commit. Lockfile travels with the `package.json` change that caused it.

**Example 2 — one file, two concerns**

Working tree: `src/user.ts` has both a new `getDisplayName()` method (feature) and reformatted imports at the top (cleanup).

Plan:
```
1. refactor: reorder imports in user module
   src/user.ts (import block only)
2. feat: add display name helper for users
   src/user.ts (new method only)
```

Execute with `git add -p src/user.ts`, selecting only the import hunks for commit 1, then the method hunks for commit 2.

**Example 3 — genuinely single concern**

Working tree: three files all implementing the same bug fix.

Plan:
```
1. fix: handle timezone offset in date comparison
   src/date.ts, src/date-utils.ts, tests/date.test.ts
```

One commit. Don't fragment a coherent change just to produce more commits.
