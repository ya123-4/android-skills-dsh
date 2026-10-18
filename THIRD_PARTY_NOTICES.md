# Third-Party Notices

The skill content in this package is copied unmodified from:

- Project: Android Skills
- Repository: https://github.com/android/skills (tag `v1.0.15`)
- License: Apache License 2.0
- Copyright: Copyright Google LLC

The Apache License 2.0 text is reproduced in `LICENSE` at the package root.

## What is copied

Every file under each skill directory declared by upstream's
`.claude-plugin/plugin.json` `skills[]` is copied byte-for-byte, support files
included (`references/`, `resources/`, `scripts/`, sample `.kt` / `.xml` / `.pftxt`
assets). The one exception is the frontmatter `description` line of each
`SKILL.md`, which skill-sync rewrites to the profile's Chinese wording (from its
`descriptions-zh.json` overlay); skill bodies, all other frontmatter, and every
support file are unchanged.

## What is not bundled

- `.claude-plugin/`, `.codex-plugin/`, `.agents/` — Claude Code / Codex / agents
  marketplace manifests, which target other harnesses.
- `.github/` — upstream CI workflows (release packaging, skill sync).
- The repository root `README.md` — upstream installation instructions for the
  Android CLI are replaced by this bundle's own README.

## Structural adaptation

DSH discovers skills only one level deep (`<root>/<name>/SKILL.md`). Upstream
nests its skills two to three levels deep (`build-system/agp/agp-9-upgrade`,
`jetpack-compose/theming/styles`, `device-ai/litert/litert-runtime`), so the
build flattens each skill to `skills/<name>/` while every sibling support file
comes along. The skill `name` in each `SKILL.md` already equals that directory
name, so flattening renames nothing.
