# Reading a Figma design for a Webflow build

Goal: leave this phase with a **build sheet** (tokens, page list, section list per page, component list, asset list) before touching Webflow. Everything below is read-only.

## 0. Load the tools once

```
ToolSearch query="select:mcp__Figma__get_metadata,mcp__Figma__get_design_context,mcp__Figma__get_screenshot,mcp__Figma__get_variable_defs,mcp__Figma__use_figma,mcp__Figma__get_figma_skill"
```

Read `skill://figma/figma-design-to-code/SKILL.md` with `get_figma_skill` before the first `get_design_context` call, and pass `skillNames="resource:figma-design-to-code"` on every `get_design_context` call. Read `skill://figma/figma-use/SKILL.md` before any `use_figma` call and pass `skillNames="resource:figma-use"`.

File key comes from the URL: `figma.com/design/<fileKey>/<name>?node-id=12-34` gives `fileKey` and node id `12:34` (hyphen becomes colon).

## 1. Map the file (2 calls)

1. `get_metadata(fileKey)` with no nodeId lists the pages. Client files from this team usually have a **design page** (designer's working file, often several versions) and a **dev page** or a file whose name ends in `(dev)` that is the approved build source. Build only from the dev source. If a Slack or Notion handoff names a node id, that node is the source.
2. `get_metadata(fileKey, nodeId=<page id>)` returns the XML tree of that page: top-level frames with names, sizes and positions. Identify:
   - page frames and their breakpoint: width 1440 or 1920 is desktop, 768 to 1024 tablet, 375 to 430 mobile. Names usually carry it ("Home", "Home - Tablet", "Home Mobile").
   - one frame per Webflow page. Group frames by page name.
   - a "Components", "Styleguide", "Design System" or "Assets" frame if present.

Keep the page tree short in context: note frame name, id, width, height only.

## 2. Screenshot every page frame you will build

`get_screenshot(fileKey, nodeId, maxDimension=1600)` for each desktop frame, and the mobile frame if it exists. Download with the curl command it returns and open the PNG with Read. The screenshot is the visual target for the whole build and the fastest way to list sections top to bottom.

## 3. Tokens

- `get_variable_defs(fileKey, nodeId=<desktop home frame>)` returns the variables actually used on that frame. Many designer files have **no variables**; then colours and type come from styles.
- For styles, run one read-only `use_figma`:

```js
const paints = await figma.getLocalPaintStylesAsync();
const texts = await figma.getLocalTextStylesAsync();
const effects = await figma.getLocalEffectStylesAsync();
const cols = await figma.variables.getLocalVariableCollectionsAsync();
const toHex = c => '#' + [c.r,c.g,c.b].map(v => Math.round(v*255).toString(16).padStart(2,'0')).join('');
return {
  paints: paints.map(p => ({ name: p.name, value: p.paints[0]?.type === 'SOLID' ? toHex(p.paints[0].color) : p.paints[0]?.type })),
  texts: texts.map(t => ({ name: t.name, family: t.fontName.family, style: t.fontName.style, size: t.fontSize, lh: t.lineHeight, ls: t.letterSpacing })),
  effects: effects.map(e => e.name),
  collections: cols.map(c => ({ name: c.name, modes: c.modes.map(m => m.name), count: c.variableIds.length }))
};
```

- If there are neither variables nor styles (common with junior designers), derive the palette from `get_design_context` output of the hero, a card section and the footer: collect every hex and every font-size/weight pair, then cluster them into a scale (see SKILL.md section on tokens).

## 4. Sections

For each page frame, walk the direct children of the frame in the metadata XML. Those are the sections. For each section record: name, height, whether the background differs from the page, and the count of repeated children (cards, logos, steps). Repeated children of the same size are a grid or a slider.

Then call `get_design_context(fileKey, nodeId=<section id>)` for the sections you need exact values for. Start with nav, hero, one card grid, one CTA, footer. The returned code is React and Tailwind-ish; use it as a measurement sheet, not as output:

- padding and gap values on wrappers give section padding and grid gaps
- `max-w-[1280px]` style values give the container width
- text nodes give font family, size, weight, line height, letter spacing, colour
- `rounded-*`, `border`, `shadow` give card styling
- image and SVG nodes come with download URLs in the response; download them then, the URLs expire

If the response is flagged sparse, request the child node ids it lists in one parallel batch.

Budget: 8 to 15 `get_design_context` calls per page is normal. Do not call it on every element; repeated cards share one call.

## 5. Components in the design

Run once per page you care about (one `use_figma` per page, in parallel, each switching page once):

```js
const page = await figma.getNodeByIdAsync('<page id>');
await figma.setCurrentPageAsync(page);
const sets = page.findAllWithCriteria({ types: ['COMPONENT_SET','COMPONENT'] })
  .filter(n => !(n.type === 'COMPONENT' && n.parent && n.parent.type === 'COMPONENT_SET'));
const instances = page.findAllWithCriteria({ types: ['INSTANCE'] });
const counts = {};
for (const i of instances) { const mc = await i.getMainComponentAsync(); const k = mc ? (mc.parent?.type==='COMPONENT_SET' ? mc.parent.name : mc.name) : '?'; counts[k] = (counts[k]||0)+1; }
return { components: sets.map(s => ({ name: s.name, id: s.id, variants: s.type==='COMPONENT_SET' ? s.children.length : 1 })), instanceCounts: counts };
```

Anything instanced 3 or more times across pages (nav, footer, button, card, CTA band) becomes a Webflow component or a shared class. Anything used once is plain elements.

## 6. Assets

From the `get_design_context` responses, collect every image and SVG download URL with the node name. Download to a local `assets/` folder immediately (`curl -L -o`). Name files `<page>-<section>-<what>.<ext>` in lowercase with hyphens. Export photos as JPG or WebP, logos and icons as SVG. Note which images are decorative (alt "") and which need alt text.

## 7. Build sheet format

Write `build-sheet.md` in the working folder before building:

```
# <Site> build sheet
Source: <figma url> page "<page name>", frames: Home 1440 (id), Home Mobile 390 (id), About 1440 (id) ...
Container: 1280px (design frame 1440, side padding 80)
Fonts: Inter 400/500/600/700 (Google), display: Fraunces 600
Colours: brand-primary #1B3A6B, brand-secondary #F4B400, neutral-900 #111111, neutral-600 #5A5A5A, neutral-100 #F5F5F5, white #FFFFFF
Type scale (desktop / mobile): h1 64/40 lh1.1 700, h2 48/32 lh1.15 700, h3 32/24, h4 24/20, body-lg 20/18, body 18/16, body-sm 16/14, eyebrow 14 uppercase 600
Spacing scale: section 120/80/64, section-small 80/64/48, gap-lg 48, gap 32, gap-sm 16, radius 16/8
Pages: home (/), about (/about), services (/services) + CMS: Services (name, slug, icon, summary, body)
Components: Navbar, Footer, Button (primary, secondary, text), Card Service, CTA Band
Home sections (top to bottom):
1. hero        | bg brand-primary | h1 + p + 2 buttons + image right
2. logos       | 6 logos in a row, grey
3. services    | h2 + 3 Card Service (CMS)
...
Assets: assets/home-hero.jpg (1200x900), assets/logo.svg, assets/icon-*.svg (6)
Open questions: tablet frames missing; hover states not designed for cards
```
