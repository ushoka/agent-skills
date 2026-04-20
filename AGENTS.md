## AGENTS.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A personal collection of **agent skills** — self-contained directories each holding a `SKILL.md` that teaches an AI agent how to perform one task well. No application code, no build system, no test runner. The "product" is the prose in each `SKILL.md`.

Published skills live under [skills/](skills/). Do not hardcode the current inventory in this file; when you need to know what skills exist, inspect `skills/*/SKILL.md` or [README.md](README.md).

## Repository layout

- One directory per skill under [skills/](skills/).
- Each skill is **self-contained** — it should be installable on its own via `gh skill install <owner>/<repo> <skill-name>` or `npx skills add <owner>/<repo> --skill <skill-name>` (see [README.md](README.md)). Don't create cross-skill imports or shared helpers at the repo root.

## Creating or editing skills

Whenever the user wants to create a new skill or substantially edit an existing one, use the `skill-creator` skill first. That skill owns the authoring workflow and quality bar.
Whenever the published skill inventory changes under `skills/`, update the `README.md` `## Skills` section in the same task so the public list stays in sync.

## Publishing after pushing skill changes

After pushing to GitHub, check whether `skills/` changed since the latest tag on the remote repository, not just the latest local tag. Fetch remote tags first if needed, then compare `skills/` against the latest tag reachable on `origin` (for example, by diffing from `origin`'s latest release tag to `HEAD`). Only if that diff is non-empty, run `gh skill publish` from the repository root so skills are validated against the spec and published via a GitHub release. When publishing, automatically choose the next semantic version tag unless the user explicitly asked for a specific version: bump `PATCH` for wording or metadata-only fixes, `MINOR` for backward-compatible skill additions or expansions, and `MAJOR` for breaking behavior changes, removals, or renames. If you want a non-interactive publish, pass that next version explicitly with `gh skill publish --tag vX.Y.Z`. If validation reports fixable metadata issues, re-run with `gh skill publish --fix` before publishing. This step is part of the same workflow as committing and pushing skill edits—do not skip it when finishing a skill change that has gone to the remote.

Requires GitHub CLI **v2.90.0** or later (`gh skill` is documented in the [GitHub CLI agent skills changelog](https://github.blog/changelog/2026-04-16-manage-agent-skills-with-github-cli/)).

Keep this file focused on repository-level context rather than duplicating skill-writing instructions.
