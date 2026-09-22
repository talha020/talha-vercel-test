# BookFlow (BookFlow AI) — Webflow Marketplace template

Figma: https://www.figma.com/design/28ujPpgb7t0r1ddfXKxLNz/BookFlow-Website--v1---final-  |  Webflow site id: `6a1af7418bee7b95cfb3a003` (shortName `bookflow-3b73f0`)  |  Live: https://bookflow-3b73f0.webflow.io/  |  Analysed: 2026-09-21

**Which Figma file was used:** the primary key `28ujPpgb7t0r1ddfXKxLNz` was accessible; the fallback `gswS2IKHrPcC26PAVpBBsd` was not needed.

**Evidence base / limitation:** the live staging host `bookflow-3b73f0.webflow.io` is blocked by this session's egress proxy (both `curl` and `WebFetch` — see §6). Everything below therefore comes from the Webflow Data API (element trees, style blocks with real CSS property values, variables, components, CMS schemas, interactions, custom code) and from Figma `get_metadata` / `get_variable_defs`. Class names, CSS values and element nesting are quoted verbatim from those APIs, which is the same data the published CSS is generated from. Where something is inference rather than observation it is marked **(guess)**.

---

## 1. Figma file anatomy

### Pages in the file
Only **two** pages exist (from `get_metadata` with `fileKey` only):

| id | name | what it holds |
|---|---|---|
| `0:1` | **Design Area** | Every page design, the Style Guide, Licenses and Changelog frames, plus one hidden archived duplicate of the home page. This is the "build from" page. |
| `25:2` | **Icon** | An icon-library contact sheet (`Container` → `Row` → `Icon` → `Title` + `icon`, 11536×5768 px) plus 11 extra frames that look like a working/archive area: a second `BookFlow AI - Home Page` (6335:5687, 1440×10764) and loose section frames `Pricing`, `Features`, `AI Features`, `Group 2147224122` at negative x. Treat this page as **dev/archive**, not a source of truth. |

There is **no separate "Components" page and no Figma component library for UI**.

### Top-level frames on "Design Area" (left→right by canvas x)

| node id | frame name | size | role |
|---|---|---|---|
| `6285:2542` | `BookFlow AI - Home Page` | 1440×10764, `hidden="true"` | archived duplicate of the home page |
| `6289:2237` | `Home page` | 1440×482 | small export frame, almost certainly the OG/social image **(guess)** |
| `99:242` | `BookFlow AI - Home Page` | 1440×10764 | **build-from source for `/`** |
| `231:5188` | `BookFlow AI - About Page` | 1440×5803 | → `/about` |
| `287:2039` | `BookFlow AI - Pricing Page` | 1440×6409 | → `/pricing` |
| `313:3983` | `BookFlow AI - Contact Page` | 1440×3192 | → `/contact` |
| `317:4139` | `BookFlow AI - Blog Page` | 1440×4039 | → `/blog` |
| `4006:1622` | `Blog Detail` | 1440×6796 | → Blog Posts Template |
| `4009:1004` | `404 Page` | 1440×739 | → `/404` |
| `4011:422` | `Style Guide` | 1441×2739 | → `/utility-pages/style-guide` |
| `4016:5782` | `Licenses` | 1441×2313 | → `/utility-pages/licenses` |
| `4016:6041` | `Changelog` | 1441×1632 | → `/utility-pages/changelog` |

**Frame naming pattern:** `BookFlow AI - <Page> Page` for the main marketing pages; bare nouns (`Blog Detail`, `404 Page`, `Style Guide`, `Licenses`, `Changelog`) for the rest. There is **no breakpoint suffix** anywhere.

**Breakpoints in Figma: one — 1440 desktop only.** No 991/768/479 frames exist for any page. All responsive behaviour was invented at build time in Webflow (see §3).

### Homepage section frames, verbatim, top to bottom (`99:242`)

```
Hero                (52:119)    y=0     1440×1121
AI Features         (74:2026)   y=1121  1440×1282
Problem Solution    (125:2053)  y=2403  1440×853
Features            (125:3183)  y=3256  1440×1042
How AI Works        (145:8040)  y=4298  1440×962
Integrations        (149:10226) y=5260  1440×1204
Use Cases           (173:13892) y=6464  1440×843
Testimonials        (173:13677) y=7307  1440×955
Pricing             (200:467)   y=8262  1440×1196
CTA                 (426:651)   y=9458  1440×800
Footer              (219:1520)  y=10258 1440×506
```
Interleaved with these, parented directly to the page frame, are 10 loose `rounded-rectangle` nodes named `Rectangle 121` … `Rectangle 130`, each **12×12 px at x=94 and x=1334**, sitting exactly on the section boundaries. These are the little corner dots of the "blueprint grid" motif; in Webflow they became the `borders_connector` / `is_top-left` / `is_bottom-right` divs (§2).

### The 1240 / 1200 grid frame
Every page frame contains a full-height empty frame named **`Frame 2147225077` at x≈100–101, width 1240** (e.g. `52:120` on Hero, `4016:6135` on Style Guide, `4016:6136` on Licenses, `4016:6137` on Changelog). Content frames sit at **x=120, width 1200**. This is the design's container system and it maps 1:1 onto Webflow (`section_borders` max-width `77.5em` = 1240px; `container-large` max-width `75em` = 1200px; `padding-global` = `6.25em` = 100px each side of 1440). This is the single cleanest Figma→Webflow correspondence in the project.

### Design tokens in Figma — almost none
`get_variable_defs(fileKey=28ujPpgb7t0r1ddfXKxLNz, nodeId=99:242)` returns only:

```json
{
  "shadow": "Effect(type: DROP_SHADOW, color: #5050501F, offset: (0, 4), radius: 24, spread: 0)",
  "Icon/Neutral/Strong": "#0a0d14"
}
```

There are **no colour variables, no text styles, no spacing/radius variables** bound on the homepage. The token system lives entirely in Webflow. The Figma "Style Guide" frame documents the palette as flat text labels instead:

| Figma label | hex |
|---|---|
| `Blue #1C62EF` | #1C62EF |
| `Black 1 #111111` | #111111 |
| `Black 2 #121212` | #121212 |
| `Black 3 #2A2A2A` | #2A2A2A |
| `Grey 1 #9A9BA3` | #9A9BA3 |
| `Grey 2 #7C7C7C` | #7C7C7C |
| `White #F4F4F4` | #F4F4F4 |
| `Gradient 1`, `Gradient 2`, `Gradient 3` | swatches, no hex written |

Type scale, also as text labels inside the Style Guide frame (`4011:562`, `4011:561`, `4012:620`):

```
Heading 1 / Bold / 62px
Heading 2 / Bold / 48px
Heading 3 / Bold / 36px
Heading 4 / Semibold / 24px
Heading 5 / Semibold / 20px
Heading 6 / Semibold / 24px      <- label looks wrong (same as H4); likely a copy-paste slip
Body Text / Large / 20px
Body Text / Regular / 16px
Body Text / Small / 14px
```

**Fonts named in the Figma style guide:** `Satoshi Variable` and `Inter Display` (frames `4011:498`, `4011:499`).

The style guide frame also documents **Colors**, **Typography** and **Buttons** with a one-paragraph rationale each, e.g.:
> "Typography defines how text appears and functions across the interface. It ensures readability, consistency, and visual hierarchy by using carefully selected fonts, sizes, and spacing."

Buttons in the guide are the two primitives only: `Try AI Scheduling Free` (primary, with a `Group 1` of three `Noise & Texture` rects behind it) and `Watch AI Demo` (secondary).

### Components in Figma
294 `<instance>` nodes exist, and **every one of them is an icon**, not a UI component. Names follow two conventions:
- **Tabler-style names:** `star-filled` (52), `circle-check-filled` (39), `check` (30), `x` (21), `flare` (13), `bolt` (12), `chevron-down` (10), `checkbox` (9), `avatar` (9), `player-play-filled` (5), `sparkles`, `puzzle-filled`, `mail-filled`, `clock`, `bell-ringing-filled` (4 each), plus singles like `shield-check-filled`, `headphones-filled`, `layout-kanban`.
- **Iconify `set:name` names**, used as raw frames for brand/integration logos: `logos:google-calendar`, `logos:google-meet`, `logos:microsoft-teams`, `logos:salesforce`, `logos:notion-icon`, `logos:linear-icon`, `devicon:slack`, `simple-icons:stripe`, `simple-icons:hubspot`, `cib:zapier`, `akar-icons:zoom-fill`, `meteor-icons:openai`, `ic:baseline-facebook`, `ic:baseline-discord`, `uil:linkedin`, `formkit:instagram`, `gridicons:user` (19×), `mingcute:robot-fill`, `solar:chart-bold`, `heroicons:users-16-solid`, `tabler:briefcase-filled`, `material-symbols:crown-rounded`, `fluent:brain-sparkle-20-filled`, `ri:palette-fill`, `streamline-plump:wallet-solid`, `mage:hospital-plus-fill`, `vscode-icons:file-type-outlook`.

Three non-icon instances exist: `button` (3), `Action Button` (3), `Job Title Selector` (2), `Pagination Icon` (2), `search-form` (1) — these are borrowed from a UI-kit library **(guess)**, not a BookFlow design system, because there is no component page in the file.

**Variants: none observed.** No `component-set` nodes appear in the metadata.

