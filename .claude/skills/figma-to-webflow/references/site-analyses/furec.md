# FUREC
Figma: https://www.figma.com/design/d7GNBQadYz62QrujZiGZZm/Furec?node-id=0-1  |  Webflow site id: 6a9e93451e229d7be1a2b527 (shortName "furec")  |  Live: https://furec.webflow.io/  |  Analysed: 2026-09-21

> Scope note: the live site could NOT be fetched (org egress policy blocks `furec.webflow.io` and `figma.com/api/mcp/asset/*` — see §6). Everything below therefore comes from the Figma MCP tools and the Webflow Data API, not from rendered HTML/CSS. All class names, CSS values, variable names and element trees are verbatim from those APIs.

---

## 1. Figma file anatomy

### Pages in the file
There is exactly **one** Figma page:

- `0:1` — **"Design"**

No separate "Dev", "Components", "Archive" or "Style guide" page. Everything (design exploration, final desktop, adaptive breakpoints, per-page designs) lives on one enormous canvas, organised by **Figma Sections** (the `<section>` node type), not by pages.

### Sections on the "Design" page (canvas organisation, in canvas x-order)

| node id | Section name (verbatim) | size | what it holds |
|---|---|---|---|
| `1:4298` | `First 3 sections  - 17/07` | 11238×5434 | Early WIP: a `FUREC` frame with the first 3 desktop sections + 4 "scroll 1..4" state frames + a `note` frame. Dated-in-name working draft. |
| `1013:10400` | `Complete homepage` | 27483×11860 | **The build-from source.** Contains the full desktop homepage frame `1013:7706 "FUREC"` (1920×10398) plus ~16 standalone `section` frames that are *scroll states* of the animated sections (`scroll 16`, `scroll 17`, `scroll 18`, and 10 `section` frames showing the "Geschäftsfelder" ellipse at successive scroll positions). |
| `1053:3056` | `Adaptive` | 8505×13470 | Breakpoint frames: `Desctop_homepage` (1920×10542), `Tablet_homepage` (834×11783), `Mobile_homepage` (393×12208), plus `Tablet menu / Active` (834×1194), two `Mobile menu / Active` frames (393×852 and 393×1063), and a loose `section` (834×1240). |
| `1058:4438` | `Performance page` | 4344×13146 | → built as `/leistungen` |
| `1058:6110` | `Recycling page` | 5824×13146 | → built as `/recycling` |
| `1058:7176` | `About us page` | 3944×13146 | → built as `/ueber-uns` |
| `1058:9591` | `Our Values page` | 4244×13146 | → built as `/unsere-werte` |
| `1058:10841` | `Download area page` | 4568×6370 | → built as `/downloads` |
| `3094:4587` | `Our Values page` (duplicate, lower row) | 4244×13146 | second/revised pass at the same page — **two sections share the same name**. |

### Which frames are the "build from" source
`1013:10400 "Complete homepage"` → frame `1013:7706 "FUREC"` (1920×10398) is the desktop master. Adaptive versions are in the `Adaptive` section as three separate full-page frames.

### Frame naming pattern for pages and breakpoints
There is **no** `"Home - Desktop 1440"` style convention. The pattern is:

- Desktop master frame is named after the **brand**: `FUREC` (1920 wide).
- Breakpoint frames are `<Device>_homepage`: **`Desctop_homepage`** (sic, misspelled), **`Tablet_homepage`**, **`Mobile_homepage`**.
- Menu states: `Tablet menu / Active`, `Mobile menu / Active`.
- Inner pages are not frames but whole **Figma Sections** named `"<Topic> page"` in English (`Performance page`, `Recycling page`, `About us page`, `Our Values page`, `Download area page`) even though the site itself is German.

### Breakpoints that exist in Figma
Frame-width census across the whole file:

```
198 × width="1920"   (desktop master + all per-page desktop frames + scroll states)
 15 × width="834"    (tablet — iPad Air portrait)
 42 × width="390"    (mobile components / small blocks)
  7 × width="767"    (an intermediate "mobile landscape" set)
  1 × width="393"    (Mobile_homepage full page)
```
So: **1920 / 834 / 767 / 393(390)** — which lines up almost exactly with Webflow's `main / medium / small / tiny`.

### Section naming inside the homepage frame (verbatim, top to bottom, `1013:7706`)
Every content band is literally called **`section`**. There is no descriptive section naming at all.

| node id | frame name | y | h | first text inside (content identifier) |
|---|---|---|---|---|
| `1013:7707` | `section` | 0 | 1080 | "Rohstofflösungen für Frankreich und Europa" (hero) |
| `1013:7724` | `section` | 1080 | 1080 | "DRH-Verbund" |
| `1013:7796` | `section` | 2160 | 1122 | "Unsere Geschäftsfelder" |
| `1013:7812` | `section` | 3282 | 1080 | "Unsere Geschäftsfelder" |
| `1013:7846` | `scroll 18` | 4362 | 1080 | (animated ellipse state) |
| `1013:8337` | `section` | 5442 | 1080 | "Lösungen für Industrie, Lieferanten und Verwerter" |
| `1013:8365` | `spacer` | 6522 | 112 | — (a literal 112px spacer frame) |
| `1013:8366` | `section` | 6634 | 1026 | "Materialien, die wir bewegen" |
| `1013:8446` | `section` | 7660 | 1038 | "Rohstoffkompetenz mit Blick auf morgen" |
| `1013:8461` | `section` | 8698 | 620 | "FUREC" (CTA band) |
| `1013:8480` | `heder-wrap` | 0 | 120 | "Kontakt aufnehmen" — the **navbar**, pinned at y=0 (sic: "heder", misspelled) |
| `1013:8505` | `footer` | 9318 | 1080 | "Sie haben Material?" |

Recurring inner frame names: `top`, `row`, `interactive`, `row steps`, `group`, `tag`, `list`, `spacer`, `bnt prm`, `bnt sec`, `bg wrap`, `heder-wrap`, `footer`. Note the misspellings that recur verbatim: **`heder-wrap`**, **`bnt prm`/`bnt sec`**, **`collums`/`collums def`**, **`Desctop_homepage`**.

### Design tokens (Figma variables, verbatim names → values)
Retrieved with `get_variable_defs` on `1013:7706`. Names use `group/name` slash notation.

**Colour**
| variable | value |
|---|---|
| `theme/background` | `#191919` |
| `theme/text` | `#ffffff` |
| `theme/text secondary` | `#191919` |
| `theme/accent` | `#d1f462` |
| `theme/border` | `#19191933` (black @ 20%) |
| `button/primary/background` | `#d1f462` |
| `button/primary/text` | `#191919` |
| `button/secondary/text` | `#ffffff` |

**Typography — font-size tokens (px, at the 1920 frame)**
| token | px |
|---|---|
| `font-size/display` | 69 |
| `font-size/h1` | 55 |
| `font-size/h2` | 44 |
| `font-size/h3` | 35 |
| `font-size/h4` | 28 |
| `font-size/h5` | 23 |
| `font-size/text-large` | 18 |
| `font-size/text-main` | 16 |
| `font-size/text-small` | 14 |
| `font-size/text-extra-small` | 12 |

**Text styles (composed)**
| style | family | size token | weight | line-height | letter-spacing |
|---|---|---|---|---|---|
| `Display` | primary-family | `font-size/display` 69 | 400 | 1.05 | −3 |
| `H1` | primary-family | 55 | 400 | 1.0 | −3 |
| `H2` | primary-family | 44 | 400 | 1.1 | −3 |
| `H3` | primary-family | 35 | 400 | 1.2 | −2 |
| `H4` | primary-family | 28 | 400 | 1.15 | −1 |
| `H5` | primary-family | 23 | 400 | 1.30 | 0 |
| `text-large` | primary-family | 18 | 400 | 1.35 | 0 |
| `text-main` | primary-family | 16 | 400 | 1.50 | 0 |
| `text-small` | primary-family | 14 | 400 | 1.40 | 0 |
| `text-extra-small` | primary-family | 12 | 400 | 1.50 | −1 |

**Spacing / geometry**
| token | px |
|---|---|
| `space/2` | 4 |
| `space/3` | 8 |
| `space/4` | 12 |
| `space/5` | 16 |
| `space/6` | 24 |
| `space/7` | 32 |
| `space/9` | 48 |
| `space/10` | 64 |
| `site/margin` | 48 |
| `site/gutter` | 20 |
| `section-space/small` | 80 |
| `section-space/main` | 112 |
| `section-space/large` | 160 |
| `radius/main` | 16 |
| `radius/round` | 99999 |
| `border-width/main` | 1.5 |

(Note the gap: `space/8` is missing from the returned set.)

**Fonts**
- `primary-family` = **Geist**; weights present as `primary-regular` = "Regular", `primary-medium` = "Medium".
- `font/primary-family` = **Manrope**, `font/primary-bold` = "Semi Bold" — a *second, unused* family token left behind from an earlier kit. Nothing on the built site uses Manrope.

