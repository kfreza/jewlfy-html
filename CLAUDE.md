# Jewlfy — Project Conventions

Static HTML/SCSS/jQuery **jewelry & accessories eCommerce template** by Laralink. Every CSS class uses the **`cs_`** prefix (snake_case, not BEM) — match it. All "eCommerce" behavior (cart, wishlist, quantity) is a **client-side `localStorage` simulation** — there is no backend.

## Build / Run
- **No bundler, no `package.json`.** Sass is compiled manually. Don't introduce npm/webpack/vite.
- Just open any `*.html` in a browser to preview.
- After editing any `assets/scss/**/*.scss`, recompile to `assets/css/style.css`:
  ```
  sass assets/scss/style.scss assets/css/style.css --no-source-map --style=expanded
  ```
- **Never edit `assets/css/style.css` directly** — it is generated. Edit the SCSS source.

## File layout
- Pages live at the repo root as siblings: `index.html`, `shop.html`, `product-details.html`, `cart.html`, `checkout.html`, `blog.html`, etc. Add new pages as siblings. Many pages ship as numbered variants (`home-v2..v6`, `shop-v2..v6`, `product-details-v2..v7`, `blog-v2..v5`).
- `assets/scss/` — note the folder is **`scss`**, not `sass`:
  - `default/` — `_variable`, `_mixins`, `_fonts`, `_typography`
  - `common/` — `_general` (base + utility classes), `_header`, `_footer`, `_slider`, `_video-modal`
  - `shortcode/` — one partial per component (`_products`, `_hero`, `_cart`, `_about`, …)
  - All wired through `assets/scss/style.scss`, which has a numbered Table of Contents.
- `assets/js/main.js` — single IIFE `(function ($) { ... })(jQuery)` containing every behavior.
- `assets/img/` — descriptive snake_case names (`hero-bg-1.jpg`, `category-rings.jpg`); subfolders `categories/` and `icons/` exist.

## HTML patterns
- Wrap each page section with comments: `<!-- Start X Section -->` … `<!-- End X Section -->`. Indent 2 spaces.
- Section root: `<section class="cs_<name>_section">` (e.g. `cs_blog_section`, `cs_cart_section`). A numeric `_1` variant slot exists but is used sparingly — omit it unless building an alternate.
- Component variants use a `_style_N` suffix: `cs_product_style_1`, `cs_btn_style_1`, `cs_footer_style_1`.
- **Compose classes**: a structural class + atomic utility classes on the same element, e.g.
  `class="cs_product_badge cs_accent_bg cs_fs_12 cs_white_color"`.
- Utility classes (defined in `common/_general.scss`):
  - Font size: `cs_fs_12`, `cs_fs_14`, `cs_fs_18`, `cs_fs_22`, `cs_fs_36`, …
  - Weight: `cs_medium`, `cs_semibold`
  - Color: `cs_primary_color`, `cs_accent_color`, `cs_white_color`
  - Background: `cs_accent_bg`, `cs_primary_bg`, `cs_gray_bg`, `cs_white_bg`
  - Reset / helper: `cs_mp_0` (margin+padding reset), `cs_center`
- **Spacing**: use Bootstrap margins (`mb-0`) + section `padding` defined in SCSS. Gap utilities `cs_gap_y_24 / _30 / _40 / _48` exist. Do **not** invent spacer divs.
- Layout: Bootstrap grid (`container`, `row`, `col-lg-*`).
- Backgrounds via `data-src="path/to.jpg"` on `.cs_bg_filed` — `main.js` sets `background-image`. Don't inline `style="background-image:..."`.
- Icon-only buttons/links get an `aria-label`; all `<img>` get meaningful `alt`.
- Icons use Remix Icon: `<i class="ri-...-line"></i>`.

## SCSS patterns
- New component → new partial `assets/scss/shortcode/_<name>.scss` and `@import` it from `style.scss` (add it to the TOC comment too).
- New layout-level primitive → `assets/scss/common/_<name>.scss`.
- Use the CSS custom properties from `default/_variable.scss` — don't hardcode brand colors or fonts:
  - Colors: `--primary-color: #2C2C2C`, `--accent-color: #D4AF37` (gold), `--secondary-color`, grays `--gray-color`..`--gray6-color`, `--error-color`, `--success-color`, `--border-color`.
  - Fonts: `--primary-font: "Playfair Display", serif` (headings), `--secondary-font: "Inter", sans-serif` (body).
- Responsive: use the breakpoint mixins from `default/_mixins.scss`.
  - `@include cs-respond("tablet") { ... }` — max-width. Named map: `mobile 575`, `tablet 767`, `tablet-lg 991`, `laptop 1199`, `desktop 1399` (or pass a raw px value).
  - `@include cs-respond-up("laptop") { ... }` — min-width, for the occasional large-screen rule.

## JS patterns ([assets/js/main.js](assets/js/main.js))
- Add new behaviors as a numbered, comment-banner-separated `function` inside the IIFE, then call it from the init block on DOM-ready (`$(function () { ... })`).
- Guard DOM lookups with `$.exists(selector)` (defined at top).
- Drive configuration through **data attributes**, not per-page inline scripts (`data-src`, and the Swiper wrapper reads `data-thumbs`, `data-slides-per-view`, `data-loop`, `data-autoplay`, `data-effect`, `data-direction`, `data-speed`, …).
- Keep styling and behavior decoupled: JS hooks use **un-prefixed** classes (e.g. `.addToCartBtn`), while `cs_` classes are for styling.
- Match existing jQuery idioms. Libraries in use: jQuery, Bootstrap, **Swiper**, nice-select, flatpickr, LightGallery, Odometer, Remix Icon. Don't introduce ES modules, bundlers, or framework code.
- Cart/wishlist/quantity persist via `localStorage` only — no server calls.

## Branding gotchas
- Author meta is **"Laralink"**; the `main.js` header comment reads **"Template Name: Jewlfy"**.
- Copyright is **© 2026**.
- Topbar language switcher `data-lang` values are `english` / `french` / `deutsch` / `arabic`. `languageSwitch()` writes the raw `data-lang` value straight into the visible label, so keep them clean and lowercase.

## Style of work
- **No emojis** in markup or code.
- Don't refactor unrelated code while adding a feature.
- Preserve existing commented-out blocks — they're intentional, not dead code to clean.