### Auto-layout usage
`get_metadata` does not expose layout mode, so this cannot be stated definitively. What is observable:
- Children sit at tidy, round offsets (content at x=120, gaps of 39/43/46px, 12×12 dots at exactly x=94/1334), which is consistent with auto-layout.
- Many wrappers carry Figma's *auto-generated* names (`Frame 2147225077`, `Frame 2147237546`, `Group 2147224139`), which is what you get when you wrap a selection in an auto-layout frame and never rename it.
- **Content max width: 1200px; grid band: 1240px; page: 1440px.** Section vertical rhythm from the Hero frame: nav band 65px tall at y=22, hero copy block at y=187, dashboard at y=553, trusted-teams strip at y=962, section ends at 1121.

### Asset types
- **Photos / UI renders:** the dashboard screenshot (`Home page (5) 1`, 1197×917 placed inside a 1200×343 mask), feature card images, team/author avatars.
- **Vector/SVG icons:** the 294 icon instances above.
- **Decorative geometry built natively in Figma, not as assets:** 4310 `ellipse` nodes and 3908 `rounded-rectangle` nodes — mostly the starfield (`Star, Round, 2.13`, `Star, Round, 2.93` … ~100 distinct names repeated 21–49 times each) and the `Noise & Texture` rounded rects (64 on the homepage) that sit behind every primary button.
- **Logos:** `Logomark wrapper` (3×) for the BookFlow mark; `Company Logo` / `Company logo` / `logos` frames for the trusted-teams strip.

---

## 2. Webflow site anatomy

### Pages (17 total, from `data_pages_tool > list_pages`)

| title | slug | published path | notes |
|---|---|---|---|
| Home | *(none)* | `/` | SEO: "BookFlow AI Powered Scheduling" |
| About | `about` | `/about` | |
| Pricing | `pricing` | `/pricing` | |
| Contact | `contact` | `/contact` | |
| Blog | `blog` | `/blog` | |
| Privacy Policy | `privacy-policy` | `/privacy-policy` | |
| Terms of Service | `terms-of-service` | `/terms-of-service` | |
| Cookie Policy | `cookie-policy` | `/cookie-policy` | |
| **Style Guide** | `style-guide` | `/utility-pages/style-guide` | in folder `6a555aa626c5f69f0ac9095f` |
| **Licenses** | `licenses` | `/utility-pages/licenses` | same folder |
| **Changelog** | `changelog` | `/utility-pages/changelog` | same folder |
| 404 | `404` | `/404` | SEO title "Not Found" |
| Password | `401` | `/401` | SEO title "Protected page" |
| Blog Posts Template | `detail_blog-posts` | `/blog-posts` | CMS `6a57e704fc6f1cdf8a4aa9ad` |
| Authors Template | `detail_authors` | `/authors` | CMS `6a591b15a4152c9dafac7d4a` |
| Categories Template | `detail_categories` | `/categories` | CMS `6a591b323e484c06e9addbea` |
| Image Licenses Template | `detail_image-license` | `/image-license` | CMS `6a6e01b4669054aa40900253` |

**Team rule visible here:** CMS template pages are named `<Collection> Template` and slugged `detail_<collection-slug>`. Utility pages live in a `utility-pages` folder, not at the root — exactly what Webflow Marketplace QA expects.

### Class naming convention — a **customised Client-First**, not stock Client-First and not Relume

667 style blocks: **391 base classes, 276 combo-class blocks (139 distinct combo names).**

The evidence that the base is Finsweet **Client-First**:
1. The `Global Styles` component is the verbatim Client-First global-styles embed (`.text-style-3lines`, `.hide-tablet`, `.spacing-clean`, `.margin-horizontal`, the "Leave this message for future hand-off" comment) — see *Custom code* below.
2. Client-First structural classes are present unchanged: `page-wrapper`, `main-wrapper`, `padding-global`, `container-large`, `padding-section-large/medium/small`, `max-width-large/medium/small/full`, `heading-style-h1…h6`, `text-size-large…tiny`, `text-weight-semibold`, `background-color-alternate`, `global-styles`, `margin-top`, `padding-vertical`.
3. One stray Client-First/Finsweet artefact survives in the class list: the combo name `fs-styleguide`.

The evidence that it was **customised**:
1. **Component classes use `_` where Client-First uses `_` only after a folder-ish prefix — here the prefix is the section, e.g. `section_hero`, `nav_container`, `footer_menu`, `features_card`, `pricing_tabs-menu`, `blog_post-card`.** That is Client-First's `folder_element` idea applied per section.
2. **Modifiers use `is_` with an underscore** (`is_home-hero`, `is_features`, `is_footer`), not Client-First's `is-`. Only two stragglers use the hyphen: `is-secondary`, `is-3rd`.
3. `container-medium` / `container-small` do not exist as classes (only in the global CSS embed); container sizing is done with variables instead.

**30+ verbatim examples, grouped**

*Layout / structure*
`page-wrapper`, `main-wrapper`, `padding-global`, `container-large`, `section_borders`, `section_divider`, `absolute_borders-wrapper`, `borders_connector`, `content-wrapper`, `content-left-wrapper`, `content-right-wrapper`, `inner_wrapper`, `relative_wrapper`, `flex_wrapper`, `grid`, `max-width-full`, `max-width-large`, `max-width-medium`, `max-width-small`, `max-width-custom`

*Sections (one class per section type)*
`section_hero`, `section_ai-features`, `section_features`, `section_solution`, `section_works`, `section_integrations`, `section_usecases`, `section_testimonials`, `section_pricing`, `section_cta`, `section_footer`, `section_trusted-teams`, `section_faq`, `section_team`, `section_story`, `section_stats`, `section_principles`, `section_comparison`, `section_difference`, `section_newsletter`, `section_blog-post`, `section_insights-detail`, `section_image-license`, `section_legal`, `section_styleguide`, `section_title-wrapper`, `section_pill`

*Spacing utilities*
`padding-section-large`, `padding-section-medium`, `padding-section-xmedium`, `padding-section-small`, `padding-section-tiny`, `padding-vertical`, `margin-top`, `margin_top-small`, `margin_top-tiny`, `gap_0.5em`, `gap_0.75em`, `gap_1em`

*Typography*
`heading-style-h1` … `heading-style-h6`, `text-size-xlarge`, `text-size-large`, `text-size-medium`, `text-size-regular`, `text-size-small`, `text-size-xsmall`, `text-size-tiny`, `text-size-xtiny`, `text-weight-semibold`, `is_weight-600`, `blockquote`, `details_rich-text`

*Buttons*
`button_primary`, `button_secondary`, `button-borders`, `button-bg`, `button-group`, `button-text`, `button_icon`, `button_icon-slide`, `button_slide-wrapper`, `button_text-wrapin`, `button_text-wrapout`, `button_text-relative`, `buton_text-absolute` *(typo, in production)*, `absolute_button`, `pagination-button`, `menu-button`

*Components / cards*
`features_card`, `ai_feature-card`, `insights_card`, `blog_post-card`, `testimonial_card`, `pricing_card`, `inner_pricing-card`, `usecase_card`, `works_card`, `team_card`, `stats_card`, `principles_card`, `differences_card`, `traditional_card`, `accordion_card`, `bookflow_card`, `contact_form-card`, `color-plate`

*Colour / background utilities*
`background-color-alternate`, `background-color-white-smoke`, `background-color-woodsmoke`, `background-color-cedar`, `background-color-cool-grey`, `background-color-medium-grey`, `background-color-secondary`, `background-gradient-primary`, `background-gradient-secondary`, `background-gradient-tertiary`, `text_color-blue`, `text_color-light-blue`, `text_color-pale-slate`, `text_color-stone`, `text-color-cinder`, `text-color-medium-grey`, `text-color-woodsmoke`

*State / position modifiers (all combo)*
`is_home`, `is_home-hero`, `is_about-hero`, `is_pricing-hero`, `is_contact-hero`, `is_license-hero`, `is_styleguide-hero`, `is_404`, `is_top`, `is_bottom`, `is_left`, `is_right`, `is_top-left`, `is_top-right`, `is_bottom-left`, `is_bottom-right`, `is_center-left`, `is_center-right`, `is_integration-top-left`, `is_integration-bottom-right`, `is_desktop-active`, `is_mobile-active`, `is_mobile-stretch`, `is_height-100%`, `is_height-180px`, `is_max-width-none`, `is_tilt`, `is_reverse`, `is_semi-bold`, `is_12px`, `is_gap-20px`, `is_next`, `is_previous`, `is_primary`, `is_secondary`(as `is-secondary`), `is_white`, `is_dark`, `is_blue`, `is_teal-blue`

### DOM skeleton — homepage, body → hero

