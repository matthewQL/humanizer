# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

## What this repo is
This repository is a **Claude Code plugin** that ships a single skill (also usable as a standalone Markdown skill).

The "runtime" artifact is `skills/humanizer/SKILL.md`: Claude Code reads the YAML frontmatter (metadata + allowed tools) and the prompt/instructions that follow.

`README.md` is for humans: installation, usage, and a compact overview of the patterns.

## Key files (and how they relate)
- `.claude-plugin/plugin.json`
  - Plugin manifest. Declares `name`, `description`, `version`, and other metadata used by Claude Code's plugin manager.
- `.claude-plugin/marketplace.json`
  - Marketplace catalog. Lets this repo be added directly with `/plugin marketplace add matthewQL/humanizer` so users can install with one command.
- `skills/humanizer/SKILL.md`
  - The actual skill definition.
  - Starts with YAML frontmatter (`---` … `---`) containing `name`, `version`, `description`, and `allowed-tools`.
  - After the frontmatter is the editor prompt: the canonical, detailed pattern list with examples.
- `README.md`
  - Installation and usage instructions.
  - Contains a summarized pattern table and a short version history.

When changing behavior/content, treat `skills/humanizer/SKILL.md` as the source of truth, and update `README.md` to stay consistent. The plugin and marketplace manifests intentionally omit a `version` field, so every commit counts as a new version (Claude Code uses the git SHA). When bumping the human-facing version, update it in `SKILL.md` frontmatter and the README version history only.

## Common commands
### Install as a plugin (recommended)
Inside Claude Code:
```
/plugin marketplace add matthewQL/humanizer
/plugin install humanizer@humanizer
```

### Install as a standalone skill
```bash
mkdir -p ~/.claude/skills/humanizer
cp skills/humanizer/SKILL.md ~/.claude/skills/humanizer/
```

## How to "run" it (Claude Code)
- Plugin install: `/humanizer:humanizer` then paste text
- Standalone install: `/humanizer` then paste text

## Making changes safely
### Versioning (keep in sync)
- `SKILL.md` has a `version:` field in its YAML frontmatter.
- `README.md` has a “Version History” section.

If you bump the version, update both.

### Editing `SKILL.md`
- Preserve valid YAML frontmatter formatting and indentation.
- Keep the pattern numbering stable unless you’re intentionally re-numbering (since the README table and examples reference the same numbering).

### Documenting non-obvious fixes
If you change the prompt to handle a tricky failure mode (e.g., a repeated mis-edit or an unexpected tone shift), add a short note to `README.md`’s version history describing what was fixed and why.