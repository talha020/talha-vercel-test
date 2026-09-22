# Wall + Baur
Figma: https://www.figma.com/design/igFQ1w3wojCczTW6m3BMkk/Wall-Baur---Design  |  Webflow site id: 69f5d16aa866b75c3684242e  |  Live: https://wall-baur-090f38.webflow.io/  |  Analysed: 2026-09-21

> **Scope warning — read first.** Two of the three evidence sources in the brief were unavailable in this
> environment (details in §6):
> * **The Figma file could not be opened at all.** Every Figma MCP call returned
>   `Looks like you don't have edit access to this file. The file owner can share it with you and make you an editor.`
>   §1 is therefore **not observed** — it is reconstructed *backwards* from the Webflow build, and every
>   statement in it is explicitly marked as an inference.
> * **The live site could not be fetched.** `*.webflow.io` is blocked by the org egress proxy, so no
>   rendered HTML, no `webflow.css`, and no screenshots. Everything in §2–§5 comes from the Webflow
>   Data MCP APIs, which turned out to be a complete substitute.
> * No screenshots were saved to the run folder — image egress is blocked too.

---

## 1. Figma file anatomy

**NOT OBSERVED — access denied.** The file key `igFQ1w3wojCczTW6m3BMkk` is valid and well-formed, but the
MCP account has no access to it. `get_metadata` (with and without `nodeId`), `get_screenshot` and
`get_variable_defs` all failed identically. Nothing below is read from Figma; it is inferred from the
built site, where the build preserves the design's vocabulary unusually literally.

- **Pages in the file** — unknown.
- **Which frames are the "build from" source** — unknown. The Webflow site has exactly **one page**, so
  the design is almost certainly a single-page (one-pager) file.
- **Breakpoints in Figma** — unknown, but the Webflow build only ever overrides `medium` (≤991px),
  `small` (≤767px) and `tiny` (≤478px), and never `large`/`xl`/`xxl`. *(Inference)* the design was
  delivered at a desktop width plus roughly a tablet and a phone frame — the `tiny` overrides are the
  most numerous after `medium`, and `small` is used sparingly, which is the signature of "desktop +
  mobile designed, tablet interpolated by the builder".
- **Section naming inside the page frame** — *(Strong inference.)* The builder names a Webflow combo
  class after each design section, in German, and these names are almost certainly verbatim Figma frame
  names. Top to bottom, the homepage's section combos are:
  `hero` → `uber-uns` → `marquee` → `kompetenzen` → `architektur-in-aktion` (the Referenzen slider) →
  `unser` (Unser Versprechen) → `team` → `cta` → `footer`.
  Note `architektur-in-aktion` is the combo name but the visible heading is "Referenzen" — *(inference)*
  the section was renamed in copy after the frame was named, and the class kept the old name.
- **Design tokens** — unknown in Figma. **However the built CSS lets the token values be read off almost
  exactly**, because of the `px → em` rule described in §2 and §3: every class name carries the design's
  pixel number and every CSS value is that number ÷ 16 expressed in `em`. So the design's type ramp was
  almost certainly **15 / 16 / 18 / 20 / 24 / 40 / 96 px** and its vertical section paddings were
  **40, 48, 73, 80, 88, 120, 201, 223 px**. Colours, likewise, are the literal hexes in §2.
- **Fonts** — **Mulish** (every single text class declares `font-family: Mulish`). No second family.
- **Components in Figma** — unknown. Worth noting that **zero Webflow components** were created (§2), so
  either the design had no components or the builder chose not to mirror them.
- **Auto-layout usage** — *(Inference, but well supported.)* The build is essentially 100% flexbox with
  `grid-column-gap`/`grid-row-gap` set on almost every wrapper and almost no margins. That is the
  fingerprint of a design built in auto layout and translated one-frame-to-one-flex-div. Only four
  elements in the whole page use absolute positioning (`.slider-number`, `.year-box`,
  `.img.play-button`, `.left-arrow-mini`/`.right-arrow-mini`).
- **Content max width** — the build uses `max-width: 120em` on `.main-container`, i.e. **1920px** at the
  16px reference. *(Inference)* the design was drawn on a 1920-wide frame, not 1440.
- **Asset types** — 16 distinct image assets, all raster/`<img>`; **no inline SVG anywhere**. Icons
  (arrows, play button, bullets) are either `<img>` assets or CSS-drawn divs (`.yellow-bullet`,
  `.mini-grey-line`, `.hamburger-bar`).

---

## 2. Webflow site anatomy

### Pages
| Page | id | slug | created | last updated |
|---|---|---|---|---|
| Home | `69f5d16ba866b75c36842434` | `/` | 2026-05-02 | 2026-05-07 |

- **That is the entire site — one page.** No CMS collection pages, no 404, no password page, **no style
  guide page**, no utility pages, no folders.
- `seo: {}` and `openGraph: {imageUrl: null, imageAssetId: null}` — **no SEO title, no meta description,
  no OG image were ever set.**
- CMS: **no collections**, and no CMS bindings anywhere in the element tree. All content is static.

### Class system — what it is
**A fully custom, project-local system. It is NOT Client-First and NOT Relume.**

Evidence for "custom":
- No `padding-global`, no `container-large`/`container-medium`, no `padding-section-large`, no
  `max-width-*`, no `spacing-*`, no `text-weight-*`, no `text-style-*` — none of the Client-First
  backbone is present.
- Typography classes are named by **literal pixel size** (`text-size-15`, `text-size-40`), not by
  Client-First's t-shirt scale (`text-size-medium`, `text-size-large`).
- Section padding classes are named by the **literal Figma pixel pair** (`padding-vertical` +
  `_88-73`, `_201-80`, `_223-223`), which no public system does.

Evidence of **light Client-First influence** (borrowed vocabulary, not the system):
- `page-wrapper`, `main-wrapper` — the exact Client-First page shell names, used in the exact order
  (`body > .page-wrapper > .main-wrapper > section`).
- `heading-style-h1` — a Client-First name, but it is the **only** one of its family (there is no
  `heading-style-h2`…`h6`), and it is applied to `Block<div>` elements, not headings.
- `is-hide` — a Client-First-style `is-*` state modifier, used twice.

**30+ verbatim class names, grouped:**

