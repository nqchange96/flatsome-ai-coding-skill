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
