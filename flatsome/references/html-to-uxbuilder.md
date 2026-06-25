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
| eyebrow + heading + sub-text (3 stacked lines) | **3× `[ux_text]`** (eyebrow / `<h2>` / sub) — **not** `[title sub_text]` (its sub renders *below*) |
| `<p>`, rich text, inline `<strong>/<a>/<br>` | `[ux_text] … [/ux_text]` |
| `<a class="btn">` | `[button text="…" link="…"]` |
| `<img>` (content image) | `[ux_image id="…"]` (attachment ID) |
| hero with bg image + text | `[ux_banner]` **or** `[section]` with Background image |
| `<ul>` icon/feature list item | `[featured_box]` / `[ux_image_box]` |
| even image grid (no caption/overlay) | `[ux_gallery]` |
| gallery with caption / play button / bento sizes | **custom**: `[row]`+`[col]`+`[ux_image]`+`[ux_text]` + CSS (not `[ux_gallery]`) |
| carousel / slider | `[ux_slider]` |
| vertical spacing between bands/elements | `[gap height="…"]` (native, builder-editable) — not loose CSS margins |
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

## UX Builder save behaviors (gotchas when the page is opened/saved in the builder)

The render you get from hand-pasted shortcodes is **not** what you get after the page is opened
in UX Builder and **Saved** — the builder re-serializes the markup and mutates it. These bite
*after the fact* (the page was fine, someone opened the builder, now spacing/colours broke).
Design CSS that **survives a save**; don't hand-fix each time. Verified on live render.

- **Single-line text gets wrapped in `<p>…<br></p>` on save.**
  - *Symptom:* `[ux_text class="x"]20+[/ux_text]` first renders `<div class="text x">20+</div>`,
    but after open-in-builder + Save → `<div class="text x"><p>20+<br></p></div>`.
  - *Why:* the builder normalizes text content into a paragraph; the stray `<br>` adds a blank
    line and the `<p>` adds theme margins. CSS on `.x` never reaches the text inside the `<p>`.
  - *Fix:* scope to the inner `<p>`, and only kill the **junk** `<br>` inside `<p>` (never a
    deliberate `<br>` inside an `<h2>`):
    ```css
    .text[class*="myprefix-"] > p { margin:0!important; padding:0!important; color:inherit!important; font:inherit!important; }
    .text[class*="myprefix-"] > p > br { display:none!important; }
    ```
- **`color`/`font` set directly on the `<p>` beats values inherited from the `.text` parent.**
  - *Symptom:* inside `[section dark="true"]`, Flatsome puts `color:#fff` straight on the `<p>`.
    `color:gold!important` on the parent `.dk-stat__num` div does **not** win — the number stays white.
  - *Why:* a direct property on the element always beats an inherited one from an ancestor — even
    an ancestor's `!important` (`!important` only arbitrates between rules on the *same* element).
  - *Fix:* force the `<p>` itself to inherit (covered by the snippet above):
    `.text[class*="dk-"] > p { color:inherit!important; font:inherit!important; }`
- **`padding="T R B L"` (4-value) gets written as invalid CSS — and re-breaks on every save.**
  - *Symptom:* `[section padding="42px 0px 42px 0px"]` →
    `#section_x{ padding-top:42px 0px 42px 0px; padding-bottom:42px 0px 42px 0px }` (invalid →
    browser drops it → the band loses its padding). Recurs **each time the builder saves**.
  - *Fix:* don't rely on the padding attribute for important bands — force it in CSS:
    `.dk-section{ padding:42px 0!important; }` (or `.dk-section .section-content{…}`).
- **Saving re-serializes the WHOLE page.** One small edit + Save rewrites *every* shortcode on
  the page, so the `<p><br>` wrap and padding corruption apply to **all** elements at once.
  - *Principle:* write **builder-resistant CSS** — broad selectors (`.text[class*="dk-"] > p`),
    `!important` on the properties Flatsome re-asserts (padding, colour, `.text > p` margins) —
    instead of fixing elements one by one. **Classes are NOT lost on save** (verified) — only the
    `<p><br>` wrap and the padding corruption happen.

---

## Render gotchas — what Flatsome actually outputs (so your CSS matches)

These bite every HTML→builder conversion. Verified against live Flatsome render.

- **`[ux_text]` wraps content in `<div class="text …"><p>…</p></div>`.** Your class lands
  **directly on the `.text` div** (`<div class="text x">`, *not* `<div class="x"><div class="text">`),
  and the visible text is inside a `<p>` that carries theme margins.
  ```css
  .x { … }            /* ✅ correct — class IS on .text */
  .x .text { … }      /* ❌ wrong — no nested .text exists; rule never matches */
  ```
  Reset the inner paragraph (`.x > p{ margin:0 }`) and control spacing on the `.text` wrappers.
  See "UX Builder save behaviors" for the `<p><br>` wrap that appears after a save.
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
  (`padding-top: 96px 0px 96px 0px`) → ignored. Force band padding in CSS
  (`.mysection .section-content{ padding:… }`). This recurs on every save — see "UX Builder
  save behaviors".
