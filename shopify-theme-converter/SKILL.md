---
name: shopify-theme-converter
version: 3.0
description: Convert any web project (React, Next.js, Vue, HTML/CSS, any framework) into a COMPLETE, launch-ready Shopify store package. Goes beyond theme conversion to include page templates with pre-configured sections, navigation menus, asset manifests, content extraction, client handoff documentation, and a full operability audit ensuring the client can run the store forever from the Shopify dashboard without developer involvement.
updated: 2026-03-03
---

# Shopify Theme Converter v3

Convert any web project into a **complete, launch-ready, merchant-operable Shopify store package**.

## What v3 Adds Over v2

v2 stopped at "theme folder + setup checklist."
v3 closes every gap discovered in real client delivery:

| Gap | v3 Fix |
|-----|--------|
| Checkout expectations not set | Phase 0.1: Checkout Limitations briefing before build |
| No Liquid validation | Phase 8: `shopify theme check` before zip |
| No performance target | Phase 7: Lighthouse 80+ mobile gate |
| Metafields not in Customizer | Phase 2.7: Metafield settings in section schemas |
| Placeholder images look bad | Phase 2: Every image picker needs `placeholder_svg_tag` fallback |
| App → section mapping missing | Phase 6: `dependencies.md` maps each app to its section |
| No localization/multi-currency | Phase 2.8: `{% form 'localization' %}` + locale file completeness |
| Dashboard settings incomplete | Phase 5.5: Settings schema audit (favicon, share image, announcement bar) |
| App blocks not in schemas | Phase 2.9: `{ "type": "@app" }` in all key sections |

---

## The Operability Thesis

> "Can we build a custom theme and hand it to a client so they can operate it without ever touching code again?"

Every decision in this skill answers YES to that question. The test:

- **Content** → in section settings with good defaults
- **Behavior toggles** → exposed as schema checkboxes
- **Cross-sells** → controlled by product tags in admin
- **Email** → controlled by pasting a List ID
- **Tracking** → controlled by pasting pixel IDs in Theme Settings
- **UI copy** → wired to locale file, editable in Languages panel
- **App injection** → `@app` blocks registered so apps plug in without code

---

## Phase 0: Input Detection & Checkout Limitations

### Phase 0.1: ALWAYS DO THIS FIRST — Set Checkout Expectations

Before writing a single line of Liquid, communicate this to the client:

```
Shopify checkout is locked on Basic/Grow/Advanced plans.
We cannot touch the checkout page appearance.
Checkout Extensibility (custom checkout UI) requires Shopify Plus (~$2,000 CAD/month).

What we CAN control:
- Everything before checkout (PDP, cart, collections, homepage)
- Thank you / order status page (post-purchase)
- Cart drawer UX

What we CANNOT control without Plus:
- Checkout page layout or colours
- Custom fields at checkout
- Checkout upsells (requires Plus apps like ReConvert)
```

Set this expectation in writing before the build. Prevents the most common client callback.

### Phase 0.2: Detect Input Type

```
Is it a single HTML file with multiple page sections?
└─→ YES: Gemini HTML → load references/gemini-patterns.md
Is it a multi-file React/Vue/Next.js project?
└─→ YES: Standard conversion → Phase 1
Is it an existing Shopify theme needing upgrades?
└─→ YES: Run Phase 5.5 (Settings Audit) + Phase 8 (Theme Check) on existing code first
```

### Phase 0.3: Cart Strategy Decision

Decide before building. Do not leave both wired without a decision.

| Scenario | Recommendation |
|----------|---------------|
| Fashion/apparel, browse-heavy | Cart drawer — keeps user on page |
| High-consideration items (furniture, jewellery) | Cart page — less distraction |
| Mobile-first audience | Cart page — drawer UX is worse on touch |
| Multiple items per session | Cart drawer |

Document the decision. Update `main-product.liquid` ATC button to match.

---

## Phase 1: Analyse Source Project

Map all pages, components, navigation, assets. (See v2 for full detail.)

**v3 addition — Metafield Inventory:**

For any custom data patterns found (custom badges, specifications, sizing info, custom tabs), document as potential metafields:

