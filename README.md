# NEXUS Content Pipeline — Codex version

Codex plugin port of the [Claude Code nexus-plugin](https://github.com/<your-github-username>/nexus-plugin). Same pipeline, same rules — restructured for Codex's plugin format, since Codex doesn't use `.claude-plugin/` manifests or slash commands.

## Key differences from the Claude Code version

| | Claude Code | Codex |
|---|---|---|
| Manifest | `.claude-plugin/plugin.json` | `.codex-plugin/plugin.json` |
| Marketplace | `.claude-plugin/marketplace.json` | `.agents/plugins/marketplace.json` |
| Invocation | `/research`, `/outline`, etc. | `$research`, `$outline`, etc. (or just describe the task — Codex auto-matches skills by description) |
| Structure | `commands/` for pipeline steps, `skills/` for shared reference | Everything lives under `skills/` — Codex has no separate commands folder |

## Install

**Repo-level marketplace** (this repo):
```
codex plugin marketplace add <your-github-username>/nexus-codex-plugin
```

Or from inside the Codex TUI: run `/plugins`, then add this repo as a marketplace source.

## Pipeline

```
$research → $improve → $geo → $outline → $firstdraft → $finaldraft → [$convert] → $export-docx
```

`$humanize` also works standalone, outside the pipeline.

## Skills

- `skills/research/` — NEX-R, structured SERP analysis
- `skills/improve/` — NEX-I, intent + semantic gap + E-E-A-T analysis
- `skills/geo/` — NEX-G, query fan-out + GEO structure prescription
- `skills/outline/` — NEX-O, outline with real internal link discovery + conversion-point tagging
- `skills/firstdraft/` — NEX-F, full draft + silent humanizer pass
- `skills/finaldraft/` — NEX-Fn, final polish + compliance checks + SEO pack + schema
- `skills/humanize/` — NEX-H, standalone humanizer
- `skills/convert/` — NEX-U, site-branded CTA injection + product upsell
- `skills/export-docx/` — NEX-X, verified DOCX export with SEO metadata table, formatted article, and schema
- `skills/humanizer-id/`, `skills/humanizer-en/` — the humanizer rule sets used by firstdraft/finaldraft/humanize
- `skills/wa-cta-standard/` — CTA visual variants + color token system
- `skills/product-upsell/` — insurance/Prudential-specific upsell rules

## Not just TrueMission

Same as the Claude Code version: `$outline`'s internal link discovery and conversion-point tagging are domain-agnostic, and `$convert` asks for the target site's own colors and contact link instead of assuming TrueMission every time. `product-upsell` stays insurance-specific; other verticals get `$convert`'s generic fallback.

## A note on keeping both versions in sync

This repo is a manual port, not an auto-synced mirror of the Claude Code plugin. If you update pipeline logic in `nexus-plugin` (Claude Code), the same change needs to be copied into the matching `skills/*/SKILL.md` file here, with slash-command references (`/research`) converted to `$` syntax (`$research`).