### Components in Figma (verbatim names)
Only **7 real component definitions** exist (`<symbol>` nodes) — and none of them is a nav, footer or button:

```
Frame 45 wcu, card Geschäftsfelder  v1, card Loop  v1, collums, collums def, stems v1, type v1
```

Most-instanced symbols/instances across the file:
```
208 × collums def      121 × nav link v1        72 × Link item 1      56 × Impressum desctop v1
 39 × stems v1          38 × card Geschäftsfelder  v1   27 × col title   21 × icon-furec / recycle
 19 × icon-furec / factory   18 × icon-furec / search-check   14 × icon-furec / package-open
 14 × Frame 45 wcu      11 × card Loop  v1       10 × icon / package, icon / cart,
 Interface/recycle, Interface/funnel, spacer    9 × icon-furec / iteration-ccw, bg wrap
  8 × type v1, collums    6 × icon-furec / scan-search    5 × Interface/boxes
  3 × icon-furec / route, icon-furec / leaf, Interface/package-check, Interface/grid-2x2-check
```
Icon naming uses three parallel libraries: **`icon-furec / <lucide-name>`**, **`Interface/<lucide-name>`**, and **`icon / <name>`** — all Lucide icon names (`recycle`, `factory`, `search-check`, `package-open`, `iteration-ccw`, `scan-search`, `boxes`, `route`, `leaf`, `funnel`, `workflow`, `cog`, `blend`, `clipboard-check`, `grid-2x2-check`). Variants: the `v1` suffix (`card Geschäftsfelder  v1`, `card Loop  v1`, `stems v1`, `type v1`, `nav link v1`) is the only versioning marker; **no Figma variant properties** were observed.

There is also at least one garbage layer name: `дшые шефь` (11 instances) — Cyrillic keyboard-layout typo of "list item".

### Auto-layout usage
**Mixed, and mostly NOT used at section level.** `get_design_context` on the hero section `1013:7707` returns:

```jsx
<div className="bg-[var(--theme/background,#191919)] relative size-full" data-name="section">
  <div className="absolute ... left-[-57.94px] top-[-38.82px] w-[2035.871px]" />     {/* bg photo, rotated -0.5deg */}
  <div className="absolute bg-gradient-to-b bottom-0 from-[rgba(0,0,0,0)] h-[385px] left-0 opacity-90 right-0 to-black" />
  <p className="absolute ... left-[48px] top-[778px] ... text-[length:var(--font-size/h5,23px)]">Rohstofflösungen …</p>
  <p className="absolute ... left-[48px] top-[828px] text-[120px] tracking-[-3.6px] w-[756px] leading-[0.83]">The future of recycling</p>
  <p className="absolute ... left-[1135px] top-[812px] w-[756px]">FUREC verbindet …</p>
  <div className="absolute content-stretch flex gap-[12px] items-start left-[1135px] top-[972px]" data-name="group">
    <div className="bg-[var(--button/primary/background,#d1f462)] content-stretch flex gap-[8px] items-center
                    justify-center px-[24px] py-[var(--space/5,16px)] rounded-[var(--radius/round,99999px)]" data-name="bnt prm">…</div>
    <div className="border-[length:var(--border-width/main,1.5px)] border-[var(--button/secondary/text,white)]
                    border-solid content-stretch flex gap-[8px] items-center px-[24px] py-[16px]
                    rounded-[var(--radius/round,99999px)]" data-name="bnt sec">…</div>
  </div>
</div>
```

So: **section frames are absolute-positioned canvases**; auto-layout is used only inside small atoms (button rows, tag rows, card interiors). Typical desktop numbers visible in the metadata:

- Content starts at `x = 48` and inner content frames are `1824` wide → **48px side margin, 1824px content width** on a 1920 frame (= `site/margin` 48).
- Section top offset `y = 112` inside a 1080-tall section → `section-space/main` 112.
- Button: `px-[24px] py-[16px]`, `gap 8px`, radius 99999 → 56px tall (matches `bnt prm` frame height 56).
- Button row gap: 12px (`space/4`).
- Section heights are almost always exactly **1080** (a few 1026/1038/1122/620) — the designer worked in full-viewport slabs.

### Asset types
- **Photos**: large raster `rounded-rectangle` fills (hero `Rectangle 5517` 2035×1157 rotated −0.5°, machinery shots, world map). Exported and uploaded to Webflow as **`.webp`**.
- **SVG icons**: 16/24/28/36px `Frame` nodes → in Webflow they are inlined as `HtmlEmbed` (47 embeds on the homepage alone).
- **Illustrations**: the dotted-ellipse "Geschäftsfelder" diagram, the `stems v1` bar-chart strip, `collums def` tick strips — all vector, rebuilt in Webflow with divs (`small_verticle-bar` ×60+, `ellipse_circle_wrapp.is--1…is--5`).
- **Logos**: FUREC wordmark, DRH logo, membership/certification logos.

---

## 2. Webflow site anatomy

### Pages (16 total)

| Title | Slug / published path | Draft? | Notes |
|---|---|---|---|
| Home | `/` | no | homepage, page id `6a9e93451e229d7be1a2b509` |
| Über uns | `/ueber-uns` | no | ← Figma "About us page" |
| Leistungen | `/leistungen` | no | ← Figma "Performance page" |
| Recycling | `/recycling` | no | ← Figma "Recycling page" |
| Unsere Werte | `/unsere-werte` | no | ← Figma "Our Values page" |
| Download Bereich | `/downloads` | no | ← Figma "Download area page" |
| Kontakt | `/kontakt` | no | form page |
| Karriere | `/karriere` | no | |
| Home 21 | `/home-21` | no | **stray duplicate of Home created 2026-09-21**, still `shouldPublish: true` |
| Components | `/components` | **draft** | scratch page for building components |
| Inner Page | `/inner-page` | **draft** | starter-template leftover |
| Location | `/location` | **draft** | starter-template leftover |
| Sustainability | `/sustainability` | **draft** | starter-template leftover (English) |
| News | `/news` | **draft** | starter-template leftover |
| Blogs Template | `/blogs` (`detail_blogs`) | no | CMS collection page |
| Blog Categories Template | `/blog-categories` (`detail_blog-categories`) | no | CMS collection page |

No 404 page, no password page, **no style-guide page** in the list. The nearest thing to a style guide is the draft **`Components`** page.

> **Mismatch with the brief**: the brief listed `/performance`, `/about-us`, `/our-values`. The real slugs are German (`/leistungen`, `/ueber-uns`, `/unsere-werte`). The English names survive only as the *Figma section* names.

### Class naming convention
**A custom system that borrows Client-First's skeleton and typography layer, then goes fully bespoke for everything else.** Evidence:

- Client-First core classes present *verbatim*: `page-wrapper`, `main-wrapper`, `container-large`, `padding-global`, `padding-vertical`, `text-size-tiny/xsmall/small/regular/medium/large`, `text-color-white`, `hide`.
- But: **no** `section_` → CF uses `section_<name>` with an underscore, and this site does too (`section_hero`, `section_cta`) — that part *is* CF.
- **Divergences from Client-First**: CF's `padding-section-small/medium/large` are absent, replaced by `padding-vertical` + a per-section combo (`padding-vertical.is_home-hero`). CF's `heading-style-h2` is spelled **`heading-styles-h2`** (plural "styles"). CF's `max-width-large` etc. are replaced by ad-hoc `max-wd-710px`, `max-wd-1006px`. CF's `spacer-*` utilities are replaced by `flex-v-gap-8px`, `gap_16px`, `margin_top-36px`. CF forbids combo-class stacking beyond 2; this site routinely stacks 3 (`.text-size-regular.is_ellipse-absolute.is--2._16px_mbl`).
- **Not Relume** (no `layout###_component`, no `rl-` prefixes).

**Counts:** 1015 styles total — 585 global classes, 424 combo classes, 6 tag styles (`body, p, h1, h2, h3, h4`).

**30+ verbatim examples, grouped**

*Layout / structure*
`page-wrapper`, `main-wrapper`, `container-large`, `padding-global`, `padding-vertical`, `outer_wrapper`, `inner_wrapper`, `inner_wrapp`, `center_wrapp`, `position-wrap`, `grid`, `grid-2x1`, `grid_3x1`, `grid_flex`, `grid_wrapper`, `flex-wrapper`, `flex_v`, `sticky-wrap`

*Section shells (one per design section, `section_<topic>`)*
`section_hero`, `section_drh-network`, `section_recycling-solutions`, `section_areas`, `section_raw-materials`, `section_marketing`, `section_industry`, `section_materials-move`, `section_expertise`, `section_cta`, `section_footer`, `section_contact`, `section_figures`, `section_timeline`, `section_locations`, `section_vacancies`, `section_memberships`, `section_why-drh` (…~60 of these)

*Section inner wrappers (mirrors the section list, `wrapper_<topic>`)*
`wrapper_hero`, `wrapper_drh-network`, `wrapper_solutions`, `wrapper_areas`, `wrapper_cta`, `wrapper_footer`, `wrapper_contact`, `wrapper_timeline`, `wrapper_material`, `wrapper_move`, `wrapper_memberships`, `wrapper_mobile-content` (…~40)

