# HAUSVERTRAUEN

German real-estate advisory brochure site (Baden-Baden / Wiesbaden region), 6 pages, built Jan–Feb 2026 by Xerosol. Figma page analysed: "Page 1" (`figma_page1_metadata.xml`, a metadata-only export, 345KB / ~4300 lines). Webflow analysed: workspace copy, site id embedded in the element tree as component `6992ed88c69c1323d7a4bcef`, **never published** (per `extra_from_main.md`).

> Scope note: this pass used only files already on disk — no live Figma or Webflow MCP calls were made (they hang in this session). See §6 for exactly what that does and doesn't let this report claim.

---

## 1. Figma file anatomy

### Top-level frames/sections on "Page 1" (canvas x-order, from `figma_page1_metadata.xml`)

The XML is a flat metadata dump of one Figma page. Parsing its direct children (Python `xml.etree`, stripping the trailing "IMPORTANT: call get_design_context…" tool-instruction text that is appended after `</canvas>` and breaks naive parsers) gives **26 top-level nodes**:

| # | name | type | x | w | h | role |
|---|---|---|---|---|---|---|
| 1 | `IMMOBILIENBEWERTUNG Subapge` | frame | 35645 | 1920 | 9107 | built as `/immobilienbewertung` (multi-step valuation wizard: "Q1", "Wo befindet sich Ihre Immobilie?", progress "1/8") |
| 2 | `Über Mich & Partnernetzwerk` | frame | 49794 | 1920 | 13249 | **no matching live page** — About-me/partner content, not one of the 6 shipped pages |
| 3 | `Referenzen V1` | frame | 52051 | 1920 | 7530 | **no matching live page** — references/testimonials exploration |
| 4 | `Referenzen V2` | frame | 56268 | 1920 | 9728 | second pass at the same idea, also unbuilt |
| 5 | `Referenzen V1` (dup, smaller) | frame | 54011 | 1920 | 1962 | third copy, same name reused twice |
| 6 | `Group 1577705873` | frame | 49794 | 778 | 836 | stray asset group |
| 7–10 | `Frame 152/153/154/167` | frame | 37772 | 1920 | 1080 each | stacked hero-concept explorations, generic Figma auto-names |
| 11 | `Homepage design` | **section** | −37952 | 12675 | 11380 | exploration container — holds a **draft** `Home` frame (1728×11220) plus `Frame 143` (nested `V1`/`V2` variants) plus 8 more `Frame 144…151` (1728×1080 each) — early iterations, not the build source |
| 12 | `Home` | frame | 30659 | **1728** | 11220 | **the actual build-from frame** (see below) |
| 13 | `Sub Page: Ihre Situation` | **section** | −24739 | 19162 | 12165 | one shared template, duplicated (`Ihre Situation` appears twice inside, plus `Inner`/`Info`/"State 01→02 (50%)"/"State 04 (last)" scroll/hover-state frames) — this single template is the source for **three** live pages: Erbe, Scheidung, Senioren |
| 14 | `Updated 02/2/2026` | text | 30578 | — | — | a manual date label the designer left on the canvas — corroborates the Feb-2026 build date first-hand |
| 15 | `Frame 1577705891` | frame | 43841 | 4401 | 2981 | leftover reference/moodboard block |

**Only 2 of the 26 top-level nodes are the real build sources**: the `Home` frame (12) and the `Sub Page: Ihre Situation` section (13). Everything else — `Über Mich & Partnernetzwerk`, both `Referenzen` explorations, the `Homepage design` scratch section, the 4 stray `Frame 152-167` hero concepts, `Frame 1577705891` — is design debris with no corresponding shipped page. That's roughly **60% of the top-level canvas content never got built.**

### The build-from `Home` frame — width and internal structure

`Home` (id `88:4062`) is **1728px wide**, not the more common 1920px "desktop master" convention seen on other Xerosol projects — worth flagging because every *subpage* frame (`IMMOBILIENBEWERTUNG Subapge`, `Über Mich…`, `Referenzen…`, `Ihre Situation`) is 1920px wide. The homepage master and the subpage masters use two different canvas widths in the same file.

Its direct children, sorted by `y` (top → bottom, i.e. the section order):

