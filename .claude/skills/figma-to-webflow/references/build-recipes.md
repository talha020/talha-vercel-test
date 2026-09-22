# Build recipes: what the Webflow MCP actually does (verified on a blank site, Sep 2026)

## The HTML builder is the fast path

`data_whtml_builder` takes one HTML fragment plus raw CSS and creates elements **and** styles in one call. Verified behaviour on a blank site:

| you write | Webflow creates |
|---|---|
| `<section class="section_hero">` | `Section` element, tag `section` |
| `<main class="main-wrapper">` / `<div>` | `Block`, tag `main` / `div` |
| `<h1 class="heading-style-h1">` | `Heading` with `headingLevel: 1` |
| `<p class="text-size-large text-color-secondary">` | `Paragraph` with two styles (base + combo) |
| `<a href="#" class="button-primary">` | `Link` with `href` attribute |
| `<div class="padding-vertical is_hero">` | `Block` with styles `["padding-vertical","is_hero"]`, and `.padding-vertical.is_hero` registered as a **combo class** |
| `<svg>…<path/></svg>` | `DOM` elements (tag `svg`, child `path`) with attributes preserved; visible on the canvas |
| text | `String` child nodes |
| `.x { padding: 1em 1.5em; border-radius: 6.25em }` | longhand `padding-top/right/bottom/left`, four `border-*-radius` properties |
| `.x:hover { … }` | the `hover` pseudo on the base breakpoint |
| `@media (max-width: 991px) { … }` | `medium` breakpoint properties; `767px` → `small`; `479px` → `tiny` |
| `.grid.is_hero { grid-template-columns: 1fr 1fr 1fr }` | combo with `grid-template-columns` and an added `grid-template-rows: auto` |

Rules the builder enforces: one root element per action, no `<style>` tags in the HTML, no `@keyframes`, only those three media queries, at most 5 actions per call. A class that already exists is reused and its CSS in the string updates it, so keep each section's CSS to the classes that section introduces.

Styles survive when the element is removed (`data_element_tool > remove_element`), so a throwaway `<div>` carrying every system class is a valid way to register the whole class system in one call.

Colours: write hex or rgba in the builder CSS, then point the property at a variable with `data_style_tool > update_style` and `variable_as_value: "<variable id>"`. Verified: the property then stores `{"id": "variable-…"}` and follows the variable. Variable ids come back from `create_color_variable` (`id`, `cssName` like `--_colors---brand-primary`).

Inline SVG becomes DOM elements rather than the team's usual `HtmlEmbed`. Both render; DOM elements are editable per node in the Designer. If a project insists on embeds, create the icon with `data_element_builder` type `HtmlEmbed` and set its code through `data_element_settings_tool > set_settings` key `code`.

## Verified on a real build (Furec hero + section on a blank site)

- `update_style` on `body`, `h1`, `p`, `a` fails with "Style not found" on a fresh site: tag styles only exist once the Designer has touched them. Put the body font, colour, background and the heading, paragraph and link resets in the site head CSS instead (see SKILL.md phase 2).
- `element_snapshot_tool` needs an open Designer session; headless it returns `status: false`. Verify through the published staging URL (ask the user to open it when the sandbox cannot fetch `*.webflow.io`) or by reading back the tree and styles.
- A 30-element section with 60 CSS rules and three media queries inserts in one `data_whtml_builder` call in a few seconds. Two sections per call is fine; keep the HTML under roughly 12KB per action.
- `get_design_context` on a whole Figma section can return 70KB and spill to a file. Parse the saved JSON with python: text nodes are `<p className="…" data-node-id="…">text</p>`, layout frames are `<div className="…" data-node-id="…" data-name="…">`; Tailwind tokens carry the values (`gap-[var(--space\/6,24px)]`, `text-[length:var(--font-size\/display,69px)]`, `leading-[1.05]`, `tracking-[-2.07px]`).
- `get_metadata` on a page frame (not the page) is 120KB and lists the section frames as direct children with `y` and `height`, enough to map sections without reading the page.
- `data_pages_tool > update_page_settings` with `seo` and `openGraph {titleCopied, descriptionCopied}` sets the page meta in one call; `publish_site` with `publishToWebflowSubdomain: true` returns `publishScope: "site"` immediately.

## Verified on two full homepages (Furec, Wangku; Sep 2026)

