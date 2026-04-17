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

Keep this file focused on repository-level context rather than duplicating skill-writing instructions.
