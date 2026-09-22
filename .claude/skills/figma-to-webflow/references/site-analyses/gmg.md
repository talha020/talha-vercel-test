# GMG Ingenieurgesellschaft
Figma: https://www.figma.com/design/qV3DGBOElz4e35sMZiHSaj/GMG-Ingenieurgesellschaft (file key `qV3DGBOElz4e35sMZiHSaj`)  |  Webflow site id: `68d437e4c634ef38eb6b907c` ("Copy of GMG", shortName `gmg-627677-887e417c736414d16bf09a58542a`)  |  Live: `https://gmg-627677-887e417c736414d16bf09a58542a.webflow.io/` — **NOT REACHABLE from this environment** (egress proxy returns `connect_rejected` / `EGRESS_BLOCKED` for every `*.webflow.io` host; the fallback `https://gmging-neu.webflow.io/` is blocked identically)  |  Analysed: 2026-09-21

> **Scope caveat, read this first.** Two hard limits shaped this document and both must be carried into the next agent's instructions:
> 1. **The live site could not be fetched.** All `*.webflow.io` hosts are blocked by the agent egress proxy, so the richest evidence source named in the brief (rendered HTML + `webflow.css`) was unavailable. Every class name and CSS value below comes from the Webflow MCP Designer API instead, which is *more* authoritative for class names but gives no rendered DOM, no text content and no `<link>`/`<script>` tags.
> 2. **The Webflow site this token can reach is a stripped workspace copy.** `list_pages` returns exactly **one** page ("Home", `/`) and `get_collection_list` returns **zero** CMS collections. The publikationen / aktuelles / projekte / karriere pages and their collections do not exist in this copy. Everything said about those pages below is sourced from **Figma only**, or explicitly marked as not verifiable.
> Consequently the CMS-filter question (the reviewer's recurring bug) is answered from design evidence + absence-of-evidence, not from a built implementation. See §2 "CMS" and §4.

Run folder: `/tmp/claude-0/-home-user-talha-vercel-test/10672cda-24bd-508e-8a36-0bcc891af513/scratchpad/figma-to-webflow-workspace/analysis/gmg/`
- `wf-home-tree.txt` — full rendered Webflow homepage element tree (415 lines)
- `sub.py` — helper that slices any node subtree out of the cached Figma metadata dump
- Figma metadata dump (1,590,048 chars): `/root/.claude/projects/-home-user-talha-vercel-test/10672cda-24bd-508e-8a36-0bcc891af513/tool-results/mcp-Figma-get_metadata-1790016943920.txt`
- No screenshots were saved: the Figma seat hit its MCP tool-call cap after 2 calls (see §6).

---

## 1. Figma file anatomy

### Pages in the file
**There is exactly one page: `0:1` "Page 1".** No separate design / dev / components / archive pages. Everything — every page design, every iteration, the icon scraps — lives on one canvas.

### How the canvas is organised: dated iteration rows
The canvas is laid out as a **grid of iteration rows**. Each row is a full set of page frames at the same `y`, separated from the next row by a full-width horizontal `<line>` and labelled with a **date text node whose layer name is the date itself**. Verbatim:

| Row `y` | Divider line | Date label (layer name) | Contains |
|---|---|---|---|
| `0` | — | *(none — original)* | Publikationen, Aktuelles, Projekte, Aktuelles Inner, Mainbrücke Schwanheim |
| `11340` | `8:2783 "Line 13"` | `8:2784 "12/1/2026"` | full page set + Hero Slider + Unternehmen |
| `27811` | `54:4961 "Line 14"` | `54:4962 "24/1/2026"` | full page set + Leistungen added |
| `45183` | `77:3310 "Line 15"` | `77:3311 "27/1/2026"` | full page set |
| `46190` @ `x=39043` | `381:2558 "Line 17"` | `381:2559 "2/2/2026"` | full page set (parked far right) |
| `61951` | `135:2775 "Line 16"` | `135:2776 "8/5/2026"` | **latest** — full page set + Mitarbeiter + 2nd Hero Slider |

Dates are `D/M/YYYY`. The newest row (`8/5/2026`, `y=61951`) is the **"build from" source**. Note the rows are *not* in date order along the y-axis (2/2 sits after 27/1 but is parked at `x=39043`), so "latest = bottom row" only holds if you read the date labels.

### Breakpoints in Figma
**None.** Every page frame is `width="1920"`. There is no mobile, tablet or 1440 frame anywhere in the file (top-level width histogram: `1920` × 73, then only card/icon widths). Responsive behaviour was invented entirely in Webflow — see §3 and §4.

### Page frames in the latest row (verbatim names)
`GMG - Homepage Design` (`135:389`) · `GMG - Publikationen ` (`135:758`, note trailing space) · `GMG - Aktuelles` (`135:1139`) · `GMG - Projekte` (`135:1477`) · `GMG - Karriere` (`135:1843`) · `GMG -  Aktuelles Inner` (`135:2196`, double space) · `GMG -  Technischer Zeichner/Konstrukteur (m/w/d)` (`135:2285`) · `GMG -  Bauingenieur/Tragwerksplaner (m/w/d)` (`135:2393`) · `GMG -  Bauingenieur/Tragwerksplaner ` (`135:2501`) · `GMG - Mainbrücke Schwanheim` (`135:2609`) · `GMG - Hero Slider` (`135:2699`, `416:557`) · `GMG - Unternehmen` (`135:2777`) · `GMG - Leistungen` (`135:3319`) · `GMG - Mitarbeiter` (`381:4002`)

Naming pattern: `GMG - <German page name>`. Detail/CMS-item pages are named after **one concrete item** (`GMG - Mainbrücke Schwanheim`, `GMG - Technischer Zeichner/Konstrukteur (m/w/d)`) rather than `… Template`. Leading/trailing/double spaces are common and inconsistent.

### Homepage section frames, top to bottom (verbatim, `135:389`)
```
hero            135:571   1920 × 980
  bg-img        135:572     hero-img / shadow / blurs
  hero-text     135:584
  nav           135:594     nav > nav-main > buttons ; btn ; logo
  card-nav      135:619     card-project slide / slider-time / arrows
services        135:493   1880 × 890   text(label + h) ; cards (3) ; cards (3) ; btn 416:632
about           135:480   1920 × 860   logo-pattern ; text ; btn
mission         135:474   1900 × 980   text(label) ; text ; Rectangle 5
facts           135:446   1920 × 540   logo-pattern ; nuumbers (4 × number) ; text(label)
news            135:390   1880 × 809   text(label + h) ; btn ; news (3 × card-news)
Group 1577709352 135:647            <- wrapper holding the last two sections
  Career        135:692   1920 × 980   img ; text ; logo-pattern ; card-project slide ; btn
  footer        135:648   1920 × 911   left ; © text ; policy ; logo-pattern ; logo ; links footer
```
Note `nuumbers` (sic) at `135:450` — a typo preserved in the file. Note also that the sections are **not in visual order in the layer list**; they are ordered `news, facts, mission, about, services, hero, Group…`, i.e. roughly reverse.

### Layer-naming vocabulary inside frames
Lowercase, short, English, reused across every page: `hero`, `bg-img`, `hero-img`, `shadow`, `blurs`, `hero-text`, `nav`, `nav-main`, `buttons`, `logo`, `logo-sign`, `logo-pattern`, `btn`, `arrow`, `card`, `card bg`, `card-news`, `card-project slide`, `cards`, `photo`, `text`, `label`, `center`, `date`, `links`, `links footer`, `policy`, `group`, `line`, `divider`, `under text`, `number`, `job`, `left`, `footer`, `active`. German only appears where it *is* the copy.

Mixed in with these are hundreds of Figma auto-names that were never cleaned: `Frame 8` ×128, `Frame 7` ×122, `Rectangle 2` ×724, `Group 1577709352`, `Rectangle 34624698`, `Vector 3` ×190, `Vector 4` ×191. The Projekte and Publikationen pages are built almost entirely out of `Frame NN` names.

### Design tokens
**Could not be read.** `get_variable_defs` was refused by the Figma MCP rate limiter (see §6), and there is no style-guide frame in the file. What *is* observable:
- **No local component set, no colour styles page, no text styles page** exist as frames.
- The only `<instance>` nodes in the whole file (52 of them) are third-party icon components: `vuesax/linear/call` ×18, `vuesax/linear/sms` ×15, `vuesax/outline/arrow-left` ×12, `vuesax/outline/arrow-down` ×7.
- The Webflow side has 9 colour variables (§2) and one font (Manrope); those are the de-facto token set.

### Components in Figma
**None authored locally.** Buttons, nav, footer and cards are copy-pasted frame groups, not components — evidenced by the fact that `nav` appears 142 times, `logo` 136 times, `footer` 65 times as plain `<frame>` nodes rather than `<instance>` nodes. The only components are the Vuesax icons above. Additional icon artwork is pasted as detached frames: `Arrow / Arrow_Left_MD` ×558, `Edit / Add_Plus` ×159, `comp_United-Kingdom`, and Flaticon-numbered service icons `012-plan 1`, `016-calculator 1`, `020-engineering-1 1`, `033-notes 1`, `037-building 1`, `037-building 2`, `049-blueprint-1 1`, plus `fi_875110`, `fi_13887866`, `fi_2063733`, `fi_15885481`.

### Auto-layout and measurements
Auto-layout usage cannot be confirmed from metadata (it only reports x/y/w/h), but the geometry is consistent with hand-positioned frames snapped to a grid:
- Page frame `1920`, content frames `1880` at `x=20` → **20 px side gutter, 1880 px content width**.
- Cards: services `400 × 440`, gap `10` (`0`, `410`, `820`) → 3-up row of 1220.
- News cards: `613 × 474/501`, pitch `633.5` → gap `20.5` on an 1880 track.
- Publikationen grid: `613.333 × 613`, pitch `633.33` → 3 columns, gap `20`.
- Projekte rows: `1283 × 408`, stacked at `430` pitch → gap `22`; left filter rail `549` wide at `x=20`; content starts `x=617`.
- Buttons: height `54`, inner `arrow` frame `46` high; icon square `46 × 46`.
- Hero: `980` high on the homepage, `871.076` high on every inner page (a value that recurs exactly, so it was copied not redrawn).

### Assets
Photography as raster fills (`hero-img`, `photo`, `image 2`…`image 6`, `Rectangle N`), the GMG mark as vector (`logo-sign` → `Vector 3` + `Vector 4`), a large decorative `logo-pattern` (same two vectors scaled to `1429 × 2424`) repeated as a watermark in `about`, `facts`, `Career`, `footer` and most inner pages, and SVG-ish icon frames (arrows, plus, Flaticon service icons).

---

## 2. Webflow site anatomy

### Pages
| Page | Slug | SEO | Notes |
|---|---|---|---|
| Home | `/` | `seo: {}`, `openGraph` all null/false | the only page in this copy |

No CMS collection pages, no 404, no password page, **no style-guide page**, no utility pages. `get_collection_list` → `{"collections":[]}`. `data_fonts_tool.list_fonts` → 0 custom fonts (so **Manrope comes from Google Fonts**). `get_registered_scripts` → 0.

### Class naming convention
**A custom in-house system — not Client-First, not Relume.** Evidence for "custom":
- It borrows two Client-First names (`page-wrapper`, `main-wrapper`) but then diverges completely: the container is `main-container` (CF: `container-large`), vertical rhythm is `padding-verticle` (CF: `padding-section-large`), and there is no `padding-global`, `spacing-*`, `text-size-*`, `heading-style-*`, `u-*` or `is-*` anywhere.
- Typography classes are `H1`/`H2`/`H3`/`H4` (display names) rendering as `.h1`…`.h4`, plus `paragraph-12px` / `paragraph-14px` / `paragraph-18px` — a size-in-the-name convention neither framework uses.
- There is no Finsweet/Relume library class (`isFromLibrary` is `false` on all 170 styles).

**The system's actual rule: one generic base class + a section-name combo class.** Every structural class is generic and gets its per-section values from a combo whose name is the section: `.section` → `.section.home-hero` / `.section.about-us` / `.section.facts` / `.section.career` / `.section.footer` / `.section.overflow-hide`; `.padding-verticle` → `.padding-verticle.home-hero` / `.services` / `.mission` / `.more-about-us` / `.facts` / `.footer`; `.content-wrapper` → `.content-wrapper.home-hero` / `.services` / `.mission` / `.more-about-us` / `.facts` / `.news` / `.career` / `.footer`; and likewise `.content-left-wrapper.*`, `.content-right-wrapper.*`, `.flex-wrapper.*`, `.grid.*`, `.section-title-wrapper.*`.

30+ verbatim examples, grouped:

- **Layout / structure:** `page-wrapper`, `main-wrapper`, `section`, `main-container`, `main-container.max-width-1920`, `padding-verticle`, `content-wrapper`, `content-left-wrapper`, `content-right-wrapper`, `flex-wrapper`, `grid`, `inner-wrapper`, `upper-wrapper`, `heading-wrapper`, `section-title-wrapper`, `relative`
- **Typography:** `H1` (`.h1`), `H2` (`.h2`), `H3` (`.h3`), `H4` (`.h4`), `paragraph-12px`, `paragraph-14px`, `paragraph-18px`, `section-subtitle`, `copywrite-text`, `white-color-span`
- **Buttons / links:** `button`, `button.secondary`, `button.secondary.home-hero`, `button.news`, `button-chevron`, `button-chevron.static`, `button-chevron.absolute`, `icon-wrapper`, `arrow-icon`, `nav-link`, `footer-link`, `footer-link.imprint`, `project-link`, `job-link`
- **Navbar:** `navigation`, `nav-container`, `nav-brand`, `brand-logo`, `nav-menu`, `menu-inner-wrapper`, `nav-links-wrapper`, `menu-button`, `hamburger`, `lottie`
- **Components / cards:** `services-card`, `inner-services-wrapper`, `services-img`, `news-card`, `news-card-wrapper`, `news-inner-wrapper`, `news-image`, `job-card`, `jobs-card-wrapper`, `inner-job-wrapper`, `fact-wrapper`, `projects-wrapper`, `project-info-wrapper`, `thumbnail-img-wrapper`, `thumbnail-image`, `footer-brand`, `footer-brand-logo`, `footer-menu`, `footer-links-wrapper`, `inner-menu`, `privacy-wrapper`, `imprint-wrapper`, `divider`, `point`, `see-wrapper`, `time-wrapper`, `trigger-wrapper`, `trigger-icon`, `rectangle-icon`
- **Slider:** `home-hero-slider`, `home-hero-slider-mask`, `home-hero-slide`, `home-hero-slider-wrapper`, `progress-bar`, `progress-bar-wrapper`, `inner-bar`, `inner-bar._2nd-slide`, `inner-bar._3rd-slide`, `left-arrow`, `right-arrow`
- **Decoration / background:** `hero-image`, `hero-image.first-slide` / `.second-slide` / `.third-slide`, `hero-image-bg-layer`, `mission-image`, `career-bg`, `loggo-pattern`, `transparent-pattern`, `gradeint-layer`
- **Utility / modifier:** `hide`, `section.overflow-hide`, `max-width-500px`, `fact-wrapper.max-width-399px`, `main-container.max-width-1920`, `button-chevron.absolute`, `button-chevron.static`, `news-card-wrapper.first` / `.second` / `.third`
- **State-ish:** `services-card.desktop-active`, `services-card.tablet-active`, `h2.split-lines` (hook for the SplitType/GSAP script), `h1.year` / `h1.employes` / `h1.projects` / `h1.standard` (hooks for PureCounter)

Spelling errors baked into the class names: **`padding-verticle`** (vertical), **`loggo-pattern`** (logo), **`gradeint-layer`** (gradient), **`employes`** (employees), **`copywrite-text`** (copyright).

### Homepage DOM skeleton — body → hero
```
Body
  Block.page-wrapper
    ComponentInstance name="Global Styles"          <- 1 component on the whole site
    Block.main-wrapper
      Section<section>.section.home-hero
        NavbarWrapper.navigation
          Block.nav-container
            NavbarBrand.nav-brand
              Image.brand-logo
            NavbarMenu.nav-menu
              Block.menu-inner-wrapper
                Block.nav-links-wrapper
                  NavbarLink.nav-link  × 7
                Link.button
                  Block  (label)
                  Block.icon-wrapper
                    Image.button-chevron.static
                    Image.button-chevron.absolute
            NavbarButton.menu-button
              Block.hamburger
                Animation.lottie
        Block.main-container
          Block.padding-verticle.home-hero
            Block.content-wrapper.home-hero
              Block.content-left-wrapper.home-hero
                Heading<h1>.H1
                Link.button.secondary.home-hero
                  Block
                  Block.icon-wrapper
                    Image.button-chevron.static
                    Image.button-chevron.absolute
              Block.content-right-wrapper.home-hero
                SliderWrapper.home-hero-slider
                  SliderMask.home-hero-slider-mask
                    SliderSlide.home-hero-slide          × 3
                      Block.home-hero-slider-wrapper
                        Block.upper-wrapper
                          Block.projects-wrapper
                            Block.thumbnail-img-wrapper
                              Image.thumbnail-image
                              Block.gradeint-layer
                            Block.project-info-wrapper
                              Heading<h4>.H4
                              Link.project-link > Block
                        Block.progress-bar-wrapper
                          Block (label) ; Block.progress-bar > Block.inner-bar[._2nd-slide|._3rd-slide] ; Block (label)
                        Block.see-wrapper
                          Block.paragraph-12px ; Block.paragraph-18px
                  SliderArrow.left-arrow  > Image.arrow-icon
                  SliderArrow.right-arrow > Image.arrow-icon
                  SliderNav.hide
        Image.hero-image.first-slide
        Image.hero-image.second-slide
        Image.hero-image.third-slide
        Image.hero-image-bg-layer
```
Note the hero background is **three sibling `Image` elements** parked at the end of the section (one per slide) plus a tint layer — not a Webflow background-image, and not inside the slider.

### Representative card grid — the news section
```
Section<section>.section
  Block.main-container
    Block.padding-verticle.facts                 <- reuses the *facts* padding combo, not a "news" one
      Block.content-wrapper.news
        Block.flex-wrapper.news
          Block.section-title-wrapper.news
            Block.section-subtitle > Block (eyebrow text)
            Heading<h2>.H2
          Link.button.news
            Block
            Block.icon-wrapper
              Image.button-chevron.static
              Image.button-chevron.absolute
        Block.grid.news
          Block.news-card-wrapper                 (then .second, .third on cards 2 and 3)
            Block.time-wrapper
              Image.rectangle-icon
              Block (date text)
            Block.news-card
              Image.news-image
              Block.news-inner-wrapper
                Heading<h3>.H3                    (card 2 wraps this in Block.max-width-500px)
                Paragraph.paragraph-18px.news
              Block.trigger-wrapper.news
                Image.trigger-icon
```
The three cards are **hand-built static blocks**, not a Collection List — consistent with this copy having no CMS.

### Footer
```
Section<section>.section.footer
  Block.main-container
    Block.padding-verticle.footer
      Block.content-wrapper.footer
        Block.flex-wrapper.footer
          Block.content-left-wrapper.footer
            Block.section-title-wrapper.footer
              Heading<h3>.H3
              Link.button > Block + Block.icon-wrapper(2 × Image.button-chevron)
            Link.footer-brand  link={"linkType":"page","pageId":"68d437e4c634ef38eb6b907b"}
              Image.footer-brand-logo
          Block.content-right-wrapper.footer
            Block.footer-menu
              Block.inner-menu > Block (column heading)
              Block.footer-links-wrapper > Link.footer-link × 2
            Block.footer-menu
              Block.inner-menu > Block
              Block.footer-links-wrapper > Link.footer-link × 8
        Block.privacy-wrapper
          Block.copywrite-text
          Block.imprint-wrapper
            Link.footer-link.imprint × 2
Image.transparent-pattern          <- sibling of the footer section, inside Block.relative
```

### Navbar and mobile menu
Native Webflow Navbar (`NavbarWrapper` / `NavbarBrand` / `NavbarMenu` / `NavbarLink` / `NavbarButton`), styled `.navigation` `{margin-top:1.25em; background-color:var(--transparent)}` → at `medium` `{margin-top:0em}`. The mobile menu is the **stock Webflow overlay**, widened at tablet: `.nav-menu` has no base styles and gains `{width:100%; background-color:var(--transparent)}` at `medium` only. The burger is `.menu-button` (`medium`: `{z-index:9; padding:0}`) wrapping `Block.hamburger > Animation.lottie` — i.e. **the hamburger is a Lottie animation**, with two Lottie JSON assets in the site (`rvkGluqkSb.json`, alt text `"hamburger"`, and `XVq8GpmrJm.json`).

### Buttons
```css
.button {
  display: flex;
  padding: 0.25em 0.2em 0.25em 1.25em;
  justify-content: flex-start; align-items: center;
  grid-column-gap: 1.25em; grid-row-gap: 1.25em;
  border-radius: 0.31em;             /* all four corners set individually */
  background-color: var(--accent-blue);
  color: var(--gmg-white);
  font-size: 0.88em; line-height: 1.2; font-weight: 700;
  text-decoration: none; text-transform: uppercase;
}
@media medium { .button { padding-right: 0.2em } }   /* re-declares the same value */
.button.secondary { background-color: var(--gmg-white); color: var(--accent-blue) }
.icon-wrapper { position: relative; display: flex; width: 2.88em; justify-content: center; align-items: center }
.button-chevron { width: 100% }
.button-chevron.static { /* no properties */ }
.button-chevron.absolute { position: absolute; inset: 0 0 0 0; opacity: 0 }
```
**`.button` has no `:hover` rule at all.** The hover is the two-chevron swap: a visible `.static` arrow and an `.absolute` arrow at `opacity:0` stacked in `.icon-wrapper`, crossfaded/slid by an interaction. Variants in use: `.button` (blue), `.button.secondary` (white), `.button.secondary.home-hero`, `.button.news`.

### Variables
One collection, **"Colors collection"**, one mode ("Base mode"), 9 variables. No size, spacing, radius or font-family variables.

| Name | CSS name | Value |
|---|---|---|
| GMG White | `--gmg-white` | `#fefefe` |
| White | `--white` | `white` |
| Black | `--black` | `black` |
| Transparent | `--transparent` | `transparent` |
| Accent Blue | `--accent-blue` | `#00425c` |
| Black Text | `--black-text` | `#1a1a1a` |
| Pale Sky | `--pale-sky` | `#6d7d8c` |
| Cyan Blue | `--cyan-blue` | `#e8f2fb` |
| Light Blue | `--light-blue` | `#9bafc3` |

Mapping to Figma: **no Figma variables were readable**, so this mapping is unverified. `#fefefe` does appear verbatim in the homepage head `<style>` (`linear-gradient(90deg,#FEFEFE,#FEFEFE var(--line-width),…)`), so `--gmg-white` is the page background token. Several colours are still hard-coded rather than tokenised: `#e9edf1` (`.news-card` bg), `#839da7` (`.section-subtitle` text), `#979696` (`.paragraph-12px`), `silver` (`.paragraph-14px`), `#333` (body).

### Typography per breakpoint
Webflow breakpoints: `main` (desktop ≥992), `medium` (≤991 tablet), `small` (≤767 mobile landscape), `tiny` (≤479 mobile portrait). Values below are verbatim.

```css
body { font-family: Manrope; color:#333; font-size:14px; line-height:1.2; font-weight:400;
       background-color: var(--gmg-white) }

.h1  { color: var(--gmg-white);  font-size:5.5em;  line-height:1.2; font-weight:500; letter-spacing:-3.5px }
  medium { font-size:3.25em; letter-spacing:-1.75px }
  small  { font-size:2.4em;  letter-spacing:-1px   }
  tiny   { font-size:1.8em;  letter-spacing:-0.3px }

.h2  { color: var(--black-text); font-size:4.25em; line-height:1.2; font-weight:500; letter-spacing:-2.4px }
  medium { font-size:3em;   letter-spacing:-1px   }
  small  { font-size:2.3em; letter-spacing:-0.5px }
  tiny   { font-size:1.5em; letter-spacing:0px    }

.h3  { color: var(--black-text); font-size:2.5em;  line-height:1.4; font-weight:500; letter-spacing:-0.8px }
  medium { font-size:1.75em }
  small  { font-size:1.25em; letter-spacing:-0.3px }
  /* no tiny override */

.h4  { color: var(--gmg-white);  font-size:1.5em;  line-height:1.6; font-weight:500 }
  small { font-size:1.25em }
  tiny  { font-size:1.13em }

.paragraph-18px { color: var(--gmg-white); font-size:1.13em; line-height:1.4; font-weight:500 }
  tiny { font-size:1em }
.paragraph-14px { color: silver;  font-size:0.88em; line-height:1.4; font-weight:400 }
.paragraph-12px { color:#979696;  font-size:0.75em; line-height:1.4; font-weight:500 }
.section-subtitle { padding:0.75em; border-radius:0.25em; background-color:var(--cyan-blue);
                    color:#839da7; font-size:0.88em; line-height:1.6; font-weight:700; text-transform:uppercase }

/* Webflow tag defaults left untouched underneath: */
h1 {font-size:38px; line-height:44px; font-weight:700}
h2 {font-size:32px; line-height:36px; font-weight:700}
h3 {font-size:24px; line-height:30px; font-weight:700}
h4 {font-size:18px; line-height:24px; font-weight:700}
p  { /* default */ }
```
**Everything is sized in `em` against a `14px` body.** So `.h1` computes to `5.5 × 14 = 77px`, `.h2` to `59.5px`, `.h3` to `35px`, `.h4` to `21px`, `.paragraph-18px` to **`15.8px` despite being named 18px** (and `.paragraph-14px` → 12.3px, `.paragraph-12px` → 10.5px). The class names encode the *design* px value; the built values are ~12 % smaller because the root is 14px, not 16px. Flag this to the reviewer — see §4.

Semantics vs. class: heading level and heading class are routinely mismatched. `Heading<h3>.H1.year`, `Heading<h3>.H1.employes`, `Heading<h3>.H1.projects`, `Heading<h3>.H1` (facts + career) — i.e. `h3` tags styled as H1. `Block.H4.services` puts a heading class on a plain div.

### Spacing system
```css
.main-container { width:100%; max-width:117.5em; margin-right:auto; margin-left:auto }
.section        { /* no properties at all */ }
.padding-verticle { /* no properties at all — every value lives on the combo */ }

.padding-verticle.home-hero    { padding-top:7.69em; padding-bottom:6.25em }
   medium {2em / 4.5em}   small {2.5em / 3.5em}   tiny {1.5em / 3em}
.padding-verticle.services     { padding-top:6.25em; padding-bottom:0.63em }
   medium {4.5em / 4.5em} small {3.5em / 3.5em}   tiny {3em / 3em}
.padding-verticle.facts        { padding-top:6.25em; padding-bottom:6.25em }
   medium {4.5em / 4.5em} small {3.5em / 3.5em}   tiny {3em / 3em}
.padding-verticle.footer       { padding-top:6.25em; padding-bottom:1.63em }
   medium {4.5em / 1em}   small {padding-top:3.5em} tiny {padding-top:2.5em}
```
The rhythm scale is **6.25em / 4.5em / 3.5em / 3em** (= 87.5 / 63 / 49 / 42 px at a 14px root) for desktop / tablet / mobile-landscape / mobile-portrait — applied identically to every section except the hero and footer, which have bespoke top/bottom values.

`max-width:117.5em` is the interesting one: `117.5 × 16 = 1880`, exactly the Figma content width — but because the root is 14px it actually computes to **1645 px**. Almost certainly a px→em conversion done against 16 while the body was set to 14. Marked as inference; it could not be confirmed against rendered CSS because the live site is unreachable.

**There is no horizontal padding class.** No `padding-global`, no side padding on `.main-container` or `.section`. The 20 px Figma gutter is not reproduced by a utility; at viewports below 1645 px the content runs edge to edge unless individual elements carry their own padding.

### Grids and responsive behaviour
```css
.grid { display:grid; grid-auto-columns:1fr; grid-column-gap:1em; grid-row-gap:1em;
        grid-template-columns:1fr 1fr; grid-template-rows:auto auto }
.grid.services { gap:0.63em; grid-template-columns:1fr 1fr 1fr; grid-template-rows:auto }
   medium { grid-template-columns:1fr 1fr }   small { grid-template-columns:1fr }
.grid.news     { justify-items:stretch; align-items:start; gap:1.25em;
                 grid-template-columns:1fr 1fr 1fr; grid-template-rows:auto }
   medium { grid-row-gap:2em; grid-template-columns:1fr 1fr }
   small  { grid-template-columns:1fr }        tiny { grid-row-gap:1.5em }

.services-card { position:relative; display:flex; width:100%; height:27.5em;
                 padding:7.94em 2.5em 5.94em; justify-content:center; align-items:flex-start;
                 background-color:var(--cyan-blue) }
   medium { height:23em; padding-top:2.5em; padding-bottom:2.5em; justify-content:center; align-items:center }
   small  { height:auto; padding-top:3em;  padding-bottom:3em }
   tiny   { padding-right:1.5em; padding-left:1.5em }
```
All four breakpoints are customised. What typically changes: grid columns 3 → 2 → 1, font sizes stepped down at every breakpoint, section padding stepped down on the 6.25/4.5/3.5/3 scale, fixed card heights relaxed to `auto` at `small`, and **card visibility juggled with combo classes** — `.services-card.tablet-active` and `.services-card.desktop-active` exist so the 4+3 card split reflows to 4+3 / 3+3 depending on breakpoint.

### Components
**One** Webflow component on the entire site: `Global Styles` (id `4c22120e-09e5-a906-3974-9f89afb3dc21`, instanceCount 1, **0 props**), placed as the first child of `.page-wrapper`. Nav, footer, buttons and cards are **not** components — they are repeated raw element trees. (In this single-page copy the nav and footer exist once; whether the full site used a symbol cannot be checked.)

### CMS
`get_collection_list` → `{"collections": []}`. **There is no CMS in the site this token can reach.** The real build (per the brief) had publikationen, aktuelles, projekte and karriere collections; none are present here, and no Collection List / Collection Item elements appear in the homepage tree (the news row is three hand-built `.news-card-wrapper` blocks with `.first`/`.second`/`.third` combos).

### Interactions / animations
- `data_interactions_tool.list_interactions` → `{"items":[], "total":0}`. **No IX3 interactions.** Caveat: this endpoint does not report legacy **IX2**, and the site clearly relies on IX2 — the `.button-chevron.absolute { opacity:0 }` swap, the Lottie hamburger, and the `.inner-bar._2nd-slide` / `._3rd-slide` progress bar all need a trigger that exists nowhere in CSS or custom code.
- Native Webflow **Slider** for the hero (`SliderWrapper.home-hero-slider` + `SliderNav.hide` — dots hidden, custom `.left-arrow`/`.right-arrow` and a custom `.progress-bar`).
- **Lottie** via `Animation.lottie` for the burger; 2 JSON assets.

### Custom code (verbatim)
**Site → footer:**
```html
<link rel="stylesheet" href="https://unpkg.com/lenis@1.2.3/dist/lenis.css">
<script src="https://unpkg.com/lenis@1.2.3/dist/lenis.min.js"></script>
<script>
let lenis = new Lenis({ lerp:0.1, wheelMultiplier:0.7, gestureOrientation:"vertical",
                        normalizeWheel:false, smoothTouch:false });
function raf(time){ lenis.raf(time); requestAnimationFrame(raf); }
requestAnimationFrame(raf);
$("[data-lenis-start]").on("click", function(){ lenis.start(); });
$("[data-lenis-stop]").on("click",  function(){ lenis.stop();  });
$("[data-lenis-toggle]").on("click", function(){
  $(this).toggleClass("stop-scroll");
  if ($(this).hasClass("stop-scroll")) { lenis.stop(); } else { lenis.start(); }
});
</script>
```
Site → head: empty. No registered (Apps-style) scripts.

**Home page → head:**
```html
<style>
  .line strong { color: black; }
  .line { color: transparent; --line-width: 100%; position: relative;
    background-color: rgba(142, 146, 162, 0.5);
    background-image: linear-gradient(90deg,#FEFEFE,#FEFEFE var(--line-width),
                                      hsla(0,0%,100%,0) var(--line-width));
    background-clip: text; }
</style>
```
**Home page → footer:** GSAP 3.8.0 + ScrollTrigger + `split-type.js` (from `cdn.jsdelivr.net/gh/timothydesign/script/split-type.js`), splitting `.split-lines` into `lines, words`, then a per-`.line` ScrollTrigger timeline (`start:"top center"`, `end:"bottom center"`, `scrub:1`) tweening `--line-width` from `0%` — i.e. a scroll-scrubbed text reveal. Re-splits on width change. Then PureCounter (`cdn.jsdelivr.net/npm/@srexi/purecounterjs/dist/purecounter_vanilla.js`) with four instances:
| selector | start | end | duration |
|---|---|---|---|
| `.year` | 0 | **1995** | 1 |
| `.employes` | 0 | **40** | 1 |
| `.projects` | 0 | **100** | 1 |
| `.standard` | 0 | **100** | 1 |
all with `delay:10, once:true, repeat:false, decimals:0, legacy:true`.

### Images
37 assets. Formats: **25 webp, 6 png, 4 svg, 2 json (Lottie)**. Names are raw Figma export names with Figma's duplicate suffixes: `Rectangle 5 (1).webp`, `Rectangle 3 (2).webp`, `Group 1 (1).webp`, `Vector 3 (1).png`, `logo-pattern (3).png`, `photo (1).webp`, `arrow (2).webp`, `012-plan 1.webp`, `11be7dfa36acf02550c0c6092e31b98b59304920 (2).webp` (531 KB). Icons are inline-ish `Image` elements pointing at SVG assets (`Arrow_Left_MD.svg` uploaded **twice**, `Add_Plus.svg`, `Rectangle 11.svg`) — **no inline SVG embeds**. **Alt text: 1 of 37 assets has any (`rvkGluqkSb.json` → `"hamburger"`); the other 36 are empty.**

### Forms, links, SEO
No form elements on the homepage. Internal links use `{"linkType":"page","pageId":"68d437e4c634ef38eb6b907b"}` (footer brand + first footer link both point at Home). Page SEO is completely empty (`seo: {}`); Open Graph is unset with `titleCopied:false` / `descriptionCopied:false`, so not even the "copy from page title" default is on.

---

## 3. Figma → Webflow mapping observed

| Design concept (Figma) | Realised in Webflow as | Changed in build? |
|---|---|---|
| Page frame `GMG - Homepage Design` (1920) | page `Home` (`/`) | — |
| Section frame `hero` | `Section<section>.section.home-hero` + `.main-container` + `.padding-verticle.home-hero` + `.content-wrapper.home-hero` | 4-deep wrapper chain added; Figma has 1 frame |
| Section frame `services` | `Section.section` > `.padding-verticle.services` > `.content-wrapper.services` > `.flex-wrapper.services` > 2 × `.grid.services` | 3+3 cards in Figma became **4 + 3** in the build (7 `.services-card`), with `.tablet-active` / `.desktop-active` to hide one per breakpoint |
| Section frame `about` | `Section.section.about-us` > `.padding-verticle.more-about-us` > `.content-wrapper.more-about-us` > `.content-right-wrapper.about-us` | combo name diverges from the Figma frame name (`about` → `about-us` / `more-about-us`) |
| Section frame `mission` | `Section.section.overflow-hide` > `.main-container.max-width-1920` > `.padding-verticle.mission` | gets its own wider container variant |
| Section frame `facts` / `nuumbers` | `Section.section.facts` > `.padding-verticle.facts` > `.flex-wrapper.facts` > 4 × `.fact-wrapper` | typo not carried over; counters added (PureCounter) |
| Section frame `news` | `Section.section` > `.padding-verticle.facts` (!) > `.content-wrapper.news` > `.grid.news` | **reuses the `facts` padding combo** instead of getting a `news` one |
| Section frame `Career` | `Block.relative` > `Section.section.career` > `.padding-verticle.facts` (!) + sibling `Image.career-bg` | same padding-combo reuse |
| Section frame `footer` | `Section.section.footer` > `.padding-verticle.footer` + sibling `Image.transparent-pattern` | — |
| Figma `nav` (frame, copied 142×) | native Webflow **Navbar** (`.navigation` / `.nav-menu` / `.nav-link` / `.menu-button`) | not a component/symbol; burger is a **Lottie**, which is not in the design |
| Figma `btn` + `arrow` + `Rectangle 2` + `Arrow / Arrow_Left_MD` | `Link.button` > `Block`(label) + `Block.icon-wrapper` > 2 × `Image.button-chevron` (`.static` visible, `.absolute` at `opacity:0`) | the second, duplicated arrow is a build-only construct for the hover |
| Figma `card` (services, 400×440) | `.services-card` `{height:27.5em; padding:7.94em 2.5em 5.94em; background:var(--cyan-blue)}` | `27.5em × 14px = 385px` vs 440 in design; padding values are px→em conversions (`111/14`, `35/14`, `83/14`) |
| Figma `card-news` (613×474) | `.news-card-wrapper` > `.time-wrapper` + `.news-card` | date chip lifted **outside** the card, matching the Figma `date` frame sitting at `y=0` above `card bg` at `y=32` |
| Figma `card-project slide` (hero + Career) | hero: `SliderSlide.home-hero-slide`; Career: `.jobs-card-wrapper` > `.job-card` + `.divider` | Figma shows 4 `job` rows in Career, build has 2 `.job-card` + 1 `.divider` |
| Figma `logo-pattern` (vector watermark) | `Image.loggo-pattern` / `Image.transparent-pattern` / `Image.career-bg` as **section siblings** | exported as PNG (`logo-pattern.png`, `(1)`, `(3)`), not kept vector |
| Figma colour fills | 9 variables in "Colors collection" | several values stayed hard-coded (`#e9edf1`, `#839da7`, `#979696`, `silver`, `#333`) |
| Figma text styles | `.h1`–`.h4` + `.paragraph-12px/14px/18px` + `.section-subtitle`, all `em` on a **14px** body | sizes land ~12 % below the px number in the class name |
| Figma content width 1880 (1920 − 2×20) | `.main-container { max-width:117.5em }` | `117.5 × 16 = 1880` intended, `× 14 = 1645` actual; **no side-gutter class at all** |
| Figma auto-layout gaps (10 / 20 / 20.5) | `grid-column-gap` `0.63em` (services) / `1.25em` (news) | 0.63em × 16 ≈ 10, 1.25em × 16 = 20 — again converted against 16 |
| Figma breakpoint frames | **none existed**; `medium` / `small` / `tiny` invented in Webflow | entire responsive design is build-side |
| Figma icons (Vuesax instances, Flaticon `fi_*`, `012-plan 1`) | uploaded SVG/webp assets referenced by `Image` elements | `Arrow_Left_MD.svg` uploaded twice |
| Figma hero (980 high, 3-slide `GMG - Hero Slider` frames) | native Slider + 3 sibling `Image.hero-image.first/second/third-slide` + `.hero-image-bg-layer` | backgrounds live outside the slider, presumably cross-faded by IX2 |
| Figma `Filter zurücksetzen`, `Kacheln`/`Karte`, year range slider (Publikationen/Projekte) | **not present in this Webflow copy** | see §4 |

Inferable reasons for changes: none — no Slack/Notion/design notes were available in this environment. The 16-vs-14 px conversion pattern is consistent enough across `max-width`, gaps and paddings that it reads as a single habit (design values divided by 16 to produce `em`) rather than per-value decisions.

---

## 4. Quality notes and gotchas

### Rules that look deliberate
1. **Every section is `Section<section>.section[.name]` → `Block.main-container` → `Block.padding-verticle.<name>` → `Block.content-wrapper.<name>`.** Four levels, always, even when three of them carry no properties. This is the single strongest convention in the build.
2. **Base class carries structure, section-named combo carries values.** `.section`, `.padding-verticle` and `.grid`'s row/column template are empty or generic; `.padding-verticle.facts`, `.grid.services` etc. hold the actual numbers. The combo is always named after the section, never after the effect.
3. **Everything is `em`.** Not one `rem`, not one raw `px` (outside Webflow's own tag defaults and `letter-spacing`). Even radii: `0.31em`, `0.63em`, `0.25em`.
4. **Headings use `.h1`–`.h4` classes, and the HTML tag is chosen independently** for document outline (facts use `<h3>` tags with `.h1` class).
5. **Buttons are always** `Link.button` > `Block`(label) + `Block.icon-wrapper` > `Image.button-chevron.static` + `Image.button-chevron.absolute`. Five occurrences on the homepage, identical every time.
6. **Decorative background images are siblings of the section, never children** (`Image.loggo-pattern`, `Image.career-bg`, `Image.transparent-pattern`, `Image.hero-image.*`).
7. **A `Global Styles` component is dropped as the first child of `.page-wrapper`.**
8. **Animation is bought in, not built in Webflow:** Lenis (site footer) + GSAP/ScrollTrigger/SplitType + PureCounter (page footer). Webflow IX is reserved for micro-interactions (button arrow, burger, slider bar).
9. **Class hooks for JS are combo classes on a heading:** `.h2.split-lines`, `.h1.year`, `.h1.employes`, `.h1.projects`, `.h1.standard`.
10. **Assets are uploaded straight from Figma with Figma's names**, converted to webp for photos and left as SVG for icons.

### Mistakes and inconsistencies
- **Five typos frozen into class names:** `padding-verticle`, `loggo-pattern`, `gradeint-layer`, `employes`, `copywrite-text`. (Figma has its own: `nuumbers`.) These are load-bearing — the PureCounter script targets `.employes`.
- **`.paragraph-18px` renders ≈15.8px**, `.paragraph-14px` ≈12.3px, `.paragraph-12px` ≈10.5px, because the scale is `em` on a `14px` body. The class names lie.
- **`.main-container { max-width:117.5em }` ⇒ ~1645px, not the 1880px the design specifies** (same 16-vs-14 root mismatch). Inference — unverifiable without the live CSS.
- **No horizontal gutter anywhere.** Neither `.main-container` nor `.section` nor `.padding-verticle` sets left/right padding, so below ~1645px the layout has no side margin unless a child provides one. The Figma design has a consistent 20px gutter.
- **`news` and `career` sections reuse `.padding-verticle.facts`.** Changing the facts spacing silently moves three sections.
- **`.button { padding-right:0.2em }` is re-declared identically at `medium`** — a no-op override, the classic sign of someone editing at the wrong breakpoint.
- **`.news-card` ships a commented-out box-shadow in its base rule:** `box-shadow: none /* 1px 1px 9px 1px rgba(0,0,0,0.2) */` with the real shadow on `:hover`. Works, but leaves dead CSS.
- **SEO is entirely unset** on the one page that exists — empty `seo`, no OG title/description/image, `titleCopied:false`.
- **36 of 37 assets have no alt text.** Decorative patterns and content photos alike.
- **`Arrow_Left_MD.svg` is uploaded twice** (identical 346-byte files).
- **`11be7dfa36acf02550c0c6092e31b98b59304920 (2).webp` is 531 KB**, ~5× the next largest image.
- **`Block.H4.services`** applies a heading class to a non-heading element inside the services card.
- Figma hygiene: hundreds of `Frame NN` / `Rectangle NNNNNNNN` / `Group 1577709XXX` auto-names, trailing and double spaces in page frame names, layer order not matching visual order, and the same sections copy-pasted six times across iteration rows with no components.

### Special things present
- **Smooth scroll:** Lenis 1.2.3, site-wide, with `data-lenis-start` / `-stop` / `-toggle` hooks (jQuery-dependent — relies on Webflow's bundled jQuery).
- **Scroll-scrubbed text reveal:** SplitType + GSAP ScrollTrigger driving a `--line-width` CSS custom property through `background-clip:text`. Hook classes `.split-lines` (splitter) and `.line` (generated).
- **Animated counters:** PureCounter on `.year`, `.employes`, `.projects`, `.standard`.
- **Lottie hamburger.**
- **Slider:** native Webflow Slider with hidden `SliderNav.hide`, custom arrows and a bespoke `.progress-bar` whose fill width is switched by `.inner-bar._2nd-slide` / `._3rd-slide`.
- Not present: RTL, localisation (single locale), dark mode, sticky elements (no `position:sticky` in any style read), tabs, accordions, modals, video.

### The CMS filters on projekte / publikationen — what can actually be said
**Direct answer: no filter implementation exists in the Webflow site this token can reach, and no Finsweet asset of any kind is present.** Concretely:
- `list_pages` → 1 page (`Home`). There is no `/projekte` or `/publikationen` page to inspect.
- `get_collection_list` → `[]`. No collections, so no Collection Lists to filter.
- `get_site_freeform_code` (footer) contains **only Lenis**. `get_page_freeform_code` on Home contains only GSAP/SplitType/PureCounter. **No `@finsweet/attributes`, no `cmsfilter`, no `fs-cmsfilter-*`, no `cmsload`, no `cmssort` script tag anywhere in site- or page-level custom code.**
- `get_registered_scripts` → 0 registered scripts.
- No `isFromLibrary:true` styles, i.e. no Finsweet/Relume component library installed.
- The homepage carries no `fs-*` attributes (the element tree returned no `attributes` on any node).

**What the design asks for** (Figma, latest row) — the next agent should treat this as the spec that the real site must satisfy:
- **Publikationen** (`135:772 "Frame 5"`): a **year-range filter**. `Frame 3` = label `Zeitraum` + `Group 1` (a two-handle range slider: `Line 1` track, `Line 2` active segment, `Ellipse 1` + `Ellipse 2` handles) + two value boxes `Frame 1` (`From` / `2020`) and `Frame 2` (`T0` / `2023` — the label is a typo for "To"). `Frame 4` = a reset control: `Group 2` icon + text **`Filter zurücksetzen`**. Results are grouped under year headings (`2024`, `2023`) each followed by a `Line`, in a 3-column grid of `613.33 × 613` cards.
- **Projekte** (`135:1520 "Frame 47"` + `135:1709 "links"`): a **left rail of single-select service filters** — a radio `Ellipse 3` + label `Alle`, then `Brückenbau`, `Hochbau`, … each row `492 × 34` with an expand `arrow` (`Arrow / Arrow_Left_MD` in a 24×24 `Rectangle 2`), under the heading `Leistungen`; plus a **view toggle** `Frame 41` = `Frame 39` (`fi_875110` icon + `Kacheln` = tiles) and `Frame 40` (`fi_13887866` icon + `Karte` = map); plus the same **`Filter zurücksetzen`** reset; plus **pagination** `475:444 "Frame 3"` = prev `btn`, `Aktuell` / `02`, `Bis` / `18`, next `btn`.
- The first iteration of Projekte (`1:701`) had **three** filter groups (three `Line` dividers, `Frame 51` / `Frame 54` / `Frame 55`) and **no** pagination; the final has one group plus pagination and the map/tiles toggle. So the filter scope was cut down and paging was added late — exactly the kind of late change that breaks a filter setup.

Given a nested-list radio filter, a numeric range slider, a reset button, a tiles/map view switch **and** pagination on the same lists, the natural Webflow implementation is **Finsweet Attributes `cmsfilter` (+ `cmsload` for the paging, `cmssort` possibly)** — but that is an inference about the real site, **not something observed here**. The next agent must re-check on a site copy that actually contains the collections, and should specifically look for: the `@finsweet/attributes-cmsfilter` script tag, `fs-cmsfilter-element="filters|list|empty|reset|results-count"`, `fs-cmsfilter-field`, `fs-cmsfilter-range="from|to"`, and `fs-cmsload-element="list|page-button"` — and whether Attributes **v1** or **v2** is loaded, since the v1→v2 attribute rename (`fs-cmsfilter-*` → `fs-list-*`) is the most common cause of "filters silently stopped working" after a script URL is updated.

---

## 5. Verbatim dumps

Omitted from the skill copy; the full dump lives in the analysis workspace.

## 6. Tool quirks

- **`*.webflow.io` is blocked by the agent egress proxy.** `curl` returns `000` with `connect_rejected`; `WebFetch` returns `{"error_type":"EGRESS_BLOCKED"}`. `curl -sS "$HTTPS_PROXY/__agentproxy/status"` shows a history of identical `connect_rejected` / "gateway answered 403 to CONNECT (policy denial)" entries for other agents' webflow.io hosts, so this is a standing policy, not a transient. **The brief's primary strategy (fetch live HTML + webflow.css) is unavailable; plan around the Designer API instead.**
- **Figma MCP has a hard per-seat tool-call cap.** After exactly two `get_metadata` calls, the third Figma call (`get_variable_defs`) returned: `"You've reached the Figma MCP tool call limit for your View seat on the Professional plan."` No screenshots, no `get_design_context`, no variable defs were obtainable. **Budget Figma calls: spend them on `get_metadata` for the page you need and mine the cached dump locally.**
- **`get_metadata` with a page id returns ~1.6 M characters** and is auto-spilled to `/root/.claude/projects/<…>/tool-results/mcp-Figma-get_metadata-<ts>.txt` as `[{type,text}]`. Joining the `text` fields gives an indented XML tree; slicing subtrees by indentation (see `analysis/gmg/sub.py`) is cheap and is the only Figma evidence you get once the cap hits.
- **Webflow MCP Designer tools are aggressively rate-limited and fail two different ways.** `data_element_tool.get_all_elements` failed with `Tool get_all_elements failed: GET /v2/assets returned 429` (it internally hits the assets endpoint); `data_style_tool` failed with a bare `Too Many Requests`. Both recovered after ~1–2 minutes of doing other work. Data-API tools (`data_pages_tool`, `data_cms_tool`, `data_sites_tool`, `data_scripts_tool`, `data_assets_tool`, `data_fonts_tool`, `data_interactions_tool`) never rate-limited. **Interleave Designer calls with Data-API calls and retry rather than backing off wholesale.**
- **`query_styles` with a single-segment `name_path` does not match combo classes.** `name_path:["padding-verticle"]` returns only the (empty) base class. To read the values you must pass the full chain: `name_path:["padding-verticle","facts"]`. This silently looks like "the class has no styles".
- **`get_styles {query:"all"}` ignores `include_properties` in practice** — it returned names/selectors only. Use `query_styles` with `include_properties:true` (and `include_breakpoints`, `include_base_pseudos`) for values.
- **`get_all_elements` returns no text content and no attributes.** Text nodes come back as bare `String` with nothing in them, and `attributes` is absent everywhere, so you cannot read copy or `fs-*`/`data-*` attributes from the tree. Use `data_element_tool.get_attributes` per element (expensive) or the live HTML (blocked here).
- **`get_site_scripts` returns 404 `"Custom code block not found"`** when a site has no *registered* scripts applied — this is normal, not an error. The actual custom code is in `get_site_freeform_code` / `get_page_freeform_code`.
- **`data_interactions_tool.list_interactions` only covers IX3.** An empty result does **not** mean the site has no animations; this site plainly uses IX2 (button-arrow swap, Lottie burger, slider progress bar) and none of it is visible through this tool.
- **`list_assets` and `get_all_elements` both blow the response-size limit** (51 KB and 83 KB) and spill to files; parse them with python rather than re-requesting.
- Webflow MCP session: first call must send `session_id:"start"`, which issued `ses_3JeMDTW9yJ2Ffuerlxh0C5f2FZu`; every later call reused it. `agent_id` used throughout: `fable-5.1|claude-code|gmg42`.
- **Site-copy trap:** the site id in the brief is a *workspace copy* (`displayName: "Copy of GMG"`). Copies can lose the CMS and all non-home pages. `list_sites` (195 sites) shows no other GMG site, so there is no richer copy to switch to from this token. Verify `list_pages` + `get_collection_list` **first** on any future project; if they come back thin, say so immediately rather than analysing a shell.
