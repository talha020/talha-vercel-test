# CleanFlow (Webflow template)
Figma: https://www.figma.com/design/hzrM8kfVMW8J6mLl0LOtOW/CleanFlow-Website--tR-?node-id=0-1  |  Webflow site id: 6a76f1e1a54f8b5343353365 (shortName `cleanflow-cb3c1a`)  |  Live: https://cleanflow-cb3c1a.webflow.io/  |  Analysed: 2026-09-21

> **Headline finding, stated up front because it reframes everything below.** The brief described a "5-page Webflow template". The Figma file does contain five 1440-wide page frames (Home, Services, Gallery, Reviews, Book Cleaning). **The Webflow site contains exactly one page ("Home", slug `/`), no CMS collections, no style-guide page, no licences page, no 404/password page, no footer, and no form.** `list_pages` returns `"total":1`; `get_collection_list` returns `{"collections":[]}`. The built page stops after the gallery slider and ends on a bare `._100vh` spacer block. So this is a **build in progress that was published once (2026-08-25) with roughly the top 60 % of the Home page done**, not a finished 5-page template. Every "how the team does X" observation below is therefore drawn from the Home page, which is fully representative of their system; the requested style-guide / licences / form / service-page sections are answered with the evidence that exists (mostly in Figma, not in Webflow).

---

## 1. Figma file anatomy

### Pages in the file (in order)
| # | Page name (verbatim) | id | Holds |
|---|---|---|---|
| 1 | `Design Area` | `0:1` | Everything: the five hi-fi page frames, plus "shelf" sections of component/state variations and loose working scraps. |
| 2 | `Icon` | `25:2` | Icon library page (Vuesax icon set — every icon instance in the design resolves to `vuesax/bulk/<name>`, e.g. `data-name="vuesax/bulk/star"`, `vuesax/bulk/broom`, `vuesax/bulk/call-calling`, `vuesax/bulk/medal`, `vuesax/bulk/flash`). |

There is **no separate dev page, no archive page, no "components" page, no style-guide frame.** Design tokens are not formalised: `get_variable_defs` on the Home frame returns `{}` — **the Figma file defines zero variables and no published colour/text styles.** Every colour and size in the design is a raw value on the layer.

### The "build from" frames
Five top-level page frames on `Design Area`, all **1440 px wide, desktop only**:

| Frame name (verbatim) | id | Size |
|---|---|---|
| `Hi-Fi - CleanFlow - Home` | `7059:44648` | 1440 × 9121 |
| `Hi-Fi - CleanFlow - Services` | `7188:1159` | 1440 × 6340 |
| `Hi-Fi - CleanFlow - Gallery` | `7188:1340` | 1440 × 3087 |
| `Hi-Fi - CleanFlow - Reviews` | `7228:2938` | 1440 × 4101 |
| `Hi-Fi - CleanFlow - Book Cleaning` | `7243:3633` | 1440 × 4346 |

Naming pattern: `Hi-Fi - <Project> - <Page>`. **There are no breakpoint frames at all** — no "Mobile 390", no "Tablet 768". A grep of the whole page metadata for `mobile|tablet|390|768` returns only false positives (a stray 768-wide scrap frame). **Responsive behaviour was never designed; it was invented in Webflow.**

Alongside the page frames sit Figma **Section** nodes used as variation shelves, plus a scatter of loose frames (`Frame 44`, `Frame 60`, `Group 5`, `Frame 52`, `Frame 87`, `Frame 89`…) parked in the canvas margins — spare card states and cut-outs:

- `Home - How It Works` (`7291:10748`, 1640 × 4508) — 4 × 1440 × 1002 states of the tabs section, one per tab.
- `Home - Packages` (`7199:2238`) — named children: `Standard Clean`, `Deep Clean`, `Move In Out Clean` (expanded, 600 × 337) and `Standard Clean (minimize)`, `Deep Clean (minimize)`, `Move In Out Clean (minimize)` (collapsed, 600 × 126). **This is how accordion states are specced: two separate frames per item, not variants.**
- `Home - Credentials` (`7279:8741`) — six 400 × 192 credential cards.
- `Services - Residential Services` (`7284:9205`), `Gallery - Projects` (`7286:10042`).

### Section naming inside the Home page frame (verbatim, top to bottom)
This is the single most distinctive (and worst) naming habit in the file. **Eleven of the eleven section frames on the Home page are named `CleanFlow - Hero`.** They were duplicated from the hero and never renamed:

```
7041:44301  CleanFlow - Hero   y=0     1440x1024   (hero)
7059:44539  CleanFlow - Hero   y=1024  1440x146    (marquee strip)
7059:44673  CleanFlow - Hero   y=1170  1440x869    (packages / instant quote)
7078:45080  CleanFlow - Hero   y=2039  1440x1007   (reviews)
7078:45450  CleanFlow - Hero   y=3046  1440x600    (credentials)
7291:10760  CleanFlow - Hero   y=3646  1440x1002   (how it works / tabs)
7126:1004   CleanFlow - Hero   y=4648  1440x106    (second marquee)
7137:1716   CleanFlow - Hero   y=4754  1440x1129   (gallery)
7145:1937   CleanFlow - Hero   y=5883  1440x760
7153:2128   CleanFlow - Hero   y=6643  1440x1055
7189:1661   CleanFlow - Hero   y=7698  1440x1423   (CTA + quote form + footer)
```

The same is true on the other four pages (only `Standard Clean` on the Services page breaks the pattern). Inner layers are Figma defaults: `Frame 6`, `Frame 24`, `Frame 25`, `Frame 28`, `Frame 44`, `Group 5`, `Mask group`, `Rectangle 21`, `Ellipse 5`. **Text layers, by contrast, are auto-named from their content** (`A Spotless Home`, `Without Lifting a Finger`, `Choose the Clean That Fits Your Life`), which is what makes the file navigable at all.

### Design tokens (read off the layers, since no styles/variables exist)
Colours actually used in the hero, verbatim from `get_design_context`:

| Hex | Where |
|---|---|
| `#fefcfa` | page background, nav background, button text on orange, pill chips |
| `#ea580c` | brand orange — primary button fill, italic accent word, phone-number chip text |
| `#000000` (`text-black`) | H1 first line, logotype, "4.9" |
| `#7c7c7c` | H1 second line, "+300 Ratings", the 2 px divider pill |
| `#727272` | hero paragraph |
| `#646464` | nav links |
| `#f0f0f0` | nav border |
| `#fff5ef` | phone chip background |
| `#ffede4` | phone chip border |

Type (Figma, hero): family **`Inter Display`** in weights `Regular` / `Medium` / `SemiBold` / `Light Italic` / `Italic`.
- H1 line 1: 72 px, `leading-[1.1]`, `tracking-[-2.88px]`, colour black + orange `Light Italic` span
- H1 line 2: 64 px, `leading-[1.1]`, `tracking-[-2.56px]`, `#7c7c7c`
- Body: 16 px, `leading-[1.45]`, `tracking-[-0.32px]`, `#727272`
- Eyebrow / meta: 18 px, `leading-[1.5]`, `tracking-[-0.36px]`
- Nav + button label: 14 px, `leading-[1.25]`–`[1.3]`, `tracking-[-0.28px]`

