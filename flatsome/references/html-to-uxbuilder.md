# Converting an HTML layout to UX Builder

A repeatable playbook for taking an existing HTML/CSS design (a mockup, a competitor page, a
Figma export, a static template) and reproducing it **faithfully** as Flatsome UX Builder
shortcodes + Custom CSS — so it renders the same and stays editable in the builder.

This is the workflow to use whenever the task is *"build this design in Flatsome"*, *"convert
this HTML to UX Builder"*, or *"make the Flatsome page match this mockup."*

## Recommended flow

1. **Design/confirm the HTML first.** If you only have a description, build a self-contained
   HTML mockup and get sign-off on look before converting. Scope its CSS under one wrapper class
   (e.g. `.vm`) — that maps cleanly to namespaced Flatsome classes later.
2. **Analyze → map → emit** (steps below).
3. **Verify against the live render** (don't trust the shortcode alone — see "Verify").

---

## Step 1 — Analyze the HTML

Break the page into a tree before writing any shortcode:

- **Bands → sections.** Each visually distinct horizontal band (hero, intro, features,
  gallery, testimonials, CTA, contact) becomes **one `[section]`**. One band = one section.
- **Within a band → grid.** A row of columns → `[row]` + `[col]`. Read the column count and
  widths off the CSS (`grid-template-columns`, flex-basis, `width:33%`, Bootstrap `col-4`) and
  translate to 12-col spans: 3-up = `span="4"`, 2-up = `span="6"`, sidebar = `3/9`, etc.
- **Leaf content → elements.** Map each piece of content to a Flatsome element (table below).
- **Note the responsive behavior.** Where the design reflows (3-up → 1-up), record the
  `span__md` / `span__sm` you'll need.
- **Record the styling intent** per block (bg color, padding, image, overlay) so you can decide
  builder-control vs CSS for each (see [customization.md](customization.md#prefer-builder-controls-over-custom-css-use-css-only-for-what-the-builder-cant-do)).

## Step 2 — Map HTML → Flatsome

| HTML | Flatsome |
|---|---|
| `<section>` / outer band `<div>` | `[section]` |
| `<div class="container/row">` (grid wrapper) | `[row]` |
| grid/flex column | `[col span="N" span__sm="12"]` |
| nested grid inside a column | `[row_inner]`/`[col_inner]` (one level only) |
| `<h1>…<h6>` | `[ux_text]` with the real `<h…>` tag inside (full style control), or `[title]` |
| `<p>`, rich text, inline `<strong>/<a>/<br>` | `[ux_text] … [/ux_text]` |
| `<a class="btn">` | `[button text="…" link="…"]` |
| `<img>` (content image) | `[ux_image id="…"]` (attachment ID) |
| hero with bg image + text | `[ux_banner]` **or** `[section]` with Background image |
| `<ul>` icon/feature list item | `[featured_box]` / `[ux_image_box]` |
| carousel / slider | `[ux_slider]` |
| product grid | `[ux_products]` |
| anything with no Flatsome equivalent | `[ux_html] … [/ux_html]` (last resort) |

Apply the three core conventions while mapping: **one block = one section**, **flat
`section→row→col`** (no needless nesting), **unique `class` on every section/row/col** for CSS.
See [grid-system.md](grid-system.md#structure-discipline-clean-maintainable-layouts).

## Step 3 — Decide styling: builder vs CSS

For each block, route styling to the right place:

- **Builder controls** (set in UX Builder, not CSS): background **color**, background **image**,
  the **Dark** toggle for light text, **padding/margin**, **solid overlay** color, text align,
  column widths/v-align.
- **Custom CSS** (namespaced, scoped to your `class`): gradients, gradient/animated overlays,
  hover effects, `object-fit` image cropping, pseudo-elements, and **inner component surfaces**
  (a card built from `.col-inner`, a badge, a pill, a translucent panel).
- **Account for default spacing** (`.col` has `padding:0 15px 30px`, `.section` has
  `padding:30px 0`) instead of overriding theme core. See
  [grid-system.md](grid-system.md#default-spacing--box-model--design-with-it-dont-fight-it).

---

## Render gotchas — what Flatsome actually outputs (so your CSS matches)

These bite every HTML→builder conversion. Verified against live Flatsome render.

- **`[ux_text]` wraps content in `<div class="text …"><p>…</p></div>`.** Your class lands on the
  `.text` div; the visible text is inside a `<p>` that carries theme margins. Reset them:
  `.myblock-section p{ margin:0; }` and control spacing on the `.text` wrappers.
- **Headings:** put a real `<h1>`/`<h2>`/`<h3>` *inside* `[ux_text]` for exact styling, then
  `.myclass h2{ … }`. (Theme heading margins apply — reset to taste.)
- **`[button]` puts your class on the `<a class="button …">` itself** (e.g.
  `<a class="button primary vm-btn vm-btn--gold">`). Target **`.button.vm-btn--gold`**, NOT
  `.vm-btn--gold .button` (there is no `.button` descendant). Use `!important` to beat the
  theme's `primary`/color classes.
- **`[ux_image id="…"]` renders `<div class="img"><div class="img-inner"><img></div></div>`.**
  An invalid/placeholder id renders an **empty** `.img-inner` (no `<img>`) → blank. Use a real
  attachment id. To crop to a fixed height like the mockup, set height on the wrapper **and**
  inner and `object-fit:cover` on the img:
  `.myimg, .myimg .img-inner{ height:230px!important } .myimg img{ width:100%!important; height:100%!important; object-fit:cover!important }`.
- **Section padding via the `padding="T R B L"` attribute can mis-render** to invalid CSS
  (`padding-top: 96px 0px 96px 0px`) on some builds → ignored. Prefer setting band padding via
  the builder's **Padding** control, or scope to `.mysection .section-content{ padding:… }`.
- **`.col-inner` is the inner wrapper of every `[col]`** — use it as the "card surface"
  (background, border, radius, shadow) so the `[col]`'s gutter stays as spacing between cards.
- **Section background layers:** `[section]` renders `.section-bg` (image/overlay, behind) +
  `.section-content` (content, front). A builder-set bg image lands on
  `#section_x .section-bg.bg-loaded`; the solid overlay on `#section_x .section-bg-overlay`. To
  swap a solid overlay for a gradient, override `.mysection .section-bg-overlay{ background:…!important }`.

---

## Verify (don't skip)

Shortcode that *looks* right often renders differently. After pasting:

1. Fetch the live page HTML (`curl -sL <url>`), or Inspect in the browser.
2. Confirm each `[section]` rendered with your `class`, and check the **actual** wrapper
   structure (`.text>p`, `.button`, `.img-inner`, `.col-inner`) against your CSS selectors.
3. Look for **invalid generated CSS** (e.g. multi-value `padding-top`) and **empty images**
   (placeholder ids).
4. Diff against the HTML mockup block by block; fix selectors/spacing; re-verify.

This analyze → map → emit → **verify** loop is what makes the converted page actually match.
