# Xerosol build conventions for Webflow

What the team's finished sites actually look like inside Webflow. Derived from forensic analysis of Xerosol's Figma-to-Webflow builds (Sep 2026): Furec, Medicini Plus, DRH, BookFlow, CleanFlow, GMG, Wall+Baur, HausVertrauen and Atlas in depth (full reports in `site-analyses/`), with MovePro, EASIT, Primegate and Autoglas only partially read. Where sites disagree, the rule below is the majority pattern and the variants are named. The two marketplace templates (BookFlow, CleanFlow) are the cleanest builds; DRH is the reviewer's reference for navbar and motion; GMG, Wall+Baur and HausVertrauen show the older px-named variant to avoid.

## 1. The fluid root: 1em = 16 Figma pixels

Every site starts with the same global style block. It sets the root font size as a fraction of the viewport so that the whole desktop design scales with the window, and every measurement in the site is written in `em`:

```html
<style>
body { font-size: 0.8333333333333334vw; }           /* 16 / 1920 */
@media screen and (min-width: 1920px) { body { font-size: 16px; } }   /* max */
@media screen and (max-width: 991px)  { body { font-size: 16px; } }   /* min: tablet and below stop scaling */
body { -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; }
</style>
<style>
/* Reset Apple form styles */
input, textarea, select { -webkit-appearance: none; -moz-appearance: none; appearance: none; border-radius: 0; background-image: none; }
* { font-variant-ligatures: none; }
</style>
```

Consequences you must respect:

- Convert every Figma value on a 1920 frame by dividing by 16: 24px padding becomes `1.5em`, 112px section padding becomes `7em`, a 1720px content width becomes `107.5em`. On a 1440 frame divide by 15 (`1440/96`) and change the vw to `1.1111vw` (16/1440); keep the clamps.
- Never write `px` or `rem` for sizes. The only px values are hairline borders (1px, 2px) and letter-spacing.
- Below 991px the root snaps to 16px, so the medium, small and tiny breakpoints need real overrides (heading steps, grid columns, paddings). The team's weakest builds skipped this and shipped desktop em values into narrow viewports. Do not repeat that: write the tablet and mobile overrides for every typography class, every grid and every `padding-vertical` combo.
- The block lives in a component named `Global Styles` (an HtmlEmbed as the first child of `page-wrapper`, placed on every page) in the hand-built sites. When building through the MCP, put the same CSS in the site head custom code instead; it is one call and cannot be forgotten on a new page.

## 1b. Responsive through variable modes (the template-grade variant)

The marketplace template BookFlow, the cleanest build in the corpus, keeps the fluid root (`1.1111vw` for its 1440 frame, clamped to 16px at ≥1440 and ≤991) but moves **all type and spacing values into a `Responsive` variable collection with four modes**: `Base mode`, `Tablet`, `Mobile Landscape`, `Mobile Portrait`. Each mode is attached to a breakpoint, so `.heading-style-h1 { font-size: var(--heading-size--h1) }` has no breakpoint CSS at all and still steps down on tablet and phone. Only layout changes (grid columns, stacking, hide/show) are written per breakpoint.

Prefer this over per-class breakpoint overrides on every new build. It is fewer style blocks, the whole scale is editable in one panel, and it is exactly what the MCP can create headlessly:

```
data_variable_tool > create_variable_collection  name "Responsive"
data_variable_tool > create_variable_mode        name "Tablet",           breakpoint_id "medium"
data_variable_tool > create_variable_mode        name "Mobile Landscape", breakpoint_id "small"
data_variable_tool > create_variable_mode        name "Mobile Portrait",  breakpoint_id "tiny"
data_variable_tool > create_size_variable        "Heading Size/H1"  {value 3.5, unit em}   (desktop value)
data_variable_tool > update_size_variable        same id, mode_id <Tablet>, {3.25, em} … and so on per mode
data_style_tool    > update_style  heading-style-h1  properties [{property_name "font-size", variable_as_value <id>}]
```

Groups and values measured on BookFlow (1em = 16px):