Radii: `4px` (buttons, chips), `8px` (nav bar), `20px` (floating pills), `2px` (the small white arrow square inside the primary button).
Effects: `drop-shadow(0px 4px 6px rgba(0,0,0,0.05))` on the nav; `drop-shadow(0px 0px 6px rgba(0,0,0,0.05))` on floating pills; `shadow-[0px_0px_12px_0px_rgba(0,0,0,0.25)]` on hero photos; `drop-shadow(0px 2px 4px rgba(0,0,0,0.25))` on the button arrow square.

### Components in Figma
No button/nav/footer/card components. The only real components are **icons** — 35 distinct instance names, all Vuesax bulk icons, by usage count:

`star(79), check(58), social-icons(26), flash-circle(18), slash(18), broom(17), chevron-down(16), quote-up(15), box(14), arrow-right(12), call-calling(10), brush-4(9), profile-2user(9), card-pos(8), add-square(8), flash(7), messages-2(7), favorite-chart(7), mouse-square(5), emoji-normal(5), selector(4), medal(3), shield-tick(3), tree(3), note-2(3), add(2), crown-2(2), receipt-add(2), minus-square(2), diagram(2), repeat-circle(2), slider-horizontal(1), reserve(1), sms-tracking(1), location(1)`

No variants on any of them. Buttons, cards and the navbar are **copy-pasted frames**, which is why the Webflow side had to invent its own components.

### Auto-layout usage
**Mixed, and mostly absent at section level.** The hero's children are absolutely positioned (`absolute left-[529px] top-[513px]`, `-translate-x-1/2 absolute left-1/2`). Auto layout appears only inside small clusters — the nav bar (`flex items-center justify-between p-[16px] w-[1200px]`), button rows (`flex gap-[20px]`), chips (`flex gap-[8px] px-[12px] py-[8px]`), the footer form (nested `Frame 60`/`Frame 61` stacks with 30 px / 88 px rhythm). Content max width is **1200 px inside a 1440 frame** → a 120 px side gutter, which is the number the Webflow container had to reproduce.

### Asset types
- Photography: stock JPEG/PNG people shots with very long descriptive filenames straight from the stock site (`young-man-orange-t-shirt-wearing-rubber-gloves-holding-bucket-with-cleaning-tools-pointing-up-having-great-idea-smiling-confident-standing-green-background 1`, `portrait-smiling-korean-girl-holding-sorted-garbage-recycling-looks-happy-cares-nature 1`).
- SVG: all icons (Vuesax), the `Logomark`, the decorative `Grid` mesh, the big background `Ellipse 4/5/6/7` rings, and `Group 2` (the soft glow inside the orange button).
- Raster decoration: `Rectangle 21` (the 1440 × 164 fade strip at the bottom of the hero).

---

## 2. Webflow site anatomy

### Pages
| Page | Slug | Notes |
|---|---|---|
| `Home` | `/` | The only page. `seo: {}` (empty), `openGraph` all null/false — **no SEO title, no meta description, no OG image was ever set**. Created 2026-08-08, last updated 2026-08-25, last published 2026-08-25 16:29. |

- CMS collection pages: **none found** (`{"collections":[]}`).
- Utility pages: **none found** — no `style-guide`, no `licenses`/`licences`, no `404`, no `password`. (The Webflow Data API lists utility pages alongside normal ones; only `Home` came back.)
- `dataCollectionEnabled: false`, no custom domains, timezone `Asia/Karachi`.

### Class naming convention — a Client-First skeleton with a custom `block_element` body

**Verdict: a deliberate hybrid.** The page/section scaffolding, the typography scale and the utilities are lifted from **Finsweet Client-First** (with small renames); everything section-specific is a **custom `snake_case` / `kebab-case` BEM-ish system**. It is not Relume (no `layout###_component`, no `rl-` prefixes, no Relume components in the site).

Evidence for Client-First: `page-wrapper`, `main-wrapper`, `container-large`, `padding-global`, `padding-section-medium/small/xsmall`, `max-width-medium`, `text-size-small/regular/medium/large`, `heading-styles-h1…h6`, and the nesting order `section > padding-global > padding-section-* > container-large > content-wrapper`. Two tells that it is **not** stock Client-First: the class is `heading-styles-h2` (Client-First ships `heading-style-h2`, singular), and `.padding-vertical` here sets only `padding-top`.

30+ verbatim classes, grouped:

**Layout / structure (Client-First lineage)**
`page-wrapper`, `main-wrapper`, `padding-global`, `padding-vertical`, `padding-section-xsmall`, `padding-section-small`, `padding-section-medium`, `container-large`, `content-wrapper`, `max-width-medium`, `section-title-wrap`, `wrapper_hero`, `inner-flex`, `inner_lower-flex`, `lower_inner-flex`, `text-wrap`, `text_wrap`, `icon_wrap`, `100vh` (compiles to `._100vh`)

**Section wrappers (custom, `section_<name>`)**
`section_hero`, `section_marquee`, `section_packages`, `section_reviews`, `section_why`, `section_works`, `section_gallery`, `hero_background`, `hero_gradient`, `hero-bg`, `hero_image-wrap`, `hero_tag-wrap`, `heading_cover`

**Typography**
`heading-styles-h1`, `heading-styles-h2`, `heading-styles-h3`, `heading-styles-h5`, `heading-styles-h6`, `text-size-small`, `text-size-regular`, `text-size-medium`, `text-size-large`, `price_text`, `name_text`, `tag_text`, `tag-text`, `marquee-text`, `span-heading`, `span-marquee-text`, `span-orange-italic`

**Buttons**
`button-primary`, `button-secondary`, `button-group`, `button_bg`, `button_text-wrapout`, `button_text-wrapin`, `button_text-relative`, `button_text-absolute`, `button-arrow-wrap`

**Components / cards**
`navbar`, `nav_inner-flex`, `nav_menu`, `menu_inner`, `nav_link`, `nav_brand`, `nav_right-wrap`, `review_card`, `review_card-lower`, `review_lower-wrap`, `reviewer_image`, `why_card`, `why_card-title`, `why_card-bg`, `benefit_card`, `pkg_tabs-link`, `pkg_upper-wrap`, `pkg_upper-left`, `pkg_upper-right`, `pkg_lower-wrap`, `pkg_icon`, `pkg_icon-wrap`, `package_tabs`, `work_tabs`, `work_tabs-menu`, `work_tab-link`, `work_tabs-content`, `works_tab-wrapper`, `tab_link-inner`, `tab_link-inner-flex`, `tab_link-icon`, `tabs_pane`, `tabs_image-wrap`, `gallery_slider`, `gallery_slider-wrap`, `gallery-mask`, `gallery_image-grid`, `gallery_image-wrap`, `gallery_divider`, `gallery-tag`, `marquee_horizontal`, `marquee_vertical`, `track-horizontal`, `track-vertical`, `marquee-item`, `marquee-overlay`, `slider-arrow`, `slide`, `checkpoint`, `points_grid`, `stars-wrap`, `avatar-wrap`, `card_divider`, `card-count`, `count-absolute`, `section-tag`, `image_tag`, `divider-grey`, `divider-12px`, `polygon`, `review-bg`

