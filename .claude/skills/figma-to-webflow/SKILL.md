---
name: figma-to-webflow
description: Convert a Figma design into a built Webflow site the way Xerosol's team does it, using the Figma and Webflow MCP connectors end to end. Use this skill whenever a user gives a Figma link and wants pages, sections or a whole site built in Webflow, asks to "develop", "build", "implement", "convert" or "code" a design in Webflow, wants a Figma handoff turned into a webflow.io staging site, wants a Webflow build checked against its Figma, or wants an existing Webflow site extended with new sections from a design. Also use it for Webflow template builds from Figma and for QA passes before a client review, even if the user does not say "Figma to Webflow" explicitly.
---

# Figma to Webflow

You are building a Webflow site from a Figma design, the way this team builds: fluid em sizing from a 1920 frame, a fixed section shell, Client-First-flavoured class names, buttons and eyebrows as components, and a review round that will send back anything that drifts from the design. The reviewer's notes from thirteen past builds are in `references/qa-checklist.md`; the team's actual conventions, measured from those sites, are in `references/conventions.md`. Read both before you build anything. They are the standard you are held to.

The work has five phases. Do them in order, and report at the end of each one with the staging link, because the reviewer wants two or three sections early, not the whole site late.

## 0. Set up

1. Load tools once: `ToolSearch` with `select:mcp__Figma__get_metadata,mcp__Figma__get_design_context,mcp__Figma__get_screenshot,mcp__Figma__get_variable_defs,mcp__Figma__use_figma,mcp__Figma__get_figma_skill,mcp__Webflow__data_sites_tool,mcp__Webflow__data_pages_tool,mcp__Webflow__data_style_tool,mcp__Webflow__data_variable_tool,mcp__Webflow__data_element_tool,mcp__Webflow__data_element_builder,mcp__Webflow__data_whtml_builder,mcp__Webflow__data_component_tool,mcp__Webflow__data_component_builder,mcp__Webflow__data_component_props_tool,mcp__Webflow__data_assets_tool,mcp__Webflow__asset_tool,mcp__Webflow__data_scripts_tool,mcp__Webflow__data_interactions_tool,mcp__Webflow__data_cms_tool,mcp__Webflow__element_snapshot_tool`.
2. Every Webflow call needs `agent_id` (make one: `<model>|claude-code|<5 random chars>`, reuse it), `session_id` (`start` on the first call, then the `ses_…` value the first result issues) and `context`. `data_style_tool`, `data_variable_tool`, `data_element_tool`, `data_element_settings_tool`, `data_component_tool`, `data_whtml_builder` and `data_interactions_tool` take `siteId` **and** `pageId` at the top level even for site-wide reads; `data_pages_tool`, `data_cms_tool`, `data_scripts_tool`, `data_fonts_tool`, `data_assets_tool` take `site_id` inside the action instead. Getting this wrong is the most common validation error.
3. Confirm the target: which Webflow site (`data_sites_tool > list_sites` if not given), which Figma file and node, which pages are in scope, which language the copy is in. If the Figma has a design page and a dev page, or a Draft tab, build only from the dev source. If pages disagree on a shared element, the homepage version wins. If a page has two hero variants and no decision, build the rest and ask.
4. Make a working folder for the project (`<site>-build/`) with `assets/` and a `build-sheet.md`.
5. The Webflow API rate-limits at roughly 60 calls a minute per token. Batch actions (up to 5 per call), and when you get `Too Many Requests` wait a minute and retry the single failed action, not the batch.

## 1. Read the design

Follow `references/figma-extraction.md`. The output is the build sheet: container width, fonts, colour tokens, type scale, spacing scale, page list, section list per page with what each section contains, component list, asset list, open questions. Two things matter most:

- **Measure at the frame width.** On a 1920 frame every value converts by ÷16 to em; on a 1440 frame by ÷15 with the root set to `1.1111vw`. Write the divisor on the build sheet and use it for every number.
- **Decide the type mapping once.** Designers' "H1" is often the section heading while the hero uses a larger display size. Map Figma styles to `heading-style-h1..h6` by size and record it, then never guess again per section.

Get one screenshot per page frame (`get_screenshot` with `enableBase64Response: true` and `maxDimension` 900 if the sandbox cannot download URLs) and keep it open while building. Section metadata alone does not tell you what a section looks like.

## 2. Prepare the site

Do these before any element exists, in this order, because styles must exist before elements reference them and variables before styles reference them.

1. **Variables.** `data_variable_tool > create_variable_collection` named `Colors`, then one `create_color_variable` per token on the build sheet. Use the Figma semantic names when the file has them.
2. **Global styles in the site head.** `data_scripts_tool > set_site_freeform_code` location `head` with the fluid-root block from `references/conventions.md` section 1, plus font links (Google Fonts) or `data_fonts_tool > create_font` for uploaded families, plus `<meta name="theme-color">` matching the hero background.
3. **Tag styles.** `data_style_tool > update_style` on `body` (font-family, colour, background), `h1`–`h6` (`margin-top: 0; margin-bottom: 0; font-weight: inherit`), `p` (`margin-bottom: 0`), `a` (`color: inherit; text-decoration: none`). Reviewers see default blue links in Safari otherwise.
4. **Pages.** `data_pages_tool > create_page` for every page on the build sheet with title, slug in the site language, SEO title and description. Note each page id. Put utility pages in a folder.
5. **The system classes**, created through one `data_whtml_builder` call on the home page that inserts a throwaway `<div>` carrying every global class with its CSS, then removed with `remove_element`. This registers `page-wrapper`, `main-wrapper`, `padding-global`, `padding-vertical` and its size combos, `container-large`, `section-title-wrapper`, `heading-style-h1..h6`, `text-size-*`, `text-color-*`, `eyebrow-text`, `button-group`, `button-primary`, `button-secondary`, `button-text`, `grid`, `icon_*px`, `image`, `is-cover` with their breakpoint overrides in a single round trip. The styles survive the element's removal. Write the CSS with longhand properties, `em` values, and only the four Webflow media queries (none for desktop, `max-width: 991px`, `767px`, `479px`). Colour values reference the variables you created (`var(--brand-primary)`); if the builder rejects `var()`, write the hex and re-point with `update_style` + `variable_as_value` afterwards.

