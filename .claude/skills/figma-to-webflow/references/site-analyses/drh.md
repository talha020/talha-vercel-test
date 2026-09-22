# DRH (Rohstoffhandel / Metallrecycling)
Figma: https://www.figma.com/design/pSY38IMIfh8mfQavINdKfD/DRH--dev-?node-id=4-2  |  Webflow site id: 6a90318cb7ae0d1b3f2ebf33 (shortName "drh-55fa7e")  |  Live: https://drh-55fa7e.webflow.io/  |  Analysed: 2026-09-21/22

> **Scope note — read first.** The published site could **not** be fetched: the organisation's egress proxy returns `403` on `CONNECT drh-55fa7e.webflow.io:443` (and on `www.figma.com` asset URLs, so no screenshots could be downloaded either — see §6). Everything below therefore comes from the **Figma MCP metadata** and the **Webflow Data/Designer API**, not from rendered HTML/CSS. That has one important consequence for the animation brief: **legacy IX2 interactions (the ones that publish as `data-w-id` + `Webflow.require('ix2')` JSON) are not exposed by any MCP tool**, so where an animation is clearly IX-driven but does not appear in the IX3 list it is marked as *inferred IX2*. Everything that is quoted (CSS, custom code, IX3 timelines, class names, element trees) is verbatim from the API.
>
> Also note: the page slugs given in the brief are the English working names. The built site is **German-slugged**: `/leistungen` (performance), `/nachhaltigkeit` (sustainability), `/kontakt`, `/standorte` (location), `/karriere` (careers), `/ueber-uns` (about), `/recycling`, `/news`, `/materialien`.

---

## 1. Figma file anatomy

### Pages in the file
Exactly **one** Figma page:

- `4:2` — **"Design"**

No Dev / Components / Archive / Style-guide page. As on the sister project FUREC, the entire file is one canvas organised by **Figma Sections** (`<section>` nodes), not by pages. 15 sections, 3 863 frames, 1 821 text nodes, 10 032 rounded-rectangles, 5 827 vectors, 646 component instances, 9 component symbols.

### Sections on the "Design" page (canvas order left→right / top→bottom)

| node id | Section name (verbatim) | size | what it holds |
|---|---|---|---|
| `4531:8977` | `Full Homepage - Updated 25/08` | 8878×12548 | Desktop homepage `home page 1/2` (1920×11628) + 4 `note` frames + 3 alternative `footer` frames + `scroll 11`…`scroll 15` state frames. |
| `4531:29657` | `Homepage+Adaptives 02.09.2026` | 13544×14686 | **The build-from source.** Holds three nested sections: `Mobile_homepage` (393), `Tablet_homepage` (834, incl. `Tablet menu / Active`), and `Homepage_Feedbacks_02.09.2026` — which contains the final desktop frame `4531:37750 "homepage_updated"` (1920×11628), the open-navbar frame `heder-wrap` and `scroll 33`. |
| `4531:6226` | `Inner Page Update 27.07` | 11508×17296 | An 1920×15015 frame literally named `DRH` holding the re-worked inner-page sections. |
| `4531:11093` | `Contact page` | → `/kontakt` |
| `4531:14863` | `News page` | → `/news` |
| `4531:15410` | `About us page` | → `/ueber-uns` |
| `4531:18834` | `Location page` | → `/standorte` |
| `4531:24416` | `Performance page` | → `/leistungen` |
| `4531:27644` | `Sustainability page` | → `/nachhaltigkeit` |
| `4531:28290` | `Сareer page` (note: leading **Cyrillic С**) | → `/karriere` |
| `4531:39577` | `Materials page` | → `/materialien` |
| — | `Sections for other pages` | shared/leftover section blocks |

### Which frames are the "build from" source
`4531:37750 "homepage_updated"` (1920×11628) inside `Homepage_Feedbacks_02.09.2026` is the desktop master; `Tablet_homepage` (834×12042) and `Mobile_homepage` (393×9824) are the adaptive masters. Older `Full Homepage - Updated 25/08` and `Inner Page Update 27.07` are superseded but still on canvas.

### Frame naming pattern for pages and breakpoints
There is **no** `"Home - Desktop 1440"` convention. Observed pattern:

- Desktop master: named after the brand or the job — `DRH`, `home page 1/2`, `homepage_updated`.
- Breakpoint masters: `<Device>_homepage` — **`Tablet_homepage`**, **`Mobile_homepage`** (identical to FUREC, including the `Desctop_homepage` style of naming).
- Menu states: `Tablet menu / Active`.
- Inner pages: whole Figma **Sections** named `"<Topic> page"` in **English** even though the site is German.
- Scroll-state artboards: `scroll 11`, `scroll 12`, … `scroll 33` — one frame per key-frame of a scroll animation. This is how motion is specified: **as a series of static frames, not as a prototype.**
- Dated names are used as version control: `Updated 25/08`, `02.09.2026`, `Inner Page Update 27.07`, `Home Copy 16 Sep` on the Webflow side.

### Breakpoints that exist in Figma
Frame-width census over the whole file:

```
242 × 1920   desktop master + every per-page desktop frame + scroll states
 16 ×  834   tablet (iPad Air portrait)
 16 ×  393   mobile
  4 ×  767   a small intermediate set
```
→ **1920 / 834 / 767 / 393**, which maps onto Webflow `main / medium / small / tiny`.

### Section naming inside the homepage frame (verbatim, top to bottom, `4531:37750`)
Every content band is literally called **`section`** — no descriptive names at all.

| node id | frame name | y | h | first text inside (content identifier) |
|---|---|---|---|---|
| `4531:37751` | `section` | 0 | 1080 | "Rethinking Metal Recycling" (hero) |
| `4531:37771` | `section` | 1080 | 797 | "Rohstoffe im Kreislauf. Zukunft in Bewegung." |
| `4531:37780` | `section` | 1877 | 1986 | "Bis zu 95% CO₂-Einsparung" |
| `4531:37808` | `section` | 3863 | 1050 | "Geschäftsfelder" |
| `4531:37821` | `footer` | 4913 | 1080 | (world-map band — *called `footer` although it is not one*) |
| `4531:38303` | `left` | 5993 | 348 | "Metallkreislauf" |
| `4531:38317` | `Frame 2147238984` | 6341 | 2233 | "INPUT" (the material-cycle diagram) |
| `4531:38426` | `section` | 8574 | 894 | "Geschäftsfelder" |
| `4531:38440` | `footer` | 9468 | 1080 | "DRH verbindet" |
| `4531:38470` | `heder-wrap` | 0 | 120 | the **navbar**, pinned at y=0 (sic: "heder") |
| `4531:38498` | `footer` | 10548 | 1080 | "Sie haben Material?" (the real footer / CTA) |

Recurring inner frame names (census): `Frame` ×263, `row` ×205, `top` ×165, `text` ×136, `icon` ×129, **`bnt prm` ×120** (sic), `spacer` ×99, `bg wrap` ×91, `heading` ×70, `section` ×63, `list` ×59, `col` ×52, `Eyebrow` ×51, `tag` ×46, `left` ×40, `bottom` ×37, `heder inner` ×36, `date` ×32, `link group` ×26, `midl`/`midl-left`/`midl-right` ×26 each (sic), `footer` ×22, `nav` ×18, `heder-wrap` ×18, `heder` ×18.

Verbatim misspellings that recur and are worth knowing: **`heder-wrap` / `heder`**, **`bnt prm` / `bnt sec`**, **`midl`**, **`Сareer page`**, and several junk names typed with a Russian keyboard layout still in the file: `дшые шефь`, `Frame 2147238933 фвамasdfva`, `Frame 2147238922sdf`.

### Design tokens (Figma variables, verbatim `get_variable_defs` on `4531:37750`)

**Colour**
| variable | value |
|---|---|
| `theme/background` | `#191919` |
| `theme/background-2` | `#edeede` |
| `theme/text` | `#ffffff` |
| `theme/accent` | `#d1f462` |
| `theme/border` | `#ffffff33` |
| `brand-500` | `#d1f462` |
| `brand-200` | `#f6fde0` |
| `button/primary/background` | `#d1f462` |
| `button/primary/text` | `#191919` |
| `button/primary/text-hover` | `#ffffff` |
| `button/secondary/text` | `#ffffff` |

**Typography — font-size tokens (px at the 1920 frame)**
`font-size/display` 69 · `h1` 55 · `h2` 44 · `h3` 35* · `h4` 28 · `h5` 23 · `text-large` 18 · `text-main` 16 · `text-small` 14 · `text-extra-small` 12
(*h3 not returned for this node but present in the identical FUREC token set.)