```markdown
## Metafield Candidates
- Product fit notes → metafield: product.metafields.custom.fit_notes (type: multi_line_text)
- Care icons → metafield: product.metafields.custom.care_icons (type: list.single_line_text)
- Size model info → metafield: product.metafields.custom.size_model (type: single_line_text)
```

These become Customizer-accessible in Phase 2.7.

---

## Phase 2: Convert Components to Sections

Follow v2 conversion process with these mandatory additions:

### Phase 2.1: Every Image Picker Needs a Fallback

Every `image_picker` setting must have a `placeholder_svg_tag` fallback. Theme must look good on day 1 with zero images uploaded.

```liquid
{% if section.settings.image %}
  {{ section.settings.image | image_url: width: 1200 | image_tag: loading: 'lazy' }}
{% else %}
  {{ 'lifestyle-1' | placeholder_svg_tag: 'w-full h-full object-cover opacity-20' }}
{% endif %}
```

Available placeholders: `lifestyle-1` through `lifestyle-2`, `product-1` through `product-6`, `collection-1` through `collection-6`, `image`.

### Phase 2.2: Wire All UI Strings to Locale

Never hardcode user-visible strings. Always use `| t` filter.

```liquid
{%- comment -%} Wrong — client can't change this without code {%- endcomment -%}
Add to Cart

{%- comment -%} Right — client edits in Online Store → Languages {%- endcomment -%}
{{ 'products.product.add_to_cart' | t }}
```

Add any custom keys to `locales/en.default.json` and `locales/en.default.schema.json`.

### Phase 2.3: Product Card srcset Standard

Every product card image must use srcset. Never `width: 600` alone.

```liquid
<img
  src="{{ product.featured_image | image_url: width: 600 }}"
  srcset="{{ product.featured_image | image_url: width: 320 }} 320w,
          {{ product.featured_image | image_url: width: 600 }} 600w,
          {{ product.featured_image | image_url: width: 800 }} 800w"
  sizes="(min-width: 1024px) 25vw, 50vw"
  alt="{{ product.featured_image.alt | escape }}"
  width="{{ product.featured_image.width | default: 600 }}"
  height="{{ product.featured_image.height | default: 800 }}"
  loading="{{ img_loading | default: 'lazy' }}"
>
```

Pass `loading: 'eager'` for first 4 cards in any grid:
```liquid
{%- assign card_loading = 'lazy' -%}
{%- if forloop.index <= 4 -%}{%- assign card_loading = 'eager' -%}{%- endif -%}
{% render 'product-card', product: product, loading: card_loading %}
```

### Phase 2.4: Mobile Responsive Standards

Every section must follow:

| Element | Mobile | Desktop |
|---------|--------|---------|
| Section padding | `py-12 px-4` | `md:py-24 md:px-8` |
| Hero heading | `text-4xl` | `sm:text-6xl md:text-9xl` |
| Cards per row | `grid-cols-2` | `md:grid-cols-4` |
| Button padding | `px-6 py-3` | `sm:px-12 sm:py-4` |
| Form padding | `p-6` | `sm:p-12` |

Avoid `mix-blend-difference` — breaks on Android Chrome. Use explicit `text-white` with controlled overlay instead.

Wishlist / hover-only elements: always visible on mobile (`opacity-100 sm:opacity-0 sm:group-hover:opacity-100`).

Tables: wrap in `overflow-x-scroll -mx-4 px-4` on mobile.

### Phase 2.5: Tracking Events Standard

Every product page must fire:

```javascript
// On page load
fbq('track', 'ViewContent', { content_ids, content_name, value, currency })
ttq.track('ViewContent', { content_id, content_name, value, currency })
klaviyo.push(['track', 'Viewed Product', { ... }])
klaviyo.push(['trackViewedItem', { ... }])

// On ATC click
fbq('track', 'AddToCart', { content_ids, content_name, value, currency })
ttq.track('AddToCart', { content_id, value, currency })
klaviyo.push(['track', 'Added to Cart', { ... }])
```

All gated: `if (typeof fbq === 'function')` etc. Never assume SDK loaded.

### Phase 2.6: Back-in-Stock Standard

Every product page must render a BIS form when `current_variant.available == false`:

```liquid
{%- unless current_variant.available -%}
  {% render 'klaviyo-bis', variant: current_variant, product: product %}
{%- endunless -%}
```

