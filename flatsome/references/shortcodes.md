# Flatsome / UX Builder Shortcode Reference

Every UX Builder element is a shortcode stored in `post_content`. This reference covers the
content elements. For structure (`section`/`row`/`col`/`gap`/`divider`) see
[grid-system.md](grid-system.md). For product elements see [woocommerce.md](woocommerce.md).

**Universal attributes** available on most elements:
`class`, `visibility` (`hidden`, `hidden-phone`, `show-for-medium`, ...),
`animate` (`fadeIn`, `fadeInUp`, `fadeInLeft`, `fadeInRight`, `zoomIn`, `flipInY`, `bounceIn`),
`margin`, `padding`, `label` (builder tree name).

---

## Text & headings

### `[ux_text]`
Rich text container.
```
[ux_text text_align="center" font_size="1.2" line_height="1.5" text_color="#333"]
  Your <strong>rich</strong> text here.
[/ux_text]
```
Attributes: `text_align`, `text_align__sm`, `font_size`, `line_height`, `letter_spacing`,
`text_color`, `text_depth`, `max_width`.
> ⚠️ **CSS target:** the `class` lands **on the `.text` div** → `<div class="text x">`. Use
> `.x{…}` (✅), not `.x .text{…}` (❌, no nested `.text`). After a builder save the text is
> wrapped in `<p><br>` — see save behaviors in
> [html-to-uxbuilder.md](html-to-uxbuilder.md#ux-builder-save-behaviors-gotchas-when-the-page-is-openedsaved-in-the-builder).

### `[title]`
Styled section heading with optional divider/sub-text.
```
[title style="center" text="Our Products" tag_name="h2" size="100"
  color="rgb(0,0,0)" margin_top="" sub_text="Browse the collection"]
```
- `style`: `normal` | `center` | `bold` | `bold-center` | `simple-line`
- `text`: heading text
- `sub_text`: small text under/over the heading
- `tag_name`: `h1`–`h6`
- `size`: percentage scale (e.g. `100`, `150`)
- `color`, `icon` (dingbat icon name)

### `[ux_html]`
Raw HTML passthrough (escape hatch for anything not covered).
```
[ux_html]<div class="custom">Anything</div>[/ux_html]
```

---

## Images & media

### `[ux_image]`
```
[ux_image id="1234" image_size="original" width="80" height="" margin="0px"
  link="/page" target="_self" lightbox="true" caption="false"
  animate="fadeInUp" hover="zoom" depth="2" depth_hover="3"]
```
- `id`: **attachment ID** (preferred). Use the media library ID.
- `image_size`: `thumbnail` | `medium` | `large` | `original`
- `width` / `height`: percentage or px
- `link`, `target`, `lightbox` (open in popup), `hover` (`zoom`/`fade`/`overlay-...`)

### `[ux_banner]`
Hero banner: background image/video + overlay + inner content (often holds a `[text]` block).
```
[ux_banner height="500px" height__sm="300px" bg="1234" bg_color="#000000"
  bg_overlay="rgba(0,0,0,0.3)" bg_pos="50% 50%" parallax="3"
  video_mp4="https://.../movie.mp4" video_visibility="visible"
  effect="" text_color="light" link=""]
  [text_box width="50%" position_x="50" position_y="50" text_align="center"]
    [ux_text] ... [/ux_text]
    [button text="Shop now" link="/shop"]
  [/text_box]
[/ux_banner]
```
- `height`, `height__sm`: banner height
- `bg`: background image ID; `bg_color`, `bg_overlay`, `bg_pos`, `bg_size`, `parallax`
- `video_mp4`/`video_webm`/`video_ogg`, `video_visibility`
- `text_color`: `light` | `dark`
- `effect`: hover effect (`fade-out`, `zoom`, `flip`, `blur`, `sliding-anim`, `colorize`, `gray`)
- `[text_box]` inner: `width`, `position_x` (0–100), `position_y` (0–100), `text_align`,
  `animate`, `parallax_text`, `scale` (size on hover), `rotate`.

### `[ux_video]`
```
[ux_video url="https://www.youtube.com/watch?v=..." width="600px" height="56.25%"]
```
Supports YouTube/Vimeo URLs or `mp4=""` for self-hosted.

### `[ux_image_box]`
Image + heading + text, often used as feature/service card.
```
[ux_image_box img="1234" image_width="60" image_radius="100" text_align="center"
  link="/page" depth="1" depth_hover="3"]
  [ux_text] ... heading & description ... [/ux_text]
[/ux_image_box]
```
- `img`: image ID, `image_width` (%), `image_radius` (rounded), `image_size`

### `[featured_box]` (icon box)
```
[featured_box img="" icon="icon-heart" pos="center" margin_bottom="0px"
  title="Free Shipping" font_size="" link=""]
  Description text here.
[/featured_box]
```
- `icon`: Flatsome icon name (e.g. `icon-heart`, `icon-star`, `icon-checkmark`, `icon-truck`,
  `icon-gift`, `icon-phone`, `icon-email`, `icon-shopping-cart`)
- `pos`: `left` | `top` | `center` | `right`
> ⚠️ The title renders as `<h5 class="uppercase">` → **ALL CAPS by default** (add
> `text-transform:none`). The `icon` sometimes **fails to render** `.icon-box-img` — if it's
> missing, draw the icon yourself with CSS `::before` instead of relying on the attribute.

### `[gallery]` / `[ux_gallery]`
```
[ux_gallery ids="1,2,3,4" columns="4" columns__sm="2" image_size="thumbnail"
  lightbox="true" lightbox_image_size="large" depth="" caption="false"]
```
> ⚠️ `[ux_gallery]` only produces an **even grid** — no per-tile captions, overlays, play
> buttons, or bento (varied) sizes. For a captioned / play-button / bento gallery, build it
> manually from `[row]`+`[col]`+`[ux_image]`+`[ux_text]` and add CSS (`position:absolute`
> overlays, `::before/::after`).

---

## Buttons & links

### `[button]`
```
[button text="Click me" link="https://example.com" target="_blank"
  color="primary" style="default" size="medium" expand="false"
  icon="icon-angle-right" icon_pos="right" radius="0" depth="2"
  lightbox="false" lightbox_id="" rel="nofollow"]
```
- `color`: `primary` | `secondary` | `success` | `alert` | `white` | `black` | custom hex via `bg_color`/`color`
- `style`: `default` | `outline` | `link` | `bevel` | `gloss` | `shade` | `flat`
- `size`: `xsmall` | `small` | `medium` | `large` | `xlarge`
- `expand`: `true` makes it full-width
- `icon`, `icon_pos` (`left`/`right`), `radius`, `depth`, `depth_hover`
- Trigger a lightbox: `link="#mybox"` (with a matching `[lightbox id="mybox"]`).
> ⚠️ Button text is **UPPERCASE by default** (add `text-transform:none; letter-spacing:0` to
> match a lowercase mockup). The `class` lands **on the `<a class="button …">`** itself → target
> `.button.myclass` (✅), not `.myclass .button` (❌); use `!important` to beat the `primary`/color
> classes Flatsome also adds.

### `[ux_text_box]` / inline links
Use standard HTML `<a>` inside `[ux_text]` for inline links.

---

## Sliders & carousels

### `[ux_slider]`
Generic slider wrapper. Each direct child is a slide (often an `[ux_banner]` or `[ux_image]`).
```
[ux_slider slide_width="100%" auto_slide="true" timer="5000" nav_style="circle"
  arrows="true" bullets="true" infinitive="true" bg_color="" hide_nav="false"
  nav_pos="outside" nav_color="dark"]
  [ux_banner height="500px" bg="1"] ... [/ux_banner]
  [ux_banner height="500px" bg="2"] ... [/ux_banner]
[/ux_slider]
```
- `slide_width`: `100%`, `300px`, etc. Smaller = carousel of multiple items
- `auto_slide`, `timer` (ms), `arrows`, `bullets`, `infinitive` (loop)
- `nav_style`: `circle` | `simple` | `reveal`; `nav_pos`: `inside`/`outside`; `nav_color`

### `[ux_slider]` as logo/product carousel
Set a fixed `slide_width` (e.g. `200px`) and put multiple `[ux_image]` inside.

### `[blog_posts]`  — posts grid / slider (very common on content & real-estate sites)
Outputs WordPress posts as a grid, row, or full slider. Heavily used for news, projects,
listings — anything that's a custom post/category rather than a WooCommerce product.
```
[blog_posts type="row" columns="3" columns__md="1" cat="12" posts="6"
  show_date="false" excerpt="false" excerpt_length="25" readmore="Read more"
  comments="false" image_height="56.25%" image_size="original" image_hover="zoom"
  text_align="left" offset="0" style="default"]
```
- `type`: `row` | `grid` (masonry) | `slider` | `slider-full` (full-width hero slider)
- `cat`: **category ID(s)** (comma-separated), `posts` (count), `offset`
- `columns`, `columns__md`, `columns__sm`
- `style`: `default` | `vertical` | `overlay` | `bounce` | `badge` | `push`
- `show_date`: `false` | `text` | `badge`; `excerpt`, `excerpt_length`, `readmore`, `comments`
- `image_height` (aspect %), `image_width` (% for vertical style), `image_hover` (`zoom`/`fade`)
- As a hero: `type="slider-full" style="overlay" slider_bullets="true" auto_slide="5000"`
> ⚠️ Styling traps (read-more is a `<button>` not `<a>`, excerpt is `.show-on-hover`, stray
> `.is-divider`, uppercase titles come from post content): see the `[blog_posts]` entry in
> [html-to-uxbuilder.md](html-to-uxbuilder.md#render-gotchas--what-flatsome-actually-outputs-so-your-css-matches).

### `[ux_banner_grid]`
Masonry-style grid of banners.
```
[ux_banner_grid height="500px" spacing="small"]
  [col_grid span="6" height="2-2"][ux_banner ...][/col_grid]
  [col_grid span="6" height="1-2"][ux_banner ...][/col_grid]
[/ux_banner_grid]
```

---

## Interactive / structural elements

### `[lightbox]`  (modal popup)
Display content in a modal triggered by any link to `#id`.
```
[button text="Open" link="#promo"]

[lightbox id="promo" width="600px" padding="20px" auto_open="false"
  auto_timer="3000" auto_show="once"]
  Insert lightbox content here (any shortcodes / banner / form)...
[/lightbox]
```
- `id`: unique; trigger with `link="#id"` on any button/link
- `width`, `padding`
- `auto_open="true"` + `auto_timer="3000"` (ms): opens automatically
- `auto_show`: `always` | `once` (once per visitor) — for newsletter/promo popups

### `[accordion]` / `[accordion-item]`
```
[accordion]
  [accordion-item title="Question 1"]Answer 1[/accordion-item]
  [accordion-item title="Question 2" open="true"]Answer 2[/accordion-item]
[/accordion]
```

### `[tabgroup]` / `[tab]`
```
[tabgroup title="" style="" align="center"]
  [tab title="Tab 1"] content [/tab]
  [tab title="Tab 2"] content [/tab]
[/tabgroup]
```

### `[scroll_to]` / `[scroll-to]`
Smooth-scroll anchor link.
```
[scroll_to title="Go down" link="#section-id" bullet="true"]
```

### `[block id="123"]`
Insert a reusable **UX Block** (see [ux-builder.md](ux-builder.md)). `id` is the block post ID
(or use the block slug). Use for footers, mega menus, popups, repeated sections.

### `[ux_sidebar id="my-sidebar"]`
Output a registered widget area inside the builder.

### `[ux_countdown]`
```
[ux_countdown date="2026-12-31" time="23:59:59" timezone="" style="dark"]
```

### `[map]` (Google Maps)
```
[map height="400px" lat="..." long="..." zoom="15" address="..."]
```

### `[social_icons]` / `[follow]`
Social/share/follow buttons (see [snippets.md](snippets.md) for customizing links).
```
[follow scale="" style="fill" facebook="..." instagram="..." email="true"]
[share style="fill" align="left" scale=""]
```

---

## Lists, prices, misc

### `[ux_price_table]`
```
[ux_price_table title="Pro" price="$49" featured="true" button_text="Buy"
  button_link="/buy" bullet_1="Feature A" bullet_2="Feature B"]
```

### `[testimonial]`
```
[testimonial name="Jane Doe" company="Acme" image="123" pos="center" font_size=""]
  Great product!
[/testimonial]
```

### `[row_inner]` / `[col_inner]`
Nested grid for use inside a `[col]`. Same attributes as `[row]`/`[col]`.

---

## Generating shortcodes from the builder

Per the docs, the reliable way to get exact shortcode output for a complex element:
1. Create a blank page → open in **UX Builder**.
2. Build & style the element visually.
3. Save.
4. Open the page in the normal **WordPress code/text editor**.
5. Copy the generated shortcode — it works anywhere (header, block, widget, another page).

This is the best way to discover the exact attribute values for advanced styling that isn't
fully enumerated here.