| variable | desktop | tablet | landscape | portrait |
|---|---|---|---|---|
| Heading Size/H1 | 3.5em | 3.25em | 2.75em | 2em |
| Heading Size/H1 display | 6.25em | 5em | 4em | 3em |
| Heading Size/H2 | 3em | 2.75em | 2.5em | 2em |
| Heading Size/H3 | 2.25em | 2.25em | 2em | 1.5em |
| Heading Size/H4 | 2em | 2em | 1.25em | 1.13em |
| Heading Size/H5 | 1.5em | 1.25em | 1.25em | 1.13em |
| Heading Size/H6 | 1.25em | 1.25em | 1em | 0.88em |
| Text Size/XLarge | 1.5em | 1.25em | 1.25em | 1.13em |
| Text Size/Large | 1.25em | 1.25em | 1.13em | 1em |
| Text Size/Medium | 1.13em | 1.13em | 1em | 0.94em |
| Text Size/Regular | 1em | 1em | 1em | 0.88em |
| Text Size/Small | 0.88em | 0.88em | 0.88em | 0.88em |
| Padding Global | 6.25em | 1.5em | 1em | 0.75em |
| Padding Section/Large | 7.5em | 5.5em | 4.5em | 3em |
| Padding Section/Medium | 5em | 4em | 4em | 3.5em |
| Padding Section/Small | 2.88em | 2.5em | 2.5em | 2.5em |
| Padding Section/Tiny | 1.25em | 1.25em | 1.25em | 1.25em |
| Container/Large | 75em | 75em | 75em | 75em |
| Container/Medium | 65em | | | |

Variable naming there is `Group/name` (`Heading Size/H1`, `Padding Section/Large`, `Text Color/cinder`), which Webflow turns into `--_responsive---heading-size--h1`. The colour collection uses "name that colour" names (`royal blue`, `mirage`, `white smoke`); for client sites prefer the Figma semantic names, for templates the colour-name style is what the team ships.

Scale it to the frame: divide the Figma values by 16 (1920 frame) or by 15 (1440 frame), and set the vw fraction to 16 divided by the frame width.

## 2. Page and section skeleton

```
Body
  .page-wrapper
    [Global Styles component]
    main.main-wrapper
      [Navbar component]
      section.section_<topic>            one per design section, semantic topic word
        .padding-global                  horizontal gutters only
          .padding-vertical.is_<topic>   vertical rhythm; the combo carries the section's values
            .container-large             max-width + auto margins
              .wrapper_<topic>           the section's own layout (flex/grid)
                .section-title-wrapper   eyebrow + heading + paragraph, when present
                ...content...
      [Footer component]
```

- The `section_<topic>` and `wrapper_<topic>` pair share the same topic word: `section_hero` and `wrapper_hero`, `section_pricing` and `wrapper_pricing`.
- The order of the shell is fixed. One site placed the container before the vertical padding (`padding-global > main-container > section-padding-verticle.is_x > content-wrapper.is_x`); either order works, but keep one order across the whole site.
- The `padding-vertical` base class has no values of its own in some sites and a default of `7em / 5em / 3.13em / 2.5em` (desktop / tablet / landscape / portrait) in others. Prefer the default plus a scale of combos: `is_xlarge` 10em, `is_large` 8.75em, `is_medium` 8.13em (Medicini values) or per-section `is_hero`, `is_cta`, `is_footer` with asymmetric values (hero: `15em` top, `5em` bottom; CTA: `3em`; footer: `9.38em` top, `1.5em` bottom).
- Full-bleed decoration sits **outside** the padding chain as absolutely positioned children of the section: `.absolute-bg > img.image.is-cover`, `.hero_gradient`, `.section_overlay`, `.grey_radial-ellipse`, all `pointer-events:none`.
- Sticky and scroll-scrubbed sections use a dedicated wrapper (`.areas_sticky-wrapper`, `.sticky-wrap`) inside `wrapper_<topic>`.

Gutters and containers (Furec, Medicini):

| class | desktop | tablet ≤991 | landscape ≤767 | portrait ≤479 |
|---|---|---|---|---|
| `padding-global` | `1.5em` to `2.5em` sides | same | `1.25em` | `1em` |
| `container-large` / `main-container` | `max-width: 107.5em` (1720) or `117em` (1872) | 100% | 100% | 100% |

Pick the container from the Figma content column: content x offset × 2 subtracted from the frame width, divided by 16.

## 3. Class naming

A Client-First-flavoured system. Global layer from Client-First, component layer in `block_element-role` snake case, modifiers as `is_` combos.

