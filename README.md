# Assignment 1 Build Your Own Storefront-Part 1

**Note:** This project was migrated from the Dawn theme to the Horizon
theme per lecturer instruction. Horizon uses Shopify's newer theme
blocks architecture (see the `blocks/` folder) rather than Dawn's
section-only structure.

Niche: Specialty coffee — single-origin and small-batch roasts.
Coffee products naturally support multiple variant dimensions — roast level (light/medium/dark), grind type (whole bean, drip, espresso, French press), and bag size (250g/500g/1kg). That's enough to build real variant-driven filtering. It also supports rich metafields per product: origin country, farm/producer, altitude, processing method (washed, natural, honey), and tasting notes — good material for structured content later in the week.

Target audience:
Our customers are home brewing enthusiasts in their late 20s to 40s who care about origin transparency and are willing to pay a premium for traceable, small-batch coffee. They're comfortable ordering coffee online on a recurring basis and want detailed sourcing information before they buy.

3 custom pages + Metaobject needs:
- Origin/Farm Profiles — one page per producer/farm, built from a farm_profile Metaobject (fields: farm name, country, altitude, story, photo).
- Brew Guides — a page per brew method, using a brew_guide Metaobject (fields: method name, equipment, ratio, steps).
- Subscription/How It Works — explains the coffee subscription model, could use a faq_item Metaobject for repeat-question content.

## Dev Environment

Store: the-roast-office.myshopify.com
Theme base: Horizon (migrated from Dawn)
Local project repo: LikaHanekom/Build_your_own_storefront_Horizon