*Typography*
`heading-styles-h1`, `heading-styles-h2`, `heading-styles-h3`, `heading-styles-h4`, `heading-styles-h5`, `text-size-tiny`, `text-size-xsmall`, `text-size-small`, `text-size-regular`, `text-size-medium`, `text-size-xmedium`, `text-size-large`, `text-color-white`, `text-color-white-80%`, `text-color-theme-black`, `text-color-primary`, `weight-medium`, `quote-text`, `quote_style`, `help-text`, `link-text`

*Buttons*
`button-primary`, `button-secondary`, `button-secondary-black`, `button_text`, `button_arrow`, `button-group`, `button_center-wrapper`, `button_active-wrapper`, `submit_button`, `menu_btn`

*Components / cards*
`areas_card`, `adv_card`, `benefit_card`, `blog_card`, `business_card`, `connect_card`, `contact_card`, `contribution_card`, `cta_card`, `feature_card`, `figures_card`, `footer_card`, `loc_card`, `location_card`, `material_card`, `offer_card`, `quality_card`, `raw_material-card`, `reference_card`, `request_card`, `safety_card`, `service_card`, `step_card`, `timeline_card`, `vaccancy_card` (sic), `why_card`, `list_card`, `int_card`

*Navbar*
`navbar`, `navbar_bg`, `nav_inner`, `nav_brand`, `nav_menu`, `nav_menu-inner`, `nav_link`, `nav_right-wrap`, `dropdown`, `dropdown-toggle`, `dropdown_icon`, `dropdown_list`, `dropdown_list-inner`, `toggle_text`, `hamburger`, `hamburger_bar`, `mobile_active`, `desktop_active`, `bg-absolute`, `border-css`

*Utility / sizing*
`icon-16px`, `icon-17px`, `icon-18px`, `icon-20px`, `icon-24px`, `icon-28px`, `icon-36px`, `icon-40px`, `icon-48px`, `icon-112px`, `icon-160px`, `icon-100%`, `icon_wrap-33px`, `icon_wrap-40px`, `icon_wrap-56px`, `icon_wrap-72px`, `icon_wrap-84px`, `flex-v-gap-4px`, `flex-v-gap-8px`, `flex-v-gap-12px`, `flex-v-gap-16px`, `flex-v-gap-24px`, `flex-h-gap-5px`, `gap_4px`, `gap_8px`, `gap_16px`, `margin_top-12px`, `margin_top-36px`, `max-wd-190`, `max-wd-284`, `max-wd-360px`, `max-wd-400px`, `max-wd-530px`, `max-wd-710px`, `max-wd-800px`, `max-wd-1006px`, `max-width-580px`, `vector-2px`, `vector-12px`, `vector-143px`, `vector-306px`, `vector-414px`, `vector-460px`, `vector-719px`, `width-9em`, `100vh`, `hide`, `noise`, `blur`, `square-6px`

*Modifier combos (the heart of the system)*
Two families:
1. **`is--<n>` positional**: `is--1 is--2 is--3 is--4 is--5 is--6 is--7` (e.g. `.ellipse_circle_wrapp.is--1`, `.benefit_column.is--2`)
2. **`is_<context>` semantic**: `is_hero`, `is_home-hero`, `is_inner`, `is_inner-2`, `is_industry`, `is_career`, `is_contact`, `is_concepts`, `is_residual`, `is_location`, `is_expertise`, `is_cta`, `is_dark`, `is_area`, `is_sorting`, `is_recycling`, `is_timeline`, `is_quality`, `is_reference`, `is_performance`, `is_sec-materials`, `is_drh-network`, `is_marketing`, `is_value`, `is_overview`, `is_secondary`, `is_about`, `is_gap-90px`, `is_gap-110px`, `is-absolute`, `is-black-border`, `is-points`, `is-metal-cycle`, `is-16px`, `is-pagination`
3. **numeric index combos**: `.areas_card.01`, `.areas_card.02`, `.areas_card.03`, `.timeline_card.is-gap-240px.01` … `.05` (Webflow renders these as `._01`)
4. **px-override combos** (a very distinctive habit): `.heading-styles-h1.is-120px`, `.is-80px`, `.is-72px`, `.heading-styles-h2.is-69px`, `.is-60px`, `.text-size-small._14px-mbl`, `._18px-mbl`, `._13px-mbl`, `.text-size-regular._16px_mbl`, `.text-size-regular.text-color-white.opacity-60._14px_tab`, `.business_grid._1240px`, `.material_card-vector.is-385px`
5. **state / visibility**: `desktop-active`, `tab-active`, `mobile_active`, `desktop_active`, `display-none`, `hide`, `hide-mbl`, `mbl-no-border`, `no-border`, `opacity-0`, `opacity-70%`, `opacity-80%`, `success`, `relative`, `full`, `height-100`, `height_100%`

### DOM skeleton of the homepage — body → hero
```
Body
  Block.page-wrapper                                   <div>
    ComponentInstance "Global Styles"                  (HtmlEmbed with the global <style> block)
    Block.main-wrapper                                 <div>
      ComponentInstance "Navbar"
      Section.section_hero.is_home                     <section>
        Block.padding-global.is_hero                   <div>
          Block.padding-vertical.is_home-hero           <div>
            Block.container-large.is_hero               <div>
              Block.wrapper_hero                        <div>
                Block.section-title-wrapper.is_hero     <div>
                  Block.title_left-wrap.is_home-hero    <div>
                    Paragraph.text-size-xmedium.is_home-hero
                    Heading<h1>.heading-styles-h1.is-72px
                  Block.title_right-wrap.is_home-hero   <div>
                    Paragraph.text-size-xmedium.is_tab-18px
                    Block.button-group
                      ComponentInstance "Button Primary"
                      ComponentInstance "Button Secondary"
        Image.hero_image-absolute.display-none          (alt "factory whole view")
        HtmlEmbed.hero_image-absolute                   (responsive <picture>/<video> embed)
        BackgroundVideoWrapper.hero_image-absolute.display-none
        Block.hero_gradient
```
The recurring 5-level shell is **`section_x > padding-global > padding-vertical > container-large > wrapper_x`**. This is applied with near-total consistency across all 9 homepage sections and inside the CTA and Footer components.

### A representative card-grid section (`section_recycling-solutions`)
```
Section.section_recycling-solutions                    <section>
  Block.padding-global
    Block.padding-vertical.is-future
      Block.container-large
        Block.wrapper_solutions
          Block.section-title-wrapper.is_recycling
            ComponentInstance "Section Tag"
            Heading<h2>.heading-styles-h3
          Block.benefits_grid                           (display:grid, 3 cols)
            Block.benefit_column
              Block.adv_card.is_gap-110px
                Block.feature_icon-wrap
                  HtmlEmbed.icon-24px                   (inline SVG)
                Block.flex-v-gap-4px
                  Paragraph.text-size-small
                  Paragraph.text-size-small.opacity-70%
            Block.benefit_column.is--2   → same interior
            Block.benefit_column.is--3   → same interior
          Block.max-wd-710px
            Paragraph.text-size-xmedium.text-color-theme-black
```
Second example (`section_areas`, the sticky-scroll one) shows the positional-combo idiom:
```
Section.section_areas
  Block.areas_sticky-wrapper
    Block.padding-global.is_marketing
      Block.padding-vertical.is_area
        Block.container-large.height_100%
          Block.wrapper_areas
            Block.section-title-wrapper.is_areas
              Heading<h2>.heading-styles-h1.is-120px      ← h2 tag, h1 style class
            Block.cards_outer-wrapper
              Block.areas_card.01 / .02 / .03
                Block.icon_wrap-56px > HtmlEmbed.icon-28px
                Block.area_info-wrapper
                  Block.text-size-small.text-color-white
                  Paragraph.text-size-small.is_area
            Block.material_ellipse-wrapper
              Block.inner_dotted-ellipse
                Block.ellipse.small
                Block.ellipse_center-text.is_flex
                Block.ellipse_circle_wrapp.is--1 … .is--5
                  Block.text-size-regular.is_ellipse-absolute[.is--n]
                  Block.icon_wrap-84px > HtmlEmbed.icon-36px
```

### Footer (Webflow component "Footer", 12 instances)
```
Section.section_footer                                  <section>
  Block.padding-global.relative
    Block.padding-vertical.is-connects
      Block.container-large
        Block.wrapper_footer
          Block.footer_card-grid                        (grid 1fr 1fr → 1fr at ≤991)
            Block.footer_card                           (prop "Title 1" = "Sie haben Material?")
            Block.footer_card.is_dark                   (prop "Title 2" = "Sie brauchen Rohstoffe?")
          Block.footer_middle-wrap
            Block.footer_middle-left
            Block.footer_middle-right
          Block.footer_lower-wrap
            Block.footer_lower-left
            Block.divider-vertical
            Block.footer_lower-right
          Block.footer_legal
            Block.text-size-small.text-color-white.14px-mbl
            Block.legal_links-wrap
```

