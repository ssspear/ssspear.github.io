# Portfolio Refresh Implementation Plan

> **For agentic workers:** Execute task-by-task. This is a static, test-light site, so each task ends with a **browser/verification** check rather than unit tests. Steps use checkbox (`- [ ]`) syntax.

**Goal:** Replace the dated 2019 site with a modern light single-page portfolio that features Cosmic Explorer, and remove stale cruft.

**Architecture:** Hand-written static `index.html` + `css/site.css`, no build step. GitHub Pages serves it from `master` at the custom domain `sarahspeartheengineer.com`. A `.nojekyll` file opts out of Jekyll processing.

**Tech Stack:** Plain HTML5 + CSS. No framework, no JS build.

## Global Constraints (from the spec, verbatim)

- Palette: white `#ffffff`, text `#111318`, single indigo accent `#5b47e0`. System font stack. Generous whitespace.
- Title: **Senior Fullstack Engineer**. Name: **Sarah Spear**.
- Tagline (exact): "I build full-stack products for the web, and point the occasional telescope at everything else."
- Personality line (exact): "Fullstack engineer · amateur astronomer · dog person · still a little short to be a stormtrooper · she/they"
- Links: GitHub `https://github.com/ssspear`; LinkedIn `https://www.linkedin.com/in/spearsarah/`; Cosmic Explorer live `https://sarahspeartheengineer.com/cosmic-explorer/`; source `https://github.com/ssspear/cosmic-explorer`.
- No `http://` asset/link URLs; no analytics; keep `CNAME`.
- Single page only (no blog, no separate about page, no extra projects).

---

## File Structure

- `index.html` (REWRITE) — the single page (hero + featured project + footer).
- `css/site.css` (NEW) — the light design system implementing the approved mockup.
- `images/headshot.jpg` (NEW) — new photo, downsized for web (~<200 KB, ~600px).
- `images/cosmic-explorer.png` (NEW) — screenshot of the live explorer (captured or placeholder).
- `.nojekyll` (NEW) — disable Jekyll.
- REMOVE (stale/unused): `blog/`, `feed.xml`, `about/`, `fonts/`, `icons/`, `images/author.jpg`, `images/placeholder.png`, `css/main.css`.
- KEEP: `CNAME`, `404.html`.

Approved visual reference: the mockup generated during brainstorming (light hero, photo in a circular accent ring, indigo accent, featured Cosmic Explorer card). Match its layout, spacing, and color.

---

## Task 1: Clean out the stale site + add the headshot

**Files:** remove `blog/`, `feed.xml`, `about/`, `fonts/`, `icons/`, `images/author.jpg`, `images/placeholder.png`, `css/main.css`; create `.nojekyll`, `images/headshot.jpg`.

- [ ] **Step 1: Remove stale files**

```bash
cd <repo>
git rm -r blog about fonts icons feed.xml images/author.jpg images/placeholder.png css/main.css
```

- [ ] **Step 2: Add `.nojekyll`**

```bash
touch .nojekyll
git add .nojekyll
```

- [ ] **Step 3: Add the downsized headshot**

Downsize the provided photo (`.context/attachments/MORd5s/IMG_1748.jpg`, ~500 KB) to a web size and save as `images/headshot.jpg`:
```bash
# macOS sips (preferred). Target ~640px on the long edge.
sips --resampleWidth 640 <source>.jpg --out images/headshot.jpg
```
If `sips` is unavailable, use any available tool (ImageMagick `convert -resize 640x`, or Python Pillow). Verify the result is a valid JPEG under ~200 KB: `file images/headshot.jpg` and `ls -lh images/headshot.jpg`.

- [ ] **Step 4: Verify the working tree**

```bash
git status --short
ls images/
```
Expected: deletions staged for the stale files; `.nojekyll` and `images/headshot.jpg` present; `CNAME` and `404.html` untouched.

- [ ] **Step 5: Commit**

```bash
git add -A
git commit -m "chore(site): remove stale 2019 theme, add .nojekyll and new headshot"
```

---

## Task 2: Build the new single page

**Files:** Create `index.html` (rewrite), `css/site.css`.

**Interfaces:** `index.html` links `css/site.css`; references `images/headshot.jpg` and `images/cosmic-explorer.png` (the screenshot arrives in Task 3 — reference the path now; a styled panel shows until the image exists).

- [ ] **Step 1: Write `css/site.css`**

Implement the approved light design as a small stylesheet using the Global Constraints tokens. Requirements the CSS must satisfy (match the approved mockup):
- CSS custom properties for the palette (`--bg:#ffffff; --ink:#111318; --muted:#8a90a0; --accent:#5b47e0`).
- System font stack; base font-size ~16px; comfortable line-height.
- Centered container, `max-width: 960px`, horizontal padding, vertical rhythm.
- `.hero` two-column flex (text + photo) that **stacks to one column below ~760px**.
- `.headshot`: 236px circle, `object-fit:cover; object-position:52% 22%`, `5px solid #fff` border, soft shadow (`0 22px 50px rgba(20,20,60,.20)`), wrapped so a `linear-gradient(135deg,#7d6bff,#c9a6ff 55%,#f0a6c8)` ring shows ~5px around it.
- `.eyebrow` (accent, uppercase, letter-spaced), `h1` (~48px, weight 800, tight letter-spacing), `.lead` tagline (~16px), `.meta` personality line (~13px, muted).
- Buttons: `.btn-primary` (indigo bg, white text, subtle shadow) and `.btn-ghost` (1px border, ink text).
- `.featured` block on a faint tint (`#f7f7fb`) with the project card: heading, description, tech-tag pills, live/source buttons, and a screenshot area.
- `.shot` screenshot container with border/radius; if the image is missing it shows a neutral panel (set a min-height and background so the layout holds).
- A minimal footer (name + year + the two social links).