**Text styles (composed `Font(...)` values)**
| style | family | size | weight | line-height | letter-spacing |
|---|---|---|---|---|---|
| `Display` | Geist | 69 | 400 | 1.05 | −3 |
| `H1` | Geist | 55 | 400 | 1.0 | −3 |
| `H2` | Geist | 44 | 400 | 1.10 | −3 |
| `H4` | Geist | 28 | 400 | 1.15 | −1 |
| `H5` | Geist | 23 | 400 | 1.30 | 0 |
| `text-large` | Geist | 18 | 400 | 1.35 | 0 |
| `text-main` | Geist | 16 | 400 | 1.50 | 0 |
| `text-small` | Geist | 14 | 400 | 1.40 | 0 |
| `text-extra-small` | Geist | 12 | 400 | 1.50 | −1 |

Font family tokens: `primary-family` = **Geist**; weights exposed as `primary-regular` = Regular, `primary-medium` = Medium, `primary-bold` = Semi Bold.

**Spacing / geometry**
`space/2` 4 · `space/3` 8 · `space/4` 12 · `space/5` 16 · `space/6` 24 · `space/7` 32 · `space/9` 48 · `space/10` 64 · `site/margin` 24 · `site/gutter` 20 · `section-space/small` 80 · `section-space/main` 112 · `border-width/main` 1.5 · `radius/round` 99999.

This is **the same token file as FUREC** (same names, same values, same `Font(...)` shape) — i.e. the team carries one token library across the DRH group of brands.

### Components in Figma (verbatim instance names, by frequency)
`nav link v1` ×110, `Impressum desctop v1` ×74 (sic), `icon-3 / 21` ×53, `Link item 1` ×48, `col title` ×36, `bg wrap` ×28, `visual м3` ×20 (sic, Cyrillic м), `icon-furec / recycle` ×18, `Calendar` ×16, `List item 01` ×15, `Partner wrap v1` ×14, `icon / route` / `icon / package` / `icon / cart` ×13 each, `card-Metalle v1` ×10, `Interface/clipboard-check` ×10, `bento v1` ×7, `Interface/funnel` ×7, `card v4` ×6, `card co2 v1` ×6, `Technologie v1` ×6, `Process v1` ×6, `icon / factory` ×6, `Link item 3` (the navbar dropdown card) ×3.

Naming is **versioned, not variant-based**: `v1`, `v2`, `v4`, `м3` suffixes instead of Figma component variants. Icons are the only properly namespaced set (`icon / route`, `Interface/funnel`, `icon-furec / recycle` — the last one leaked in from the FUREC file).

### Auto-layout usage
Yes — frames such as `row`, `col`, `top`, `list`, `group`, `tag`, `link group` are auto-layout containers, but **explicit `spacer` frames (×99) are used instead of gaps in many places** (e.g. `spacer` 1006×12 and 1006×48 between hero headline, paragraph and buttons). Desktop content is `1872` wide inside a `1920` frame → **24 px side margin** (= `site/margin`), matching `padding-global: 1.5em` in the build. Section heights are on a 1080 grid (1080, 1050, 1008, 894, 797).

### Asset types
Photographs (hero, benefit cards, location cards), a very large inline **vector world map** (`world 1` → ~5 800 `<vector>` country paths, used for the scroll-zoom map animation), SVG icon components, the `LOGO A5 1` logo frame, and `Group 5` / `Rectangle 7108` background plates.

---

## 2. Webflow site anatomy

### Pages (verbatim title → slug)
| Title | Slug | note |
|---|---|---|
| Home | `/` | pageId `6a90318db7ae0d1b3f2ebf8f` |
| News | `/news` | Finsweet-filtered CMS index |
| Über Uns | `/ueber-uns` | |
| Leistungen | `/leistungen` | |
| Nachhaltigkeit | `/nachhaltigkeit` | |
| Kontakt | `/kontakt` | |
| Standorte | `/standorte` | |
| Karriere | `/karriere` | |
| Recycling | `/recycling` | |
| Materialien | `/materialien` | |
| Materialien V2 | `/materialien-v2` | work-in-progress duplicate, **not draft — it publishes** |
| Home Safari | `/home-safari` | Safari-specific duplicate, **publishes** |
| Home Copy 16 Sep | `/home-copy-16-sep` | dated backup, **publishes** |
| Home Copy | `/home-copy` | draft |
| Home Copy 2 | `/home-copy-2` | draft |
| Blogs Template | `/blogs` (`detail_blogs`) | CMS template |
| Blog Categories Template | `/blog-categories` (`detail_blog-categories`) | CMS template |

No style-guide page, no 404, no password page, no folders. Every real page has a hand-written German SEO title + description (e.g. Home: `"DRH Rohstoffhandel: Metallrecycling & Sekundärrohstoffe"`), and `openGraph.titleCopied / descriptionCopied = true` everywhere — OG is always inherited, never written separately.

### Class naming convention
**A custom system that is Client-First-*flavoured* but not Client-First.** Evidence: the CF structural trio is present and used on every single section (`page-wrapper` → `main-wrapper` → `section_*` → `padding-global` → `padding-vertical` → `container-large`), and CF's typography utilities exist (`text-size-small/regular/medium/large/xsmall/tiny`, `text-color-white`, `heading-styles-h1..h5`, `button-group`, `divider-vertical`, `max-wd-*`). But the CF names are consistently *altered*: CF's `padding-section-large` is replaced by **`padding-vertical` + an `is-*` combo per section**, CF's `heading-style-h2` is spelled **`heading-styles-h2`** (plural), CF's `container-large` keeps its name but is sized in **em**, and blocks use the Relume-ish **`component_element`** underscore convention (`nav_inner`, `blog_card`, `footer_card-grid`, `location_card`, `figures_card-inner`).

30+ verbatim examples, grouped:

- **Layout / structure:** `page-wrapper`, `main-wrapper`, `padding-global`, `padding-vertical`, `container-large`, `section_hero`, `section_future`, `section_benefits`, `section_business-areas`, `section_sorting`, `section_cycle`, `section_materials`, `section_connects`, `section_footer`, `section_blogs`, `section_wrapper`, `relative`, `sticky-wrap`, `sticky_scroll`, `sticky_scroll-tech`, `wrapper_hero`, `wrapper_benefits`, `wrapper_footer`, `wrapper_sorting`, `wrapper_future`, `wrapper_blogs`.
- **Typography:** `heading-styles-h1`, `heading-styles-h2`, `heading-styles-h3`, `heading-styles-h4`, `heading-styles-h5`, `text-size-large`, `text-size-medium`, `text-size-regular`, `text-size-small`, `text-size-xsmall`, `text-size-tiny`, `text-color-white`, `text-color-white-80%`, `text-color-primary`, `text-color-primary-dark`, `opacity-70%`, `quote_style`, `section-tag`, `card-eyebrow`.
- **Buttons:** `button-primary`, `button-secondary`, `button-secondary-black`, `button_secondary-small`, `button_text`, `button_arrow`, `button-group`, `submit_button`.
- **Components / cards:** `blog_card`, `blog_grid`, `blog_image`, `benefit_card`, `benefit_card-lower`, `benefit_image`, `benefits_row`, `int_card`, `int_card-bg`, `feature_card`, `figures_card`, `figures_grid`, `location_card`, `location_tag`, `loc_card-gradient`, `material_card-bg`, `process_card-bg`, `service_card`, `step_card`, `timeline_card`, `connect_card`, `adv_card`, `list_card`, `footer_card`, `footer_card-grid`, `nav_inner`, `nav_menu`, `nav_menu-inner`, `nav_link`, `nav_brand`, `navbar_bg`, `dropdown_list`, `dropdown_list-inner`, `dropdown-toggle`, `dropdown_icon`, `menu_btn`, `hamburger`, `hamburger_bar`, `filter_inner`, `filter_radio`, `filter_radio-field`, `radio_label`.
- **Utility / modifier (combo) classes:** `is_hero`, `is-home`, `is_home-hero`, `is-future`, `is-benefits`, `is-areas`, `is-metal-cycle`, `is_value-chain`, `is-footer`, `is-flow`, `is-padding`, `is-inner-padding`, `is--1`, `is--2`, `is--3`, `is--5`, `is_dark`, `is_gradient`, `is_logo`, `is-small`, `is-mbl`, `is_desktop`, `is-16px-tab`, `14px-mbl`, `0-tab`, `display-none`, `v2`, `map`, `relative`, `max-wd-490px`, `max-wd-576px`, `max-wd-840px`, `flex-v-gap-12px`, `flex-v-gap-16px`, `icon-24px`, `icon-28px`, `icon_wrap-40px`, `vector_152px`, `vector-12px`.
- **State classes:** `is-active` (`.filter_radio-field.is-active`), `is-list-loading`, `is-dropdown`, `is-dropdown.hide`, `stop-scroll` (added by the Lenis toggle script), plus Webflow's own `w--open` used in CSS selectors.