### Navbar
Uses Webflow's **native Navbar element** (`NavbarWrapper` / `NavbarBrand` / `NavbarMenu` / `NavbarLink` / `NavbarButton`) plus two native **Dropdown** widgets, wrapped in a custom component called `Navbar` (12 instances = one per page).
```
NavbarWrapper.navbar
  Block.navbar_bg
    Block.nav_inner
      NavbarBrand.nav_brand > Image.image            (assetId 6aad1159bacdf031b33a5b93)
      NavbarMenu.nav_menu
        Block.nav_menu-inner
          NavbarLink.nav_link                        "Startseite"
          DropdownWrapper.dropdown                   "Unternehmen"
            DropdownToggle.dropdown-toggle
              Block.toggle_text
              HtmlEmbed.dropdown_icon                (inline SVG chevron)
            DropdownList.dropdown_list
              Block.dropdown_list-inner
                Link.list_card → /leistungen   > Block.text-size-small.14px-mbl + Image.vector_152px (alt "dotted brand logo")
                Link.list_card → /recycling
                Link.list_card → /ueber-uns
                HtmlEmbed.gradient-css
          DropdownWrapper.dropdown                   "Nachhaltigkeit"
            DropdownList.dropdown_list.is_2
              Link.list_card → /unsere-werte
              Link.list_card → /downloads
              Link.list_card → /karriere
              HtmlEmbed.gradient-css
          NavbarLink.nav_link                        "Kontakt"
          Block.mobile_active > ComponentInstance "Button Primary" (→ /kontakt)
      Block.nav_right-wrap
        Block.desktop_active > ComponentInstance "Button Primary" (→ /kontakt)
        NavbarButton.menu_btn
          Block.hamburger
            Block.hamburger_bar.top
            Block.hamburger_bar.middel      ← sic, "middel"
            Block.hamburger_bar.bottom
        HtmlEmbed.border-css
  Block.bg-absolute
```

**Mobile menu** is Webflow's built-in `w-nav-button` / `w--open`, restyled: at `medium` (≤991) `.nav_menu-inner` becomes a full-width column with `max-height:85vh`, `overflow:auto`, `background-color: rgba(0,0,0,0.6)`, `backdrop-filter: blur(5px)`, `border-bottom-radius 0.75em`. The CTA button is duplicated: `.desktop_active` copy in `nav_right-wrap`, `.mobile_active` copy inside the menu.

Navbar CSS (verbatim):
```css
.navbar { position:fixed; left:0%; top:1.5em; right:0%; bottom:auto; width:100%;
          max-width:74.69em; margin-right:auto; margin-left:auto;
          border-radius:0.75em; background-color:var(--transparent); }
  @medium (≤991) { width:auto; max-width:none; margin-right:1.5em; margin-left:1.5em; }
  @small  (≤767) { top:1.25em; margin-right:1.25em; margin-left:1.25em; }
  @tiny   (≤479) { margin-right:1em; margin-left:1em; }
.navbar_bg { position:relative; padding:1em 1.5em; }
  @small { padding-right:1em; padding-left:1em; }  @tiny { padding-top:.8em; padding-bottom:.8em; }
.nav_inner { position:relative; z-index:5; display:flex; width:100%;
             justify-content:space-between; align-items:center; gap:4em; }
.nav_menu-inner { display:flex; justify-content:center; align-items:center; gap:1.5em; }
.nav_link { padding:0; transition:all 200ms ease; color:var(--white); font-size:1em; line-height:150%; }
.nav_link:hover { color:var(--lime-green); }
  @medium { .nav_link { margin:0; font-size:1.13em; } }
```
Plus a **site-wide head `<style>`** that hides the navbar off-canvas so an interaction can slide it in:
```css
.navbar { transform: translateY(-165%); }
.navbar, .navbar * { transform-style: flat !important; }
```
and an embed inside the navbar using `:has()`:
```css
.navbar:has(.w-nav-button.w--open) .bg-absolute { border-bottom-left-radius:0; border-bottom-right-radius:0; }
.navbar:not(:has(.w-nav-button.w--open)) .bg-absolute { border-bottom-left-radius:12px; border-bottom-right-radius:12px; }
```

### Buttons
Two classes + one dark variant, both built as Webflow **components** with `Variant`, `Link` and `Text` props.
```css
.button-primary {
  display:flex; padding:1em 1.5em; justify-content:center; align-items:center; gap:.5em;
  border-radius:96px; background-color:var(--lime-green);
  transform:none /* scale(0.95) */; transition:all 250ms ease;
  color:var(--theme-black); text-decoration:none; }
.button-primary:hover { background-color:var(--theme-black); transform:scale(0.95); color:var(--white); }
  @small { padding-top:.88em; padding-bottom:.88em; }
  @tiny  { width:100%; }

.button-secondary {
  display:flex; padding:1em 1.5em; justify-content:center; align-items:center; gap:.5em;
  border:1px solid var(--white); border-radius:9999px;
  background-color:var(--transparent); transition:all 250ms ease; color:var(--white); }
.button-secondary:hover { background-color:var(--white); transform:scale(0.95); color:var(--theme-black); }
  @small { padding-top:.88em; padding-bottom:.88em; }  @tiny { width:100%; }

.button-secondary-black {
  display:flex; padding:.63em .88em .63em 1.25em; gap:.5em; border-radius:9999px;
  background-color:var(--theme-black); color:var(--white); }
```
Internal structure: `Link.button-primary > Block.button_text + HtmlEmbed.button_arrow`, where `button_arrow` is a literal inline SVG chevron:
```html
<svg xmlns="http://www.w3.org/2000/svg" width="100%" height="100%" viewBox="0 0 16 16" fill="none">
  <path d="M6 12L10 8L6 4" stroke="currentcolor" stroke-width="1.5" stroke-linecap="square"/>
</svg>
```
Combo variants seen: `.button-primary.is-pagination`, `.button-secondary.is-black-border`, `.button_text.small`.
(Note the inconsistency: primary uses `border-radius:96px`, secondary uses `9999px`.)

### Variables in Webflow
**One collection: `Colors`** (id `collection-57d66751-8b04-cfde-cb65-5fb241cda9a9`), one mode (`base`). **No size, spacing, radius or font variables exist in Webflow** — all of those live only in Figma and were hard-coded as `em` values.

| Webflow variable | cssName | value | ← Figma token |
|---|---|---|---|
| `White` | `--white` | `white` | `theme/text` `#ffffff` |
| `Black` | `--black` | `hsla(0,0%,0%,1)` | — |
| `Lime Green` | `--lime-green` | `#d1f462` | `theme/accent`, `button/primary/background` |
| `Theme Black` | `--theme-black` | `#191919` | `theme/background`, `theme/text secondary`, `button/primary/text` |
| `Black 60%` | `--black-60` | `hsla(0,0%,0%,0.6)` | — |
| `White 80%` | `--white-80` | `hsla(0,0%,100%,0.8)` | — |
| `Transparent` | `--transparent` | `hsla(0,0%,0%,0)` | — |
| `Theme Backgroud 2` (sic) | `--theme-backgroud-2` | `#edeede` | not in the Figma set |
| `Whitesmoke` | `--whitesmoke` | `whitesmoke` | — |
| `Cyan Green` | `--cyan-green` | `#bdc7ae` | not in the Figma set |

Figma's `theme/border` `#19191933` has **no** Webflow variable — border colours are hand-written (`hsla(0, 0.00%, 100.00%, 0.16)` on `.areas_card`).

### Typography — the key mechanism
The site uses a **fluid `vw`-based root font size** injected through the `Global Styles` component (an `HtmlEmbed` at the top of `page-wrapper` on every page). This is the single most important thing to copy:

```html
<style>
body { font-size: 0.8333333333333334vw; }
/* Max Font Size */
@media screen and (min-width:1920px) { body { font-size: 16px; } }
/* Container Max Width */
.container { max-width: 1920px; }
/* Min Font Size */
@media screen and (max-width:991px) { body { font-size: 16px; } }
body { -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; font-smooth: always; }
</style>
<style>
/*Reset apple form styles*/
input, textarea, select { -webkit-appearance:none; -moz-appearance:none; appearance:none;
                          border-radius:0; background-image:none; }
* { font-variant-ligatures: none; }
.w-select { -webkit-appearance:none; -moz-appearance:none; }
select { -webkit-appearance:menulist-button; color:black; }
</style>
```
`0.83333vw` = `16 / 1920` → at a 1920px viewport 1em = 16px, i.e. **1em in Webflow == 1px×16 in the Figma 1920 frame**, and the whole desktop layout scales linearly from 992px to 1920px. *That is why every single spacing/size value in this site is written in `em`.*

Webflow tag defaults are untouched Webflow defaults and are always overridden by a class:
```css
body { font-family:Geist; color:#333; font-size:14px; line-height:20px; font-weight:400; }
h2   { margin:0; font-size:32px; line-height:36px; font-weight:700; }   /* never visible — always classed */
```

