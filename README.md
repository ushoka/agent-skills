# agent-skills

Personal collection of agent skills.

## Skills

- [`commit`](skills/commit/) - Break working changes into smaller, semantic commits with Conventional Commits messages.
- [`create-pr`](skills/create-pr/) - Create a GitHub pull request from the current branch, including a drafted title and body.

## Usage

Each skill lives in its own directory with a `SKILL.md` at the root.

### GitHub CLI

```sh
# Install a specific skill
gh skill install ushoka/agent-skills <skill-name>

# Example: install commit for Cursor at user scope
gh skill install ushoka/agent-skills commit --agent cursor --scope user
```

### Skills CLI

```sh
# Install a specific skill
npx skills add ushoka/agent-skills --skill <skill-name>

# Example: install commit
npx skills add ushoka/agent-skills --skill commit
```

To install all skills from the repo, omit `--skill`:

```sh
npx skills add ushoka/agent-skills
```

### Agent Package Manager (APM)

```sh
# Install a specific skill
apm install ushoka/agent-skills/skills/<skill-name>

# Example: install commit
apm install ushoka/agent-skills/skills/commit
```

Or declare them in `apm.yml` and run `apm install`:

```yaml
name: your-project
version: 1.0.0
dependencies:
  apm:
    # Example: install commit
    - ushoka/agent-skills/skills/commit
```