*Layout / structure (global):*
`page-wrapper`, `main-wrapper`, `section`, `main-container`, `nav-container`, `content-wrapper`,
`left-content-wrapper`, `right-content-wrapper`, `left-card-wrapper`, `text-wrapper`, `text-wrap`,
`text-box`, `inner-text-wrap`, `left-text-wrap`, `lower-text-wrap`, `lower-text-wrapper`,
`lower-cards-wrapper`, `upper-bar`, `upper-title`, `image-wrapper`, `video-wrapper`, `points-wrapper`,
`point-wrap`, `name-box`, `name-box-parent`, `name-detail-wrap`, `padding-vertical`, `bg`

*Typography (global):*
`heading-style-h1`, `text-size-15`, `text-size-16`, `text-size-18`, `text-size-20`, `text-size-24`,
`text-size-40`, `marquee-text`, `current`, `total`, `grey-color`

*Buttons / nav:*
`button`, `nav-bar`, `nav-button-wrapper`, `navigation`, `brand-logo`, `hamburger`, `hamburger-bar`,
`Menu Button`, `mini-nav-link-divider`

*Components / cards:*
`slider-wrapper`, `slides-wrapper`, `slide-content`, `slide-card-content-wrapper`,
`slides-content-wrapper`, `slide-detail`, `slider-number`, `slider-nav`, `slider-navv-bar`,
`left-arrow-mini`, `right-arrow-mini`, `year-box`, `quote-author-box`, `quote-reference`,
`tabs`, `tabs-menu`, `tab-link`, `tab-pane`, `tab-pane-content-wrapper`, `Tabs Content`,
`team-members-list-wrapper`, `team-member-detail`, `footer-links-wrapper`, `footer-divider`,
`hero-divider`, `mini-grey-line`, `yellow-bullet`, `arrows-wrapper`, `arrow-imgs-wrapper`,
`marquee-horizontal`, `track-horizontal-alt`, `img`, `Image`

*Utility / modifier combo classes (the second class in a chain):*
- **section-name modifiers:** `.hero`, `.uber-uns`, `.kompetenzen`, `.team`, `.cta`, `.footer`,
  `.unser`, `.marquee`, `.architektur-in-aktion`, `.referenzen`, `.slider`, `.slider-nav`,
  `.mini-slider`, `.slide-heading`, `.slides-detail`, `.tem-right-description`, `.inner`,
  `.uber-uns-lower-text`
- **colour modifiers:** `.black`, `.white`, `.grey`, `.light-grey`, `.yellow`, `.mustard`
- **size modifiers:** `.size-12`, `.size-24`, `.size-32`, `.profille`, `.play-button`, `.brand-logo`,
  `.arrow`
- **spacing modifiers:** `._40-48`, `._80-80`, `._80-120`, `._88-73`, `._120-120`, `._201-80`,
  `._223-223`
- **text-transform modifiers:** `.capital`, `.small-case`, `.centre`
- **visibility / state:** `.is-hide`, `.mob-hide`, `.desktop-hide`, `.main`, `.main-slider`,
  `.top`, `.middle`, `.bottom`, `.lined`, `.bottom-lined`

*Tag styles:* `body`, `img`

Totals: **178 styles = 90 global classes + 86 combo classes + 2 tag styles.** 31 of them are defined but
carry **no CSS at all** (listed in §4).

### DOM skeleton — body down to the hero
```
Body
  Block<div> .page-wrapper
    HtmlEmbed                                     <- the 1vw <style> block (see Custom code)
    Block<div> .main-wrapper
      Section<section> .section                   <- hero bg image lives here
        NavbarWrapper .nav-bar
          Block<div> .nav-container
            NavbarBrand .brand-logo.nav
              Image .img.brand-logo
            NavbarMenu .navigation
              NavbarLink .text-size-15            <- "Kompetenzen"
              Block<div> .mini-nav-link-divider.is-hide
              NavbarLink .text-size-15            <- "Referenzen"
              Block<div> .mini-nav-link-divider.is-hide
              NavbarLink .text-size-15            <- "Über uns"
              Block<div> .mini-nav-link-divider.is-hide
              NavbarLink .text-size-15            <- "Team"
              Block<div> .mini-nav-link-divider.is-hide
              NavbarLink .text-size-15            <- "Karriere"
              Block<div> .mini-nav-link-divider.is-hide
              NavbarLink .text-size-15            <- "Blog"
              Link .button.nav.desktop-hide
                Block<div> .text-size-15
                Image .img.arrow
            Block<div> .nav-button-wrapper
              Link .button.nav.mob-hide
                Block<div> .text-size-15
                Image .img.arrow
              NavbarButton .Menu Button.bottom
                Block<div> .hamburger
                  Block<div> .hamburger-bar.top
                  Block<div> .hamburger-bar.middle
                  Block<div> .hamburger-bar.bottom
        Block<div> .main-container
          Block<div> .padding-vertical._88-73
            Block<div> .content-wrapper.hero
              Block<div> .heading-style-h1
              Block<div> .hero-divider
              Block<div> .lower-cards-wrapper
                Block<div> .left-card-wrapper.hero          <- glass quote card
                  Block<div> .text-size-20
                    Emphasized
                  Block<div> .quote-author-box
                    Image .img.profille
                    Block<div> .quote-reference
                      Block<div> .yellow-bullet
                      Block<div> .text-size-16
                SliderWrapper .slider-wrapper               <- 4-slide mini slider
                  SliderMask .mask-2
                    SliderSlide .slide  (x4)
                      Block<div> .slider-number
                        Block<div> .current
                        Block<div> .current
                        Block<div> .text-size-15.grey
                        Block<div> .total
                        Block<div> .text-size-15.grey
                      Block<div> .text-wrapper.mini-slider
                        Block<div> .text-size-15.small-case (x4, each wrapping Span .grey-color)
                  SliderArrow .left-arrow-mini
                    Image .img.size-12
                  SliderArrow .right-arrow-mini
                    Image .img.size-12
                  SliderNav .slide-nav-2                    <- display:none
```

### A representative card grid — the `kompetenzen` list (section 4)
```
Section<section>
  Block<div> .main-container
    Block<div> .bg.bottom-lined
      Block<div> .padding-vertical._80-120
        Block<div> .content-wrapper.kompetenzen             <- flex row, gap 2.5em; column @medium
          Block<div> .left-content-wrapper.kompetenzen      <- max-width 27.81em
            Block<div> .upper-title.uber-uns
              Block<div> .left-text-wrap.uber-uns
                Block<div> .mini-nav-link-divider
                Block<div> .text-size-15.grey               <- eyebrow "Kompetenzen"
                Block<div> .mini-nav-link-divider
            Block<div> .image-wrapper.kompetenzen
              Image .img.kompetenzen
          Block<div> .right-content-wrapper.kompetenzen     <- max-width 56.88em
            Block<div> .text-size-40                        <- "Unsere Stärke: / Ganzheitliche Planung"
            Block<div> .points-wrapper
              Block<div> .point-wrap                        <- x6, identical structure
                Block<div> .name-detail-wrap
                  Block<div> .name-box-parent
                    Block<div> .name-box
                      Block<div> .text-size-15.grey         <- "01"
                      Block<div> .text-size-18.capital      <- "Generalplanung"
                    Block<div> .mini-grey-line
                  Block<div> .text-size-16.black            <- "Ein Partner für alle Leistungen"
                Block<div> .arrow-imgs-wrapper
                  Image .img.size-24.grey
                  Image .img.size-24.black
```
Note this is a **flex column of bordered rows**, not a CSS grid. There is no `display: grid` anywhere on
the site.

