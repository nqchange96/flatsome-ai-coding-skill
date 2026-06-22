# Flatsome Grid & Layout System

Flatsome uses a **12-column responsive grid**. The structural hierarchy is:

```
[section]            ← full-width band (background, padding, height, video bg)
  [row]             ← 12-col grid container (max-width = site content width)
    [col]           ← a column inside the row
      ...content    ← ux_image, ux_text, button, etc.
    [/col]
  [/row]
[/section]
```

For grids inside a column, use the `_inner` variants so they don't conflict:
`[row_inner]` / `[col_inner]`.

---

## Structure discipline (clean, maintainable layouts)

These three rules keep generated layouts clean, predictable, and easy to style. Follow them
on **every** layout — they prevent the messy, over-nested output Flatsome projects often
accumulate.

### 1. One content block = one `[section]`

Each distinct part of the page (hero, intro, features, testimonials, CTA, FAQ, …) gets **its
own `[section]`**. Do **not** pack several unrelated content blocks into a single section.

Why: a section is the natural "band" of the page — separate sections can each have their own
background/padding/spacing, be reordered or toggled independently, be styled in isolation, and
be lifted out into a reusable UX Block. Cramming multiple parts into one section makes all of
that harder and forces fragile spacing hacks (`[gap]` everywhere) to fake separation.

```
✅ Correct — one section per content block
[section label="Hero" class="home-hero"] [row]...[/row] [/section]
[section label="Features" class="home-features"] [row]...[/row] [/section]
[section label="CTA" class="home-cta"] [row]...[/row] [/section]

❌ Wrong — three unrelated blocks crammed into one section
[section]
  [row]...hero...[/row]
  [gap height="60px"]
  [row]...features...[/row]
  [gap height="60px"]
  [row]...CTA...[/row]
[/section]
```

### 2. Keep `section → row → col` flat — avoid needless nesting

Flatsome layouts tend to grow unnecessary nested rows/cols inside other cols (`col → row_inner
→ col_inner → row_inner …`), which bloats the markup and makes both the builder and CSS hard
to reason about. Stick to the canonical depth:

```
[section] → [row] → [col] → content
```

Only go one level deeper — **`[row_inner]` / `[col_inner]` inside a `[col]`** — when you
genuinely need a grid *within* a column (e.g. a 2×2 mini-grid inside one half of the page).
Never nest beyond that, and never wrap a single element in an extra row/col "just in case."

```
✅ Correct — flat, one inner grid only where a sub-grid is truly needed
[row]
  [col span="6" span__sm="12"][ux_image id="1"][/col]
  [col span="6" span__sm="12"]
    [row_inner]
      [col_inner span="6"]...[/col_inner]
      [col_inner span="6"]...[/col_inner]
    [/row_inner]
  [/col]
[/row]

❌ Wrong — pointless extra nesting around content that needs no sub-grid
[row]
  [col span="12"]
    [row_inner][col_inner span="12"]
      [row_inner][col_inner span="12"][ux_text]...[/ux_text][/col_inner][/row_inner]
    [/col_inner][/row_inner]
  [/col]
[/row]
```

### 3. Give each `[section]`/`[row]`/`[col]` its own CSS class

Add a **unique, descriptive `class`** to every section, row, and the columns you intend to
style. This gives "vibe coding" a stable hook to target with Custom CSS instead of relying on
fragile structural selectors (`.section:nth-child(3) .col`) that break when the layout changes.

```
[section label="Features" class="home-features"]
  [row class="home-features__row"]
    [col span="4" span__sm="12" class="home-features__col"]...[/col]
    [col span="4" span__sm="12" class="home-features__col"]...[/col]
    [col span="4" span__sm="12" class="home-features__col"]...[/col]
  [/row]
[/section]
```

Then Custom CSS targets the class directly (the class is appended to Flatsome's standard
output classes — see the shortcode→HTML table below):

```css
.home-features { background: #fafafa; }
.home-features__col { border-radius: 12px; }
```

Use a consistent naming scheme (e.g. `block__element`) and keep classes unique per content
block so styles don't leak across sections.

---

## Responsive breakpoints

Most attributes support breakpoint suffixes:

| Suffix | Target | Typical max-width |
|---|---|---|
| (none) | Desktop / default | — |
| `__md` | Tablet | ≤ 849px |
| `__sm` | Mobile | ≤ 549px |

Example: `[col span="4" span__md="6" span__sm="12"]` → 3-up desktop, 2-up tablet, 1-up mobile.

**Rule of thumb:** every multi-column row needs `span__sm="12"` on its columns, otherwise
columns stay narrow and cramped on phones.

