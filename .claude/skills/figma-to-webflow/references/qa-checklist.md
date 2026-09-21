# QA checklist: what the reviewer sends back

Distilled from the review rounds on DRH, Furec, Medicini, Anatomy, OTTO, Primegate, Netconnection, KNO, GMG and the Marina template (Slack channels #webflow-abdullah and #webflow-ali-hassan, Webflow comment threads, Notion desk cards, June to September 2026). The reviewer is Talha or the Webwavers partner (Edip). The same notes come back on nearly every build, so treat every line as a rule to satisfy **before** handing over. Where a line quotes, it quotes the review verbatim.

## 1. Navbar and menus

- Navbar spans the **full width** of the viewport, not the content container. "Navbar not full width - please like DRH."
- Dropdown panels sit **centred under the navbar**, open with a smooth transition, and hovering from one dropdown to the next must not flicker. The chevron rotates on open, and it points the right way when closed (down) and open (up).
- The navbar (and hero copy) is **visible on first paint**. Never start the navbar at opacity 0. If the hero animates, navbar and hero content come in **together**: "the navbar should be visible at first when loading the page - best would be to move in navbar and texts from below in the same time coming in together."
- Mobile menu: logo present inside the open menu, same type sizes as desktop, body scroll locked while it is open ("make sure always that its not possible to scroll when the Mobile Menu is opened"), and every button in the mobile menu links to the same target as its desktop twin. Mobile-only elements live in classes that are `display:none` above 767px, so they are invisible on the desktop canvas; check them on a phone.
- Optional polish often requested: navbar slides out on scroll down and back in on scroll up.

## 2. Animations

- **Less.** "All Fade Ins etc. should be in the correct order and please not too much, should be like DRH and not for every text since it looks too much often on mobile, but also on desktop too." Default: one reveal per section, not per element.
- A section's text block (eyebrow + heading + paragraph + buttons) rises **as one group** in one timeline. "The texts always needs to come up together in one animation, not every text seperated in different animation timing."
- Cards in the same row animate in **at the same time** (or a short stagger that finishes within ~0.4s). Do not leave the last two cards behind.
- Trigger early. Content must be visible before the user has scrolled past it: "This should appear faster, when im already scrolled through it its even not there." Use a viewport offset around 10 to 20 percent, never 50.
- Initial states are the number one cause of "blank space at the top of sections" on iPhone. Any element with an opacity 0 initial state must have a trigger that fires on every device and breakpoint, or it stays invisible. When in doubt on mobile, disable the animation rather than risk hidden content. Also verify no IX hides a whole container that never receives its trigger (Netconnection hero bug: "this whole texts and button where not visible on first load").
- Section colour changes (dark to light, light to dark) are **smooth**, never a hard cut: gradient overlay at the boundary, or a background transition. Images that continue into the next section need a gradient overlay so they do not look cut off.
- Scroll-driven graphics (timeline lines, circle steps, maps) build step by step with the scroll, starting at step 01, moving smoothly; sticky image beside scrolling text where designed.
- Loop animations only where asked: a light that pulses brighter and darker.
- Hover on cards: a light or background change, **no zoom** ("This should not zoom in, just background light can be animated on hover like DRH").
- Do not add playful rotating or bouncing motion the design does not show: "it looks too playful, also since the most other sections are not animated." Delete rather than tune.
- Glass effect (backdrop-filter) must be tested in Chrome and Safari; it "is most times not properly working". Provide a solid or semi-transparent fallback.
- Test every animation on **mobile Safari** on a real phone. "It feels really broken and not smooth" is a common verdict; reduce transform distance and duration on mobile.

## 3. Layout and structure

- Match Figma spacing and type sizes exactly. Recurring notes: "Not same Text size as in figma", "This text on live version has not the same space as in figma", "Please check Figma if this space is normal, it looks too big."
- Everything on the page aligns to one container width. The footer CTA is the reference: "This not aligns with the width of the Footer CTA, is this like in figma?"
- Full-viewport sections must fit one screen at 1440x900 and on a phone: "This section is not visible in one screen."
- Correct nesting. Elements that overlap on subpages are almost always nested inside the hero wrapper by mistake: "this always happens across all websites you created for us since its not correctly added in layers and need to drag outside the hero element."
- Same shared element (page label, breadcrumb, hero badge) sits in the same place on every page: "make sure the header shows up on the exact same spot on each page."
- Consistent components across pages. When the Figma differs between pages for the same element, follow the homepage version: "The Homepage is the best build, if you see a difference in the same elements like cards, make it like the Homepage."
- Use the **dev** Figma file or the Draft tab named in the brief, not the designer's working file.
- Pages live in the folder structure the nav implies. No page reachable at two URLs, no sibling outside its folder (the Anatomy `/ai-products/` case). Check with a `curl -o /dev/null -w "%{http_code}"` sweep of every URL in the nav.

## 4. Images and assets

- Compress. "All Images needs to be compressed and in high quality. Currently and like always images are over 1MB." Target: hero under 400KB, cards under 200KB, WebP or AVIF, and still sharp at 2x. "This images on mobile loaded 5-6sec on my phone."
- Export images from Figma **with their designed background** (the grey card background, the rounded mask) and **without baked-in text**, so the styling survives and the text stays editable.
- No image may be missing on mobile. Three separate "Image is missing mobile" notes came back on one page.
- Background images must not crop the subject; check object-position at every breakpoint.
- Icons must be the ones from Figma, in the right places: "Most Icons in this sections are wrong."
- Alt text on every image, real descriptions, never template leftovers ("Defenseflow Webflow Template" on 34 images).
- Favicon and webclip set to the client's, not the template's.
- Broken image sweep before handover (paste in the browser console):
  ```js
  [...document.querySelectorAll('img')].filter(i=>!i.complete||i.naturalWidth===0).forEach(i=>{i.style.outline='4px solid red';console.log(i.src)})
  ```
- Video: use the MUX player embed when the client supplies MUX URLs; otherwise Webflow video with a poster.
- Lottie files: ask for them per section if the zip is missing any; hide the section rather than ship a placeholder.

## 5. Typography and text

- Minimum font size **14px** anywhere on the site.
- One typeface family unless the brief names a second; accent font only for highlights.
- Add to the site head:
  ```html
  <style>body{-webkit-font-smoothing:antialiased;-moz-osx-font-smoothing:grayscale;}</style>
  ```
- Link colour set explicitly. Safari otherwise shows default dark blue links: "Link texts are dark blue on mobile & desktop (safari browser)."
- Keep copy exactly as in Figma, in the site's language. No English leftovers in a German site ("Maximal 10MB", not "Max file size").
- No section-label placeholders left as headings ("Credibility Band", "One-line positioning", "Services Overview").
- Template content removed from the DOM, not hidden: testimonials, case studies, badges, template people. "Make sure the placeholder testimonials are completely removed from the public DOM rather than only visually hidden."

## 6. Links, forms, CMS

- Every button links somewhere real. Phone numbers and "anrufen"/"Telefon" buttons use `tel:`, mail buttons use `mailto:`.
- Ask early where an unclear CTA should link ("Read the API Docs") and leave it untouched until answered.
- CMS filters must work on the published site (GMG Publikationen, Netconnection Ratgeber both came back broken). Test each filter value; "load more" must not show a white screen.
- Decide with the brief whether repeated content is CMS or static (jobs grid, projects, team). If undecided, ask before building.
- Forms: field labels in the site language, file-size hints translated, HubSpot or other embeds tested after publish (embeds do not render on the canvas).
- Meta title and description on every page, 404 page title branded, Open Graph image set.
- `theme-color` meta so the phone status bar matches the hero background, and update it on dark/light section changes if the client asks.

## 7. Responsiveness

- Build order the team uses and reports: desktop all pages, then responsive all pages, then animations, then comments. Say which stage each page is in when posting links ("Non-Responsive", "Not Completed").
- Check tablet, mobile landscape and mobile portrait. Designers usually deliver only the homepage mobile frame; derive the rest from the homepage patterns.
- Text alignment and image presence on mobile are the two most common mobile notes.

## 8. Handover protocol

The reviewer's standard, quoted: "1. Make the change 2. Publish it 3. Open the live URL yourself and check it works 4. Tell me here that it is done, and say what you checked." In Webflow nothing is live until Publish; the Designer will look correct the whole time.

Post staging links for **every** page, one per line, with the status of each. Send the first two or three homepage sections for review before building the rest ("Send me once 2-3 sections are made on homepage"). Answer questions in the Webflow comment thread they were asked in, then resolve the comment.
