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

# Example: install create-pr
gh skill install ushoka/agent-skills create-pr
```

### Skills CLI

```sh
# Install a specific skill
npx skills add ushoka/agent-skills --skill <skill-name>

# Example: install create-pr
npx skills add ushoka/agent-skills --skill create-pr
```

### Agent Package Manager (APM)

```sh
# Install a specific skill
apm install ushoka/agent-skills/skills/<skill-name>

# Example: install commit
apm install ushoka/agent-skills/skills/create-pr
```

Or declare them in `apm.yml` and run `apm install`:

```yaml
name: your-project
version: 1.0.0
dependencies:
  apm:
    # Example: install create-pr
    - ushoka/agent-skills/skills/create-pr
```