See `snippets/klaviyo-bis.liquid` pattern. Only renders if `settings.klaviyo_public_api_key` set.

### Phase 2.7: Metafields → Customizer

For each metafield candidate identified in Phase 1, add to the relevant section schema:

```json
{
  "type": "text",
  "id": "metafield_fit_notes",
  "label": "Fit notes metafield key",
  "default": "custom.fit_notes",
  "info": "Shopify metafield namespace.key — set up in Settings → Custom Data"
}
```

Then render:
```liquid
{%- assign fit_notes = product.metafields[section.settings.metafield_fit_notes] -%}
{%- if fit_notes != blank -%}
  <p>{{ fit_notes.value }}</p>
{%- endif -%}
```

Include metafield setup instructions in `SETUP-CHECKLIST.md`.

### Phase 2.8: Localization Block

Add to header section if client may sell internationally:

```liquid
{% form 'localization' %}
  <select name="locale_code" onchange="this.form.submit()">
    {% for locale in shop.published_locales %}
      <option value="{{ locale.iso_code }}" {% if locale.iso_code == request.locale.iso_code %}selected{% endif %}>
        {{ locale.endonym_name }}
      </option>
    {% endfor %}
  </select>
  <select name="currency_code" onchange="this.form.submit()">
    {% for currency in shop.enabled_currencies %}
      <option value="{{ currency.iso_code }}" {% if currency == cart.currency %}selected{% endif %}>
        {{ currency.iso_code }} {{ currency.symbol }}
      </option>
    {% endfor %}
  </select>
{% endform %}
```

Gate behind a schema setting `show_localization` defaulting to `false`.

### Phase 2.9: App Blocks in All Key Sections

Every section that a merchant might want to extend must include:

```json
"blocks": [
  { "type": "@app" }
],
```

Required sections:
- `main-product.liquid` — reviews, upsell, loyalty
- `featured-collection.liquid` — promotional banners, app badges
- `main-collection.liquid` — collection app extensions
- `main-cart.liquid` — cart upsells, gift wrap apps
- `footer.liquid` — trust badges, certifications

---

## Phase 3: Page Templates

(See v2 for full detail.)

v3 addition: every template must have a `"name"` field visible in the Theme Customizer template dropdown so the client can assign it without guessing.

---

## Phase 4: Navigation Structure

(See v2 for full detail.)

---

## Phase 5: Asset Manifest

(See v2 for full detail.)

v3 addition: document placeholder fallbacks so the store looks good before images are uploaded. Client can go live immediately and replace images at their leisure.

---

## Phase 5.5: Settings Schema Audit (MANDATORY)

Before zipping, verify `config/settings_schema.json` contains:

```
□ favicon (image_picker) — used in meta-tags
□ share_image (image_picker) — OG fallback for pages with no product image  
□ announcement_bar_enabled (checkbox)
□ announcement_bar_text (text)
□ announcement_bar_link (url)
□ All pixel IDs: google_analytics_id, facebook_pixel_id, tiktok_pixel_id, klaviyo_public_api_key
□ Color schemes (at minimum: light, dark, accent)
□ Typography settings (font_heading, font_body)
□ Social links (instagram, facebook, tiktok, etc.)
□ cart_type (drawer vs page)
```

If any are missing, add them before handoff.

---

## Phase 6: Deployment Package

Final structure:
```
deployment-package/
├── theme/                    ← ZIP this for Shopify upload
├── SETUP-CHECKLIST.md        ← Step-by-step for client
├── pages-to-create.md
├── menus.json
├── assets-manifest.md
├── dependencies.md           ← With app → section mapping (see below)
└── section-inventory.md
```

### App → Section Mapping (required in dependencies.md)

```markdown
## App Integration Map

| App | Section it injects into | How to activate |
|-----|------------------------|-----------------|
| Judge.me / Loox (reviews) | Product page → Theme Customizer → Product → Add block → Apps | Install app, then add block |
| Klaviyo BIS | Automatic — shows on sold-out variants | Paste API key in Theme Settings → Analytics |
| Klaviyo newsletter | Any page → Theme Customizer → Add section → Klaviyo Newsletter | Paste List ID in section settings |
| Search & Discovery (filters) | Collection pages — automatic | Install free app from Shopify App Store |
| Tidio / Gorgias (live chat) | Injects automatically after install | No theme changes needed |
| ReConvert (post-purchase) | Order status page (Shopify Plus only) | N/A on Basic plan |
```