```
Body
└ Block .page-wrapper                                   <div>
  ├ ComponentInstance  "Global Styles"                  (HtmlEmbed)
  ├ ComponentInstance  "Navigation"   prop Variant=base
  └ Block .main-wrapper                                  <main>
    ├ Block .section_hero .is_home                       <section>
    │ ├ Block .padding-global                            <div>
    │ │ └ Block .section_borders                         <div>
    │ │   ├ Block .container-large                       <div>
    │ │   │ └ Block .padding-section-tiny                <div>
    │ │   │   └ Block .content-wrapper .is_home-hero      <div>
    │ │   │     └ Block .home_hero-wrapper               <div>
    │ │   │       ├ Block .section_title-wrapper .is_home-hero
    │ │   │       │ ├ Block .gap_1em
    │ │   │       │ │ ├ Block .pill-borders            (the "AI-Powered scheduling" pill)
    │ │   │       │ │ ├ Heading h1 .heading-style-h1
    │ │   │       │ │ └ Block .max-width-full .is_home-hero   (the sub-paragraph)
    │ │   │       │ └ Block .card_info-wrapper
    │ │   │       │   ├ Block .button-group .is_home-hero     (2 button component instances)
    │ │   │       │   └ Paragraph .text-size-tiny             ("No credit card required")
    │ │   │       ├ Block .dashboard_image-wrapper
    │ │   │       │ └ Image .dashboard-image   alt="Hero Dashboard"  asset 6a68d3450fcc59da5fab25b5
    │ │   │       ├ Block .trusted_wrapper
    │ │   │       │ ├ Block .pill-borders  › Block .trusted_pill
    │ │   │       │ └ Paragraph .text-size-xtiny  "Powered by GPT-4"
    │ │   │       └ Block .mixing_overlay
    │ │   ├ Block .borders_connector .is_bottom-left
    │ │   ├ Block .borders_connector .is_bottom-right
    │ │   ├ Block .borders_connector .is_center-left
    │ │   └ Block .borders_connector .is_center-right
    │ └ Block .section_divider                           <div>
    ├ Block .section_trusted-teams                       <section>
    ├ Block .section_features                            <section>
    ├ Block .section_solution                            <section>
    ├ Block .section_ai-features                         <section>
    ├ Block .section_works                               <section>
    ├ Block .section_integrations                        <section>
    ├ Block .section_usecases                            <section>
    ├ Block .section_testimonials                        <section>
    ├ Block .section_pricing                             <section>
    ├ ComponentInstance  "CTA (Light)"
    └ ComponentInstance  "Footer"
```

**The universal section recipe (observed on every section of every page):**
```
section.section_<name>[.is_<page>]
└ div.padding-global
  └ div.section_borders                  (or container-large directly)
    └ div.container-large
      └ div.padding-section-<size>
        └ div.content-wrapper[.is_<section>]
```
plus, as later siblings of `.padding-global` inside the same section, a decorative stack:
```
div.absolute_borders-wrapper[.is_*]
└ div.section_borders.is_height-100%
  ├ div.borders_connector.is_top-left / .is_top-right / .is_bottom-left / .is_bottom-right
  ├ div.absolute_outer-divider[.is_right]
  └ div.absolute_inner-divider[.is_right | .is_tilt]
```
Two sections break the pattern deliberately and skip `padding-global`, using `container-large.is_xlarge` directly: `section_usecases` and `section_testimonials` (so their card rows can run wider than 1200px).

### Representative card-grid section (Features)

```
section.section_features
└ div.padding-global
  └ div.container-large
    └ div.padding-section-large
      └ div.content-wrapper.is_features
        ├ div.section_title-wrapper.is_features
        │ ├ div.section_pill
        │ │ ├ div.text-size-small.is_section-pill   "AI Features"
        │ │ ├ div.pill_icon › HtmlEmbed.icon_embed   (inline SVG)
        │ │ └ div.gradient-border
        │ ├ h2.heading-style-h2   "Meet your AI scheduling assistant"
        │ └ p.text-size-regular   "BookFlow AI doesn't just book meetings — it thinks ahead."
        └ div.grid.is_features                       (6 identical children)
          └ div.features_card
            ├ div.features_image
            │ └ img.image   alt="feature image"
            └ div.features_info-wrapper
              ├ div.features_upper-wrapper           (icon + h-level title)
              └ p.text-size-small                    (or div.max-width-small on card 3)
```

### Footer (component `Footer`, id `24552a96-57b4-fe14-4af5-552bd5746b41`, 10 instances)

```
footer.section_footer                                    <footer>
└ div.padding-section-medium.is_footer
  └ div.content-wrapper.is_footer
    ├ div.flex_wrapper.is_footer-menu
    │ ├ div.brand_outer-wrapper
    │ │ ├ a.footer_brand  (link → Home page)
    │ │ │ └ img.image  alt="brand logo"
    │ │ └ div.footer_social-wrapper
    │ │   ├ a.social_wrapper  href="//www.linkedin.com/"
    │ │   ├ a.social_wrapper  href="//discord.com/"
    │ │   ├ a.social_wrapper  href="//www.instagram.com/"
    │ │   └ a.social_wrapper  href="//www.facebook.com/"
    │ └ div.grid.is_footer                       (3 columns)
    │   └ div.footer_menu   ×3
    │     ├ p.text-size-large.is_semi-bold       (column heading)
    │     └ div.footer_nav-links-wrapper         (the a.footer_link items)
    ├ div.footer_divider
    └ div.flex_wrapper.is_copyrights
      ├ p.footer_link.is_copyright.is_desktop-active   "© 2026 BookFlow AI. All rights reserved."
      └ p.footer_link.is_copyright.is_mobile-active    "© 2026 BookFlow AI. All rights reserved."
```
Note the **duplicate-copy trick**: the copyright line exists twice, one shown on desktop and one on mobile via `is_desktop-active` / `is_mobile-active`, rather than restyling one element.

### Navbar (component `Navigation`, id `1e324cf3-550c-d338-c966-614281b5aaba`, 13 instances)

Built on **Webflow's native Navbar widget** (`NavbarWrapper` / `NavbarMenu` / `NavbarButton`), so the mobile menu is the stock Webflow interaction, not custom JS:

```
NavbarWrapper.navigation
└ div.padding-global
  └ div.nav_borders
    ├ div.nav_container                       (position: sticky)
    │ ├ div.nav_flex-left
    │ │ ├ NavbarBrand.nav_brand › img.nav_logo  alt="brand logo"
    │ │ └ NavbarMenu.nav_menu › div.nav_menu-inner  (a.nav_menu_link ×N)
    │ └ div.nav_flex-right
    │   ├ div.button-group.is_nav.is_desktop-active
    │   │ ├ ComponentInstance "Button Secondary"  Text="Watch AI Demo"      → /contact
    │   │ └ ComponentInstance "Button Primary"    Text="Try AI Scheduling Free" → /contact
    │   └ NavbarButton.menu-button › div.hamburger
    ├ div.borders_connector.is_bottom-left
    └ div.borders_connector.is_bottom-right
```

Mobile menu CSS (`medium` = ≤991px) turns `.nav_menu` into a full-width dropdown panel and `.nav_menu-inner` into a padded, rounded, white card:

```css
.nav_menu                { /* main: no properties */ }
@media (max-width:991px) .nav_menu {
  z-index:99; width:100%; margin-top:1em;
  padding-right:3em; padding-left:3em;
  background-color: var(--base-color--transparent);
}
@media (max-width:767px) .nav_menu { margin-top:.75em; padding-right:2em; padding-left:2em; }

@media (max-width:991px) .nav_menu-inner {
  padding:1.25em; flex-direction:column; align-items:flex-start;
  grid-column-gap:1em; grid-row-gap:1em;
  border-radius:1em; background-color: var(--text-color--white);
  box-shadow: inset 0 -1px 2px 1px rgba(0,0,0,.25);
}
```
The desktop CTA pair is hidden on mobile purely by the `is_desktop-active` combo.

### Buttons

Buttons are **components, not classes** — 5 of the 10 site components are buttons, and they account for 88 instances:

| component | instances | props | variants |
|---|---|---|---|
| `Button Primary` | **38** | Link, Text 1, Text 2, Image | base |
| `Button Secondary` | **41** | Variant, Link, Text 1, Text 2 | `base`, **`With Icon`** |
| `Button Primary (White)` | 3 | Link 1, Text 1, Text 2, Image | base |
| `Button Secondary (Dark)` | 3 | Variant, Link 2, Text 1/2/3 | `base`, **`Is Icon`** |

`Text 1` + `Text 2` always carry the *same* string — that is the hover text-swap rig (`button_text-wrapout` has `overflow:hidden`, with `button_text-wrapin` / `buton_text-absolute` sliding inside it).

```css
.button_primary {
  position:relative; display:flex; overflow:hidden;
  padding: .75em 1em;
  justify-content:center; align-items:center;
  border-radius: .75em;
  background-color: var(--base-color--royal-blue);       /* #1c62ef */
  background-image: linear-gradient(180deg, rgba(170,174,245,0), rgba(170,174,245,.6));
  box-shadow: inset 0 -1px 2px 1px rgba(0,0,0,.25),
              inset 0 1px 2px 1px rgba(255,255,255,.25),
              0 4px 12px 0 rgba(28,98,239,.3);
  text-align:center; text-decoration:none;
}
.button_secondary {
  position:relative; display:flex; overflow:hidden;
  padding: .75em 1em; border-radius:.75em;
  background-color: var(--text-color--soap-stone);        /* #fcfcfc */
  box-shadow: inset 0 -1px 2px 1px rgba(0,0,0,.25),
              inset 0 1px 2px 1px rgba(255,255,255,.25);
  transition: background-color 300ms ease;
}
.button_secondary:hover { background-color: var(--base-color--white-smoke); /* #f4f4f4 */ }

.button-borders {            /* 1px gradient ring wrapper around a button */
  padding:1px; border-radius:.75em;
  background-image: linear-gradient(180deg, #7aa3f6, #1c62ef);
}
.button-group { display:flex; flex-wrap:wrap; grid-column-gap:.5em; grid-row-gap:.5em; }
.button-text  { color: var(--text-color--soap-stone); font-size: var(--_responsive---text-size--small);
                line-height:1.4; font-weight:500; letter-spacing:-.3px; }
.button_icon  { display:none; width:.88em; }   /* revealed by the "With Icon" variant */
```
There is also a leftover **stock Webflow `.button`** class (`padding:.75rem 1.5rem; border-radius:.25rem; background:#1c62ef; font-weight:600`) that the team never deleted — it is in `rem`, unlike everything else in the project, so it is almost certainly unused **(guess)**.