**Heading classes (desktop px = em × 16):**
| class | main (≥992) | medium (≤991) | small (≤767) | tiny (≤479) | colour |
|---|---|---|---|---|---|
| `heading-styles-h1` | `6.69em` (107px), lh 105%, ls −3.6px, w400 | `4em` | `3.5em`, ls −2.7px | `2.5em`, ls −2px | `--white` |
| `heading-styles-h2` | `3.44em` (55px), lh 100%, ls −1.45px | `2.5em`, lh 120%, ls −0.8px | `1.88em` | `1.38em`, ls −0.4px | `--theme-black` |
| `heading-styles-h3` | `2.75em` (44px), lh 110%, ls −1.32px | `2.5em`, ls −0.8px | `1.88em` | `1.38em`, ls −0.44px | `--theme-black` |
| `heading-styles-h4` | `2.19em` (35px), lh 120%, ls −0.02em | — | `1.75em` | `1.5em` | `--white` |
| `heading-styles-h5` | `1.75em` (28px), lh 115%, w600, ls −0.02em | — | `1.5em` | `1.25em` | `--white` |

**Body text classes:**
| class | main | medium | small | tiny | colour |
|---|---|---|---|---|---|
| `text-size-large` | `1.75em` (28px), lh 115%, ls −0.01em | `1.5em` | `1.25em` | `1.13em` | `--white` |
| `text-size-medium` | `1.44em` (23px), lh 130%, ls −0.01em | `1.25em` | `1.13em` | — | `--theme-black` |
| `text-size-xmedium` | `1.44em` (23px), lh 130%, w400 | `1.25em` | — | `1.13em` | `--white` |
| `text-size-regular` | `1.13em` (18px), lh 135% | — | `1em` | — | `--theme-black` |
| `text-size-small` | `1em` (16px), lh 150% | — | — | — | `--theme-black` |
| `text-size-xsmall` | `0.88em` (14px), lh 150% | — | — | — | `--theme-black` |
| `text-size-tiny` | `0.75em` (12px), lh 150%, ls −0.01em | — | — | — | inherit |

Per-instance px overrides (combo): `.heading-styles-h1.is-120px { font-size:7.5em }` → `@medium 4.5em / ls −3px`, `@small 4em / ls −2px`, `@tiny 3em / ls −1.6px`. Same idiom for `.is-80px`, `.is-72px`, `.is-69px`, `.is-60px`.

### Spacing system
```css
.container-large { width:100%; max-width:117em; margin-right:auto; margin-left:auto; }   /* 1872px @16 */
.padding-global  { padding-right:1.5em; padding-left:1.5em; }                            /* 24px */
  @small (≤767)  { padding-right:1.25em; padding-left:1.25em; }                          /* 20px */
  @tiny  (≤479)  { padding-right:1em;    padding-left:1em; }                             /* 16px */
.padding-vertical { padding-top:7em; padding-bottom:7em; }                               /* 112px = section-space/main */
  @medium (≤991) { padding-top:5em;    padding-bottom:5em; }                             /* 80px  = section-space/small */
  @small  (≤767) { padding-top:3.13em; padding-bottom:3.13em; }                          /* 50px */
  @tiny   (≤479) { padding-top:2.5em;  padding-bottom:2.5em; }                           /* 40px */
```
Per-section overrides ride on combos:
```css
.padding-global.is_hero      { position:relative; z-index:5; width:100%; }  @tiny { padding:0 1em; }
.padding-global.is_marketing, .padding-global.relative, .padding-global.desktop-0   /* others */
.padding-vertical.is_home-hero { width:100%; height:100%; padding-top:15em; padding-bottom:5em; }
                               @medium { padding-top:10em; }  @small { padding-top:8em; padding-bottom:3em; }
.padding-vertical.is-future    { /* nothing at main */ }  @medium { 4em/4em }  @tiny { 2.5em/2.5em }
.padding-vertical.is_cta       { padding-top:3em; padding-bottom:3em; }
.container-large.is_hero       { height:100%; }
.container-large.height_100    { … }   .container-large.full { … }
```
There are **47 distinct `padding-vertical.is_*` combos** on this site — one per named section band (`is_area`, `is_blog`, `is_concepts`, `is_contact`, `is_demand`, `is_flow`, `is_industry`, `is_inner-hero`, `is_international`, `is_linkedin`, `is_loc-cards`, `is_offer`, `is_overview`, `is_partner`, `is_quality`, `is_reach`, `is_sorting`, `is_timeline`, `is_value`, `is_why-drh`, `is-benefits`, `is-connects`, `is-core_services`, `is-figures`, `is-metal-cycle`, `is-outlook`, `is-who-we-are`, …).

Grid/flex gaps come from `em`: `.benefits_grid { gap:1em }`, `.footer_card-grid { gap:1.5em }`, `.nav_inner { gap:4em }`, `.nav_menu-inner { gap:1.5em }`, `.areas_card { gap:4.31em }`, `.footer_card { gap:2em }`, plus utility classes `flex-v-gap-4px/8px/12px/16px/24px` and `gap_4px/8px/16px`.

### Responsive behaviour
Breakpoints actually customised: **`main` (≥992), `medium` (≤991), `small` (≤767), `tiny` (≤479)**. `large/xl/xxl` and `tiny`-only extras are not used above `main`.

Typical changes per breakpoint:
- **medium (≤991, tablet)** — the `vw` font scaling *stops* and `body` snaps to 16px; navbar goes full-width with 1.5em margins and the hamburger menu appears (`nav_menu` becomes a z-20 overlay with `rgba(0,0,0,0.6)` + `blur(5px)`); grids drop from 3→2 cols (`.benefits_grid`) or 2→1 (`.footer_card-grid`); headings step down (h1 6.69→4em, h2 3.44→2.5em); `padding-vertical` 7em→5em.
- **small (≤767, mobile landscape)** — headings step down again; buttons get `padding-top/bottom: .88em`; `padding-global` 1.5em→1.25em; `.feature-wrapp.small.hide-mbl { display:none }`.
- **tiny (≤479, mobile portrait)** — buttons go `width:100%`; grids collapse to 1 col with `gap .5em`; `padding-global` → 1em; `padding-vertical` → 2.5em; a family of `_14px-mbl / _16px_mbl / _18px-mbl / _32px-mbl / _13px-mbl` combos pins body copy to fixed px.
- Desktop/mobile element duplication rather than reflow: `.desktop_active` vs `.mobile_active`, `.desktop-active` vs `.tab-active` on `button-group`, `.display-none`, `.hide`, `.hide-mbl`.

### Components (Webflow) — 7, with instance counts
| name | instances | props |
|---|---|---|
| `Global Styles` | 13 | none (HtmlEmbed root) |
| `Navbar` | 12 | none |
| `Footer` | 12 | `Title 1` (textContent, "Sie haben Material?"), `Title 2` ("Sie brauchen Rohstoffe?") |
| `Section Tag` | **43** | `Variant`, `Text` ("DRH-Verbund") |
| `Button Primary` | **64** | `Variant`, `Link` (`{mode:url,to:'#'}`), `Text` ("Kontakt aufnehmen") |
| `Button Secondary` | 28 | `Variant`, `Link`, `Text` ("Profil herunterladen") |
| `CTA` | 4 | `Variant`, `Title`, `Text`, `Image` (assetId `6a9e93451e229d7be1a2b57f`) **+ nested-component prop groups**: `Section Tag > Variant/Text`, `Button Primary > Variant/Link/Text`, `Button Secondary > Variant/Link/Text` |

`Section Tag` internals: `Block.section-tag > HtmlEmbed.polygon + Block.text-size-regular.is_section_tag` — i.e. the Figma `tag` frame (polygon + label) became a component.
`CTA` internals: `Section.section_cta > padding-global > padding-vertical.is_cta > container-large > wrapper_cta > cta_card > (cta_inner + Image.cta_image-absolute + cta_overlay)`.

Every repeated *card* is NOT a component — cards are plain `Block`s with a `<name>_card` class and `is--n` / `01..05` combos.

### CMS
Two collections, both **inherited from the starter template and unused by the German pages**:
- `Blogs` (id `6a9e93451e229d7be1a2b51a`, slug `blogs`) → template page `Blogs Template` `/blogs`
- `Blog Categories` (id `6a9e93451e229d7be1a2b51b`, slug `blog-categories`) → template page `/blog-categories`

There is a `cms`, `pagination`, `blog_card`, `blog_grid`, `blog_image`, `blog_upper-wrap`, `blog_card-lower`, `filter_form`, `filter_radio` class family, and a `/news` page — but `/news` is a **draft** and no live page binds to a collection list. **All homepage/inner-page content is static.**

### Interactions / animations
**Webflow IX3 (6 interactions), all scroll-scrubbed, all scoped to the Home page, all targeting a CLASS rather than an element:**

