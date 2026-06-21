# Flatsome + WooCommerce

Flatsome is built primarily as a WooCommerce theme. Shop styling lives in
**Theme Options → Shop** (catalog layout, product page, cart, checkout). This file covers the
shop-building shortcodes and the **custom product page** system.

## Product grids & category grids

### `[ux_products]`  — product grid
```
[ux_products type="row" col_spacing="small" columns="4" columns__md="2" columns__sm="2"
  depth="1" depth_hover="4" style="normal" show_cat="true" show_rating="true"
  ids="" cat="hoodies,shirts" tags="" products="8" orderby="popularity" order="desc"
  show="featured" out_of_stock="exclude"]
```
- `columns`, `columns__md`, `columns__sm`: items per row per breakpoint
- `cat` (category slugs), `tags`, `ids` (specific products), `products` (count)
- `orderby`: `date` | `popularity` | `rating` | `rand` | `menu_order` | `price` | `sales`
- `order`: `asc` | `desc`
- `show`: `featured` | `onsale` | `` (all)
- `type`: `row` | `grid` (masonry) | `slider`
- `style`: `normal` | `vertical` | `badge` | `push` | `overlay`

### `[ux_product_categories]`  — category grid
```
[ux_product_categories style="normal" type="row" columns="4" columns__sm="2"
  ids="" parent="0" number="" image_size="medium" image_overlay="rgba(0,0,0,.2)"
  text_pos="bottom" text_bg="" repeat="" slider_nav_style="circle"]
```

### `[featured_products]` / `[sale_products]` / `[best_selling_products]` / `[recent_products]`
Standard WooCommerce-style shortcodes also work:
```
[featured_products per_page="8" columns="4"]
[sale_products per_page="8" columns="4"]
[products limit="8" columns="4" category="hoodies"]
```

---

## Single product elements (for custom product layouts)

These `ux_product_*` shortcodes render pieces of the single product page. Use them inside a UX
Block assigned as a custom product layout.

| Shortcode | Renders |
|---|---|
| `[ux_product_gallery]` | Product image gallery. `style="full-width"` for wide gallery |
| `[ux_product_breadcrumbs]` | Breadcrumb nav |
| `[ux_product_title]` | Product name |
| `[ux_product_rating]` | Star rating |
| `[ux_product_price]` | Price |
| `[ux_product_excerpt]` | Short description |
| `[ux_product_add_to_cart]` | Add-to-cart form (qty + button + variations) |
| `[ux_product_meta]` | SKU, categories, tags |
| `[ux_product_tabs]` | Description / Additional info / Reviews tabs |
| `[ux_product_upsell]` | Up-sell products. `style="grid"` |
| `[ux_product_related]` | Related products |
| `[share]` | Social share buttons |
| `[ux_sidebar id="product-sidebar"]` | Product page widget area |

You can also drop a **Product Hooks** element (`[ux_product_hooks]` in the builder) to render
WooCommerce hooks output from plugins.

---

## Custom Product Page — workflow

Source: docs.uxthemes.com/article/245 & /article/247.

1. **Create a UX Block** at `Dashboard → Blocks → Add new`. Naming convention:
   - `CPL - Global` (site-wide layout)
   - `CPL Category - <Category>` (per category)
   - `CPL Single - <Name>` (per single product)
2. **Assign it:**
   - **Global:** Theme Options → Shop → Product Page → enable **Custom**, select the block.
   - **Per category:** edit the product category → select the custom layout block.
   - **Per product:** edit the single product → assign the layout.
3. **Edit:** open the product on the frontend → admin bar → *"Edit product layout with UX
   Builder"*. Start with a Gap + Row, then add `ux_product_*` elements.
4. **Plugin hooks:** add a **Product Hooks** element so plugin output (that hooks into
   WooCommerce) still appears.

---

## Ready-made custom product layouts (paste into a CPL block)

### Left sidebar (full-height)
```
[row]
[col span="3" span__sm="12"]
[ux_sidebar id="product-sidebar"]
[/col]
[col span="9" span__sm="12"]
[row_inner]
[col_inner span="6" span__sm="12"]
[ux_product_gallery]
[/col_inner]
[col_inner span="6" span__sm="12"]
[ux_product_breadcrumbs]
[ux_product_title]
[ux_product_rating]
[ux_product_price]
[ux_product_excerpt]
[ux_product_add_to_cart]
[ux_product_meta]
[share]
[/col_inner]
[/row_inner]
[ux_product_tabs]
[ux_product_upsell style="grid"]
[ux_product_related]
[/col]
[/row]
```

