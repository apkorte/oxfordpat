# oxfordpat.co.uk — Landing Page Spec

## SECTION 1: SPEC

**One-line purpose**
A static landing page that converts ESAT-searching students and parents into leads for Dr Anthony Korte's small-group online preparation course.

**Users and use cases**
- As a Year 12/13 student targeting Cambridge Physics, Physics & Philosophy, or Engineering, I want to understand what ESAT preparation is available so that I can assess whether this course suits my needs.
- As a parent of a Cambridge applicant, I want to see the tutor's credentials and course format so that I can make a confident booking decision.
- As a student who previously searched for "Oxford PAT" preparation, I want to find this page via search so that I can discover its relevance to the ESAT.
- As a Year 12 student, I want to know whether early preparation is worthwhile so that I can plan ahead.

**Requirements**
1. Page must communicate the ESAT's scope (Mathematics 1, Mathematics 2, Physics) and its role in Cambridge admissions.
2. Page must present Dr Korte's academic credentials clearly and credibly, including all three qualifications.
3. Page must describe the course format (small group, online, Zoom).
4. Page must include at least one prominent call-to-action linking to physicsandmathscourses.com/book-a-course/oxford-pat-course.html.
5. Page must include a brief reference to the Oxford PAT legacy name for search discoverability.
6. Page must render correctly on mobile and desktop without external resources.
7. Hero section must use hero.jpeg as a full-bleed background with a dark overlay ensuring white text is legible.
8. Page must include a FAQ section with six expanded questions (no accordion) and matching FAQPage JSON-LD schema.
9. Footer must include subtle cross-links to physicscourses.co.uk and alevelmathsonline.co.uk.
10. All JSON-LD schema blocks must be in `<head>`.