| layer | pattern | verbatim examples |
|---|---|---|
| page shell | Client-First names | `page-wrapper`, `main-wrapper`, `padding-global`, `padding-vertical`, `container-large`, `section-title-wrapper`, `button-group` |
| sections | `section_<topic>` + `wrapper_<topic>` | `section_hero`, `section_cta`, `section_faq`, `wrapper_hero`, `wrapper_cta` |
| typography | `heading-style-h1..h6`, `text-size-*`, `text-color-*`, `weight-*` | `heading-style-h2`, `text-size-regular`, `text-size-large`, `text-color-white`, `eyebrow-text`, `button-text` |
| blocks | `<block>_<element>-<role>` | `nav_container`, `nav_links-wrapper`, `nav_brand-logo`, `care_service-card`, `faq_upper-wrapper`, `cta_card`, `footer_social-block` |
| cards | `<topic>_card` + child `<topic>_card-<part>` | `areas_card`, `testimonial_card`, `kompakt_card-bg`, `audience_card-content` |
| icons | `icon_<N>px` wrapper + `embed-icon` | `icon_24px`, `icon_28px`, `icon_36px`, `icon_wrap-56px` |
| modifiers (combos) | `is_<context>` for context, `is-<n>` or `01..05` for position, `is_<size>` for scale | `is_home-hero`, `is_cta`, `is_large`, `is_dark`, `is-cover`, `is-1`, `is-2`, `.areas_card.01` |
| state / visibility | | `display-none`, `hide`, `hide-mbl`, `desktop_active`, `mobile_active`, `opacity-0` |
| vendor | keep library names | `swiper`, `swiper-wrapper`, `swiper-slide`, `marquee_horizontal`, `track-horizontal` |

Rules the team keeps:

- Underscore after the block name, hyphen inside a name: `nav_links-wrapper`, not `nav-links-wrapper` or `nav_links_wrapper`.
- `is_` with an underscore for modifiers (the hyphen form `is-cover` appears but is the minority; pick one and stay with it).
- Headings always carry a `heading-style-hN` class chosen for **size**; the HTML tag is chosen separately for outline. `h2.heading-style-h4` is normal. Exactly one `h1` per page.
- Paragraphs always carry a `text-size-*` class; colour is a second combo (`text-size-regular.text-color-white`), never baked into the size class.
- Per-instance size tweaks are px-named combos on the typography class: `.heading-style-h1.is-120px { font-size: 7.5em }`, `.text-size-small._14px-mbl`. Use sparingly.
- Utilities that exist: `flex-v-gap-8px/12px/16px/24px`, `gap_8px/16px`, `margin_top-36px`, `max-wd-710px` (each holding em values despite the px name). Prefer setting gap on the wrapper combo instead of adding more of these.
- Fix typos before they ship. The corpus contains `verticle`, `Serction`, `heder-wrap`, `middel`, `vaccancy`, `Backgroud`; reviewers copy them into every future site because renaming is expensive.

## 4. Typography scale

Figma text styles map one-to-one onto classes, but the **name shifts one level** when the designer's H1 is smaller than the hero display size. Decide the mapping once on the build sheet. Two observed scales (desktop em, 1em = 16px at 1920):

| class | Furec (display-led) | Medicini | tablet | landscape | portrait |
|---|---|---|---|---|---|
| `heading-style-h1` | 6.69em, lh 1.05, ls −3.6px | 5em, lh 1.1, ls −2.5px | 4em | 3.5em | 2.5em |
| `heading-style-h2` | 3.44em, lh 1.0 | 4em, lh 1.1 | 2.5em | 1.88em | 1.38em |
| `heading-style-h3` | 2.75em, lh 1.1 | 2.75em, lh 1.3 | 2.5em | 1.88em | 1.38em |
| `heading-style-h4` | 2.19em, lh 1.2 | 2em, lh 1.3 | — | 1.75em | 1.5em |
| `heading-style-h5` | 1.75em, lh 1.15 | 1.75em | — | 1.5em | 1.25em |
| `heading-style-h6` | — | 1.5em, lh 1.4 | | | |
| `text-size-large` | 1.75em | 1.25em | 1.5em | 1.25em | 1.13em |
| `text-size-medium` | 1.44em, lh 1.3 | — | 1.25em | 1.13em | |
| `text-size-regular` | 1.13em, lh 1.35 | 1.13em | | 1em | |
| `text-size-small` | 1em, lh 1.5 | 1em, lh 1.6 | | | |
| `text-size-xsmall` | 0.88em, lh 1.5 | — | | | |
| `eyebrow-text` | via `Section Tag` component | 1.25em, w600 | | | |
| `button-text` | 1em | 1.13em, w600 | | | |

