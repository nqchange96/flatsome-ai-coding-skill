---
name: flatsome
description: >
  Comprehensive knowledge for building and customizing WordPress sites with the
  Flatsome theme by UX Themes. Use this skill whenever a project uses Flatsome,
  UX Builder, or mentions Flatsome shortcodes (ux_banner, ux_image, row/col/section,
  ux_products, etc.), Flatsome theme hooks/filters, UX Blocks, custom product pages,
  the Flatsome header builder, WooCommerce styling with Flatsome, or "vibe coding"
  a Flatsome project. Covers shortcode syntax & parameters, the responsive grid,
  UX Builder workflow, theme hooks (actions/filters), child-theme development,
  custom CSS, custom fonts, mega menus, custom post types, and WooCommerce.
license: Knowledge compiled from docs.uxthemes.com (official Flatsome documentation).
---

# Flatsome Skill

Flatsome is the most popular WooCommerce theme on ThemeForest, built by **UX Themes**. Its
signature feature is **UX Builder**, a live front-end page builder that stores layouts as
**shortcodes** in `post_content`. This means you can author any Flatsome page/section as plain
text shortcodes — making it ideal for AI-assisted ("vibe coding") workflows.

## ⚠️ OUTPUT CONTRACT — read before generating any layout

When generating page/section/product content for a Flatsome project, you **MUST output
UX Builder shortcodes**, not raw HTML. This is non-negotiable for Flatsome compatibility.

1. **Structure with the grid, not `<div>`s.** Every layout starts from
   `[section] → [row] → [col]` (or `[ux_banner]`, `[ux_slider]` as top-level blocks). Never emit
   `<div class="row">`, `<section>`, Bootstrap/Tailwind markup, or hand-rolled CSS grid for
   structural layout. Use Flatsome elements: `[ux_image]`, `[ux_text]`, `[title]`, `[button]`,
   `[ux_banner]`, `[ux_image_box]`, `[featured_box]`, `[ux_products]`, etc.
2. **Raw HTML only inside `[ux_text]` or `[ux_html]`.** Inline tags like `<strong>`, `<a>`,
   `<br>`, lists are fine *inside* a text element. Anything structural goes in shortcodes. If a
   widget genuinely has no Flatsome equivalent, wrap it in `[ux_html]...[/ux_html]`.
3. **Always add responsive fallbacks.** Multi-column rows need `span__sm="12"` on every `[col]`;
   add `__sm`/`__md` variants for alignment, padding, height, visibility where it matters.
4. **Where the output goes:** paste into the WordPress **code editor** of a page/post, a **UX
   Block** (`[block id="..."]`), a widget, or a header **Text/HTML** element — then it can be
   reopened and tweaked in UX Builder visually.
5. **Images:** prefer attachment IDs (`[ux_image id="123"]`). If you only have a URL, say so and
   use the element's URL attribute or `[ux_html]` with an `<img>`.
6. **Parameter uncertainty:** if unsure of an element's exact attribute, use the documented
   safe attributes from [references/shortcodes.md](references/shortcodes.md) and tell the user
   the ground-truth way to get exact values is: build it once in UX Builder → copy the generated
   shortcode (see "Generating shortcodes from the builder" in that file).

**Self-check before returning layout code:** Does the output start with a Flatsome shortcode
(`[section]`/`[row]`/`[ux_banner]`/...)? Are there any structural `<div>`s outside `[ux_html]`?
Does every `[col]` in a multi-column row have `span__sm`? If any answer is wrong, fix it before
responding.

## Mental model (read this first)

1. **Everything is a shortcode.** UX Builder is a visual editor on top of a shortcode language.
   Any layout you can build visually can be written by hand as shortcodes and pasted into the
   WordPress code editor, a UX Block, a text widget, or anywhere `do_shortcode()` runs.
2. **Layout = nested grid.** The structural backbone is `[section]` → `[row]` → `[col]`.
   Rows are 12-column grids. Content elements (`[ux_image]`, `[button]`, `[ux_banner]`, ...) go
   inside columns.