### The footer (section 9)
```
Section<section>
  Block<div> .main-container
    Block<div> .padding-vertical._40-48
      Block<div> .content-wrapper.footer                    <- flex column, gap 1.5em
        Block<div> .upper-bar.footer                        <- flex row space-between; column @medium
          Block<div> .left-content-wrapper.footer
            Block<div> .brand-logo
              Image
            Block<div> .footer-links-wrapper
              Block<div> .text-size-16.black                <- Kompetenzen
              Block<div> .text-size-16.black                <- Referenzen
              Block<div> .text-size-16.black                <- Über uns
              Block<div> .text-size-16.black                <- Team
              Block<div> .text-size-16.black                <- Karriere
              Block<div> .text-size-16.black                <- Blog
          Block<div> .right-content-wrapper.footer
            Block<div> .text-size-16.black                  <- "Gymnasiumstr. 12, 88400, Biberach, Deutschland"
            Block<div> .text-size-16.black                  <- "07351 440908 0"
            Block<div> .text-size-16.black                  <- "info@wallundbaur.de"
        Block<div> .footer-divider                          <- 0.06em tall, #d7d4c8
        Block<div> .upper-bar.footer                        <- second row, reused class
          Block<div> .right-content-wrapper.footer
            Block<div> .text-size-16.black                  <- "WALL+BAUR GmbH & Co. KG © 2025"
          Block<div> .left-content-wrapper.footer
            Block<div> .footer-links-wrapper
              Block<div> .text-size-16.black                <- Impressum
              Block<div> .text-size-16.black                <- Datenschutz
              Block<div> .text-size-16.black                <- Cookies
              Block<div> .text-size-16.black                <- Barrierefreiheit
```
**Every footer "link" is a `Block<div>`, not a `Link`.** The whole footer is unclickable. Note also the
second row puts `right-content-wrapper` *before* `left-content-wrapper` in source order to swap the
copyright to the left — a visual fix that leaves the DOM order backwards.

### Navbar
- Built on the **stock Webflow Navbar element** (`NavbarWrapper` / `NavbarBrand` / `NavbarMenu` /
  `NavbarLink` / `NavbarButton`), restyled rather than rebuilt from divs.
- `.nav-bar` is `background-color: transparent` — the nav sits over the hero background image, which is
  carried by the parent `.section`.
- Desktop: `.nav-container` is `flex; space-between; max-width: 120em; padding: 1.5em 2.5em 0.19em`.
  The six `NavbarLink.text-size-15` items are separated by literal `Block<div> .mini-nav-link-divider.is-hide`
  spacer bars (0.06em × 1.25em, `#898677`).
- **Mobile menu:** the stock Webflow dropdown. At `@medium` the `.navigation` menu gets
  `max-width: none; padding-right: 2.5em; padding-left: 2.5em; background-color: #262626` — i.e. it
  becomes a full-width dark panel. The dividers are hidden (`.mini-nav-link-divider.is-hide { @medium display:none }`)
  and each link gains `margin-top/bottom: 1.25em` for vertical rhythm.
- **Hamburger** is hand-built: `.hamburger` > three `.hamburger-bar` divs (`.top`, `.middle`, `.bottom`),
  each `1em × 0.13em`, `#f6f4ec`, with `.middle` taking `margin: 0.19em 0`. These bars are **only styled
  at `@medium`** — they have no base (desktop) CSS at all.
- **No sticky/fixed positioning** anywhere — the nav scrolls away with the page.
- **No IX interaction** drives the hamburger; it relies entirely on Webflow's built-in Navbar JS.

### Buttons
There is exactly **one** button pattern:
```css
.button      { display:flex; justify-content:space-between; align-items:center;
               grid-column-gap:0.5em; grid-row-gap:0.5em }
.button.nav  { text-decoration:none }
```
Contents are always `Block<div> .text-size-15[.black]` + `Image .img.arrow[.mustard]`.

Variants (all responsive-visibility, not visual):
```css
.button.nav.desktop-hide { display:none }
.button.nav.desktop-hide { @tiny  -> display:flex; justify-content:flex-start }
.button.nav.mob-hide     { @tiny  -> display:none }
```
- **There are no hover states on anything.** A `get_styles` query with
  `include_base_pseudos:["hover"]` across all 178 styles returned **zero** hover rules.
- **Only 2 of the 5 buttons are real links.** The two in the navbar are `Link` elements; the buttons in
  *Über uns*, *Unser Versprechen* and the *CTA* section are `Block<div> .button.nav` — visually
  identical, functionally dead.

### Variables
**There are none.** The site has one collection, `Base collection`
(`collection-94aa234c-2dbd-1cb6-dc21-f9ffa960e50b`, single mode `Base mode`), and
`get_variables` on it returns `[]`. **Every colour and every size in this build is a hard-coded literal
in a class.** Nothing maps to a Figma token by reference.

The de-facto palette, read off the CSS:
| Hex | Where it is used | De-facto role |
|---|---|---|
| `#f6f4ec` | `.heading-style-h1`, `.text-size-15/16/20/24`, `.current`, `.hamburger-bar`, `.text-size-18.capital.white`, `.text-size-40.light-grey` | cream — primary text on dark |
| `#262626` | `.bg.black`, `.navigation` @medium, `.text-size-15.black`, `.text-size-16.black`, `.text-size-18.capital`, `.text-size-20.black`, `.text-size-40` | near-black — dark surface + text on light |
| `#898677` | `.hero-divider`, `.mini-grey-line`, `.mini-nav-link-divider`, `.text-size-15.grey`, `.total` | warm grey — rules + muted text |
| `#c3bfae` | `.grey-color` (inline `Span`s) | pale warm grey — inline label text |
| `#d7d4c8` | `.footer-divider`, `.point-wrap` top/bottom borders | pale grey — hairlines on light |
| `#414039` | `.slide-card-content-wrapper`, `.text-wrapper.slide-heading`, `.tab-link` borders | dark hairline on dark |
| `#c2b122` | `.text-size-24.yellow`, `.text-size-15` `text-decoration-color` | mustard/olive accent |
| `#f3da05` | `.yellow-bullet` | bright yellow accent |
| `black` | `.marquee-text`, `.text-size-18` | pure black |
| `white` | `.left-card-wrapper.hero` left border | pure white |