| id | name | trigger | config |
|---|---|---|---|
| `i-3c359eae` | `Map While Scrolling` | `wf:scroll` | `start:"top top" end:"bottom center" scrub:0.8 clamp:true showMarkers:true` |
| `i-2190b1de` | `Map While Scrolling (Tablet)` | `wf:scroll` | same |
| `i-6aabe36e` | `Areas While Scrolling` | `wf:scroll` | `start:"top top" end:"bottom top" scrub:0.8` |
| `i-bddce016` | `Areas While Scrolling (Tablet))` (sic, double bracket) | `wf:scroll` | `end:"bottom center"` |
| `i-0870c53d` | `Scroll interaction` | `wf:scroll` | `start:"top top" end:"bottom bottom" scrub:0.8` |
| `i-27d04f3f` | `Scroll interaction (Mobile)` | `wf:scroll` | same |

Note `showMarkers: true` is left **on** in all six — GSAP ScrollTrigger debug markers may be visible in preview.

Naming convention: `"<Thing> While Scrolling"` for scrubbed timelines, with `(Tablet)` / `(Mobile)` suffix for the per-breakpoint duplicate. No hover IX (hovers are pure CSS transitions).

### Custom code
**Site → head:**
```html
<style>
  .navbar { transform: translateY(-165%); }
  .navbar, .navbar * { transform-style: flat !important; }
</style>
```
**Site → footer:** Lenis smooth scroll v1.2.3 from unpkg, wired to jQuery:
```html
<link rel="stylesheet" href="https://unpkg.com/lenis@1.2.3/dist/lenis.css">
<script src="https://unpkg.com/lenis@1.2.3/dist/lenis.min.js"></script>
<script>
let lenis = new Lenis({ lerp:0.1, wheelMultiplier:0.7, gestureOrientation:"vertical",
                        normalizeWheel:false, smoothTouch:false });
function raf(time){ lenis.raf(time); requestAnimationFrame(raf); }
requestAnimationFrame(raf);
$("[data-lenis-start]").on("click", function(){ lenis.start(); });
$("[data-lenis-stop]").on("click", function(){ lenis.stop(); });
$("[data-lenis-toggle]").on("click", function(){
  $(this).toggleClass("stop-scroll");
  if ($(this).hasClass("stop-scroll")) { lenis.stop(); } else { lenis.start(); }
});
</script>
```
**Page-level head/footer code on Home:** empty. **Registered scripts (Apps & Scripts):** none — everything is freeform site code.
**Embeds:** 47 `HtmlEmbed` elements on the homepage alone — used for (a) inline SVG icons, (b) inline `<style>` snippets scoped by class (`border-css`, `gradient-css`, `polygon`), (c) the hero `<picture>` / video markup.
**Fonts:** `list_fonts` returns **zero custom fonts** → **Geist** is pulled through Webflow's Google-Fonts integration, not uploaded. (Figma's second token family `Manrope` is unused.)

### Images
- All uploaded as **`.webp`** (`section (1).webp`, `Rectangle 5517 (1).webp`, `img.webp`, …) — the file names come straight from the Figma layer/export names, including `(1)`, `(2)`, `(3)` suffixes.
- Alt text is always written, lowercase, descriptive, and **frequently typo'd**: `"factory whole view"`, `"infinity logo"`, `"gradient backgorunf"` (sic, ×5), `"glowing green circle"`, `"world map with active locations"` (used for both `active_map` and `inactive_map`), `"materials image"`, `"materials crushing machine"`, `"materials moving machine"` (×2), `"silver logo"`, `"dotted brand logo"`.
- Only **16 `<Image>` elements** on the whole homepage vs **47 `HtmlEmbed`s** → icons and decorative vectors are inline SVG, photos are `Image`.
- Responsive/art-directed hero: `Image.hero_image-absolute.display-none` + `HtmlEmbed.hero_image-absolute` + `BackgroundVideoWrapper.hero_image-absolute.display-none` — three stacked variants with `display-none` toggling.

### Forms, links, SEO
- Form classes exist (`contact_form`, `contact_form-wrap`, `form_inner`, `form_inner-gap-48px`, `form_title-wrapper`, `field-wrap`, `field_grid`, `field_label`, `fields_flex`, `input_field`, `checkbox`, `checkbox_field`, `radio_label`, `filter_radio-field`, `submit_button`, `success`, `help-text`) — the live form is on `/kontakt`.
- All nav links are **`linkType:"url"` with literal hrefs** (`/leistungen`, `/recycling`, `/ueber-uns`, `/unsere-werte`, `/downloads`, `/karriere`, `/kontakt`) rather than Webflow page links.
- SEO: every published page has a hand-written German `seo.title` / `seo.description` (e.g. `"FUREC | Recycling, Sekundärrohstoffe & Rohstoffhandel"`). `openGraph.titleCopied/descriptionCopied` = `true` on the main pages, `false` on `/kontakt`, `/karriere`, `/ueber-uns`(partially). OG images are per-page `.webp` assets. Draft pages have a bare `seo.title` only.

---

## 3. Figma → Webflow mapping observed

| Design concept (Figma) | Realised in Webflow as | Notes / what changed |
|---|---|---|
| Figma **Section** `"Performance page"` etc. | a Webflow **page** | Names translated EN→DE: `Performance page`→`/leistungen`, `About us page`→`/ueber-uns`, `Our Values page`→`/unsere-werte`, `Download area page`→`/downloads`, `Recycling page`→`/recycling` |
| Desktop master frame `FUREC` (1920×10398) | the `Home` page | 1:1 section order |
| Frame named `section` (generic, absolute-positioned, 1920×1080) | `Section<section>.section_<topic>` + the 5-level shell `padding-global > padding-vertical > container-large > wrapper_<topic>` | **The builder invents all the semantic names** — Figma provides none |
| Frame `heder-wrap` (1920×120, pinned y=0) | Webflow component **`Navbar`** built on the native Navbar widget, `position:fixed; top:1.5em; max-width:74.69em` | Design shows a full-bleed 1920 header bar; build makes it a floating, rounded, 1195px-wide pill |
| Frame `footer` (1920×1080) | Webflow component **`Footer`** with 2 textContent props | |
| Frame `bnt prm` (215×56, autolayout, px 24 / py 16, gap 8, radius 99999) | component **`Button Primary`** → `.button-primary { padding:1em 1.5em; gap:.5em; border-radius:96px }` | 24px→1.5em, 16px→1em, 8px→.5em all exact. **Radius changed** from `99999` to `96px` on primary (secondary kept `9999px`) |
| Frame `bnt sec` (border-width 1.5px) | `.button-secondary { border:1px solid }` | **border width 1.5 → 1px** |
| Frame `tag` (polygon + label) | component **`Section Tag`** (`Block.section-tag > HtmlEmbed.polygon + Block.text-size-regular.is_section_tag`), 43 instances | promoted to a component; the Figma one was a plain frame |
| Figma colour variable `theme/accent #d1f462` | Webflow variable **`Lime Green` `--lime-green`** | renamed from semantic (`theme/accent`) to literal (`Lime Green`) |
| `theme/background #191919` + `theme/text secondary #191919` + `button/primary/text #191919` | one Webflow variable **`Theme Black` `--theme-black`** | three Figma tokens collapse to one |
| `theme/text #ffffff` | **`White` `--white`** | |
| `theme/border #19191933` | **nothing** — hand-written `hsla(0,0%,100%,0.16)` etc. | token dropped |
| `font-size/*` size tokens (69/55/44/35/28/23/18/16/14/12) | **no Webflow variables**; baked into classes as `em` | see next row |
| Figma text style `H1` (55px) | `.heading-styles-h2` (`3.44em` = 55px) | **off-by-one rename**: Figma H1→WF h2, H2 (44)→WF h3 (2.75em), H3 (35)→WF h4 (2.19em), H4 (28)→WF h5 (1.75em). `heading-styles-h1` (6.69em ≈ 107px) has **no** Figma token — it was created for the oversized hero/section display type |
| Figma `Display` 69px | `.heading-styles-h2.is-69px` combo | expressed as a px-named combo |
| Hero title, Figma `text-[120px] tracking-[-3.6px] leading-[0.83]` | `Heading<h1>.heading-styles-h1.is-72px` on Home, `.is-120px { font-size:7.5em }` (=120px) elsewhere | letter-spacing −3.6px copied verbatim into `.heading-styles-h1` |
| `font-size/h5` 23 | `.text-size-medium` / `.text-size-xmedium` (`1.44em` = 23px) | |
| `text-large` 18 / `text-main` 16 / `text-small` 14 / `text-extra-small` 12 | `.text-size-regular` 1.13em / `.text-size-small` 1em / `.text-size-xsmall` .88em / `.text-size-tiny` .75em | **exact** 1:1 |
| `site/margin` 48 (content = 1824 on a 1920 frame) | `.padding-global { padding:0 1.5em }` (24px) + `.container-large { max-width:117em }` (1872px) | **CHANGED**: content is 1872px wide, not 1824 — gutters halved from 48 to 24 |
| `section-space/main` 112 | `.padding-vertical { padding-top:7em; padding-bottom:7em }` (112px) | **exact** |
| `section-space/small` 80 | `@medium .padding-vertical { 5em }` (80px) | reused as the tablet value |
| `section-space/large` 160 | not found as a single class; approximated by `padding-vertical.is_home-hero { padding-top:15em }` (240px) | |
| `space/5` 16, `space/6` 24, `space/3` 8, `space/4` 12 | `1em`, `1.5em`, `.5em`, `.75em` and the `flex-v-gap-*px` / `gap_*px` utilities | |
| `radius/main` 16 | `border-radius: 1em` on `.areas_card`, `.footer_card` | exact |
| `radius/round` 99999 | `9999px` / `96px` | see buttons |
| Figma auto-layout row/column | `display:flex` with `grid-column-gap`/`grid-row-gap` in `em`, or `display:grid; grid-template-columns:1fr 1fr 1fr` | absolute-positioned Figma sections become flex/grid in Webflow — the builder does the layout work Figma never did |
| Figma absolute-positioned decorative layers (`Rectangle 5517` rotated −0.5°, gradient overlay) | `Image.hero_image-absolute` + `Block.hero_gradient` (absolute children of `.section_hero`) | kept absolute |
| Icons (16/24/28/36px `Frame` nodes, Lucide) | `HtmlEmbed.icon-16px/24px/28px/36px` wrapped in `Block.icon_wrap-40px/56px/84px` | **always** an embed inside a sized wrapper — never an `<img>` |
| Photos | `Image` elements, `.webp`, file names carried over from Figma layer names | |
| Hover states (not in Figma metadata) | invented in Webflow: `transform:scale(0.95)` + colour inversion, `transition:all 250ms ease` on buttons, `200ms` on `nav_link` | |
| `Tablet_homepage` 834 | Webflow `medium` (≤991) | |
| the 767-wide frames | Webflow `small` (≤767) | |
| `Mobile_homepage` 393 / 390 components | Webflow `tiny` (≤479) | |
| `Tablet menu / Active`, `Mobile menu / Active` | native Webflow Navbar `w--open` state + `.nav_menu-inner` medium-breakpoint override | |
| `scroll 1..4`, `scroll 16/17/18` state frames | Webflow IX3 scroll-scrubbed timelines (`Map While Scrolling`, `Areas While Scrolling`, `Scroll interaction`) | the designer delivered keyframe *frames*; the builder turned them into scrubbed GSAP timelines |
| `stems v1` ×39, `collums def` ×208 (vector bar strips) | repeated `Block.small_verticle-bar` divs (60+ siblings) | rebuilt as DOM, not exported as SVG |
| `card Geschäftsfelder  v1` ×38 | `.areas_card.01/.02/.03` (plain divs, not a component) | Figma component → Webflow class, NOT a Webflow component |
| Nothing in Figma | `Global Styles` component with the `0.8333vw` body font-size hack | **the entire responsive strategy is a build-side invention** |
| Nothing in Figma | Lenis smooth scroll | build-side addition |