Hot-reloading confirmed: verified by editing layout/theme.liquid, adding
a temporary <h1>TEST</h1> tag, and observing the local preview
(http://127.0.0.1:9292) auto-refresh with the change.

## Stretch Goal A — GitHub Integration via Admin

Connected the `main` branch of my personal GitHub repository
(LikaHanekom/Build_your_own_storefront_Horizon) to the store's theme via
Online Store > Themes > Import > Connect from GitHub.

**CLI vs. Theme Editor with GitHub sync active:**
When a theme is connected to a GitHub branch, changes made locally via
the CLI (editing files in VS Code, then `git commit` + `git push`) sync
to Shopify automatically once pushed to the connected branch — GitHub
becomes the source of truth. Changes made directly in the Shopify Theme
Editor, however, are not automatically pushed back to GitHub. They stay
live on the theme but only exist in Shopify's system until someone
manually pulls them into the repo, or they get overwritten by the next
GitHub push. This means Theme Editor edits can silently be lost if a
developer later pushes local changes without reconciling them first —
so teams using GitHub sync typically treat the Theme Editor as
read-only/preview during active development and route all real changes
through git.

## Stretch Goal B — VS Code Configuration

Installed the official Shopify Liquid VS Code extension and configured
`.vscode/settings.json` to auto-format Liquid files on save:

\`\`\`json
{
  "editor.defaultFormatter": "Shopify.theme-check-vscode",
  "[liquid]": {
    "editor.defaultFormatter": "Shopify.theme-check-vscode"
  },
  "editor.formatOnSave": true
}
\`\`\`

This scopes the formatter specifically to the `liquid` language and
enables format-on-save globally, so `.liquid` files are auto-corrected
by Shopify's official Theme Check formatter every time they're saved.


# Shopify Theme Development Module - Day 2 Assignment
## Liquid Fundamentals

**Student:** Alika Hanekom

---

# Part 1 – Written Decisions

## Step 1.1 – Filter Inventory

| Filter | Target File | What it Changes |
|---------|-------------|-----------------|
| `money` | `sections/product-information.liquid` | Formats `product.price` into the store's currency format instead of displaying the raw value. |
| `image_url` | `sections/product-information.liquid` | Resizes the product featured image to 600px wide for the summary block. |
| `divided_by` | `sections/product-information.liquid` | Calculates the image's `height` attribute from width and aspect ratio, to prevent layout shift. Added during implementation to satisfy Theme Check's missing-height-attribute rule; not part of the original Step 1.1 plan. |
| `strip_html` | `sections/product-information.liquid` | Removes HTML tags from `product.description` before display. |
| `truncate` | `sections/product-information.liquid` | Shortens `product.description` to 150 characters to keep the summary block compact. |
| `upcase` | `blocks/_product-card.liquid` | Displays `product.title` in uppercase on the collection page card. |

## Step 1.2 – Conditional Logic Plan

**Object/property:** `product.available`

**File:** `sections/product-information.liquid`

**Branches:**
- **True:** displays "In Stock" (`.product-summary-block__availability--in-stock`)
- **False:** displays "Out of Stock" (`.product-summary-block__availability--out-of-stock`)

---

# Part 2 – Product Page Changes

The following filters were implemented on the product page (`sections/product-information.liquid`):

1. **money** — formats `product.price` into store currency.
2. **image_url** + **divided_by** — resizes the featured image and computes a proportional `height` to avoid layout shift.
3. **strip_html** — strips HTML tags from the product description.
4. **truncate** — limits the description to 150 characters.

The `product.available` conditional was also added here, toggling between "In Stock" and "Out of Stock."

---

# Part 3 – Collection Page Changes

The collection page card (`blocks/_product-card.liquid`) uses the remaining filter:

- **upcase** — displays `product.title` in uppercase on each collection page card.

Combined filter count across product and collection pages: 6 distinct filters (5 required minimum).

---

# Verification Notes

- **Out of Stock branch:** Viewed "Selling Plans Ski Wax" (marked Sold out in Shopify admin) at [PASTE PRODUCT URL OR NOTE "via admin theme editor preview"] → page displayed **"Out of Stock."**
- **In Stock branch:** Viewed "[PASTE ACTUAL PRODUCT NAME, e.g. a snowboard]" (in-stock item) → page displayed **"In Stock."**
- Collection page verified: all product cards display uppercase titles alongside the regular title, no Liquid errors.
- Local `shopify theme dev` (127.0.0.1:9292) returns a persistent "Upload Errors" page due to a pre-existing schema/version mismatch in the base Horizon theme — confirmed unrelated to my edits via `git stash` (same error occurs with zero local changes).
- Verification was instead performed via the Shopify admin theme editor preview (`/admin/themes/.../editor`), where both the product page and collection page rendered correctly with all filters and the conditional visible.

---

# Git Commands

```bash
git add .
git commit -m "Day 2: add filters and conditional logic to product and collection sections"
git push
```

---

# Assignment Checklist

## Part 1
- [x] Five or more distinct Liquid filters identified.
- [x] Target file listed for each filter.
- [x] Effect of each filter explained.
- [x] Conditional logic planned using a real Shopify object.

## Part 2
- [x] Three or more filters implemented on the product page.
- [x] Availability conditional implemented.
- [x] Product page renders without Liquid errors.

## Part 3
- [x] Six distinct filters used across product and collection pages.
- [x] Collection page renders successfully.

## Part 4
- [x] Verified in Shopify admin theme editor preview (local CLI preview blocked by pre-existing theme issue, documented above).
- [x] Both conditional branches verified.
- [x] Changes committed and pushed to GitHub.


---

# Shopify Theme Development Module - Day 3 Assignment
## Sections, Blocks & Schema


---

# Part 1 – Written Decisions

## Step 1.1 – Section Concept

**Name:** Origin Spotlight
**File:** `sections/origin-spotlight.liquid`
**Page:** Homepage (default)

**Purpose:** Lets a merchant build a homepage story block around a single coffee origin/farm — combining a hero-style intro (heading + background image) with a flexible mix of content blocks (stats, a pull-quote from the farmer, and a CTA button) — without needing a developer to hand-code each new farm feature. This is an original concept, distinct from Horizon's existing "Featured collection," "Rich text," or "Image with text" sections.

## Step 1.2 – Block Inventory

| Block Type | Filename | Represents | Reusable Elsewhere? |
|---|---|---|---|
| `origin-stat` | `blocks/origin-stat.liquid` | A single stat callout (label + value pair), e.g. "Altitude: 1,800m" | Could be reused on a future Farm Profile page |
| `origin-quote` | `blocks/origin-quote.liquid` | A pull-quote attributed to the farmer/producer, with name and farm name | Specific to storytelling sections like this one |
| `origin-cta` | `blocks/origin-cta.liquid` | A button with configurable label, link, and style | Reusable generic CTA pattern |

## Step 1.3 – Settings Plan

| Setting ID | Type | File | Visible Effect |
|---|---|---|---|
| `heading` | `text` | `sections/origin-spotlight.liquid` | Sets the section's main heading text (e.g. the origin/farm name) |
| `background_image` | `image_picker` | `sections/origin-spotlight.liquid` | Sets the background/hero image behind the section |
| `stat_value` | `text` | `blocks/origin-stat.liquid` | Sets the number/value shown in a stat block (e.g. "1,800m") |
| `quote_text` | `richtext` | `blocks/origin-quote.liquid` | Sets the quote content displayed in the pull-quote block |
| `cta_style` | `select` (`solid` / `outline`) | `blocks/origin-cta.liquid` | Swaps the button's CSS class between a solid-filled and outline style |

Combined settings total: **5** (4+ required).

---



- Verified via the Shopify admin theme editor. Added Origin Spotlight section to the homepage with one Origin Stat, one Origin Quote, and one Origin CTA block. Confirmed heading and background image render; confirmed stat label/value, quote text, and attribution display; toggled cta_style between solid and outline and confirmed the button's CSS class and appearance changed.



- All four schemas (origin-spotlight.liquid, origin-stat.liquid, origin-quote.liquid, origin-cta.liquid) were validated in the Shopify Code Editor with zero errors, using t: translation keys defined in locales/en.default.schema.json.

## Stretch A 

— Conditional Settings: blocks/origin-cta.liquid has a subtext text field that only appears in the editor when show_subtext (a checkbox) is enabled, via visible_if. This lets merchants add optional supporting text under the CTA button without cluttering the settings panel when it's not needed.

### Stretch B — Translation Pass

All schema-facing strings (section/block names, setting labels, categories) were already implemented using `t:` keys during Part 2 and Part 3, rather than left as plain strings. Keys added to `locales/en.default.schema.json`:

**names:**
- `origin_spotlight`
- `origin_stat`
- `origin_quote`
- `origin_cta`

**settings:**
- `origin_spotlight_heading`
- `origin_spotlight_background_image`
- `origin_stat_label`
- `origin_stat_value`
- `origin_quote_text`
- `origin_quote_attribution_name`
- `origin_quote_attribution_farm`
- `origin_cta_label`
- `origin_cta_link`
- `origin_cta_style`
- `origin_cta_show_subtext`
- `origin_cta_subtext`

All labels use sentence case (e.g. "Attribution name", not "Attribution Name" or "attribution name"), consistent with the existing conventions in `en.default.schema.json`.

# Day 4 — Metafields & Metaobjects

## Part 1 — Written Decisions

### Step 1.1 — Metafield Plan

- **Resource type:** Product
- **Namespace.key:** `custom.altitude_meters`
- **Type:** Number (integer)
- **Storefront display:** The growing altitude in meters for the origin's featured coffee, shown as a stat line (e.g. "Altitude: 1,750m") inside `blocks/origin-stat.liquid`, replacing the block's manually-typed `value` field when a product is attached to the section.

### Step 1.2 — Metaobject Plan

- **Metaobject type:** `origin_profile`
- **Fields:**
  - `farm_name` — single line text
  - `harvest_notes` — rich text
- **Real-world content it represents:** A single origin/farm profile (farm name plus tasting and harvest notes) that is reusable across every product sourced from that farm, rather than data tied to one specific product.
- **Reference method:** Products reach it through a metafield reference — `custom.origin_profile`, type "Metaobject reference" — following the same pattern `blocks/disclosures.liquid` already uses with `shopify.disclosure`.

### Step 1.3 — Integration Plan

- **Section change:** Add a `product` setting (type: `product`) to `sections/origin-spotlight.liquid`, so the section can be pointed at a specific featured product.
- **Metafield renders in:** `blocks/origin-stat.liquid`, pulling `section.settings.product.metafields.custom.altitude_meters`.
  - **Blank state:** if the metafield is empty, the block falls back to its existing manual `label`/`value` fields. If those are also empty, no altitude line renders at all.
- **Metaobject renders in:** `blocks/origin-quote.liquid`, pulling the referenced `origin_profile` via `section.settings.product.metafields.custom.origin_profile.value`, and rendering `farm_name` and `harvest_notes` beneath the existing pull-quote.
  - **Blank state:** if no product is selected on the section, or no `origin_profile` reference exists on that product, the quote block renders exactly as it does today — attribution only, with no empty wrapper added.
## Part 2 — Admin Definitions

- **Product metafield:** created via Settings > Custom data > Products. Note: Shopify auto-prefixed the key, so the actual namespace.key is `custom.custom_altitude_meters` (not `custom.altitude_meters` as originally planned in Part 1). Type: Integer. Populated with a real value (`1750`) on the test product.
- **Metaobject type:** `origin_profile` created via Content > Metaobjects, with fields `farm_name` (single line text) and `harvest_notes` (rich text).
- **Metaobject entry:** one real entry created — "Finca Los Alpes" — with genuine harvest notes describing peak-ripeness harvest timing and tasting notes (citrus acidity, floral aroma). Status: Active.
- **Reference metafield:** created as `origin_profile_reference`; Shopify auto-prefixed the key, so the actual namespace.key is `custom.origin_profile_reference` (not `custom.origin_profile` as originally planned in Part 1). Type: Metaobject reference, pointing at the `origin_profile` type. Assigned to the test product, linked to the "Finca Los Alpes" entry.

## Part 3 — Integration Notes

- **File modified:** `sections/origin-spotlight.liquid` — added a new `product` setting (id: `product`, label: "Featured product") so the section can be pointed at a specific product.
- **Metafield rendering:** `blocks/origin-stat.liquid` checks `section.settings.product.metafields.custom.custom_altitude_meters`. If present, it renders the altitude as the stat (e.g. "Altitude — 1750m"). If blank, it falls back to the block's original manual `stat_label` / `stat_value` settings, unchanged from Day 3.
- **Metaobject rendering:** `blocks/origin-quote.liquid` checks `section.settings.product.metafields.custom.origin_profile_reference.value`. If present, it renders the referenced `farm_name` and `harvest_notes` beneath the existing pull-quote. If blank, the block renders exactly as it did in Day 3 — no new markup, no empty wrapper.
- Both blocks were edited in place; no new section or block file was created.

## Part 4 — Verification Notes

- Ran `shopify theme dev` and verified both states in the local preview at `http://127.0.0.1:9292`:
  - **Populated state:** with the test product selected as "Featured product," the stat block correctly displayed the real altitude value, and the quote block correctly displayed "Finca Los Alpes" plus its harvest notes.
  - **Blank state:** with a product that has no metafields set, the stat block fell back to its default label/value, and the quote block rendered identically to its Day 3 behavior — no empty wrapper or stray heading.
- Zero Liquid errors related to `origin-spotlight`, `origin-stat`, or `origin-quote` in either state. (Unrelated pre-existing schema warnings appear elsewhere in the base theme — e.g. `variant-picker.liquid`, `header-menu.liquid` — but none involve the files touched in this assignment.)
- Changes committed and pushed to the personal GitHub repo:
  ```
  git add .
  git commit -m "Day 4: surface metafield and metaobject content in origin-spotlight"
  git push
  ```

# Day 5 — Cart, AJAX & Interactivity

## Part 1 — Written Decisions (The Threshold & Messaging Plan)

### Step 1.1 — Threshold Plan

- **Setting id:** `enable_free_shipping_bar` (type: `checkbox`)
- **Setting id:** `free_shipping_threshold` (type: `number`)
- **Scope:** Global — added to `config/settings_schema.json`, inside the existing
  `"name": "t:names.cart"` group.
- **Reasoning:** Free shipping is a store-wide shipping/fulfillment policy, not a
  presentation choice that should vary by section. If this were section-scoped, a
  merchant could end up with the cart drawer promising free shipping at $75 while
  the cart page (a different section) promises it at $50, even though both draw
  from the exact same order and the exact same shipping policy. A single global
  setting keeps the drawer and the cart page consistent, and matches how merchants
  already think about "free shipping over $X" as one storefront-wide rule.
- **Default value:** `75` (i.e. $75.00, a realistic free-shipping threshold, not a
  placeholder number).
- **Disable toggle:** `enable_free_shipping_bar` — a checkbox that lets a merchant
  turn the entire feature off without deleting the threshold value they've configured
  (so they can toggle it back on later without re-entering it).

### Step 1.2 — Messaging Plan

- **Short of threshold (remaining state):**
  `You're {{ amount_remaining }} away from free shipping!`
  — where `{{ amount_remaining }}` is the live dollar amount computed from
  `cart.total_price` and `settings.free_shipping_threshold`, formatted with Liquid's
  `money` filter (e.g. "You're $12.50 away from free shipping!").

- **Threshold met (unlocked state):**
  `You've unlocked free shipping!`

- **Disabled or threshold = 0 fallback:**
  The entire shipping-bar block — message and progress track together — does not
  render at all. No empty wrapper `<div>`, no zero-width progress bar, nothing left
  behind in the DOM. This is enforced with a single guard clause wrapping the whole
  snippet:
  `{%- if settings.enable_free_shipping_bar and settings.free_shipping_threshold > 0 -%}`
  so that turning the setting off, or leaving the threshold at its unconfigured
  default of 0, produces identical (nonexistent) output.

### Step 1.3 — Integration Plan

- **Target file:** `snippets/cart-shipping-bar.liquid` (new file), rendered from
  inside `snippets/cart-drawer.liquid`, inside the `cart_items_children` capture
  block's non-empty-cart branch — specifically just above the existing
  `{% render 'cart-summary', section_id: 'cart-drawer-section' %}` line.

- **Confirming it sits inside the re-rendered section:** In `cart-drawer.liquid`,
  the `cart_items_children` capture is passed as the `children` param into:
```liquid
  {% render 'cart-items-component',
    children: cart_items_children,
    is_drawer: true,
    section_id: 'cart-drawer-section'
  %}
```
  This means my snippet's output becomes part of the children of
  `<cart-items-component data-section-id="cart-drawer-section">`. In
  `assets/component-cart-items.js`, the `sectionId` getter reads that exact
  `data-section-id` attribute:
```js
  get sectionId() {
    const { sectionId } = this.dataset;
    if (!sectionId) throw new Error('Section id missing');
    return sectionId;
  }
```
  So the section id this component re-renders under is confirmed to be
  `cart-drawer-section` — the same id my snippet is nested inside.

- **Does this require new JavaScript?** No. On cart change, `#handleCartUpdate`
  listens for `StandardEvents.cartLinesUpdate` on `document`, awaits the event's
  `promise`, and reads `detail.sections[this.sectionId]` — i.e.
  `sections['cart-drawer-section']` — off the cart-change response. If that HTML
  fragment is present, it calls:
```js
  morphSection(this.sectionId, cartItemsHtml, morphOptions);
```
  which morphs the new server-rendered HTML (my shipping bar included, since it's
  part of that section's output) directly into the existing DOM. Only if the
  `sections` map is missing that key does it fall back to
  `sectionRenderer.renderSection(this.sectionId, { cache: false, ...morphOptions })`
  as a true Section Rendering API fetch. Either path re-renders the whole
  `cart-drawer-section` fragment server-side and patches it into the DOM — since my
  snippet lives inside that fragment, it updates automatically with no additional
  fetch call, event listener, or custom element of my own.

# Day 6 — Collections, Filtering & Merchandising

## Filter & Swatch Plan

### 1.1 — Collection & Filter Plan
- **Collection:** `Week` (the collection holding Monday, Tuesday, Wednesday, Thursday roasts)
- **Filter dimension 1:** List filter on a new **Roast Level** product option (Light / Medium / Dark)
- **Filter dimension 2:** `price_range`
- **Data gap:** None of the four products in `Week` currently have any option set up (each is a
  single default variant), so there is nothing yet for a list filter or a swatch to attach to.
  Wednesday – Espresso Roast needs a real **Roast Level** option added with three values
  (Light, Medium, Dark) before the filter or swatches can appear. Additionally, all four
  products are currently priced/marked sold out identically-configured — pricing needs to be
  confirmed to vary across the collection, or `price_range` will render with a single-point
  range and be functionally useless as a filter.
- **Settings changed from default:**
  - `enable_filtering`: **false → true** (this block ships disabled by default — filters will
    not render at all until this is turned on)
  - `filter_style`: **horizontal → vertical** (sidebar layout, better fits a 4-product grid)
  - `enable_sorting`: left **on** (already true by default)

### 1.2 — Swatch Plan
- **Product:** Wednesday – Espresso Roast
- **Option:** Roast Level
- **Values / swatches:**
  - Light — pale tan swatch
  - Medium — medium brown swatch
  - Dark — dark espresso-brown swatch
- **Visible in:** both the collection grid card (`blocks/swatches.liquid`) and the product page
  variant picker (`blocks/variant-picker.liquid`'s `show_swatches` setting).
  `show_swatches` is already `true` by default on `variant-picker.liquid`, so no schema change
  is needed there — only the swatch color values need to be assigned to the option values in
  Admin. The `swatches.liquid` block already renders automatically once any option on the
  product has a swatch assigned (`product_has_swatches` check), so it does not need new settings
  either — just real swatch data.

### 1.3 — Customization Plan
- **File:** `blocks/filters.liquid`
- **Edit:** Added a new CSS rule to the block's existing `{% stylesheet %}` targeting
  `.facets--filters-title` (the "Filters" heading rendered next to the sort/count controls).
- **Visible change:** The heading now renders in a coffee-brown (`#4b2e2e`) color with slight
  letter-spacing instead of the theme's default foreground color, so it reads as on-brand for
  The Roast Office rather than generic theme text.

---

## Configuration Notes

- **Why no new section/block was created:** `sections/main-collection.liquid` already calls
  `content_for 'block', type: 'filters', id: 'filters', results: collection, results_size: collection.products_count`
  inside the collection wrapper grid — the filtering slot already exists and is already wired
  to the collection's product results, so building a new section or block would duplicate
  functionality that's already present and already correctly connected to `assets/facets.js`.
- **Where swatch data comes from:** `blocks/swatches.liquid` reads
  `product_option_value.swatch` directly off each product option's values
  (`closest.product.options_with_values`). Swatch color/image data lives on the product option
  values in Shopify Admin (Products → [product] → Options), not in the theme — the theme only
  renders whatever swatch is assigned there via `snippets/variant-swatches.liquid`.
- **Data change made:** Added a `Roast Level` option (Light / Medium / Dark) to
  Wednesday – Espresso Roast in Admin, which created 3 real variants where there was previously
  1 default variant.
- **Filters block settings changed:** `enable_filtering` → true (filters were entirely hidden
  before this), `filter_style` → vertical (sidebar layout for the small 4-item grid).

## Customization Notes

- File: `blocks/filters.liquid`
- Selector: `.facets--filters-title`
- Change: `color: #4b2e2e; letter-spacing: 0.04em;` added inside the block's existing
  `{% stylesheet %}` tag.
- Visible effect: the "Filters" label in the horizontal controls bar now renders in a
  coffee-brown brand color with slightly spaced-out letters, instead of the theme default
  foreground color.

## Verification Notes

_Complete this section after running `shopify theme dev` and testing at
`http://127.0.0.1:9292`, per Part 3 of the assignment:_

- [ ] Collection page loads with the filter panel and both Roast Level + Price filters visible
      on first load, before any filter is applied.
- [ ] Applying a filter and changing sort updates the grid and URL with **no full page reload**
      (confirmed via DevTools → Network: a facets/fetch request, not a document navigation).
- [ ] Copying the resulting filtered/sorted URL into a new tab reproduces the same view.
- [ ] Clicking a Roast Level swatch on the Wednesday product's grid card updates the card
      without navigating away.
- [ ] Opening the Wednesday product page shows the same 3 swatches driving the variant picker,
      and switching swatches updates price/media/availability correctly.
- [ ] Pagination/infinite scroll still works on the collection.
- [ ] Quick-add (if enabled) still works on a product that wasn't touched (Monday/Tuesday/Thursday).
- [ ] A product with no swatches assigned (Monday, Tuesday, Thursday) still renders its card and
      variant picker correctly, with no empty swatch row.
- [ ] `shopify theme check` is clean on `blocks/filters.liquid`.

# Horizon

[Getting started](#getting-started) |
[Staying up to date with Horizon changes](#staying-up-to-date-with-horizon-changes) |
[Developer tools](#developer-tools) |
[Contributing](#contributing) |
[License](#license)

Horizon is the flagship of a new generation of first party Shopify themes. It incorporates the latest Liquid Storefronts features, including [theme blocks](https://shopify.dev/docs/storefronts/themes/architecture/blocks/theme-blocks/quick-start?framework=liquid).

- **Web-native in its purest form:** Themes run on the [evergreen web](https://www.w3.org/2001/tag/doc/evergreen-web/). We leverage the latest web browsers to their fullest, while maintaining support for the older ones through progressive enhancement—not polyfills.
- **Lean, fast, and reliable:** Functionality and design defaults to "no" until it meets this requirement. Code ships on quality. Themes must be built with purpose. They shouldn't support each and every feature in Shopify.
- **Server-rendered:** HTML must be rendered by Shopify servers using Liquid. Business logic and platform primitives such as translations and money formatting don't belong on the client. Async and on-demand rendering of parts of the page is OK, but we do it sparingly as a progressive enhancement.
- **Functional, not pixel-perfect:** The Web doesn't require each page to be rendered pixel-perfect by each browser engine. Using semantic markup, progressive enhancement, and clever design, we ensure that themes remain functional regardless of the browser.

## Getting started

We recommend using the Skeleton Theme as a starting point for a theme development project. [Learn more on Shopify.dev](https://shopify.dev/themes/getting-started/create).

To create a new theme project based on Horizon:

```sh
git clone https://github.com/Shopify/horizon.git
```

Install the [Shopify CLI](https://shopify.dev/docs/storefronts/themes/tools/cli) to connect your local project to a Shopify store. Learn about the [theme developer tools](https://shopify.dev/docs/storefronts/themes/tools) available, and the suggested [developer tools](#developer-tools) below.

Please note that the `main` branch may include code for features not yet released. You may encounter Liquid API properties that are not publicly documented, but will be when the feature is officially rolled out.

### Shopify Theme Store development

If you're building a theme for the Shopify Theme Store, then do not use Horizon as a starting point. Themes based on, derived from, or incorporating Horizon are not eligible for submission to to the Shopify Theme Store. Use the [Skeleton Theme](https://github.com/Shopify/skeleton-theme) instead.

## Staying up to date with Horizon changes

Say you're building a new theme off Horizon but you still want to be able to pull in the latest changes, you can add a remote `upstream` pointing to this Horizon repository.

1. Navigate to your local theme folder.
2. Verify the list of remotes and validate that you have both an `origin` and `upstream`:

```sh
git remote -v
```

3. If you don't see an `upstream`, you can add one that points to Shopify's Horizon repository:

```sh
git remote add upstream https://github.com/Shopify/horizon.git
```

4. Pull in the latest Horizon changes into your repository:

```sh
git fetch upstream
git pull upstream main
```

## Developer tools

There are a number of really useful tools that the Shopify Themes team uses during development. Horizon is already set up to work with these tools.

### Shopify CLI

[Shopify CLI](https://shopify.dev/docs/storefronts/themes/tools/cli) helps you build Shopify themes faster and is used to automate and enhance your local development workflow. It comes bundled with a suite of commands for developing Shopify themes—everything from working with themes on a Shopify store (e.g. creating, publishing, deleting themes) or launching a development server for local theme development.

You can follow this [quick start guide for theme developers](https://shopify.dev/docs/themes/tools/cli) to get started.

### Theme Check

We recommend using [Theme Check](https://github.com/shopify/theme-check) as a way to validate and lint your Shopify themes.

We've added Theme Check to Horizon's [list of VS Code extensions](/.vscode/extensions.json) so if you're using Visual Studio Code as your code editor of choice, you'll be prompted to install the [Theme Check VS Code](https://marketplace.visualstudio.com/items?itemName=Shopify.theme-check-vscode) extension upon opening VS Code after you've forked and cloned Horizon.

You can also run it from a terminal with the following Shopify CLI command:

```bash
shopify theme check
```

You can follow the [theme check documentation](https://shopify.dev/docs/storefronts/themes/tools/theme-check) for more details.

#### Shopify/theme-check-action

Horizon runs [Theme Check](#Theme-Check) on every commit via [Shopify/theme-check-action](https://github.com/Shopify/theme-check-action).

## Contributing

We are not accepting contributions to Horizon at this time.

## License

Copyright (c) 2025-present Shopify Inc. See [LICENSE](/LICENSE.md) for further details.