Note **two different yellows** (`#c2b122` and `#f3da05`) and **two different "blacks"** (`#262626` and
`black`) coexist — see §4.

### Typography
Every text class hard-codes `font-family: Mulish`. **The `body` tag style was never touched** and still
carries the Webflow default `font-family: Arial; color:#333; font-size:14px; line-height:20px` — it is
simply always overridden by a class.

| Class | Element used on | font-size (base) | line-height | weight | other | @medium | @small | @tiny |
|---|---|---|---|---|---|---|---|---|
| `.heading-style-h1` | `Block<div>` | `6em` (96px) | `100%` | 400 | `uppercase`, `#f6f4ec` | `4em` | — | `3em` |
| `.heading-style-h1.centre` | CTA | inherits | | | `text-align:center` | | `2.5em` | |
| `.text-size-40` | section headings | `2.5em` (40px) | `1.3` | 400 | `#262626` | — | — | — |
| `.text-size-40.light-grey` | on dark | | | | `#f6f4ec` | — | `1.8em` | — |
| `.text-size-24` | card titles | `1.5em` (24px) | `1.1` | 400 | `uppercase`, `#f6f4ec` | — | — | — |
| `.text-size-20` | body lead | `1.25em` (20px) | `1.3` | 400 | `#f6f4ec` | — | — | — |
| `.text-size-20.black` | | | `1.4` | | `#262626` | — | — | — |
| `.text-size-18` | | `1.13em` (18px) | `1.4` | 600 | `black` | — | — | — |
| `.text-size-18.capital` | list item titles | | | | `uppercase`, `#262626` | — | — | — |
| `.text-size-16` | body copy | `1em` (16px) | `1.4` | 400 | `#f6f4ec` | — | — | — |
| `.text-size-16.black` | | | | | `#262626` | — | `0.88em` | — |
| `.text-size-15` | nav, eyebrows, buttons | `0.94em` (15px) | `100%` | **600** | `uppercase`, `#f6f4ec`, `text-decoration-color:#c2b122`, `text-underline-position:under` | `margin: 1.25em 0` | — | — |
| `.text-size-15.small-case` | | | | | `text-transform: capitalize` | | | |
| `.marquee-text` | keyword band | `9em` (144px) | `1.3` | 400 | `uppercase`, `black` | `5em` | — | — |
| `.current` / `.total` | slider counter | `0.94em` | `1` | 400 | `#f6f4ec` / `#898677` | — | — | — |

Verbatim, the two ends of the ramp:
```css
.heading-style-h1 {
  font-family: Mulish; color: #f6f4ec; font-size: 6em; line-height: 100%;
  font-weight: 400; text-transform: uppercase;
}
.heading-style-h1 { @medium -> font-size: 4em }
.heading-style-h1 { @tiny   -> font-size: 3em }

.text-size-15 {
  padding: 0px; font-family: Mulish; color: #f6f4ec; font-size: 0.94em;
  line-height: 100%; font-weight: 600; text-decoration: none;
  text-transform: uppercase; text-decoration-color: #c2b122;
  text-decoration-style: solid; text-underline-position: under;
}
.text-size-15 { @medium -> margin-top: 1.25em; margin-bottom: 1.25em }
```

### Spacing system
Two classes carry all of it.

**Horizontal — `.main-container`** (one class, used on 6 of 9 sections):
```css
.main-container {
  max-width: 120em; margin-right: auto; margin-left: auto;
  padding-right: 2.5em; padding-left: 2.5em;
}
.main-container { @small -> padding-right: 1.25em; padding-left: 1.25em }
.main-container { @tiny  -> padding-right: 1em;    padding-left: 1em }
```
`.nav-container` duplicates the same numbers instead of reusing the class:
```css
.nav-container {
  display:flex; max-width:120em; margin-right:auto; margin-left:auto;
  padding: 1.5em 2.5em 0.19em 2.5em; justify-content:space-between; align-items:center;
}
.nav-container { @tiny -> padding-right: 1em; padding-left: 1em }
```

**Vertical — `.padding-vertical` + a numeric combo.** The combo's *name is the Figma pixel pair* and the
value is that pair ÷ 16 in `em`:
```css
.padding-vertical._88-73   { padding-top: 5.50em;  padding-bottom: 4.56em  }   /*  88 / 73  */
.padding-vertical._201-80  { padding-top: 12.56em; padding-bottom: 5.00em  }   /* 201 / 80  */
.padding-vertical._80-80   { padding-top: 5.00em;  padding-bottom: 5.00em  }   /*  80 / 80  */
.padding-vertical._80-120  { padding-top: 5.00em;  padding-bottom: 7.50em  }   /*  80 / 120 */
.padding-vertical._120-120 { padding-top: 7.50em;  padding-bottom: 7.50em  }   /* 120 / 120 */
.padding-vertical._223-223 { padding-top: 13.94em; padding-bottom: 13.94em }   /* 223 / 223 */
.padding-vertical._40-48   { padding-top: 2.50em;  padding-bottom: 3.00em  }   /*  40 / 48  */
```
Responsive collapse of the same:
```css
.padding-vertical          { @medium -> padding-bottom: 5em }
.padding-vertical._120-120 { @medium -> padding-top:5em;    padding-bottom:5em }
.padding-vertical._120-120 { @small  -> padding-top:4em;    padding-bottom:4em }
.padding-vertical._201-80  { @medium -> padding-top:5em }
.padding-vertical._201-80  { @small  -> padding-top:4em;    padding-bottom:4em }
.padding-vertical._201-80  { @tiny   -> padding-top:3em;    padding-bottom:3em }
.padding-vertical._223-223 { @small  -> padding-top:5em;    padding-bottom:5em }
.padding-vertical._80-120  { @medium -> padding-bottom:5em }
.padding-vertical._80-120  { @small  -> padding-top:4em;    padding-bottom:4em }
.padding-vertical._80-80   { @small  -> padding-top:4em;    padding-bottom:4em }
.padding-vertical._88-73   { @tiny   -> padding-top:3em;    padding-bottom:3em }
```
So the designed values are honoured literally on desktop and then **snapped to a coarse 5em / 4em / 3em
scale** on tablet / mobile / small mobile.