**Utilities / modifiers (sizes baked into the name)**
`icon-size-16px`, `icon-size-18px`, `icon-size-20px`, `icon-size-24px`, `avatar-32px`, `gap-4px`, `gap-8px`, `gap-16px`, `spacer-8px`, `spacer-28px`, `square-4px`, `divider-12px`, `weight-light`, `weight-extra-light`, `weight-semibold`, `text-black`, `text-light-grey`, `text-medium-grey`, `text-dark-grey`, `hide`, `Global Styles` (`.global-styles`)

**Combo classes (`is-*` / `is_*`, always applied second)**
`.section-title-wrap.is_hero`, `.section-title-wrap.is_package`, `.section-title-wrap.is_review`, `.section-title-wrap.is_works`, `.section-title-wrap.is_gallery`, `.content-wrapper.is_packages`, `.content-wrapper.is_reviews`, `.content-wrapper.is_work`, `.content-wrapper.is-gallery`, `.heading-styles-h1.is-64px`, `.heading-styles-h2.is-64px`, `.heading-styles-h3.is-64px`, `.heading-styles-h5.is-64px`, `.heading-styles-h6.is-64px`, `.button-primary.is-arrow`, `.button-secondary.is_black`, `.text-size-medium.is-rating`, `.text-size-large.is-rating`, `.text-size-small.is-price`, `.text-size-small.text-black`, `.text-size-small.text-light-grey`, `.text-size-small.text-dark-grey`, `.tag-text.text-medium-grey`, `.image.is-cover`, `.image_tag.is_hero-2`, `.padding-global.relative`, `.button-group.margin-top`, `.slider-arrow.is-left`, `.slider-arrow.is-right`, `.review-bg.is-right`, `.why_card-bg.is-right`, `.polygon.lower`, `.tab_link-icon.absolute`, `.marquee-overlay.is_right`, `.gallery-tag.is--2` … `.gallery-tag.is--6`

**State classes:** none found beyond Webflow's own `w--current` / `w--tab-active`; hover states are drawn in CSS on the class itself or built structurally (see Buttons).

**Inconsistencies in the convention itself** (real, verbatim): both `tag_text` and `tag-text` exist; both `text-wrap` and `text_wrap` exist; `is_gallery` vs `is-gallery`, `is_package` vs `is_packages`, `is-right` vs `is_right`. Separator choice (`_` vs `-`) is not stable.

### DOM skeleton — body down to the hero (verbatim classes)

```
Body
  Block .page-wrapper
    Block .main-wrapper
      ComponentInstance "Global Styles"
      Block .hero_background
        NavbarWrapper .navbar
          Block .container-large
            Block .nav_inner-flex
              Block .nav_menu
                NavbarMenu .menu_inner
                  NavbarLink .nav_link   "Services"
                  NavbarLink .nav_link   "Pricing"
                  NavbarLink .nav_link   "Gallery"
                  NavbarLink .nav_link   "Reviews"
              NavbarBrand .nav_brand
                Image .image
              Block .nav_right-wrap
                ComponentInstance "Button Secondary"
                ComponentInstance "Button Primary"
              NavbarButton
                Icon
        Section<section> .section_hero
          Block .padding-global
            Block .padding-vertical
              Block .container-large
                Block .content-wrapper
                  Block .wrapper_hero
                    Block .section-title-wrap.is_hero
                      Block .hero_tag-wrap
                        Block .avatar-wrap
                          Image .avatar-32px   (x3)
                        Paragraph .text-size-medium      "+300 Ratings"
                        Block .divider-12px
                        Block .gap-4px
                          Paragraph .text-size-medium.is-rating  "4.9"
                          HtmlEmbed .icon-size-20px
                      Block .heading_cover
                        Heading<h1> .heading-styles-h1          "A Spotless" + Span .span-orange-italic "Home"
                        Heading<h1> .heading-styles-h1.is-64px  "Without Lifting a Finger"
                      Paragraph .text-size-regular  "Professional residential and commercial clean…"
                      Block .button-group.margin-top
                        ComponentInstance "Button Primary Arrow"
                        Link .button-secondary.is_black
                          Block .button_text-wrapout
                            Block .button_text-wrapin
                              Block .button_text-relative
                                Block > "Get an Instant Quote"
                              Block .button_text-absolute
                                Block > "Get an Instant Quote"
                          HtmlEmbed .icon-size-16px
                    Block .hero_image-wrap
                      Image .image
                      Block .image_tag
                        Block .tag_text
                          Span .weight-semibold "5K+" + "Cleaning Services"
                        HtmlEmbed .icon-size-18px
                      Block .image_tag.is_hero-2
                        Block .tag_text
                          Span .weight-semibold "100%" + "Satisfaction Guarantee"
                        HtmlEmbed .icon-size-18px
          Block .hero_gradient
        Image .hero-bg
```

Two H1s in a row (`heading-styles-h1` and `heading-styles-h1.is-64px`) — the two Figma text layers were each promoted to an `h1` rather than one heading with a line break. Accessibility/SEO smell, but it is their consistent pattern for two-tone display headings.

### A representative card grid section (packages)

```
Section<section> .section_packages
  Block .padding-global
    Block .padding-section-medium
      Block .container-large
        Block .content-wrapper.is_packages
          Block .section-title-wrap.is_package
            Block .section-tag
              HtmlEmbed .icon-size-16px
              Block > "Our Packages"
            Heading<h2> .heading-styles-h2
              "Choose the Clean" + Span .weight-extra-light "That Fits Your Life"
          Block .package_tabs
            Block .pkg_tabs-link
              Block .tab_link-inner-flex
                Block .pkg_upper-wrap
                  Block .pkg_upper-left
                    Block .pkg_icon-wrap
                      HtmlEmbed .pkg_icon
                    Block .gap-8px
                      Heading<h3> .heading-styles-h5   "Standard" + Span .weight-light > Emphasized "Clean"
                      Block .text-size-small           "Weekly or bi-weekly maintenance"
                  Block .pkg_upper-right
                    Block .text-size-small.is-price    "from"
                    Block .price_text                  "$120"
                Block .pkg_lower-wrap
                  Block .spacer-28px
                  Block .divider-grey
                  Block .lower_inner-flex
                    Block .lower_left
                      Paragraph .text-size-large  "What's Includes:"
                      Block .points_grid
                        Block .checkpoint  (x7)
                          HtmlEmbed .icon-size-16px
                          Paragraph > "Surface dusting" / "Appliance exteriors" / …
                    ComponentInstance "Button Primary"
```