### Variables in Webflow — two collections

`Colors` (48 variables, single `Base mode`) and `Responsive` (26 variables, **4 modes**: `Base mode`, `Tablet`, `Mobile Landscape`, `Mobile Portrait`).

**This is the centre of the whole system.** There are *no* per-breakpoint CSS overrides for type or spacing; the variables change value per mode, and Webflow attaches the modes to breakpoints. Verified: `.padding-section-large`, `.padding-global`, `.heading-style-h1…h6`, `.text-size-*` all have `properties` under `main` only, with the value being a variable reference.

Naming convention is `<Group>/<name>` with the group encoded as the CSS prefix:
- `- Base Color/<colour-name>` → `--base-color--<name>` (note the leading `- ` — a sort hack to keep the group first in the panel; one entry, `- Base Color /white smoke`, has a **stray space before the slash**)
- `Border Color/<name>` → `--border-color--<name>`
- `Text Color/<name>` → `--text-color--<name>`
- `Text Size/<Step>` → `--_responsive---text-size--<step>`
- `Heading Size/<Hn>` → `--_responsive---heading-size--<hn>`
- `Container/<Size>` → `--_responsive---container--<size>`
- `Padding Global/Padding Global`, `Padding Section/<Size>` → `--_responsive---padding-*`

Colour names are the **"name that hex" style** the team clearly standardised on: `royal blue`, `crystal blue`, `soft blue (10%)`, `white smoke`, `vista white`, `amour`, `mercury`, `jagged ice`, `corel blue`, `titan white`, `mirage`, `wood smoke`, `cinder`, `cool grey`, `davy grey`, `ironside grey`, `star dust`, `hit grey`, `silver chalice`, `pale slate`, `soap stone`, `cedar`, `charcoal`, `lavendar blue`, `lavendar mist`, `pearl white`, `pearl bush`, `dawn pink`, `soft peach`, `seashell`, `mareno`, `gainsboro`, `dove grey`, `french grey`, `pink sawan`, `warm grey`, `teal blue`, `pale blue`.

**Mapping back to the Figma palette:**

| Figma style-guide label | Webflow variable | value |
|---|---|---|
| `Blue #1C62EF` | `- Base Color/royal blue` | `#1c62ef` |
| `Black 1 #111111` | `Text Color/cinder` | `#111` |
| `Black 2 #121212` | `Text Color/wood smoke` | `#121212` |
| `Black 3 #2A2A2A` | `Text Color/cedar` | `#2a2a2a` |
| `Grey 1 #9A9BA3` | `Text Color/cool grey` | `#9a9ba3` |
| `Grey 2 #7C7C7C` | `Text Color/medium grey` | `#7c7c7c` |
| `White #F4F4F4` | `- Base Color /white smoke` | `#f4f4f4` |
| `Gradient 1/2/3` | no variable — hard-coded `linear-gradient(...)` on `.button-borders`, `.button_primary`, `.background-gradient-*` | — |

The other ~40 colour variables have **no counterpart in the Figma style guide**; they were derived at build time from picking colours off the comps.

### Typography — full table
`body { font-family: Interdisplay; font-size: 1em; line-height: 1.5; font-weight: 400; color: var(--text-color--black); background-color: var(--base-color--white-smoke); }`

Because of the fluid root size (next section), **1em = 16px at ≥1440px and at ≤991px**, and scales between.

| class | tag | weight | line-height | letter-spacing | size var | desktop | tablet ≤991 | mob-land ≤767 | mob-port ≤479 |
|---|---|---|---|---|---|---|---|---|---|
| `heading-style-h1` | h1 | 700 | 1.1 | −3.1px | `heading-size--h1` | 3.5em (56px) | 3.25em (52px) | 2.75em (44px) | 2em (32px) |
| *(alt)* | — | — | — | — | `heading-size--h1-100px` | 6.25em (100px) | 5em (80px) | 4em (64px) | 3em (48px) |
| `heading-style-h2` | h2 | 700 | 1.2 | −2.4px → **−1.5px @767, −1px @479** | `heading-size--h2` | 3em (48px) | 2.75em (44px) | 2.5em (40px) | 2em (32px) |
| `heading-style-h3` | h3 | 600 | 1.2 | −0.7px | `heading-size--h3` | 2.25em (36px) | 2.25em | 2em (32px) | 1.5em (24px) |
| `heading-style-h4` | h4 | 700 | 1.4 | — | `heading-size--h4` | 2em (32px) | 2em | 1.25em (20px) | 1.13em (18px) |
| `heading-style-h5` | h5 | 700 | 1.4 | — | `heading-size--h5` | 1.5em (24px) | 1.25em (20px) | 1.25em | 1.13em (18px) |
| `heading-style-h6` | h6 | 700 | 1.5 | — | `heading-size--h6` | 1.25em (20px) | 1.25em | 1em (16px) | 0.88em (14px) |
| `text-size-xlarge` | p/div | 600 | 1.2 | −0.5px | `text-size--xlarge` | 1.5em (24px) | 1.25em | 1.25em | 1.13em |
| `text-size-large` | p/div | 500 | 1.2 | −0.4px | `text-size--large` | 1.25em (20px) | 1.25em | 1.13em | 1em |
| `text-size-medium` | p/div | 600 | — | −0.4px | `text-size--medium` | 1.13em (18px) | 1.13em | 1em | 0.94em |
| `text-size-regular` | p | 400 | — | −0.3px | `text-size--regular` | 1em (16px) | 1em | 1em | 0.88em |
| `text-size-small` | p | 400 | — | −0.3px | `text-size--small` | 0.88em (14px) | 0.88em | 0.88em | 0.88em |
| `text-size-xsmall` | p/div | 500 | 1.2 | +0.4px | `text-size--xsmall` | 0.75em (12px) | 0.75em | 0.75em | 0.63em |
| `text-size-tiny` | p | 500 | 1.4 | `text-transform:uppercase` | `text-size--tiny` | 0.63em (10px) | 0.63em | 0.63em | 0.55em |
| `text-size-xtiny` | p | 600 | 1.4 | `text-transform:uppercase` | `text-size--xtiny` | 0.5em (8px) | 0.5em | 0.5em | 0.45em |

