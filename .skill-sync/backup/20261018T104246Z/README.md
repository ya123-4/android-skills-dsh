# android-skills-dsh

DeepSeek Harness skill bundle for Google's [android/skills](https://github.com/android/skills) v1.0.15.

Packs all 30 upstream skills — the exact set declared by upstream's
`.claude-plugin/plugin.json` — with every support file, byte-for-byte, as
model-invocable DSH skills. Skill-only bundle: no hooks, no native tools, no
install scripts, no Android CLI dependency.

[中文说明](README.zh.md)

## Install

```powershell
# from the DSH profile (DSH itself must not be running for an upgrade of an
# already-installed bundle; a first install can use the plugin manager)
plugin_manager install_bundle <path>\dsh-plugin-build\dist\android-skills-dsh-1.0.15.tgz
```

The bundle ships one filesystem skill provider rooted at its own `skills/`
directory; no other configuration is needed.

## Sync with upstream

This bundle is managed by `skill-sync` (registry entry `android-skills-dsh`,
`kind: github-tags`, `tagPrefix: v`). The package version follows the upstream
tag line, so `check` compares `1.0.15` against the newest `vX.Y.Z` tag:

```powershell
python dsh-plugin-build\skill-sync-dsh\skills\skill-sync\scripts\skill_sync.py check android-skills-dsh
python dsh-plugin-build\skill-sync-dsh\skills\skill-sync\scripts\skill_sync.py update android-skills-dsh
```

`update` downloads the upstream tag tarball, re-flattens the declared skill set,
re-applies the Chinese frontmatter descriptions from
`recipes/overlays/descriptions-zh.json`, verifies the result and lands it with a
rollback-safe transaction, then packs `dist/android-skills-dsh-<version>.tgz`.
An upstream change to the skill set is reported as exit code 4 and needs
`--accept-structure-change` after review.

## Bundled skills (30)

| Area | Skill | Purpose |
|---|---|---|
| Build | `agp-9-upgrade` | Upgrade or migrate a project to Android Gradle Plugin 9 |
| Camera | `camerax` | CameraX feature work, lifecycles, CameraX/ML Kit/Media3 interop |
| Cars | `android-for-cars-parked-apps` | Parked apps on Android Auto / Automotive OS |
| Device AI | `appfunctions` | Expose app workflows as AppFunctions for on-device agents |
| Device AI | `litert-runtime` | LiteRT `.tflite` inference with CompiledModel (CPU/GPU) |
| Device AI | `ml-kit-genai-prompt-api` | ML Kit GenAI Prompt API on Gemini Nano |
| Devtools | `android-cli` | Install and use the `android` CLI |
| Identity | `restore-credentials` | Restore Credentials `androidx.credentials` keys |
| Identity | `verified-email` | Verified email retrieval via Credential Manager |
| Compose | `adaptive` | Adapt UI to window size, input devices; MediaQuery, Grid, FlexBox |
| Compose | `jetpack-compose-performance` | Diagnose and optimize Compose recomposition |
| Compose | `migrate-xml-views-to-jetpack-compose` | XML View → Compose migration workflow |
| Compose | `styles` | Compose Styles API integration and theming |
| Media | `media3-cast-integration` | Google Cast with Jetpack Media3 |
| Navigation | `navigation-3` | Jetpack Navigation 3, scenes, deep links, back stacks |
| Navigation | `navigation-event` | Predictive Back via the NavigationEvent library |
| Performance | `r8-analyzer` | Analyze R8 keep rules and build config redundancies |
| Play | `engage-sdk-integration` | Play Engage SDK integration and debugging |
| Play | `play-billing-library-version-upgrade` | Upgrade Play Billing Library versions |
| Play | `play-policy-insights` | Play policy compliance audit (static analysis + declarations) |
| Profilers | `android-profiler` | Record/analyze Perfetto traces, jank, ANRs, startup |
| Security | `android-intent-security` | Intent redirection and component exposure review |
| Security | `android-permissions-security` | Permissions and IPC component security audit |
| System | `edge-to-edge` | Adaptive edge-to-edge migration and inset fixes |
| System | `glance-canonical-layouts` | Home screen widgets with Glance Canonical Layouts |
| Testing | `testing-setup` | Testing strategy, libraries and harness setup |
| TV | `leanback-to-compose-tv-migration` | Leanback/Views → Compose for TV migration |
| Wear | `wear-compose-m3` | Wear OS Compose Material 3 apps and migrations |
| Wear | `wear-widgets` | Wear OS widgets with Glance Wear / Remote Compose |
| XR | `display-glasses-with-jetpack-compose-glimmer` | Android XR display glasses with Compose Glimmer |

## DeepSeek Harness adaptation notes

- **Flattening only.** Upstream nests skills two to three levels deep; DSH
  discovers `<root>/<name>/SKILL.md` one level deep, so each skill is flattened
  to `skills/<name>/`. Names and bodies are unchanged.
- **No prose rewriting.** Unlike the mattpocock bundle, upstream's text uses no
  Claude Code tool names or slash commands, so nothing is normalised.
- **Chinese descriptions.** The profile's Chinese `description` line for each
  skill comes from skill-sync's `descriptions-zh.json` and is re-applied on every
  sync; upstream wording is kept whenever an entry is missing (reported on
  stderr).
- **Upstream install paths are not used.** `android skills add` (Android CLI) and
  the Claude Code / Codex plugin marketplaces target other harnesses; DSH loads
  this bundle directly. The upstream `android` CLI is still worth having for
  project work — see the `android-cli` skill.

## License

Skill content: Apache License 2.0, Copyright Google LLC — see [LICENSE](LICENSE)
and [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md).