**Things dropped / simplified between design and build**
- Figma `Manrope` family token — unused.
- Figma `theme/border` token — not carried into Webflow variables.
- `site/gutter` 20 — no matching class; grid gaps are 1em/1.5em.
- The 48px Figma side margin became 24px.
- `border-width/main` 1.5px became 1px.
- The 1920-only "scroll N" state frames were never built as separate sections; they became IX timelines.
- The whole Blog/News/CMS layer designed nowhere in Figma remains as inherited template drafts.

---

## 4. Quality notes and gotchas

### Rules that look deliberate (copy these)
1. **Every section gets the same 5-level shell**: `Section<section>.section_<topic>` → `.padding-global` → `.padding-vertical` → `.container-large` → `.wrapper_<topic>`. The `section_x` / `wrapper_x` pair always share the same topic word.
2. **All sizing is in `em`, never `px` or `rem`**, so the `0.8333vw` body font-size scales the entire desktop design fluidly from the 1920 Figma frame. 1em ≡ 16 Figma px.
3. **`heading-styles-hN` is a class, never a tag.** Tag level is chosen for semantics independently: the homepage has `Heading<h2>.heading-styles-h1.is-120px` and `Heading<h2>.heading-styles-h3`. Exactly one `<h1>` per page.
4. **Per-section spacing lives in a combo, not a new class**: `.padding-vertical.is_<section>`. 47 of them.
5. **Modifiers are `is--<n>` for position and `is_<context>` for context**; a px-value combo (`.is-120px`, `._14px-mbl`) is used whenever a single instance needs a different size.
6. **Icons are always an inline-SVG `HtmlEmbed` with an `icon-<Npx>` class, inside a `Block.icon_wrap-<N>px`.** Photos are always `Image` with hand-written alt.
7. **Text colour is a combo, not part of the size class**: `.text-size-small.text-color-white`, `.text-size-regular.text-color-white.opacity-60`.
8. **Component props are always `Variant` + `Link` + `Text`** on buttons, and nested component prop groups are surfaced on the parent (the `CTA` component exposes 12 props including `Button Primary > Text`).
9. **Scoped CSS goes in a class-named `HtmlEmbed`** next to the thing it styles (`border-css`, `gradient-css`, `polygon`) rather than into site-wide head code.
10. Interactions named `"<Thing> While Scrolling"` with `(Tablet)` / `(Mobile)` duplicates; scrub `0.8`, `start:"top top"`.

### Mistakes / inconsistencies
- **`body { font-size: 0.8333vw }` has a hard discontinuity at 992px**: at 992px viewport 1em = 8.27px, but at 991px the `max-width:991px` rule snaps it to 16px. Everything roughly halves in size across a 1px boundary. All the `medium` breakpoint overrides exist to paper over this.
- **`.container { max-width:1920px }`** in the Global Styles embed targets Webflow's legacy `.container` class, which this site doesn't use — dead code.
- **`showMarkers: true`** left enabled on all 6 IX3 scroll triggers.
- **Stray page `Home 21`** (`/home-21`, created the day of analysis) is a full duplicate of Home and is set to publish.
- **Four starter-template pages** (`Inner Page`, `Location`, `Sustainability`, `News`) and two unused CMS collections left in place as drafts.
- **Duplicate Figma section** `Our Values page` exists twice (`1058:9591` and `3094:4587`).
- Radius inconsistency: `.button-primary` uses `96px`, `.button-secondary` uses `9999px`, Figma says `99999`.
- Typos baked into names, in both files: `heder-wrap`, `Desctop_homepage`, `bnt prm`, `collums`, `hamburger_bar.middel`, `vaccancy_card` / `vaccany_name` / `is_vaccines`, `Theme Backgroud 2`, `wrapper_jouney`, `is_infrastrucure`, `section_responsibilty`, `ion_wrap-56px` (should be `icon_`), `"gradient backgorunf"` alt text, `Areas While Scrolling (Tablet))`.
- **Duplicate/near-duplicate classes**: `why_card-vector` appears twice; `int_bg-image` and `int_bg-image 2`; `active_map` and `active_map 2`; `icon_wrap-56px` and `ion_wrap-56px`; `max-wd-580px` vs `max-width-580px`; `text-size-medium` and `text-size-xmedium` are identical at `main`.
- `.section-title-wrapper` and `.page-wrapper` and `.main-wrapper` have **no CSS at all** — pure structural hooks.
- Alt text `"world map with active locations"` reused for both the active and inactive map images.
- Heavy class proliferation: 585 global classes for a 9-page site, including ~60 `section_*` and ~40 `wrapper_*` that mostly carry only a background colour.

### Special features present
- **Sticky scroll sections**: `.areas_sticky-wrapper`, `.marketing_sticky-scroll`, `.sticky_scroll-tech`, `.sticky-wrap`, `.areas_card.01/.02/.03` driven by the `Areas While Scrolling` IX.
- **Smooth scrolling**: Lenis 1.2.3 with `data-lenis-start` / `data-lenis-stop` / `data-lenis-toggle` attribute hooks.
- **Slider**: Swiper is scaffolded (`swiper`, `swiper-component`, `swiper-slide`, `swiper-slide-inner`, `swiper-wrapper`) — but no Swiper script is registered or in the footer code, so it is either page-level code on another page or unfinished.
- **Dropdown nav** (native Webflow Dropdown ×2), **`:has()`-based radius switching** on the mobile menu.
- **Video**: one `BackgroundVideoWrapper` in the hero (currently `.display-none`).
- **Map**: `map_embed`, `map_wrap`, `map_bg`, `active_map`/`inactive_map`, `location_pin`, `location_pins-wrap` — a scroll-scrubbed world map.
- **Backdrop blur** used repeatedly (`.areas_card`, `.nav_menu-inner`, `.blur`, `.bg-blur-ellipse`, `.blur_ellipse`, `.noise`).
- **No RTL, no localisation (single locale), no dark-mode toggle** — the site is permanently dark (`--theme-black #191919` background).
- Accordions/tabs/modals: no Webflow tab/accordion widgets found; `dropdown` is the only widget besides Navbar.

---

## 5. Verbatim dumps

Omitted from the skill copy; the full dump lives in the analysis workspace.

## 6. Tool quirks  (PILOT — exact call shapes that worked)