- Line height is written as a unitless ratio, letter spacing in px. Compute the ratio from Figma (92/80 = 1.15) rather than rounding to 1.1; the reviewer measures text against the design.
- Weight comes from the class (`font-weight: 500` on headings in Medicini, 400 in Furec) and from `weight-600` combos.
- Nothing under 14px (0.88em) anywhere.
- Fonts: Google Fonts when the family is on Google (Manrope, Geist, Inter); otherwise upload woff2 through `data_fonts_tool`. Body tag style sets `font-family` and colour; heading tags get `margin: 0` and inherit.

## 5. Colour variables

One collection, colour variables only. Nobody creates size, spacing or font variables in Webflow; those live in the em values.

- Naming in the corpus is inconsistent: literal colour names (`Lime Green`, `Theme Black`, `White 80%`) on Furec, name-that-colour names (`mirage`, `liver grey`, `geyser`) on Medicini. Neither matches the Figma token names. Prefer the Figma **semantic** token names when they exist (`theme/accent`, `text/primary`) so the design and build share a vocabulary; fall back to literal colour names, never to `blue-2`.
- Always include: `white`, `black`, `transparent`, the brand primary, the text primary, the text secondary, the page background, and the alpha tints actually used (`white 10%`, `white 30%`, `black 60%`).
- Reference them from styles with `variable_as_value`, never re-type the hex.

## 6. Components

Componentise what repeats with only text or link changing. Leave structurally different repeats as classed markup.

| component | props | instances observed |
|---|---|---|
| `Global Styles` | none | one per page |
| `Navbar` | none | one per page |
| `Footer` | optional `Title` textContent props | one per page |
| `Button Primary` | `Variant`, `Link`, `Text` | 11 to 64 per site |
| `Button Secondary` | `Variant`, `Link`, `Text` | 5 to 28 |
| `Section Tag` / `Section Eyebrow` | `Variant`, `Text` | 17 to 43 |
| `CTA` | `Variant`, `Title`, `Text`, `Image` + nested button props | 2 to 4 |

- Button internals: `a.button-primary > .button-text + .button_arrow (HtmlEmbed inline SVG)`, or with an icon circle `.button_circle-block > .button_circle-inner-block > two arrow embeds` for the swap-on-hover effect.
- Variants live on the component (`Base`, `Small`, `Blue Gradient`, `Light Pink`) and are styled through `data_component_variants_tool`.
- Cards are **not** components. They are repeated `.<topic>_card` trees with `is-1`, `is-2` or `.01`, `.02` combos when one differs.

## 7. Navbar

Native Webflow Navbar element set (`NavbarWrapper`, `NavbarBrand`, `NavbarMenu`, `NavbarLink`, `NavbarButton`) plus native `Dropdown` widgets, wrapped in the `Navbar` component.

```
NavbarWrapper.navbar (position: fixed; top 1.5em; width 100%; z-index high)
  .navbar_bg (padding 1em 1.5em; the visible bar, full width)
    .nav_inner (flex, space-between, align center, gap 4em)
      NavbarBrand.nav_brand > img.image
      NavbarMenu.nav_menu
        .nav_menu-inner (flex, gap 1.5em)
          NavbarLink.nav_link ...
          DropdownWrapper.dropdown
            DropdownToggle.dropdown-toggle > .toggle_text + HtmlEmbed.dropdown_icon (chevron svg)
            DropdownList.dropdown_list > .dropdown_list-inner > a.list_card ...
          .mobile_active > [Button Primary]        shown ≤991 only
      .nav_right-wrap
        .desktop_active > [Button Primary]         hidden ≤991
        NavbarButton.menu_btn > .hamburger > .hamburger_bar.top/.middle/.bottom
```

- Reviewer rules: bar spans the **full viewport width**; dropdown list centred under its toggle; chevron rotates 180° on open; mobile menu shows the logo, keeps desktop type sizes, locks body scroll, and duplicates the CTA (`mobile_active` copy inside the menu, `desktop_active` copy on the right).
- At ≤991 `.nav_menu-inner` becomes a column with `max-height: 85vh; overflow: auto; background: rgba(0,0,0,.6); backdrop-filter: blur(5px)`.
- Navbar is visible on first paint; if it animates, it moves in together with the hero content, never from opacity 0 with a delay.

