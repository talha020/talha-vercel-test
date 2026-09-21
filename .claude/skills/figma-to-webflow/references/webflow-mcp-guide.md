# Webflow MCP Guide (condensed from webflow_guide_tool, MCP v2.1.0)

Source: Webflow MCP server's own `webflow_guide_tool` usage guide. Call `webflow_guide_tool` once per session before using any other Webflow tool (per the server's own instructions) — this file is a durable condensed copy of that guide for offline reference.

## Designer API vs Data API — the core distinction

> "Data Tools are REST API calls, and Designer Tools are UI tools. You must use the correct tool for the action you want to perform."

- **Data tools** (`data_*` prefixed, e.g. `data_element_tool`, `data_style_tool`, `data_pages_tool`, `data_cms_tool`, etc.) are REST API calls against Webflow's backend. They work regardless of whether a Designer session is open, and are the primary way to read/write site structure, styles, CMS, assets, scripts, etc.
- **`designer_tool`** operates the live Webflow Designer UI (the user's open canvas/session): selecting elements, switching pages/canvases, entering/exiting component view, reading current mode, etc. Many of its actions require an actual open Designer session.
- After creating/updating an element via a data tool, it is **not automatically selected** in the Designer. Use `designer_tool > select_element` with the element ID to select it, or `data_element_tool > query_elements` with an `element_id` lookup to inspect it without touching the UI.

## General rules

- Do not assume `site_id`. If a tool requires `site_id`, pass it explicitly; ask the user if unknown.
- Always plan actions before calling any tool — do not call tools "randomly" without understanding the full workflow.
- When creating/updating elements, **reuse existing styles** if they exist, unless the user explicitly wants new ones.
- Localization: use `data_localization_tool`. Discover locales/ids via `data_sites_tool` (site details → `locales` field). **The primary locale is read-only** — `data_localization_tool` can only write to secondary locales.

## Agent Instructions (site-specific rules/skills)

- Call `data_agent_instructions_tool > search_instructions` early once `site_id` is known, to discover site-specific instructions. Repeat when switching sites.
- Fetch instruction bodies via the search result's `resourceUri` (agent_instruction resource) or `data_agent_instructions_tool > read_instruction`.
- To generate a new instruction skill from site evidence: `generate_instruction`; poll by calling it again with `site_id` + `task_id` until finished/failed, then read the result's `resourceUri`.
- CMS guideline generation requires a `cms-collection` primary context source with the target collection ID.
- Generated instructions are saved as **drafts** — inspect before treating as durable guidance.

## Elements: `data_element_tool` + `data_element_settings_tool` + `designer_tool`

- `data_element_tool` reads/edits a page's element tree; `data_element_settings_tool` reads/writes element settings and data bindings. **Every call to either takes `siteId` and `pageId` at the top level**; all actions in the call run against that page. Discover page ids via `data_pages_tool > list_pages`.
- `designer_tool > select_element` selects; `designer_tool > get_selected_element` reads current selection — both return the element's `id` object and the `pageId` it lives on.
- Most actions accept optional `scope_component_id` to address elements inside a component definition (walks that component's tree instead of the page tree).
- `get_all_elements` — retrieve a page's elements; `depth` limits the tree (0 = root only, N = N levels, -1 = whole tree); `scope_component_id` roots at a component definition.
- `query_elements` — pass `queries[]`, each filtering by `element_id` (exact match, bypasses other filters), `element_filter` (`{type, text, style, tag, attribute_name, attribute_value}`), or `component_filter` (`{component_name, slot_name}`), with optional `scope_element_id`, `return_parent`, `children_depth`, `limit`. Supports multiple queries per call.
- `move_element` — pass `{id, anchor_element_id, creation_position}`. append/prepend = child of anchor; before/after = sibling adjacent to anchor. Element keeps bindings/styles.
- `remove_element` — pass `id`. **DANGEROUS — cannot be undone via the tool.**
- `set_attributes` (replaces old `add_or_update_attribute`) — pass `id` and `attributes[]` of `{name, value}`.
- `get_attributes` — pass `id` (optional `name`/`index` for a single attribute); `with_resolved_bindings: true` evaluates bindings to plain strings.
- `remove_attribute` — pass `id` and `attribute_names[]`.
- `set_style` — pass style names **that already exist**. Replaces the element's *full* style list; multiple styles = combo classes.
- `set_link` — for Button, TextLink, LinkBlock elements.
- `set_text` — pass `id` and `text`, for text-capable elements.
- `set_image_asset` — pass a valid `image_asset_id` (discover via `data_assets_tool`).
- `set_heading_level` — valid levels are integers 1–6 (h1–h6).
- `get_display_name` / `set_display_name` — Navigator label, not rendered markup. `set_display_name` with `''` resets to automatic name. Only valid on elements with the DisplayName capability (String nodes excluded).

### Element settings & bindings

- `get_bindable_sources` — pass `queries[]` with `{label, element_id, setting_key?, value_type?, limit?, scope_component_id?}`. Filter by `setting_key` for an exact setting (e.g. `"altText"`, `"src"`, `"domId"`) or `value_type` for a data shape (e.g. `"string"`, `"imageAsset"`).
- `get_settings` — pass `{type, element_id, queries?}` (one element per action; batch via the tool's `actions[]`). Three modes: `type: "all_raw_settings"` (raw values + binding metadata), `type: "all_resolved_settings"` (evaluated values), `type: "query_settings"` (full `ElementSetting` entries — `valueType`, `canBind`, `value`, `resolvedValue`, `display`; pass `queries[]` of `{label?, key?, value_type?}`, or omit for unfiltered).
- For HtmlEmbed/CodeBlock: `get_settings` reads code content; `set_settings` writes it.
- `set_tag` — pass `{element_id, static_value? | binding?}` (exactly one). Valid enums depend on type: Heading → h1–h6; Block/Section/Container/Article/Aside/Header/Footer/Nav/Main/etc. → div|header|footer|nav|main|section|article|aside|...; List → ul|ol; DOM → any string.
- `set_visibility` — pass `{element_id, static_value? | binding?}` (exactly one). `static_value: true/false`. `binding`: only `{source_type: "prop", prop_id}` (props are the only binding source on the data surface). Only valid on elements with the Visibility capability.
- `set_dom_id` (replaces old `set_id`/`update_id_attribute`) — pass `{element_id, static_value? | binding?}`. `static_value`: id string without leading `#` (normalized: invalid chars stripped, spaces→hyphens, lowercased).
- `set_settings` (write path) — pass `operations[]` of `{label?, element_id, settings[], scope_component_id?}`. Each operation targets one element; its `settings[]` apply atomically in one call. Each setting entry: the `key` plus **exactly one** of `clear: true` (reset to default), `binding` (`{source_type: "prop", prop_id}`), or a `static_*` group (`static_text` / `static_number` / `static_boolean` / `static_link` / `static_video` / `static_json`). Special `"attributes"` key: pass `attributes[]` where each entry has `name` (`name_static`/`name_binding`) and `value` (`value_static`/`value_binding`) — **replaces the element's full attribute list**. Discover keys via `get_settings`, bindable sources via `get_bindable_sources`.
- **Workflow: bind a component prop to an element setting** — (1) `get_bindable_sources` on the element (pass `scope_component_id` if inside a component definition — no canvas navigation needed); (2) choose a compatible prop source (filter by `setting_key` or `value_type`); (3) `set_settings` with a binding entry `{source_type: "prop", prop_id}` on the chosen key, same `scope_component_id`.

## Creating elements

- **To create a new element, use `data_element_builder`.** Pass the `type` of element to create. After creation, use `designer_tool > select_element` to select and inspect it.
- **Important rules for creating elements:**
  > "Always create styles first if you plan to apply them while creating the element. This ensures style references are valid at the time of creation."
  > "Always plan out your actions before calling data_element_builder. Know exactly what type of element to create, what styles or attributes to apply, and how you will use it."
  - Once created, the element is not automatically selected — select it via `designer_tool > select_element` with the returned ID.
  - **Only Container, Section, DivBlock, and some valid DOM elements can have children.**
  - **Only component instances are allowed inside slots.** Never create/insert a regular element into a slot — use `data_component_builder` to create a component instance and place it there.

## Components

### Component Builder Tool (`data_component_builder`)
- To insert a component **instance**, pass `siteId` and `pageId`. Two action types:
  - `insert_in_element` — insert as a child of a parent element (Container, DivBlock, Section, etc.).
  - `insert_in_slot` — insert into a specific slot of an existing component instance; **must** provide `slot_name`.
- Pass the component **name** in `component_schema` (not the ID). Optionally populate child components in `slots[]`.
- Pass `scope_component_id` to insert inside a component definition (parent lookup walks that component's tree).
- After creation, use `designer_tool > select_element`.
- **Only component instances (via `data_component_builder`) are allowed inside slots** — always use `insert_in_slot` for that.

### Component Tool (`data_component_tool`) — discovery & metadata
- `get_all_components` — optional `options` for prop defs, instance counts, variants. `options.includeInstanceCount: true` for per-component instance counts (no standalone "get instance count" action).
- `get_component` — pass `component_id` OR `name` (with optional `group`); same `options` object.
- `query_components` — fuzzy search by keywords, filter by prop characteristics; pass `queries[]` each with `label`, `keywords`, optional `hasProps`/`propsFilter`/`resultOptions`/`limit`.
- `designer_tool > check_if_inside_component_view` — check current context.
- `designer_tool > get_current_component` — returns null if on a regular page.
- `get_parent_component` — pass element ID; returns null if element is at page level.

### Component Tool — create / transform / delete
- `create_blank_component` — pass `name`, optional `group`, `description`.
- `transform_element_to_component` — pass element ID + component name. Optional `replace: false` keeps original element on canvas instead of swapping for the new instance (default `true`).
- `duplicate_component` — pass `source_component_id`, `name`, optional `group`, `description`.
- `set_component_metadata` — pass `component_id` plus any of `name`/`group`/`description`.
- `unregister_component` — pass `component_id`. **DESTRUCTIVE and not reversible — confirm with user first; removes all instances across the site.**

### Component Tool — instances on canvas
- `insert_component_instance` — pass `parent_element_id`, `component_id`, `creation_position` (append/prepend/before/after).
- `designer_tool > open_component_view` (pass `component_instance_id`) / `close_component_view` — enter/exit in-context editing of an instance on the current page.

### Component Variants Tool (`data_component_variants_tool`)
- Variants are per-component style overrides. Base variant id is reserved `'base'`; accepted everywhere a `variant_id` is needed. Base **cannot be deleted** but can be renamed.
- Variant ops don't require canvas context — operate by `component_id`/`variant_id`.
- **Variant reads go through `data_component_tool > get_component`**, not this tool: `options.includeVariants` (list all), `options.includeVariant` (fetch one, `'base'` for base), `options.includeSelectedVariant` (current selection). **There is no `get_variants` action.**
- `create_variant` — pass `component_id`, `name`. New variants are **not auto-selected**.
- `duplicate_variant` — pass `component_id`, `source_variant_id` (`'base'` allowed). Default name: `"<source> Copy"`.
- `delete_variant` — pass `component_id`, `variant_id`. Base cannot be deleted.
- `get_variant_settings` — pass `component_id`, `variant_id` (`'base'` allowed).
- `set_variant_name` — pass `component_id`, `variant_id`, new `name`.
- **Variant styles are scoped by `variant_id` + `style_name`. Style names are global at the site level, so variant style actions do NOT take `component_id`.**
  - `get_variant_styles` — pass `variant_id`, `style_name`; optional `breakpoint_id`, `pseudo`.
  - `set_variant_styles` — pass `variant_id`, `style_name`, then `properties[]` (set/update) and/or `remove_properties[]` (clear). `breakpoint_id`/`pseudo` default to main/noPseudo.
  - Properties set on a variant are **overrides**; unset properties fall back to the base variant's values at render time.
  - Variant style writes don't affect base or other variants. To modify base styles, pass `variant_id: 'base'` to `set_variant_styles` (equivalent to `data_style_tool > update_style` with no variant).
- `reorder_variants` — pass `component_id`, `variant_ids` (ordered list incl. `'base'`). Partial list moves specified variants, preserving relative order of omitted ones.
- **Common workflow:** `get_component` with `options.includeVariants` to discover variant IDs → `create_variant`/`duplicate_variant` → `set_variant_styles` per breakpoint/pseudo as needed.

### Component Props Tool (`data_component_props_tool`)
- `create_prop` — pass `component_id`, `props[]`. Each prop: `{type, name, group?, tooltip?, + ONE matching type group}`. **10 creatable types:** `textContent`, `string`, `richText`, `image`, `link`, `video`, `number`, `boolean`, `id`, `altText`. Type-specific settings live in the matching group:
  - `default_text: {value?, rich_text_inner_text?, multiline?}` — for string/textContent/id/altText/image (asset id via `value`) and richText (via `rich_text_inner_text`).
  - `default_number: {value?, min?, max?, decimals?}`.
  - `default_boolean: {value?, true_label?, false_label?}`.
  - `default_link: {mode?, to?, to_element_id?, open_in_new_tab?, email_subject?, rel?}`. `pageSection` mode → structured `to_element_id: {component, element}`; `page`/`file`/`collectionPage` → id in `to`; `url`/`email`/`phone` → literal in `to`.
  - `default_video: {src?, title?}`.
- `update_prop` — pass `component_id`, `props[]` of `{prop_id, ...partial fields}`. Same groups as create. `clear_default_value: true` clears a default. Pass `null` inside a group to clear individual constraints (e.g. `default_number: {min: null}`). **Prop type and id are immutable — remove and re-create to change them.**
- `remove_prop` — pass `component_id`, `prop_ids[]`. Per-item errors don't abort the rest.
- Discover `prop_id`s via `get_component` (`options.includeProps: true`) or `get_component_instance_props`.
- `get_component_instance_props` — pass `element_id` (a ComponentInstance element), optional `resolved_value`.
- `set_component_instance_prop_values` — pass `element_id`, `values[]` using flat `FlatPropValue` shape (`prop_id` + `type` + matching type-specific fields). Discover `prop_id`s first.
- `reset_all_props_value` — pass `element_id`.
- `unlink_component_instance` (in `data_component_tool`) — pass `component_instance_id`, converts back to regular editable elements. **Cannot be used on library or code components.**

### Slots (Designer)
- To insert a Slot into a component definition: `data_element_builder` with `type: "ComponentSlot"`. Pass `parent_element_id`, `creation_position`, `scope_component_id` = the component id. **Headless does not use `designer_tool > open_canvas` context for this workflow.**

## Canvas Navigation (Designer)
- `designer_tool > open_canvas` — primary way to switch canvas context: pass `component_id` to open a component canvas; pass `page_id` to exit to a page.
- Distinct from `open_component_view`/`close_component_view`: `open_canvas` switches the **entire** canvas context (like clicking a component in the Components panel); the other pair enters/exits **in-context editing** of an instance on the current page.

## Visual verification
- `element_snapshot_tool` — pass an element ID to capture a PNG of its current visual state. Use after creating/updating elements to verify visually before continuing; use when the user asks to see/preview.

## Assets
- `asset_tool > upload_image_by_url` — pass `url`; optional `asset_name`, `alt_text`. Returns id/name/mimeType/url/altText.
- Everything else (list/fetch/rename/move/delete assets, folders) → `data_assets_tool`:
  - `create_asset` — pass `site_id`, `file_name` (with extension, <100 chars), `file_hash` (MD5), optional `parent_folder`. Returns `uploadUrl`/`uploadDetails`; complete by POSTing file bytes to `uploadUrl` as multipart/form-data with every `uploadDetails` property as a form field plus the file under `'file'`. 201 = success. (See resource `webflow://guides/asset-upload`.)
  - `list_assets` — `site_id`; optional `limit` (1-100, default 100), `offset`, `folder_id`, `localeId` (secondary locales only — primary/unknown locale → 400).
  - `get_asset` — `asset_id`; optional `localeId` (secondary only).
  - `update_asset` — `asset_id` + any of `display_name` (extension NOT auto-preserved — include it), `alt_text` (string sets; `null` clears/marks decorative), `folder_id` (`null` = move to root). 404 on `folder_id` = nothing applied.
  - `delete_asset` — **DESTRUCTIVE (soft delete, no restore API) — confirm with user first.**
  - `list_asset_folders` — default page size 25; pass `limit 100` + paginate to enumerate all. `get_asset_folder` for one.
  - `create_asset_folder` — `site_id`, `display_name` (1-256 chars), optional `parent_folder`. **Folders cannot be deleted via API — confirm with user before creating.** Names unique per parent (409 on dup).
  - `update_asset_folder` — rename/move; `parent_folder: null` = move to root; moving into itself/descendant → 400.
  - `get_asset_preview` — visual preview; downloads smallest variant or original; non-image or >2MB files error.
  - `compress_assets` — jpg/jpeg/png/webp → webp/avif. **Replaces hosted file in place (same asset ID); original not retained — confirm with user first.** One task per site at a time (409 if pending). Poll `get_compression_task` (1s→10s backoff, up to 10 min).

## Pages
- `data_pages_tool > create_page` — pass `site_id`, `title`, `slug` (both required). Optional `parentFolderId`, `duplicateOf`, `draft`, seo/openGraph metadata.
- `designer_tool > create_page_folder` — pass folder `name`; optional `parent_id` for nesting (else root).
- `designer_tool > get_current_page` / `switch_page` (pass `page_id`) / `list_branches` (pass `page_id`) / `get_current_branch_id` / `get_branch_parent_page_id` / `get_current_mode` (returns `'design'|'build'|'preview'|'edit'|'comment'`|null).
- Schema markup: `query_pages_schema_markup` (site_id + pages[] 1..100, each `{id, localeId?}`); `bulk_update_pages_schema_markup` (site_id + pages[] 1..25, each `{id, localeId?, jsonLdSchema}` — object/string/null to clear).

## Branch Management (Data API) — Enterprise only

> "PREREQUISITE: branching is an Enterprise-only feature. Every action in this section requires the site's workspace to be on an Enterprise plan. On any other plan they all fail with 403 code 'not_enterprise_plan_site'."

- **Do not propose a branch workflow until confirmed** via `list_branches`: 200 = available, 403 `not_enterprise_plan_site` = not available.
- **Do not read the page field `canBranch` as an entitlement signal** — it's true even without branching entitlement, and flips true again after a branch is deleted.
- Treat `not_enterprise_plan_site` as final for the whole site — don't retry or try other branch actions hoping for a different result.
- `create_branch` / `delete_branch` / `update_branch` (pull main→branch) / `publish_branch` (branch preview→staging domain only, not production, not a merge) / `merge_branch` (branch→main) are all **async**: return `{taskId, status: "queued", nextActions}` — poll with `get_branch_task_status` (site_id + task_id) until `finished`/`failed`.
- `get_branch_conflicts` (site_id + branch_id) — read-only, lists `{id, name, type: "style"|"component", updatedAt?, updatedBy?}`. Empty array usually safe but conflicts are computed against last synced state — re-check after very recent edits.
- Conflict resolution: pass `resolutions[]` (`{id, resolution: "main"|"branch", type}`) to `update_branch`/`merge_branch` covering every conflict. Omitting on first `merge_branch` attempt returns code `branch_merge_conflict` with the list, so you can retry with resolutions in one step.
- `publish_branch` 409 `branch_publish_failed` = branch has no staging domain configured.
- On `merge_branch` success, the branch is consumed and disappears from `list_branches`.

## Styles — `data_style_tool`

- `create_style` — pass `name`; for combo classes pass `parent_style_name`. **Always use longhand property names when defining style properties.**
- `update_style` — `breakpoint` defaults to main if omitted; `pseudo` defaults to `"noPseudo"` if omitted.
- `get_styles` — use filters/queries; may return a lot of data.
- **Breakpoint-specific style behavior:**
  - xxl (1920px): applies to screens ≥ 1920px.
  - xl (1440px): applies to screens ≥ 1440px.
  - large (1280px): applies to screens ≥ 1280px.
  - main: applies to all devices unless overridden at other breakpoints.
  - medium (tablet): applies to screens ≤ 991px.
  - small (mobile landscape): applies to screens ≤ 767px.
  - tiny (mobile portrait): applies to screens ≤ 478px.

## Variables — `data_variable_tool`

- `create_variable_collection` — pass `name`. `create_variable_mode` — pass `name`, `variable_collection_id`.
- `get_variable_collections` / `get_variables` — filter by `query` or `filter_collections_by_ids`/`filter_variables_by_ids`.
- Type-specific create tools: `create_color_variable`, `create_size_variable`, `create_number_variable`, `create_percentage_variable`, `create_font_family_variable` — all take `name`, `variable_collection_id`.
- Matching `update_*_variable` tools take `name`, `variable_collection_id`.
- `rename_variable` — `variable_collection_id`, `variable_id`, `new_name`.
- `delete_variable` — `variable_collection_id`, `variable_id`. **Destructive — confirm with user before calling.**
- Each variable value: exactly **one** of `static_value` (typed literal, e.g. `"#ff0000"` or `{value:16, unit:"px"}`), `existing_variable_id` (alias to another variable), or `custom_value` (arbitrary CSS expression string, e.g. `"calc(100vh - 60px)"`, `"color-mix(in srgb, red 50%, blue)"`).
- Variables function like CSS custom properties, linked to styles.
- **Style variable modes** (scope = `style_name` + optional `parent_style_names`/`variant_id`/`breakpoint_id`/`pseudo`; omit `variant_id` or pass `'base'` for base variant):
  - `get_style_variable_modes` — returns `{collectionId: modeId}` map.
  - `set_style_variable_mode` — pass `style_name`, `variable_collection_id`, `mode_id` + optional scope fields. Call once per collection to override.
  - `remove_style_variable_mode` — clears one collection's override, reverts to inherited/default mode.
  - `remove_all_style_variable_modes` — clears every override on a style.
  - **Common workflow:** `get_variable_collections` → discover collection/mode IDs → `set_style_variable_mode` → `get_style_variable_modes` to verify.

## CMS — `data_cms_tool` / `cms_tool`

- Collections: `get_collection_list` (site_id), `get_collection_details` (collection_id), `create_collection` (name, site_id), `create_collection_static_field` / `create_collection_option_field` / `create_collection_reference_field` (collection_id, data), `update_collection_field` (collection_id, field_id, data), `delete_collection_field` (collection_id, field_id — **not bulk, one field per call**).
- Items: `list_collection_items` (collection_id; `request.cmsLocaleId` single or comma-separated for multiple locales — each locale variant returns as a separate entry with its own `cmsLocaleId`), `create_collection_items` (collection_id, data), `update_collection_items` (collection_id, data), `delete_collection_items` (as draft; collection_id, items), `unpublish_collection_items` (removes from live, sets isDraft=true).
- `publish_collection_items` — pass `collection_id`, `request.items` (array of `{id, cmsLocaleIds?}`). **Publishing ignores draft status** — a draft item publishes and becomes `isDraft=false`; no need to `update_collection_items` first. **Archived items never publish** — call still succeeds, `publishedItemIds` empty, error entry per requested locale — check `errors`, don't assume success.
- Filters/sort on `list_collection_items`: `request.filter` keyed by field slug or pseudo-field (`id`, `slug`), AND-combined. Operators by type: id → eq/ne/in/nin; slug → eq/ne/in/nin/contains/ncontains/exists; PlainText → same as slug; Number/DateTime → eq/ne/gt/gte/lt/lte/in/nin/exists; Switch/Color/Reference → eq/ne/in/nin/exists; Email/Phone/Link → eq/ne/in/nin/contains/ncontains/exists; Option → eq/ne/in/nin; RichText/Image/MultiImage/VideoLink/MultiReference → exists only. `contains`/`ncontains` case-insensitive (`ncontains` also matches empty/unset). `exists` takes `"true"`/`"false"`. `in`/`nin` take array or comma-string, up to 100 values. `request.sort` = `{fieldSlugOrPseudoField: "asc"|"desc"}`; sortable types: PlainText, Email, Phone, Number, DateTime, Switch, plus id/slug. Custom sort takes precedence over `sortBy`/`sortOrder`.
- All-locales reads: `request.allCmsLocales: true` reads every current CMS locale (cannot combine with `cmsLocaleId`, even empty string). Omitting both reads primary locale. Each locale variant = separate row; `pagination.total` counts variants.
- Item creation across locales: omit `id` to create new; list `cmsLocaleIds` to create linked variants. `allCmsLocales: true` on an item entry creates it in every current locale (cannot combine with `id`/`cmsLocaleIds`). Creation writes **staging content only**. 100-item request limit counts each locale variant. To add locales to an existing item, set `id` + new `cmsLocaleIds`.
- Collection field groups: `get_collection_field_groups` (collection_id) → `collectionFieldGroups`, `collectionFields`, `groupableCollectionFields` (internal fields like Name/Slug can't be grouped). `update_collection_field_groups` replaces the full array (empty array removes all). **Not available for Ecommerce collections.** Limits: max 50 groups; `displayName` required, unique per collection (case-insensitive), 1-64 chars; `description` optional, max 256 chars; each field ID in at most one group.

### Collection List element (DynamoWrapper) — Beta
- A "Collection List" on canvas is a `DynamoWrapper` (wraps `DynamoList` → `DynamoItem`, plus `DynamoEmpty`). **Its query config (source/filters/sort/pagination) lives on the element SETTINGS, not the CMS Data Tool** — read/write via `data_element_settings_tool > get_settings`/`set_settings`. `data_cms_tool` manages the collection/fields/items (the database) only.
- Value groups: object/array-shaped settings (source, filters, sort, pagination) → `static_json` (JSON-encoded **string**; rejects bare `null`); numeric (limit, offset) → `static_number`; reset → `clear: true` (writes null; **doesn't always persist for every key** — verify with `get_settings` after).
- Settable keys: `source`, `queryMode`, `filters`, `filterMatch`, `sort`, `limit`, `offset`, `pagination`, `curatedItemIds` (+ generic `domId`, `tag`, `visibility`, `attributes`).
- `source` = `{"collectionId": "..."}`; must be a valid source for that list or fails.
- `filters` = array of `{fieldSlug, operator, value?}`; `filterMatch` = `"all"`(AND)/`"any"`(OR). Operator vocabulary: equals, doesNotEqual, greaterThan, lessThan, greaterThanOrEqual, lessThanOrEqual, isSet, isNotSet, contains, doesNotContain, isOn, isOff — **validity is per field type**; error lists valid operators.
- `sort` = array of `{fieldSlug, direction: "ascending"|"descending"}`.
- `pagination`: `null` = off; `{"itemsPerPage": N}` = paginated. `queryMode`: `"dynamic"`|`"curated"`. `curatedItemIds` only valid when `queryMode` is curated.

## WHTML Builder — `data_whtml_builder`

- Insert elements from HTML/CSS strings. Pass `siteId`, `pageId`, `actions` with `html` and optionally `css`, plus `parent_element_id`, `creation_position`.
- **HTML rules:** `html` must be a single root element (`<div><p>Hello</p></div>` OK; two sibling roots not allowed). `html` must not contain `<style>` tags — CSS goes via `css`.
- **CSS rules:** `css` must be raw CSS rules only (no `<style>` wrapper). **`@keyframes` not allowed. Custom media queries not allowed** — only these Webflow breakpoints: Desktop/main = no media query; Tablet/medium = `@media screen and (max-width: 991px)`; Mobile Landscape/small = `@media screen and (max-width: 767px)`; Mobile Portrait/tiny = `@media screen and (max-width: 479px)`.
- `return_element_info: true` returns full inserted-element info. `scope_component_id` to insert into a component definition.

## Interactions/Animations — `data_interactions_tool`

- **This is the only tool that creates or edits animations** — element/style/component tools cannot. Authors IX3: click, hover, load, scroll, mouse-move, custom-event.
- Actions: `list_interactions`, `get_interaction`, `create_interaction`, `update_interaction`, `delete_interaction`, `guide`. `create` takes the whole definition (object-format triggers + inline timelines) in one call.
- **Every call takes `siteId` and `pageId` as TOP-LEVEL arguments**, not inside the action payload.
- **Call `data_interactions_tool > guide` before writing a payload** (or read resource `webflow://guides/interactions`) — write path enforces Designer-parity rules the JSON schema can't express; schema-valid payloads are often still refused.
- Two most-common first-attempt failures: **animate opacity under `wf:transform`, never `wf:style`**; **write property values as arrays/tuples, never `{from, to}` objects**.
- A successful write is not proof it runs — some accepted shapes save but never animate, and `get_interaction` can't tell the difference. **Confirm in Preview or on a published page.**

## Forms — `data_forms_tool`
- `list_forms` (site_id; limit 1-100, offset), `get_form` (form_id), `list_site_form_submissions` (site_id; optional element_id = form element GUID not ObjectId, locale_id; limit/offset), `list_form_submissions` (site_id, form_id), `get_form_submission` (site_id, form_submission_id), `update_form_submission` (site_id, form_submission_id, form_submission_data — only user-hidden fields writable), `delete_form_submission` (site_id, form_submission_id — **DESTRUCTIVE, confirm first**).

## Comments — `data_comments_tool`
- `list_comment_threads` (site_id; filter by pageId/isResolved/createdOn range), `get_comment_thread`, `list_comment_replies`, `search_comment_user_by_email`, `create_reply` (write; site_id, comment_thread_id, content 1-2040 chars incl. non-whitespace; optional localeId).
- Comments with `elementId {component, element}`: check `designer_tool > get_current_page` first; if it doesn't match `component`, `switch_page` to it; then `query_elements` with `queries: [{element_id: {component, element}}]`.

## Scripts — `data_scripts_tool`
- Scripts must be **registered before applying**: `register_inline_script` (site_id, source_code max 2000 chars, version, display_name) or `register_hosted_script` (site_id, hosted_location, integrity_hash, version, display_name).
- `update_registered_script` — at least one update field required.
- List: `get_registered_scripts`, `get_registered_script` (optional version), `get_site_scripts`, `get_page_scripts`.
- Add/remove single (recommended): `add_site_script`/`remove_site_script`, `add_page_script`/`remove_page_script` (location = header/footer, version, optional attributes) — doesn't affect other applied scripts.
- Bulk (replaces ALL): `set_site_scripts`/`set_page_scripts` — **WARNING: any scripts not in the array are removed.**
- Clear all: `clear_site_scripts`/`clear_page_scripts` (scripts stay registered, can be re-applied).
- Common workflow: `register_inline_script` → `add_site_script`/`add_page_script` → verify with `get_site_scripts`/`get_page_scripts`.
- **Freeform custom code** (distinct — raw head/footer blocks, no registration): `get_site_freeform_code`/`set_site_freeform_code` (location 'head'|'footer' only, content replaces full block, must fit char limit), `get_page_freeform_code`/`set_page_freeform_code` (localeId optional but must equal site's primary locale ID; folder-type pages unsupported → error).

## Fonts — `data_fonts_tool`
- `list_fonts`, `get_font` (font_id). `create_font` — site_id, file_name (woff2/woff/ttf/otf/eot), file_hash (MD5), font_family, weight (1-1000), italic, font_display, optional axes. Returns presigned upload (`customFont` + `upload`); binary not yet uploaded — POST bytes to `upload.url` multipart with `upload.fields` + file under `'file'`. **Presigned upload expires in ~15 minutes.**
- `update_font` — metadata only fields. `replace_font_file` — same presigned-upload pattern as create. `delete_font` / `batch_delete_fonts` (up to 100) — **DESTRUCTIVE, confirm first.**

## Sitemap — `data_sitemap_tool`
- Controls `includeInSitemap` flag for pages and CMS items. **Page endpoints do not support folder pages, collection template pages, or utility pages.**
- Read: `get_page_sitemap_status`/`list_pages_sitemap_status`; `get_item_sitemap_status`/`list_items_sitemap_status` (type 'staged'|'live', cmsLocaleId).
- Write: `update_page_sitemap_status`/`bulk_update_pages_sitemap_status` (up to 100); `update_item_sitemap_status`/`bulk_update_items_sitemap_status` (up to 100). **Item changes are STAGED — publish the site afterward to go live.**

## Analytics — `data_analyze_tool`
- Read-only. Requires `ALLOW_ANALYZE_CORE` entitlement; 404 if feature flag off. All actions require `site_id`, `startTime`, `endTime` (ISO 8601); `startTime` ≥ 2025-04-09T00:00:00.000Z; window ≤ 100 days.
- `get_traffic_report`, `get_top_pages_report`, `get_top_dimensions_report`, `get_top_events_report`, `get_time_on_page_report`. Call `get_query_guide` before running queries. `get_resolve_event_element_guide` to connect top_events rows to Webflow elements/components/CMS.
- 429 with Retry-After on top_events/time_on_page → back off before retrying.

## Webflow Cloud Apps — `data_apps_tool`
- Manages Cloud apps/GitHub sources/environments/deployments/domains/logs. CLI/local-source apps and variable writes require the Webflow CLI; machine tokens get 403 on `update_app` source changes.
- Resolve chain: `list_apps` → app_id → `list_environments` → env_id → `list_deployments` → deployment_id.
- Actions are sequential and **nontransactional**; do not batch dependent writes. Confirm before consequential/destructive actions; never retry a confirmed delete.
- `create_app` only accepts GitHub repo URLs. Set one stable `idempotency_key` for `create_app`/`create_environment`/`trigger_deployment`/`redeploy` retries. GitHub deploy actions return no deployment ID — record newest deployment before, then poll for a new one above baseline.
- Never request variable values from the user or put them on a command line.

## Campaigns — `data_campaigns_tool`
- One campaign per site landing page (`landingPage.pageId` references a regular page). `list_campaign_hub_sites` (workspace_id) to discover Campaign Hub sites.
- `create_campaign` — site_id, `markdown_brief` (structured with `###` headings: campaignName, campaignGoals, timeline, summary, targetAudiences, painPoints, channels, tone, proofPoints, additionalContext), `campaign_description`, optional `asset_ids`. Campaign + blank page created synchronously; content generation is async — poll `get_campaign_task_status` (site_id, taskId). Visibility via `get_campaign`/`list_campaigns` is not proof generation succeeded.
- Poll pattern (also used for branch/compression tasks): starting at 1s, doubling up to 10s, for at most 10 minutes; stop at "finished"/"failed".
- `update_campaign` is metadata-only (does not regenerate/republish page). `delete_campaign` hard-deletes campaign+assets (page archived not removed); only draft/ready-to-publish/no-landing-page/no-page states are deletable (live/paused/archived → 409).
- Page variations: `create_page_variation`, `get_page_variation_suggestions`, `get_page_variations`, `bulk_update_page_variations` (1-30 items), `delete_page_variation`.
- Personalization: `personalize_property` (component-instance prop binding) vs `personalize_element` (element setting binding) — use the right one; `unlink_delete_personalized_field` (destructive), `update_personalized_field` (rename only).
- Campaign analytics ONLY via `get_time_series`, `get_form_submissions_count`, `get_engaged_accounts`, `get_opportunity_summary` — **do not use `data_analyze_tool` for campaigns** (different entitlement).

## Localization — `data_localization_tool`
- **Only writes to SECONDARY locales** — primary locale not editable via this tool.
- `list_components` (site_id) to find localizable component IDs.
- `get_page_content`/`update_static_content` (page_id, localeId required for write), `get_component_content`/`update_component_content` (site_id, component_id, localeId required for write), `get_component_properties`/`update_component_properties` (localeId required for write).
- Workflow: `data_sites_tool` for locale IDs → `list_components` if needed → `get_*_content` with localeId to see current state → `update_*` with localeId to write.

## Limits & warnings summary

- Branching: Enterprise plan only.
- `register_inline_script`: source code max 2000 chars.
- Comment reply `content`: 1-2040 chars.
- Schema markup: `query_pages_schema_markup` pages[] 1..100; `bulk_update_pages_schema_markup` pages[] 1..25.
- Sitemap bulk updates: up to 100 items.
- CMS create/list item requests: 100-item limit (counts locale variants).
- Collection field groups: max 50, displayName 1-64 chars unique, description max 256 chars.
- Font: weight 1-1000; presigned upload URLs expire ~15 minutes.
- Asset display name: 1-256 chars for folders; asset preview fails for non-image or >2MB files.
- `compress_assets`: 1-100 asset_ids; one compression task per site at a time.
- Analytics window: ≤100 days, startTime ≥ 2025-04-09.
- Campaign markdown_brief: max 20,000 chars.
- Destructive/irreversible actions requiring user confirmation first: `remove_element`, `unregister_component`, `delete_form_submission`, `delete_variable`, `delete_asset` (soft, no restore API), `create_asset_folder` (folders can't be deleted via API), `compress_assets` (replaces file in place, original not retained), `delete_font`/`batch_delete_fonts`, `unlink_delete_personalized_field`, `delete_campaign`.