---

## Phase 7: Performance Gate

**Do not ship a zip without checking these.**

Target: **80+ Lighthouse mobile score**.

Checklist:
```
□ CSS under 80KB (Tailwind purged / compiled output only)
□ No render-blocking scripts in <head> (use defer/async)
□ Alpine.js self-hosted (not CDN)
□ All images use srcset
□ All images have width + height attrs
□ First 4 product images use loading="eager", rest lazy
□ Critical CSS inlined (above-fold styles in <head>)
□ No unused Google Fonts (use system fonts or self-hosted)
□ Hero has a static fallback image (YouTube autoplay blocked on iOS Safari)
```

Test: `npx @lhci/cli collect --url=https://your-preview-url` or Shopify's built-in speed score.

---

## Phase 8: Theme Check (MANDATORY before zip)

Run before every delivery:

```bash
# Install once
gem install shopify-cli
# or
brew install shopify-cli

# Run check
shopify theme check ./deployment-package/theme
```

**Must pass with 0 errors before handoff.** Warnings are acceptable; errors are not.

Common errors to fix:
- Missing translation keys (`MissingRequiredTemplateFile`)
- Deprecated Liquid filters (`DeprecatedFilter`)
- Missing `alt` attributes (`ImgLazyLoading`)
- Parser blocking scripts (`ParserBlockingScript`)

---

## Phase 9: Setup Checklist (Updated for v3)

In addition to v2 checklist, add:

```markdown
## 10. Enable Storefront Filtering
- Install Search & Discovery (free Shopify app)
- Open Search & Discovery → Filters → add: Availability, Price, Size, Colour
- Filters appear automatically on collection pages

## 11. Upload Favicon
- Theme Settings → Brand & SEO → Favicon
- Recommended: 32×32px PNG

## 12. Set OG Share Image  
- Theme Settings → Brand & SEO → Default share image
- Used when sharing pages on social without a product image
- Recommended: 1200×630px

## 13. Configure Announcement Bar
- Theme Settings → Announcement Bar → toggle on
- Set your message (e.g. "Free shipping over $150")

## 14. Wire Review App
- Install Judge.me or Loox
- Go to Theme Customizer → Product → click Add block → select Apps
- Reviews appear under product description automatically

## 15. Metafields (if applicable)
- Shopify Admin → Settings → Custom Data → Products
- Add definitions as documented in assets-manifest.md

## 16. Contact Form Emails
- Shopify Admin → Settings → Notifications → scroll to "Contact form"
- Confirm the recipient email is correct
```

---

## Quick Reference

| User Says | Action |
|-----------|--------|
| "Convert this to Shopify" | Phase 0 → Full workflow |
| "Client feedback to fix" | Identify which section, edit Liquid, rebuild zip |
| "Make it mobile responsive" | Phase 2.4 standards across all sections |
| "Wire tracking" | Phase 2.5 events pattern |
| "Audit existing theme" | Phase 5.5 + Phase 8 |
| "Ship to client" | Phase 7 gate + Phase 8 check + Phase 6 zip |

---

## Lesson Log (real delivery learnings)

**2026-03-03 — Fashion athleisure client (v3 first live delivery):**
- `mix-blend-difference` on hero text breaks on Android Chrome — remove it, use explicit text-white + controlled overlay
- YouTube iframe `autoplay` blocked on iOS Safari — always upload a static fallback image
- Logo clipping: `max-height` default of 60px is too tight for wordmark logos — default 80px, range to 200px
- `playlist` param required for YouTube loop: `?loop=1&playlist={{ video_url.id }}`
- Contact form messages → Shopify admin → Settings → Notifications → Contact form (clients always ask this)
- Announcement bar is the first thing every fashion client asks for — build it by default
- App blocks (`@app`) in schema are zero-effort to add but prevent "my review app isn't showing" callbacks
- Favicon and share_image are always missing from settings_schema unless you explicitly add them
- Cart drawer vs cart page: decide before build, document the decision — having both wired creates confusion
- `only_x_left` locale key needs `count` variable: `{{ 'products.product.only_x_left' | t: count: quantity }}`