Note: the design specs **three** packages (Standard / Deep / Move-In-Out, each with an expanded and a collapsed frame). **Only one `.pkg_tabs-link` was built**, and `.package_tabs` is a plain `Block`, not a Webflow Tabs widget — the accordion behaviour is unbuilt. `.tabs_menu-pkg` exists as a class with no element using it.

### Footer
**None found.** The page's last children are the gallery section and a bare `Block ._100vh`. The footer (with its four link columns, social row, legal links and copyright) exists only in Figma, as the bottom half of `7189:1661`.

### Navbar
- Native `NavbarWrapper` (Webflow's Navbar widget) with `.navbar`, containing `.container-large > .nav_inner-flex`.
- Left `.nav_menu > NavbarMenu .menu_inner` with four `NavbarLink .nav_link`; centre `NavbarBrand .nav_brand > Image .image` (the logomark PNG); right `.nav_right-wrap` holding the `Button Secondary` and `Button Primary` component instances.
- Mobile menu: the default Webflow `NavbarButton > Icon` hamburger, **left entirely at defaults** — no custom class on the button, no open/close interaction, no styling of the dropdown. Mobile nav is effectively untouched.
- `.navbar` CSS is a floating pill, matching the Figma nav card:
```css
.navbar{position:relative;z-index:12;width:100%;max-width:75em;margin-right:auto;margin-left:auto;
  padding:1em;border:1px solid var(--lightest-gray);border-radius:.5em;
  background-color:var(--off-white);box-shadow:0 4px 12px 0 rgba(0,0,0,.05)}
.nav_link{padding:.25em .63em;color:var(--charcoal-gray);
  font-size:var(--_responsive---text-size--small);line-height:125%;letter-spacing:-.02em;text-decoration:none}
```
No `:hover` state is defined on `.nav_link`.

### Buttons
Three Webflow components — `Button Primary`, `Button Secondary`, `Button Primary Arrow` — each with **exactly three props: `Link` (link, default `#`), `Text 1` (textContent, default "Book Your Cleaning"), `Text 2` (textContent, default "Book Your Cleaning")**. Two text props because the hover is a **structural text-swipe**: `.button_text-wrapout > .button_text-wrapin > .button_text-relative` (visible copy) + `.button_text-absolute` (duplicate copy offset below), so one copy slides out while the other slides in. No variants defined on any component.

```css
.button-primary{position:relative;padding:.56em .75em;border-radius:.25em;
  background-color:var(--brand-orange);background-image:url(<Group 2 (1).png>);
  background-size:cover;background-repeat:no-repeat;color:var(--off-white);
  font-size:.88em;line-height:130%;font-weight:500;text-align:center;letter-spacing:-.02em;text-decoration:none}

.button-secondary{position:relative;display:flex;padding:.56em .75em;justify-content:center;align-items:center;
  grid-column-gap:.5em;grid-row-gap:.5em;border:1px solid var(--soft-peach);border-radius:.25em;
  background-color:var(--peach-cream);color:var(--brand-orange);
  font-size:.88em;line-height:130%;font-weight:500;text-align:center;letter-spacing:-.02em;text-decoration:none}
```
Note `background-image:@img_6a77256a971741fc33018b1a` → the Figma `Group 2` glow SVG was exported as **`Group 2 (1).png` and applied as a cover background-image on the button class**, rather than rebuilt as a gradient. No `:hover` pseudo is declared on either button class (queried explicitly with `include_base_pseudos:["noPseudo","hover"]` — only `noPseudo` came back), confirming hover lives in the component's markup/interaction layer, and since there are **zero IX interactions on the site**, the swipe is currently inert.

Variants in use: `.button-primary.is-arrow`, `.button-secondary.is_black`, `.button-group.margin-top`.

### Variables in Webflow (3 collections, 35 variables)

`Colors` (18) — collection `collection-7763ffa8-…`, single Base mode:

| Name | CSS | Value | Figma source |
|---|---|---|---|
| Transparent | `--transparent` | `hsla(0,0%,0%,0)` | — |
| White | `--white` | `white` | — |
| Off White | `--off-white` | `#fefcfa` | hero/nav background |
| Black | `--black` | `black` | H1 |
| Brand Orange | `--brand-orange` | `#ea580c` | accent |
| Charcoal Gray | `--charcoal-gray` | `#646464` | nav links |
| Medium Gray | `--medium-gray` | `#7c7c7c` | H1 line 2 |
| Light Grey | `--light-grey` | `#9a9a9a` | — |
| Dark Gray | `--dark-gray` | `#727272` | body copy |
| Peach Cream | `--peach-cream` | `#fff5ef` | chip fill |
| Soft Peach | `--soft-peach` | `#ffede4` | chip border |
| Lightest Gray | `--lightest-gray` | `#f0f0f0` | all card/nav borders |
| Golden Yellow | `--golden-yellow` | `#eacc05` | star rating |
| Soft Ivory | `--soft-ivory` | `#f8f5f2` | — |
| Soft Cream | `--soft-cream` | `#faf7f4` | — |
| Warm White | `--warm-white` | `#fbf8f6` | — |
| Light Silver | `--light-silver` | `#cacaca` | — |
| Neutral Gray | `--neutral-gray` | `#e8e8e8` | — |

Naming note: **"Gray" and "Grey" are both used** (`Charcoal Gray`, `Medium Gray`, `Dark Gray`, `Neutral Gray`, `Light Silver` vs `Light Grey`). Four near-identical off-whites (`#fefcfa`, `#fbf8f6`, `#faf7f4`, `#f8f5f2`) were all tokenised although only `Off White` is used on the page.

`Font Family` (1) — `Inter Display` → `--_font-family---inter-display: Interdisplay`.

`Responsive` (16) — collection `collection-edb7cd1c-…`, with **four modes: `Base mode`, `Tablet`, `Mobile Landscape`, `Mobile Portrait`**. All sizes are **`em`**:

| Variable | CSS name | Base | Tablet | Mob-L | Mob-P |
|---|---|---|---|---|---|
| Text Size/small | `--_responsive---text-size--small` | .875em | .875em | .875em | .875em |
| Text Size/Regular | `--_responsive---text-size--regular` | 1em | 1em | 1em | 1em |
| Text Size/Medium | `--_responsive---text-size--medium` | 1.125em | 1.125em | 1.125em | 1.125em |
| Text Size/Large | `--_responsive---text-size--large` | 1.25em | 1.25em | 1.25em | 1.25em |
| Heading/H1 | `--_responsive---heading--h1` | 4.5em | 4.5em | 4.5em | 4.5em |
| Heading/H1 64px | `--_responsive---heading--h1-64px` | 4em | 4em | 4em | 4em |
| Heading/H2 | `--_responsive---heading--h2` | 3em | 3em | 3em | 3em |
| Heading/H3 | `--_responsive---heading--h3` | 2em | 2em | 2em | 2em |
| Heading/H5 | `--_responsive---heading--h5` | 1.5em | 1.5em | 1.5em | 1.5em |
| Heading/H6 | `--_responsive---heading--h6` | 1.25em | 1.25em | 1.25em | 1.25em |
| Padding Global/Padding Global | `--_responsive---padding-global--padding-global` | 2.5em | 2.5em | 2.5em | 2.5em |
| Container/Container Large | `--_responsive---container--container-large` | 75em | 75em | 75em | 75em |
| Padding Section/xsmall | `--_responsive---padding-section--xsmall` | 2.5em | 2.5em | 2.5em | 2.5em |
| Padding Section/Small | `--_responsive---padding-section--small` | 3.75em | 3.75em | 3.75em | 3.75em |
| Padding Section/Regular | `--_responsive---padding-section--regular` | 5em | 5em | 5em | 5em |
| Padding Section/Medium | `--_responsive---padding-section--medium` | 7.5em | 7.5em | 7.5em | 7.5em |

There is no `Heading/H4` (the design never needed one) — hence `heading-styles-h5` being applied to `h3` elements in the packages card.

### Typography — tag → class → CSS
Root: `body` carries the font, colour and background; heading/text classes supply size.

```css
body{background-color:var(--off-white);font-family:var(--_font-family---inter-display);
     color:#333;font-size:14px;line-height:20px}

.heading-styles-h1{color:var(--black);font-size:var(--_responsive---heading--h1);
                   line-height:110%;letter-spacing:-2.88px}
.heading-styles-h2{color:var(--black);font-size:var(--_responsive---heading--h2);
                   line-height:110%;letter-spacing:-1.92px}
.heading-styles-h3{color:var(--black);font-size:var(--_responsive---heading--h3);
                   line-height:120%;letter-spacing:-.04em}
.text-size-large  {color:var(--black);font-size:var(--_responsive---text-size--large);
                   line-height:115%;font-weight:500;letter-spacing:-.03em}
.text-size-medium {color:var(--medium-gray);font-size:var(--_responsive---text-size--medium);
                   line-height:150%;letter-spacing:-.02em}
.text-size-regular{color:var(--dark-gray);font-size:var(--_responsive---text-size--regular);
                   line-height:145%;letter-spacing:-.02em}
.text-size-small  {color:var(--medium-gray);font-size:var(--_responsive---text-size--small);
                   line-height:145%;letter-spacing:-.02em;text-decoration:none}
```

`h1`/`h2`/`h3` and `p` tag selectors exist but are Webflow defaults; **all real typography is class-driven**, never tag-driven. Body `color:#333` is a leftover Webflow default that nothing inherits because every text class sets its own colour.

**Per-breakpoint values:** none of the heading/text classes carry breakpoint overrides. Sizing changes are meant to come from the variable modes — and those are currently identical across modes, so **desktop, tablet, mobile-landscape and mobile-portrait typography are all the same right now**. The mechanism is wired, the numbers are not.

### Spacing system
```css
.padding-global{width:100%;padding-left:var(--_responsive---padding-global--padding-global);   /* 2.5em */
                padding-right:var(--_responsive---padding-global--padding-global)}
.container-large{width:100%;max-width:var(--_responsive---container--container-large);          /* 75em */
                 margin-left:auto;margin-right:auto}
.padding-section-xsmall{padding-top:var(--…--xsmall);padding-bottom:var(--…--xsmall)}           /* 2.5em */
.padding-section-small {padding-top:var(--…--small);padding-bottom:var(--…--small)}             /* 3.75em */
.padding-section-medium{padding-top:var(--…--medium);padding-bottom:var(--…--medium)}           /* 7.5em */
.padding-vertical{padding-top:var(--_responsive---padding-section--regular)}                     /* 5em, TOP ONLY */
.page-wrapper{}      /* no properties — structural only */
.content-wrapper{}   /* no properties — structural only */
```
Gap utilities are literal: `.gap-4px`, `.gap-8px`, `.gap-16px`, `.spacer-8px`, `.spacer-28px`; grid gaps inside components are em (`.points_grid` → `.75em`, `.track-horizontal` → `2em`, `.review_card` → `1.25em`).

Section-padding assignment on the Home page:
| Section | padding class |
|---|---|
| `.section_hero` | `.padding-vertical` (5em top only) |
| `.section_marquee` (1st) | `.padding-section-small` |
| `.section_packages` | `.padding-section-medium` |
| `.section_reviews` | `.padding-section-medium` |
| `.section_why` | `.padding-section-small` |
| `.section_works` | `.padding-section-medium` |
| `.section_marquee` (2nd) | `.padding-section-xsmall` |
| `.section_gallery` | `.padding-section-medium` |

**Rule observed: `padding-section-medium` is the default for a content section; `small`/`xsmall` for strips; the hero is the only section that uses `padding-vertical`.** The two marquee sections deliberately skip `.padding-global` and `.container-large` so the track can bleed full width.

### Responsive behaviour
- Breakpoints customised: **only through variable modes on `body`.** The only breakpoint-specific CSS in the whole site is:
```css
/* body, breakpoint "medium" */ ---mode--collection-edb7cd1c…: mode-cf43c38d…  /* Tablet */
/* body, breakpoint "small"  */ ---mode--collection-edb7cd1c…: mode-8d995fcd…  /* Mobile Landscape */
/* body, breakpoint "tiny"   */ ---mode--collection-edb7cd1c…: mode-851761ba…  /* Mobile Portrait */
```
- No class in the site carries a `medium`/`small`/`tiny` override (every `query_styles` result came back with a `base` block only).
- **Consequence: the site is desktop-only today.** Because every internal size is `em`/`%` and the modes are un-tuned, nothing reflows: `.container-large` stays 75em, `.why_card` keeps `padding:8.25em 7.5em 8.25em 10em`, `.review_card` keeps `width:20em`, `.points_grid` keeps `grid-template-columns:1fr 1fr`. This is clearly "responsive pass not started yet" rather than a design decision.

### Components
| Component | Instances on Home | Props |
|---|---|---|
| `Global Styles` | 1 (first child of `.main-wrapper`) | none |
| `Button Primary` | 2 | Link, Text 1, Text 2 |
| `Button Secondary` | 1 | Link, Text 1, Text 2 |
| `Button Primary Arrow` | 3 | Link, Text 1, Text 2 |

No variants on any component. Cards, the navbar and sections are **not** components — they are repeated markup with shared classes.

### CMS
**None.** No collections, so the packages, reviews, gallery projects and "how it works" steps are all **static, hand-duplicated markup**. Service content is likewise static: there is no Services page and no `Services` collection — the five Figma pages were to be five static pages, not CMS templates. (Inference, marked as such: given zero collections and a template whose Figma "Services" page is a bespoke one-off layout rather than a repeating item layout, the team's plan appears to have been static pages throughout.)

### Interactions / animations
- Webflow IX: `list_interactions` → `{"items":[],"total":0}` — **zero interactions on the whole site.**
- No GSAP, no Lottie, no registered scripts (`get_registered_scripts` → `total:0`).
- The marquee is built structurally (`.marquee_horizontal > .track-horizontal` with 22 `.marquee-item`s, plus `.marquee-overlay` / `.marquee-overlay.is_right` fade masks and a `.track-horizontal-copy` class prepared for the duplicate track) **but has nothing animating it** — no IX, no keyframes in custom code. Same for `.marquee_vertical > .track-vertical` in the "why" section (12 `.benefit_card`s).
- The only animation actually declared anywhere is a CSS transition on the tab link:
```css
.work_tab-link{…transition-property:padding;transition-duration:350ms;transition-timing-function:ease;…}
```
- Native widgets in use: **Tabs** (`work_tabs` → 4 `TabsLink` / 4 `TabsPane`) for "How CleanFlow Works", and **Slider** (`gallery_slider` → `gallery-mask`, 4 `SliderSlide`, `.slider-arrow.is-left/.is-right`, `SliderNav .hide`) for the proof gallery. No third-party slider library.

### Custom code
- Site-wide head: `""`. Site-wide footer: `""`.
- Page (Home) head, **verbatim and complete**:
```html
<style>
.text_wrap {
  width: 100% !important;
}
</style>
```
- Page footer: `""`.
That one `!important` patch is the entire custom-code footprint of the site.

### Fonts
Eight **uploaded custom `.ttf` files** (not Google Fonts, not Adobe), all family `Interdisplay`, `font-display:swap`, uploaded 2026-08-08 09:15 in a single batch:
`InterDisplay-Thin.ttf` (100), `-ExtraLight` (200), `-Light` (300), `-Regular` (400), `-Medium` (500), `-SemiBold` (600), `-Bold` (700), `-ExtraBold` (800).
**No italic face was uploaded** — yet the design uses `Inter Display Light Italic` and `Italic` for the orange accent words, and the build uses `Emphasized` (`<em>`) elements plus `.span-orange-italic`. Those render as a **browser-synthesised oblique**, not the real italic cut.

### Images
- 34 assets total (8 of them the font files), **all at the site root — `folderId: null` on every asset, no asset folders created.**
- Naming is **raw Figma export names, duplicates and all**: `Frame 27.webp`, `Frame 27 (1).webp` … `Frame 27 (7).webp`, `Frame 29.webp`, `Frame 29 (1..3).webp`, `Mask group (1).webp`, `Mask group (3).webp`, `Group 5.webp`, `Ellipse 1/2/3.png`, `Polygon 1.png`, `Polygon 1 (1).png`, `Polygon 2.png`, `Rectangle 22.png`, `Logomark (3).png`, `Frame 1.svg`, `quote-up.svg`. One asset is mislabelled: `displayName "Polygon 1.png"` over `originalFileName "…_Mask group.png"`.
- Formats: **photography and photo-composites exported as `.webp`** (42 KB–232 KB); small decorative shapes as `.png`; only two `.svg` files uploaded.
- Responsive images: Webflow's automatic `-p-500 / -p-800 / -p-1080 / -p-1600 / -p-2000 / -p-2600` variants exist on the larger webp/png assets; nothing hand-tuned.
- **`altText` is `null` on every single asset** — no alt text anywhere on the site.
- Icons are **not** `<img>`: 103 `HtmlEmbed` elements carry inline SVG, sized by `.icon-size-16px/18px/20px/24px` (and `.pkg_icon`, `.tab_link-icon`). Photos and the logo are `Image` elements bound by `assetId`.

### Forms, links, SEO
- **Forms: none found.** `list_forms` was rate-limited on first attempt; independently, the element tree contains no `FormForm`/`FormBlockWrapper`/`FormTextInput` node of any kind, and the section that would host the form (CTA + quote form + footer) is not built. **The quote form exists only in Figma** — see §"Quote/contact form" below.
- Links: the only hand-built `Link` element on the page is the hero's `.button-secondary.is_black`; everything else is a button component whose `Link` prop still defaults to `#`. `NavbarLink`s have no destinations (there is nowhere to go — one page).
- SEO: page `seo:{}`, `openGraph` empty, no JSON-LD, `dataCollectionEnabled:false`, no `googleTagIds`.

---

### Extra: the four items specifically requested

**Style-guide page — not found.** No `style-guide` page, no `styleguide` classes, no swatch/typography specimen markup anywhere in the element tree. What plays that role is the `Global Styles` component (a single empty-styled `.global-styles` block dropped as the first child of `.main-wrapper`) plus the three variable collections. The team's "style guide" for this project is the variable panel, not a page.

**Licences page — not found.** No `licenses`/`licences`/`attribution` page or classes. The template ships no licensing/attribution page yet, which for a template using Vuesax icons and stock photography is a gap.

**Quote / contact form — designed, not built.** In Figma it lives in the shared bottom section (`7189:1661`, the frame duplicated onto all five pages). Structure, verbatim layer names and geometry:
```
Frame 28 (1200x688)  ← footer + form container, x=120 in a 1440 frame
  Frame 67 (480x462)          ← the form column
    Frame 66 (480x394)        ← field stack
      Frame 61  "Full Name"          label 18px + Frame 60 input 480x42
      Frame 64  → Frame 62 "Email" (230x72) + Frame 63 "Phone Number" (230x72)   ← 2-up row, 20px gutter
      Frame 65  → Frame 62 "Service Type" (select, chevron-down instance at x=200)
                 + Frame 63 "Preferred Date" (select, chevron-down instance)
      Frame 66  "Notes (optional)"   textarea 480x100, pajamas:resize grip bottom-right
    Frame 3 (164x36)  submit          "Book My Cleaning" + Frame 5/Frame 4 arrow square
```
Placeholders verbatim: `Enter full name...`, `Enter email...`, `Enter phone number...`, `Select service type`, `Select date`, `Notes (special requests, pets, access details)...`. Inputs are 42 px tall, labels sit 30 px above each input, field groups are 88 px apart, the submit is 426 px below the stack. Eyebrow above the CTA reads `Quote Form` with a `favorite-chart` icon; heading `Your Clean Home Is One Click Away`. **None of this exists in Webflow yet** — no form block, no success/error states, no field classes.

**Service pages — CMS vs static: static, and unbuilt.** Zero CMS collections exist. The Figma `Hi-Fi - CleanFlow - Services` page (1440 × 6340) is a bespoke six-section layout (hero, a `Standard Clean` block 1440 × 1149, a 1440 × 760 band, a 1440 × 1467 band, a 1440 × 741 band, then the shared 1440 × 1423 CTA+footer) — a single marketing page about services, not a repeating item template. The supporting shelf section `Services - Residential Services` (1640 × 3847) holds per-service card states. On the Home page, services appear as the static `.package_tabs` block (one `.pkg_tabs-link` built of three designed), with each package's inclusions as seven hand-written `.checkpoint` divs. The footer's `Services` column lists six services (Standard Clean, Deep Clean, Move-In / Move-Out, Office Cleaning, Airbnb Turnover, Post-Construction Cleaning) as plain text links — again, no collection behind them.

---

## 3. Figma → Webflow mapping observed

| Design concept (Figma) | Realised in Webflow as | Changed / notes |
|---|---|---|
| Page frame `Hi-Fi - CleanFlow - Home` (1440 wide) | Page `Home`, slug `/` | The other four page frames were **not** built. |
| Section frame (all named `CleanFlow - Hero`) | `Section<section> .section_<semantic-name>` | **Names were invented at build time** (`section_packages`, `section_reviews`, `section_why`, `section_works`, `section_gallery`, `section_marquee`) because the Figma names were useless. This is the single biggest translation step. |
| 1440 frame, content inset 120 px | `.padding-global` (2.5em ≈ 40px) + `.container-large` (max-width 75em = 1200px) | **Changed:** the design's fixed 120 px gutter became `max-width:1200px + auto margins + 40px safety padding`. The 1200 px content width was preserved exactly (75em @ 16px). |
| Figma "sections" shelf (`Home - Packages` expanded/minimised frames) | Intended as an accordion; built as a single static `.pkg_tabs-link` block | **Dropped/simplified:** collapsed states and items 2–3 not built. |
| Figma text layer 72px/`-2.88px` | `h1.heading-styles-h1` → `font-size:var(--…heading--h1)` (4.5em), `letter-spacing:-2.88px` | Size tokenised; **letter-spacing copied as a literal px value from Figma's `tracking-[-2.88px]`** instead of being converted to `-0.04em` like `.heading-styles-h3` was. |
| Figma text layer 64px second line | `h1.heading-styles-h1.is-64px` → 4em | Second display line became a **second `h1`**, not a `<br>`. |
| Orange italic accent word (`Inter Display Light Italic` `#ea580c`) | `Span .span-orange-italic`, and elsewhere `Span .weight-light > Emphasized` / `Span .weight-extra-light` | **Changed:** no italic font file was uploaded, so `<em>` renders synthetic oblique. |
| Raw hexes on layers (no Figma variables) | 18 named Webflow colour variables | **Tokens were created on the Webflow side, not in Figma.** Names invented at build time (`Brand Orange`, `Peach Cream`, `Soft Peach`, `Lightest Gray`). |
| Figma has no size tokens | `Responsive` collection, 16 em-based size variables, 4 modes bound to body breakpoints | Pure Webflow-side invention; the team's standard scaffold. |
| Auto-layout row `flex gap-[20px] items-center` | `.button-group` (+`.margin-top` combo) | 1:1. |
| Auto-layout `p-[16px]` nav card with 8px radius, `0 4px 6px rgba(0,0,0,.05)` | `.navbar` `padding:1em;border-radius:.5em;box-shadow:0 4px 12px 0 rgba(0,0,0,.05)` | **Changed:** shadow blur 6→12, and padding/radius converted from px to em (16px→1em, 8px→.5em) assuming a 16px base. |
| Figma absolute-positioned hero composition | `.wrapper_hero` + `.hero_image-wrap` + absolutely-positioned `.image_tag` / `.image_tag.is_hero-2` chips, `.hero_gradient`, `.hero-bg` | Absolute positioning was **kept**, not re-flowed. |
| Big decorative `Ellipse 4/5/6/7` SVG rings + `Grid` mesh | Exported as raster `Ellipse 1/2/3.png`, `Mask group (1)/(3).webp`, `Group 5.webp`; placed as `Image` elements (`.hero-bg`, `.review-bg`, `.why_card-bg`, `.polygon`) | **Changed:** vector → raster. `Mask group (1).webp` is 106 KB, `Group 5.webp` 232 KB. |
| `Group 2` glow inside the orange button | `background-image` on `.button-primary` from `Group 2 (1).png` | Vector → PNG background. |
| Vuesax icon instances | 103 `HtmlEmbed` inline-SVG elements sized by `.icon-size-16px/18px/20px/24px` | Consistent and clean; icons are never `<img>`. |
| Stock photos with long descriptive filenames | Re-exported from Figma as `Frame 27 (n).webp` / `Frame 29 (n).webp` | **Changed for the worse:** every descriptive filename was lost; alt text never added. |
| Hover states | Not drawn in Figma at all | Invented in Webflow as the `button_text-wrapout/wrapin/relative/absolute` text-swipe structure + a 350ms padding transition on `.work_tab-link`. |
| Four tab states drawn as four 1440×1002 frames (`Home - How It Works`) | One native Webflow `TabsWrapper` with 4 `TabsLink` + 4 `TabsPane` | Correct consolidation of duplicated frames into one widget. |
| Gallery project frames (`Gallery - Projects` shelf) | Native `SliderWrapper` with 4 `SliderSlide`, custom `.slider-arrow.is-left/.is-right`, `SliderNav .hide` | Slider dots hidden via `.hide` rather than disabled in settings. |
| Marquee strip (1440 × 146 / 1440 × 106 bands) | `.section_marquee > .padding-section-small > .content-wrapper > .marquee_horizontal > .track-horizontal` with 22 `.marquee-item`s | Structure built, **motion not built** (no IX). |
| Breakpoint frames | — | **None existed**, so tablet/mobile were never designed; the Webflow variable modes exist but hold desktop values. |
| Footer + quote form (frame `7189:1661`, on all 5 pages) | — | **Not built.** |

No Slack/Notion source was visible to me, so "why" above is inferred from the artefacts only and marked as such.

---

## 4. Quality notes and gotchas

**Rules that look deliberate (repeat ≥ 3 times):**
1. **Fixed section skeleton:** `section.section_<name> > .padding-global[.relative] > .padding-section-<size> > .container-large > .content-wrapper[.is_<name>]`. Used identically in packages, reviews, why, works and gallery. Full-bleed strips (marquee) legitimately skip `padding-global` + `container-large`.
2. **Every section opens with `.section-title-wrap` + a `.is_<section>` combo**, containing a `.section-tag` eyebrow (`HtmlEmbed .icon-size-16px` + label) and then the `h2.heading-styles-h2`.
3. **Two-tone headings are one heading with a weight span:** `"Choose the Clean" + Span .weight-extra-light "That Fits Your Life"`, `"Trusted in Homes" + Span .weight-extra-light "Like Yours"`, `"Real Results" + Span .weight-extra-light "from Real Homes"`. In cards the pattern becomes `Span .weight-light > Emphasized`.
4. **Icons are always inline SVG in an `HtmlEmbed` with an `icon-size-*px` class** — never an image, never a font icon.
5. **Everything internal is sized in `em`** (paddings, radii, gaps, widths like `.review_card{width:20em}`), so the whole page scales from the body font size. Only literal utility classes (`gap-16px`, `spacer-28px`) and two letter-spacings use px.
6. **Buttons are always component instances with `Text 1` + `Text 2`** so the hover swipe has both copies.
7. **`Global Styles` component is the first child of `.main-wrapper` on the page.**
8. **Borders are always `1px solid var(--lightest-gray)` and card shadows are always `0 4px 20px 0 rgba(0,0,0,.05)`** (`.review_card`, `.why_card`); the nav uses `0 4px 12px`.

**Things that look like mistakes or unfinished work:**
- **11/11 Figma section frames named `CleanFlow - Hero`.**
- **Placeholder content never replaced:** all 10 `.review_card`s carry the identical quote `"I've tried four different cleaning services. "` (note trailing space) and the identical name `James Whitney`; `.section_works` and `.section_gallery` share the same intro paragraph `"Know exactly what to expect before, during, a…"`.
- Only 1 of 3 designed package cards built; `.package_tabs` is a plain div, so the accordion does nothing; `.tabs_menu-pkg` is an orphan class.
- **Orphan classes with no element using them:** `tabs_menu-pkg`, `track-horizontal-copy`, `link_detail`, `why_slider`, `div-block` (plus `Div Block`'s default name never cleaned up).
- Duplicated near-identical classes: `tag_text` / `tag-text`; `text-wrap` / `text_wrap`; `text-light-grey` exists both as a global class and as a combo on `.text-size-small`.
- `.padding-vertical` sets `padding-top` only — a section that uses it has no bottom padding.
- `.heading-styles-h5` applied to an `h3` element (packages card) — the H4 token is missing from the scale.
- Two `h1`s in the hero.
- **No alt text on any asset; no SEO title/description/OG image; no asset folders.**
- The `!important` patch `.text_wrap{width:100% !important}` in page head code instead of fixing the class.
- A stray `._100vh{height:100vh}` block at the very end of the page — a placeholder holding space for the unbuilt CTA/footer.
- Uploaded fonts are `.ttf` (~400 KB each × 8 = ~3.2 MB) rather than `.woff2`; italic cuts missing.
- **Responsive pass not started:** zero breakpoint overrides, all four variable modes identical.

**Special features present:** native Tabs, native Slider, two marquee scaffolds (horizontal + vertical), floating pill navbar, decorative absolute-positioned background imagery with `z-index` stacking (`.section_hero{z-index:10}`, `.navbar{z-index:12}`, `.why_card-title{z-index:2}`).
**Not present:** RTL, localisation (single locale), dark mode (colour collection has one Base mode only), sticky elements, accordions, modals, video, Lottie, GSAP, any third-party library.

---

## 5. Verbatim dumps

Omitted from the skill copy; the full dump lives in the analysis workspace.

## 6. Tool quirks

- **Egress proxy blocks `*.webflow.io` and `figma.com` entirely.** `curl -sL https://cleanflow-cb3c1a.webflow.io/` → exit 56, `connect_rejected (the egress proxy denied the CONNECT (organization policy))`; `WebFetch` → `{"error_type":"EGRESS_BLOCKED","domain":"cleanflow-cb3c1a.webflow.io"}`. The proxy status endpoint shows the same rejection for other analyses' hosts (`medicini-plus.webflow.io`, `furec.webflow.io`), so **the "fetch the live HTML and CSS" half of the brief is impossible in this environment**. Everything here comes from the MCP APIs instead. Budget for that: the element tree + style + variable tools cover the same ground, but you cannot see compiled CSS (e.g. `.w-…` widget CSS, real computed px values) or the published `<head>`.
- **Same block applies to `mcp__Figma__get_screenshot`:** it returns a `https://www.figma.com/api/mcp/asset/….png` URL plus curl instructions, and the curl is refused (`exit 56`). **No screenshots could be saved to the run folder.** The workaround, untested here to save context, is `enableBase64Response:true` (inline image, no file on disk).
- **`mcp__Figma__get_metadata` on a whole page blows the token budget:** page `0:1` returned 303,823 characters and was auto-spilled to `…/tool-results/mcp-Figma-get_metadata-1790016897634.txt`. Parse it with python/jq (it is `[{type,text}]` with the XML in `text`), don't try to read it.
- **`mcp__Figma__get_variable_defs` returns `{}` when a file has no variables** — that is a real answer, not an error. Don't retry it.
- **Webflow MCP rate limiting is aggressive and endpoint-specific.** Exact errors seen:
  - `Tool get_all_elements failed: GET /v2/assets returned 429` (the element tool internally hits `/v2/assets`; it failed three times over ~4 minutes before succeeding)
  - `{"name":"Error","message":"Too Many Requests"}` from `data_style_tool` and `data_variable_tool`
  - `{"name":"WebflowApiError","message":"Too Many Requests","status":429,"code":"too_many_requests"}` from `data_forms_tool` and `data_scripts_tool`
  There is no `Retry-After`. What worked: **one action per call, small queries, and a ~40–45 s back-off between failures** (`python3 -c "import time;time.sleep(45)"` — the harness blocks a foreground `sleep`). A 5-query `query_styles` with `include_breakpoints` failed repeatedly where the same queries **without** `include_breakpoints` succeeded; breakpoint-inclusive queries are markedly heavier.
- **`data_element_tool > get_all_elements` output is huge** (189,800 chars for one page, one line) and gets spilled to a file. Best call shape that worked:
  `actions:[{label,get_all_elements:{depth:-1}}]` with `pageId` + `siteId`, then parse the JSON locally.
- **`data_style_tool > get_styles {query:"all", include_properties:false}` returned all 190 styles in one shot** and is the cheapest way to get the class inventory; add properties only via targeted `query_styles`.
- **`query_styles` `name_path` is a substring match**, so `["padding-section"]` returns all three padding-section classes and `["why_card"]` also returns `why_card-title` and `why_card-bg` — useful, but watch the `limit` (default 50).
- **`get_styles` does not tell you which classes are actually used.** Cross-reference against the element tree to find orphans (that is how the five unused classes above were found).
- **Element-tree nodes do not include `HtmlEmbed` markup or image alt text** — `"svg"` appears 0 times in the 189 KB tree. Images expose only `settings.assetId`; join against `data_assets_tool > list_assets` for names and `altText`.
- **`data_sites_tool > list_sites` returns 195 sites paginated at 50** — go straight to `get_site` with a known id instead.
- Webflow MCP session handling worked as documented: `session_id:"start"` on the first call issued `ses_3JeM9Ce2md2nd5FAvfLgR00S8U8`, reused on every later call with `agent_id: fable-5.1|claude-code|q7x2m`.