**Edge cases**
- hero.jpeg fails to load: page must remain fully legible — overlay and text must not depend on the image being present (background-color fallback: #111).
- JavaScript disabled: page must be fully functional — no JS required.
- Narrow viewport (320px): all text, CTAs, and layout must reflow without horizontal scroll.
- Chrome desktop mode on mobile: `width=device-width` viewport meta prevents scaled-down rendering.

**Acceptance criteria**
```
Given a visitor arrives via a "Cambridge ESAT preparation" search
When the page loads
Then the hero communicates the exam name, subjects covered, and tutor name above the fold

Given a visitor reads the credentials section
When they scan Dr Korte's qualifications
Then First Class Oxford degree, MSc with Distinction (UCL), and Imperial PhD are all visible

Given a visitor is ready to book
When they click the primary CTA
Then they are taken to physicsandmathscourses.com/book-a-course/oxford-pat-course.html in a new tab

Given a visitor arrived via a "Oxford PAT" search
When they read the page
Then they encounter a clear explanation that PAT preparation is now ESAT preparation

Given the page loads on a 375px mobile viewport
When no interaction has occurred
Then no horizontal scroll exists and all text is readable without zooming

Given hero.jpeg is unavailable
When the page renders
Then the hero section has a solid dark fallback background and text remains legible

Given a search engine crawls the page
When it reads the FAQ section
Then FAQPage JSON-LD matches the visible questions and answers exactly
```

---

## SECTION 2: PLAN

**Stack and architecture**
Single `index.html` file. All CSS inlined in `<style>` block in `<head>`. No JavaScript. No external fonts, no CDN, no framework. Deployed to GitHub Pages.

**Data model changes**
None. Static content only.

**API contracts**
None. Outbound links use `target="_blank" rel="noopener noreferrer"`.
- Primary CTA: `https://physicsandmathscourses.com/book-a-course/oxford-pat-course.html`
- Privacy policy: `https://physicsandmathscourses.com/our-services/privacy-policy.html`
- Cross-links: `https://physicscourses.co.uk`, `https://alevelmathsonline.co.uk`

**Page structure (sections in order)**
1. `<head>` — meta charset, viewport, title, description, canonical (`https://oxfordpat.co.uk/`), Open Graph, favicon, sitemap link, JSON-LD schemas
2. **Header** — wordmark + CTA button
3. **Hero** — full-viewport section, hero.jpeg background with `rgba(0,0,0,0.55)` overlay, white H1, subheadline, primary CTA
4. **What is the ESAT** — brief factual paragraph + numbered module list
5. **What the Course Covers** — three-column cards (stacks on mobile): Mathematics 1, Mathematics 2, Physics
6. **About Dr Korte** — credentials as `<dl>` definition list (label / value rows)
7. **Course Format** — four format items: group size, delivery, coverage, focus
8. **Previously the Oxford PAT** — short paragraph for SEO and returning visitors
9. **FAQ** — six questions and answers, expanded (no accordion), `<dl>` structure
10. **Footer** — second CTA, cross-links, copyright (2026), privacy policy link

**JSON-LD schemas in `<head>`**
- `Person` — Dr Anthony Korte, with `hasCredential` (all three qualifications), `alumniOf` (Oxford, UCL, Imperial)
- `Course` — ESAT Preparation Course
- `EducationalOrganization` — `url: https://oxfordpat.co.uk/`, `sameAs: https://physicsandmathscourses.com`
- `FAQPage` — six questions matching the FAQ section verbatim

**Patterns to follow**
Semantic HTML5 (`<header>`, `<main>`, `<section>`, `<footer>`, `<article>`, `<dl>`). CSS custom properties. Flexbox and CSS Grid. No JS.

**Mobile CSS**
- `text-size-adjust: 100%` on body — prevents Chrome text inflation
- Portrait orientation query (`max-width: 768px`): `html { font-size: 18px }`
- High DPI query (`-webkit-min-device-pixel-ratio: 2`, `max-width: 768px`): `body { font-size: 20px }`, `p, li, dd, dt { font-size: 20px !important }`
- Note: Chrome "Desktop site" mode bypasses `width=device-width` and cannot be overridden in CSS — this is a browser setting issue, not a page issue.

**Testing strategy**
Manual only (static page):
- Render at 320px, 375px, 768px, 1280px
- Verify hero.jpeg loads and overlay is applied
- Verify all CTA links navigate to the correct URL in a new tab
- Validate JSON-LD with Google Rich Results Test
- Check accessibility with axe for WCAG AA

**Security and performance constraints**
- All external links use `rel="noopener noreferrer"`
- No user input, no forms, no JS — attack surface is minimal
- Target: under 200KB total page weight (dominated by hero.jpeg)

---

## SECTION 3: TASKS

## Task 1: Write index.html with full structure, copy, and inline CSS

**What to build:** Complete `index.html` containing all sections listed in the plan, with all CSS inlined, hero.jpeg background with dark overlay, system font stack, #0077bb brand colour, mobile-responsive layout, FAQ section, and real copy throughout.

**Files likely affected:** `index.html`

**Acceptance criteria:**
1. Opening `index.html` in a browser with no network connection renders the full page correctly (hero image aside).
2. At 375px viewport width, no horizontal scrollbar appears and all text is legible without zooming.
3. Clicking the primary CTA opens `https://physicsandmathscourses.com/book-a-course/oxford-pat-course.html` in a new tab.
4. Google Rich Results Test reports valid FAQPage schema.

**Dependencies:** none

---

**Review checkpoint:** Before deploying, open index.html locally, resize to 375px, and confirm: (a) hero image and overlay look correct, (b) credential copy matches exactly (Oxford First, MSc Distinction UCL, Imperial PhD), (c) FAQ questions match JSON-LD verbatim, (d) all CTA and footer links resolve correctly.

---

## Assumptions to review

1. No portrait photo of Dr Korte available — **Impact: MEDIUM**
   Correct this if: a headshot is available — it would strengthen the credentials section.

2. No specific pricing or session dates on this page — **Impact: MEDIUM**
   Correct this if: a price point or next cohort date should appear.

3. Canonical URL is `https://oxfordpat.co.uk/` (no www, with trailing slash) — **Impact: LOW**
   Correct this if: the GitHub Pages CNAME is configured for www.oxfordpat.co.uk.

4. hero.jpeg is already web-optimised — **Impact: LOW**
   Correct this if: the file is very large (>500KB).

5. WCAG AA compliance is desirable but not a hard requirement — **Impact: LOW**
   Correct this if: accessibility compliance is formally required.