Two conventions collide and both are in use: **hyphen** (`benefit_card-lower`, `text-size-small`) and **underscore** (`nav_inner`, `blog_card`). `is-` and `is_` are both used, sometimes on the same page (`is_hero` next to `is-home`).

### DOM skeleton — homepage, body → hero (verbatim classes)

```
Body
└ Block .page-wrapper
  └ Block .main-wrapper
    ├ ComponentInstance "Global Styles"          ← single HtmlEmbed, root CSS (see below)
    ├ ComponentInstance "Navbar"
    ├ Section .section_hero.is_home              (tag=section)
    │ ├ Block .padding-global.is_hero
    │ │ └ Block .padding-vertical.is_home-hero
    │ │   └ Block .container-large.is_hero
    │ │     └ Block .wrapper_hero
    │ │       └ Block .section-title-wrapper.is_hero
    │ │         ├ Block .title_left-wrap.is-padding
    │ │         │ └ Heading h1 .heading-styles-h1  "Rethinking Metal Recycling"
    │ │         ├ Block .title_right-wrap
    │ │         │ └ Paragraph .text-size-large.is-16px-tab.text-color-white-80%
    │ │         │     ├ Span .text-color-white "Hightech-Recycling,"
    │ │         │     ├ " internationale Rohstoffvermarktung und moderne Kreislaufwirtschaft. Wir verwandeln "
    │ │         │     ├ Span .text-color-white "komplexe Metallströme"
    │ │         │     ├ " in "
    │ │         │     ├ Span .text-color-white "hochwertige Sekundärrohstoffe"
    │ │         │     └ " für die Industrie von morgen."
    │ │         └ Block .button-group.is-absolute
    │ │           ├ ComponentInstance "Button Primary"   (Text="Kontakt aufnehmen", Button Link→/kontakt)
    │ │           └ ComponentInstance "Button Secondary" (Text="Profil herunterladen", override Link→/ueber-uns)
    │ ├ Image     .hero_image-absolute.is-home.display-none   (assetId 6a904096cab25430c5647d1b)
    │ ├ HtmlEmbed .hero_image-absolute.is-home                ← the live hero visual
    │ ├ BackgroundVideoWrapper .hero_image-absolute.is-home.display-none
    │ └ Block     .hero_gradient.is-home
    ├ Section .section_future        → .padding-global.relative → .padding-vertical.is-future → .container-large
    ├ Section .section_benefits      → .padding-global.relative → .padding-vertical.is-benefits → .container-large
    ├ Section .section_business-areas→ .padding-global.0-tab    → .padding-vertical.is-future  → .container-large
    ├ Section .section_sorting       → .padding-global.map      → .sticky_scroll → .wrapper_sorting.v2
    ├ Section .section_cycle         → .padding-global.is-flow  → .padding-vertical.is-metal-cycle → .container-large
    ├ Section .section_materials     → .padding-global.relative → .padding-vertical.is-areas   → .container-large
    ├ Section .section_connects      → .padding-global.relative → .padding-vertical.is_value-chain → .container-large
    └ ComponentInstance "Footer"     (props Title 1="Sie haben Material?", Title 2="Sie brauchen Rohstoffe?")
```

Note the **three stacked hero media elements** (Image / HtmlEmbed / BackgroundVideoWrapper) where two carry `display-none` — a leftover of trying video, then an embed, then a static image.

### Representative card grid (homepage `section_benefits`)

```
Block .container-large
└ Block .wrapper_benefits.is-home
  ├ Block .sticky-wrap
  │ └ Block .benefit_vector-wrap
  │   └ Image .benefit_vector          (assetId 6a92dde25162f1b7d9f125e2)
  ├ Block .benefits_row.is--1
  │ ├ Block .benefit_card
  │ │ ├ Image .benefit_image
  │ │ └ Block .benefit_card-lower
  │ └ Block .benefit_card.is--2
  │   ├ Image .benefit_image
  │   └ Block .benefit_card-lower
  └ Block .benefits_row.is--2
    ├ Block .benefit_card            (+ .benefit_card-lower)
    └ Block .benefit_card.is--2      (+ .benefit_card-lower)
```
i.e. **rows + index combos (`is--1`, `is--2`) rather than a CSS grid** — the offset/staggered card layout is built by hand.

### Footer (component "Footer", 15 instances)

```
Section .section_footer (tag=section)
└ Block .padding-global.relative
  └ Block .padding-vertical.is-footer
    └ Block .container-large
      └ Block .wrapper_footer
        ├ Block .footer_card-grid
        │ ├ Block .footer_card              ← bound to prop "Title 1" ("Sie haben Material?")
        │ └ Block .footer_card.is_dark      ← bound to prop "Title 2" ("Sie brauchen Rohstoffe?")
        ├ Block .footer_middle-wrap
        │ ├ Block .footer_middle-left
        │ └ Block .footer_middle-right
        ├ Block .footer_lower-wrap
        │ ├ Block .footer_lower-left
        │ ├ Block .divider-vertical
        │ └ Block .footer_lower-right
        └ Block .footer_legal
          ├ Block .text-size-small.text-color-white
          └ Block .legal_links-wrap
```

### Navbar (component "Navbar", 15 instances) — full structure

```
NavbarWrapper .navbar                                (position:fixed; top:1.5em; max-width:74.69em; margin auto)
└ Block .navbar_bg
  └ Block .nav_inner
    ├ NavbarBrand .nav_brand → Image .image          (logo asset 6a905de5e74a458dc3b6f74c)
    ├ NavbarMenu .nav_menu
    │ └ Block .nav_menu-inner
    │   ├ NavbarLink .nav_link.is_home               "Home"
    │   ├ DropdownWrapper .dropdown                  "Unternehmen"
    │   │ ├ DropdownToggle .dropdown-toggle
    │   │ │ ├ Block .toggle_text  "Unternehmen"
    │   │ │ └ HtmlEmbed .dropdown_icon               ← inline SVG chevron (rotated by IX)
    │   │ └ DropdownList .dropdown_list
    │   │   └ Block .dropdown_list-inner             (display:grid; 3 columns; max-width 74.69em)
    │   │     ├ Link .list_card → .text-size-small.14px-mbl "Leistungen"  + Image .vector_152px
    │   │     ├ Link .list_card … "Recycling"  (page 6a9bf0c7…)
    │   │     ├ Link .list_card … "Standorte"
    │   │     ├ Link .list_card … "Über uns"
    │   │     ├ Link .list_card … "News"
    │   │     ├ Link .list_card … "Kontakt"
    │   │     └ HtmlEmbed .gradient-css
    │   ├ DropdownWrapper .dropdown  #nachhaltigkeit-dd   "Nachhaltigkeit"
    │   │ └ DropdownList .dropdown_list.is--2 #nachhaltigkeit-menu
    │   │   └ .dropdown_list-inner → 3 × Link .list_card → /nachhaltigkeit#kreislaufwirtschaft,
    │   │                                   #mitgliedschaften, #zertifikate      (anchor links)
    │   ├ DropdownWrapper .dropdown  #nachhaltigkeit-dd   "Materialien"   ← duplicated id, see §4
    │   │ └ DropdownList .dropdown_list.is--3 #nachhaltigkeit-menu
    │   │   └ .dropdown_list-inner
    │   │     ├ Link .list_card "Metalle & Legierungen"
    │   │     ├ Link .list_card "Aluminium & Aluminium legierungen"
    │   │     ├ Link .list_card "Kupfer & Kupferlegierungen"
    │   │     └ Block .link_grid
    │   │       ├ Link .list_card "Sekundärrohstoffe & Recycling fraktionen"
    │   │       └ Link .list_card "Metall-Lexikon & Rohstoffwissen"
    │   ├ NavbarLink .nav_link  "Kontakt"
    │   ├ NavbarLink .nav_link  "Karriere"
    │   └ Link .button-primary.is-mbl → .button_text.small "Kontakt aufnehmen" + HtmlEmbed .button_arrow
    ├ Block .nav_right-wrap
    │ ├ Link .button-primary.is-small.is_desktop → .button_text.small + HtmlEmbed .button_arrow
    │ ├ NavbarButton .menu_btn
    │ │ └ Block .hamburger
    │ │   ├ Block .hamburger_bar.top
    │ │   ├ Block .hamburger_bar.middel      (sic)
    │ │   └ Block .hamburger_bar.bottom
    │ └ HtmlEmbed .border-css
    └ (sibling of .navbar_bg) Block .bg-absolute      ← the glass plate behind the whole bar
```

Key CSS, verbatim:

```css
.navbar{position:fixed;left:0%;top:1.5em;right:0%;bottom:auto;width:100%;
        max-width:74.69em;margin-right:auto;margin-left:auto;
        justify-content:center;align-items:flex-start;
        border-radius:.75em;background-color:var(--transparent)}
@media medium { .navbar{width:auto;max-width:none;margin-right:1.5em;margin-left:1.5em} }
@media small  { .navbar{top:1.25em;margin-right:1em;margin-left:1em} }

.navbar_bg{position:relative;display:flex;width:100%;
           padding:1em 1em 1em 1.5em;justify-content:center;align-items:flex-start;
           border-radius:.75em}
@media small { .navbar_bg{padding:1em} }

.nav_inner{position:relative;z-index:5;display:flex;width:100%;
           justify-content:space-between;align-items:center;gap:2em}

.nav_link{padding:0;transition:color 250ms ease;color:var(--white);
          font-size:1em;line-height:150%}
@media medium { .nav_link{margin:0;font-size:1.13em} }

.dropdown-toggle{display:flex;padding:.56em 0;justify-content:center;align-items:center;
                 gap:.5em;color:var(--white)}
@media medium { .dropdown-toggle{padding:0;justify-content:flex-start;align-items:center} }

.dropdown_icon{display:flex;width:1em;height:1em;justify-content:center;align-items:center}

.dropdown_list-inner{display:grid;width:100%;max-width:74.69em;
                     padding:2em 1.13em 1.5em;grid-auto-columns:1fr;gap:.5em;
                     grid-template-columns:1fr 1fr 1fr;grid-template-rows:auto}
@media medium { .dropdown_list-inner{padding:.5em 0 0;grid-template-columns:1fr} }

.list_card{position:relative;display:flex;width:100%;height:15em;padding:1.5em;
           flex-direction:column;justify-content:flex-end;align-items:flex-start;
           border:1px solid #2e131f;border-radius:.5em;text-decoration:none}

.bg-absolute{position:absolute;inset:0;border-radius:.75em;
             background-color:var(--black-60);backdrop-filter:blur(5px)}

@media medium {
  .nav_menu{z-index:20;width:100%;background-color:var(--transparent)}
  .nav_menu-inner{overflow:auto;width:100%;max-height:85vh;padding:2em 1em;
                  flex-direction:column;justify-content:flex-start;align-items:flex-start;gap:1.5em;
                  border-bottom-left-radius:.75em;border-bottom-right-radius:.75em;
                  background-color:hsla(0,0%,0%,.60);backdrop-filter:blur(5px)}
  .menu_btn{display:flex;width:40px;height:40px;padding:0;justify-content:center;align-items:center;
            border:1px solid hsla(0,0%,100%,.10);border-radius:6px;
            background-color:hsla(0,0%,100%,.04)}
}
```

**The navbar is "full width" in the sense that the fixed bar spans the viewport and centres a 74.69em (≈1195px at 1920) pill**; the dropdown panel uses the *same* `74.69em` max-width and a 3-column grid, so the dropdowns read as **centred mega-menus** the width of the bar, not as little link lists. The rounded corners are joined to the bar when open by the `border-css` embed (below).

### Buttons
| class | look | hover (verbatim) |
|---|---|---|
| `.button-primary` | lime pill: `padding:.94em 1.44em; border:1px solid var(--lime-green); border-radius:96px; background:var(--lime-green); color:var(--theme-black); transition:border-color, transform, color, background-color 250ms ease` | `border-color:var(--white); background-color:var(--theme-black); transform:scale(0.95); color:var(--white)` |
| `.button-secondary` | ghost pill: `padding:1em 1.5em; border:1px solid var(--white); border-radius:9999px; background:transparent; color:var(--white); transition:background-color,color,transform 250ms ease` | `background-color:var(--white); transform:scale(0.95); color:var(--theme-black)` |
| `.button-secondary-black` | `padding:.63em .88em .63em 1.25em; border:1px solid var(--theme-black); background:var(--theme-black); color:var(--white); transition:all 250ms ease` | `background-color:var(--transparent); transform:scale(0.95); color:var(--theme-black)` |
| `.button_secondary-small` | `padding:1em 1.5em; border:1px solid var(--theme-black); border-radius:96px; background:var(--theme-black); color:var(--lime-green)` | `background-color:var(--white); transform:scale(0.95); color:var(--theme-black)` |
| `.submit_button` | `padding:1em 3em 1em 1.5em; border-radius:96px; background:var(--theme-black) + background-image:@img_6a99feb7e4dc6b7a6d367c90 at 12em/1em no-repeat; color:var(--lime-green)` | `transform:scale(0.95)` |
| `.button_arrow` | `display:flex;width:1em;height:1em;min-width:1em;justify-content:center;align-items:center` — an inline-SVG **HtmlEmbed**, not an image | — |
| `.button_text` | `font-size:1em;line-height:150%` | — |
| `.button-group` | `display:flex;justify-content:flex-start;align-items:stretch;gap:.75em` | — |

**The universal hover signature on this site is `transform: scale(0.95)` + colour inversion over 250 ms ease** — it is on every button class. The arrow inside the button is its own 1em embed, which is what the IX2 arrow slide/rotate hooks onto.

### Variables in Webflow
One collection, **"Colors"** (`collection-57d66751-8b04-cfde-cb65-5fb241cda9a9`), single "Base mode", 8 variables:

| name | cssName | value | Figma token it maps to |
|---|---|---|---|
| White | `--white` | `white` | `theme/text` |
| Black | `--black` | `hsla(0,0%,0%,1)` | — |
| Lime Green | `--lime-green` | `#d1f462` | `theme/accent` / `brand-500` |
| Theme Black | `--theme-black` | `#191919` | `theme/background` |
| Black 60% | `--black-60` | `hsla(0,0%,0%,.60)` | — (glass plates) |
| White 80% | `--white-80` | `hsla(0,0%,100%,.80)` | — (body copy on dark) |
| Transparent | `--transparent` | `hsla(0,0%,0%,0)` | — |
| Theme Backgroud 2 | `--theme-backgroud-2` | `#edeede` | `theme/background-2` (**typo "Backgroud" shipped**) |

**No size, spacing, radius or font variables were created** — the Figma spacing/type tokens were flattened into em values in classes instead.

### Typography — class → CSS per breakpoint (verbatim)
```css
.heading-styles-h1{color:var(--white);font-size:6.69em;line-height:105%;font-weight:400;letter-spacing:-.02em}
  medium 4em | small 2.5em | tiny 2.25em
.heading-styles-h2{color:var(--theme-black);font-size:3.44em;line-height:100%;font-weight:400;letter-spacing:-.03em}
  medium 2.5em/120% | small 1.75em | tiny 1.38em
.heading-styles-h3{color:var(--theme-black);font-size:2.75em;line-height:110%;font-weight:400;letter-spacing:-.02em}
  medium 2em | small 1.75em | tiny 1.38em
.heading-styles-h4{color:var(--white);font-size:2.19em;line-height:120%;font-weight:400;letter-spacing:-.02em}
  medium 1.75em | small 1.38em | tiny 1.19em
.heading-styles-h5{color:var(--white);font-size:1.75em;line-height:115%;font-weight:600;letter-spacing:-.02em}
  small 1.25em/600 | tiny 1.38em      ← note tiny is LARGER than small (bug)
.text-size-large{color:var(--white);font-size:1.75em;line-height:115%;letter-spacing:-.01em}
  medium 1.5em | small 1.25em | tiny 1.13em
.text-size-medium{color:var(--theme-black);font-size:1.44em;line-height:130%;letter-spacing:-.01em}
  medium 1.25em | small 1.13em
.text-size-regular{color:var(--theme-black);font-size:1.13em;line-height:135%}   small 1em
.text-size-small{color:var(--theme-black);font-size:1em;line-height:150%}        tiny .88em
.text-size-xsmall{color:var(--theme-black);font-size:.88em;line-height:150%}
.text-size-tiny{font-size:.75em;line-height:150%;letter-spacing:-.01em}
```
Cross-check with Figma: H1 6.69em × 16px = **107px**, but the Figma `H1` token is 55px and `Display` is 69px — the built hero headline is **much bigger than the design token**, i.e. the hero type was scaled up during the build. H2 3.44em = 55px = the Figma `H1` value; H3 2.75em = 44px = Figma `H2`; H4 2.19em = 35px = Figma `H3`; H5 1.75em = 28px = Figma `H4`. **The whole heading scale is shifted by one step** relative to the Figma token names.

### Spacing system (verbatim)
```css
.padding-global{padding-right:1.5em;padding-left:1.5em}        /* = 24px = Figma site/margin */
  small { padding-right:1.25em;padding-left:1.25em }
.padding-vertical{padding-top:7em;padding-bottom:7em}          /* = 112px = Figma section-space/main */
  medium 5em | small 3.13em | tiny 2.5em
.container-large{width:100%;max-width:117em;margin-right:auto;margin-left:auto}  /* = 1872px */
.title_left-wrap.is-padding{padding-bottom:5.25em}  medium 0em
.grid-2x1.is-inner-padding{padding:1.5em 0;gap:4.5em}  medium {padding:0;gap:3em} small 3.5em tiny 3em
```
`container-large: 117em` is exactly the Figma content width (1872 = 1920 − 2×24).