| Figma frame name | y | h | maps to Webflow section |
|---|---|---|---|
| `Hero` | 0 | 1080 | `.section-hero` |
| `Intro` | 1080 | 1471 | `.section-step` ("Ihr nächster Schritt" 2-card grid) |
| `Case` | 2551 | 1080 | `.section-strategy` (the "12% über Bewertung in 39 Tagen" case study) |
| *(unnamed, `Frame 92`/`partners list`)* | 3743–4009 | ~560 | `.section-partners` |
| *(unnamed, `Frame 93`/`Frame 91`)* | 4843–5034 | ~558 | `.section-reviews` |
| `So funktioniert es` | 5762 | 966 | `.section-works` |
| *(unnamed, `Frame 37`/`Frame 113`)* | 6830–7096 | ~645 | `.section-situation` |
| *(unnamed, `Group 1577705883`/`Frame 111`)* | 7981–8300 | ~600 | `.section-location` |
| *(unnamed, `Rectangle 15`/`Frame 112`/`Frame 18`)* | 9060–10053 | ~1080 | CTA component |
| `Rectangle 35` / `HAUS VERTRAUEN E3` / `ELEMENT C1` / `links` ×5 / `Frame 96` (Google badge) / `Group 31` | 10140–11220 | ~1080 | Footer component |

**Only 4 of ~10 content sections got a real name** (`Hero`, `Intro`, `Case`, `So funktioniert es` — the last one literally the German copy, not a design-system label). The other six are generic Figma auto-names (`Frame 91`, `Frame 92`, `Frame 93`, `Frame 111`, `Frame 112`, `Frame 113`, `Group 1577705883`). All the descriptive `section-*` class names seen in Webflow (`section-partners`, `section-reviews`, `section-situation`, `section-location`) were **invented by the builder**, not carried over from Figma layer names.

### Breakpoints in Figma: there aren't any

A full width-census over every node in the file (`Counter` over all `width` attributes) turns up **no** frames at the common Webflow breakpoint widths — no 991/992, no 767/768, no 479/480, no 390–393. The two hits for "834" width shown in a naive scan are unrelated groups, and the only two 375px-wide nodes are text boxes ("Wo liegt Ihre Immobilie?"), not frames. The dominant widths are **1728** (140 occurrences — the Home master and its internal group repeats), **1920** (134 — every subpage master), and **1872** (137 — the common inner content width, see §3).

**This Figma file is desktop-only.** There is no tablet or mobile design anywhere on the page. Yet the shipped Webflow build has tablet/mobile combo classes (`heading-styles-h2.tablet`, `heading-styles-h3.desktop`, `nav-links.desktop`, a hamburger menu) — meaning all responsive behaviour on this site was **built blind**, invented by the Webflow builder with no Figma reference at any narrower breakpoint. Design tokens (spacing/type scale as Figma Variables) are likewise **not available** in this export — there is no `get_variable_defs`-equivalent data on disk for this file; only 4 *Webflow* colour variables exist (§2).

### The "Ihre Situation" template → 3 live pages

`Sub Page: Ihre Situation` (id `88:1574`, 19162 wide) contains the string **`Ihre Situation`** as a frame name **twice**, plus a set of `Inner`/`Info`/interaction-state frames (`Scroll intro / enter`, `hover on card`, `State 02 (rest)`, `State 01→02 (50%)`, `State 04 (last)`) that look like tab/accordion states for a single situation-selector component, not four separate page layouts. Text layer names inside this section include `Erbe`, `Scheidung`, `Senioren` and also **`Notverkauf`** (with three spelling variants found on canvas: `Notverkauf`, `Notverkauf – 24-h-Plan`, `Notverkauf – 24h–Plan` — inconsistent hyphen vs. en-dash). Four scenarios were designed on one shared template; **only three were built as pages** (see §4 for the dangling 4th link).

---

## 2. Webflow site anatomy

### Pages (from `extra_from_main.md`, 6 total, no CMS)

`Home` (`/`, id `6992ed88c69c1323d7a4bcef`), `Immobilienbewertung`, `Verkaufen`, `Erbe`, `Scheidung`, `Senioren`. Every page's SEO title is just the page name — **no meta description, no OG tags, on any page.** Not checked for CMS collections because the site is a static brochure with no collection-template pages in the page list; `extra_from_main.md` confirms this explicitly.

### Class-naming verdict: literal-pixel naming, not a token scale

This site's dominant naming convention is **unique among the sites in this batch**: instead of a semantic type/spacing scale (`text-size-regular`, `gap-md`), classes are named after the **literal pixel value** they were meant to hold: `text-size-14px` … `text-size-36px` (10 sizes), `gap-5px` … `gap-48px` (9 gaps), `icon-14px` … `icon-120px` (16 sizes), `max-wd-524px` … `max-wd-1358px` (12 widths), `padding-10px`, `logo-70px`/`logo-265px`. 30+ of these verbatim, grouped:

- **Type-size classes (px in the name)**: `text-size-14px`, `text-size-15px`, `text-size-16px`, `text-size-18px`\* , `text-size-19px`, `text-size-22px`, `text-size-25px`, `text-size-26px`, `text-size-28px`, `text-size-36px`.
- **Gap/spacing classes**: `gap-5px`, `gap-8px`, `gap-10px`, `gap-14px`, `gap-15px`, `gap-16px`, `gap-18px`, `gap-24px`, `gap-48px`, `padding-10px`.
- **Icon-size classes**: `icon-14px`, `icon-18px`\*, `icon-20px`, `icon-24px`, `icon-32px`, `icon-40px`, `icon-43px`, `icon-44px`, `icon-49px`, `icon-50px`, `icon-53px`, `icon-57px`, `icon-60px`, `icon-65px`, `icon-70px`, `icon-120px`, plus a broken `icon-80` (missing its `px` suffix).
- **Width-cap classes**: `max-wd`, `max-wd-197`, `max-wd-524px`, `max-wd-532px`, `max-wd-580`, `max-wd-600`, `max-wd-647`\*, `max-wd-728`, `max-wd-847px`, `max-wd-850`, `max-wd-1092`, `max-wd-1358` — note the **inconsistent `px` suffix**: some carry it (`max-wd-524px`), some don't (`max-wd-580`).
- **Logo classes**: `logo-70px`, `logo-265px`.
- **Grid-count classes** (same habit, applied to columns): `grid-2x1`, `grid-4x1`, `grid-5x1`.

\*`text-size-18px`, `icon-18px` and `max-wd-647` are not plain ASCII — every one of their trailing `px`/digits is followed by a **non-breaking space (U+00A0)**, not a regular space (confirmed byte-for-byte with Python: `'text-size-18px\xa0'`, `'icon-18px\xa0'`, `'max-wd-647\xa0'`). There is no clean ASCII counterpart for any of the three — the NBSP-suffixed name is the *only* version that exists, and it's the one actually applied 30+ times on the homepage alone (`grep -c 'text-size-18px' wf_home_tree.txt` → 30). This is very likely a copy-paste artefact (pasted from a doc/Figma layer name carrying an NBSP) baked permanently into the class name.

The site also has **no `padding-global`** class (the wrapper-level side-gutter utility every other class-naming report in this batch has flagged) — `main-container` here is a bare block class with no separate global-padding partner; side margins are presumably set directly on `main-container` or `wrapper-*` instead of split into a `padding-global`/`container` pair.

**Verdict**: this is a "measure-and-name" convention, not a design-token system. Every size the designer drew gets its own literal-value class; there's no evidence of a disciplined 4/8-pt spacing grid (`gap-5px`, `gap-10px`, `gap-14px`, `gap-15px` all coexist) or a clean type scale (14/15/16/18/19/22/25/26/28/36 — no consistent ratio). See §3 for the px÷16 check.

### Homepage DOM skeleton (body → hero → one card grid → footer)