3. **Responsive by suffix.** Most attributes accept breakpoint suffixes: `__sm` (mobile),
   `__md` (tablet). e.g. `span="4" span__sm="12"` = 4 cols on desktop, full width on mobile.
4. **Customize via hooks & child theme, never edit core.** Flatsome exposes ~67 action hooks
   and ~67 filters. Put PHP in a **child theme** `functions.php`, CSS in **Theme Options →
   Custom CSS** (or `Advanced > Custom CSS` on an element), JS in Theme Options → Advanced.
5. **Reusable content = UX Blocks.** The `Blocks` custom post type stores reusable layouts
   (mega menus, custom product pages, footers, popups) inserted via `[block id="..."]`.

## How to apply this skill

- **Generating a page/section:** Write shortcodes directly. Start from the grid
  (`section > row > col`), then place elements. Always add `__sm` fallbacks for mobile.
  See [references/grid-system.md](references/grid-system.md) and
  [references/shortcodes.md](references/shortcodes.md).
- **Custom product page (WooCommerce):** Build a UX Block ("CPL - ...") and assign it in
  Theme Options → Shop → Product Page, or per-category/per-product. Use the `ux_product_*`
  shortcodes. See [references/woocommerce.md](references/woocommerce.md).
- **Behavior/PHP customization:** Find the right hook or filter in
  [references/hooks.md](references/hooks.md), add code to child theme `functions.php`.
- **Styling:** Prefer Theme Options (colors, typography, header) → then Custom CSS. See
  [references/customization.md](references/customization.md).
- **Builder workflow / blocks / generating reusable shortcodes:** See
  [references/ux-builder.md](references/ux-builder.md).
- **Ready-made code snippets** (payment icons, share/follow links, pjax, infinite scroll,
  enabling UX Builder on CPTs): [references/snippets.md](references/snippets.md).

## Quick reference — the elements you'll use constantly

```
[section] ... [/section]          Full-width container (bg, padding, height, video)
[row] ... [/row]                  12-column grid wrapper
[col span="6" span__sm="12"]...   A column (span 1–12); span__sm for mobile
[row_inner]/[col_inner]           Nested grid (use inside a col)
[gap height="30px"]               Vertical spacing
[divider]                         Horizontal rule

[ux_banner]                       Hero/banner with bg image/video + overlay text
[ux_image id="123"]               Image (use attachment ID)
[ux_text] ... [/ux_text]          Rich text block
[button text="Buy" link="#"]      Button
[title text="Heading"]            Styled section heading
[ux_slider] ... [/ux_slider]      Slider/carousel (wrap slides/images)
[ux_products]                     Product grid (WooCommerce)
[ux_product_categories]           Category grid
[featured_box]/[ux_image_box]     Icon/image feature box
[lightbox id="x"] ... [/lightbox] Modal popup (trigger with link="#x")
[block id="123"]                  Insert a reusable UX Block
[ux_sidebar id="..."]             Widget area
```

Full parameter tables for every element are in
[references/shortcodes.md](references/shortcodes.md).

## Critical conventions & gotchas

- **Grid math:** `[col]` spans must sum to 12 per row (or they wrap). Default span is 12.
- **Mobile first fallback:** ALWAYS set `span__sm="12"` on multi-column rows or the layout
  breaks on phones. Likewise use `text_align`, `padding`, `visibility` responsive variants.
- **Image references use attachment IDs** (`ux_image id="123"`), not URLs, when generated by
  the builder. You can also use external URLs in some elements via dedicated attributes.
- **Color attributes** accept hex (`bg_color="#000000"`) or rgba
  (`bg_color="rgba(0,0,0,0.5)"`). `text_color` is often `"light"` or `"dark"` for banners.
- **Don't hand-edit shortcodes you'll re-open in UX Builder** unless valid — malformed
  shortcodes can make the builder fail to load that element.