- [ ] **Step 2: Write `index.html`**

Semantic HTML using the exact copy and links from Global Constraints. Structure:
- `<head>`: UTF-8, viewport, `<title>Sarah Spear — Senior Fullstack Engineer</title>`, meta description, `<link rel="stylesheet" href="/css/site.css">`, a favicon link is optional. NO analytics, NO `http://` URLs.
- `<header class="hero">`: text column (`.eyebrow` "SENIOR FULLSTACK ENGINEER", `<h1>Sarah Spear</h1>`, `.lead` tagline, `.meta` personality line, actions: primary "Explore Cosmic Explorer" → live demo, ghost "GitHub" → github, ghost "LinkedIn" → linkedin) and photo column (`img.headshot` `src="/images/headshot.jpg"` `alt="Sarah Spear"`).
- `<section class="featured">`: label "FEATURED PROJECT · LIVE", `<h2>🪐 Cosmic Explorer</h2>`, description (from spec), tech-tag pills (FastAPI, React, Recharts, NASA data), actions (Live demo → live, Source on GitHub → source), and `<div class="shot"><img src="/images/cosmic-explorer.png" alt="Cosmic Explorer — exoplanet size-families explorer"></div>`.
- `<footer>`: "© 2026 Sarah Spear" + GitHub/LinkedIn links.
- All links to external sites use `https://` and `rel="noopener"` with `target="_blank"` for social/source.

- [ ] **Step 3: Verify locally in a browser**

Open the file directly:
```bash
open index.html   # macOS; or drag into a browser
```
Confirm: hero renders with the photo in the accent-ring circle; exact tagline + personality copy; four working links (hover/click GitHub, LinkedIn, live demo, source); featured section shows description + tags + buttons + screenshot panel; page **stacks cleanly when the window is narrowed** (mobile); browser console shows **no errors**; view-source shows **no `http://`** asset URLs and **no analytics** snippet.

- [ ] **Step 4: Commit**

```bash
git add index.html css/site.css
git commit -m "feat(site): modern light single-page portfolio featuring Cosmic Explorer"
```

---

## Task 3: Capture and wire the Cosmic Explorer screenshot

**Files:** Create `images/cosmic-explorer.png`.

- [ ] **Step 1: Capture a screenshot of the live explorer**

Try a headless browser (downloads Chromium on first run):
```bash
npx --yes playwright@latest screenshot \
  --viewport-size=1280,900 --wait-for-timeout=3500 \
  https://sarahspeartheengineer.com/cosmic-explorer/ images/cosmic-explorer.png
```
If Playwright/network isn't available in this environment, STOP and report it — leave the styled placeholder panel from Task 2 and note that a screenshot should be dropped in later (the layout already holds without it). Do NOT fabricate an image.

- [ ] **Step 2: Verify the screenshot**

```bash
file images/cosmic-explorer.png && ls -lh images/cosmic-explorer.png
```
Expected: a valid PNG, non-trivial size (tens–hundreds of KB). Open `index.html` again and confirm the screenshot now fills the featured panel and looks right (charts/list visible, not a blank/error page). If it captured a blank or errored page, retry with a longer `--wait-for-timeout`.

- [ ] **Step 3: Commit**

```bash
git add images/cosmic-explorer.png
git commit -m "feat(site): add live Cosmic Explorer screenshot to featured project"
```

---

## Task 4: Final review, push, and PR

- [ ] **Step 1: Full local pass**

Re-open `index.html`; final check against the spec's Verification list (copy exact, links resolve, responsive, no http/analytics, no console errors). Grep to be sure nothing stale remains:
```bash
grep -rn "http://" index.html css/ ; grep -rn "google-analytics\|UA-" . --include=*.html
```
Expected: no matches.

- [ ] **Step 2: Push the branch and open a PR**

```bash
git push -u origin feat/portfolio-refresh
gh pr create --repo ssspear/ssspear.github.io --base master \
  --title "feat: modern portfolio refresh featuring Cosmic Explorer" \
  --body "Rewrites the dated 2019 site as a clean light single-page portfolio featuring Cosmic Explorer (live demo + source + screenshot); new headshot; removes lorem-ipsum blog, dead analytics, and http URLs. Merging to master auto-deploys to sarahspeartheengineer.com."
```

- [ ] **Step 3: Post-merge verification (after the user merges)**

Confirm `https://sarahspeartheengineer.com/` serves the new page (hard-refresh / cache-bust), the photo and screenshot load, and all four links work.

---

## Deferred (Linear/optional)

Dark-mode/cosmic variant, restyled `404.html`, favicon + OpenGraph card, more projects later.