---

## `[section]`

Full-width container. Use for distinct page bands with their own background.

| Attribute | Values / example | Notes |
|---|---|---|
| `label` | `"Hero"` | Name shown in builder tree |
| `bg` | `123` | Background image attachment ID |
| `bg_color` | `#f5f5f5`, `rgba(0,0,0,.5)` | Background color |
| `bg_overlay` | `rgba(0,0,0,0.3)` | Color overlay on top of bg image |
| `bg_pos` | `center center` | Background position |
| `bg_size` | `cover` / `contain` | |
| `parallax` | `1`–`10` | Parallax scroll amount |
| `video_mp4` / `video_webm` / `video_ogg` | URL | Background video |
| `video_visibility` | `visible` / `hidden-phone` | |
| `dark` | `true` | Light text for dark backgrounds |
| `height` | `300px`, `500px`, `100vh` | Section min-height |
| `height__sm` | `200px` | Mobile height |
| `padding` | `60px 0px 60px 0px` | Inner padding (T R B L) |
| `margin` | `0px` | |
| `border` | `1px solid #ddd` | |
| `text_align` | `left` / `center` / `right` | |
| `text_color` | `light` / `dark` | |
| `visibility` | `hidden` / `show-for-*` | |
| `class` | `my-class` | Custom CSS class |

```
[section label="CTA" bg_color="#222222" dark="true" padding="80px 0px 80px 0px" height="auto"]
  [row]
    [col span__sm="12" text_align="center"]
      [title text="Join the club" color="rgb(255,255,255)"]
      [button text="Sign up" link="/register" color="primary"]
    [/col]
  [/row]
[/section]
```

---

## `[row]`

12-column grid container.

| Attribute | Values / example | Notes |
|---|---|---|
| `label` | `"Row"` | |
| `style` | `default` / `collapse` / `small` / `large` | Gutter style. `collapse` = no gutter |
| `width` | `full-width` / `feature-width` / `custom` | Row width |
| `v_align` | `top` / `middle` / `bottom` / `equal` | Vertical alignment of columns |
| `h_align` | `left` / `center` / `right` | Horizontal alignment when cols < 12 |
| `padding` | `15px` | |
| `depth` | `1`–`5` | Box shadow |
| `col_style` | `divided` / `dividers` | Lines between columns |
| `class` | | |

```
[row style="collapse" v_align="middle"]
  [col span="6" span__sm="12"]...[/col]
  [col span="6" span__sm="12"]...[/col]
[/row]
```

---

## `[col]`

A column. **`span` 1–12.** Columns in a row should sum to 12 (extra wrap to next line).

| Attribute | Values / example | Notes |
|---|---|---|
| `span` | `1`–`12` | Desktop width. Default `12` |
| `span__md` / `span__sm` | `6` / `12` | Responsive widths |
| `offset` | `1`–`11` | Push column right by N cols |
| `force_first` / `force_last` | `medium` / `small` | Reorder on mobile |
| `padding` | `20px 20px 20px 20px` | |
| `margin` | `0px` | |
| `bg_color` | `#ffffff` | Column background |
| `color` | `light` / `dark` | Text color scheme |
| `align` | `left` / `center` / `right` | |
| `text_align` | `center` | |
| `max_width` | `300px` | |
| `height` | `100%` | |
| `depth` / `depth_hover` | `1`–`5` | Shadow + hover shadow |
| `animate` | `fadeInUp`, `fadeInLeft`, `flipInY`, `zoomIn`, ... | Entrance animation |
| `visibility` | `hidden-phone`, `show-for-medium` | |
| `class` | | |

```
[row]
  [col span="4" span__sm="12" padding="30px" bg_color="#fafafa" depth="2" animate="fadeInUp"]
    [ux_image_box img="123"][/ux_image_box]
  [/col]
[/row]
```

---

## Spacing & dividers

```
[gap height="30px" height__sm="15px"]      ← vertical space
[divider align="center" width="50px" height="3px" color="rgb(0,0,0)"]
[ux_html] raw HTML here [/ux_html]          ← raw HTML escape hatch
```

## Shortcode → rendered HTML (Flatsome standard output)

This is how Flatsome's grid shortcodes always render — standard theme behavior. Knowing the
output classes helps you write Custom CSS targeting the right element.

