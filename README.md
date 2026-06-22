# Flatsome AI Coding Skill

A reusable **AI coding skill** packaging complete knowledge of the **Flatsome** WordPress
theme (by UX Themes) + **UX Builder**, for AI-assisted ("vibe coding") development across
projects. Distilled from UX Themes' official Flatsome documentation.

It's plain Markdown, so it works with **any** AI coding tool — **Claude Code, Cursor, Codex,
opencode, Cline, Windsurf, GitHub Copilot, OpenClaw**, or any agent/chat assistant you can give
context to.

## What it does

When a project uses Flatsome / UX Builder, this skill makes your AI assistant:
- Generate layouts as **UX Builder shortcodes** (`[section]/[row]/[col]`, `[ux_banner]`,
  `[ux_products]`, `[blog_posts]`, …) — **not** plain HTML/Bootstrap/Tailwind.
- Use the correct **12-column responsive grid** with `span__sm`/`span__md` fallbacks.
- Customize behavior via Flatsome **hooks/filters** in a child theme, not core edits.
- Know the standard **rendered HTML classes** (`.col.medium-6.small-12`, `.header-main`,
  `.nav-dropdown`, `.absolute-footer`, …) so Custom CSS targets the right elements.

## Contents

| File | What's inside |
|---|---|
| `SKILL.md` | Entry point: mental model, **OUTPUT CONTRACT** (shortcodes not HTML), quick reference, conventions, accuracy/provenance |
| `references/grid-system.md` | `section/row/col` grid, breakpoints, shortcode→HTML mapping, layout recipes |
| `references/html-to-uxbuilder.md` | Playbook: analyze an HTML/mockup design → map to shortcodes → emit → verify; render gotchas |
| `references/shortcodes.md` | Full element reference w/ parameters (text, image, banner, button, slider, lightbox, blog_posts, …) |
| `references/hooks.md` | All 67 action hooks + 67 filters (verbatim) with recipes |
| `references/woocommerce.md` | Product grids, single-product elements, 7 custom product-page layouts |
| `references/ux-builder.md` | Builder workflow, Blocks, header builder, mega menu, **theme-region HTML structure** (header/menu/footer/sidebar) |
| `references/customization.md` | Theme Options, Custom CSS, custom fonts, child theme, debug/cache |
| `references/snippets.md` | Copy-paste PHP/JS snippets (payment icons, share/follow links, CPT, sticky cart, pjax, …) |

## Install

Clone the repo:

```bash
git clone https://github.com/nqchange96/flatsome-ai-coding-skill.git
```

The skill is plain Markdown (`flatsome/SKILL.md` + `flatsome/references/*.md`), so any AI coding
tool can consume it. Wire it up the way your tool expects:

### Claude Code
Copy the `flatsome/` folder into the skills dir — it then **auto-triggers** when a task mentions
Flatsome, UX Builder, or Flatsome shortcodes:
- **Global (all projects):** `~/.claude/skills/flatsome/`
  (Windows: `C:\Users\<you>\.claude\skills\flatsome\`)
- **Per project:** `<project>/.claude/skills/flatsome/`

### Cursor
Copy `flatsome/` into your project, then add a Project Rule at `.cursor/rules/flatsome.mdc`
that points the agent to the skill, e.g.:

```mdc
---
description: Flatsome / UX Builder knowledge
globs: ["**/*.php", "**/*.html"]
alwaysApply: false
---
When building Flatsome or UX Builder layouts, read @flatsome/SKILL.md and the files in
@flatsome/references/, and output UX Builder shortcodes (not raw HTML/Bootstrap/Tailwind).
```

### Codex / opencode / other AGENTS.md agents
Copy `flatsome/` into your project and add a pointer in `AGENTS.md` at the repo root:

```md
## Flatsome
For any Flatsome / UX Builder work, read `flatsome/SKILL.md` and `flatsome/references/*.md`
first, and output UX Builder shortcodes (not raw HTML).
```

### GitHub Copilot
Reference the skill from `.github/copilot-instructions.md` (point it at `flatsome/SKILL.md`
and the `references/` files).

### OpenClaw
OpenClaw (a local general-purpose AI agent) has a Skills System, persistent memory, and GitHub
access. The knowledge here transfers as-is — only OpenClaw's native skill *wrapper/metadata*
differs from Claude Code's. Wire it up any of these ways:
- **Via GitHub (simplest):** give OpenClaw access to this repo and tell it to read
  `flatsome/SKILL.md` and `flatsome/references/*.md` before any Flatsome task.
- **Via memory:** have it remember "for Flatsome work, follow `flatsome/SKILL.md` and output
  UX Builder shortcodes, not raw HTML."
- **As a native skill:** repackage this Markdown into OpenClaw's own skill format (the content
  stays identical; only the wrapper changes).

### Any other agent / chat assistant
Just attach or paste `flatsome/SKILL.md` (plus the relevant `references/*.md`) as context
before asking for Flatsome work.

**The trigger is the same everywhere:** when the task touches Flatsome, UX Builder, or its
shortcodes, point the assistant at this knowledge.

## How to use

Just ask for Flatsome work, e.g.:
- "Build a Flatsome homepage hero + 3 feature columns"
- "Create a custom product page layout with a left sidebar"
- "Add a free-shipping bar above the header via a hook"
- "Write Custom CSS to restyle the menu dropdown"

Output will be valid UX Builder shortcodes you can paste into the WordPress code editor, a UX
Block, or a header element — then reopen in UX Builder to tweak visually.

## Accuracy & provenance

- **Verbatim from docs (100%):** hook lists, the 7 custom product layouts, lightbox/CSS/fonts/
  mega-menu/custom-product workflows, `ux_product_*` element list.
- **Compiled from Flatsome conventions + doc examples:** per-element attribute tables (the
  official docs have no single exhaustive shortcode reference — they teach via the builder/video).
- **Verified against live render:** shortcode→HTML class mapping and header/menu/footer/sidebar
  structure reflect Flatsome's standard output (theme-standard classes only; no project data).
- **Ground truth for exact attributes:** build the element once in UX Builder, save, open the
  page in the WordPress code editor, and copy the generated shortcode.

This skill is intentionally **generic** — it contains no data from any specific website. Sample
sites are used only to verify that shortcodes render to correct standard HTML.

## Author

Made by **chuyenmkt** — [chuyenmkt.com](https://chuyenmkt.com)

Official Flatsome docs: https://docs.uxthemes.com/