### Right sidebar (full-height)
```
[row]
[col span="9" span__sm="12"]
[row_inner]
[col_inner span="6" span__sm="12"]
[ux_product_gallery]
[/col_inner]
[col_inner span="6" span__sm="12"]
[ux_product_breadcrumbs]
[ux_product_title]
[ux_product_rating]
[ux_product_price]
[ux_product_excerpt]
[ux_product_add_to_cart]
[ux_product_meta]
[share]
[/col_inner]
[/row_inner]
[ux_product_tabs]
[ux_product_upsell style="grid"]
[ux_product_related]
[/col]
[col span="3" span__sm="12"]
[ux_sidebar id="product-sidebar"]
[/col]
[/row]
```

### Wide gallery (full-width gallery, info in colored box)
```
[ux_product_gallery style="full-width"]
[row]
[col span="7" span__sm="12"]
[ux_product_breadcrumbs]
[ux_product_title]
[ux_product_excerpt]
[share]
[/col]
[col span="5" span__sm="12" padding="30px 30px 30px 30px" bg_color="rgba(233, 228, 228, 0.67)"]
[ux_product_price]
[ux_product_add_to_cart]
[ux_product_meta]
[/col]
[/row]
[row]
[col span__sm="12"]
[ux_product_tabs]
[ux_product_upsell style="grid"]
[ux_product_related]
[/col]
[/row]
```

### Standard left sidebar (3 / 6 / 3)
```
[row]
[col span="3" span__sm="12"]
[ux_sidebar id="product-sidebar"]
[/col]
[col span="6" span__sm="12"]
[ux_product_gallery]
[/col]
[col span="3" span__sm="12"]
[ux_product_breadcrumbs]
[ux_product_title]
[ux_product_rating]
[ux_product_price]
[ux_product_excerpt]
[ux_product_add_to_cart]
[ux_product_meta]
[share]
[/col]
[/row]
[ux_product_tabs]
[ux_product_upsell style="grid"]
[ux_product_related]
```

### Standard right sidebar (3 / 6 / 3, info first)
```
[row]
[col span="3" span__sm="12"]
[ux_product_breadcrumbs]
[ux_product_title]
[ux_product_rating]
[ux_product_price]
[ux_product_excerpt]
[ux_product_add_to_cart]
[ux_product_meta]
[share]
[/col]
[col span="6" span__sm="12"]
[ux_product_gallery]
[/col]
[col span="3" span__sm="12"]
[ux_sidebar id="product-sidebar"]
[/col]
[/row]
[ux_product_tabs]
[ux_product_upsell style="grid"]
[ux_product_related]
```

### Left sidebar small (2 / 4 / 6)
```
[row]
[col span="2" span__sm="12"]
[ux_sidebar id="product-sidebar"]
[/col]
[col span="4" span__sm="12"]
[ux_product_breadcrumbs]
[ux_product_title]
[ux_product_rating]
[ux_product_price]
[ux_product_excerpt]
[ux_product_add_to_cart]
[ux_product_meta]
[share]
[/col]
[col span="6" span__sm="12"]
[ux_product_gallery]
[/col]
[/row]
[ux_product_tabs]
[ux_product_upsell style="grid"]
[ux_product_related]
```

### Right sidebar small (4 / 6 / 2)
```
[row]
[col span="4" span__sm="12"]
[ux_product_breadcrumbs]
[ux_product_title]
[ux_product_rating]
[ux_product_price]
[ux_product_excerpt]
[ux_product_add_to_cart]
[ux_product_meta]
[share]
[/col]
[col span="6" span__sm="12"]
[ux_product_gallery]
[/col]
[col span="2" span__sm="12"]
[ux_sidebar id="product-sidebar"]
[/col]
[/row]
[ux_product_tabs]
[ux_product_upsell style="grid"]
[ux_product_related]
```

---

## Shop header with UX Builder

Theme Options → Shop lets you enable a **Custom Shop Page** header built with UX Builder
(banner over the category archive). Edit it via the frontend admin bar on a shop/category page.

## Useful WooCommerce-related filters (see hooks.md)

- `flatsome_show_buy_now_button` — toggle "Buy now" button
- `flatsome_sticky_add_to_cart_enabled` — toggle sticky add-to-cart bar
- `flatsome_product_labels` — customize sale/new labels
- `flatsome_payment_icons` — footer payment icons
- Swatch filters (`flatsome_swatch_html`, `flatsome_swatches_box_attribute`, ...) — variation
  swatches behavior

## WooCommerce setup articles (reference)

The docs include full setup guides: installing, general/product/tax/shipping/checkout/account
settings, product types (simple, grouped, variable, external/affiliate, downloadable), and
coupons — under https://docs.uxthemes.com/category/201-woocommerce.