| Shortcode | Rendered HTML |
|---|---|
| `[section]` | `<section class="section"><div class="section-bg fill">…</div><div class="section-content relative">…</div></section>` |
| `[section class="my-class"]` | same, with `my-class` appended to `.section` |
| `[section bg="12" bg_overlay="…"]` | adds `<img class="bg attachment-original …">` + `<div class="section-bg-overlay absolute fill">` inside `.section-bg` |
| `[row]` | `<div class="row">` |
| `[row v_align="middle"]` | `<div class="row align-middle">` |
| `[col span="6" span__sm="12"]` | `<div class="col medium-6 small-12">` |
| `[col span="3" span__sm="12"]` | `<div class="col medium-3 small-12">` |
| `[gap]` | `<div class="gap-element clearfix">` |
| `[blog_posts type="row" columns="3" columns__md="1"]` | `<div class="row large-columns-3 medium-columns-1">` with `<div class="col post-…">` children |

**Key mapping:** `span` → `medium-*` (desktop/tablet), `span__sm` → `small-*` (mobile). The
grid is column-class based (`.col.medium-6.small-12`), so target columns in CSS via those
classes — not by element position.

## Default spacing / box model — design WITH it, don't fight it

Flatsome ships built-in spacing on the grid primitives. **Account for these in your layout
(use the elements' own spacing attributes); do NOT override Flatsome's core `.col`/`.section`
CSS** — that breaks the grid site-wide and fights the cascade. Know the defaults so your output
lands at the right spacing the first time.

```css
/* Flatsome core defaults (do not override globally) */
.col, .columns { margin: 0; padding: 0 15px 30px; width: 100%; }  /* 15px L/R gutter + 30px BOTTOM */
.row           { margin: 0 -15px; }                                /* negative margin cancels the 15px */
.section       { padding: 30px 0; display: flex; align-items: center; }  /* 30px top/bottom band */
```

What this means when you generate layouts:

- **Horizontal gutter is already handled.** Each `[col]` has `15px` left/right padding, and the
  `[row]` has `-15px` margins that cancel it at the edges — so column **content aligns flush to
  the content-width edge**, with a `~30px` gap *between* adjacent columns. Don't add your own
  side padding expecting flush content, and don't try to recreate the gutter.
- **`[col]` has `30px` padding-BOTTOM by default** — this is the **vertical gutter** between
  columns that wrap/stack (e.g. a 3-up card row dropping to 1-up on mobile). For card grids,
  **rely on it** instead of adding `margin-bottom`. When you specifically need a column flush to
  the band bottom (e.g. a stat bar docked to the hero's edge), zero it **on that column** via its
  own attribute — `[col padding="0px 15px 0px 15px"]` — not a global `.col` override.
- **`[section]` has `30px` top/bottom by default.** To set a different band height, use the
  section's **own** padding attribute: `[section padding="96px 0px 96px 0px"]`. This *replaces*
  the default 30px (it doesn't stack on top of it). Keep left/right at `0` — the row handles
  horizontal width.
- **Spacing math / "trừ hao":** the visible space below the last row of a section ≈ the section's
  bottom padding **plus** the `30px` col padding-bottom. So if a design calls for `96px` of clear
  space under a card row, setting `[section padding="…66px…"]` lands at ~96px once the col's `30px`
  is added — or set the section to `96px` and accept the extra `30px` as the inter-card gutter.
  Decide per design which value to dial; **adjust via the shortcode attributes, not CSS resets.**
- **Per-element spacing belongs on the element.** Use `[section padding=…]`, `[col padding=…
  margin=…]`, `[row style="collapse|small|large"]` (gutter size) and `[gap height=…]` to tune
  spacing. This keeps everything editable in UX Builder and avoids overriding theme core.

> Last resort only: if a pixel-exact match truly needs it, scope an override to your **own**
> namespaced class (`.myblock .col-inner { … }`) — never restyle bare `.col`/`.section`.

## Common layout recipes

**3-column feature row (responsive):**
```
[row]
  [col span="4" span__sm="12"] ... [/col]
  [col span="4" span__sm="12"] ... [/col]
  [col span="4" span__sm="12"] ... [/col]
[/row]
```

**Sidebar + content (1/4 + 3/4):**
```
[row]
  [col span="3" span__sm="12"][ux_sidebar id="shop"][/col]
  [col span="9" span__sm="12"] ... main content ... [/col]
[/row]
```

**Centered narrow content:**
```
[row h_align="center"]
  [col span="8" span__sm="12" text_align="center"] ... [/col]
[/row]
```

**Image left / text right, vertically centered, no gutter:**
```
[row style="collapse" v_align="middle"]
  [col span="6" span__sm="12"][ux_image id="123"][/col]
  [col span="6" span__sm="12" padding="0 0 0 40px"][ux_text]...[/ux_text][/col]
[/row]
```