- **Define each global class once.** If a later `data_whtml_builder` call carries CSS for a class that already exists with different values, the builder forks it to `name-1` and applies the fork to the new elements. Recovery is `remove_style` (old) + `rename_style` (`name-1` → `name`) per class, then re-link any variables. Register the system classes in the first call and never repeat their CSS; later calls carry only the classes they introduce.
- **No descendant selectors.** `.faq-item.is-open .faq-chevron { … }` fails the whole action ("Nested selectors are not allowed"). Put the state class on the element that changes (`.faq-chevron.is-open`) and toggle it from the page script.
- **Second design system on the same site:** when the site already carries one client's classes (a practice site, a template), prefix the new system (`wk-h2`, `wk-padding-global`) rather than reusing `heading-style-h2` with different values, and put its fluid root and fonts in the page head code (`set_page_freeform_code`, location `head`), which loads after the site head and overrides it.
- A Google Font named in builder CSS (`font-family: Playfair Display, serif`) returns `partial_success` with a `missing_font` warning, but the property is stored; the page renders it once the Google Fonts `<link>` is in the head. `data_fonts_tool` is for uploaded files only.
- `asset_tool > upload_image_by_url` is a Designer tool: headless it fails with "Unable to connect to Webflow Designer" and returns a link the user must open in the Designer. `data_assets_tool > create_asset` needs the file bytes, which a sandbox that blocks Figma asset URLs cannot get. Build with `wk-placeholder` gradient blocks, keep the Figma asset URLs in the build sheet (they expire after 7 days), and swap them in when a Designer session is open.
- **Local render check when staging is blocked.** Assemble the builder inputs into one HTML file (page head CSS + every action's CSS in a `<style>` + the fragments in order) and screenshot it with the pre-installed Chromium: `chrome --headless=new --no-sandbox --hide-scrollbars --window-size=1440,7600 --screenshot=out.png file://…`. Desktop Chrome will not go narrower than about 500px, so for phone widths wrap the page in an `<iframe style="width:390px">` inside a 600px window. Chrome cannot fetch Google Fonts through the proxy, so headings render in the fallback serif; layout and spacing are still valid. Find overflow with a `--dump-dom` script that lists elements whose `getBoundingClientRect().right` exceeds the viewport.
- `update_style` with `breakpoint_id` (`medium` 991, `small` 767, `tiny` 479) on a combo (`style_name: "is-center", parent_style_names: ["wk-hero-card"]`) is the way to patch responsive values after the build without re-sending the builder CSS.

## Element ids

Every element id is a two-field object `{component, element}`. Copy it verbatim from the tool result. The page's Body is the root returned by `get_all_elements` with `depth: 0`; a component definition's root comes from `get_all_elements` with `scope_component_id`.

Find the insertion point on a page without dumping the tree: `data_element_tool > query_elements` with `element_filter: {style: "main-wrapper"}`.

## Reading sites (for QA or extension work)

- `get_all_elements` with `depth: -1` on a real page is 100 to 200KB; the harness saves it to a file. Parse it with python and skip `String` nodes.
- `data_style_tool > get_styles {query:"all"}` lists every class (names, ids, combo flag); do not add `include_properties` on large sites. Read specific classes with `query_styles`, and pass the full combo chain (`name_path: ["padding-vertical","is_hero"]`) to read a combo.
- Colour values come back as `{"id":"variable-…"}`; resolve them against `data_variable_tool > get_variables`.
- `data_component_tool > get_all_components` with `options: {includeProps: true, includeInstanceCount: true, includeVariants: true}` tells you what was componentised in one call.
- `data_element_settings_tool > get_settings` with `type: "all_resolved_settings"` reads an HtmlEmbed's code (key `code`).
- `data_fonts_tool > list_fonts` lists uploaded fonts only; Google Fonts show up in the `body` tag style's `font-family`.
- `data_interactions_tool > list_interactions` returns IX3 triggers whose targets are style-block ids; cross-reference with the style list.

## Rate limits and environment

- Webflow returns `Too Many Requests` under load; it hits single actions inside a batch. Wait a minute and re-issue the failed action only. Keep batches to two or three data-heavy actions.
- In the cloud sandbox `*.webflow.io` and Figma asset URLs may be blocked (403 from the proxy). Then everything comes through the MCP tools: use `get_screenshot` with `enableBase64Response: true` and `get_design_context` with `excludeScreenshot: true`, and verify pages with `element_snapshot_tool` rather than curl.
- Publishing: `data_sites_tool > publish_site` with `publishToWebflowSubdomain: true` publishes staging; pass `customDomains` only when the user explicitly asks to go live.