### Environment blockers
- **`curl` to the live site is BLOCKED.** `curl -sL https://furec.webflow.io/` → `curl: (56) CONNECT tunnel failed, response 403`. `curl -sS "$HTTPS_PROXY/__agentproxy/status"` confirms `"kind":"connect_rejected","detail":"gateway answered 403 to CONNECT (policy denial or upstream failure)","host":"furec.webflow.io:443"`. This is an org egress-policy denial — **do not retry, and do not plan on fetching the rendered HTML/CSS.** Same for `medicini-plus.webflow.io`, so expect it for every `*.webflow.io` in this batch.
- **Figma asset URLs are also blocked.** `curl -L -o x.png "https://www.figma.com/api/mcp/asset/<uuid>.png"` → same 403. So `get_screenshot`'s recommended download path does not work. **Workaround: call `get_screenshot` with `enableBase64Response: true`** to get the image inline (costs a lot of context — use a small `maxDimension`, e.g. 700–900, and only for one or two frames). No screenshots were saved to the run folder for this project.
- Because of the above, **everything must come from the MCP APIs.** Budget accordingly: ~20 API calls got full coverage here.

### Figma MCP
- `mcp__Figma__get_metadata` with **`fileKey` only (no `nodeId`)** → lists top-level pages. Worked: returned `- 0:1: Design`.
- `mcp__Figma__get_metadata` with `fileKey` + `nodeId:"0:1"` → **1,851,033 characters, exceeds the token limit.** The harness saves it to `/root/.claude/projects/<proj>/<sess>/tool-results/mcp-Figma-get_metadata-<ts>.txt` as `[{type,text}]` JSON. Recovery that worked:
  ```bash
  python3 -c "
  import json; d=json.load(open('<that file>'))
  open('figma_meta_page.xml','w').write(d[0]['text'])"
  ```
  then `grep -nE '^ {0,6}<(section|frame)' figma_meta_page.xml` to walk the tree by indentation. **Do this first — one call gets you the whole file's structure, and greps are free after that.**
- `mcp__Figma__get_variable_defs` — `{fileKey, nodeId:"1013:7706"}` (the homepage frame). Returns a flat `{"token/name":"value"}` map including composed `Font(...)` strings. Node ids must be `\d+:\d+` (no `I`-prefixed instance ids).
- `mcp__Figma__get_design_context` — `{fileKey, nodeId:"1013:7707", clientFrameworks:"unknown", clientLanguages:"html,css", skillNames:"resource:figma-design-to-code", excludeScreenshot:true}`. **Pass `excludeScreenshot:true`** — the screenshot is useless here (can't download) and costs context. The returned React+Tailwind is the fastest way to see whether a frame is auto-layout (`content-stretch flex gap-[..]`) or absolute (`absolute left-[..] top-[..]`), and it inlines the variable names (`var(--font-size/h5,23px)`).
- `mcp__Figma__get_screenshot` — `{fileKey, nodeId, maxDimension}`; returns a URL that is **unfetchable in this environment** (see above).
- `mcp__Figma__use_figma` was not needed; `get_metadata` + `get_variable_defs` + one `get_design_context` covered everything.

### Webflow MCP
- **Every** `data_*` call needs `agent_id`, `session_id`, `context`. First call `session_id:"start"` → the result begins with a text block `[session_id issued …]` followed by `session_id: ses_3JeKT2VvUxHQm3aFu6Zsif6NRMT`. Reuse that exact value on every later call. `agent_id` used here: `fable-5.1|claude-code|q7m2x` (format `<model>|<harness>|<5 chars>`).
- **`data_style_tool`, `data_variable_tool`, `data_element_tool`, `data_element_settings_tool`, `data_component_tool` require BOTH `siteId` AND `pageId` at the top level** — even for site-wide reads like "list all styles". Pass the homepage id. `data_pages_tool`, `data_cms_tool`, `data_scripts_tool`, `data_fonts_tool` take `site_id`/`page_id` **inside the action object instead**, and take **no** top-level `siteId`/`pageId`. Getting this wrong is the #1 validation error.
- `data_pages_tool` → `actions:[{label, list_pages:{site_id, limit:100}}]`. Worked first try.
- `data_style_tool > get_styles` with `{query:"all", include_properties:false}` → **163,285 chars, over the limit**, saved to a tool-results file. Recovery:
  ```bash
  python3 -c "
  import json; d=json.load(open('<tool-results file>'))['result']
  print(len(d))
  open('global_classes.txt','w').write('\n'.join(sorted(s['name'] for s in d if not s['isComboClass'])))
  open('combo_selectors.txt','w').write('\n'.join(sorted(s['selector'] for s in d if s['isComboClass'])))"
  ```
  Each entry is `{id, name, isFromLibrary, isComboClass, type:"global"|"combo"|"tag", selector}`.
  **Never** call `get_styles` with `include_properties:true` on a site this size — use `query_styles` instead.
- `data_style_tool > query_styles` is the workhorse. Shape that worked (8 queries in one call, well under the limit):
  ```json
  {"queries":[{"label":"padglobal","name_path":["padding-global"],"include_properties":true,
               "include_breakpoints":["main","medium","small","tiny"]},
              {"label":"btn","name_path":["button-primary"],"include_properties":true,
               "include_breakpoints":["main","medium","small","tiny"],
               "include_base_pseudos":["hover"],"limit":4}]}
  ```
  - `name_path` with **one** segment = case-insensitive substring match on the class's own name (so `["p"]` matches dozens — always add `limit` and expect noise; `["body"]` returns the `body` **tag** style, not a class).
  - `name_path` with **two** segments targets a combo chain of exactly that depth: `["padding-global","is_hero"]` → `.padding-global.is_hero`. This is the only reliable way to read a specific combo.
  - `include_breakpoints` omitted ⇒ base only. Passing all four is fine for a handful of classes but "data intensive" — don't do it over 20+ classes at once.
  - Colour values come back as `{"id":"variable-<guid>"}` rather than a hex — cross-reference with `data_variable_tool > get_variables`.
  - `properties.base` and `properties.breakpoints.main` are duplicates of each other.
- `data_variable_tool` → first `{get_variable_collections:{query:"all"}}` (returns `collection-<guid>` ids), then `{get_variables:{variable_collection_id:"collection-…", include_all_modes:true}}`.
- `data_element_tool > get_all_elements` with `{depth:-1}` on the homepage → **195,654 chars, over the limit**, saved to a tool-results file. It is valid JSON; flatten it with:
  ```python
  d=json.load(open(f)); root=d['result'][0]['data']
  # walk n['type'], n.get('styleNames'), n['settings'].get('tag'),
  #   n['instanceDetails']['name'] for ComponentInstance, n.get('children')
  ```
  This is the single richest artefact — it replaces the blocked live HTML entirely.
  Use `{depth:6}` to stay under the limit for components.
- `data_element_tool > get_all_elements` with **`scope_component_id`** reads a component definition's tree without any Designer session: `{"get_all_elements":{"depth":-1,"scope_component_id":"b9847110-1761-96d1-c3e6-8b1bd8a28167"}}`. Four such actions in one call worked fine.
- `data_component_tool > get_all_components` with `{options:{includeProps:true, includeInstanceCount:true}}` → names, ids, instance counts and prop definitions in one shot. Nested-component prop groups appear with a `"group"` field.
- `data_element_settings_tool > get_settings` with `{type:"all_resolved_settings", element_id:{component,element}, scope_component_id}` is how you read **HtmlEmbed code**. The `element_id` is a **two-field object copied verbatim** from the element tree — not a string. Returns a `{"key":"code","value":"<style>…"}` entry. Three of these in one call worked.
- `data_scripts_tool` — `{get_site_freeform_code:{site_id}}`, `{get_registered_scripts:{site_id}}`, `{get_page_freeform_code:{page_id}}`. All three in one `actions[]` array worked; note **no top-level `siteId`/`pageId` on this tool**.
- `data_interactions_tool > list_interactions` `{limit:100}` — needs top-level `siteId` + `pageId`. Returns full trigger configs inline.
- `data_fonts_tool > list_fonts` `{site_id, limit:100}` — **only lists UPLOADED custom fonts.** Empty result does NOT mean "no fonts"; it means the fonts are Google/Adobe. There is no MCP action that lists the site's Google Fonts — infer the family from `body { font-family: … }` via `query_styles` on `["body"]`.
- `element_snapshot_tool` was not attempted (it renders via the Designer; likely needs an open session).
- No rate limiting encountered. ~20 Webflow calls in ~10 minutes.

### Missing capabilities worth knowing
- No API to read Webflow's **Google Fonts list**, **breakpoint definitions**, **page folder structure**, or **published HTML**.
- No API returns a class's **compiled CSS string** — you get a property map with variable ids, and you must reassemble it.
- Figma `get_metadata` gives **no layout mode / auto-layout flag / padding / fill** — only id, name, x, y, w, h, `hidden`. You must call `get_design_context` on a node to learn whether it is auto-layout, and what its paddings and colours are.
- Figma text node **names are the text content** (truncated) — that is how the section-content mapping in §1 was derived without any extra calls.
