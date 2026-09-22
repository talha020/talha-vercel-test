# Medicini Plus
Figma: https://www.figma.com/design/1k2mRLF8eY3dHofsw916eK/Medicini-Plus?node-id=57-5822  |  Webflow site id: 6aabcea58545a9ba3b4b1a49 (shortName `medicini-plus`)  |  Live: https://medicini-plus.webflow.io/  |  Analysed: 2026-09-21

Builder: Abdullah Naeem (Xerosol), Sep 2026. Slack note: *"The Design was taken by two different designers – the Homepage is the best build."*
Run folder: `analysis/medicini-plus/` (element tree + style dump saved there; **no screenshots** – see §6, image downloads are egress-blocked).

**Status caveat, read first:** the Webflow site was still under construction at the time of analysis (pages last updated 2026-09-21 17:55–17:58, i.e. minutes before this run). Only 2 of the ~8 designed pages exist, and **there are essentially zero tablet/mobile breakpoint overrides** (12 property overrides across 230 classes). Treat this as a snapshot of the team's *desktop build conventions*, not of a finished responsive site.

---

## 1. Figma file anatomy

### Pages in the file
The document has exactly **one Figma page**: `Draft` (node `57:5822`). Everything – design, dev-ready frames, archive, notes – lives on that single canvas, organised by **Figma Sections** and loose top-level frames rather than by pages.

Top level of `Draft`, in canvas order (id | name | size):

| kind | id | name | w × h | role |
|---|---|---|---|---|
| section | `2002:2561` | `Full homepage UPD: 18/08` | 5406 × 14645 | earlier homepage iteration (18 Aug) + hero alternatives + sticky notes |
| section | `2002:3879` | `Final Design` | 11118 × 18096 | Desktop / Mobile / Tablet sub-sections of the signed-off homepage |
| section | `2002:5783` | `Website swicher 15.09` | 12203 × 17069 | **latest revision (15 Sep)** – Desktop / Mobile / Tablet, adds the Personal ↔ Personal Plus site switcher in the header. *This is the build-from source.* |
| frame | `75:6290` | `Medicini - For Facility (German)` | 1920 × 15776 | other page design |
| frame | `103:5041` | `Medicini - About Us (German)` | 1920 × 9773 | other page design |
| frame | `75:5043` | `Medicini - Nursing staff and medical professionals` | 1920 × 16640 | other page design (the 2nd page that got built) |
| frame | `75:5515` | `Medicini - Career at Medicini` | 1920 × 8834 | other page design |
| frame | `75:5811` / `60:7592` | `Medicini - Contact Us` | 1920 × 6669 / 1258 | other page design |
| frame | `60:6779` | `Medicini - Contact Us Hero` | 1920 × 1258 | hero-only exploration |
| frame | `75:6823` | `Treatment Cost Calculator` | 1920 × 1258 | other page design |

Note the two naming conventions living side by side: the old top-level page frames are `Medicini - <Page name>` (sometimes with `(German)` appended), while the newest work is organised as `Section > Breakpoint section > <Page>` frames. The `note` frames (600 × 132/164/196/228) are designer-to-developer comments pinned beside the Sections.

### Build-from source and breakpoint frames
Inside both `Final Design` and `Website swicher 15.09` the structure is identical:

```
Section "Website swicher 15.09"
  Section "Desktop"  (2002:5784)
    frame "Homepage"          2002:5785   1920 × 14437   <-- build-from
    frame "Personal website"  2002:6227   1920 × 940
    frame "Hero"              2002:6413   1920 × 940
    frame "note" ×3
  Section "Mobile"   (2002:6615)
    frame "Homepage_Mobile"        393 × 16596
    frame "Mobile menu / Default"  393 × 826
    frame "Mobile menu / Expanded" 393 × 826
  Section "Tablet"   (2002:7499)
    frame "Homepage_Tablet"         834 × 16043
    frame "Mobile menu / Default"   834 × 1195
    frame "Mobile menu / Expanded"  834 × 1195
```

**Breakpoints designed in Figma: 1920 (Desktop), 834 (Tablet, iPad Air portrait), 393 (Mobile, iPhone 14/15).** Naming pattern is `Homepage` / `Homepage_Tablet` / `Homepage_Mobile` – i.e. the desktop frame is unsuffixed and the others carry an underscore suffix. Menu states are named with slashes as pseudo-variants: `Mobile menu / Default`, `Mobile menu / Expanded`.

### Section frame names inside the homepage (verbatim, top to bottom)
Identical in all three breakpoint frames and in both revisions:

1. `Hero`
2. `Serction 2 variant`   ← *sic*, "Serction" is misspelled consistently throughout
3. `Serction 4`
4. `Serction 5`
5. `Serction 6`
6. `reviews section`
7. `Serction 8`
8. `Serction 9`
9. `Closing CTA Section`
10. `Footer` (a component instance on Desktop; a plain frame on Tablet/Mobile)

So the section naming is **positional and non-semantic** ("Serction 4"), with only Hero / reviews section / Closing CTA Section / Footer named by meaning. Note `Serction 3` and `Serction 7` do not exist – they were deleted and the numbers never re-flowed.

Inner frame names are generic Figma defaults mixed with intent names: `Text`, `text`, `Content`, `content`, `CONTENT`, `cards`, `Cards`, `card 1`, `card 3`, `stats`, `buttons`, `fab`, `eyebrow`, `actions`, `Links`, `Link`, `Header`, `Background`, `Bg`, `map image`, `features`, `BADGES`, `navigation`, `Topic 1`…`Topic 6`, plus auto-names like `Frame 2085664661`, `Rectangle 12`, `Ellipse 19`, `image 112`.

### Design tokens (Figma variables, verbatim names → values)
Colour variables:
| name | value |
|---|---|
| `Text Black` | `#1A1A1A` |
| `Text Paragraph` | `#4D4D4D` |
| `White` | `#FFFFFF` |
| `Red` | `#EB0000` |
| `Blue` | `#85ABE7` |
| `Light Blue` | `#B9D0F2` |
| `Dark Purple` | `#491E42` |
| `Background Grey` | `#F7F7F7` |
| `Diving line 1` | `#000000` (sic – "Dividing line") |
| `Red gradient`, `Red blue gradient`, `Background 1`…`Background 4` | gradient/effect variables, no flat value returned by `get_variable_defs` |

Text styles are Figma **variables** (not text styles), named `<Breakpoint>_<Role>`:

| variable | family | style | size | weight | line-height | letter-spacing |
|---|---|---|---|---|---|---|
| `Desktop_H1` | Manrope | Medium | 80 | 500 | 92 | -5 (%) |
| `Desktop_H2` | Manrope | Medium | 64 | 500 | 76 | -5 |
| `Desktop_H3` | Manrope | Medium | 44 | 500 | 56 | -3 |
| `Desktop_H4` | Manrope | Medium | 32 | 500 | 40 | -2 |
| `Desktop_H5` | Manrope | Medium | 24 | 500 | 32 | -2 |
| `Desktop_Eyebrow` | Manrope | SemiBold | 20 | 600 | 28 | -2 |
| `Desktop_Subtitle` | Manrope | Medium | 20 | 500 | 28 | -2 |
| `Desktop_Button` | Manrope | SemiBold | 18 | 600 | 24 | -2 |
| `Desktop_Button small` | Manrope | SemiBold | 16 | 600 | 24 | -2 |
| `Desktop_Paragraph large` | Manrope | Medium | 18 | 500 | 24 | -1 |
| `Desktop_Paragraph small` | Manrope | Medium | 16 | 500 | 20 | -1 |
| `Tablet_Subtitle` / `Tablet_H5` / `Tablet_Paragraph` / `Tablet_Paragraph small` | Manrope | Medium | 18 / 18 / 16 / 14 | 500 | 24 / 28 / 24 / 20 | -2 |
| `Mobile_H1` | Manrope | Medium | 40 | 500 | 48 | -2 |
| `Mobile_H2` | Manrope | Medium | 32 | 500 | 40 | -2 |
| `Mobile_H3` | Manrope | Medium | 24 | 500 | 32 | -2 |
| `Mobile_H4` | Manrope | Medium | 18 | 500 | 24 | -1 |
| `Mobile_H5` | Manrope | Medium | 16 | 500 | 24 | -1 |
| `Mobile_Eyebrow` | Manrope | SemiBold | 16 | 600 | 24 | -2 |
| `Mobile_Subtitle` / `Mobile_Paragraph large` | Manrope | Medium | 16 | 500 | 24 | -2 |
| `Mobile_Paragraph small` | Manrope | Medium | 14 | 500 | 20 | -2 |