- **`.col-inner` is the inner wrapper of every `[col]`** — use it as the "card surface"
  (background, border, radius, shadow) so the `[col]`'s gutter stays as spacing between cards.
- **Section background layers:** `[section]` renders `.section-bg` (image/overlay, behind) +
  `.section-content` (content, front). A builder-set bg image lands on
  `#section_x .section-bg.bg-loaded`; the solid overlay on `#section_x .section-bg-overlay`. To
  swap a solid overlay for a gradient, override `.mysection .section-bg-overlay{ background:…!important }`.
- **`[title]` puts the sub-text BELOW the heading**, not above. It renders
  `<span class="section-title-main">Heading<small class="sub-title">SUB</small></span>` — the
  `sub_text` is a trailing `<small>`. For the common "eyebrow on top + heading + description
  below" pattern, **don't use `[title]`** — use three `[ux_text]` blocks (eyebrow / `<h2>` / sub).
- **`[blog_posts]` structure & traps.** Renders
  `.col.post-item > .col-inner > a.plain > .box.box-blog-post > .box-image + .box-text >
  .box-text-inner > h5.post-title + div.is-divider + p…excerpt + button.button`.
  - The "read more" is a **`<button>`**, not `<a>` → target `.box-text .button`, not `a.button`.
  - The excerpt carries **`.show-on-hover`** (hidden until hover). To always show:
    `opacity:1!important; visibility:visible!important; position:static!important`.
  - There's a stray `<div class="is-divider">` (hide if unwanted).
  - ALL-CAPS post titles come from the **post content** (typed uppercase), not CSS —
    `text-transform` won't cleanly lowercase Vietnamese.
- **`[ux_slider]` (Flickity) nav is generated by JS — it's NOT in the server HTML.** Wrapper:
  `.slider.slider-nav-circle.slider-nav-large.slider-nav-light.slider-nav-inside`.
  - `slider-nav-light` ⇒ **white** arrows/dots → invisible on light backgrounds.
  - Arrows: `button.flickity-prev-next-button.previous/.next` containing
    `<svg class="flickity-button-icon"><path class="arrow">`. Recolor the arrow via
    **`.arrow{ fill:… }`** (the path), not `color`. Position via `.previous{left}` / `.next{right}`.
  - Dots: `.flickity-page-dots .dot` and `.dot.is-selected` (style the active dot here).
  - To inspect the generated nav, dump the runtime DOM (Chrome headless `--dump-dom`) — `curl`
    won't show it because Flickity initializes client-side.

---

## Verify (don't skip)

Shortcode that *looks* right often renders differently. After pasting:

1. Fetch the live page HTML (`curl -sL <url>`), or Inspect in the browser.
2. Confirm each `[section]` rendered with your `class`, and check the **actual** wrapper
   structure (`.text>p`, `.button`, `.img-inner`, `.col-inner`) against your CSS selectors.
3. Look for **invalid generated CSS** (e.g. multi-value `padding-top`) and **empty images**
   (placeholder ids).
4. **Screenshot and diff visually, section by section.** Render the live page with Chrome
   headless and compare each band to the mockup:
   ```bash
   chrome --headless --screenshot=out.png --window-size=1280,4000 --force-device-scale-factor=2 "<url>"
   ```
   Caveat: forcing a `width` larger than the real window distorts the capture — shoot at a native
   width (and use `--force-device-scale-factor=2` for crispness) rather than an oversized window.
5. Diff against the HTML mockup block by block; fix selectors/spacing; re-verify.

This analyze → map → emit → **verify** loop is what makes the converted page actually match.

### Quick native self-check (3 axes)

Before calling a build done, score it on three axes — pass = the page is genuinely "native"
and maintainable, not just visually close:

| Axis | Pass when… |
|---|---|
| **Layout native** | structure is `section→row→col`; no HTML shell holding the layout together |
| **Content native** | text/headings/buttons/images are native elements (`[ux_text]`/`[title]`/`[button]`/`[ux_image]`); `[ux_html]` only for embeds/maps/iframes |
| **Editability** | re-opens cleanly in UX Builder and the section/row/col tree is readable to the next editor |

If any axis fails, fix it before shipping — a page that looks right but can't be edited in the
Builder is not done. (Lightweight checklist, not a mandatory scoring artifact.)
