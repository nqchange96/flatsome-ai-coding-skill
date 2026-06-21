# Flatsome Theme Hooks — Actions & Filters

Source: https://docs.uxthemes.com/article/385-hooks (Flatsome 3.20.0).
Use these to customize behavior **without editing core theme files**. Put code in a
**child theme** `functions.php`.

- **Actions** = inject/output content at a location: `add_action('hook', 'callback');`
- **Filters** = modify a value: `add_filter('hook', 'callback');` (callback must `return`).

```php
// Action example: add a notice above the footer
add_action('flatsome_before_footer', function () {
    echo '<div class="container text-center">Free shipping over $50</div>';
});

// Filter example: add a custom CSS class to every product box
add_filter('flatsome_product_box_classes', function ($classes) {
    $classes[] = 'my-custom-box';
    return $classes;
});
```

---

## Action hooks (67)

Grouped for orientation; names are exact.

### Body / page / 404
```
flatsome_after_body_open
flatsome_before_404            flatsome_after_404
flatsome_before_page           flatsome_after_page
flatsome_before_page_content   flatsome_after_page_content
```

### Header
```
flatsome_before_header         flatsome_after_header
flatsome_after_header_bottom
flatsome_header_background
flatsome_header_elements
flatsome_header_wrapper
```

### Footer
```
flatsome_before_footer         flatsome_after_footer
flatsome_footer
flatsome_absolute_footer_primary
flatsome_absolute_footer_secondary
```

### Sidebar menu (off-canvas / mobile)
```
flatsome_before_sidebar_menu            flatsome_after_sidebar_menu
flatsome_before_sidebar_menu_elements   flatsome_after_sidebar_menu_elements
```

### Blog / post / breadcrumb / comments
```
flatsome_before_blog           flatsome_after_blog
flatsome_blog_post_before      flatsome_blog_post_after
flatsome_before_breadcrumb     flatsome_after_breadcrumb   flatsome_breadcrumb
flatsome_before_comments
flatsome_category_title        flatsome_category_title_alt
```

### Account
```
flatsome_account_links
flatsome_after_account_user
```

### Single product
```
flatsome_before_product_page          flatsome_after_product_page
flatsome_before_product_images        flatsome_after_product_images
flatsome_before_product_sidebar
flatsome_product_title                flatsome_product_title_tools
flatsome_product_image_tools_top      flatsome_product_image_tools_bottom
flatsome_before_single_product_custom
flatsome_before_single_product_lightbox   flatsome_after_single_product_lightbox
flatsome_single_product_lightbox_product_gallery
flatsome_single_product_lightbox_summary
flatsome_sale_flash
```

### Product loop / product box (grid card)
```
flatsome_products_before              flatsome_products_after
flatsome_product_box_tools_top       flatsome_product_box_tools_bottom
flatsome_product_box_actions
flatsome_product_box_after
flatsome_woocommerce_shop_loop_images
flatsome_product_attribute_term_fields
```

### Mini cart (side cart)
```
flatsome_cart_sidebar
flatsome_before_mini_cart_total
flatsome_after_mini_cart_contents
flatsome_before_mini_cart_cross_sells   flatsome_after_mini_cart_cross_sells
flatsome_before_mini_cart_empty_message flatsome_after_mini_cart_empty_message
```

### Portfolio (custom post type)
```
flatsome_portfolio_title_left
flatsome_portfolio_title_right
flatsome_portfolio_title_after
```

---

## Filter hooks (67)

### Markup / classes
```
flatsome_html_atts
flatsome_main_class
flatsome_header_class           flatsome_header_title_class
flatsome_header_element
flatsome_sidebar_class
flatsome_product_box_classes    flatsome_product_box_text_classes
flatsome_product_box_image_classes
flatsome_product_box_actions_classes
flatsome_single_product_thumbnails_classes
flatsome_relay_classes          flatsome_relay_control_classes
flatsome_icon
flatsome_viewport_meta
```

### Header / account / menu
```
flatsome_header_account_username
flatsome_admin_menu_items_enabled
```

### AJAX search
```
flatsome_ajax_search_function
flatsome_ajax_search_post_type
flatsome_ajax_search_query
flatsome_ajax_search_products_search_query
flatsome_ajax_search_products_by_sku_search_query
flatsome_ajax_search_products_by_sku_search_meta_query_args
flatsome_ajax_search_products_by_tag_search_query
```

### Product / shop behavior
```
flatsome_product_block
flatsome_product_block_primary_term_id
flatsome_product_block_product_terms_args
flatsome_product_labels
flatsome_new_flash_html
flatsome_sale_bubble_percentage_cache_enabled
flatsome_show_buy_now_button
flatsome_sticky_add_to_cart_enabled
flatsome_loop_review_count_html
flatsome_woocommerce_shop_loop_category
flatsome_woocommerce_get_alt_product_thumbnail
flatsome_custom_product_single_product_hooks
flatsome_is_shop_archive        flatsome_is_blog_archive
```

### Swatches (variation swatches)
```
flatsome_swatch_html
flatsome_swatch_image_size
flatsome_swatches_box_attribute
flatsome_swatches_box_display_min_count
flatsome_swatches_cache_enabled
flatsome_swatches_cache_time
```

### Mini cart
```
flatsome_disable_mini_cart
flatsome_show_mini_cart_item_quantity
flatsome_mini_cart_cross_sells_total
flatsome_mini_cart_cross_sells_order
flatsome_mini_cart_cross_sells_orderby
```

### Lightbox
```
flatsome_lightbox_close_button
flatsome_lightbox_close_btn_inside
```

### Shipping / payment / icons / links
```
flatsome_payment_icons
flatsome_shipping_free_shipping_threshold
flatsome_share_links
flatsome_follow_links
```

### Infinite scroll / relay (load more)
```
flatsome_infinite_scroll_params
flatsome_relay_pagination_args
```

### Misc / content
```
flatsome_text_formats
flatsome_dummy_text
flatsome_attachment_size
flatsome_author_bio_avatar_size
flatsome_maintenance_mode
flatsome_cache_clear_items
flatsome_before_body_close_priority
flatsome_show_block_edit_tooltip
flatsome_single_product_thumbnails_render_without_attachments
flatsome_wpseo_breadcrumb_remove_last
```

---

## Common recipes

**Add free-shipping bar above header:**
```php
add_action('flatsome_before_header', function () {
    echo '<div class="header-top-bar text-center">Free shipping over $50</div>';
});
```

**Add a badge inside each product card:**
```php
add_action('flatsome_product_box_tools_top', function () {
    global $product;
    if ($product->is_featured()) echo '<span class="badge">Hot</span>';
});
```

**Disable the sticky add-to-cart bar:**
```php
add_filter('flatsome_sticky_add_to_cart_enabled', '__return_false');
```

**Customize payment icons in footer:** filter `flatsome_payment_icons` — see
[snippets.md](snippets.md).

**Customize share/follow buttons:** filters `flatsome_share_links` / `flatsome_follow_links` —
see [snippets.md](snippets.md).