### The em system — the single most important build rule
The `Global Styles` component is one `HtmlEmbed` placed as the **first child of `.main-wrapper` on every page (15 instances)**. Verbatim:

```html
<style>
body { font-size: 0.8333333333333334vw; }
/* Max Font Size */
@media screen and (min-width:1920px) { body {font-size: 16px;} }
/* Container Max Width */
.container { max-width: 1920px; }
/* Min Font Size */
@media screen and (max-width:991px) { body {font-size: 16px;} }
</style>

<style>
/*Reset apple form styles*/
input, textarea, select { -webkit-appearance:none; -moz-appearance:none; appearance:none; border-radius:0; background-image:none; }
* { font-variant-ligatures: none; }
.w-select { -webkit-appearance:none; -moz-appearance:none; }
select { -webkit-appearance:menulist-button; color:black; }
</style>
```

`0.83333vw` = 16 px at 1920 px. So **every `em` in the stylesheet is 1/120th of the viewport width between 992 px and 1920 px, and a hard 16 px outside that range.** That is why literally every padding, width, gap, radius and font-size in this project is written in `em` (`7em`, `117em`, `74.69em`, `6.69em`, `1.56em`, `0.48em`) — the desktop design scales fluidly and pixel-locks on tablet/mobile, where the `medium/small/tiny` overrides take over.

### Responsive behaviour
Breakpoints customised: `medium` (≤991), `small` (≤767), `tiny` (≤479). `large/xl/xxl` untouched. Typical changes: font-size steps on every heading/text class; `padding-vertical` 7em→5em→3.13em→2.5em; `padding-global` 1.5em→1.25em at small; navbar switches from centred pill to edge-margin bar and the menu becomes an overlay panel (`.nav_menu-inner` gets `max-height:85vh`, column direction, `backdrop-filter:blur(5px)`); `dropdown_list-inner` 3 columns → 1 column; dedicated mobile/tablet helper combos (`is-mbl`, `is_desktop`, `14px-mbl`, `is-16px-tab`, `0-tab`, `vector-mbl`, `display-none`) toggle which of two duplicated elements is shown; IX3 scroll animations are switched off per breakpoint via `conditionalPlayback`.

### Components
Only **six** Webflow components exist, all site-wide:

| component | instances | props |
|---|---|---|
| `Global Styles` | 15 | none (a single HtmlEmbed) |
| `Navbar` | 15 | none |
| `Footer` | 15 | `Title 1` (textContent, default "Sie haben Material?"), `Title 2` (textContent, default "Sie brauchen Rohstoffe?") |
| `Button Primary` | 39 | `Link` (link, default `#`), `Text` (textContent, default "Kontakt aufnehmen"), `Button Link` (link, default page → /kontakt) |
| `Button Secondary` | 15 | `Button Link` (link), `Text` (default "Profil herunterladen") |
| `Button Secondary Black` | 0 | `Link`, `Text` (default "Mehr erfahren") — **built but never used** |

No variants anywhere. Cards, sections and grids are **not** components — they are repeated markup with classes.

### CMS
Two collections:

- **Blogs** (`blogs`) — fields: `main-image` (Image, "Hauptbild"), `short-description-2` (PlainText single-line, "Kurzbeschreibung"), `published-date-2` (DateTime, "Veröffentlichungsdatum"), `category` (Reference → Blog Categories, "Kategorie"), plus `name` / `slug`.
- **Blog Categories** (`blog-categories`) — `icon-embed` (RichText, "Icon (SVG-Embed)"), `name`, `slug`.

Field **display names are German, slugs are English**, and the `-2` suffixes show the fields were deleted and recreated once. Templates: `Blogs Template` → `/blogs/:slug`, `Blog Categories Template` → `/blog-categories/:slug`.

### Interactions / animations — how the motion is actually built
Four distinct mechanisms, no GSAP anywhere:

**1. Lenis smooth scroll (site-wide footer code).** Verbatim, site custom code → footer:
```html
<link rel="stylesheet" href="https://unpkg.com/lenis@1.2.3/dist/lenis.css">
<script src="https://unpkg.com/lenis@1.2.3/dist/lenis.min.js"></script>
<script>
let lenis = new Lenis({
  lerp: 0.1,
  wheelMultiplier: 0.7,
  gestureOrientation: "vertical",
  normalizeWheel: false,
  smoothTouch: false,
});
function raf(time) { lenis.raf(time); requestAnimationFrame(raf); }
requestAnimationFrame(raf);
$("[data-lenis-start]").on("click", function () { lenis.start(); });
$("[data-lenis-stop]").on("click", function () { lenis.stop(); });
$("[data-lenis-toggle]").on("click", function () {
  $(this).toggleClass("stop-scroll");
  if ($(this).hasClass("stop-scroll")) { lenis.stop(); } else { lenis.start(); }
});
</script>
```
This is the whole "premium feel": every scroll-scrubbed IX3 timeline is being driven through Lenis' interpolated scroll, which is why the map zoom and the section transitions feel weighted.

**2. Webflow IX3 scroll timelines (9 of them, all `wf:scroll` with `scrub`).** `data_interactions_tool > list_interactions` returns exactly:
`Map Scroll`, `Map While Scrolling`, `Map While Scrolling Mobile`, `Map While Scrolling Tablet`, `Map While Scrolling Tab V2`, `Map While Scrolling Mobile V2`, `Concept Ellipse While Scrolling`, `Section Transition`, `Section Transition Mobile`.
Every one of them uses the same trigger config:
```json
{"extensionKey":"wf:scroll","config":{"controlType":"scroll","scrollTriggerConfig":{
  "showMarkers":true,"clamp":true,"start":"top 5%","end":"bottom top",
  "scrub":0.8,"enter":"play","leave":"none","enterBack":"none","leaveBack":"none"}},
 "target":{"extensionKey":"wf:class","value":["e235a9d0-…"]}}
```
i.e. **class-targeted, clamped, `scrub: 0.8`, markers left switched on** (`showMarkers:true` ships in production). Playback is disabled per breakpoint, e.g. the homepage map:
```json
"conditionalPlayback":[{"type":"breakpoint","behavior":"dont-animate","breakpoints":["medium","small","tiny"]}]
```

**3. The dark→light section transition** — this is an IX3 timeline that *animates a CSS variable-backed background-colour*, not a class swap. From `Section Transition` (page `/leistungen`, trigger class `.section_why-drh`, `start:"top 15%"`, `end:"bottom top"`, `scrub:0.8`):
```json
{"id":"ta-f7d8e5da","name":"Bg Color","presetId":"preset-custom-animation",
 "targets":[{"extensionKey":"wf:class","value":["b7b6221c-…"]}],      // .section_wrapper
 "timing":{"duration":0.1,"position":0,"ease":7},"tt":2,
 "properties":{"wf:style":{"backgroundColor":[
    {"type":"ref","value":{"variableId":"variable-6bff4cab-…"}},        // Theme Black #191919
    {"type":"ref","value":{"variableId":"variable-4674e421-…"}}]}}}    // White
{"id":"ta-f4f7f249","name":"Fade In",
 "targets":[{"extensionKey":"wf:class","value":["d421c7e9-…"]}],
 "timing":{"duration":0.06,"position":0.09,"ease":4},"tt":2,
 "properties":{"wf:transform":{"opacity":["0%","100%"]}}}
```
and the mirror image on the homepage (`Map While Scrolling`, first action):
```json
{"id":"ta-61f14646","name":"Section Bg Initial",
 "targets":[{"extensionKey":"wf:class","value":["e235a9d0-…"]}],       // .section_sorting
 "timing":{"duration":0.01,"position":0.01,"ease":0},"tt":2,
 "properties":{"wf:style":{"backgroundColor":[White, Theme Black]}}}
```
The CSS side backs it up with a transition on the class itself, so that non-scrubbed changes still ease:
```css
.section_sorting{overflow:clip;background-color:var(--theme-black);
                 transition:background-color 100ms ease}
.section_wrapper{transition:background-color 400ms ease}
```
So: **wrap the band in `.section_wrapper` (or put the colour on the `section_*` class), give that class a `background-color` transition, then scrub `wf:style.backgroundColor` between two Webflow colour variables over the scroll of the section.** That is the whole dark↔light trick.

**4. The homepage world-map sequence** (`Map While Scrolling`, timeline `t-0f234dae`, 16 actions on a `canvasDuration:1` normalised timeline). Abridged but verbatim in structure:

