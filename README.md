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