**One font in the whole file: Manrope** (Medium 500 and SemiBold 600 only). No spacing, radius or effect variables were found – spacing and radii are hard numbers on frames.

### Components in Figma (verbatim names)
Component/instance names observed on the homepage and its neighbours:
- `Button / Primary / Large`, `Button / Primary / Small`, `Button link` (slash-path naming = Figma variant folders)
- `eyebrow` (instance used 17×; matches Webflow's `Section Eyebrow` component 17 instances)
- `Footer` (symbol / instance, 1920 × 291)
- `card` (testimonial/kompakt cards – some instances, some detached frames in the same row, e.g. `Serction 8` has `instance card`, `frame card`, `instance card`)
- `Minus` (symbol, 24 × 24 – an icon)
- Icon names come straight from Phosphor Icons: `CaretRight`, `CalendarDots`, `Phone`, `EnvelopeSimple`, `Plus`
- `logo medicini 1`

### Auto-layout usage
Yes, heavily. `get_design_context` on the hero returns `content-stretch flex … gap-[…]` for nearly every content frame, with the page-level background / hero-image layers absolutely positioned.

Typical desktop values:
- **Content column is 1720 px wide starting at x = 100** in every section (`Serction 4`, `6`, `8`, `9`, `reviews section`, `Closing CTA` all have a child frame at `x=100 w=1720`). That is the *content max width*: 1720 on a 1920 frame → 100 px gutters.
- Hero `Content` frame: `left-[100px]`, `flex flex-col gap-[56px]`; inner `Text` frame `gap-[32px] w-[643px]`; inner `text` frame `gap-[24px] w-[613px]`.
- Buttons: `pl-[24px] pr-[8px] py-[8px] gap-[16px] rounded-[78px]`, icon circle `size-[40px] rounded-[1000px]`.
- Eyebrow: `px-[16px] py-[8px] rounded-[130px]`, `bg-[rgba(255,255,255,0.4)]`, 1 px border `rgba(255,255,255,0.4)`, `shadow-[0px_4px_4px_0px_rgba(0,0,0,0.02)]`.
- Hero stats card: `p-[24px] gap-[24px] rounded-[24px] backdrop-blur-[45px] bg-[rgba(255,255,255,0.3)] border-2 border-[rgba(255,255,255,0.4)]`.
- FAB buttons: `size-[72px] rounded-[100px]` with `linear-gradient(#ff7070 → #eb0000)`.
- Nav pill: `h-[56px] rounded-[100px] px-[32px] py-[8px] bg-[rgba(255,255,255,0.5)] border border-[rgba(255,255,255,0.3)]`, links `gap-[32px]`.

Glassmorphism (translucent white fills + blur + inset highlights) is the dominant visual idiom.

### Asset types
- **Photos**: large PNG photography (hero background, audience cards, care-service cards, testimonial portraits, CTA image, map image). Exported as PNG, not WebP/JPG.
- **SVG icons**: Phosphor-style line icons (`CaretRight`, `Plus`, `Phone`, `EnvelopeSimple`, `CalendarDots`), plus custom badge glyphs (`Frame 2085664631`…`35`).
- **Logo**: `logo medicini 1` as SVG in Figma, shipped to Webflow as `logo.png`.
- **Decorative**: large blurred ellipses (`Ellipse 19/21/22`) and gradient rectangles (`Rectangle 12/13/16/17`) used as section glows.

---

## 2. Webflow site anatomy

### Pages
| title | slug | id | notes |
|---|---|---|---|
| Home | *(root)* | `6aabcea78545a9ba3b4b1a6c` | created 2026-09-17, last updated 2026-09-21 17:55. SEO object empty (`"seo":{}`), no OG image. |
| Nursing staff and medical professionals | `nursing-staff-and-medical-professionals` | `6ab15597ff502d94b89da519` | created 2026-09-21, SEO title = page title only. |

- **CMS collection pages: none.** `get_collection_list` returns `{"collections":[]}`.
- **Utility pages: none found** – no style guide, no licences page, no 404/password page in `list_pages`. There is no Client-First `style-guide` page.
- 8 of the designed pages have not been built yet.

### Class naming convention — a custom, Client-First-*flavoured* hybrid

**Verdict: custom system, clearly Client-First-inspired but not Client-First.** Evidence:

*Client-First markers present:* `page-wrapper`, `main-wrapper`, `padding-global`, `heading-style-h1`…`h6`, `text-size-small/regular/large/xlarge`, `button-group`, `display-none`, `is-cover`, and the `is_*` combo-modifier idiom.

*Client-First markers absent / diverged:*
- Client-First's `container-large` → renamed **`main-container`**; `padding-section-large` → renamed **`section-padding-verticle`** (with the British-ish misspelling "verticle" kept everywhere).
- No `spacing-*` utilities, no `padding-section-*`/`margin-*` utility stack, no `text-weight-*`, no `align-center`, no `max-width-*` scale (instead ad-hoc `max-wd-33em`, `max-wd-41em`).
- Modifiers use an **underscore** (`is_home-hero`) rather than Client-First's hyphen (`is-home-hero`) — except `.image.is-cover`, `.flex-wrapper.is-testimonial`, `.link_icon-embed.is-1/.is-2` which slipped back to hyphens.
- Most component classes are **snake/underscore-first**: `section_footer`, `nav_container`, `care_service-card`, `faq_upper-wrapper` — a `block_element-modifier` house style that Client-First does not use.
- Not Relume: no `layout###_component`, `section_layout###`, `button-group`, `w-layout-blockcontainer`-driven Relume class set.

So: **two naming dialects coexist** — a Client-First-derived global layer (`padding-global`, `heading-style-h*`, `text-size-*`) and a bespoke `section_*` / `<block>_<element>-<role>` component layer.

**30+ verbatim examples, grouped**

*Layout / structure*
`page-wrapper`, `main-wrapper`, `padding-global`, `main-container`, `section-padding-verticle`, `section-hero`, `section_audience`, `section_care-givers`, `section_locations`, `section_why-medicini`, `section_testimonial`, `section_kompakt`, `section_faq`, `section_cta`, `section_footer`, `section_support`, `section_benefits`, `section_applications`, `content-wrapper`, `content-left-wrapper`, `content-right-wrapper`, `section-title-wrapper`, `flex-wrapper`, `grid`

*Typography*
`heading-style-h1`, `heading-style-h2`, `heading-style-h3`, `heading-style-h4`, `heading-style-h5`, `heading-style-h6`, `text-size-xlarge`, `text-size-large`, `text-size-regular`, `text-size-small`, `eyebrow-text`, `button-text`, `red-pill`

*Buttons / interactive*
`button-primary`, `button-secondary`, `button-group`, `button_circle-block`, `button_circle-inner-block`, `link_icon-embed`, `action_button`, `actions_button-wrapper`, `slider-arrow`, `arrow_embed`, `footer-link`, `footer_social-block`, `nav-link`, `toggle_trigger`

*Components / cards*
`audience_card`, `audience_card-content`, `inner_audience-wrapper`, `care_service-card`, `care_image-wrapper`, `services_list-card`, `service_list-wrapper`, `why_medicini-card`, `why_medicini-image-wrapper`, `kompakt_card`, `kompakt_card-bg`, `kompakt_icon`, `kompakt_info-wrapper`, `loc_card`, `loc_upper-wrapper`, `testimonial_card`, `testimonialupper-wrapper`, `testimoniallower-wrapper`, `support_card`, `cta_card`, `cta_card-upper-wrapper`, `faq_wrapper`, `faq_upper-wrapper`, `faq_lower-wrapper`, `faq_outer-wrapper`, `staff_list`, `staff_list-item`, `staff_wrapper`, `hero_benefit-item`, `hero_desc-wrapper`, `index_wrapper`

*Icons / media*
`image`, `image_overlay`, `absolute-bg`, `embed-icon`, `icon`, `icon_20px`, `icon_21px`, `icon_24px`, `icon_28px`, `icon_32px`, `icon_36px`, `red_circle`, `horizontal_line`, `verticle_line`, `care_horizontal-line`, `care_verticle-line`, `faq_horizontal-line`, `faq_verticle-line`, `grey_radial-ellipse`, `blue_radial-ellipse`, `section_overlay`, `badges_wrapper`, `support_bg`, `hero_staff_bg`

*Utility / modifier (combo classes)*
`is_home-hero`, `is_large`, `is_medium`, `is_xlarge`, `is_services`, `is_footer`, `is_locations`, `is_care-givers`, `is_why-medicini`, `is_kompakt`, `is_faq`, `is_cta`, `is_audience`, `is_testimonial`, `is_benefits`, `is_support`, `is_applications`, `is_nursing-staff`, `is_badges`, `is_grid`, `is_marquee`, `is_index`, `is_insta`, `is_radius-small`, `is-cover`, `is-1`, `is-2`, `v2`, `weight-600`, `text-color-liver-grey`, `margin_top-2.5em`, `margin-top-8px`, `max-wd-33em`, `max-wd-41em`, `display-none`
(Note the inconsistency: `margin_top-2.5em` → selector `.margin_top-2-5em` is a **combo** on `.button-group`, while `margin-top-8px` is a **global** class — and `margin-top-8px` actually sets `0.5em`, not 8px.)

*State classes*: none beyond Webflow's own (`w--current`, dropdown open). Hover behaviour lives in IX3 + three CSS `:hover` blocks (see below).

*Third-party / vendor classes*: `swiper1`, `swiper-wrapper`, `swiper-slide`, `swiper-cover`, `basic-slider-list-blog`, `basic-swiperr-item` (sic), `button-next`, `button-prev`, `marquee_horizontal`, `marquee_horizontal-css`, `track-horizontal` (Osmo-style CSS marquee).

### Homepage DOM skeleton: body → hero
```
Body
  Block .page-wrapper
    ComponentInstance <Global Styles>              ← HtmlEmbed with the global <style>
    Block[main] .main-wrapper
      Block[section] .section-hero.is_home-hero
        NavbarWrapper .navigation
          Block .nav_container
            Block .nav_links-outer
              NavbarBrand .nav_brand-logo
                Image .image
              NavbarMenu .nav_menu
                Block .nav_links-wrapper
                  DropdownWrapper .nav_dropdown
                    DropdownToggle .nav_dropdown-toggle
                      Block .toggle_text
                      Block .toggle_trigger
                        Block .horizontal_line
                        Block .verticle_line
                    DropdownList
                      DropdownLink ×3
                  DropdownWrapper .nav_dropdown          (2nd dropdown, same shape)
                  NavbarLink .nav-link ×3
            Block .nav_menu-wrapper
              ComponentInstance <Button Primary>
              NavbarButton
                Icon
        Block .padding-global
          Block .main-container
            Block .section-padding-verticle.is_home-hero
              Block .content-wrapper.is_home-hero
                Block .content-left-wrapper.is_home-hero
                  Block .section-title-wrapper.is_home-hero
                    ComponentInstance <Section Eyebrow>
                    Heading[h1] .heading-style-h1
                    Block .hero_desc-wrapper
                      Paragraph .text-size-large.text-color-liver-grey
                  Block .button-group
                    ComponentInstance <Button Primary>
                    ComponentInstance <Button Secondary>
                Block .content-right-wrapper.is_home-hero        ← the glass "stats" card
                  Block .hero_benefit-item ×5
                    Block .icon_28px
                      HtmlEmbed .embed-icon
                    Paragraph .text-size-large.is_benefits
                Block .actions_button-wrapper                     ← the 3 FABs
                  Link .action_button ×3
                    Block .icon_36px
                      HtmlEmbed .embed-icon
        Block .absolute-bg
          Image .image.is-cover
        Block .section_overlay
```

**The canonical section skeleton** (repeated verbatim for `section_audience`, `section_care-givers`, `section_locations`, `section_why-medicini`, `section_kompakt`, `section_faq`, `section_cta`, `section_footer`):
`Block[section] .section_<name>` → `.padding-global` → `.main-container` → `.section-padding-verticle.is_<size|name>` → `.content-wrapper.is_<name>` → …

### Representative card-grid section (care-givers)
```
Block[section] .section_care-givers
  Block .padding-global
    Block .main-container
      Block .section-padding-verticle.is_services
        Block .content-wrapper.is_care-givers
          Block .section-title-wrapper.is_care-givers
            ComponentInstance <Section Eyebrow>
            Heading[h2] .heading-style-h2
            Block .margin-top-8px
              Paragraph .text-size-large.text-color-liver-grey
          Block .flex-wrapper.is_care-givers
            Block .grid.is_care-givers                 ← grid-template-columns: 1fr 1fr 1fr
              Block .care_service-card ×6
                Block .care_image-wrapper
                  Image .image.is-cover.is_radius-small
                  Block .image_overlay
                Block .service_list-wrapper
                  Heading[h3] .heading-style-h4
            Block .services_list-wrapper               ← the "Health Care Plus" link list
              Link .services_list-card ×12
                Block .service_list-wrapper
                  Block .health_care_plus-wrapper
                    Block .care_horizontal-line
                    Block .care_verticle-line
                  Heading[h4] .heading-style-h4
                Block .icon_20px
                  HtmlEmbed .embed-icon
  Block .grey_radial-ellipse
  HtmlEmbed .display-none                              ← parked/disabled embed
```

### Footer
```
Block[section] .section_footer
  Block .padding-global
    Block .main-container
      Block .section-padding-verticle.is_footer
        Block .content-wrapper.is_footer
          Block .flex-wrapper.is_footer
            Link .footer_brand
              Image .image
            Block .footer_menu
              Link .footer-link ×5
            Block .footer_social-outer-wrapper
              Link .footer_social-block
                Block .icon_21px
                  HtmlEmbed .embed-icon
              Link .footer_social-block
                Block .icon_21px.is_insta
                  HtmlEmbed .embed-icon
          Paragraph .text-size-small
```
The footer is **not** a Webflow component — it is plain elements repeated per page (Figma had it as a component instance).

### Navbar
- Built on Webflow's native **Navbar** element set (`NavbarWrapper` / `NavbarBrand` / `NavbarMenu` / `NavbarLink` / `NavbarButton`), not a custom div nav.
- `.navigation` is transparent (`background-color: var(--transparent)`); the visible "pill" is `.nav_links-outer` — `padding: .5em 1.5em; gap: 4em; border: 2px solid var(--white-opacity-30); border-radius: 6.25em; background-color: var(--geyser); backdrop-filter: blur(5px)`.
- `.nav_container { display:flex; width:100%; max-width:107.5em; margin-inline:auto; padding-block:1.5em; justify-content:space-between; align-items:center }` — i.e. the navbar re-declares the container instead of reusing `.main-container`.
- Two `DropdownWrapper .nav_dropdown` menus, each with a hand-built `+`/`×` toggle made of two 1 px divs (`.horizontal_line` 0.88em × 1px, `.verticle_line` 1px × 0.88em) inside `.toggle_trigger` (1.25em box). No icon font, no SVG.
- **Mobile menu: not implemented.** The `NavbarButton` (hamburger) exists with a default `Icon` child, but no `.w-nav-overlay` styling, no mobile-breakpoint styles on `.nav_menu` (which has an empty style block), and Figma's `Mobile menu / Expanded` frame has no Webflow counterpart.
- `.nav_brand-logo { width:7em; min-width:7em }` with a `.v2` combo at `9em` and `.nav_links-outer.v2 { padding-right:0.5em }` — the `v2` combos are the Personal-Plus header variant from the 15 Sep Figma revision.

### Buttons
Two Webflow **components**, plus link-styled cards.

`Button Primary` (component `ff2ec39b-…`, 11 instances, props: `Variant` (variant), `Link` (link, default `#`), `Text` (textContent, default `"Für Pflegekräfte"`); variants: `Base`, `Blue Gradient`, `Small`):
```
Link .button-primary
  Block .button-text
  Block .button_circle-block
    Block .button_circle-inner-block
      HtmlEmbed .link_icon-embed.is-1
      HtmlEmbed .link_icon-embed.is-2     ← two arrow SVGs for the swap-on-hover effect
```
```css
.button-primary{display:flex;padding:.5em .5em .5em 1.5em;justify-content:space-between;align-items:center;
  grid-column-gap:1em;grid-row-gap:1em;border-radius:6.25em;background-color:var(--white);text-decoration:none}
.button-text{color:var(--mirage);font-size:1.13em;line-height:1.8;font-weight:600;letter-spacing:-.36px}
.button_circle-block{display:flex;width:2.5em;height:2.5em;min-width:2.5em;min-height:2.5em;
  justify-content:center;align-items:center;flex:0 0 auto;border-radius:100%;background-image:var(--light-coral)}
```
`Button Secondary` (component `b4c7208c-…`, 5 instances, props `Link`, `Text`; only a Base variant):
```css
.button-secondary{display:flex;padding:.5em 1em .5em 1.5em;justify-content:space-between;align-items:center;
  grid-column-gap:1em;grid-row-gap:1em;border-radius:6.25em;background-color:var(--white-opacity-30);
  box-shadow:inset 1px -1px 2px 0 hsla(0,0%,100%,.7), inset 10px 0 20px -5px hsla(309.77,41.75%,20.2%,.07), inset -1px 1px 2px 0 white;
  backdrop-filter:blur(5px);text-decoration:none}
```
**Hover** is not CSS — it is two site-scoped IX3 interactions targeting the style blocks:
- `i-59983446` "Primary Button Hover" → `wf:hover` mouseenter (play) / mouseleave (reverse) on `wf:class` `6763f79e-…` = `.button-primary`, timeline `t-b833102a`.
- `i-1edb37b3` "Secondary Button Hover" → same shape on `.button-secondary`, timeline `t-e51a34a4`.

The only CSS `:hover` rules on the whole site are three, all identical in intent (fill turns red-coral, text/icon turns white):
```css
.footer-link:hover, .footer_social-block:hover, .slider-arrow:hover{
  border-color:var(--light-coral); background-color:var(--transparent);
  background-image:var(--light-coral); color:var(--white)}
```

### Variables in Webflow
**One collection: `colors collection`** (`collection-e4ecf511-045b-4cba-1651-a4c61ad43aa8`), one mode (`Base mode`), 20 variables. No size, number, percentage or font-family variables at all.

| name | cssName | value | maps to Figma |
|---|---|---|---|
| `white` | `--white` | `white` | `White #FFFFFF` |
| `black` | `--black` | `black` | – |
| `transparent` | `--transparent` | `transparent` | – |
| `white lilac` | `--white-lilac` | `#f7f7f7` | `Background Grey #F7F7F7` |
| `mirage` | `--mirage` | `#1a1a1a` | `Text Black #1A1A1A` |
| `liver grey` | `--liver-grey` | `#4d4d4d` | `Text Paragraph #4D4D4D` |
| `bright red` | `--bright-red` | `#eb0000` | `Red #EB0000` |
| `light coral` | `--light-coral` | `#ff7070` | top stop of `Red gradient` |
| `bright red (opacity 10%)` | `--bright-red-opacity-10` | `rgba(235,0,0,0.1)` | – |
| `rock blue` | `--rock-blue` | `#94b5ea` | ≈ `Blue #85ABE7` (**changed**) |
| `pale aqua` | `--pale-aqua` | `#c4d7ed` | ≈ `Light Blue #B9D0F2` (**changed**) |
| `jagged ice` | `--jagged-ice` | `#d4e2f2` | – |
| `ceramic` | `--ceramic` | `#fefeff` | – |
| `geyser` | `--geyser` | `#e0e0e0` | – |
| `white (opacity 10/30/40/50/70%)` | `--white-opacity-10/-30/-40/-50/-70` | `rgba/hsla` | the glass fills |
| `black (opacity 10%)` | `--black-opacity-10` | `rgba(0,0,0,0.1)` | `Diving line 1` at 10% |

Naming is **colour-name-based (name-that-color style: "mirage", "liver grey", "jagged ice", "geyser")**, *not* semantic and *not* matching the Figma token names. `Dark Purple #491E42` was dropped entirely (it survives only inside one hard-coded `box-shadow` on `.button-secondary`).

### Typography — tag → class → CSS
Base:
```css
body{background-color:var(--white-lilac);font-family:Manrope;color:var(--mirage);
     font-size:14px;line-height:1.4;font-weight:400}
```
…but that `14px` is overridden by the Global Styles embed (see next section), so the real root size is `0.8333vw`.

| Webflow class | tag used with | CSS (main breakpoint) | px @1920 | Figma token |
|---|---|---|---|---|
| `.heading-style-h1` | `h1` | `color:var(--mirage);font-size:5em;line-height:1.1;font-weight:500;letter-spacing:-2.5px` | 80 / 88 | `Desktop_H1` 80/92 |
| `.heading-style-h2` | `h2` | `font-size:4em;line-height:1.1;font-weight:500;letter-spacing:-2.4px` | 64 / 70.4 | `Desktop_H2` 64/76 |
| `.heading-style-h3` | `h3` | `font-size:2.75em;line-height:1.3;font-weight:500;letter-spacing:-1.32px` | 44 / 57.2 | `Desktop_H3` 44/56 |
| `.heading-style-h4` | `h2`,`h3`,`h4` | `font-size:2em;line-height:1.3;font-weight:500;letter-spacing:-0.64px` | 32 / 41.6 | `Desktop_H4` 32/40 |
| `.heading-style-h5` | – | `font-size:1.75em;line-height:1.4;font-weight:500;letter-spacing:-0.56px` | 28 / 39.2 | (no `Desktop` 28 token) |
| `.heading-style-h6` | `h3`,`h4` | `font-size:1.5em;line-height:1.4;font-weight:500;letter-spacing:-0.3px` | 24 / 33.6 | `Desktop_H5` 24/32 |
| `.text-size-xlarge` | – | `font-size:1.75em;line-height:1.4;font-weight:500` | 28 | – |
| `.text-size-large` | `p` | `font-size:1.25em;font-weight:500;letter-spacing:-0.4px` | 20 | `Desktop_Subtitle` 20/28 |
| `.text-size-regular` | `p` | `color:var(--liver-grey);font-size:1.13em;font-weight:500;letter-spacing:-0.18px` | 18 | `Desktop_Paragraph large` 18/24 |
| `.text-size-small` | `p` | `font-size:1em;line-height:1.6;font-weight:400;letter-spacing:-0.2px` | 16 | `Desktop_Paragraph small` 16/20 |
| `.eyebrow-text` | div | `font-size:1.25em;font-weight:600;letter-spacing:-0.4px` | 20 | `Desktop_Eyebrow` 20/28 ✔ |
| `.button-text` | div | `font-size:1.13em;line-height:1.8;font-weight:600;letter-spacing:-0.36px` | 18 / 32.4 | `Desktop_Button` 18/24 |
| `.nav-link` | a | `font-size:1em;line-height:1.6;font-weight:400;letter-spacing:-0.32px` | 16 | `Desktop_Button small` 16/24 (weight **400 vs 600**) |

**Per-breakpoint typography: there is none.** No heading or text class carries a `medium`, `small` or `tiny` override. See §4.

### Spacing system
```css
.padding-global{padding-left:2.5em;padding-right:2.5em}          /* 40px @1920 */
.main-container{width:100%;max-width:107.5em;margin-inline:auto} /* 1720px @1920 */
.nav_container{max-width:107.5em;padding-block:1.5em}            /* 24px */
```
Vertical rhythm — `.section-padding-verticle` itself is empty; all padding lives on its combos:
| combo | padding-top | padding-bottom | px @1920 |
|---|---|---|---|
| `.is_xlarge` | `10em` | `10em` | 160 / 160 |
| `.is_large` | `8.75em` | `8.75em` | 140 / 140 |
| `.is_medium` | `8.13em` | `8.13em` | 130 / 130 |
| `.is_services` | `8.75em` | `17.5em` | 140 / 280 |
| `.is_home-hero` | `9.38em` | `2.56em` | 150 / 41 |
| `.is_footer` | `9.38em` | `1.5em` | 150 / 24 |
| `.is_locations` | `0em` | `8.75em` | 0 / 140 |
| `.is_nursing-staff` | `4.63em` | `4.19em` | 74 / 67 |

Gaps are set per-context via `grid-column-gap`/`grid-row-gap` (Webflow's flex-gap pair), always in `em`: `0.31em`, `0.5em`, `0.7em`, `0.75em`, `1em`, `1.13em`, `1.5em`, `2em`, `2.5em`, `3em`, `3.5em`, `4em`, `6.5em`. Card padding is almost always `1.5em`, `2em` or `2.5em`; card radius is almost always `1.5em` (24px), with `1.25em` for images (`is_radius-small`) and `6.25em`/`100%` for pills and circles.

**Everything is `em`, nothing is `rem`, nothing is `px` except hairlines (1px/2px borders) and letter-spacing.**

### Responsive behaviour
Webflow breakpoints customised: **effectively none.** Out of 230 style blocks, the entire set of non-desktop overrides is:

| bp | selector | properties |
|---|---|---|
| medium (≤991) | `.nav-wrapper` | `margin-top:3em` |
| medium | `.slider-arrow` | `top:100%; margin-top:48px` |
| medium | `.slider-arrow.button-next` / `.button-prev` | `margin-top:0px` |
| medium | `.marquee_horizontal` | `height:22.6em` |
| medium | `.track-horizontal` | `gap:1em` |
| small (≤767) | `.nav-wrapper` | `margin-top:2.5em; gap:0.7em` |
| small | `.icon` | `width:1.25em` |
| small | `.track-horizontal` | `gap:1.5em` |
| small | `.swiper-cover` | `overflow:hidden` |
| tiny (≤479) | `.nav-wrapper` | `margin-top:2em` |
| tiny | `.track-horizontal` | `gap:1.13em` |

Instead, **all "responsiveness" so far is fluid scaling** driven by the Global Styles embed: the root font-size is `0.8333vw`, so every `em` value scales linearly with the viewport from 0 → 1920 px, then locks at 16 px above 1920 px and at 16 px below 991 px. Below 991 px that means the desktop layout renders at desktop `em` sizes in a narrow window — i.e. tablet/mobile is **not done yet**; the Figma Tablet and Mobile frames have not been implemented.

### Components (Webflow)
| component | id | instances | props | variants |
|---|---|---|---|---|
| `Global Styles` | `8787658e-…` | 2 (one per page) | – | Base |
| `Button Primary` | `ff2ec39b-…` | 11 | `Variant` (variant), `Link` (link, default `url:#`), `Text` (textContent, default `"Für Pflegekräfte"`) | `Base`, `Blue Gradient`, `Small` |
| `Button Secondary` | `b4c7208c-…` | 5 | `Link`, `Text` (default `"Für Pflegekräfte"`) | `Base` |
| `Section Eyebrow` | `c69438e7-…` | 17 | `Variant`, `Text` (textContent, default `"Personaldienstleister in der Pflege"`) | `Base`, `Light Pink` |

Only 4 components. Cards, nav, footer, FAQ items and testimonial slides are **not** componentised — they are copy-pasted element trees sharing classes. The `Global Styles` component is a single `HtmlEmbed` dropped as the first child of `.page-wrapper` on every page — this is the team's mechanism for site-wide CSS.

### CMS
**No collections at all.** Every list (services, locations, FAQ topics, testimonials, care-giver cards) is hand-built static markup.

### Interactions / animations
- **Webflow IX3**: 2 interactions only, both site-scoped hover interactions on the two button classes (see Buttons above).
- **Lenis 1.2.3** smooth scroll — site footer code (see below).
- **Swiper 8** — testimonial carousel (page footer code).
- **CSS marquee** — `.marquee_horizontal` / `.marquee_horizontal-css` / `.track-horizontal` / `.care_service-card.is_marquee` (`width:35.13em;min-width:35.13em`). The `-css` class is empty at the Designer level, so the animation itself is presumably in a `display-none` HtmlEmbed found at the end of `section_care-givers`.
- No Lottie, no GSAP.

### Custom code (verbatim)
**Site → footer:**
```html
<link rel="stylesheet" href="https://unpkg.com/lenis@1.2.3/dist/lenis.css">
<script src="https://unpkg.com/lenis@1.2.3/dist/lenis.min.js"></script>
<script>
let lenis = new Lenis({ lerp: 0.1, wheelMultiplier: 0.7, gestureOrientation: "vertical",
  normalizeWheel: false, smoothTouch: false });
function raf(time){ lenis.raf(time); requestAnimationFrame(raf); }
requestAnimationFrame(raf);
$("[data-lenis-start]").on("click", function(){ lenis.start(); });
$("[data-lenis-stop]").on("click", function(){ lenis.stop(); });
$("[data-lenis-toggle]").on("click", function(){ $(this).toggleClass("stop-scroll");
  if ($(this).hasClass("stop-scroll")) { lenis.stop(); } else { lenis.start(); } });
</script>
```
Site → head: empty. Registered (Apps-API) scripts: none.

**Home page → head:**
```html
<style>
.swiper1 .swiper-wrapper { transition-timing-function: cubic-bezier(0.22, 1, 0.36, 1) !important; }
</style>
```
**Home page → footer:**
```html
<script src="https://unpkg.com/swiper@8/swiper-bundle.min.js"></script>
<script>
const swiper1 = new Swiper(".swiper1", {
  direction: "horizontal", loop: true, slidesPerView: 1, slidesPerGroup: 1, spaceBetween: 16,
  loop: false, centeredSlides: false, mousewheel: { forceToAxis: true }, speed: 1200,
  breakpoints: { 480: { slidesPerView: 1 }, 768: { slidesPerView: 1.7 }, 992: { slidesPerView: 2 } },
  navigation: { nextEl: ".button-next", prevEl: ".button-prev" }
});
</script>
```
**`Global Styles` component embed (the most important piece of code on the site):**
```html
<style>
body { font-size: 0.8333333333333334vw; }
/* Max Font Size */
@media screen and (min-width:1920px) { body {font-size: 16px;} }
/* Container Max Width */
.container { max-width: 1920px; }
/* Min Font Size */
@media screen and (max-width:991px) { body {font-size: 16px;} }
body { -webkit-font-smoothing: antialiased; -moz-osx-font-smoothing: grayscale; font-smooth: always; }
</style>
```
(`0.8333333vw` = 16/1920 × 100 — so **1em = 1px of the Figma 1920 frame × 16**; put plainly, an `em` value in Webflow equals the Figma px value ÷ 16.)

### Fonts
No custom fonts uploaded (`list_fonts` → 0). **Manrope is pulled from Google Fonts** through Webflow's font manager; `body{font-family:Manrope}` with weights 400/500/600 in use.

### Images
- 21 assets, all at the site root — **no asset folders**.
- Names are the raw Figma layer names, spaces and all: `Background.png`, `Rectangle 16.png`, `Rectangle 17.png`, `image 112.png`, `map image.png`, `card 1.png`, `card 3.png`, `card.png`, `image.png`, `image (1).png` … `image (7).png`, `badge 1.png`, `badge 2.png`, `logo.png`, `happiness-young-white-men-race-adult 1.png` (a stock-photo filename left untouched), `back (1).webp`.
- Formats: 19 × PNG, 1 × WebP. Several are heavy (`Background.png` 2.4 MB, `Rectangle 16.png` 1.8 MB, `happiness-…png` 1.6 MB). Webflow auto-generated `-p-500/-p-800/-p-1080/-p-1600` responsive variants.
- **`altText` is `null` on every single asset.** No alt-text convention in place.
- **All icons are inline SVG inside `HtmlEmbed` elements**, never `<img>`: the pattern is a sized wrapper div (`.icon_20px` … `.icon_36px`, all `display:flex;justify-content:center;align-items:center;flex:0 0 auto` with `width` = px/16 em) containing `HtmlEmbed .embed-icon` (`display:flex;width:100%;height:100%;justify-content:center;align-items:center`).
- Photos use `Image .image.is-cover` (`width:100%;height:100%;object-fit:cover`), usually inside `.absolute-bg` (`position:absolute;inset:0;width:100%;height:100%;pointer-events:none`) or a fixed-height wrapper like `.care_image-wrapper` (`height:21.25em`).

### Forms, links, SEO
- **Forms: none** on either page (no `FormForm` element in the tree). The Figma contact page was not built.
- Links: buttons default to `url: "#"` via the component prop; footer links and service cards are `Link` elements.
- SEO: Home has an **empty** `seo` object and no Open Graph image; the second page has only `seo.title` (auto-copied from the page name). No JSON-LD.

---

## 3. Figma → Webflow mapping observed

| Design concept (Figma) | How it was realised (Webflow) | Changed? |
|---|---|---|
| Page frame `Homepage` (1920 wide, inside `Website swicher 15.09 > Desktop`) | Page `Home` (`/`) | – |
| Frame `Medicini - Nursing staff and medical professionals` | Page `nursing-staff-and-medical-professionals` | – |
| 1920 frame width | `body{font-size:0.8333vw}` + everything in `em` ⇒ the design scales fluidly instead of being fixed | **Yes** – no fixed 1440/1280 container; the design is treated as a *ratio*, not a pixel spec |
| Content column `x=100 w=1720` in every section | `.padding-global{padding-inline:2.5em}` (40px) + `.main-container{max-width:107.5em}` (1720px) ⇒ content starts at exactly 100px | Exact match |
| Section frame `Serction 4` etc. | `Block[section] .section_<semantic-name>` with the fixed `.padding-global → .main-container → .section-padding-verticle.is_* → .content-wrapper.is_*` chain | **Yes** – positional Figma names were replaced with semantic Webflow names (`section_care-givers`, `section_kompakt`, `section_why-medicini`, `section_faq`, `section_cta`) |
| Vertical whitespace between sections (arbitrary px in Figma) | Quantised to a 4-step scale on `.section-padding-verticle`: `is_medium` 8.13em, `is_large` 8.75em, `is_xlarge` 10em, plus asymmetric one-offs (`is_services`, `is_home-hero`, `is_footer`, `is_locations`) | **Yes** – rounded onto a scale |
| Text variable `Desktop_H1` (80/92, ls −5%) | `.heading-style-h1{font-size:5em;line-height:1.1;letter-spacing:-2.5px}` → 80/88 | **Yes** – line-height re-expressed as a unitless ratio and rounded down (92 → 88); letter-spacing converted from % to px |
| `Desktop_H2` 64/76 | `.heading-style-h2` 4em / 1.1 → 64/70.4 | **Yes** (76 → 70.4) |
| `Desktop_H5` 24/32 | `.heading-style-h6` 1.5em / 1.4 → 24/33.6 | **Yes** – the *name* shifted one level (Figma H5 → Webflow h6 class) |
| `Desktop_Subtitle` 20 | `.text-size-large` 1.25em | Renamed by size-role, not by design-role |
| `Desktop_Paragraph large` 18 / `small` 16 | `.text-size-regular` 1.13em / `.text-size-small` 1em | Renamed |
| `Desktop_Button` 18/24 | `.button-text` 1.13em / **1.8** (→32.4px) | **Yes** – line-height inflated; a likely mistake |
| `Desktop_Button small` 16/24 SemiBold | `.nav-link` 1em/1.6 **weight 400** | **Yes** – weight dropped from 600 to 400 |
| Colour variable `Text Black #1A1A1A` | `--mirage #1a1a1a` | Renamed (name-that-color), value kept |
| `Text Paragraph #4D4D4D` | `--liver-grey #4d4d4d` | Renamed, value kept |
| `Red #EB0000` | `--bright-red #eb0000`; the gradient's light stop became `--light-coral #ff7070` | Value kept, gradient decomposed into two flat vars |
| `Blue #85ABE7` | `--rock-blue #94b5ea` | **Yes** – different hex |
| `Light Blue #B9D0F2` | `--pale-aqua #c4d7ed` | **Yes** – different hex |
| `Background Grey #F7F7F7` | `--white-lilac #f7f7f7`, applied as `body` background | Renamed, value kept |
| `Dark Purple #491E42` | dropped (survives only inside a `box-shadow` literal) | **Yes** – dropped |
| Figma component `Button / Primary / Large` | Webflow component `Button Primary` with `Text`/`Link`/`Variant` props | Slash-path name flattened; variants `Base`/`Blue Gradient`/`Small` |
| Figma component `Button / Primary / Small` | the `Small` variant of `Button Primary` | Merged into one component |
| Figma `Button link` (glass pill, no icon circle) | `Button Secondary` component | Renamed |
| Figma `eyebrow` instance (17 uses) | `Section Eyebrow` component (17 instances) — 1:1 count match | Renamed |
| Figma `Footer` component instance | plain repeated elements under `.section_footer` | **Yes** – de-componentised |
| Figma cards (`card 1`, `card 3`, `card`, `Topic 1..6`) | repeated element trees sharing `audience_card`, `care_service-card`, `kompakt_card`, `testimonial_card`, `loc_card`, `faq_wrapper` | **Yes** – not componentised, not CMS |
| Auto-layout **row** with gap | `display:flex` + `grid-column-gap`/`grid-row-gap` (Webflow always writes the pair) | – |
| Auto-layout **column** with gap | `display:flex;flex-direction:column;flex-wrap:nowrap` + gap pair | – |
| Multi-column card row (`Cards` frames) | `.grid` (`display:grid;grid-auto-columns:1fr;gap:1em;grid-template-columns:1fr 1fr`) + `is_*` combo redefining the columns (`is_care-givers` → `1fr 1fr 1fr`, `is_why-medicini` → `1fr 1fr 1fr 1fr`, `is_kompakt`) | – |
| Figma padding `24px` | `1.5em`; `40px` → `2.5em`; `32px` → `2em`; `16px` → `1em`; `8px` → `0.5em` | Direct ÷16 conversion |
| Figma radius `24px` | `1.5em`; `20px` → `1.25em`; `78px`/`100px`/`130px` pills → **all normalised to `6.25em` (100px)**; circles → `100%` | **Yes** – pill radii unified |
| Photos (PNG in Figma) | uploaded as PNG with the Figma layer name verbatim; `Image .image.is-cover` inside `.absolute-bg` or a fixed-height wrapper | alt text dropped |
| SVG icons | pasted as **inline SVG in an `HtmlEmbed .embed-icon`** inside a `.icon_<N>px` wrapper — never an `<img>` or Webflow Icon element | – |
| Hero FABs (3 × 72px gradient circles) | `.actions_button-wrapper` (`position:absolute;right:-5.25em;bottom:19.38em;width:4.5em`) with 3 × `Link .action_button` (72px = 4.5em) | Position re-anchored to the content column instead of the viewport |
| Hero glass "stats" card (5 rows) | `.content-right-wrapper.is_home-hero` (max-width 19.5em, padding 1.5em, radius 1.5em, 2px `--white-opacity-40` border, `--white-opacity-10` fill, `blur(5px)`) with 5 × `.hero_benefit-item` | Figma used `backdrop-blur-[45px]` / `bg-[rgba(255,255,255,0.3)]`; build uses `blur(5px)` / 10% white — **weaker glass** |
| Header: logo + `Personal` / `Personal Plus` switcher pill + centre nav pill + right CTA | Webflow Navbar: `.nav_links-outer` pill holds brand + menu, `.nav_menu-wrapper` holds the CTA; switcher realised through the `.v2` combos (`nav_brand-logo.v2`, `nav_links-outer.v2`) | **Yes** – restructured from 3 absolutely-positioned groups into one flex row |
| Figma `Plus` icon on nav links | hand-built `+` from `.horizontal_line` (0.88em × 1px) + `.verticle_line` (1px × 0.88em) inside `.toggle_trigger` | **Yes** – SVG replaced with divs so it can animate to `×` |
| Breakpoint frames `Homepage_Tablet` (834) / `Homepage_Mobile` (393) | **not implemented** — only 12 property overrides exist across medium/small/tiny | **Yes – dropped (so far)** |
| `Mobile menu / Default` + `Expanded` frames | not implemented (default Webflow NavbarButton only) | **Yes – dropped (so far)** |
| Testimonial carousel | Swiper 8 via page footer code, wrapper classes `swiper1` / `swiper-wrapper.basic-slider-list-blog` / `swiper-slide.basic-swiperr-item`, arrows `.slider-arrow.button-prev/.button-next` | Library choice is a build-time decision, not in the design |
| Smooth scroll (not in design) | Lenis 1.2.3 added site-wide | Added |
| Other 6 designed pages (`For Facility`, `About Us`, `Career`, `Contact Us`, `Treatment Cost Calculator`, `Contact Us Hero`) | not built yet | Pending |

---

## 4. Quality notes and gotchas

### Rules that look deliberate (repeat them on new projects)
1. **The fluid-root rule.** Every project starts with a `Global Styles` component — a lone `HtmlEmbed` as the first child of `.page-wrapper` — containing `body{font-size:0.8333333333333334vw}` plus a 1920 max clamp and a 991 min clamp. All subsequent sizing is in `em`, so an `em` value = Figma px ÷ 16. This is the single most distinctive convention here.
2. **The four-div section chain, without exception:** `section` → `.padding-global` → `.main-container` → `.section-padding-verticle.is_*` → `.content-wrapper.is_*`. Even `section_testimonial` follows it (minus `.padding-global`, because it bleeds full-width — see mistakes).
3. **Bare wrapper + `is_*` combo.** Six generic wrappers (`content-wrapper`, `content-left-wrapper`, `content-right-wrapper`, `section-title-wrapper`, `flex-wrapper`, `grid`, `section-padding-verticle`) carry almost no base CSS; every section gets its own `is_<section-name>` combo that supplies the flex/grid/gap/max-width. 86 of 230 classes are combos.
4. **Headings always carry a `heading-style-h*` class**, and the class is chosen for *size*, not for the tag — `h2.heading-style-h4`, `h3.heading-style-h4`, `h3.heading-style-h6`, `h4.heading-style-h6` are all present. Tag = document outline, class = visual scale.
5. **Paragraphs always carry a `text-size-*` class**, colour applied via a second combo (`.text-color-liver-grey`) rather than baked in.
6. **Icons are always** `Block .icon_<N>px` → `HtmlEmbed .embed-icon` → inline SVG. Sizes are 20/21/24/28/32/36 px, named literally.
7. **Photos are always** `Image .image.is-cover` (+ `.is_radius-small` when rounded), and full-bleed ones always sit inside `Block .absolute-bg`.
8. **Decorative glows are dedicated absolutely-positioned divs**: `.grey_radial-ellipse`, `.blue_radial-ellipse`, `.section_overlay`, `.image_overlay`, all with `pointer-events:none`.
9. **Pill radius is always `6.25em`**, card radius `1.5em`, image radius `1.25em`, circles `100%`.
10. **Button hover is an IX3 interaction targeting the class**, not a CSS `:hover` — so the hover survives every instance of the component.
11. **Buttons are components with `Text` + `Link` props**; the eyebrow is a component with a `Text` prop. Anything that repeats with only text changing becomes a component; anything with structural variation stays as duplicated markup.
12. Text is German throughout (`Für Pflegekräfte`, `Personaldienstleister in der Pflege`) and typed directly into elements — no localisation layer.

### Mistakes and inconsistencies (evidence, not judgement)
- **No responsive work.** 12 breakpoint overrides across 230 classes; Figma's Tablet (834) and Mobile (393) frames are unimplemented. Below 991 px the site will render the desktop layout at desktop `em` sizes.
- **Mobile menu missing** – `NavbarButton` exists but is unstyled and has no target.
- **`section_testimonial` skips `.padding-global`** (goes `section → .main-container → …`), breaking the otherwise-universal chain.
- **Typo class names, shipped:** `section-padding-vertic**le**` (should be "vertical"), `verticle_line`, `care_verticle-line`, `faq_verticle-line`, `testimonia**L**upper-wrapper` / `testimonia**L**lower-wrapper` (capital L mid-word; selectors render as `.testimonialupper-wrapper`), `basic-swipe**rr**-item`. These mirror the Figma typo "Serction".
- **`margin-top-8px` actually sets `0.5em`** (≈8px only at the 1920 reference) — a px-named class holding an em value.
- **Modifier separator is inconsistent**: mostly `is_foo`, but `is-cover`, `is-testimonial`, `is-1`, `is-2` use a hyphen. `.flex-wrapper.is_testimonial` does not exist; `.flex-wrapper.is-testimonial` does.
- **`.section-title-wrapper.is_care-givers` is reused on the FAQ section** instead of a dedicated `is_faq` combo.
- **`.heading-style-h5` (28px) is defined but never used** on the homepage; `.text-size-xlarge` likewise only appears via `.text-size-xlarge.weight-600`.
- **Every asset has `altText: null`** and no asset folders; filenames are raw Figma/stock exports (`happiness-young-white-men-race-adult 1.png`, `image (7).png`).
- **Heavy unoptimised PNGs**: 2.4 MB, 1.8 MB, 1.6 MB, 1.1 MB originals.
- **Home page SEO is empty** — no title, no description, no OG image.
- **A `HtmlEmbed .display-none`** is parked at the end of `section_care-givers` (probably the marquee keyframes, or dead code).
- Line-heights drift from the design (H1 92→88, H2 76→70.4, button 24→32.4) because they were re-expressed as unitless ratios (1.1 / 1.3 / 1.4 / 1.8) rather than computed.
- `.button-secondary` carries a hard-coded `hsla(309.77, 41.75%, 20.2%, 0.07)` — the only surviving trace of the `Dark Purple` token.

### Special features present
- **Smooth scroll** (Lenis) with `data-lenis-start` / `data-lenis-stop` / `data-lenis-toggle` hooks wired in the site footer.
- **Slider**: Swiper 8, one instance (`.swiper1`), custom easing `cubic-bezier(0.22,1,0.36,1)`, `slidesPerView` 1 / 1.7 / 2 at 480 / 768 / 992.
- **Marquee**: CSS-driven horizontal marquee (`.marquee_horizontal` 29.75em tall, `.track-horizontal` flex + gap, `.care_service-card.is_marquee` 35.13em wide) — this is the only place where medium/small/tiny overrides exist.
- **Accordion**: FAQ built from `.faq_wrapper` (`overflow:hidden; cursor:pointer`) + `.faq_upper-wrapper` / `.faq_trigger-wrapper` (`+`/`×` made of two 1px divs) + `.faq_lower-wrapper` / `.faq_spacer`. No IX3 interaction is registered for it — so the open/close is either unbuilt or handled by a script not yet present.
- **Dropdowns**: two native Webflow `DropdownWrapper` menus in the nav.
- **Glassmorphism everywhere**: `backdrop-filter: blur(5px|7px|8px|15px)` on eyebrow, secondary button, footer links, social blocks, audience card content, services list cards, support cards, nav pill.
- No RTL, no localisation (single German locale), no dark mode, no sticky elements (`.navigation` has no `position:fixed/sticky`), no modals, no video.

---

## 5. Verbatim dumps

Omitted from the skill copy; the full dump lives in the analysis workspace.

## 6. Tool quirks

**Egress / network (biggest blocker on this run)**
- `curl -sL https://medicini-plus.webflow.io/` → **exit 56**, agent proxy reports `connect_rejected … gateway answered 403 to CONNECT`. `WebFetch` on the same URL → `{"error_type":"EGRESS_BLOCKED","domain":"medicini-plus.webflow.io"}`. **`*.webflow.io` is blocked.** The brief's "fetch the live HTML + webflow.css" step is impossible in this environment — plan on getting everything through the Webflow MCP tools instead.
- `curl https://www.figma.com/api/mcp/asset/<id>.png` → **also blocked** (`www.figma.com:443 connect_rejected`). So **screenshots cannot be downloaded to the run folder.** `mcp__Figma__get_screenshot` returns only a URL by default. If you need to *see* a frame, call it with `enableBase64Response: true` (costly in context) — otherwise skip screenshots entirely and rely on `get_design_context`, which returns exact px/gap/colour values in Tailwind-ish form and is far more useful for this task.
- Proxy state check that works: `curl -sS "$HTTPS_PROXY/__agentproxy/status"`.

**Figma MCP**
- `mcp__Figma__get_metadata` with `fileKey` only (no `nodeId`) lists pages — here it returned one page, `57:5822 Draft`.
- `get_metadata` with `nodeId:"57:5822"` returned **645 KB and overflowed** to `/root/.claude/projects/.../tool-results/mcp-Figma-get_metadata-*.txt`. Format is a JSON array `[{type,text}]`; join the `text` fields to get the XML. Working recipe:
  ```bash
  python3 -c "import json;d=json.load(open(F));open('/tmp/figmeta.xml','w').write(''.join(x['text'] for x in d))"
  ```
  then walk it with an indentation-aware regex on `^(\s*)<(frame|section|text|instance|...) id="..." name="..." x=... width=... height=...`. This is *much* cheaper than repeated `get_metadata` calls per node.
- `get_variable_defs` returns the variables **used by that node's subtree**, not the whole file. Call it once on the desktop homepage frame and once on the mobile frame to get the full `Desktop_*` / `Tablet_*` / `Mobile_*` token set. Gradient variables come back with an empty string value.
- `get_design_context` worked first try with `nodeId` + `fileKey` + `clientFrameworks:"unknown"` + `clientLanguages:"html,css"` + `skillNames:"resource:figma-design-to-code"` + `excludeScreenshot:true`. Output for one hero section was ~9 KB of React/Tailwind — rich but expensive; budget 1–2 calls, not 6.
- `get_screenshot` `nodeId` pattern is stricter than `get_metadata`'s (`^\d+[:-]\d+$` — no instance `I…;…` ids).

**Webflow MCP**
- Every `data_*` call needs `agent_id`, `session_id`, `context`. Shape that worked on the first call: `agent_id:"fable-5.1|claude-code|m7k2p"`, `session_id:"start"`. The response's first text block is `session_id: ses_3JeKVfc6O9N1nRtfaX2gzb6WVga` — reuse that exact value on every later call.
- `data_style_tool`, `data_variable_tool`, `data_element_tool`, `data_component_tool` **all require both `siteId` AND `pageId` at the top level**, even for site-wide reads like "list all styles" or "list all variables". Pass the homepage id. `data_sites_tool`, `data_scripts_tool`, `data_fonts_tool`, `data_assets_tool`, `data_cms_tool` do **not** take `pageId`.
- **`data_style_tool > query_styles` with a single-segment `name_path` does NOT match combo classes.** `name_path:["section-padding-verticle"]` returns only the bare global class (empty properties). To read a combo you must pass the full chain: `name_path:["section-padding-verticle","is_large"]`. This is stated in the schema but very easy to miss and will make you think a site has no styles.
- `include_breakpoints` only returns breakpoints that actually carry overrides; a class with no tablet styles simply has no `medium` key. To audit responsiveness in one shot use:
  ```json
  {"get_styles":{"query":"all","include_properties":true,
    "include_breakpoints":["medium","small","tiny"],"include_base_pseudos":["hover"]}}
  ```
  (89 KB here → overflowed to a file; parse with python and filter on `properties.breakpoints`.)
- Colour values inside style properties come back as `{"id":"variable-…"}` references, not hex. You must join them against `data_variable_tool > get_variables` output yourself.
- `data_element_tool > get_all_elements` with `depth:-1` on the homepage returned **193 KB** and overflowed to a file. It is plain JSON on one line; `json.load` it and walk `children` recursively. Skip `type:"String"` nodes — they hold the text but bloat the tree. Element text is *not* on the parent node.
- To read a component's internals: `get_all_elements` with `scope_component_id:"<component id>"` (no `element_id` needed). Works headlessly.
- To read an `HtmlEmbed`'s code: `data_element_settings_tool > get_settings` with `type:"all_resolved_settings"`, `element_id:{component,element}` and `scope_component_id` when it lives inside a component. The code arrives under key `"code"`.
- **Rate limiting is real.** A single `data_scripts_tool` call with three actions returned `{"name":"WebflowApiError","message":"Too Many Requests","status":429,"code":"too_many_requests"}` for the *second* action only — the first and third succeeded. Retrying that one action ~2 minutes later worked. Keep multi-action batches to ≤2 network-heavy actions, and re-issue individual failed actions rather than the whole batch.
- `data_component_tool > get_all_components` with `options:{includeProps:true,includeInstanceCount:true,includeVariants:true}` is cheap and gives components + props + variants + instance counts in one call — do this early, it tells you what was componentised.
- `data_interactions_tool > list_interactions` needs `siteId`+`pageId` and returns IX3 triggers with `wf:class` targets keyed by **style-block id** — cross-reference against the style list to know which class is animated.
- `element_snapshot_tool` was not attempted (image egress is blocked anyway, so it would likely be useless here).
- `webflow_guide_tool` was not called — the condensed copy at `/home/user/talha-vercel-test/.claude/skills/figma-to-webflow/references/webflow-mcp-guide.md` exists and is accurate; read it instead.

**Budget observed:** ~24 tool calls total (2 blocked network attempts, 1 rate-limited retry). The expensive ones are `get_metadata` on the Figma page, `get_all_elements` depth −1, and `get_styles` with properties — all three overflow to files, which is fine and actually preferable: parse them locally with python instead of paging through the API.