| position | action name | target | property |
|---|---|---|---|
| 0.01 | `Section Bg Initial` | `.section_sorting` | `backgroundColor: White → Theme Black` |
| 0 | `Initial State` | 16 different class/combo targets | `opacity: 100% → 0%` |
| 0 | `Map Initial` | map class | `x:-0.8%`, `width:100%`, `y:19em` |
| 0.06 | `Title All Fade In` | title combo | `opacity → 100%` |
| 0.07 | `Map Fade In` | map + 3 more | `opacity → 100%` |
| 0.22 | `Full Map Fade Out` / `Default Map Fade In` | two map layers | crossfade |
| 0.24 | `Title 1 Fade Out` | title 1 | `opacity → 0%` |
| 0.33 | `Map Zoom In` | map | `width: → 436.75em`, `y: → 0em` (duration 0.3) |
| 0.56 | `EU Map Fade In` / `Default Map Fade Out` | EU layer / world layer | crossfade |
| 0.59 | `Title EU Fade In` | title 2 | `opacity → 100%` |
| 0.65 / 0.68 / 0.71 / 0.74 | `Loc Pin 1..4 Fade In` | `.location_tag` + `.location_pin` filtered by combo | `opacity → 100%` |

Note the targeting idiom: **`wf:class` + `filterContext.filterBy: ["wf:class", [<combo ids>]]`** — i.e. "every element with class X that also carries combo Y", and `filterBy:["wf:trigger-only",""]` for "only inside the element that triggered". The zoom is done by animating **`width` in `em` (100% → 436.75em)**, which works precisely because of the `vw`-based root font-size.

**5. Not visible through the API (inferred IX2).** The navbar entrance, the dropdown chevron rotation, the button-arrow slide and the hamburger→X morph do **not** appear in the IX3 list, yet their hooks are in the markup and CSS:
```css
/* site custom code → head, verbatim */
<style>
  .navbar { transform: translateY(-165%); }
  .navbar, .navbar * { transform-style: flat !important; }
</style>
```
The bar therefore ships **parked 165 % above the viewport** and is slid in by an interaction on page load/scroll; `.hamburger`, `.hamburger_bar`, `.hamburger_bar.top/.middel/.bottom`, `.dropdown_icon` and `.button_arrow` all exist as classes **with no CSS of their own**, which is the signature of elements whose whole appearance/motion is set by IX. `transform-style: flat !important` is the standard Webflow fix for the 3-D transform flicker that IX adds. Treat these as legacy IX2 that the MCP cannot read.

**6. Other JS, per page.**
- `/news` head: `<!-- Finsweet Attributes --> <script async type="module" src="https://cdn.jsdelivr.net/npm/@finsweet/attributes@2/attributes.js" fs-list></script>`
- `/news` footer: **Swiper 8** (`https://unpkg.com/swiper@8/swiper-bundle.min.js`) initialised on `.swiper1`:
```js
const swiper1 = new Swiper(".swiper1", {
  direction:"horizontal", loop:true, slidesPerView:1.7, slidesPerGroup:1,
  spaceBetween:24, centeredSlides:false,
  mousewheel:{forceToAxis:true}, speed:300,
  autoplay:{delay:4000, disableOnInteraction:false},
  breakpoints:{480:{slidesPerView:1.7,spaceBetween:24},
               768:{slidesPerView:3,spaceBetween:24},
               992:{slidesPerView:4.6,spaceBetween:8}},
  pagination:{el:".swiper-pagination", clickable:true},
  navigation:{nextEl:".button-next", prevEl:".button-prev"}
});
// Pause autoplay on hover
const swiperContainer = document.querySelector(".swiper1");
swiperContainer.addEventListener("mouseenter", () => { swiper1.autoplay.stop(); });
swiperContainer.addEventListener("mouseleave", () => { swiper1.autoplay.start(); });
```
- **Every other page** (`/leistungen`, `/nachhaltigkeit`, `/ueber-uns`, `/standorte`, `/karriere`, `/recycling`, `/kontakt`, `/materialien`) has exactly one head line and an empty footer:
```html
<!-- Flowbase Booster [Count Up] -->
<script src="https://cdn.jsdelivr.net/npm/@flowbase-co/boosters-countup@1.0.0/dist/countup.min.js" type="text/javascript"></script>
```
- Home page has **no** page-level custom code at all.
- `get_registered_scripts` → `[]`; `get_site_scripts` → 404 "Custom code block not found". So nothing is registered through the Apps/scripts API — it is all freeform head/footer code.

### The news CMS filter + load more
Implemented entirely with **Finsweet Attributes v2 (`fs-list`)** — no custom JS at all. Verbatim structure from `/news`:

```
Section .section_blogs
└ … .padding-global → .padding-vertical → .container-large
  └ Block .wrapper_blogs
    ├ Block .blog_upper-wrap
    │ ├ Block .section-title-wrapper.is_blog
    │ │ ├ Block .section-tag → HtmlEmbed .polygon + Block .text-size-regular
    │ │ └ Heading h2 .heading-styles-h2  "Aktuelle Beiträge"
    │ └ FormWrapper .filter_form                 (stock "Email Form", method=get, no action)
    │   ├ FormForm .filter_inner   id="email-form"  fs-list-element="filters"
    │   │ ├ FormRadioWrapper .filter_radio-field  fs-list-activeclass="is-active"
    │   │ │ └ FormRadioInput .filter_radio  id="radio" fs-list-field="category" fs-list-value=""  checked
    │   │ ├ FormRadioWrapper … → radio  fs-list-value="Unternehmen"
    │   │ ├ FormRadioWrapper … → radio  fs-list-value="Recycling & Technologie"
    │   │ ├ FormRadioWrapper … → radio  fs-list-value="Nachhaltigkeit"
    │   │ └ FormRadioWrapper … → radio  fs-list-value="Team & Karriere"
    │   ├ FormSuccessMessage
    │   └ FormErrorMessage
    └ DynamoWrapper .cms   fs-list-filteringclass="is-list-filtering"  fs-list-loadingclass="is-list-loading"
      ├ DynamoList .blog_grid   fs-list-element="list"   fs-list-load="more"
      │ └ DynamoItem → Block .blog_card
      │     └ … Block .date_text  fs-list-field="category"     ← the value the radios match against
      ├ DynamoEmpty → "Keine Beiträge gefunden."
      └ Pagination .pagination
        ├ PaginationPrevious .hide
        └ PaginationNext .button-primary.is-pagination → text + HtmlEmbed .icon-16px
```

So the recipe is: **native Webflow Collection List + native Pagination, then hand the pagination to Finsweet.** `fs-list-load="more"` turns Webflow's "Next" pagination link into a Load-More button (it is styled `.button-primary.is-pagination`), `PaginationPrevious` is hidden with a `.hide` class, the filter form is an untouched stock Email Form whose only job is to hold five radios, and the "Alle Kategorien" reset is simply the radio with `fs-list-value=""` marked `checked`. Filtering matches the **rendered category text** in `.date_text`, not the CMS reference id. The loading/filtering states are pure CSS hooks: `.is-list-loading`, `.is-list-filtering`, `.filter_radio-field.is-active`.

### Custom code summary
- Site head: the `.navbar` parking + `transform-style:flat` style block (quoted above).
- Site footer: Lenis (quoted above).
- Per page: Flowbase Count-Up on 8 pages; Finsweet + Swiper on `/news`.
- In-canvas embeds: `Global Styles` (root em system + form reset), `.border-css` in the navbar, `.gradient-css` in the first dropdown, `.dropdown_icon`, `.button_arrow`, `.hero_image-absolute` (hero visual), `.marquee_css`, `.map_embed`, `.metal-cycle_chart`, `Calendar-embed`, `Code Embed`.
- `.border-css`, verbatim — this is what makes the open dropdown look welded to the bar:
```html
<style>
  /* Open state */
  .navbar:has(.w-nav-button.w--open) .bg-absolute {
    border-bottom-left-radius: 0;
    border-bottom-right-radius: 0;
  }
  /* Close state */
  .navbar:not(:has(.w-nav-button.w--open)) .bg-absolute {
    border-bottom-left-radius: 12px;
    border-bottom-right-radius: 12px;
  }
</style>
```