Default colours baked into the text classes: `heading-style-h2` and `text-size-medium`/`large` → `--text-color--wood-smoke` (#121212); `heading-style-h3` and `text-size-xlarge` → `--text-color--soap-stone` (#fcfcfc); `text-size-regular`/`small` → `--text-color--medium-grey` (#7c7c7c); `text-size-xsmall`/`tiny`/`xtiny` → `--text-color--star-dust` (#9c9c9c).

**Letter-spacing is the only typography property overridden per breakpoint**, and only on `heading-style-h2`.

### Spacing system

```css
.padding-global  { width:100%;
                   padding-right: var(--_responsive---padding-global--padding-global);
                   padding-left:  var(--_responsive---padding-global--padding-global); }

.container-large { width:100%; max-width: var(--_responsive---container--large);
                   margin-right:auto; margin-left:auto; padding-left:0px; }
@media (max-width:991px) .container-large {
                   padding-right: var(--_responsive---padding-global--padding-global);
                   padding-left:  var(--_responsive---padding-global--padding-global); }

.section_borders { position:relative; display:flex; width:100%; max-width:77.5em;
                   margin:0 auto; justify-content:center; align-items:center;
                   border-right:1px solid var(--border-color--lavendar-blue);
                   border-left:1px solid var(--border-color--lavendar-blue); }

.padding-section-large   { padding-top/bottom: var(--_responsive---padding-section--large); }
.padding-section-medium  { padding-top/bottom: var(--_responsive---padding-section--medium); }
.padding-section-xmedium { padding-top/bottom: var(--_responsive---padding-section--small); }  /* <- see gotchas */
.padding-section-small   { padding-top/bottom: var(--_responsive---padding-section--small); }
.padding-section-tiny    { padding-top/bottom: var(--_responsive---padding-section--tiny); }

.content-wrapper { width:100%; margin-right:auto; margin-left:auto; }
.max-width-large  { width:100%; max-width:53.75em; }   /* 860px */
.max-width-medium { width:100%; max-width:22em; }      /* 352px */
.max-width-small  { width:100%; max-width:18.75em; }   /* 300px */
@media (max-width:767px) .max-width-small { max-width:none; }
.max-width-full   { width:100%; max-width:none; }
.container-large.is_max-width-none { max-width:none; }
```

Resolved values (px at a 16px root):

| token | desktop | tablet ≤991 | mob-land ≤767 | mob-port ≤479 |
|---|---|---|---|---|
| `Padding Global` | **6.25em = 100px** | 1.5em = 24px | 1em = 16px | 0.75em = 12px |
| `Padding Section/Large` | 7.5em = 120px | 5.5em = 88px | 4.5em = 72px | 3em = 48px |
| `Padding Section/Medium` | 5em = 80px | 4em = 64px | 4em = 64px | 3.5em = 56px |
| `Padding Section/XMedium` | 4.13em ≈ 66px | 4em = 64px | 3.5em = 56px | 3em = 48px |
| `Padding Section/Small` | 2.88em ≈ 46px | 2.5em = 40px | 2.5em = 40px | 2.5em = 40px |
| `Padding Section/XSmall` | 2.5em = 40px | 2em = 32px | 2em = 32px | 1.5em = 24px |
| `Padding Section/Tiny` | 1.25em = 20px | 1.25em | 1.25em | 1.25em |
| `Container/XLarge` | 90em = 1440px | 1440 | 1440 | 1440 |
| `Container/Large` | **75em = 1200px** | 1200 | 1200 | 1200 |
| `Container/Medium` | 65em = 1040px | 1040 | 1040 | 1040 |
| `Container/Small` | 60em = 960px | 960 | 960 | 960 |

Gaps are hand-set on the flex/grid wrapper, usually in em: `.button-group` 0.5em, `.nav_flex-left` 3em, `.nav_menu-inner` 2em, `.nav_flex-right` 1em, `.styleguide-button-wrapper` 1.5em, `.grid` 1em, `.grid.is_features` 0.75em. There are three named gap utilities: `gap_0.5em`, `gap_0.75em`, `gap_1em` (class names containing a literal `.`, which is unusual but legal).

### Responsive behaviour
Webflow breakpoints in play: `main` (≥992), `medium` (≤991 tablet), `small` (≤767 mobile landscape), `tiny` (≤479 mobile portrait). `large`/`xl`/`xxl` are **not used at all** — no style has properties above `main`.

What actually changes per breakpoint:
1. **Everything sized in `em` changes by variable mode**, not by CSS. Type and section padding shrink automatically.
2. **Grid columns**, hand-set per breakpoint, e.g.
   ```css
   .grid.is_features { grid-template-columns: 1fr 1fr 1fr; gap:.75em; }
   @media (max-width:991px) .grid.is_features { grid-template-columns: 1fr 1fr; }
   @media (max-width:767px) .grid.is_features { grid-template-columns:1fr; grid-template-rows:auto auto;
                                                grid-auto-columns:1fr; gap:1.25em; }
   @media (max-width:479px) .grid.is_features { gap:1.13em; }
   .grid { display:grid; width:100%; grid-template-columns:1fr 1fr; gap:1em; grid-auto-columns:1fr;
           grid-template-rows:auto auto; }
   ```
3. **Container gutters move**: `padding-global` supplies them on desktop; below 992 `container-large` takes over the same variable, because `section_borders` (the 1240px bordered band) stops being meaningful.
4. **Nav collapses** to the stock Webflow dropdown (above).
5. **Elements swap** rather than restyle, via `is_desktop-active` / `is_mobile-active` pairs (copyright line, nav CTA group).
6. `.max-width-small { max-width:none }` at ≤767 so 300px-capped body copy goes full width.

### Components (10, site-wide)

| name | id | instances | props | variants |
|---|---|---|---|---|
| `Global Styles` | `d36a88a9-…6cad` | 13 | — | base |
| `Navigation` | `1e324cf3-…aaba` | 13 | `Variant` | `base`, `Without Borders` |
| `Footer` | `24552a96-…6b41` | 10 | — | base |
| `Button Secondary` | `05c17a6d-…b058` | 41 | Variant, Link, Text 1, Text 2 | `base`, `With Icon` |
| `Button Primary` | `404033a5-…4f6b` | 38 | Link, Text 1, Text 2, Image | base |
| `CTA (Dark)` | `99901c99-…148a` | 3 | Image ×3, Title, Text 1–6, Link ×2 | base |
| `CTA (Light)` | `8099822a-…9494` | 1 | Image ×3, Title, Text 1–7, Link ×2 | base |
| `Newsletter` | `16cd5aea-…4f6b` | 3 | Title, Text 1–5, Image | base |
| `Button Primary (White)` | `7f009e5b-…9e84` | 3 | Link 1, Text 1, Text 2, Image | base |
| `Button Secondary (Dark)` | `989d5273-…ac1b` | 3 | Variant, Link 2, Text 1–3 | `base`, `Is Icon` |

**Prop naming is generic and positional** — `Text 1`, `Text 2`, `Image 1`, `Link 2`, with only `Title` given a real name. No slots are used anywhere. `Global Styles`, `Navigation` and `Footer` appear on 10–13 pages, i.e. on every page (13 pages minus the CMS templates that only need some of them).

### CMS

Four collections — a textbook marketplace-template blog stack plus the required image-licence collection:

**Blog Posts** (`6a57e704fc6f1cdf8a4aa9ad`, slug `blog-posts`, singular "Blog Post")
| field slug | type | required | help text |
|---|---|---|---|
| `name` | PlainText | ✓ | |
| `slug` | PlainText | ✓ | |
| `post-body` | RichText | | |
| `post-summary` | PlainText (single line) | | "A summary of the blog post that appears on blog post" |
| `main-image` | Image | | display name **"Main Iimage"** (typo) |
| `thumbnail-image-2` | Image | | "Smaller version of main image that is used on blog post grid" |
| `time-to-read` | Number (integer, ≥0) | | |
| `author` | Reference → Authors | | |
| `categories` | MultiReference → Categories | | |
| `date` | DateTime | | |
| `featured` | Switch | | display name "Featured?" |

**Authors** (`6a591b15a4152c9dafac7d4a`): `name`, `slug`, `author-profile` (Image).
**Categories** (`6a591b323e484c06e9addbea`): `name`, `slug` only.
**Image Licenses** (`6a6e01b4669054aa40900253`): `name`, `slug`, `thumbnail-image` (Image), `license-link` (Link).

No field groups are defined on any collection.

**How collection lists are laid out** (from the Blog page — 15 `collection-list-wrapper` elements on that one page):
```
DynamoWrapper.collection-list-wrapper
└ DynamoList.grid.is_blog-post                 (featured list uses .collection-list.is_blog)
  └ DynamoItem
    └ div.blog_post-card
      ├ div.blog_thumbnail
      ├ div.blog_lower-wrapper
      ├ div.author_outer-wrapper
      └ a.blog_post-link                       (whole-card overlay link)
└ DynamoEmpty › div › "No items found."
└ Pagination.pagination
  ├ PaginationPrevious.pagination-button.is_previous  › "Previous"
  └ PaginationNext.pagination-button.is_next          › "Next"
```
A category filter row reuses the same rig with `DynamoList.collection-list` → `DynamoItem.collection-item` → `div.traditional_pill-border` › `div.traditional_pill`.

Conventions: the **collection list always carries a layout class** (`.grid.is_blog-post` or `.collection-list.is_blog`) rather than being styled on the wrapper; the empty state is always left with Webflow's default "No items found." text; card-level navigation is a **full-card absolutely-positioned `a.blog_post-link`**, so the card itself is a `div`, not a link.

### Interactions / animations
**8 IX3 interactions, no GSAP/Lottie/custom JS.** (`lottie` exists as a class name but no registered script backs it.)

| name | page | trigger | target |
|---|---|---|---|
| `Team Logos Scroll In View` | Home (site scope) | `wf:scroll`, `start:"top 85%"`, `end:"bottom top"`, no scrub | `wf:class` → 2 style blocks |
| `Dashboard Move On Scroll` | Home | `wf:scroll`, `start:"top bottom"`, `end:"bottom 50%"`, **scrub 1.5** | `wf:class` (`.dashboard-image`) |
| `Features Card Fade In` | Home | `wf:scroll`, `start:"top bottom"`, `end:"bottom top"` | `wf:class` → `.grid` + one combo |
| `Scroll interaction` | Home | `wf:scroll`, scrub 0.8 | `wf:inst` (single element) — **`timelineIds: []`, i.e. an empty, dead interaction** |
| `1–4 Image Hover Interaction` | About | `wf:hover` restart + `wf:hover` reverse (`mouseleave`) | **`wf:attribute` → `[hover-image="image-1..4"]`** |

Two rules worth copying: **scroll animations target classes, hover animations target custom attributes** (`hover-image="image-N"`), which keeps the interaction reusable and decoupled from element ids. Every scroll trigger has `showMarkers:true` left on — a development flag that should have been turned off before shipping.

### Custom code
- **Site head: empty. Site footer: empty. Home page head/footer: empty. Registered scripts: 0.**
- All global CSS lives in the **`Global Styles` component**, a single `HtmlEmbed.global-styles` (`position:fixed; left:0; top:0; display:block`) dropped as the first child of `.page-wrapper` on every page. It contains three `<style>` blocks:

  **(a) the fluid-root-font trick — the keystone of the whole system**
  ```css
  body { font-size: 1.1111111111111112vw; }
  /* Max Font Size */
  @media screen and (min-width:1440px) { body { font-size: 16px; } }
  /* Container Max Width */
  .container { max-width: 1440px; }
  /* Min Font Size */
  @media screen and (max-width:991px) { body { font-size: 16px; } }
  ```
  `1.1111111111111112vw` = exactly `16/1440 * 100`. So the whole page scales linearly between 992px and 1440px, then locks at 16px in both directions. **This is why every variable and every class is in `em` and not `rem`** — `em` inherits the fluid root.

  **(b) the stock Finsweet Client-First global stylesheet**, unmodified, including `-webkit-font-smoothing`, the `*[tabindex]:focus-visible { outline:.125rem solid #4d65ff }` focus ring, `.w-richtext` first/last-child margin resets, `.container-medium/.container-small/.container-large { margin: auto !important }`, `.text-style-2lines` / `.text-style-3lines`, `.hide` / `.hide-tablet` / `.hide-mobile-landscape` / `.hide-mobile`, and the whole `.margin-0` / `.padding-0` / `.spacing-clean` / `.margin-top` / `.padding-vertical` … family. The commented-out "inherit typography" block is left in place with its original comment:
  > "Uncomment this CSS to use it in the project. Leave this message for future hand-off."

  **(c) a 3-line gradient-border mask helper**
  ```css
  .gradient-border {
    -webkit-mask: linear-gradient(#fff 0 0) content-box, linear-gradient(#fff 0 0);
    -webkit-mask-composite: xor;
    mask-composite: exclude;
  }
  ```

- **Fonts are self-hosted uploads, not Google Fonts / Adobe.** 9 custom fonts: `Interdisplay` at weights 100/200/300/400/500/600/700/900 (all `.ttf`, `font-display:swap`, uploaded 2026-05-30) plus one variable font `Satoshi Variable` (`wght` axis 300–900, default 900). **`Satoshi Variable` is uploaded but no style block references it** — Figma's style guide advertises both fonts; only Inter Display made it into the build.

### Images
- Every image is a Webflow asset referenced by `assetId`; there are no external image URLs in the markup.
- **Alt-text pattern is short, lowercase and generic, repeated across instances:** `"feature image"` on all six feature cards, `"brand logo"` on the nav logo and the footer logo, `"Hero Dashboard"` on the hero screenshot. Alt text is never left empty, but it is never descriptive either.
- Icons are **inline SVG via `HtmlEmbed.icon_embed`** inside a `div.pill_icon` / `div.features_icon-wrapper`, not `<img>`. Other embed-ish classes: `check_embed`, `priniciple_icon-embed` (*typo: "priniciple"*), `flare_icon`, `flash_icon`, `slash_icon`, `toggle_icon`, `pill_icon`, `social_icon`, `feature-icon`.
- Images are always wrapped: `div.features_image > img.image`, `div.dashboard_image-wrapper > img.dashboard-image`, `div.blog_thumbnail > img`, `div.usecase_image-wrapper`, `div.works_image-wrapper`, `div.license_image-wrapper`, `div.team_member-image`, `div.stack_image-wrapper`, `div.image_wrapper`.
- OG images are `.webp` served from `cdn.prod.website-files.com` — one shared image (`6a68d3451b615e1c2706dbd6_Home%20page.webp`) is reused as the OG image for **every** page. The Style Guide page still points at the `.png` version of the same file.

### Forms, links, SEO
- **Forms:** `contact_form` / `contact_form-block` / `contact_form-card` (Contact), `cta_form` / `cta_form-block` / `cta_form-wrapper` / `cta_subscribe` (CTA + Newsletter), `filteration_form` / `filteration_form-block` / `filteration_form-wrapper` (blog filter), `utility-page_form` (password page). Field furniture: `input-field`, `field-label`, `field_wrapper`, `radio-field`, `radio-button`, `success-message`, `form_message-error`, `empty_state`.
- **Links:** social links use protocol-relative hrefs (`//www.linkedin.com/`, `//discord.com/`, `//www.instagram.com/`, `//www.facebook.com/`) with no real accounts — correct for a template. Button component defaults are `{mode:"url", to:"#"}`, overridden per instance to `{mode:"page", to:{pageId:"6a22353da5c71760124a7c1b"}}` (Contact).
- **SEO:** every real page has a hand-written SEO title + meta description, e.g.
  - Home — *"BookFlow AI Powered Scheduling"* / *"AI assistant that books meetings while you sleep. Machine learning finds perfect times, predicts no-shows, auto-reschedules conflicts. Free start."*
  - Style Guide — *"Style Guide BookFlow AI | AI-Powered Scheduling Platform"*
  - Licenses — *"Licenses BookFlow AI | AI-Powered Scheduling Platform"*
  - Changelog — *"Changelog BookFlow AI | AI-Powered Scheduling Platform"*
  - Utility-page titles follow the pattern `<Page> BookFlow AI | AI-Powered Scheduling Platform`; product pages use `<Topic> | BookFlow AI`.
  - Blog Posts Template binds CMS fields: `title = {{wf {"path":"name","type":"PlainText"} }}`, `description = {{wf {"path":"post-summary",…} }}`, og image = `{{wf {"path":"main-image","type":"ImageRef"} }}`.
  - `openGraph.titleCopied:true` and `descriptionCopied:true` everywhere — OG is never written separately.
- No localisation: the Colors collection has one mode, no secondary locales were encountered.

---

## 3. Figma → Webflow mapping observed

| Design concept in Figma | How it was realised in Webflow | Changed / notes |
|---|---|---|
| Page frame `BookFlow AI - Home Page` (1440×10764) | Page `Home`, slug `/` | 1:1 |
| Page frame `Style Guide` / `Licenses` / `Changelog` (1441 wide) | Pages inside the `utility-pages` folder | 1:1 — the 1441 width is just a stray pixel, not a design decision |
| Hidden frame `BookFlow AI - Home Page` (6285:2542) | nothing | archived; the Webflow build follows `99:242` |
| Section frame `Hero`, `AI Features`, … | `section.section_hero`, `section.section_ai-features`, … one class per Figma section name | **Direct, lowercase, snake-prefixed translation of the frame name.** `Problem Solution` → `section_solution`, `How AI Works` → `section_works`, `Use Cases` → `section_usecases` |
| Empty frame `Frame 2147225077`, x=100, w=1240 | `div.section_borders` (`max-width:77.5em` = 1240px, 1px left/right borders) + `div.padding-global` (`6.25em` = 100px) | Exact. What is an invisible guide frame in Figma became a *visible* 1px bordered band in the build |
| Content frames at x=120, w=1200 | `div.container-large` (`max-width: var(--container--large)` = 75em = **1200px**) | Exact |
| Loose 12×12 `Rectangle 121…130` at x=94 / x=1334 | `div.borders_connector` + `.is_top-left` / `.is_top-right` / `.is_bottom-left` / `.is_bottom-right` / `.is_center-left` / `.is_center-right` | The corner-dot motif was **generalised into a reusable positional combo set** and then extended far beyond the design (`is_integration-top-left`, `is_comparison-left`, …) |
| Horizontal `Vector 958` rules across the frame | `div.section_divider` (`.is_top` / `.is_bottom`), `div.absolute_outer-divider`, `div.absolute_inner-divider` (`.is_right`, `.is_tilt`), `div.top_divider`, `div.light_blue-divider`, `div.footer_divider` | Expanded into a whole divider family |
| Figma colour labels (`Blue #1C62EF`, `Grey 1 #9A9BA3`, …) | Webflow variables in collection **Colors**, named after the colour not the role (`royal blue`, `cool grey`) | **Tokens were created in Webflow, not Figma.** Figma has no colour variables at all |
| Figma type labels (`Heading 2 / Bold / 48px`) | `.heading-style-h2` + variable `Heading Size/H2` = `3em` | H2 (48), H3 (36), Body Large (20), Regular (16), Small (14) match exactly |
| Figma `Heading 1 / Bold / 62px` | `.heading-style-h1` = `3.5em` = **56px** | **Changed** — H1 shrank from 62 → 56px |
| Figma `Heading 4 / Semibold / 24px` | `.heading-style-h4` = `2em` = **32px**, weight **700** | **Changed** in both size and weight |
| Figma `Heading 5 / Semibold / 20px` | `.heading-style-h5` = `1.5em` = **24px**, weight 700 | **Changed** |
| Figma `Heading 6 / Semibold / 24px` | `.heading-style-h6` = `1.25em` = **20px**, weight 700 | Figma label itself looks wrong; build value (20px) is the sane one |
| Figma fonts `Satoshi Variable` + `Inter Display` | Only `Interdisplay` is used in CSS; Satoshi is uploaded but unreferenced | **Dropped** |
| Figma shadow variable `Effect(DROP_SHADOW #5050501F 0,4 blur 24)` | Not reused as a token; shadows hand-written per class, e.g. `.button_primary` `inset 0 -1px 2px 1px rgba(0,0,0,.25), inset 0 1px 2px 1px rgba(255,255,255,.25), 0 4px 12px 0 rgba(28,98,239,.3)` | **Changed** — the button got a richer 3-layer shadow than the design token |
| Figma auto-layout row/column | `display:flex` on `flex_wrapper`, `button-group`, `nav_flex-left/right`, `card_info-wrapper`; `display:grid` on `.grid` + `.is_<section>` for column counts | Gaps translated to `grid-column-gap`/`grid-row-gap` in `em` |
| Figma spacing (100px page gutter, 120px section padding) | Variables `Padding Global` = 6.25em (100px), `Padding Section/Large` = 7.5em (120px) | **Rounded onto a named scale** (large/medium/xmedium/small/xsmall/tiny) rather than kept as raw numbers |
| Figma button `Try AI Scheduling Free` + `Noise & Texture` rects | Component `Button Primary` (38 instances) with `button-borders` gradient ring + `button_primary` inset-shadow body; the noise rects became `background-image: linear-gradient(180deg, rgba(170,174,245,0), rgba(170,174,245,.6))` | **Simplified** — three overlapping noise rects → one gradient overlay |
| Figma button `Watch AI Demo` | Component `Button Secondary` (41 instances), variant `With Icon` | |
| Figma nav `Frame 11` (logo + 4 text links + 2 buttons) | Component `Navigation` with Webflow's native Navbar widget, variants `base` / `Without Borders` | |
| Figma `Footer` frame (logo + 3 link columns + social) | Component `Footer`: `flex_wrapper.is_footer-menu` + `grid.is_footer` (3 cols) | |
| Figma icon instances (`star-filled`, `logos:slack`, …) | Inline SVG in `HtmlEmbed.icon_embed` | **Not** exported as image assets |
| Figma starfield (`Star, Round, 2.xx` ellipses, thousands of nodes) | `div.sprinkles_bg`, `div.textured_bg`, `div.blur_ellipse`, `div.xsmall_ellipse`, `div.glowing-circle`, `div.diagonal_gradients` | **Simplified heavily** — thousands of vector stars became a handful of CSS/background divs |
| Figma breakpoint frames | **none exist** — tablet/mobile were designed directly in Webflow using variable modes `Tablet` / `Mobile Landscape` / `Mobile Portrait` | The single biggest gap between design and build |
| Hover states | Not in Figma at all. Built in Webflow as `:hover` pseudos (`.button_secondary:hover`, `.pagination-button:hover`) plus the `button_text-wrapin/wrapout` slide rig and IX3 hover interactions on About | Invented at build time |
| Blog Detail frame | Blog Posts Template page + Blog Posts/Authors/Categories collections | CMS modelling was a build-time decision; nothing in Figma indicates it |

I found **no Slack/Notion notes** in this session to explain the changes, so all "why" above is inference from the artefacts.

---

## 4. Quality notes and gotchas

### Deliberate team rules (repeated often enough to be rules)
1. **Every section is `section.section_<name>` → `div.padding-global` → `div.container-large` → `div.padding-section-<size>` → `div.content-wrapper.is_<section>`.** No section ever skips `padding-global` except `section_usecases` and `section_testimonials`, which deliberately go wide.
2. **`padding-section-large` is the default for content sections; `padding-section-medium` for utility pages (Style Guide, Licenses) and the footer; `padding-section-tiny` for the hero.**
3. **Every size, every spacing, every font size is a variable in `em`.** Almost nothing is a hard-coded px length except 1px borders, letter-spacings and shadows.
4. **Nothing is styled per breakpoint unless it is a layout change.** Type/spacing responsiveness is entirely variable modes. The only per-breakpoint typography override in the whole site is `heading-style-h2`'s letter-spacing.
5. **Headings always use `heading-style-hN` on a real `hN` tag** — the h1 in the hero is `Heading` with `headingLevel:1` and class `heading-style-h1`.
6. **Body copy always uses a `text-size-*` class**, never a bare `<p>`.
7. **Images always sit inside a wrapper div** (`features_image`, `dashboard_image-wrapper`, `blog_thumbnail`, …) so the wrapper can crop/round and the `img` stays a plain `.image`.
8. **Icons are inline SVG embeds (`icon_embed`), never `<img>`.**
9. **Buttons are components with `Text 1` + `Text 2` duplicated** for the hover slide; overriding a button means setting props, never restyling.
10. **`Global Styles` + `Navigation` + `Footer` are dropped on every page as the first/last children of `.page-wrapper`.**
11. **Section-specific tuning is always a `is_<context>` combo on a shared base class**, never a new base class — hence 276 combo blocks over only 391 base classes.
12. **Desktop/mobile variants of the same content are duplicated and toggled** with `is_desktop-active` / `is_mobile-active` rather than restyled.
13. **No custom code outside the one Global Styles embed. No third-party scripts. All animation is native IX3.** (Exactly what Marketplace QA wants.)

### Mistakes and inconsistencies found
- **`.padding-section-xmedium` points at the wrong variable.** It uses `variable-024c4a66…` = `Padding Section/Small` (2.88em), identical to `.padding-section-small`. The `Padding Section/XMedium` variable (`variable-d7ee9404…`, 4.13em) is defined but referenced by nothing. Real bug.
- **Typos shipped into class names:** `buton_text-absolute` (missing "t"), `priniciple_icon-embed` (should be "principle"). CMS field display name **"Main Iimage"**.
- **Modifier prefix is inconsistent:** 137 combos use `is_…`, but `is-secondary` and `is-3rd` use `is-`.
- **Colour-modifier prefix is inconsistent:** `text_color-blue`, `text_color-light-blue`, `text_color-pale-slate`, `text_color-stone` (underscore) vs `text-color-cinder`, `text-color-medium-grey`, `text-color-woodsmoke` (all hyphen).
- **Junk combo class names left in the project:** `1`, `2`, `cta`, `works`, `fs-styleguide`.
- **Stock Webflow leftovers not deleted:** `.button` (in `rem`, contradicting the em system), `.collection-item`, `.collection-list`, `.lottie`, `.badge`, `.a`, `.img`.
- **A dead interaction:** `Scroll interaction` on the homepage has `timelineIds: []` — a trigger with no animation attached.
- **All 4 scroll interactions still have `showMarkers: true`**, the GSAP ScrollTrigger debug flag.
- **`Satoshi Variable` is uploaded (and advertised in the Figma style guide) but never used** — dead weight in a template that Marketplace reviewers will open.
- **The variable name `- Base Color /white smoke` has a stray space before the slash**, so it sorts/reads differently from its 18 siblings (`- Base Color/…`).
- **Duplicate tag styles:** the style list contains `h1, h1, h2, h2, h3, h3, h4, h4, h5, h5, h6, h6, p, p, ul, ul, ol, ol, li, li, blockquote, blockquote, figure, figure` — each base HTML tag style appears twice.
- **Every page shares one OG image** (`Home page.webp`), and the Style Guide page points at the `.png` twin of it.
- **Alt text is generic and repeated** (`"feature image"` six times on one page) — passable but weak for accessibility review.
- **Figma has no mobile designs at all**, so the responsive build is entirely unreviewed against a comp.

### Special / notable
- **Fluid root typography** (`body { font-size: 1.1111111111111112vw }` clamped 992–1440) is the defining technical choice of this template.
- **Variable modes as the responsive engine** (`Tablet` / `Mobile Landscape` / `Mobile Portrait`) rather than breakpoint CSS.
- **The "blueprint grid" motif**: 1px vertical rules at the 1240 band (`section_borders`, `nav_borders`) with 12×12 corner dots (`borders_connector`), replicated with an absolutely-positioned overlay (`absolute_borders-wrapper` → `section_borders.is_height-100%`) on sections that need the rules to run full-bleed.
- **Sticky nav:** `.nav_container { position: sticky }`.
- **Tabs:** native Webflow Tabs on Pricing (`pricing_tabs`, `pricing_tabs-menu`, `pricing_tabs-link`, `pricing_tabs-content`, `pricing_tabs-pane`) and on Blog (`blog_tabs`, `blogs-tabs-menu`, `blogs_tabs-link`, `blogs-tabs-content`).
- **Accordion:** hand-built (`accordion_card`, `accordion_title-wrapper`, `accordion_toggle`, `accordion_toggle-border`, `accordion_description-wrapper`, `accordion_description-card`, `accordion_spacer`), driven by IX — no library.
- **Marquee:** CSS-only, two implementations (`marquee_horizontal`, `marquee_horizontal-css`, `marquee_cover`, `marquee_overlay`, `track-horizontal`, `track-horizontal.is_reverse`). **No Swiper/Splide/slick anywhere.**
- **No sliders, no modals, no video embeds, no dark-mode toggle, no RTL, no localisation.**
- **Accessibility:** the Client-First focus ring (`outline:.125rem solid #4d65ff`) is present; heading levels are semantic; landmarks `<main>` and `<footer>` are used.

---

## 5. Verbatim dumps

Omitted from the skill copy; the full dump lives in the analysis workspace.

## 6. Tool quirks

**These are the exact call shapes that worked. Copy them.**

### Environment / network
- **`*.webflow.io` is blocked by the egress proxy.** `curl -sSL https://bookflow-3b73f0.webflow.io/` → `curl: (56) CONNECT tunnel failed, response 403`. `WebFetch` on the same URL → `{"error_type":"EGRESS_BLOCKED","domain":"bookflow-3b73f0.webflow.io"}`. `curl -sS "$HTTPS_PROXY/__agentproxy/status"` confirms `connect_rejected … gateway answered 403 to CONNECT` for `bookflow-3b73f0.webflow.io:443` (and for `medicini-plus.webflow.io`, `furec.webflow.io` — so it is a blanket block, not site-specific). **Do not plan around fetching the live HTML/CSS; the Data API is the only source.** Do not retry — it is a policy denial.
- **`figma.com` is blocked too.** `curl -sSL -o x.png "https://www.figma.com/api/mcp/asset/<uuid>.png"` → `curl: (56) CONNECT tunnel failed, response 403`. So **`get_screenshot`'s recommended URL+curl path does not work here**; the only way to see a Figma image is `enableBase64Response: true`, which returns the image inline into context and **cannot be saved to a file**. No screenshots were saved for this run.
- `cd` inside a Bash compound command silently broke a `curl -o relative/path` (working directory resets between Bash calls in agent threads). **Always use absolute paths in Bash.**

### Figma MCP
- `mcp__Figma__get_metadata({fileKey})` with **no `nodeId`** lists the document's pages. Cheap and safe — always start here.
  ```
  get_metadata({ fileKey: "28ujPpgb7t0r1ddfXKxLNz" })
  → "0:1: Design Area", "25:2: Icon"
  ```
- `get_metadata({fileKey, nodeId:"0:1"})` returned **1,910,517 characters** and was auto-spilled to a tool-results file. Same for page `25:2` (1,845,601 chars). **Expect page-level metadata to blow the context limit on any real marketing site.** Recovery recipe that worked:
  ```bash
  python3 -c "
  import json
  t=''.join(x['text'] for x in json.load(open('<spill file>')))
  open('<run folder>/figma_meta.xml','w').write(t)"
  # then grep/sed the XML: top-level frames are the lines matching ^  <frame
  grep -n '^  <frame' figma_meta.xml          # page frames
  sed -n '<start>,<end>p' figma_meta.xml | grep -nE '^    <'   # section frames inside one page
  ```
- The spill file is `[{type,text}]` JSON whose single `text` entry holds the whole XML — `jq -r '.[].text'` also works.
- `get_variable_defs({fileKey, nodeId:"99:242"})` works with a **frame** node id and is tiny. If the file has no variables it returns a nearly empty object — that is a real finding, not an error.
- `get_screenshot({fileKey, nodeId:"4011:422", maxDimension:1600})` succeeded and returned `{image_url, width, height, original_width, original_height}` — but see the egress note above.
- `get_design_context` was **not used** on this run: its tool description requires loading `skill://figma/figma-design-to-code/SKILL.md` first (via `mcp__Figma__get_figma_skill`), which is 2 extra calls, and the structural data from `get_metadata` + the Webflow API already answered every question. If you need it: load the skill first, then pass `skillNames:"resource:figma-design-to-code"` and `excludeScreenshot:true` to keep the response small.
- Node ids: colon form (`99:242`). The `nodeId` regex rejects empty strings; `get_variable_defs` and `get_screenshot` accept only plain `\d+[:-]\d+` (no instance `I…;…` paths).

### Webflow MCP
- **Identity params, on every single call:**
  ```
  agent_id:   "opus-5-1m|claude-code|bkf7q"     (chosen once, never changed)
  session_id: "start"  on the first call only
  session_id: "ses_3JeKSwAaah9nz0P15HEBXUTeaeh" on every call after
  context:    15–25 words, third person, no I/we/you
  ```
  The first call returns a text block `[session_id issued …]` followed by `session_id: ses_…`. Grab it immediately.
- **`data_style_tool`, `data_variable_tool`, `data_element_tool`, `data_element_settings_tool`, `data_component_tool` and `data_interactions_tool` all require BOTH `siteId` and `pageId` at the top level**, even when the action is site-wide (variables, components, interactions). Any valid page id works. `data_cms_tool`, `data_pages_tool`, `data_fonts_tool` and `data_scripts_tool` take **no** `pageId` — passing one is not needed and `siteId` goes *inside* the action there.
- Every tool takes `actions: [ { label, <exactly one action object> } ]`. `label` is mandatory and is echoed back. Several actions can be batched in one call and run sequentially — used here to pull 4 CMS schemas in one round trip.

**Call shapes that worked (copy verbatim, swapping ids):**
```jsonc
// pages
data_pages_tool  actions:[{label:"list", list_pages:{site_id:"<site>", limit:100}}]

// all classes, names only (667 rows ≈ 109k chars → will spill to a file)
data_style_tool  siteId, pageId,
  actions:[{label:"all", get_styles:{query:"all", include_properties:false}}]

// targeted CSS with real values + breakpoints (this is the one to use)
data_style_tool  siteId, pageId, actions:[{label:"css", query_styles:{queries:[
  {label:"headings", name_path:["heading-style"], include_properties:true,
   include_breakpoints:["main","small","tiny"], limit:20},
  {label:"buttons",  name_path:["button"], include_properties:true,
   include_base_pseudos:["noPseudo","hover"], limit:25},
  {label:"grid features", name_path:["grid","is_features"], include_properties:true,
   include_breakpoints:["main","medium","small","tiny"]}   // 2 segments = a combo chain
]}}]

// variables
data_variable_tool siteId, pageId,
  actions:[{label:"cols", get_variable_collections:{query:"all"}}]
data_variable_tool siteId, pageId, actions:[
  {label:"colors",     get_variables:{variable_collection_id:"collection-…", include_all_modes:true}},
  {label:"responsive", get_variables:{variable_collection_id:"collection-…", include_all_modes:true}}]

// page tree
data_element_tool siteId, pageId:"<page>",
  actions:[{label:"tree", get_all_elements:{depth:6}}]
// component definition tree
data_element_tool siteId, pageId:"<any page>",
  actions:[{label:"nav", get_all_elements:{depth:6, scope_component_id:"<component id>"}}]
// drill into one subtree
data_element_tool siteId, pageId, actions:[{label:"q", query_elements:{queries:[
  {label:"hero", element_id:{component:"<pageId>", element:"<element uuid>"}, children_depth:6}]}}]
// find elements by class
data_element_tool siteId, pageId, actions:[{label:"q", query_elements:{queries:[
  {label:"lists", element_filter:{style:"collection-list-wrapper"}, children_depth:4, limit:3}]}}]

// components + props + variants + instance counts, one call
data_component_tool siteId, pageId, actions:[{label:"c", get_all_components:{options:{
  includeProps:true, includeInstanceCount:true, includeVariants:true}}}]

// the Global Styles embed's CSS
data_element_settings_tool siteId, pageId, actions:[{label:"code", get_settings:{
  type:"all_resolved_settings",
  element_id:{component:"<componentId>", element:"<componentId>"},
  scope_component_id:"<componentId>"}}]   // returns key "code" with the raw <style> blocks

// CMS, custom code, fonts, interactions
data_cms_tool          actions:[{label:"l", get_collection_list:{siteId:"<site>"}},
                                {label:"d", get_collection_details:{collection_id:"<id>"}}]
data_scripts_tool      actions:[{label:"s", get_site_freeform_code:{site_id:"<site>"}},
                                {label:"r", get_registered_scripts:{site_id:"<site>"}},
                                {label:"p", get_page_freeform_code:{page_id:"<page>"}}]
data_fonts_tool        actions:[{label:"f", list_fonts:{site_id:"<site>", limit:100}}]
data_interactions_tool siteId, pageId, actions:[{label:"ix", list_interactions:{limit:100}}]
```

**Errors hit and what fixed them:**
- `get_all_elements` with `depth: -1` on the homepage:
  `Tool get_all_elements failed: GET /v2/assets returned 429`
  → the element tool resolves image assets internally and **gets rate-limited by the assets endpoint**, *not* by the element endpoint. Two fixes, both verified: (a) drop to `depth: 6` and drill in with `query_elements`; (b) just retry a call or two later — the same `depth:6` call that 429'd on a component succeeded verbatim ~5 calls afterwards. `depth: -1` *did* work on a tiny component (`Global Styles`, 1 element).
- `query_elements` with `element_filter:{type:"CollectionWrapper"}` returned `total_matches: 0`. The real element `type` is **`DynamoWrapper`** (and `DynamoList` / `DynamoItem` / `DynamoEmpty`, `Pagination` / `PaginationPrevious` / `PaginationNext`). Filtering by `style` instead (`{style:"collection-list-wrapper"}`) is more reliable than guessing type names.
- `get_styles {query:"all"}` returned 109,143 chars and spilled to a file. Parse it with:
  ```bash
  python3 -c "
  import json; r=json.load(open('<spill>'))['result']
  print(len(r), sum(1 for x in r if x['isComboClass']))
  print(', '.join(sorted(x['name'] for x in r if not x['isComboClass'])))"
  ```
  Each row is `{id, name, isFromLibrary, isComboClass, type: "global"|"combo"|"tag", selector}` — **`selector` is the gold**, because for combos it prints the full chain (`.text-size-xsmall.is_comparison.text_color-blue`) which `name` alone does not.
- In `query_styles`, `name_path` is a **substring match per segment**: `["button"]` matched `pagination-button`, `menu-button`, `absolute_button` too. A 1-segment path never walks combos; pass `["grid","is_features"]` to hit a 2-deep combo exactly.
- `include_properties` defaults to **false** — a `query_styles` result with `"properties":{"base":{}}` may mean "no properties" *or* "you forgot the flag". Always pass `include_properties:true`.
- `include_breakpoints` defaults to `main` only, and the tool warns it is "data intensive" — it is fine for ≤25 styles at a time.
- Property values come back as **`{"id":"variable-…"}`** rather than a CSS string when a variable is bound. You must join them against `get_variables` output yourself; there is no resolved view.
- No capability found for: reading the published CSS/HTML, listing assets' alt text in bulk, or reading page-level "custom code" per breakpoint. Asset alt text is only visible inline on each `Image` element's `settings`.
- `data_scripts_tool` returning `content: ""` for both head and footer *and* an empty `registeredScripts` array is the correct "no custom code" signal — the real CSS was hiding in an `HtmlEmbed` component, found only via `data_element_settings_tool > get_settings`. **Always check for a `Global Styles`-style embed component before concluding a site has no custom code.**
- No rate limit was hit on `data_style_tool`, `data_variable_tool`, `data_cms_tool`, `data_component_tool`, `data_scripts_tool`, `data_fonts_tool` or `data_interactions_tool` across ~20 calls — only `data_element_tool` (via `/v2/assets`).
- Total: **~30 tool calls**, of which 3 failed and were recovered.

### Run folder contents
`/tmp/claude-0/-home-user-talha-vercel-test/10672cda-24bd-508e-8a36-0bcc891af513/scratchpad/figma-to-webflow-workspace/analysis/bookflow/`
- `figma_meta_designarea.xml` — full XML tree of Figma page `0:1` (1.75 MB)
- `figma_meta_icon.xml` — full XML tree of Figma page `25:2`
- `styles_all.json` — all 667 Webflow style blocks
- `class_names.txt` — all class names, comma separated
- `combo_names.txt`, `combo_selectors.txt` — combo classes and their full selectors
- *(no screenshots — figma.com asset downloads are blocked by the egress proxy)*