## 3. Build components, then pages

**Components first** (`data_component_tool > create_blank_component`, then `data_whtml_builder` with `scope_component_id` to fill it, then `data_component_props_tool > create_prop` and `data_element_settings_tool > set_settings` to bind props): `Navbar`, `Footer`, `Button Primary`, `Button Secondary`, `Section Eyebrow`, `CTA`. Props on buttons are always `Text` (textContent) and `Link` (link); eyebrow has `Text`; CTA has `Title`, `Text`, `Image`. The navbar uses Webflow's native Navbar and Dropdown elements: build it with `data_element_builder` (`type: "NavbarWrapper"` and children) rather than plain divs, so the mobile menu works without custom code.

**Then each page, section by section.** For every section on the build sheet:

1. Write the section as one HTML fragment with the shell from `references/conventions.md` section 2 (`section.section_<topic> > .padding-global > .padding-vertical.is_<topic> > .container-large > .wrapper_<topic>`), real copy from Figma in the site language, headings with the mapped `heading-style-*` class and the right tag, paragraphs with `text-size-*`, icons as inline `<svg>` inside `.icon_<N>px`, images as `<img>` placeholders with the final alt text.
2. Write its CSS: only the new classes this section introduces (`wrapper_<topic>`, `<topic>_card`, `grid.is_<topic>`, `padding-vertical.is_<topic>`), with tablet and mobile overrides in the same string.
3. Insert with `data_whtml_builder` appended to `main.main-wrapper` (get its id once with `query_elements`, `element_filter.style: "main-wrapper"`). Insert component instances (buttons, eyebrow, CTA) with `data_component_builder > insert_in_element` afterwards, replacing the placeholder button markup, or leave plain buttons and swap them when the section is approved.
4. Upload the section's images with `asset_tool > upload_image_by_url` (public URL) or `data_assets_tool > create_asset` + the S3 POST, then `set_image_asset` on each image element. WebP, under 400KB for heroes and 200KB for cards, alt text set.
5. Snapshot the section (`element_snapshot_tool`) and compare with the Figma screenshot. Fix spacing and type before moving on; the reviewer measures both.

After the first two or three homepage sections, publish to staging (`data_sites_tool > publish_site` with `publishToWebflowSubdomain: true`) and report the link. Then continue.

**Order across the site:** homepage desktop, all other pages desktop, then the responsive pass over every page, then animations, then the QA round. Say which stage each page is in when you report links.

## 4. Responsive and motion

- Responsive is not optional even though the fluid root does most of the desktop work. Below 991px the root snaps to 16px, so every typography class, every grid and every `padding-vertical` combo needs its tablet, landscape and portrait values. Designers usually provide only a homepage mobile frame; derive the rest from it. Check every page at 991, 767 and 479 with `element_snapshot_tool` or a published-site screenshot.
- Motion follows `references/qa-checklist.md` section 2: one reveal per section group, cards in a row together, early trigger, no navbar starting at opacity 0, smooth dark-to-light transitions, no zoom on hover. Build reveals with `data_interactions_tool` (call its `guide` action first; opacity lives under `wf:transform`, values are arrays not `{from,to}`), targeting classes so every instance animates. Add Lenis in the site footer only if the design brief or reference site uses smooth scroll.
- Sliders: Swiper from the page footer code with the `swiper*` classes on the markup.

## 5. QA and handover

Run the checklist in `references/qa-checklist.md` top to bottom on the published staging site. The items that come back most often: navbar full width and dropdowns centred, animation count and order, text sizes and spacing versus Figma, image weight and mobile presence, wrong icons, elements nested inside the hero, min font size 14px, alt text and meta descriptions, template leftovers, link targets on mobile menu buttons.

Then close the way the team closes: publish, open the live staging URL yourself, check what you changed, and report one line per page with its state and what you verified. Anything unresolved (missing assets, undecided hero, unclear link target) goes in the report as a question, not as a guess built into the site.

## Reference files

- `references/figma-extraction.md`: reading the Figma file, tool call shapes, the build sheet format.
- `references/conventions.md`: the team's class system, section shell, type scale, components, navbar, images, motion, CMS, with measured values.
- `references/qa-checklist.md`: what the reviewer sends back, quoted from real review rounds. Read before building and again before handover.
- `references/webflow-mcp-guide.md`: condensed Webflow MCP tool guide, parameter shapes, ordering rules, destructive actions to confirm first.
- `references/site-analyses/`: the full forensic reports on the thirteen reference builds, for when you need a real example of a navbar, a footer, a CMS layout or a scroll animation.