- **Child theme is mandatory** for any PHP/template change so updates don't wipe it.
- **Where PHP goes:** child theme `functions.php`. **Where CSS goes:** Theme Options → Style →
  Custom CSS (global) or element's `Advanced > CSS class` + Custom CSS. **Where JS goes:**
  Theme Options → Advanced → Custom Scripts (header/footer).
- **Clear cache after changes:** Flatsome has its own CSS cache (Theme Options regenerates it);
  also clear any caching plugin and regenerate CSS via Theme Options → Advanced.

## Example — right vs wrong output

A request like *"a hero with heading, subtext and a button, plus a 3-column feature row"*:

❌ **Wrong (plain HTML — breaks the Flatsome way):**
```html
<section class="hero" style="background:#111">
  <div class="container"><h1>Welcome</h1><a class="btn">Shop</a></div>
</section>
<div class="row"><div class="col-4">...</div>...</div>
```

✅ **Correct (UX Builder shortcodes):**
```
[ux_banner height="500px" height__sm="300px" bg="123" bg_overlay="rgba(0,0,0,0.4)" text_color="light"]
  [text_box width="60%" position_x="50" position_y="50" text_align="center"]
    [title style="center" text="Welcome" tag_name="h1" color="rgb(255,255,255)"]
    [ux_text]Your subtext here[/ux_text]
    [button text="Shop now" link="/shop" color="white" style="outline"]
  [/text_box]
[/ux_banner]

[section padding="60px 0px 60px 0px"]
  [row]
    [col span="4" span__sm="12" text_align="center"][featured_box icon="icon-truck" title="Free shipping"]Over $50[/featured_box][/col]
    [col span="4" span__sm="12" text_align="center"][featured_box icon="icon-gift" title="Gift wrap"]On request[/featured_box][/col]
    [col span="4" span__sm="12" text_align="center"][featured_box icon="icon-shield" title="Secure"]SSL checkout[/featured_box][/col]
  [/row]
[/section]
```

## Accuracy & provenance (source: docs.uxthemes.com, Flatsome 3.20.0)

This skill was compiled by crawling the official documentation on 2026-06-21. Trust levels:

- **Verbatim from docs (100% reliable):** the hook lists in [references/hooks.md](references/hooks.md);
  the 7 custom product layouts in [references/woocommerce.md](references/woocommerce.md);
  lightbox params, custom CSS, custom fonts, mega-menu and custom-product workflows; the
  `ux_product_*` element list.
- **Compiled from Flatsome conventions + doc examples (accurate, but not from a docs spec page):**
  the per-element attribute tables in [references/shortcodes.md](references/shortcodes.md) and
  [references/grid-system.md](references/grid-system.md). The official docs intentionally have
  **no exhaustive shortcode parameter reference** — they teach via the visual builder and videos.
- **Ground truth for any exact attribute set:** build the element once in UX Builder, save, open
  the page in the WordPress code editor, and copy the generated shortcode. Use this whenever an
  attribute value matters and isn't confirmed here.

If Flatsome ships a new major version, re-crawl `/article/385-hooks` and the shortcodes/snippets
categories to refresh.

## Documentation map (source: docs.uxthemes.com)

| Topic | Where |
|---|---|
| Shortcode reference (all elements + params) | [references/shortcodes.md](references/shortcodes.md) |
| Grid / layout system | [references/grid-system.md](references/grid-system.md) |
| UX Builder, Blocks, custom layouts, header | [references/ux-builder.md](references/ux-builder.md) |
| Theme hooks — actions & filters | [references/hooks.md](references/hooks.md) |
| WooCommerce + custom product pages | [references/woocommerce.md](references/woocommerce.md) |
| Customization: CSS, fonts, child theme, options | [references/customization.md](references/customization.md) |
| Copy-paste code snippets | [references/snippets.md](references/snippets.md) |

Official docs: https://docs.uxthemes.com/
