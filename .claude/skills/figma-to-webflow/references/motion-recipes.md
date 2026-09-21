# Motion recipes (measured on DRH, the build the reviewer calls the reference)

DRH uses four mechanisms and no GSAP. Copy these shapes; the reviewer approved them.

## 1. Lenis smooth scroll (site footer code, verbatim)

```html
<link rel="stylesheet" href="https://unpkg.com/lenis@1.2.3/dist/lenis.css">
<script src="https://unpkg.com/lenis@1.2.3/dist/lenis.min.js"></script>
<script>
let lenis = new Lenis({ lerp: 0.1, wheelMultiplier: 0.7, gestureOrientation: "vertical", normalizeWheel: false, smoothTouch: false });
function raf(time) { lenis.raf(time); requestAnimationFrame(raf); }
requestAnimationFrame(raf);
$("[data-lenis-start]").on("click", function () { lenis.start(); });
$("[data-lenis-stop]").on("click", function () { lenis.stop(); });
$("[data-lenis-toggle]").on("click", function () {
  $(this).toggleClass("stop-scroll");
  if ($(this).hasClass("stop-scroll")) { lenis.stop(); } else { lenis.start(); }
});
</script>
```

Put `data-lenis-toggle` on the hamburger so the page stops scrolling behind the open mobile menu (a reviewer rule). Install with `data_scripts_tool > set_site_freeform_code` location `footer`.

## 2. Scroll-scrubbed IX3 timelines

Every DRH scroll interaction shares this trigger:

```json
{"extensionKey":"wf:scroll","config":{"controlType":"scroll","scrollTriggerConfig":{
  "showMarkers":false,"clamp":true,"start":"top 5%","end":"bottom top",
  "scrub":0.8,"enter":"play","leave":"none","enterBack":"none","leaveBack":"none"}},
 "target":{"extensionKey":"wf:class","value":["<style block id>"]}}
```

- Target a **class**, not an element, so every instance animates and the interaction survives copy-paste.
- `scrub: 0.8`, `clamp: true`. Name it `"<Thing> While Scrolling"`; make `(Tablet)` and `(Mobile)` copies when values differ, and switch the desktop one off below 991 with `"conditionalPlayback":[{"type":"breakpoint","behavior":"dont-animate","breakpoints":["medium","small","tiny"]}]`.
- Set `showMarkers` to false before handover. DRH shipped with markers on; do not.
- Text of a section fades in as one action targeting the title wrapper class (`Title All Fade In`), not one action per text node. Initial state: one `Initial State` action at position 0 setting `opacity: 100% → 0%` on the classes that will reveal.
- Call `data_interactions_tool > guide` before writing a payload. Opacity lives under `wf:transform` (`"opacity": ["0%","100%"]`), never `wf:style`; every property value is an array pair, never `{from, to}`.

## 3. Dark to light section transition (the reviewer's "smooth, never hard cut")

Wrap the band in a class that owns the background, give it a CSS transition, then scrub its background colour between two Webflow colour variables over the section's scroll:

```css
.section_sorting { overflow: clip; background-color: var(--theme-black); transition: background-color 100ms ease; }
.section_wrapper { transition: background-color 400ms ease; }
```

```json
{"name":"Bg Color","presetId":"preset-custom-animation",
 "targets":[{"extensionKey":"wf:class","value":["<.section_wrapper id>"]}],
 "timing":{"duration":0.1,"position":0,"ease":7},"tt":2,
 "properties":{"wf:style":{"backgroundColor":[
    {"type":"ref","value":{"variableId":"<Theme Black variable id>"}},
    {"type":"ref","value":{"variableId":"<White variable id>"}}]}}}
```

Trigger: `wf:scroll`, `start: "top 15%"`, `end: "bottom top"`, `scrub: 0.8`, on the section class. Images that run into the boundary get a gradient overlay div so nothing looks cut.

## 4. Hover

Buttons: CSS only, `transition: all 250ms ease`, hover `transform: scale(0.95)` plus colour inversion (lime → black background, black → white text). Cards: a light or background change on hover, no scale, no zoom.

## 5. Sticky scroll stages

`.sticky-wrap { position: sticky; top: 0; height: 100vh }` inside a tall `.sticky_scroll` wrapper; the scrubbed timeline moves the stage children (map zoom: `width` and `y` in em) while the wrapper scrolls. Disable below 991 and show the final state instead.

## 6. Other libraries in use

- Swiper 8 for sliders (page footer code), Finsweet Attributes v2 for CMS filter and load-more on news pages, Flowbase count-up for numbers, a CSS marquee. Pin exact versions from unpkg or jsdelivr in freeform code; the team does not use the scripts registry.
