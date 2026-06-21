# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A **documentation-only AI skill** — no build system, no tests, no runtime code. All files are plain Markdown. The deliverable is the content of `flatsome/SKILL.md` and `flatsome/references/*.md`, which are consumed verbatim by AI coding tools to give them Flatsome/UX Builder expertise.

## File layout

```
flatsome/
  SKILL.md                  Entry point: output contract, mental model, quick reference
  references/
    grid-system.md          section/row/col grid, breakpoints, shortcode→HTML mapping
    shortcodes.md           Full element reference with parameters
    hooks.md                67 action hooks + 67 filters with recipes
    woocommerce.md          Product grids, single-product elements, custom product pages
    ux-builder.md           Builder workflow, Blocks, header builder, HTML structure
    customization.md        Theme Options, Custom CSS, custom fonts, child theme
    snippets.md             Copy-paste PHP/JS snippets
```

No other directories or source files exist. `README.md` and `flatsome/README.md` are identical (installation instructions for end users).

## Editing guidelines

- All content must be accurate against the **official Flatsome docs** at `https://docs.uxthemes.com/`. Never invent shortcode attributes or hook names.
- Trust levels must be maintained as documented in `SKILL.md` (verbatim-from-docs vs. compiled-from-conventions vs. builder-verified).
- The **OUTPUT CONTRACT** section in `SKILL.md` is the most critical section — any edit there must preserve the rule that layouts must be UX Builder shortcodes, never raw HTML.
- When adding new shortcode attributes, verify via the builder (build element → save → copy from WordPress code editor) rather than guessing.
- Responsive variants use `__sm` (mobile) and `__md` (tablet) suffixes — this naming convention must be consistent across all reference files.

## How the skill is consumed

- **Claude Code:** place `flatsome/` under `~/.claude/skills/flatsome/` (global) or `<project>/.claude/skills/flatsome/` (per-project). Auto-triggers on Flatsome/UX Builder tasks.
- **Cursor:** `.cursor/rules/flatsome.mdc` points agent at the files.
- **Codex/opencode:** pointer in `AGENTS.md`.
- **Copilot:** pointer in `.github/copilot-instructions.md`.
- **Other tools:** attach/paste `flatsome/SKILL.md` + relevant `references/*.md` as context.

## Key Flatsome concepts (for editing reference files accurately)

- All layouts are stored as **shortcodes** in WordPress `post_content`; UX Builder is a visual editor on top of this shortcode language.
- Structural backbone: `[section]` → `[row]` → `[col]` (12-column grid). Content elements go inside `[col]`.
- `[col]` spans must sum to 12 per row. Always set `span__sm="12"` on multi-column rows.
- Reusable layouts are stored as **UX Blocks** (a custom post type), inserted via `[block id="..."]`.
- PHP customizations go in a **child theme** `functions.php` via hooks/filters — never edit theme core.
- CSS goes in **Theme Options → Custom CSS**; JS in **Theme Options → Advanced → Custom Scripts**.
- After any CSS/PHP change: clear Flatsome's CSS cache (Theme Options → Advanced → Regenerate CSS) and any caching plugin.