- `.gradient-css` (inside the first dropdown), verbatim — it repaints every mega-menu card:
```html
<style>
  .list_card {
    background: radial-gradient(
      119.49% 119.11% at 98.05% 98.04%,
      #D1F462 0%,
      #EDEEDE 100%
    );
  }
</style>
```
- `.button_arrow` and `.dropdown_icon` are hand-written 1em inline SVGs using `stroke="currentcolor"` so they inherit the button/link colour through every hover and IX colour change:
```html
<!-- .button_arrow : chevron pointing right -->
<svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%" viewBox="0 0 16 16" fill="none">
  <path d="M6 12L10 8L6 4" stroke="currentcolor" stroke-width="1.5" stroke-linecap="square"/>
</svg>

<!-- .dropdown_icon : chevron pointing up (rotated on open by IX) -->
<svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%" viewBox="0 0 16 16" fill="none">
  <path d="M12 10L8 6L4 10" stroke="currentcolor" stroke-width="1.5" stroke-linecap="square"/>
</svg>
```
Both live in a `1em × 1em` flex box (`.button_arrow{display:flex;width:1em;height:1em;min-width:1em;…}`, `.dropdown_icon{display:flex;width:1em;height:1em;…}`) with **no transform in CSS** — the slide/rotate is added by the interaction, which is why the arrow animation cannot be read from the style API.

### The glass effect
There is no single "glass" class — it is a **recipe repeated ~15 times**: an absolutely-positioned child that carries a dark gradient *and* a `backdrop-filter: blur()`, sitting under the content at `z-index:2`, inside a parent with `border-radius:1em` and a `1px solid hsla(0,0%,100%,.16)` hairline. Verbatim examples:

```css
/* the navbar plate */
.bg-absolute{position:absolute;inset:0;border-radius:.75em;
             background-color:var(--black-60);backdrop-filter:blur(5px)}

/* the standard dark card */
.int_card{position:relative;overflow:clip;width:100%;padding:1.56em;
          border:1px solid hsla(0,0%,100%,.16);border-radius:1em;backdrop-filter:blur(5px)}
.int_card-bg{position:absolute;inset:0;z-index:2;width:100%;height:100%;
             background-image:linear-gradient(161deg,hsla(0,0%,0%,.88),hsla(0,0%,0%,.64));
             backdrop-filter:blur(5px)}

.process_card-bg{position:absolute;inset:0;z-index:2;
                 border:1px solid hsla(0,0%,100%,.16);border-radius:1em;
                 background-image:linear-gradient(132deg,hsla(0,0%,0%,.88) 1%,hsla(0,0%,0%,.64));
                 opacity:.5;backdrop-filter:blur(10px)}

.benefit_card{position:relative;overflow:hidden;width:100%;max-width:25em;padding:.5em;
              border-radius:1em;
              background-image:linear-gradient(161deg,hsla(0,0%,0%,.88),rgba(0,0,0,.64));
              backdrop-filter:blur(10px)}

.blog_card{overflow:clip;width:100%;height:100%;border-radius:1em;
           background-color:hsla(0,0%,0%,.04);backdrop-filter:blur(10px);cursor:pointer}

.card_overlay{position:absolute;inset:0;z-index:2;width:100%;height:100%;
              background-color:hsla(0,0%,0%,.04);opacity:.4;backdrop-filter:blur(10px)}

/* the light "glass" variant used on the light sections */
.position-wrap{position:absolute;left:1.5em;bottom:1.5em;max-width:32em;
               padding:1.5em 5em 1.5em 1.5em;border-radius:1em;
               background-color:hsla(0,0%,100%,.50);backdrop-filter:blur(10px)}
.connect_card.is_gradient{background-image:linear-gradient(180deg,hsla(0,0%,100%,.25),rgba(255,255,255,.25));
                          box-shadow:inset -1px 120px 70px -1px #e9fbb3;backdrop-filter:blur(5px)}

/* the map location pill — glass + lime glow */
.location_tag{position:relative;display:flex;padding:.48em;gap:.75em;
              border:2px solid hsla(0,0%,100%,.15);border-radius:1.56em;
              background-image:linear-gradient(98deg,rgba(0,0,0,.35),rgba(0,0,0,.26));
              box-shadow:0 0 11.2px 0 rgba(209,244,98,.45);backdrop-filter:blur(5px)}
```
Blur is always **5px or 10px** (20px once, on `.figures_card.is_logo`); the fill is always a **161deg / 132deg black gradient from .88 to .64 alpha** on dark sections, or **white at .25–.50 alpha** on light ones.

### Images
Assets are referenced by `assetId` only (no filenames exposed through the API). Patterns observed: hero uses a **triple** Image / HtmlEmbed / BackgroundVideoWrapper stack with `display-none` on the unused two; icons and arrows are **inline-SVG HtmlEmbeds** (`.dropdown_icon`, `.button_arrow`, `.vector_down`, `.arrow_embed`) while decorative vectors are `Image` elements with size-named classes (`.vector_152px`, `.vector-12px`, `.icon-24px`, `.icon-28px`, `.icon_wrap-40px`, `.icon_wrap-56px`); the same chevron asset `6a9ffa4abc5913bdd11e9d15` is reused on all 14 dropdown link cards. CMS images come from the `main-image` ("Hauptbild") field. The category icon is stored as a **RichText SVG embed field** (`icon-embed`), not as an image.

### Forms, links, SEO
- One real form (`/kontakt`) plus the filter "Email Form" on `/news`; `submit_button` has the arrow baked in as a background-image. Field classes: `field_label`, `checkbox_field`, `help-text`, `form_title-wrapper`.
- Links: navbar dropdown items are a mix of `linkType:"page"` (Leistungen, Recycling, Standorte, Über uns, News, Kontakt) and raw anchor URLs (`/nachhaltigkeit#kreislaufwirtschaft`, `#mitgliedschaften`, `#zertifikate`); the Materialien dropdown's five links have **no link target set at all** (see §4).
- SEO: per-page German title/description, OG copied from SEO on every page, no JSON-LD found.

---

## 3. Figma → Webflow mapping observed

| Design concept (Figma) | How it was realised (Webflow) | Changed in the build? |
|---|---|---|
| Section `Design` → Figma Section per page (`Contact page`, `News page`, …) | one Webflow page each, **German slug** | yes — English design names → German slugs |
| Frame named `section` (1920 × ~1080) | `Section .section_<topic>` with `tag=section` + `.padding-global` + `.padding-vertical.is-<topic>` + `.container-large` | yes — sections get *descriptive* class names that exist nowhere in the design |
| 1920 frame width, 1872 content, 24 px margin | `body{font-size:.8333vw}` → `.container-large{max-width:117em}` (=1872px) + `.padding-global{1.5em}` (=24px) | no — exact |
| `site/margin` 24, `site/gutter` 20 | `padding-global` 1.5em; gutters become per-component `gap` in em | partly — gutter token not carried over |
| `section-space/main` 112 / `small` 80 | `.padding-vertical{7em}` (=112px) with medium 5em / small 3.13em / tiny 2.5em | yes — the 80 variant became breakpoint steps instead of a second class |
| Figma text style `H1` (55px) | `.heading-styles-h2` (3.44em = 55px) | **yes — the scale is shifted one step**; `.heading-styles-h1` is 6.69em ≈ 107px, bigger than any Figma token |
| Figma text styles `text-large/main/small` | `.text-size-large / -small / -xsmall` (1.75em / 1em / .88em) | yes — `text-large` 18px in Figma vs 28px in build |
| Colour variables `theme/*`, `brand-*` | Webflow collection **Colors**, 8 variables (`--theme-black`, `--lime-green`, `--white`, `--black-60`, `--white-80`, `--transparent`, `--theme-backgroud-2`, `--black`) | mostly 1:1; `brand-200 #f6fde0` dropped; alpha helpers added |
| `space/*`, `radius/*`, `font-size/*` tokens | **not** created as Webflow variables — flattened into em literals in classes | yes |
| Figma component `bnt prm` / `bnt sec` | Webflow components `Button Primary` / `Button Secondary` with `Text` + `Button Link` props | name normalised, typo dropped |
| Figma `heder-wrap` / `heder inner` / `nav link v1` | `Navbar` component: `.navbar > .navbar_bg > .nav_inner`, `.nav_link`, `.dropdown`, `.dropdown_list-inner` | typo dropped; dropdown built with **native Webflow Dropdown** elements |
| Figma `Link item 3` (dropdown card, 336×240) | `Link .list_card` (`height:15em` = 240px, `padding:1.5em`, `border-radius:.5em`) in a 3-col grid | exact |
| Figma footer frames (3 alternatives on canvas) | one `Footer` component with two text props | simplified |
| Auto-layout row / column | `display:flex` + `gap` in em, or hand-built rows with index combos (`.benefits_row.is--1/.is--2`) | grid used only where the design is a true grid (`.dropdown_list-inner`, `.footer_card-grid`, `.blog_grid`) |
| Figma `spacer` frames (×99) | dropped — replaced by padding/gap, except where a `spacer`-equivalent combo survived (`.title_left-wrap.is-padding{padding-bottom:5.25em}`) | yes |
| `scroll 11 … scroll 33` state frames | one IX3 scroll timeline per sequence, scrubbed 0→1 across the section | yes — the static key-frames become `position` values on a `canvasDuration:1` timeline |
| Figma `world 1` (5 800 vector paths) | an embedded SVG/graphic animated by `width: → 436.75em` + opacity crossfades | yes — zoom driven by em width, not by scale |
| Breakpoint frames 1920 / 834 / 767 / 393 | Webflow `main` / `medium` (≤991) / `small` (≤767) / `tiny` (≤479) | Figma's 834 tablet maps to Webflow `medium`; below 992 the em system freezes at 16px |
| Hover states (not drawn in Figma) | invented in build: `transform:scale(0.95)` + colour inversion, 250 ms ease, on every button | added |
| Dark/light section rhythm (drawn as separate dark and light frames) | IX3 scrubbed `wf:style.backgroundColor` between two colour variables + `transition:background-color` on the class | added mechanism |

