# Customization: Theme Options, CSS, Fonts, Child Theme

## Where things go (decision guide)

| You want to change... | Put it here |
|---|---|
| Colors, typography, header/footer layout, shop layout | **Theme Options** (Customizer) |
| Site-wide CSS | Theme Options → **Style → Custom CSS** |
| One element's CSS | Element's **Advanced → CSS class**, then target in Custom CSS |
| Custom JS | Theme Options → **Advanced → Custom Scripts** (header/footer) |
| PHP (hooks, functions, template overrides) | **Child theme** `functions.php` / template files |
| Reusable layout/content | A **UX Block** (`[block id="..."]`) |

> After CSS/option changes, regenerate Flatsome's CSS cache (Theme Options → Advanced) and
> clear any caching plugin.

### Prefer builder controls over custom CSS (use CSS only for what the builder can't do)

When something can be set with the element's **own UX Builder control**, set it there — not in
Custom CSS. It stays editable in the builder, survives re-saves, and doesn't fight the cascade.
Reach for CSS only for things the builder genuinely can't express.

| Style it natively in the builder | Examples |
|---|---|
| **Section/row/col background color** | section/row/col → **Background → Color** |
| **Background image** | **Background → Image** (section/banner/col) |
| **Light text on dark backgrounds** | section's **Dark** toggle (don't hand-set `color:#fff`) |
| **Padding / margin / spacing** | element's **Padding/Margin** fields, `[gap]`, row gutter `style` |
| **Solid color overlay on a bg image** | section **Overlay color** |
| **Text align, column width, vertical align** | element controls (`text_align`, `span`, `v_align`) |

**Use Custom CSS only when the builder can't:** CSS **gradients** (builder backgrounds/overlays
are solid only), gradient/animated overlays, fine hover effects, `object-fit` image cropping,
pseudo-elements, and styling **inner component surfaces** that aren't builder elements (e.g. a
card built from `.col-inner`, a badge, a custom pill). Scope those to your own namespaced class.

> Rule of thumb: if there's a field for it in the builder sidebar, use the field. If you'd be
> writing CSS to re-do something the sidebar already offers, stop and use the sidebar.

---

## Theme Options (Customizer)

`Appearance → Customize` → **Flatsome** panel. Key sections:
- **Style:** primary/secondary colors, link colors, typography (Google Fonts), Custom CSS,
  body/site layout (boxed/full), border radius, shadows.
- **Header:** the drag-and-drop header builder, transparent/sticky, mobile header, search, cart.
- **Footer:** footer widgets, absolute footer (copyright bar), payment icons.
- **Shop:** catalog layout, product page (custom layout), cart, checkout, product cards.
- **Blog:** archive & single post layouts.
- **Advanced:** custom scripts, performance/lazy-load, CSS regeneration, Pjax.

---

## Custom CSS workflow

Source: docs.uxthemes.com/article/235.

1. Right-click an element → **Inspect** (Chrome/Firefox/Safari).
2. Find the element, **Copy → Copy selector**, then **simplify** the long selector to the
   meaningful tail, e.g. `p.name.product-title > a`.
3. Add rules in Theme Options → **Custom CSS** (or an element's `Advanced > Custom CSS`).

```css
/* hide */    p.name.product-title > a { display: none; }
/* size */    p.name.product-title > a { font-size: 200%; }
/* color */   p.name.product-title > a { color: #777; }
```

### Useful structural classes/selectors
- `.row` / `.col` — grid
- `.nav > li > a` — top menu links
- `.header`, `.header-main`, `.header-top`, `.header-bottom`
- `.is-sticky` / `.stuck` — sticky header state
- `.product-small`, `.box`, `.box-text` — product card
- `.add-to-cart-button`, `.single_add_to_cart_button`
- Responsive helpers: `.show-for-medium`, `.hide-for-small`, `.hidden`

---

## Custom fonts

Source: docs.uxthemes.com/article/224.

1. **Disable default Google Fonts** first if using custom fonts only:
   Theme Options → Style → Typography → disable Google Fonts.
2. **Load the font** via a plugin (Use Any Font / Custom Fonts) or `@font-face`, or TypeKit
   (TypeKit Fonts for WordPress).
3. **Apply via Custom CSS:**

```css
/* Base font */
body { font-family: "Custom Font Name", sans-serif; }

/* Navigation font */
.nav > li > a,
.mobile-sidebar-levels-2 .nav > li > ul > li > a { font-family: "Custom Font Name", sans-serif; }

/* Headings */
h1, h2, h3, h4, h5, h6, .heading-font,
.off-canvas-center .nav-sidebar.nav-vertical > li > a { font-family: "Custom Font Name", sans-serif; }

/* Alt font (titles, buttons in some places) */
.alt-font { font-family: "Custom Font Name", sans-serif; }
```

Self-hosted `@font-face` example:
```css
@font-face {
  font-family: "Custom Font Name";
  src: url("/wp-content/uploads/fonts/myfont.woff2") format("woff2");
  font-weight: 400;
  font-display: swap;
}
```

---

## Child theme (required for PHP/template changes)

Flatsome ships an official child theme (`flatsome-child`) — activate it and put your code there
so theme updates don't overwrite your work.

```
flatsome-child/
├── style.css        ← header comment with Template: flatsome
├── functions.css    (optional)
├── functions.php    ← hooks, filters, custom functions
└── woocommerce/...  ← template overrides (mirror parent path)
```

`style.css` header:
```css
/*
Theme Name: Flatsome Child
Template: flatsome
*/
```

`functions.php` starter:
```php
<?php
add_action('wp_enqueue_scripts', function () {
    wp_enqueue_style('flatsome-child', get_stylesheet_uri(), ['flatsome-style']);
});

// your hooks/filters here (see hooks.md)
```

**Template overrides:** copy a parent template into the child theme keeping the same path
(e.g. `flatsome/woocommerce/single-product/title.php` →
`flatsome-child/woocommerce/single-product/title.php`) and edit the copy. Prefer hooks over
overrides when possible — overrides can break on major updates.

---

## Performance / caching

- Flatsome generates a combined CSS cache. After theme-option or CSS changes, **regenerate**
  it (Theme Options → Advanced) and clear page-cache plugins.
- Compatible caching plugins documented: WP Rocket, W3 Total Cache, etc. (docs category 197).
- **Lazy load**, **Pjax** (AJAX page transitions), and **infinite scroll** are toggled in Theme
  Options → Advanced / Shop. See [snippets.md](snippets.md) for tweaks.

---

## Debugging

- **Enable WP Debug:** set `define('WP_DEBUG', true);` (and `WP_DEBUG_LOG`) in `wp-config.php`.
- **Web console:** browser DevTools → Console to read JS errors.
- **System Status:** Theme Options / Flatsome status screen reports environment issues.
- **Release channel:** opt into beta Flatsome builds via Theme Options (for testing fixes).