From `wf_home_tree.txt` (rendered element tree) / `wf_home_tree_raw.json` (raw, confirms no extra data beyond what's rendered):

```
Body
  Block.page-wrapper
    ComponentInstance "Global Styles"      ← no props, no slots, purely a marker/embed carrier
    Block.main-wrapper
      ComponentInstance "Navbar"           ← no props
      Section.section-hero    → Block.main-container > Block.wrapper-hero
      Section.section-step    → "Ihr nächster Schritt" 2-card grid (.step-grid > .step-card ×2)
      Section.section-about   → 3 stat cards (168 / 9,8% / 27)
      Section.section-strategy→ case-study block with absolute .card-absolute overlay
      Section.section-partners→ .partner-grid, 5 partner cards + 1 "become a partner" CTA card
      Section.section-reviews → .reviews-grid, 3 Google-review cards + 1 aggregate-rating card (4.9, 152 Bewertungen)
      Section.section-works   → .sticky-cover > .cards-scroll, 6 numbered step cards (horizontal-scroll pattern)
      Section.section-situation → .situation-grid, 4 cards: Erbe / Scheidung / Senioren / Notverkauf
      Section.section-location→ Block.main-container>...  +  Block.location-wrap (SIBLING, not nested — see §4)
      ComponentInstance "CTA"
      ComponentInstance "Footer"
```

Every `Section` except `section-location` follows the same `Section > Block.main-container > Block.wrapper-<topic>` shell. There is exactly **one card grid pattern reused everywhere** (`*-grid > *-card`), just re-skinned per section (`step-grid/step-card`, `partner-grid/partner-card`, `reviews-grid/review-card`, `situation-grid/situation-card`) — hence "one card grid" in the brief is accurate: it's the same structural idea eight times over, not eight different layouts.

### Navbar

Not expanded in the element tree (it's a `ComponentInstance` with `props: []`, `slots: []` — internals live inside the component, out of scope for this pull). Its supporting classes exist in `styles_global.txt`: `navbar`, `nav-fixed-cover`, `nav-flex`, `nav-brand`, `nav-links` (+ combo `.nav-links.desktop`), `nav-link` (+ combo `.nav-link-large.is-tab`), `nav-menu`, `nav-menu-inner` (+ combos `.is-1920px`, `.is-1920px.is-relative`, `.is-control`, `.is-method`, `.is-relative` — page-specific width/position variants), `nav-right-wrap`, `nav-vector`, `hamburger`, `hamburger-bar` (+ combos `.top`, `.bottom`), `menu-button`, `dropdown`, `dropdown-toggle`, `dropdown-icon`, `dropdown-list`, `dropdown-link` **and** `dropdown-link Copy` (a Webflow auto-generated duplicate that was never renamed — dead/duplicate class), `dropdown-spacer`, `logo-70px`/`logo-265px`.

### Buttons / links

Confirmed by `extra_from_main.md`: **buttons are not a component** on this site — every CTA is a plain `Link` element with a size/style combo: `.link` (base), `.link.is-cream`, `.link.border`, `.link.desktop`. All button icon-arrows are `Image.icon-24px`/`icon-relative`/`icon-absolute` pairs (a two-layer icon technique — a relatively-positioned placeholder plus an absolutely-positioned actual icon, seen repeatedly: `icon-24px.flex > icon-relative + icon-absolute`, and again at 44px/90px for the review-badge stars).

**Every `Link` element on the homepage has an unset href.** Walking `wf_home_tree_raw.json` and printing `settings.link` for all 14 `Link` nodes on the homepage returns `None` for all 14 — not `"#"`, just **absent**. The only two `"#"` placeholder links on the whole homepage tree come from the **CTA component's** `Lower Link`/`Upper Link` props (`{"mode":"url","to":"#"}`), not from anything built directly on the page. So: 14 dead/unset links + 2 explicit placeholder links = **every single interactive link on the homepage goes nowhere.** Consistent with the site never having been published.

### Variables (Base collection, single mode, 4 total — from `extra_from_main.md`)

| name (verbatim) | value |
|---|---|
| `Cream ` (**trailing space in the variable name itself**) | `#e9e2d2` |
| `Dark Blue` | `#122a3f` |
| `Brown` | `#af9770` |
| `White` | `hsla(41.7, 0%, 100%, 1)` — 0% saturation makes the hue meaningless, but it's stored as a non-zero 41.7° hue rather than the simpler `hsla(0,0%,100%,1)` or plain `#ffffff` |

No font, spacing, or radius variables exist — the only tokenised thing on this site is **colour**. Everything else (type sizes, gaps, icon sizes, max-widths) is hard-baked into the literal-px class names described above, the opposite of a token-first system. The 4 colours surface as `text-color-blue`, `text-color-brown`, `text-color-cream`, `text-color-white` combo modifiers throughout `styles_combo.txt` (e.g. `.card-title.text-color-brown`, `.heading-styles-h1.text-color-blue`, `.text-size-18px.text-color-cream`).

### Typography

No computed CSS (font-size/line-height/letter-spacing/colour per breakpoint) is available on disk for this site — `wf_styles_raw.json` is a **names-only** style list (`id`, `name`, `isFromLibrary`, `isComboClass`, `type`, `selector` — no `properties`/`breakpoints` key on any of its 569 entries; confirmed with `python3 -c "... 'properties' in x"` → 0 hits). What *is* available:

- **Tag-level styles**: `body`, `h1`, `h2`, `a` — only 4 bare-tag selectors (no `h3`/`h4`/`h5`/`h6` tag style; those sizes exist only as classes).
- **Size classes**: `heading-styles-h1`, `heading-styles-h2`, `heading-styles-h3`, `heading-styles-h4`, plus the 10 literal `text-size-*px` classes above.
- **Responsive duplication pattern**: `heading-styles-h2.desktop` / `heading-styles-h2.tablet` and `heading-styles-h3.desktop` / `heading-styles-h3.tablet` exist as combo pairs. On the homepage this shows up once, and tellingly: the "Über HausVertrauen" h2 appears **twice**, back to back —
  ```
  Heading<h2>.heading-styles-h2.desktop
    String "                                      Unabhängige Immobilienberatung f"
  Heading<h2>.heading-styles-h2
    String " Unabhängige Immobilienberatung für Baden-Baden und Region. Klarheit v"
  ```
  — i.e. two copies of the same heading, one padded with ~35 leading spaces (probably to force a visual line-break at desktop width) and a bare-class copy meant for narrower breakpoints, toggled by `display` rather than by a single fluid heading. This is a **duplicate-content responsive technique**, not real responsive typography — and it's fragile: two independently-editable copies of the same sentence.
- Weight/decoration combos ride on top of size classes rather than being separate: `.weight-400`, `.weight-500`, `.weight-600`, `.underline`, `.opacity-70`/`.opacity-80`, `._1st-cap` (a "capitalise first letter only" modifier), `.all-caps`, `.spacing` (likely letter-spacing).

### Spacing

Same literal-px habit as typography: `gap-5px`…`gap-48px`, `padding-10px`. No evidence of a disciplined 8pt grid — `gap-5px`, `gap-10px`, `gap-14px`, `gap-15px` sit alongside `gap-8px`, `gap-16px`, `gap-24px`, `gap-48px` in the same file, i.e. some gaps are on an 8pt rhythm and some aren't.

### Responsive approach

No `main/medium/small/tiny` breakpoint-override classes were captured in the styles dump the way an em-based system would show (`is-*` combos here read as *content* variants — `.is-erbe`, `.is-divorce`, `.is-senior` — more than breakpoint variants). The two confirmed breakpoint-scoped combos are `heading-styles-h2/h3 .desktop`/`.tablet` (duplicate-content swap, see above) and the navbar's `hamburger`/`hamburger-bar`/`menu-button`/`.nav-links.desktop` classes (native Webflow Navbar widget responsive behaviour). Given §1's finding that **no tablet/mobile frames exist in Figma at all**, this site's mobile/tablet layout was designed live in the Webflow Designer, not handed off.

### Components (4, from `extra_from_main.md`)

| component | instances (site-wide) | props |
|---|---|---|
| `Global Styles` | 6 | none — invisible marker/embed carrier, first child of every page's `page-wrapper` |
| `Navbar` | 6 | none |
| `Footer` | 6 | none |
| `CTA` | 6 | **Tag Text**, **Title**, **Lower Link Text**, **Lower Link** (link), **Upper Link Text**, **Upper Link** (link), **Upper Link Visibility** (visibility toggle) |

**CTA is the only configurable component**, and its real values on the homepage (pulled from `wf_home_tree_raw.json`) are:

```
Tag Text:              "Klarheit in 45 Minuten."
Title:                 "Wert, Optionen, nächster Schritt – neutral beraten, Verkauf nur auf Wunsch."
Lower Link Text:        "Immobilienwert berechnen"
Lower Link:             {mode: "url", to: "#"}
Upper Link Text:        "Gespräch vereinbaren"
Upper Link:              {mode: "url", to: "#"}
Upper Link Visibility:   false   (hasOverride: true — deliberately hidden on Home)
```

So the "Upper Link" (a secondary CTA, "Gespräch vereinbaren" / book a call) is switched off specifically on the homepage via the visibility prop, while its text/link fields are still populated underneath — a real, working use of a visibility prop to trim a shared component per-instance.

### Special features / interaction patterns visible in the class list

- **Section-tag pattern**: every section opens with a small pill — `Block.section-tag > Image.icon-*px + Block.text-size-18px` (e.g. "Ihr nächster Schritt", "Über HausVertrauen", "FALL", "Unsere Partner", "Verifizierte Stimmen", "Ihre Situation", "Standorte") — a consistent eyebrow-label component reused verbatim across all 9 homepage sections (combo variants: `.section-tag.absolute`, `.section-tag.is-center`).
- **Horizontal sticky-scroll**: `.section-works` wraps its 6 numbered step cards in `Block.sticky-cover > Block.cards-cover > Block.cards-scroll`, the classic Xerosol pattern for a pinned section whose card row scrolls horizontally while the page scrolls vertically. No IX/interaction data was pulled, so the trigger mechanism itself isn't confirmed, only the structural scaffolding for it.
- **Two-layer relative/absolute icon technique**, applied uniformly: `icon-relative` (spacer/placeholder in flow) + `icon-absolute` (the actual icon, positioned over it) inside a sized wrapper — used for hero button icons, the "45 Min" arrow icons, and the 44px/90px Google-badge icon+star pair.
- **Literal `spacer` class** exists as a dedicated empty-div spacing utility (as distinct from the `gap-*px` flex-gap classes) — a second, structural way of adding whitespace alongside the flex-gap system.
- **`grid-choose` / `grid-2x1` / `grid-4x1` / `grid-5x1`**: grid classes named by literal column count rather than by content (`.grid-choose` is the exception — content-named), continuing the literal-value naming habit from spacing/type into layout.
- **`span-brown`** — an inline `<span>`-level colour-accent class (`.span-brown.no-underline` combo), used to recolour a word or two inside body copy without breaking it into a separate block — appears purely typographic, no structural role.

### Custom code

None visible in the data pulled (`wf_home_tree_raw.json` has 0 hits for `HtmlEmbed`, `Embed`, `tel:`, `mailto:`). The `Global Styles` component is the most likely carrier for any site-wide `<style>`/script embed, but its internal structure wasn't part of this data pull (see §6) — its presence as a no-props, no-slots marker component in the same page-wrapper slot on every page is itself the pattern worth flagging, not proof of what it contains.

---

## 3. Figma → Webflow mapping observed

| Figma | Webflow | notes |
|---|---|---|
| `Home` frame (1728 wide) | `Home` page | 1:1 section order, see §1 table |
| `Hero` / `Intro` / `Case` / `So funktioniert es` (named frames) | `.section-hero` / `.section-step` / `.section-strategy` / `.section-works` | direct name carry-over (partial — 4 of 10) |
| `Frame 91/92/93/111/112/113`, `Group 1577705883` (generic Figma auto-names) | `.section-partners` / `.section-reviews` / `.section-situation` / `.section-location` / CTA / Footer | **the builder invented every semantic name**; Figma provided none for 6 of 10 sections |
| `Sub Page: Ihre Situation` (1 shared template, 2 near-duplicate frames + interaction-state frames) | 3 live pages: `Erbe`, `Scheidung`, `Senioren` | one design template → three built pages, differentiated via `is-erbe`/`is-divorce`/`is-senior*` combo classes (e.g. `.section-hero.is-erbe-hero`, `.section-hero.is-divorce`, `.wrapper-safety.is-senior`, `.works-lower-wrap.is-erbe`) |
| `Notverkauf` text layers on the same template | **nothing** | designed as a 4th scenario, linked to from the homepage situation grid ("Zum 24-h-Plan"), never shipped as a page — see §4 |
| `IMMOBILIENBEWERTUNG Subapge` (1920 wide, wizard steps "Q1", progress "1/8") | `Immobilienbewertung` page | direct 1:1 |
| `Über Mich & Partnernetzwerk`, `Referenzen V1`/`V2` ×3 | **nothing built** | ~60% of top-level canvas content, unshipped |
| Figma colour swatches (not captured as Figma Variables in this export) | 4 Webflow variables: `Cream ` #e9e2d2, `Dark Blue` #122a3f, `Brown` #af9770, `White` hsla(41.7,0,100,1) | can't confirm 1:1 fidelity to Figma source — no Figma variable defs on disk for this file (§6) |
| No tablet/mobile frames anywhere in Figma | tablet/mobile combo classes (`heading-styles-h2.tablet`, hamburger menu, `.nav-links.desktop`) exist in Webflow | responsive layout is a 100% build-side invention, not a handoff |

### px ÷ 16 = em check

This site names classes after literal px values, which only reads as a coherent "1em = 16px" token system if those values divide cleanly. They mostly don't:

| class | px | ÷16 (em) | clean? |
|---|---|---|---|
| `text-size-16px` | 16 | 1.0 | yes |
| `gap-16px` | 16 | 1.0 | yes |
| `gap-24px` | 24 | 1.5 | yes |
| `gap-48px` | 48 | 3.0 | yes |
| `icon-24px` | 24 | 1.5 | yes |
| `text-size-18px` | 18 | 1.125 | borderline |
| `text-size-14px` | 14 | 0.875 | borderline |
| `icon-43px` | 43 | 2.6875 | no |
| `icon-49px` | 49 | 3.0625 | no |
| `icon-53px` | 53 | 3.3125 | no |
| `gap-5px` | 5 | 0.3125 | no |
| `gap-10px` | 10 | 0.625 | no |
| `max-wd-847px` | 847 | 52.9375 | no |
| `max-wd-1092` | 1092 | 68.25 | no |

**Verdict**: roughly half the values land on an ugly em fraction, which means these class names are **literal Figma pixel measurements copy-pasted into the name**, not the product of a deliberate 16px-root em scale (a true token scale would show clean halves/quarters throughout, the way `gap-16/24/48px` happen to). The clean ones (16, 24, 48) are clean by coincidence of being common round numbers, not because the whole set was designed on a grid.

---

## 4. Deliberate rules vs. mistakes

### Rules that read as deliberate
1. **Literal-px class naming** for every size category (type, gap, icon, max-width) — makes the intended value legible straight from the class name without opening the Designer style panel. A genuine (if unusual) convenience trade-off, not an accident, given it's applied consistently across 40+ classes.
2. **`Section > Block.main-container > Block.wrapper-<topic>` shell**, repeated for 8 of 9 homepage sections — a real, consistent structural pattern.
3. **Colour is the only tokenised property** (4 Variables); everything else is baked into classes. Consistent, if minimal.
4. **One shared "Ihre Situation" template** driving 3 near-identical pages via `is-erbe`/`is-divorce`/`is-senior*` combo classes — sensible reuse for near-identical page shells (Erbe / Scheidung / Senioren).
5. **CTA component's `Upper Link Visibility` prop** genuinely used to hide the secondary CTA on the homepage while keeping its text/link fields populated underneath — correct use of a visibility prop, not dead config.
6. **Icon pattern**: every icon is a 2-layer `icon-relative` + `icon-absolute` `Image` pair inside a sized wrapper (`icon-24px.flex`, `icon-44px.is-center`, `stars-90px`) — applied consistently for hero button icons, card arrows, and the Google review badge.
7. **A manual `"Updated 02/2/2026"` timestamp label** left on the Figma canvas — informal but deliberate version-tracking habit.

### Mistakes / inconsistencies
- **`Cream ` variable name has a trailing space baked in** (`extra_from_main.md`, verbatim) — the same contamination pattern shows up independently in the class names (`text-size-18px`, `icon-18px`, `max-wd-647`, all with a trailing **non-breaking space**, not even a regular space). This happened in at least two unrelated places (a Variable and 3 classes), suggesting a systemic copy-paste habit rather than a one-off typo.
- **`.section-location` breaks the shell pattern**: every other section nests its content inside `Block.main-container`; `.section-location`'s `Block.location-wrap` (holding the 3 location cards: Baden-Baden, Karlsruhe, Weitere Orte) sits as a **sibling** of `Block.main-container`, not nested inside it — meaning this one section isn't width-constrained the same way as the rest of the page.
- **Every Link element on the homepage has no href set** (14 of 14 checked programmatically return `settings.link == null`); the only 2 explicit links anywhere on the page are CTA-component placeholders pointing at `"#"`. Matches the "never published" status but means literally no navigation works if this were shipped as-is.
- **The homepage situation grid links to a "Notverkauf" (emergency sale) card** ("Zum 24-h-Plan") that was designed on the shared Figma template but **was never built as a page** — only Erbe/Scheidung/Senioren exist among the 6 live pages. A dangling 4th scenario.
- **Inconsistent `px` suffixing on `max-wd-*` classes**: `max-wd-524px`, `max-wd-532px`, `max-wd-847px` carry the suffix; `max-wd-580`, `max-wd-600`, `max-wd-728`, `max-wd-850`, `max-wd-1092`, `max-wd-1358` don't, for what's presumably the same kind of value.
- **`icon-80`** is missing its `px` suffix entirely (every other icon class has one).
- **`divier-grey` / `divier-blue`** — "divider" misspelled as "divier", consistently, in 2 classes.
- **`dropdown-link Copy`** — a literal, unrenamed Webflow auto-duplicate class (the " Copy" suffix Webflow appends when a class is duplicated without renaming) shipped into the live class list.
- **`Notverkauf` spelled 3 different ways** on the Figma canvas for what should be one label: `Notverkauf`, `Notverkauf – 24-h-Plan`, `Notverkauf – 24h–Plan` (hyphen vs. en-dash inconsistency).
- **~60% of the top-level Figma canvas never got built**: `Über Mich & Partnernetzwerk` (13249px tall — a substantial page design) and 3 separate `Referenzen`/testimonials explorations exist in the file but have no corresponding live page among the 6 shipped.
- **Two different desktop canvas widths in one file**: the homepage master is 1728px; every subpage master (`IMMOBILIENBEWERTUNG`, `Über Mich`, `Referenzen`, `Ihre Situation`) is 1920px. Not fatal, but an inconsistency a design-system audit would flag.
- **No tablet/mobile Figma frames anywhere** — the entire responsive layer of this site (hamburger nav, `.tablet`/`.desktop` heading swaps) was designed live inside Webflow with no design reference, which is a process gap more than a build defect, but it means there is nothing to QA a mobile build against.
- **No SEO on any page**: 6/6 pages have a title identical to the page name and no meta description or Open Graph tags (`extra_from_main.md`).
- **The `Über HausVertrauen` h2 is duplicated verbatim** (once padded with ~35 leading spaces under `.desktop`, once bare) as the responsive-swap technique instead of one fluid heading — two independently-editable copies of the same sentence that can drift out of sync on future edits.

---

## 5. Verbatim dumps

Omitted from the skill copy.

## 6. Data caveats

- **No computed CSS values exist on disk for this site.** `wf_styles_raw.json` (`action: "get_styles"`) returns only `{id, name, isFromLibrary, isComboClass, type, selector}` for all 569 styles — confirmed programmatically (`0` entries contain a `properties`/`breakpoints` key). This report therefore **cannot quote a single literal font-size, line-height, colour hex, or breakpoint override actually applied in the CSS** the way it can for class *names*. Every size claim in §2/§3 is inferred from the class name itself (e.g. "`text-size-22px` implies a 22px font-size") or from the raw px math in §3 — not confirmed against a rendered stylesheet. Where the format modeled on `furec.md` shows exact `em`/`px` CSS values pulled from a fuller styles API response, this report cannot reproduce that for HausVertrauen because the underlying data was never captured that way.
- **`wf_home_tree_raw.json` matches `wf_home_tree.txt` exactly** — it is the same `get_all_elements` response, just unrendered. No additional per-node styling, interaction, or custom-code data was found in it beyond what the rendered tree already shows (checked directly: 0 `HtmlEmbed`/`Embed` nodes, and every `Link` node's `settings` was walked programmatically).
- **Figma design tokens/variables are not available for this file.** `extra_from_main.md`'s 4-variable list is *Webflow* Variables data, not a Figma `get_variable_defs` pull; there is no on-disk record of the Figma-side colour/spacing/type tokens, so §3's "Figma colour swatches → Webflow variables" row can't be checked for 1:1 hex fidelity, only asserted as plausible.
- **`figma_page1_metadata.xml` is metadata-only** (frame/text/vector node names, positions, sizes — the same `get_metadata` shape as the sibling reports use for "anatomy"), with no fills, effects, or type-style data attached to individual nodes; typography in §1 is inferred only from frame content and section §2's Webflow class names, not from Figma text styles directly. The file also required stripping a trailing tool-instruction string (`"IMPORTANT: After you call this tool…"`) appended after `</canvas>` before it would parse as valid XML — a quirk of how this metadata was captured, not of the design file itself.
- **Navbar, Footer, Global Styles and CTA component internals are opaque.** The homepage element tree only shows `ComponentInstance` nodes with their exposed props (`props: []` for Navbar/Footer/Global Styles, the 7 CTA props listed in §2) — none of their internal element trees were pulled, so navbar link targets, footer column contents, and whatever `Global Styles` actually injects are all unverified beyond the class names that happen to appear in the global style list.
- **Only the homepage's element tree was pulled** (`wf_home_tree.txt`/`_raw.json` cover `Home` only). Claims about `Immobilienbewertung`, `Verkaufen`, `Erbe`, `Scheidung`, `Senioren` page structure in this report are inferred from (a) their Figma frame content (§1) and (b) the page-scoped combo classes that appear in `styles_combo.txt` (`is-erbe`, `is-divorce`, `is-senior*`, `is-control`, `is-method`) — not from a direct element-tree pull of those pages.
- **No live/rendered site was checked.** The site was never published (`extra_from_main.md`), so there is no staging URL to cross-reference against; all findings come from the Webflow Data API captures and the Figma metadata export, both already on disk before this pass started.
