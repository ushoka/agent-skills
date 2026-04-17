# agent-skills

Personal collection of agent skills.

## Skills

- [`commit`](skills/commit/) - Break working changes into smaller, semantic commits with Conventional Commits messages.
- [`create-pr`](skills/create-pr/) - Create a GitHub pull request from the current branch, including a drafted title and body.

## Usage

Each skill lives in its own directory with a `SKILL.md` at the root.

To install a skill from a published skills repository, use either GitHub CLI or the Skills CLI:

```sh
# Install a specific skill with GitHub CLI
gh skill install ushoka/agent-skills <skill-name>

# Example: install commit for Cursor at user scope
gh skill install ushoka/agent-skills commit --agent cursor --scope user
```

```sh
# Install from a repo with the Skills CLI
npx skills add ushoka/agent-skills

# Install a specific skill from a multi-skill repo
npx skills add ushoka/agent-skills --skill commit
```
