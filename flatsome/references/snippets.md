# Code Snippets

Copy-paste snippets for common Flatsome customizations. PHP goes in **child theme**
`functions.php` (see [customization.md](customization.md)). Based on docs.uxthemes.com
category 315 (Snippets) and the documented filter names in [hooks.md](hooks.md).

> Snippet features generally require recent Flatsome (3.17+ for share/follow filters).

---

## Payment icons (footer)

Filter: `flatsome_payment_icons`. Built-in icons include `visa`, `mastercard`, `amex`,
`discover`, `paypal`, `apple-pay`, `google-pay`, `klarna`, `bitcoin`, `stripe`, etc.
Enable display in Theme Options → Footer → Payment Icons, then customize:

```php
add_filter('flatsome_payment_icons', function ($icons) {
    // keep only some
    return ['visa', 'mastercard', 'paypal', 'apple-pay'];
});
```

Add a fully custom icon:
```php
add_filter('flatsome_payment_icons', function ($icons) {
    $icons['my-pay'] = '<img src="' . get_stylesheet_directory_uri() . '/img/my-pay.svg" alt="MyPay">';
    return $icons;
});
```

---

## Share links

Filter: `flatsome_share_links`. Add/remove/reorder the `[share]` element's buttons.

```php
// Add a new share link
add_filter('flatsome_share_links', function ($links) {
    $links['telegram'] = [
        'priority' => 50,
        'enabled'  => true,
        'url'      => 'https://t.me/share/url?url={url}&text={title}',
        'icon'     => 'icon-telegram',
        'label'    => 'Telegram',
        'nofollow' => true,
    ];
    return $links;
});

// Change priority / remove an existing one
add_filter('flatsome_share_links', function ($links) {
    if (isset($links['facebook'])) $links['facebook']['priority'] = 5;
    unset($links['email']);
    return $links;
});
```

---

## Follow links

Filter: `flatsome_follow_links` — same shape as share links, for the `[follow]` element.

```php
add_filter('flatsome_follow_links', function ($links) {
    $links['tiktok'] = [
        'priority' => 60,
        'enabled'  => true,
        'url'      => 'https://tiktok.com/@yourbrand',
        'icon'     => 'icon-tiktok',
        'label'    => 'TikTok',
    ];
    return $links;
});
```

---

## Enable UX Builder for a custom post type

To make UX Builder available on your own CPT: register it with editor support and ensure the
CPT's single template outputs `the_content()`. Then expose it to the builder:

```php
// 1) Register the CPT with editor support
add_action('init', function () {
    register_post_type('project', [
        'label'        => 'Projects',
        'public'       => true,
        'has_archive'  => true,
        'supports'     => ['title', 'editor', 'thumbnail'],
        'show_in_rest' => true,
    ]);
});

// 2) Allow UX Builder on this post type
add_filter('ux_builder_post_types', function ($post_types) {
    $post_types[] = 'project';
    return $post_types;
});
```
**Important:** the CPT's single template must call `the_content();` so the shortcode output
renders. If using a generic template you usually get this for free.

---

## Sticky add-to-cart / buy-now buttons

```php
// Disable the sticky add-to-cart bar on product pages
add_filter('flatsome_sticky_add_to_cart_enabled', '__return_false');

// Hide the "Buy now" button
add_filter('flatsome_show_buy_now_button', '__return_false');
```

---

## Mini cart

```php
// Disable the side mini-cart entirely
add_filter('flatsome_disable_mini_cart', '__return_true');

// Show item quantity badge in mini cart
add_filter('flatsome_show_mini_cart_item_quantity', '__return_true');
```

---

## Lightbox close button

Filters: `flatsome_lightbox_close_button`, `flatsome_lightbox_close_btn_inside`.

```php
// Place the close (X) button inside the lightbox
add_filter('flatsome_lightbox_close_btn_inside', '__return_true');

// Customize the close button markup
add_filter('flatsome_lightbox_close_button', function ($html) {
    return '<button class="mfp-close">×</button>';
});
```

---

## Infinite scroll: disable history (URL changes)

Filter: `flatsome_infinite_scroll_params`. Stops the URL from updating while infinite-scrolling.

```php
add_filter('flatsome_infinite_scroll_params', function ($params) {
    $params['history'] = false;   // don't push page URLs into browser history
    return $params;
});
```

---

## Pjax (AJAX page transitions)

Flatsome Pjax loads pages via AJAX for faster navigation (toggle in Theme Options → Advanced).
Re-init your custom JS after a Pjax page load:

```js
// in a custom script
jQuery(document).on('flatsome-pjax-complete', function () {
    // re-run your init code here
});
```
(Event names can vary by version — inspect emitted events if needbe.)

---

## Product box (card) classes

```php
add_filter('flatsome_product_box_classes', function ($classes) {
    $classes[] = 'has-hover-actions';
    return $classes;
});
```

---

## Add content via hooks (most common pattern)

```php
// Promo bar above the header
add_action('flatsome_before_header', function () {
    echo '<div class="promo-bar text-center" style="background:#111;color:#fff;padding:8px">'
       . 'Free shipping on orders over $50</div>';
});

// Trust badges under add-to-cart
add_action('flatsome_after_product_page', function () {
    echo do_shortcode('[ux_image id="123"]');
});
```

See [hooks.md](hooks.md) for the full list of 67 actions + 67 filters.