## 8. Icons, images, decoration

- Icons: inline SVG inside an `HtmlEmbed.embed-icon`, wrapped by `.icon_<N>px` (`width/height: N/16 em`, flex, shrink 0). Never an `<img>` for an icon; never a Webflow icon font. Use `stroke="currentColor"` so the icon takes the text colour.
- Photos: `img.image.is-cover` (`width:100%; height:100%; object-fit: cover`) inside a wrapper that defines the box (`.card_image-wrapper` with a fixed em height or aspect ratio, or `.absolute-bg` for full-bleed). Upload WebP under 400KB with the alt text set at upload time.
- Radii: cards `1.5em` (24px), images `1.25em`, pills `6.25em` (100px) or `9999px`, circles `100%`. Pick one pill radius per site.
- Glass: `background: var(--white-10)` plus `backdrop-filter: blur(5px)` and a 1 to 2px `var(--white-30)` border. Provide a solid fallback colour because the reviewer tests Chrome and Safari.
- Gradients that continue into the next section get an overlay div at the boundary, not a hard edge.

## 9. Layout mechanics

- Auto-layout row → `display:flex; align-items:center; column-gap / row-gap` in em. Auto-layout column → `flex-direction: column`. Webflow writes both gap properties; do the same.
- Card grids → `display:grid; grid-template-columns: repeat(3, 1fr); gap: 1.5em` on the wrapper combo, then `1fr 1fr` at ≤991 and `1fr` at ≤479. The base `.grid` class is generic; the `is_<topic>` combo sets the column count.
- Two-column content sections → `.content-wrapper.is_<topic>` (flex, gap 4 to 6.5em) with `.content-left-wrapper` and `.content-right-wrapper`; stack at ≤767.
- Text columns get a `max-width` in em on the wrapper (`max-wd-33em`), not on the paragraph.
- Buttons stack inside `.button-group` (flex, gap 1em; `width:100%` on each button at ≤479).

## 10. Motion and libraries

- Smooth scroll: Lenis 1.2.x from the site footer code, with `data-lenis-stop` on modals and menus.
- Sliders: Swiper (8 or 11) from page footer code; classes `swiper`, `swiper-wrapper`, `swiper-slide`, arrows `.slider-arrow.button-prev/.button-next`. Set `slidesPerView` per breakpoint (1 / 1.7 / 2 at 480 / 768 / 992 on Medicini).
- Reveal animations: Webflow IX3 targeting a **class** (so it applies to every instance), one timeline per section group, scrub or scroll-into-view with an early offset. Name them `"<Thing> While Scrolling"` with `(Tablet)` and `(Mobile)` duplicates when values differ. Remove `showMarkers` before handover.
- Hover: primary and secondary button hover as a site-scoped IX3 hover interaction on the class, or plain CSS `:hover` with `transition: all 250ms ease`. Card hover is a light or background change, never scale.
- Marquee: CSS keyframes in a scoped HtmlEmbed (`.marquee_horizontal > .track-horizontal`).
- Scoped CSS lives in an HtmlEmbed next to the element it styles (`gradient-css`, `border-css`, `polygon`) rather than in the site head.

## 11. CMS

- Client sites are mostly static; CMS is used for news/blog, projects, jobs, team, properties and publications, each with a filter UI (Finsweet CMS Filter attributes) and load-more.
- Templates ship `Blog Posts`, `Blog Categories`, `Image Licenses` (marketplace requirement) collections, a `Style Guide`, `Licenses`, `Changelog` and `404` under a `utility-pages` folder.
- Collection lists reuse the same card classes as static cards so the two look identical.

## 12. Pages, SEO, code

- Slugs in the site language (`/leistungen`, `/ueber-uns`, `/kontakt`), lowercase, hyphenated, in folders that match the nav.
- Every page: SEO title, meta description, OG image, correct `h1`. 404 page title branded.
- Site head: Global Styles block (if not a component), font links, `theme-color` meta. Site footer: Lenis, Swiper init, GSAP only when scroll-scrubbing is needed.
- Delete starter-template leftovers (pages, collections, classes) before handover.