---

## 4. Quality notes and gotchas

**Rules that look deliberate (repeat them on a new build):**
1. Every page: `page-wrapper > main-wrapper > [Global Styles] [Navbar] …sections… [Footer]`.
2. Every section: `Section .section_<topic>` (tag `section`) → `.padding-global` (+ a context combo) → `.padding-vertical.is-<topic>` → `.container-large` → `.wrapper_<topic>`.
3. `padding-vertical` is the only vertical rhythm class; per-section deviation is expressed as a combo, never as a new padding class.
4. Everything is `em`, because `body{font-size:.8333vw}`. No `px` except hairlines (`1px`/`2px` borders), a few `96px`/`9999px` radii and the mobile `menu_btn` 40×40.
5. One colour system only: `--theme-black` / `--white` / `--lime-green` + alpha helpers; dark sections set the colour on the `section_*` class itself.
6. Buttons are components with a `Text` prop and a `Button Link` prop; the arrow is always a 1em SVG embed sibling of `.button_text`.
7. Hover = `scale(0.95)` + invert, 250 ms ease. Always.
8. Glass = parent with `1px solid hsla(0,0%,100%,.16)` + `border-radius:1em`, child `*_card-bg` absolute with a 161deg black gradient `.88 → .64` and `backdrop-filter:blur(5|10px)`.
9. Scroll motion = IX3, class-targeted, `scrub:0.8`, `clamp:true`, disabled on small breakpoints via `conditionalPlayback`.
10. Third-party JS is pinned to exact versions from unpkg/jsdelivr and lives in freeform code, never in the Apps/scripts registry.

**Mistakes / inconsistencies found:**
- `showMarkers: true` is set on **all nine** IX3 scroll triggers — debug markers ship to production.
- Duplicate DOM ids in the navbar: the **Materialien** dropdown reuses `id="nachhaltigkeit-dd"` and `id="nachhaltigkeit-menu"` from the Nachhaltigkeit dropdown.
- Five news filter radios all carry `id="radio"`.
- The five links in the **Materialien** mega-menu have **no link target** (no `settings.link`) — they are dead.
- `--theme-backgroud-2` — typo shipped in a variable name.
- `.hamburger_bar.middel` — typo shipped in a class name.
- `.heading-styles-h5` is `1.25em` at `small` but `1.38em` at `tiny` — the mobile size is *larger* than the tablet-portrait size.
- Three published duplicate pages (`/home-safari`, `/home-copy-16-sep`, `/materialien-v2`) are **not** marked draft and will publish and be crawled.
- `Button Secondary Black` component exists with 0 instances.
- `section_responsibilty` (missing 'i'), `section_cta`, `section_intro`, `section_contact`, `section_loc-overview`, `section_partner`, `section_vacancies`, `section_why-drh` carry **no properties at all** — empty classes kept as hooks/leftovers.
- The style list contains **2 000+ classes**, many obviously from other Xerosol projects (`atlas-slider`, `bookflow_divider`, `ai_feature-card`, `amenities-collection`, `about-richtext`, `advanced_ai-list`). Strong evidence the site was **started from the studio's master/starter Webflow project rather than from a blank site**, and the unused classes were never cleaned.
- Figma side: junk layer names typed in a Russian keyboard layout (`дшые шефь`, `фвамasdfva`), a Cyrillic `С` in `Сareer page`, three rival homepage sections still on canvas.

**Special features:** Lenis smooth scroll site-wide (with `data-lenis-stop/-start/-toggle` hooks and a `stop-scroll` class for modals/menus); scroll-scrubbed dark↔light section transitions; a sticky scroll stage (`.sticky_scroll`, `.sticky-wrap`, `.sticky_scroll-tech`, `.ellipse_outer-sticky-wrapper`); Swiper 8 slider on `/news`; Finsweet Attributes v2 list filter + load-more; Flowbase Count-Up counters on 8 pages; a marquee (`.marquee_css`); accordions (`.accordion_toggle`, `.accordion_card`); tabs (`.blog_tabs`, `.blogs-tabs-menu`); a background-video wrapper in the hero (disabled). No RTL, no localisation (single locale, German), no dark-mode toggle (the dark/light alternation is a scroll effect, not a theme).

---

## 5. Verbatim dumps

Omitted from the skill copy; the full dump lives in the analysis workspace.

## 6. Tool quirks

- **Live site unreachable.** `curl https://drh-55fa7e.webflow.io/` → exit 56; the agent proxy logs `connect_rejected … gateway answered 403 to CONNECT` for `drh-55fa7e.webflow.io:443`. `WebFetch` returns `{"error_type":"EGRESS_BLOCKED","domain":"drh-55fa7e.webflow.io"}`. Same policy blocked `furec.webflow.io` and `medicini-plus.webflow.io` in the earlier runs. **Consequence: no published HTML/CSS, therefore no `data-w-id` / IX2 JSON, no `webflow.css`, no rendered DOM.** Anyone repeating this analysis needs either an allow-listed staging domain or the site exported.
- **Figma asset URLs are blocked too.** `get_screenshot` succeeds and returns `https://www.figma.com/api/mcp/asset/<uuid>.png`, but `curl` on it → `connect_rejected` for `www.figma.com:443`. No screenshots could be saved; §1 is text-only. (`enableBase64Response:true` is the only possible workaround and was not used to save context.)
- **`mcp__Figma__get_metadata` with `nodeId=4:2`** (the whole page) returns **3 398 949 characters** and is auto-spilled to a file. Saved locally as `drh/figma_page_meta.xml` and mined with grep/python. Calling it on the page is still the right move — but budget for file-based post-processing.
- **`mcp__Figma__get_variable_defs` fails on a page node** ("You currently have nothing selected"); it needs a *frame* node id (`4531:37750` worked).
- **Webflow `data_style_tool > get_styles {query:"all"}` returns `Too Many Requests` every single time** (4 attempts, spaced out). Workaround that works: `query_styles` with `name_path` substring queries (`["_"]`, `["-"]`, `["a"]`, …) plus `include_properties:true` / `include_breakpoints:[...]`. Each query caps at `limit:200` and reports `truncated:true` — you must union several substring queries.
- **Aggressive 429s in general.** Any action that internally does `GET /v2/pages` (i.e. `get_all_elements`, `query_elements`, `get_settings`, and `get_styles`) fails with `Tool … failed: GET /v2/pages returned 429` if fired back-to-back, and **switching `pageId` between calls makes it much worse**. Practical recipe: one tool call at a time, batch several *queries* inside a single *action* (that costs one page fetch), and do unrelated local work between calls. Re-issuing the identical call after ~30-60 s usually succeeds.
- **`get_site_scripts`** → `404 resource_not_found: "Custom code block not found"` when a site has no registered scripts applied. Not an error to chase — `get_registered_scripts` returning `[]` confirms it.
- **Embed/HTML code is readable**, but not through the element tree: `data_element_tool` shows `"type":"HtmlEmbed"` with no content. Use `data_element_settings_tool > get_settings {type:"all_resolved_settings"}` and read the `"code"` key (works inside component definitions if you pass `scope_component_id`).
- **IX2 is invisible.** `data_interactions_tool > list_interactions` returns IX3 only (9 here). Legacy interactions (navbar reveal, hover/arrow/hamburger animations) cannot be listed, read or counted through MCP — the only way to see them is the published HTML, which is blocked.
- `list_interactions` results include `pageId` **and** a separate `scope.value` page list, and they disagree (e.g. `Map Scroll` has `pageId` = Home but `scope` = `/home-copy`). Trust `scope`.
- Two style-tool responses exceeded the inline token cap and were spilled to `/root/.claude/projects/.../tool-results/*.txt`; parsing them with python/regex was necessary.

---

### Run folder
`analysis/drh/` contains:
- `figma_page_meta.xml` — the full 3.1 MB `get_metadata` XML dump of Figma page `4:2` (the only way to work with it is grep/python, not Read).
- `class_names.txt` / `combo_names.txt` / `combo_selectors.txt` — the extracted Webflow class inventory.
No screenshots: `www.figma.com` asset downloads are blocked by the egress policy (§6).