**Gaps** are always `grid-column-gap` + `grid-row-gap` set to the same value on a flex parent, in `em`,
with values carried straight from the design (`10.75em`, `7.88em`, `5.63em`, `5.38em`, `3.75em`,
`2.5em`, `1.5em`, `0.5em`, `0.25em`…). Margins are used in only four places on the entire site.

### Responsive behaviour
Breakpoints actually customised: **`medium` (≤991), `small` (≤767), `tiny` (≤478)**. `large`, `xl` and
`xxl` are untouched — the site is desktop-up-scaled by the `1vw` trick instead (below).

What typically changes, in order of frequency:
1. **Flex row → column at `@medium`** — `.content-wrapper.kompetenzen`, `.content-wrapper.uber-uns`,
   `.content-wrapper.unser`, `.upper-bar.footer`, `.tabs`, and at `@small` `.lower-cards-wrapper`,
   `.left-content-wrapper.footer`, `.right-content-wrapper.footer`.
2. **Gaps shrink** — e.g. `.left-content-wrapper.uber-uns` 10.61em → 4em @small;
   `.content-wrapper.team` 7.88em → 4em @medium; `.left-card-wrapper.hero` 10.75em → 5em @small → 3em @tiny.
3. **`max-width` released** — `max-width: none` at `@medium` on `.left-content-wrapper.kompetenzen`,
   `.left-content-wrapper.uber-uns`, `.left-content-wrapper.unser`, `.right-content-wrapper.unser`,
   `.tab-pane`, `.navigation`, `.img.kompetenzen`; at `@small` on `.left-card-wrapper.hero`,
   `.slider-wrapper`.
4. **Font sizes step down** — only 6 classes do this (see the type table).
5. **Elements hidden** — `.mini-nav-link-divider.is-hide` @medium; `.button.nav.mob-hide` @tiny;
   `.button.nav.desktop-hide` shown @tiny.
6. **Container padding** 2.5em → 1.25em @small → 1em @tiny.

### Components
**Zero.** `get_all_components` returns `[]`. Nothing in this build is a Webflow component, so the
navbar and footer exist only once (which is fine here — there is only one page) and nothing has props
or variants.

### CMS
**None.** No collections, no collection lists, no bindings. The 12 team members, 7 reference projects
and 6 competence rows are all hand-duplicated static elements.

### Interactions / animations
- **Webflow IX: none.** `list_interactions` returns `{items: [], total: 0}`.
- Motion comes from exactly two places:
  1. **Webflow's native Slider and Tabs widgets** (two sliders, one tab set), using their built-in JS.
  2. **One CSS-only marquee**, driven by an `HtmlEmbed` inside `.marquee-horizontal`. The embed carries
     the class `👌-marquee-horizontal-alt-css` and animates its sibling `.track-horizontal-alt`:
     ```html
     <style>
     .track-horizontal-alt {
       position: absolute;
       white-space: nowrap;
       will-change: transform;
       animation: marquee-horizontal-alt 40s linear infinite;
       /* manipulate the speed of the marquee by changing "40s" line above*/
     }
     @keyframes marquee-horizontal-alt {
       from { transform: translateX(-50%); }
       to   { transform: translateX(0%); }
     }
     </style>
     ```
     The emoji-prefixed class name, the `-alt` suffix and the retained author comment are the signature
     of a third-party Webflow "marquee" cloneable pasted in wholesale. Note the CSS relies on
     `translateX(-50%)`, which only loops seamlessly if the track's content is duplicated ×2 — the
     track here holds **one** set of six keywords, so the band will visibly jump on each 40s cycle.
- Also present: one **native Webflow Lightbox** (`LightboxWrapper .video-wrapper`) wrapping a poster
  `Image` plus an absolutely-positioned `Image .img.play-button`.

### Custom code
- **Site head custom code: empty. Site footer custom code: empty.**
- **Page head custom code: empty. Page footer custom code: empty.**
- **Registered scripts: none.** No GSAP, no Lottie, no analytics, no cookie banner.
- All custom code on this site is **two `HtmlEmbed` elements on the page** (the `1vw` style block below,
  and the marquee keyframes quoted under *Interactions*). The first, the very first child of
  `.page-wrapper`, is the one that makes the whole system work:

```html
<style>
body {
 font-size: 1vw;
}
/* Max Font Size */
@media screen and (min-width:1920px) {
 body {font-size: 16px;}
}
/* Container Max Width */
.container {
  max-width: 1920px;
}
/* Min Font Size */
@media screen and (max-width:991px) {
 body {font-size: 16px;}
}
</style>
```
  This is **the single most important convention in the build.** Because `body` font-size is `1vw`
  between 992px and 1919px, and every length in the site is expressed in `em`, the *entire layout*
  scales proportionally with the viewport in that range, then locks to a 16px base above 1920 and below
  992. It is why 1em ≡ 16px at the design reference, and therefore why `px ÷ 16 = em` holds exactly
  everywhere.
  (Note the rule targets `.container`, a Webflow stock class that **is not used anywhere on this
  site** — that line is dead code carried in from the snippet's source.)

### Images
- 55 `Image` elements drawing on **16 distinct assets**.
- **Zero alt text.** Every one of the 55 images has settings `{visibility, assetId}` and no `altText`
  key at all.
- **No inline SVG.** Icons are raster assets sized by combo class (`.img.size-12`, `.size-24`,
  `.size-32`) or drawn in CSS (`.yellow-bullet`, `.mini-grey-line`, `.hamburger-bar`).
- Sizing convention: a global `img` tag style plus an `.img` class, then a size combo:
```css
img       { display:inline-block; width:100%; max-width:100% }
.img.size-12 { height:0.75em; max-width:0.75em }
.img.size-24 { height:1.5em;  max-width:1.5em  }
.img.size-32 { height:2em;    max-width:2em    }
.img.arrow   { height:1.5em;  max-width:1.5em  }
```
- Background images are attached to classes, not elements: `.section`, `.bg.lined`, `.bg.cta`, `.slide`
  each carry a `background-image: @img_<assetId>` with `background-size: cover`.

### Forms, links, SEO
- **Forms: none.** No `FormForm`, `FormBlock` or input anywhere. The two "Kontakt aufnehmen" buttons go
  nowhere.
- **Links: effectively none.** Only 2 `Link` elements and 6 `NavbarLink` elements exist, and
  **not one of them has an href/link setting** — their settings are `{visibility: true}` only. All
  footer navigation is `Block<div>`.
- **SEO: nothing set.** Page `seo: {}`, `openGraph: {imageUrl: null, imageAssetId: null}`.

---

## 3. Figma → Webflow mapping observed

| Design concept | How it was realised in Webflow | Notes / what changed |
|---|---|---|
| Page frame | The single `Home` page (`/`) | One-pager; no other pages exist |
| Section frame | `Section<section>` (unclassed) wrapping `.main-container` > `[.bg.*] >` `.padding-vertical.<px-px>` > `.content-wrapper.<section-name>` | Only the first section carries a class (`.section`, for the hero bg) |
| Section *name* | Becomes the **combo class**: `.content-wrapper.uber-uns`, `.content-wrapper.kompetenzen`, `.content-wrapper.team`… in German | Strongest signal that frame names were copied verbatim |
| Figma text style | A `text-size-<px>` global class + a colour combo | `text-size-15/16/18/20/24/40`; **named by px, valued in em** |
| Figma heading style | `.heading-style-h1` only | Applied to a `Block<div>`, **not** an `h1`. No h2–h6 classes exist at all |
| Colour variable | **Not carried across.** Hard-coded hex on each class, plus colour combos `.black` `.white` `.grey` `.light-grey` `.yellow` `.mustard` | The Webflow variable collection is empty |
| Figma component | **Not carried across.** Zero Webflow components; repeated elements are duplicated by hand | 12 team tabs, 7 slides, 6 competence rows all hand-copied |
| Auto-layout row/column | `display:flex` + `grid-column-gap`/`grid-row-gap` (always set as a pair, same value) | Gap values carried literally, in `em` |
| Auto-layout vertical padding | `.padding-vertical` + a combo **named after the literal px pair** (`._88-73`, `._201-80`, `._223-223`) | `px ÷ 16 = em`, exactly, every time |
| Content max width | `.main-container { max-width: 120em }` = 1920px | Design frame was almost certainly 1920, not 1440 |
| Horizontal page padding | `.main-container { padding: 0 2.5em }` = 40px | → 1.25em @small, 1em @tiny |
| Images | Webflow `Image` element + `.img` + a `.size-*` combo | No alt text on any of 55 |
| Icons | Raster `<img>` assets, or CSS-drawn divs | **No inline SVG anywhere** |
| Buttons | `.button` (flex) + `.nav` combo; label `div` + arrow `Image` | Only 2 of 5 are real `Link`s |
| Hover states | **Not built.** Zero `:hover` rules on any of the 178 styles | Either not designed or dropped |
| Breakpoint frames | `medium` (991), `small` (767), `tiny` (478) only | `large`/`xl`/`xxl` untouched |
| Fluid scaling between breakpoints | **`body { font-size: 1vw }`** in an HtmlEmbed, clamped to 16px at ≥1920 and ≤991 | The mechanism that makes the `em` values work |
| Sliders | Native Webflow Slider ×2 | Stock arrows replaced by `.left-arrow-mini`/`.right-arrow-mini`; `SliderNav` set to `display:none` |
| Tabs (team) | Native Webflow Tabs, 12 links / 12 panes | All 12 panes contain identical placeholder content |
| Video | Native Webflow Lightbox + poster image + play-button overlay | |
| Marquee | Third-party CSS cloneable pasted as an HtmlEmbed | *(inference)* |

**Changed / simplified between design and build (observed):**
- The section labelled `architektur-in-aktion` in the class names displays the heading **"Referenzen"** —
  the copy was renamed after the class was created.
- Desktop paddings are honoured exactly; **tablet/mobile paddings were rounded to a 5em / 4em / 3em
  scale** rather than being taken from mobile frames.
- **No design tokens were set up** (no Webflow variables), so the design system exists only as repeated
  literals.
- **No components were made**, so nothing is reusable.
- **Hover/interaction states were not built at all.**
- Section 6 (*Unser Versprechen*) uses a **bare `.padding-vertical`** with no numeric combo — it has no
  desktop vertical padding at all, only the inherited `@medium padding-bottom: 5em`. Instead
  `.left-content-wrapper.unser` carries `padding-top: 2.9em; padding-bottom: 2.9em`. This section was
  clearly spaced by a different method than the other eight.
- No Slack/Notion notes were visible in this environment, so no "why" can be sourced.

---

## 4. Quality notes and gotchas

### Deliberate team rules (consistent enough to be conventions)
1. **`body { font-size: 1vw }` + everything in `em`.** Clamped 16px above 1920 and below 992. This is
   the house fluid-scaling technique and it is the first element on the page.
2. **`px ÷ 16 = em`, always.** 15→0.94, 18→1.13, 24→1.5, 40→2.5, 88→5.5, 201→12.56, 223→13.94. No
   exceptions found.
3. **Class names carry the design's numbers.** `text-size-<px>`, `padding-vertical._<top>-<bottom>`,
   `.img.size-<px>`.
4. **Section shell is always** `Section > .main-container > [.bg.*] > .padding-vertical.<combo> >
   .content-wrapper.<section-name>`.
5. **Every section's wrapper gets a combo class named after the section** (in German, from the design).
6. **Base class = structure, combo class = variation.** Colour, size, case, and visibility are *all*
   expressed as combo classes on top of a bare global.
7. **Gaps, not margins.** `grid-column-gap` and `grid-row-gap` are always set together to the same
   value; margins appear only 4 times sitewide.
8. **Flex only.** `display: grid` appears nowhere.
9. **Native Webflow widgets preferred** over custom builds — Navbar, Slider, Tabs, Lightbox all stock,
   restyled.
10. `page-wrapper` → `main-wrapper` → `section` shell, with `.main-wrapper { overflow: clip }` to
    contain the marquee.

### Mistakes and inconsistencies
- **All 12 team tab panes contain identical placeholder content** — every one reads "Leitender
  Architekt", `telefon: 07351 440908 12`, `email: Jpvl@wallundbaur.de`, which belongs to tab 1
  (Jörg Pietzonka von Lorentz). The 12 tab *links* do have correct distinct names. **The team section is
  unfinished.**
- **The competence list numbers "05" twice** — items 5 and 6 are both `05` ("Projektsteuerung &
  Bauleitung" and "Gutachten & Beratung"). Item 6 should be `06`.
- **All six competence rows share the identical description** "Ein Partner für alle Leistungen" —
  placeholder copy never replaced.
- **Zero alt text** on all 55 images.
- **Zero links set** — six nav links, two nav buttons, ten footer items, all inert. The site is
  entirely unnavigable.
- **Three "buttons" are `Block<div>`, not `Link`** (Über uns, Unser Versprechen, CTA) — not focusable,
  not clickable, not keyboard-reachable.
- **No heading tags at all.** `.heading-style-h1` is on a `Block<div>`; every "heading" on the page is a
  `div`. The document has no `h1`–`h6` and therefore no outline.
- **`body` tag style never configured** — still Webflow's default `Arial / 14px / #333`. Harmless
  (always overridden) but it means there is no typographic fallback.
- **31 of 178 styles are empty** (defined, carrying no CSS at all): `.image.tabs`,
  `.content-wrapper.marquee`, `.text-size-15.white`, `.upper-bar`, `.page-wrapper`, `.slide-number`,
  `.lower-text-wrapper`, `.hamburger`, `.text-wrapper`, `.inner-text-wrap`, `.marquee-horizontal-alt-css`,
  `.hamburger-bar.top`, `.menu-button.bottom`, `.bg.bottom-lined`, `.arrow-imgs-wrapper`,
  `.left-content-wrapper`, `.left-text-wrap`, `.slides-wrapper`, `.content-wrapper`, `.img.size-24.grey`,
  `.upper-title`, `.img.arrow.mustard`, `.hamburger-bar.bottom`, `.img`, `.img.size-24.black`,
  `.text-box.uber-uns-lower-text`, `.bg`, `.right-content-wrapper`, `.lower-text-wrap`, `.image`,
  `.left-card-wrapper`.
  Several are load-bearing names used in the DOM (`.bg.bottom-lined`, `.img.arrow.mustard`,
  `.img.size-24.grey/.black`) that **do nothing** — the intended visual difference (e.g. the "mustard"
  arrow vs. the plain arrow, the grey vs. black hover-swap icons) was never actually styled.
- **The grey/black icon pair is a hover-swap that was never wired.** `.arrow-imgs-wrapper` holds
  `.img.size-24.grey` + `.img.size-24.black` stacked, but both combos are empty and there is no IX and
  no `:hover` rule, so both icons render identically and simultaneously.
- **Section wrapper order is inconsistent**: sections 2, 4, 9 nest `.main-container > .bg.*`, while
  sections 7 and 8 nest `.bg.* > .main-container`. Section 5 has `.bg.black` with **no**
  `.main-container` at all and compensates with a hand-written `padding-left: 2.5em` on
  `.text-wrapper.architektur-in-aktion`. Section 3 (marquee) has neither.
- **`.nav-container` duplicates `.main-container`'s numbers** (`max-width:120em`, `padding: 0 2.5em`,
  `@tiny 1em`) instead of reusing the class — two places to change.
- **Responsive visibility gap**: `.button.nav.mob-hide` only hides at `@tiny` (≤478) and
  `.button.nav.desktop-hide` only shows at `@tiny`, so between **479px and 991px both buttons render at
  once**.
- **Two accent yellows** (`#c2b122` mustard, `#f3da05` bright) and **two blacks** (`#262626`, `black`)
  are in play with no rule distinguishing them; `.text-size-18` uses `black` while its own
  `.capital` combo overrides to `#262626`.
- **Typos in class names**: `.img.profille` (profile), `.slider-navv-bar` (double v),
  `.text-wrap.tem-right-description` (team), `.heading-style-h1.centre` (British spelling in an
  otherwise US/German-mixed system).
- **Mixed naming case**: most classes are kebab-case, but `Heading-style-h1`, `Image`, `Mask`, `Mask 2`,
  `Menu Button`, `Slide`, `Slide Nav`, `Slide Nav 2`, `Tabs Content`, `Left Arrow`, `Right Arrow`,
  `Referenzen` are Title Case **with spaces** — these are Webflow's auto-generated widget class names
  that were never renamed.
- **One unit slip**: `.point-wrap` uses `padding-bottom: 24px` (raw px) while its siblings use
  `padding-top: 1.5em; padding-right: 1.5em`. In the `1vw` system this breaks the proportional scale.
  Same for `.marquee-horizontal { height: 120px }`.
- **`.left-arrow` and `.right-arrow` are `display:none`** and unused — leftovers from a stock slider.

### Special features
- **Language: German** throughout (content and class modifiers). No localisation, no RTL, no dark-mode
  toggle (the "dark" sections are just `#262626` backgrounds).
- **Sliders:** native Webflow Slider ×2 — hero mini-slider (4 slides) and Referenzen (7 slides). Both
  have their `SliderNav` dots set to `display:none` and use custom mini arrows.
- **Tabs:** native Webflow Tabs, 12 links / 12 panes, used as a team-member switcher.
- **Modal/video:** native Webflow Lightbox on `.video-wrapper`.
- **Marquee:** CSS-only, from a pasted cloneable.
- **Glassmorphism:** exactly one instance — the hero quote card
  `.left-card-wrapper.hero { background-image: linear-gradient(90deg, hsla(50,7.09%,50.20%,0.20), rgba(137,134,119,0)); backdrop-filter: blur(12px); border-left: 1px solid white }`.
- **No sticky elements, no scroll-triggered animation, no accordion, no modal, no form.**

---

## 5. Verbatim dumps

Omitted from the skill copy; the full dump lives in the analysis workspace.

## 6. Tool quirks

### Environment blockers (both of the brief's primary sources were unavailable)
- **The Figma file is inaccessible to this MCP account.** Every call —
  `get_metadata {fileKey}` (no nodeId), `get_metadata {fileKey, nodeId:"0:1"}`,
  `get_screenshot {fileKey, nodeId:"0:1", maxDimension:1200}` — returned the identical error:
  `Looks like you don't have edit access to this file. The file owner can share it with you and make you an editor.`
  (Figma Debug UUIDs `70037743-…`, `954bc9d6-…`, `5cc98b4d-…`.) Note the Figma MCP appears to require
  **edit** access even for read-only tools. Retried 3×; do not retry further — this needs the file owner
  to share the file. **§1 of this brief cannot be completed for this project without that.**
- **`curl` to the live site is blocked.** `curl -sL https://wall-baur-090f38.webflow.io/` → **exit 56**,
  and `curl -sS http://127.0.0.1:39079/__agentproxy/status` reports
  `"kind":"connect_rejected","detail":"gateway answered 403 to CONNECT (policy denial or upstream failure)","host":"wall-baur-090f38.webflow.io:443"`.
  The same host appears in `recentRelayFailures` alongside `medicini-plus.webflow.io` and
  `furec.webflow.io` from earlier runs — **`*.webflow.io` is blanket-denied by org egress policy.**
  Do not plan on rendered HTML or `webflow.css` for any project in this batch.
- **Figma/image asset URLs are blocked too**, so no screenshots could be downloaded into the run folder.
  (`get_screenshot` with `enableBase64Response:true` is the documented workaround, but it is moot here
  since the file itself is inaccessible.)
- **Consequence: the Webflow Data MCP is a complete substitute.** `get_all_elements` + `get_styles` +
  `get_settings` between them yielded every fact in §2–§5. ~18 tool calls covered the whole site.

### Webflow MCP — call shapes that worked
- `agent_id` used throughout: `fable-5.1|claude-code|w9r4t`. First call `session_id:"start"`; the result
  opens with a text block `[session_id issued …]` then `session_id: ses_3JeMC64ZIW2Vd9MWy5AsV4fARMP` —
  reuse that exact value on every later call.
- **The `siteId`/`pageId` split is the #1 source of validation errors** (confirms earlier runs):
  - Require **both `siteId` AND `pageId` at the top level**, even for site-wide reads:
    `data_style_tool`, `data_variable_tool`, `data_element_tool`, `data_element_settings_tool`,
    `data_component_tool`, `data_interactions_tool`.
  - Take `site_id`/`page_id` **inside the action object**, and **no** top-level `siteId`/`pageId`:
    `data_pages_tool`, `data_scripts_tool`, `data_fonts_tool`.
- `data_pages_tool` → `actions:[{label, list_pages:{site_id, limit:100}}]`. Worked first try.
- `data_element_tool > get_all_elements {depth:-1}` → **175,945 chars, over the limit**, auto-saved to
  `/root/.claude/projects/<proj>/<sess>/tool-results/mcp-Webflow-data_element_tool-<ts>.txt`.
  **It is a plain JSON object** (not the `[{type,text}]` array shape the Figma tool uses):
  ```python
  d = json.load(open(F))['result'][0]['data']   # -> the Body node
  ```
  Then walk `n['type']`, `n.get('styleNames')`, `n['settings']`, `n.get('children')`.
  **Gotcha: element text is on `String` child nodes under the key `textContent`** — not `text`, and not
  on the parent. Skipping `String` nodes loses all copy; my first pass produced a tree with no text.
- `data_style_tool > get_styles {query:"all", include_properties:false}` → 178 entries, fits inline
  (~28KB). Each is `{id, name, isFromLibrary, isComboClass, type:"global"|"combo"|"tag", selector}`.
- `data_style_tool > get_styles {query:"all", include_properties:true, include_breakpoints:["medium","small","tiny"], include_base_pseudos:["hover"]}`
  → 54,527 chars, overflowed to a file. **This one call replaced the entire CSS-scraping step** — do it
  early. **The property nesting is two levels deeper than it looks:**
  ```
  style.properties.base.properties          -> {css-prop: value}   # the desktop rules
  style.properties.base.pseudos.hover.properties
  style.properties.breakpoints.medium.properties
  style.properties.breakpoints.medium.pseudos.<x>.properties
  ```
  Reading `properties.base` directly gives you `{}`-looking output and will make you think the site has
  no CSS. Also: `breakpoints` only contains keys that actually carry overrides.
- Background images come back as `background-image: @img_<assetId>` (a token, not a URL). Colours here
  came back as literal hex **because this site has no variables**; on a site that uses them you would get
  `{"id":"variable-<guid>"}` and need to join against `data_variable_tool`.
- `data_variable_tool` → `get_variable_collections {query:"all"}` first (returns `collection-<guid>`),
  then `get_variables {variable_collection_id, include_all_modes:true}`. Here it returned `[]` —
  **an empty array is a real finding, not an error.**
- `data_component_tool > get_all_components {options:{includeProps,includeInstanceCount,includeVariants}}`
  → `[]`. Cheap; do it early to learn whether anything was componentised.
- `data_interactions_tool > list_interactions {limit:100}` → `{items:[], total:0}`.
- `data_scripts_tool` → `get_site_freeform_code {site_id}`, `get_page_freeform_code {page_id}`,
  `get_registered_scripts {site_id}` — two actions in one call worked fine.
- `data_element_settings_tool > get_settings {type:"all_resolved_settings", element_id:{component, element}}`
  is how you read an **HtmlEmbed's code**; it arrives under `{"key":"code","value":"<style>…"}`.
  `element_id` is a **two-field object copied verbatim** from the element tree, never a string.

### Rate limiting — worse on this run than previously documented
- **429s hit four separate tools**, not just one: `data_variable_tool`, `data_element_settings_tool`,
  `data_scripts_tool`. Errors arrive in two different shapes:
  - `{"name":"Error","message":"Too Many Requests"}`
  - `{"name":"WebflowApiError","message":"Too Many Requests","status":429,"code":"too_many_requests"}`
  - and a third, path-specific: `{"name":"Error","message":"Tool get_settings failed: GET /v2/pages returned 429"}`
- In a 2-action batch the **second action alone** 429'd while the first succeeded — re-issue the single
  failed action, not the batch.
- **The worst case took five attempts:** the second `HtmlEmbed`'s code
  (element `c76c5549-3f3d-cbdc-40f1-a16e6fd14bf9`, the marquee CSS) returned
  `GET /v2/pages returned 429` on **four consecutive attempts** spread over several minutes — including
  with `type:"query_settings"` + `key:"code"` instead of `all_resolved_settings` — then succeeded on the
  fifth, roughly 10 minutes after the first. The first embed on the same page had read fine moments
  earlier, so this is a per-path budget on `/v2/pages` that recovers slowly. **Read all `HtmlEmbed`
  codes as multiple actions in one single call** rather than one call per embed, and **do not give up
  after two or three 429s — keep the request queued and retry later in the run.**
- Practical pacing that worked: ≤2 network-heavy actions per call, and a ~100s pause after a 429 before
  retrying a *different* endpoint (the same endpoint stayed limited much longer).

### Misc
- `webflow_guide_tool` was not called — the condensed copy at
  `/home/user/talha-vercel-test/.claude/skills/figma-to-webflow/references/webflow-mcp-guide.md`
  already exists and is accurate.
- `element_snapshot_tool` was not attempted (image egress is blocked, so it would be useless).
- **Budget observed: ~20 tool calls**, of which 3 were blocked network attempts, 4 were 429 retries and
  1 needed five attempts. The two calls that overflow to files (`get_all_elements depth:-1`,
  `get_styles include_properties:true`) are the high-value ones — let them overflow and parse locally.
