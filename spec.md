# oxfordpat.co.uk — Landing Page Spec

## SECTION 1: SPEC

**One-line purpose**
A static landing page that converts ESAT-searching students and parents into leads for Dr Anthony Korte's small-group online preparation course.

**Users and use cases**
- As a Year 12/13 student targeting Cambridge Physics, Physics & Philosophy, or Engineering, I want to understand what ESAT preparation is available so that I can assess whether this course suits my needs.
- As a parent of a Cambridge applicant, I want to see the tutor's credentials and course format so that I can make a confident booking decision.
- As a student who previously searched for "Oxford PAT" preparation, I want to find this page via search so that I can discover its relevance to the ESAT.

**Requirements**
1. Page must communicate the ESAT's scope (Mathematics 1, Mathematics 2, Physics) and its role in Cambridge admissions.
2. Page must present Dr Korte's academic credentials clearly and credibly.
3. Page must describe the course format (small group, online, Zoom).
4. Page must include at least one prominent call-to-action linking to physicsandmathscourses.com.
5. Page must include a brief reference to the Oxford PAT legacy name for search discoverability.
6. Page must render correctly on mobile and desktop without external resources.
7. Hero section must use hero.jpeg as a full-bleed background with a dark overlay ensuring white text is legible.

**Edge cases**
- hero.jpeg fails to load: page must remain fully legible — overlay and text must not depend on the image being present (background-color fallback: #111).
- JavaScript disabled: page must be fully functional — no JS required.
- Narrow viewport (320px): all text, CTAs, and layout must reflow without horizontal scroll.

**Acceptance criteria**
```
Given a visitor arrives via a "Cambridge ESAT preparation" search
When the page loads
Then the hero communicates the exam name, subjects covered, and tutor name above the fold

Given a visitor reads the credentials section
When they scan Dr Korte's qualifications
Then First Class Oxford degree, MSc with Distinction, and Imperial PhD are all visible in one section

Given a visitor is ready to book
When they click the primary CTA
Then they are taken to physicsandmathscourses.com in a new tab

Given a visitor arrived via a "Oxford PAT" search
When they read the page
Then they encounter a clear explanation that PAT preparation is now ESAT preparation

Given the page loads on a 375px mobile viewport
When no interaction has occurred
Then no horizontal scroll exists and all text is readable without zooming

Given hero.jpeg is unavailable
When the page renders
Then the hero section has a solid dark fallback background and text remains legible
```

---

## SECTION 2: PLAN

**Stack and architecture**
Single `index.html` file. All CSS inlined in `<style>` block in `<head>`. No JavaScript. No external fonts, no CDN, no framework. Deployed to GitHub Pages. [ASSUMPTION: no build step — the committed index.html is served directly by GitHub Pages]

**Data model changes**
None. Static content only.

**API contracts**
None. Outbound link only: `https://physicsandmathscourses.com` opened in a new tab via `target="_blank" rel="noopener noreferrer"`.

**Page structure (sections in order)**
1. `<head>` — meta charset, viewport, title, description, canonical, Open Graph tags [ASSUMPTION: canonical URL is https://oxfordpat.co.uk]
2. **Header** — wordmark + CTA button
3. **Hero** — full-viewport section, hero.jpeg background with `rgba(0,0,0,0.55)` overlay, white H1 headline, subheadline, primary CTA button
4. **What is the ESAT** — brief factual paragraph + numbered module list
5. **What the Course Covers** — three-column cards (stacks to single column on mobile): Mathematics 1, Mathematics 2, Physics
6. **About Dr Korte** — credentials as `<dl>` definition list (label / value rows)
7. **Course Format** — four format items: group size, delivery, coverage, focus
8. **Previously the Oxford PAT** — short paragraph for SEO and returning visitors
9. **Footer** — second CTA linking to physicsandmathscourses.com, copyright line

**Patterns to follow**
Semantic HTML5 elements (`<header>`, `<main>`, `<section>`, `<footer>`, `<article>`, `<dl>`). CSS custom properties for brand colour and font stack. Flexbox and CSS Grid for layout. No JS.

**Testing strategy**
Manual only (static page):
- Render at 320px, 375px, 768px, 1280px
- Verify hero.jpeg loads and overlay is applied
- Verify CTA links navigate to physicsandmathscourses.com in a new tab
- Check accessibility with axe or similar for WCAG AA [ASSUMPTION: WCAG AA compliance is desirable but not formally required]

**Security and performance constraints**
- All external links use `rel="noopener noreferrer"`
- No user input, no forms, no JS — attack surface is minimal
- Target: under 200KB total page weight (dominated by hero.jpeg)
- [ASSUMPTION: hero.jpeg is already reasonably compressed — no build-time optimisation step]

---

## SECTION 3: TASKS

## Task 1: Write index.html with full structure, copy, and inline CSS

**What to build:** Complete `index.html` containing all sections listed in the plan, with all CSS inlined, hero.jpeg background with dark overlay, system font stack, #0077bb brand colour, mobile-responsive layout, and real copy throughout. The file must be self-contained — no external dependencies.

**Files likely affected:** `index.html` (create)

**Acceptance criteria:**
1. Opening `index.html` in a browser with no network connection renders the full page correctly (hero image aside).
2. At 375px viewport width, no horizontal scrollbar appears and all text is legible without zooming.
3. Clicking the primary CTA opens `https://physicsandmathscourses.com` in a new tab.

**Dependencies:** none

---

**Review checkpoint:** Before deploying, open index.html locally in a browser, resize to 375px, and confirm: (a) hero image and overlay look correct, (b) all credential copy is accurate (check spelling of Dr Korte's name and qualifications), and (c) the CTA link resolves correctly.

---

## Assumptions to review

1. No portrait photo of Dr Korte available — **Impact: MEDIUM**
   Correct this if: a headshot is available — it would strengthen the credentials section significantly.

2. No specific pricing or session dates on this page — **Impact: MEDIUM**
   Correct this if: a price point or next cohort date should appear — even a single figure can improve conversion.

3. Canonical URL is https://oxfordpat.co.uk (no www) — **Impact: LOW**
   Correct this if: the GitHub Pages CNAME is configured for www.oxfordpat.co.uk.

4. hero.jpeg is already web-optimised — **Impact: LOW**
   Correct this if: the file is very large (>500KB) — consider compressing before deploy.

5. WCAG AA compliance is desirable but not a hard requirement — **Impact: LOW**
   Correct this if: accessibility compliance is formally required.
