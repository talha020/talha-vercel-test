# ATLAS

Figma: file key `AhDWMTo3UrCBWlHjxG8qQM` (not read this run — see §1)  |  Webflow site id: not captured in this run's inputs (see §6)  |  Home page id `6a47971ad9af5f1a1ac42985`  |  Vertical: real-estate / vacation rentals  |  Built Jul–Sep 2026 by Shagufta Siraj for Xerosol  |  Analysed: 2026-09-22

> Scope note: this report was produced **without calling any Figma or Webflow MCP tool** (both hang in this session per the task's instructions). Everything below comes only from six files already staged on disk in `analysis/atlas/`: `home_tree.txt` / `home_tree_raw.json` (a `get_all_elements` dump of the Home page), `styles_raw.txt` / `class_names.txt` / `global_selectors.txt` / `combo_selectors.txt` (a `get_styles` name/selector dump — **not** a `query_styles`-with-properties dump, see §6), and `extra_from_main.md` (prose notes on pages, CMS schemas, components and variables gathered separately). No CSS property values, no component-internal trees, no interactions/custom-code dumps and no live-site fetch exist for this project in this run. Every class name, page name, field name and quoted string below is verbatim from those files; anything inferred is labelled as such.

---

## 1. Figma file anatomy

**Not collected: Figma file `AhDWMTo3UrCBWlHjxG8qQM` was not readable in this run.** No `get_metadata`, `get_variable_defs`, `get_design_context` or `get_screenshot` call was made against it — the task instructions for this report explicitly excluded Figma (and Webflow) MCP tool calls, so no page list, section census, Figma variable set, component list or auto-layout inspection is available. Everything Figma-shaped that appears later in this report (§3's mapping notes) is inferred purely from the Webflow-side evidence, not read from the design file.

---

## 2. Webflow site anatomy

### Pages (15 total)

| Title | Slug / published path | Notes |
|---|---|---|
| Home | `/` | homepage, id `6a47971ad9af5f1a1ac42985`; SEO title "Atlas Stays \| Luxury Vacation Rentals in Blue Ridge & DR", OG image set |
| Blue Ridge | `/blue-ridge` | static destination hub, SEO set; id `6a4b3c03d30e0dd535752466` (target of every "Explore/View Blue Ridge" CTA on Home) |
| Dominican Republic | `/dominican-republic` | static destination hub, SEO set; id `6a4b3c20b6e5359eecf18d9b` (target of every "Explore/View Caribbean/DR" CTA on Home) |
| About | `/about` | SEO set |
| Contact | `/contact` | SEO + OG asset set |
| Properties | `/properties` | **CMS template page** — SEO title bound to `{{name}}`, description bound to `description` + `bedrooms` + `bathrooms` fields, OG image bound to `thumbnail-image` |
| Properties Catagories (sic) | `/properties-catagories` | CMS template page, empty SEO |
| Domains | `/domain` (sic — collection is plural "Domains", page slug is singular "domain") | CMS template page, empty SEO |
| Sleeping Arrangements | template page | CMS template page, empty SEO |
| Highlights | template page | CMS template page, empty SEO |
| Amenities | template page | CMS template page, empty SEO |
| Amenity Categories | template page | CMS template page, empty SEO |
| Reviews | template page | CMS template page, empty SEO |
| Locations | template page | CMS template page, empty SEO |
| FAQs | template page | CMS template page, empty SEO |

Every one of the 10 CMS collections has its own template page — including collections (Sleeping Arrangements, Highlights, Amenities, Amenity Categories, Locations, FAQs) that read as sub-item/reference collections meant to be rendered *inside* a Properties detail page, not browsed as their own pages. Only `Properties` has real SEO bindings; the other nine template pages carry **empty SEO** (see §4).

### Class naming convention

**Same skeleton borrowed from Client-First as the sibling project (`furec`), but the section-naming discipline is much weaker.** Evidence, from the 509-style census (`styles_raw.txt`: 289 global classes, 196 combo classes, 24 tag styles):

- Client-First core primitives present verbatim: `page-wrapper`, `main-wrapper`, `container-large`, `padding-global`, `max-width-medium`.
- Client-First's `padding-section-small/medium/large` survive **unmodified** here (closer to real CF than furec, which had replaced them with `padding-vertical`) — plus a non-CF `padding-section-0` escape hatch.
- Section shells use one generic class over and over: `section_hero` (×1), `section_about` (×6 different sections), `section_standard` (×2), `section_cta` (×1), `section_footer` (×1) — 11 sections, only 5 distinct section-shell classes, vs. furec's ~60 one-per-topic `section_<name>` classes. The builder did not invent semantic names the way furec's did.
- Heading scale goes to **six** levels (`heading-style-h1` … `heading-style-h6`), one more than furec's five, while the body-text scale is inconsistent: mostly semantic tokens (`text-size-tiny`, `text-size-xtiny`, `text-size-small`, `text-size-regular`, `text-size-xlarge`) but with two standalone **literal-px classes bolted on as siblings, not combos**: `.text-size-12px`, `.text-size-18px`.
- Heavy modifier-combo stacking, same idiom as furec: `is-alt` / `is-alternate` for style variants, `mob-hide` / `mob-size-12px` / `mob-size-14px` / `tab-show` / `tab-hide` / `desktop-show` / `desktop-hide` for responsive duplication, numeric-px combos (`padding-48-0`, `padding-48-96`, `gap-36px`, `height-624px`, `width-432`) for per-instance sizing.
- **Not Relume** (no `layout###_component`, no `rl-` prefixes).

**30+ verbatim classes, grouped**

*Layout / structure*
`page-wrapper`, `main-wrapper`, `container-large`, `padding-global`, `padding-section-0`, `padding-section-small`, `padding-section-medium`, `padding-section-large`, `content-wrapper`, `content-wrapper-left`, `content-wrapper-right`, `max-width-medium`, `flex-horizon`, `flex-horizontal`, `flex-vertical`, `space-dummy`, `widget-wrapper`

*Section shells (only 5 distinct, reused across 11 sections)*
`section_hero`, `section_about`, `section_standard`, `section_cta`, `section_footer`, `section_ridge`, `section_terms`, `section_detail-link`

*Typography*
`heading-style-h1`, `heading-style-h2`, `heading-style-h3`, `heading-style-h4`, `heading-style-h5`, `heading-style-h6`, `text-size-tiny`, `text-size-xtiny`, `text-size-12px`, `text-size-small`, `text-size-regular`, `text-size-18px`, `text-size-xlarge`, `subtitle`, `subtitle-wrapper`, `subtitle-size-20px`, `text-suisse-font`, `text-font-suisse-400`, `about-richtext`, `detail-richtext`, `events-gathering-richtext`

*Buttons*
`primary-button`, `secondry-button` (sic), `detail-link`, `underline-link-wrapper`, `email-link`, `secondry-button.show-more`, `secondry-button.show-less`

*Cards / repeated components (all plain classed `Block`s, never Webflow Components — see below)*
`here-card`, `here-card-content`, `here-card-img`, `guest-card`, `guest-card-content`, `guest-img`, `about-card`, `about-card-icon`, `card-top-content`, `review-card-wrapper`, `reviewa-card` (sic), `special-card`, `contact-card`, `cms-item`, `cms-item-property`, `cms-small-card`

*Navbar*
`navbar`, `nav-container`, `nav-inner-dropdown`, `nav-menu`, `nav-menu-wrapper`, `nav-menu-line`, `nav-link`, `nav-link-navigation`, `nav-links-wrapper`, `nav-dropdown`, `nav-drp-toggle`, `nav-drop-down-icon`, `hamburger`, `hamburger-bar`, `menu-btn`, `brand-logo`, `brand-img`

*CMS / detail-page scaffolding*
`amenities-collection`, `amenities-collection-list`, `amenities-collection-item`, `amenity-collection`, `amenity-collection-list` (note the singular/plural split), `highlights-list`, `highlights-list-wrapper`, `highlights-item`, `faq-collection`, `faq-collection-list`, `faq-collection-item`, `review-collection`, `review-collection-list`, `review-collection-item`, `reviews-cards-wrapper`, `reviewes-cards-wrapper` (sic), `reviewes-cards-cms` (sic), `sleeping-cms-item`, `sleeping-cms-list`, `sleeping-cms-slider`, `sleeping-slideer-mask` (sic), `slieeping-slide` (sic), `detail-hero-wrapper`, `detail-slider`, `detail-slide`, `detail-pill-wrapper`, `calendar-embed`, `ownerrez-calendar`, `calanders-wrapper`, `calander-check` (sic), `calander-imge` (sic)

*Utility / sizing*
`gap-4px`, `gap-8px`, `gap-12px`, `gap-24px`, `gap-32px`, `gap-36px`, `gap-40px`, `gap-48/32`, `gap-48-32`, `vertical-gap-16`, `vertival-gap-32px` (sic), `gap24px` (sic, no separator), `width-100%`, `width-267`, `width-368px`, `width-432px`, `width-440px`, `height-38px`, `height-48px`, `height-50px`, `height-51px`, `height-59px`, `height-480`, `height-624px`, `height-688`, `size-48px`, `mob-hide`, `mob-size-12px`, `mob-size-14px`, `tab-show`, `tab-hide`, `desktop-show`, `desktop-hide`, `hide`, `relative`, `bg-img`

### DOM skeleton of the homepage — body → hero → one card section → footer

Verbatim from `home_tree.txt` (11 `Section`s total under `.main-wrapper`: `section_hero` ×1, `section_about` ×6, `section_standard` ×2, `section_cta` ×1, `section_footer` ×1):

```
Body
  Block.page-wrapper                                          <div>
    ComponentInstance["Global Style"]
    Block.main-wrapper                                        <div>
      ComponentInstance["Navigation"]
      Section.section_hero                                    <section>
        Block.padding-global
          Block.container-large
            Block.padding-section-medium.padding-section-0
              Block.padding-section-0.is-hero
                Block.hero-content-wrapper
                  Block.hero-text-content                      (Blue Ridge column)
                    Block.hero-text-wrapper
                      Block.about-text-content
                        Paragraph.text-size-tiny.color-white    | "Blue Ridge, Georgia"
                        Heading<h1>.heading-style-h1             | "Mountain Retreats"
                      Block.subtitle-wrapper
                        Paragraph.text-size-regular.color-white.align-left
                    Link.primary-button.is-alt                  | "Explore Properties"  → /blue-ridge
                  Block.hero-text-content                       (Dominican Republic column)
                    Block.hero-text-wrapper
                      Block.about-text-content
                        Paragraph.text-size-tiny.color-white    | "Dominican Republic"
                        Heading<h1>.heading-style-h1             | "Caribbean Escapes"
                      Block.subtitle-wrapper
                        Paragraph.text-size-regular.color-white.align-left
                    Link.primary-button.is-alt                  | "Explore Properties"  → /dominican-republic
        Block.hero-bg-wrapper
          Block.hero-img-left
            Image.hero-img
            Block.hero-gradient
          Block.hero-img-right
            Image.hero-img.is-second-hero
            Block.hero-gradient
      Section.section_about … (×6)                              <section>
      Section.section_standard … (×2)                            <section>
      Section.section_cta                                        <section>
      Section.section_footer                                     <section>   ← hand-built, see below
      ComponentInstance["Footer"]                                 ← a SEPARATE component instance, sibling
```

**Card-grid example** (`section_about`, "Stays in Blue Ridge, GA" — one of the 6 static, non-CMS-bound `section_about` instances):
```
Section.section_about                                          <section>
  Block.padding-global
    Block.container-large
      Block.padding-section-medium.padding-section-0
        Block.content-wrapper.vertival-gap-32px                 (typo: vertival)
          Block.max-width-medium.spacebtween
            Block.about-text-content
              Paragraph.text-size-tiny.mob-size-12px             | "Guest Favorites"
              Heading<h2>.heading-style-h2                        | "Stays in Blue Ridge, GA"
            Link.primary-button.is-alternate.mob-hide             | "View All"   (linkType: none — see §4)
          Block.guest-cards-wrapper
            Block.guest-card
              Block.small-card-wrapper
                Image.guest-img
                Block.small-card-overlay
              Block.guest-card-content
                Paragraph.text-size-tiny.mob-size-12px            | "Blue Ridge, Georgia · Lakefront"
                Heading<h3>.heading-style-h3                       | "Rocky Point Retreat"
                Paragraph.text-size-regular.mob-size-14px         | "18 guests · 6 bedrooms · 5.5 bathrooms"
            Block.guest-card  (×2 more, same interior)
          Block.mob-btn-wrapper
            Link.primary-button                                   | "View All Properties"  (linkType: none — see §4)
```
This is static, hand-authored content mimicking a CMS card grid — it is **not** a Webflow Collection List element (see §2's CMS discussion: the real Properties collection only renders on the `/properties` template page, whose DOM was not captured this run).

**Footer** — the raw tree shows something unusual: `.section_footer` is a fully hand-built `<section>` (not a component), and a **separate, empty `ComponentInstance["Footer"]`** immediately follows it as a sibling under `.main-wrapper` (confirmed by walking `home_tree_raw.json`: `main-wrapper`'s children are `Navigation → 9×Section → Section.section_footer → Footer`). See §4 for why this looks like a mistake.
```
Section.section_footer                                          <section>
  Block.padding-global
    Block.container-large
      Block.padding-section-small
        Block.content-wrapper.gap-36px
          Block.footer-top-content
            Block.footer-logo-wrapper
              Link.footer-logo                                    → Home (self-link)
              Block.subtitle-wrapper.width-432px
                Paragraph.text-size-regular                        | "Curated short-term rentals across…"
            Block.footer-links-wrapper
              Block.footer-link-wrapper                            ("Blue Ridge, GA" — 7 property-name links)
                Paragraph.text-size-regular.weight-600-bold        | "Blue Ridge, GA"
                Block.subtitle-wrapper.vertical-gap-16
                  Link.text-size-regular.footer-link ×7             (all linkType: none — see §4)
              Block.footer-link-wrapper                            ("Dominican Republic" — 9 property-name links)
                … Link.text-size-regular.footer-link ×9             (all linkType: none)
              Block.footer-link-wrapper                            ("Support")
                … Link.text-size-regular.footer-link ×5             (mixed tab-show/tab-hide duplicate pairs, all linkType: none)
          Block.footer-bottom-content
            Block.subtitle-wrapper.order-second
              Paragraph.text-size-regular                          | "© 2026 Atlas Stays. All rights reserved."
            Block.subtitle-wrapper
              Link.text-size-regular.footer-link                    | "Terms & Conditions"  (linkType: none)
ComponentInstance["Footer"]                                        ← sibling of the section above, no props, contents not captured
```

### Navbar

No element tree was captured for the `Navigation` component this run (it appears in `home_tree.txt` only as an unexpanded `ComponentInstance["Navigation"]`, no props). Structure below is **inferred solely from class names** in `global_selectors.txt` / `combo_selectors.txt` — treat as inference, not a verified DOM:
```
Block.navbar
  Block.nav-container
    Block.brand-logo > Image.brand-img
    Block.nav-menu-wrapper
      Block.nav-menu
        Link.nav-link  ×N
        Block.nav-dropdown                    (.nav-dropdown.desktop-hide / .nav-dropdown.tab-hide — two breakpoint variants)
          Block.nav-drp-toggle
            Block.nav-menu-line               (default Webflow name kept as class, "Nav Menu Line")
            Block.nav-drop-down-icon
          Block.nav-inner-dropdown
            Link.nav-link.is-dropdown         (.nav-link.is-dropdown.hide combo also exists)
    Block.menu-btn
      Block.hamburger
        Block.hamburger-bar.top / .center / .bottom
```
No dropdown-list item classes matching the destination pages (Blue Ridge / Dominican Republic) were found beyond `nav-link-navigation`, so the exact dropdown content is not recoverable from class names alone.

### Buttons

Unlike `furec`, **buttons here are not Webflow Components** — they are plain `Link` elements carrying one of two classes, `primary-button` or `secondry-button` (sic, throughout — never spelled "secondary" anywhere in the 509-style census). No `query_styles`-with-properties data exists for this site (§6), so no CSS values can be quoted; only the modifier vocabulary is visible:
- `.primary-button.is-alt` — used on the two hero CTAs and the CTA-section buttons (dark hero background context).
- `.primary-button.is-alternate` / `.primary-button.is-alternate.width-100%` / `.primary-button.is-alternate.mob-hide` — used on the About and guest-card "View All" buttons (light background context).
- `.primary-button.is-form-btn` / `.is-form-btn.height-48px` — form submit variant.
- `.primary-button.width-100` / `.width-100.height-48px` — full-width mobile variant (note: `width-100` here has **no** `%`, unlike the `width-100%` combo used elsewhere on the same button class — an inconsistent unit suffix, see §4).
- `.secondry-button` — with `.hide`, `.show`, `.show-more`, `.show-less`, `.show-more.hide`, `.show-less.hide`, `.width-100`, `.width-100.height-48px` — this is a show‑more/show‑less toggle button (likely on the FAQ/description read-more), not a stylistic second CTA.
- Every homepage `Link` with `linkType: null` (25 of them — see §4) is one of these button classes or a `footer-link`.

### Variables (from `extra_from_main.md`)

One collection, "Base collection", single mode. 23 variables total:

| Name | Value |
|---|---|
| Light Beige | `#ddd3c2` |
| Warm Ivory | `#f4f0e8` |
| Sand | `#c2b49a` |
| Ivory | `#f5f0e8` |
| Sand Beige | `#ede6d8` |
| Charcoal Brown | `#1e1b17` |
| Charcoal | `#3a3530` |
| Vellum | `#faf8f4` |
| Off White | `#fdfcfb` |
| Gray | `hsla(0,4.64%,74.6%,1)` |
| White / Black / Transparent | — |
| Ochre | `#a8853a` |
| **"#EEEEEE"** | `#eee` — a **literal hex string used as the variable's own name** |
| Terra | `#c2956a` |

Font-family variables: `Poppine` (typo of "Poppins"), `Canela Trial`, `Caneladeck Trial`, `Canelatext Trial`, `Suisse Intl Trial`, `Suisse Intl Trial 1`, **`Suisse Intl Trial 2`** — whose *value* is not a font name at all but a garbage hash string (`"B 8 E 87569 C…"`, per `extra_from_main.md`). No size, spacing or radius variables exist — same pattern as `furec`: all geometry is hard-coded per class, not tokenised.

The colour variable names (`Charcoal Brown`, `Terra`, `Ochre`, `#EEEEEE`) leak directly into combo-class display names verbatim, capitals and all — see §4.

### Typography classes

**No CSS property values are available for this site** (see §6 — `styles_raw.txt` is a name/selector list only, with zero `properties`). The only numeric evidence is what's encoded literally in class names themselves, which is not the same as a verified computed style:

| class | encoded/likely size hint | notes |
|---|---|---|
| `heading-style-h1` … `heading-style-h6` | none encoded — base scale | 6 levels (furec had 5); combo `heading-style-h2.size-48px` implies an instance-level 48px override exists |
| `heading-style-h2.text-align-center.is-alt` | — | 3-deep combo stack on the CTA heading |
| `heading-style-h3.is-alt`, `heading-style-h4.align-center` | — | |
| `text-size-tiny` / `text-size-xtiny` / `text-size-small` / `text-size-regular` / `text-size-xlarge` | semantic tokens, no literal px in the name | the "normal" 5-step scale |
| `text-size-12px`, `text-size-18px` | 12px / 18px | **standalone global classes**, not combos on top of the semantic scale — the size system is mixed (see §4) |
| `.text-size-regular.mob-size-14px`, `.text-size-tiny.mob-size-12px` | 14px / 12px at mobile | the site's per-breakpoint px-override idiom, same pattern as furec's `_14px-mbl` |
| `subtitle-size-20px`, `title-size-18px 2` (sic, trailing " 2" — a duplicate, see §4) | 20px / 18px | |
| `.text-size-regular.weight-600-bold`, `.text-size-regular.bold-600` | — | two differently-named classes for what looks like the same "bold 600" concept |
| Colour combos | `color-white`, `color-charcoal`, `color-charcoal-brown` (and the mismatched-display-name `color-Charcoal Brown`), `color-terra`/`color-Terra`, `color-ochre`/`color-Ochre`, `color-#615F5F`, `color-#EEEEEE` | pulled straight from the Base-collection variable names, inconsistently cased (see §4) |

Tag styles (24 total, from `styles_raw.txt`): bare `body`, `p`, `h2`, `h3`, `h4`, `li`, `ol`, `ul`, `strong` plus nested tag styles scoped to three RichText wrapper classes (`about-richtext h2/h3/p`, `detail-richtext h2/h3/h4/li/ol/p/strong/ul`, `events-gathering-richtext h2/h3/p/li/ul`). **No bare `h1` tag style exists** — every `<h1>` on the site must be manually classed, which is significant given the homepage puts a `.heading-style-h2` class on an `<h1>` tag (§4).

### Spacing

Again, no computed values — only the class-name vocabulary:
- `padding-global` — global page gutter (base primitive, no per-instance combos observed).
- `container-large` — content max-width wrapper, with a `.container-full-width` combo escape hatch.
- `padding-section-small` / `padding-section-medium` / `padding-section-large` / `padding-section-0` — the vertical-rhythm scale, closer to unmodified Client-First naming than `furec`'s `padding-vertical`.
- Per-instance overrides on `padding-section-medium`/`-small`: `.padding-48-0`, `.padding-48-96`, `.padding-48/24` (class_names.txt), `.padding-48/0` (class_names.txt), `.padding-top-0`, `.padding-top-0.top-6em` — **the separator between the two numbers is inconsistent**: some combos use a dash (`padding-48-0`, `padding-48-96`) and others use a literal forward slash (`padding-48/24`, `padding-48/0`) for what is presumably the same "top/bottom" pattern.
- Gap utilities: `gap-4px`, `gap-8px`, `gap-12px`, `gap-24px`, `gap-32px`, `gap-36px`, `gap-40px`, `vertical-gap-16`, `vertival-gap-32px` (sic), plus **two separately-registered classes for the same 48/32 pairing**: `gap-48/32` (slash, in `class_names.txt`) and `.guest-cards-wrapper.gap-48-32` (dash, in `combo_selectors.txt`) and a third, separator-less `gap24px`.
- `padding-global.padding-global-0` — a zeroed-out override combo.

### Responsive approach

Breakpoint vocabulary: `mob-hide`, `mob-size-12px`, `mob-size-14px`, `tab-show`, `tab-hide`, `desktop-show`, `desktop-hide`, `show-desktop`, `show-on-tab`. Element duplication (not CSS reflow) is the dominant technique, same as `furec`'s `.desktop_active`/`.mobile_active` pattern:
- The footer "Support" column duplicates two links per breakpoint: `"About Us"` (`.tab-hide`) vs. `"About Atlas"` (`.tab-show`); `"Policies"` (`.tab-hide`) vs. `"Rules & Policies"` (`.tab-show`).
- The "Art of an Exceptional Stay" 3-card group ("Designer Interiors" / "Exceptional Locations" / "Unique Experiences") is built **three separate times** for what appear to be overlapping breakpoints:
  1. `.standard-cards-wrapper.desktop-show` — plain flex `here-card` blocks.
  2. `.slider-design.tab-show` — a native Webflow `SliderWrapper`/`SliderMask`/`SliderSlide`/`SliderNav` carousel (class `atlas-slider`).
  3. `.swiper.is-tab-show` — a **separate Swiper.js** implementation (`swiper-wrapper`, `swiper-slide`, `swiper-pagination`, `swiper-button-prev/next`) with identical card content.
  
  Both (2) and (3) claim a "tab" breakpoint with two different modifier spellings (`tab-show` vs. `is-tab-show`) — see §4.

### Components (6, from `extra_from_main.md`)

| name | instances | props |
|---|---|---|
| `Global Style` | 7 | none |
| `Navigation` | 7 | none |
| `Footer` | 7 | none |
| `Footer_Secondary` | **0** | — unused, see §4 |
| `FAQ` | 3 | `Title` (textContent), `Image 1`…`Image 7` (image), `Text` (textContent) |
| `Ridge_Republic` | 2 | `Image`, `Text 1`, `Title`, `Text 2` — a paired location-intro block |

No Button, Card, or Nav-link component exists — every repeated card (`here-card`, `guest-card`, `about-card`, `review-card-wrapper`/`reviewa-card`, `special-card`, `contact-card`) is a plain classed `Block`, matching the pattern `furec` also used (cards = classes, not components). Buttons are also plain classed `Link`s (§2's Buttons subsection), which is a *less* componentized approach than `furec` (where `Button Primary`/`Button Secondary` were real components).

### CMS design in depth

10 collections (from `extra_from_main.md`): **Properties, Properties Catagories** (sic), **Domains, Sleeping Arrangements, Highlights, Amenities, Amenity Categories, Reviews, Locations, FAQs**.

**Properties** — 47 fields, no field groups:
- `PlainText` (single-line): `short-description`, `designer-credit`, `rating`, `description` (display-named **"People"** — the field's own label doesn't match its slug or apparent content), `bedrooms`, `bathrooms`, `area`, `location`, `pill-subtitle`, `price-night`, `map-location`, `location-description`, `ownerrez-property-id`.
- `MultiImage`: `gallery`, `gallery-2`, `gallery-3`. `Image`: `thumbnail-image`.
- `Reference`: `blue-ridge-catagory` → **Properties Catagories** (required); `catagory` → **Domains**. Both fields carry the same "catagory" typo but point to two *different* collections — a naming collision that would confuse a content editor (§4).
- `MultiReference`: `highlights` → Highlights, `sleeping-arrangements-3` → Sleeping Arrangements (the trailing "-3" implies at least two earlier field attempts were made and discarded), `reviews` → Reviews, `location-distance` (display-named "Nearby Locations") → Locations, `faqs` → FAQs.
- `RichText` (17 fields covering house-info sections): `about-richtext`, `events-gatherings-day-guests`, `check-in-check-out`, `booking`, `occupancy-guests`, `staff-service`, `quiet-hours-security`, `house-rules`, `parking-access`, `pool-spa-lake`, `pool-spa-river`, `dogs`, `hot-tub`, `hot-tub-dock-lake`, `hot-tub-creek-pond`, `beach-pool-ocean`, `nature-disclosure` (display-named "Good To Know"), `what-to-bring`.
- `Link`: `google-map-location`. `Number`: `order` (integer, manual sort).

**Amenities → Properties join direction**: the **Amenities** collection schema carries `properties` (`MultiReference` → Properties) — i.e. the relationship is stored on the **Amenities side**, each amenity item lists which properties have it. This is the *opposite* direction from every other Properties relation (`highlights`, `sleeping-arrangements-3`, `reviews`, `location-distance`, `faqs`), which are all stored forward, directly on the Properties item. To show "this property's amenities" on a detail page, the build has to run a reverse/filtered lookup rather than reading a field straight off the Properties item — a structurally asymmetric, and easy-to-miss, data model choice. Amenities' other field is `amenity-categories-2` (`Reference` → Amenity Categories), plus `amenities-icon` (Image), `name`, `slug`.

**Reviews** schema: `date-2` (DateTime), **`review-text` is `PlainText` single-line** (flagged explicitly in `extra_from_main.md` with an exclamation mark — a review field that cannot hold more than one line/no line breaks), `reviewer-progile-image` (sic, Image), `name`, `slug`. This pairs with a concrete symptom visible on the homepage: the "What Guests Say" marquee (`home_tree.txt` lines ~322–744) renders **~30 `marquee-item` cards from only 2 distinct review snippets and 1 reviewer name**, repeated verbatim throughout ("Sarah & James Whitemore" / "Ridgeline Retreat · Blue Ridge, GA" appears on every single card regardless of which of the two quotes it pairs with) — plausibly placeholder content never swapped for real per-review data, consistent with a review data model that wasn't built out for variety.

**Class-name evidence for the (uncaptured) Properties detail-page template**, cross-referenced against the schema above — inferred only, no DOM was captured for `/properties`:
- `detail-hero-wrapper`, `detail-richtext`, `detail-pill-wrapper`, `detail-pill-icon`, `detail-links-wrapper`, `detail-link` — the property overview/pills block (bedrooms/bathrooms/area pills, matching the PlainText fields).
- `detail-slider*` / `detail-slide*` / `detail-slider-nav` / `detail-slider-arrow` (with an `.is-detail` combo family and a separate, near-duplicate `.detail-slider2-content`) — the photo gallery, matching the three `MultiImage` fields (`gallery`, `gallery-2`, `gallery-3`).
- `sleeping-cms-item`, `sleeping-cms-list`, `sleeping-cms-slider`, `sleeping-slideer-mask` (sic), `slieeping-slide` (sic) — the `sleeping-arrangements-3` MultiReference rendered as a nested CMS slider.
- `amenities-collection` / `amenities-collection-list` / `amenities-collection-item` **and** the separately-named `amenity-collection` / `amenity-collection-list` (singular vs. plural split for what looks like the same amenities-grid concept, §4) — the reverse-referenced Amenities list.
- `highlights-list`, `highlights-list-wrapper`, `highlights-item` — the `highlights` field.
- `faq-collection`, `faq-collection-list`, `faq-collection-item` — the `faqs` field (distinct from the sitewide `FAQ` *component* used on the homepage).
- `review-collection`, `review-collection-list`, `review-collection-item`, plus three near-duplicate spellings for the same reviews-grid idea: `reviews-cards-wrapper` (correct), `reviewes-cards-wrapper` (sic), `reviewes-cards-cms` (sic) — the `reviews` field.
- `location-wrapper`, `location-content-wrapper`, `location-embed`, `map-wrapper`, `map-img` — the `location-distance` ("Nearby Locations") field plus `map-location`/`google-map-location`.
- `calendar-embed`, `ownerrez-calendar`, `calanders-wrapper`, `calanders-checks-wrapper`, `calander-check` (sic, `.is-available`/`.is-not` combos), `calander-imge` (sic) — an embedded availability calendar, almost certainly the **OwnerRez** booking-software widget (the `ownerrez-property-id` PlainText field on Properties is the direct integration key for this).

**Domains** and **Properties Catagories** are inferred (not directly evidenced) to be the site's region/brand taxonomy: `Properties Catagories` values plausibly gate the homepage's Blue-Ridge-vs-Dominican-Republic split (`blue-ridge-catagory` is the *required* Reference on every Property), while `Domains` (the *optional* `catagory` Reference) may be a white-label/booking-domain tag — this is inference only, no collection-item content was captured.

### Interactions / animations

**Not collected.** No `data_interactions_tool` dump exists for this project in this run's staged files — `list_interactions` was never called, so whether this site uses Webflow IX3 (scroll-scrubbed timelines, hover interactions, etc.) is unknown. Class names hint at interactive behaviour that IX3 or custom JS would drive (`overlay-reveal-in-hower` [sic, "hover"], `faq-animated-div`, `accordion-toggle`, `accordion-icon`) but none of this is confirmed.

### Custom code

**Not collected.** No `data_scripts_tool` dump (site head/footer freeform code, page-level code, registered scripts) exists for this project in this run's staged files.

---

## 3. Figma → Webflow mapping notes (inferable only — no Figma data was read)

Since §1 has no Figma evidence at all, nothing here is a confirmed mapping — it is what the Webflow-side evidence alone makes *plausible*:

- The shared primitives (`page-wrapper`, `main-wrapper`, `container-large`, `padding-global`) are identical to the ones seen in the `furec` report from the same analysis batch, suggesting the same builder/team methodology (a Client-First-derived starter, customised per project) rather than anything provable about the Figma file itself.
- The extremely generic section-shell naming (`section_about` reused across 6 unrelated content sections: the About-Atlas intro, two "Guest Favorites" destination grids, the "Why Guests Return" feature-card block, the reviews marquee, and the FAQ block) suggests either (a) the Figma frames themselves were named generically (the pattern `furec.md` documented — Figma sections just called `"section"`), or (b) this builder, unlike `furec`'s, chose not to invent semantic classes per section. Both are equally consistent with the evidence; it cannot be decided without reading the Figma file.
- The dual hero (Blue Ridge column + Dominican Republic column, identical structure, different copy) and the two parallel "Guest Favorites" card grids strongly suggest a Figma design that was built as two literal side-by-side content tracks rather than one CMS-filterable "Location" swiper — again only inferable from the duplicated Webflow markup, not confirmed against any Figma frame.
- The component set actually promoted to Webflow (`FAQ`, `Ridge_Republic`) — reusable *content blocks* with rich prop sets (`Ridge_Republic` has `Image`, `Text 1`, `Title`, `Text 2` — a paired intro-block shape) but **not** buttons or cards — matches the `furec` pattern of promoting only specific named units to components while leaving repeated cards as plain classed divs, again suggesting shared build conventions across this team's projects rather than anything specific to Atlas's Figma file.
- Nothing about breakpoints, spacing tokens, colour tokens, or font choices can be cross-checked against Figma variables here (unlike `furec.md`'s §3, which had both sides); the Webflow "Base collection" variables (§2) are the *only* design-token evidence available, and there is no way to tell whether they were copied faithfully from a Figma variable set or invented independently during the build.

---

## 4. Quality notes and gotchas

### Rules that look deliberate

1. **Five-level section shell**, applied with total consistency across all 11 homepage sections: `Section.section_<x>` → `padding-global` → `container-large` → `padding-section-<size>` → a topic wrapper (`content-wrapper`, `hero-content-wrapper`, `standard-content-wrapper`, `cta-content`).
2. **Cards are always plain classed `Block`s, never components** — `here-card`, `guest-card`, `about-card`, `review-card-wrapper`, `special-card`, `contact-card` are all consistent with this rule.
3. **Breakpoint duplication over reflow**: paired `tab-show`/`tab-hide`, `desktop-show`/`desktop-hide`, `mob-hide`/`mob-size-Npx` classes used consistently to show/hide or resize whole duplicated elements, the same idiom `furec` used with `.desktop_active`/`.mobile_active`.
4. **Per-instance px overrides live in a combo, not a new base class**, for most of the system: `mob-size-12px`, `mob-size-14px`, `padding-48-0`, `gap-36px`, etc.
5. **Component props are consistently `Title`/`Text`/`Image` textContent/image pairs** (`FAQ`: `Title`, `Image 1..7`, `Text`; `Ridge_Republic`: `Image`, `Text 1`, `Title`, `Text 2`).
6. **CMS collections and template pages are provisioned 1:1**, even for what look like sub-item collections — a uniform (if perhaps over-broad) policy applied without exception across all 10 collections.

### Mistakes / inconsistencies

- **Three `<h1>` tags on one page.** `home_tree_raw.json` shows `<h1>` used for "Mountain Retreats", "Caribbean Escapes" (both carrying `.heading-style-h1`, at least internally consistent with each other) **and** for the FAQ section's "Need to Know" heading — which carries the **`.heading-style-h2`** class on an `<h1>` tag. That third one is a genuine tag/class mismatch, not just a multi-H1 design choice.
- **Duplicate footer.** `main-wrapper`'s children end with a fully hand-built `Section.section_footer` (all real content: logo, 21 property links, support links, copyright) immediately followed by a second, separate `ComponentInstance["Footer"]` as a sibling — the site appears to carry both a static footer and an unused Footer component instance stacked on the same page.
- **`Footer_Secondary` component has 0 instances anywhere** on the site (per `extra_from_main.md`) — a dead component.
- **25 `Link` elements on the homepage have `linkType: null`** (no href, no page target at all) — every one of the 16 Blue-Ridge/Dominican-Republic property-name footer links, all 5 "Support" footer links, "Terms & Conditions", and both mobile-only "View All Properties" buttons. These render as dead/inert links.
- **`review-text` is a single-line PlainText field**, and the homepage's live "What Guests Say" marquee is built from only 2 unique review quotes and 1 reviewer name copy-pasted across roughly 30 `marquee-item` cards — a strong sign the reviews CMS content was never populated with real, varied data before/at the point this was captured.
- **Two competing "tab" carousel implementations of the exact same content.** The "Art of an Exceptional Stay" 3-card set exists three times: `.standard-cards-wrapper.desktop-show` (plain flex), `.slider-design.tab-show` (native Webflow Slider), and `.swiper.is-tab-show` (Swiper.js) — two different slider libraries, with two different modifier spellings (`tab-show` vs `is-tab-show`) for what is presumably meant to be the same breakpoint. One of these is almost certainly dead/leftover.
- **Amenities↔Properties relationship is asymmetric.** Every other Properties MultiReference (`highlights`, `sleeping-arrangements-3`, `reviews`, `location-distance`, `faqs`) is stored forward on Properties; `amenities` is not even a field on Properties — the join only exists as a `properties` MultiReference on the **Amenities** collection, the reverse direction from the rest of the schema.
- **Two independently-typo'd "catagory" fields** on Properties (`blue-ridge-catagory` → Properties Catagories, `catagory` → Domains) both misspell "category" and both point to *different* collections — a naming collision that invites editor confusion.
- **Field display-name mismatches**: `description` is display-named **"People"**; `location-distance` is display-named "Nearby Locations"; `nature-disclosure` is display-named "Good To Know" — slugs and labels have drifted apart.
- **Collection/page slug mismatch**: the `Domains` collection (plural) publishes its template page at the singular `/domain`.
- **Garbage font-family variable**: `Suisse Intl Trial 2`'s stored value is not a font name — it is a nonsense hash string (`"B 8 E 87569 C…"`).
- **Hex-literal variable/class names**: the Base-collection variable is literally named `"#EEEEEE"` rather than something semantic, and this leaks into the combo class display name `color-#EEEEEE` (real selector `.footer-logo.color--eeeeee`); likewise `color-#615F5F` (real selector `.text-size-18px.color--615f5f`) has no variable at all behind it — a raw hex value typed directly into a class name.
- **Combo display names copy-pasted from Figma-style variable names, not slugified to match their real selectors**: `color-Charcoal Brown` (real selector `.color-charcoal-brown`), `color-Terra` (→ `.color-terra`), `color-Ochre` (→ `.color-ochre`) all keep spaces/capitals in the Webflow *display* name while the actual CSS selector is auto-kebabbed — the two representations are out of sync.
- **Copy-pasted, never-renamed duplicate combos**: `.faq-question._80` and `.faq-question._80-copy`, displayed in the Webflow panel simply as `"80%"` and `"80% Copy"` — a class was duplicated and the copy was never given a real name.
- **Raw Webflow auto-rename artifacts kept in production**: `"faq-question-wrapper 2"` (→ `.faq-question-wrapper-2`) and `"title-size-18px 2"` (→ `.title-size-18px-2`) are both trailing-" 2" duplicates left in place.
- **Default Webflow element-type names kept as real classes**: `"Paragraph"` (→ `.paragraph`), `"Icon"` (→ `.icon`), `"Collection Item"` (→ `.collection-item`), `"Nav Menu Line"` (→ `.nav-menu-line`), `"Code Embed"` (→ `.code-embed`), `"Calendar-embed"` (→ `.calendar-embed`) — none of these were ever renamed away from Webflow's auto-generated defaults.
- **Inconsistent numeric-pair separators**: `padding-48-0`/`padding-48-96` (dash) vs. `padding-48/24`/`padding-48/0` (slash); `gap-48-32` vs. `gap-48/32` — the same kind of value is expressed two different ways, and in the gap case both spellings exist as literally separate registered classes.
- **`gap24px`** has no separator at all, breaking the `gap-Npx` convention used everywhere else.
- **Singular/plural class-family split** for what is presumably one concept: `amenities-collection*` vs. `amenity-collection*`.
- **Three spellings of "review(s) card(s)"**: `reviews-cards-wrapper` (correct), `reviewes-cards-wrapper` (sic), `reviewes-cards-cms` (sic) — plus the base card itself is named `reviewa-card` (sic, distinct again from the correctly-spelled `review-card-wrapper` used on the homepage marquee).
- **Inconsistent unit suffix on the same button class**: `.primary-button.width-100` (no `%`) vs. `.primary-button.is-alternate.width-100%` (with `%`) — both exist for what looks like the same "full width" intent.
- **Typo family** (non-exhaustive, all verbatim from the class/field census): `secondry-button` (secondary), `Properties Catagories` / `catagory` ×2 (category), `vertival-gap-32px` (vertical), `sleeping-slideer-mask` / `slieeping-slide` (sleeping), `reviewer-progile-image` (profile), `dispplay-none` (display), `acoordion-item` (accordion), `card-pading-0` (padding), `ohone-no-wrapper` (likely "phone-no-wrapper"), `phne-borld` (unclear/garbled), `regu` (truncated, unclear), `calander-check`/`calander-imge` (calendar/image).
- **Two mostly-empty/unclear global classes** `but` and `butt` — apparent abandoned fragments of "button".
- **Two vocabulary words for what looks like one "alternate style" concept**: `is-alt` (hero/CTA buttons, dark background) vs. `is-alternate` (about/guest-card buttons, light background) — never reconciled into one modifier name.
- **Mixed size-token system**: most body copy uses semantic tokens (`text-size-tiny/small/regular/xlarge`) but two literal-pixel classes (`text-size-12px`, `text-size-18px`) sit alongside them as independent base classes rather than combos on the semantic scale.
- **Every one of the 10 CMS collections gets a full template page**, including ones that read as pure sub-item/reference data (Sleeping Arrangements, Highlights, Amenities, Amenity Categories, Locations, FAQs) — all nine non-Properties template pages ship with **empty SEO**, suggesting these pages were provisioned by policy/default rather than intentionally built out as indexable content.

---

## 5. Verbatim dumps

Omitted from the skill copy.

## 6. Tool / data caveats

- **No Figma MCP tool was called.** §1 is entirely "not collected" — file key `AhDWMTo3UrCBWlHjxG8qQM` was never read, per the task's explicit instruction that Figma and Webflow MCP tools hang in this session.
- **No Webflow MCP tool was called either.** Every fact in §2–§5 comes from six pre-staged files in `analysis/atlas/`, read with `Read`/`Bash`/`python3` only: `home_tree.txt`, `home_tree_raw.json`, `styles_raw.txt`, `class_names.txt`, `global_selectors.txt`, `combo_selectors.txt`, plus the prose in `extra_from_main.md`. None of it was re-verified against a live session this run.
- **`styles_raw.txt` is not a `query_styles`-with-properties dump**, despite its name suggesting otherwise and despite the sibling `furec.md` report having exactly that kind of data. Parsed with `python3 -c "import json; d=json.loads(open('styles_raw.txt').read()); print(d['action'], len(d['result']))"` → `get_styles 509`, and every one of the 509 entries has only `{id, name, isFromLibrary, isComboClass, type, selector}` — a `grep -c properties styles_raw.txt` on the raw file returns `0`. **No CSS property values (px, em, colour, breakpoint overrides) exist anywhere in this run's inputs for Atlas.** Every size mentioned in §2/§4 is a number literally embedded in a class *name* (e.g. `gap-36px`, `height-624px`), not a verified computed style.
- **`variables.json` and `cms_schemas.json`**, referenced in `extra_from_main.md`'s own text ("Schemas for Properties/Amenities/Reviews in cms_schemas.json (if present)" / "See variables.json"), **do not exist in the `analysis/atlas/` folder** — confirmed by `find … -iname "*cms*" -o -iname "*variable*"` returning nothing. Only the prose summary inside `extra_from_main.md` itself was available; anything beyond that prose (exact field-validation rules, full Reviews/Amenities field lists beyond what's quoted, the full 23-variable list beyond the sample given) is not collected.
- **No component-internal element trees exist** for `Navigation`, `Footer`, `Footer_Secondary`, `FAQ`, or `Ridge_Republic` (no `scope_component_id` dumps, unlike `furec`'s run) — `home_tree.txt`/`home_tree_raw.json` show these only as unexpanded `ComponentInstance` placeholders with `props: []`, `slots: []`. The Navbar/Footer descriptions in §2 are reconstructed from class names only and are explicitly labelled as inferred.
- **No interactions (IX3) or custom-code dumps exist** — §2's "Interactions/animations" and "Custom code" subsections are "not collected".
- **No Webflow site id, live `*.webflow.io` URL, or image alt text exists in any input file.** The only ids captured are the Home page id (`6a47971ad9af5f1a1ac42985`) and the two destination-hub page ids read directly out of `home_tree_raw.json`'s `Link.settings.link.pageId` values (`6a4b3c03d30e0dd535752466` = Blue Ridge, `6a4b3c20b6e5359eecf18d9b` = Dominican Republic).
- **Only the Home page's DOM was captured.** No element tree exists for `/properties`, `/blue-ridge`, `/dominican-republic`, `/about`, `/contact`, or any of the 10 CMS template pages. The "Properties detail-page template" discussion in §2's CMS section is built entirely by cross-referencing class names that only make sense on a property detail page against the Properties field schema in `extra_from_main.md` — it is inference, not a verified DOM reading.
- **Method used for this report**: `Bash`/`Read`/`Grep` to inspect the six files, and `python3 -c "..."` one-liners (via `Bash`) to parse the two large JSON dumps (`home_tree_raw.json`, `styles_raw.txt`), walk the element tree for link/heading/footer structure, and cross-tabulate `styles_raw.txt`'s `type`/`name`/`selector` fields. No network access, no MCP calls, no files outside `analysis/atlas/` and the sibling `furec.md` (read only for report-format reference) were used.
