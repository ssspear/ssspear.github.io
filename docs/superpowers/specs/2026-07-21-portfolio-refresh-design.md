# Portfolio Refresh (sarahspeartheengineer.com) — Design

- **Date:** 2026-07-21
- **Repo:** `ssspear/ssspear.github.io` (plain static site, GitHub Pages deploy-from-`master`, custom domain `sarahspeartheengineer.com`)
- **Status:** Approved (brainstorm, via visual mockup) — ready for implementation planning.

## Overview

Replace the dated 2019 single-screen "hero card" with a modern, clean **single-page portfolio** that features one project prominently: **Cosmic Explorer**. Direction **B (clean light)** was chosen over a dark/cosmic option because it complements the headshot. Stays a no-build static site (HTML + CSS) that deploys on push to `master`.

## Goals

- A current, professional one-pager that reads as senior.
- New headshot, refreshed copy, updated title.
- **Cosmic Explorer** as the single big featured project (live demo + source links + screenshot).
- Remove stale cruft (lorem-ipsum blog, dead analytics, `http://` URLs).

## Non-goals

- No dark/cosmic theme (deferred/rejected in favor of the light direction).
- No additional projects, blog, or multi-page structure.
- No build tooling or framework — remains hand-written static HTML/CSS.

## Design (approved from the visual mockup)

**Layout:** single page, centered max-width container, responsive (columns stack on mobile).

**Palette / type:** white canvas (`#ffffff`), near-black text (`#111318`), one indigo accent (`#5b47e0`). System font stack. Generous whitespace.

**Hero:**
- Headshot in a **circular crop** (~236px), object-position ~`52% 22%` to center the face, `5px` white border, soft shadow, and a subtle violet→pink gradient accent ring.
- Eyebrow label: `SENIOR FULLSTACK ENGINEER` (indigo, letter-spaced).
- `h1`: **Sarah Spear**.
- Tagline (exact copy): *"I build full-stack products for the web, and point the occasional telescope at everything else."*
- Personality line (exact copy): *"Fullstack engineer · amateur astronomer · dog person · still a little short to be a stormtrooper · she/they"*
- Actions: primary **Explore Cosmic Explorer** (→ live demo), plus **GitHub** and **LinkedIn** ghost buttons.

**Featured project — Cosmic Explorer:**
- Section label `FEATURED PROJECT · LIVE`, heading "🪐 Cosmic Explorer".
- Description: interactive explorer for NASA exoplanet data — classifies 500+ confirmed worlds into size families (rocky, super-Earth, Neptune-like, gas giant) and surfaces patterns across our stellar neighborhood; FastAPI + React.
- Tech tags: FastAPI, React, Recharts, NASA data.
- Links: **Live demo** (`https://sarahspeartheengineer.com/cosmic-explorer/`) and **Source on GitHub** (`https://github.com/ssspear/cosmic-explorer`).
- A **screenshot** of the live explorer beside the copy.

**Links (verified current):**
- GitHub `https://github.com/ssspear` · LinkedIn `https://www.linkedin.com/in/spearsarah/`
- Dropped: Polling Stone blog, Twitter/X.

## Assets

- **Headshot:** new photo (provided) → committed to `images/` (replace the old `author.jpg` or add a new file; downsize the source ~500 KB to a web-appropriate size).
- **Cosmic Explorer screenshot:** capture from the live site (headless browser during implementation) or user-provided; committed to `images/`. Until captured, a styled placeholder panel.

## File structure / changes

- **Rewrite `index.html`** — new semantic single-page markup; remove the dead Universal Analytics snippet; use protocol-relative/`https` and root-relative asset paths.
- **New `css/site.css`** (or a small inline `<style>`) — the light design system above. Retire reliance on the old `css/main.css` theme.
- **`images/`** — add the new headshot + Cosmic Explorer screenshot.
- **Keep** `CNAME`, `404.html` (optionally restyle later).
- **Remove stale cruft:** `blog/` (lorem-ipsum posts), `feed.xml`, the old `about/` page + its hero link, unused `fonts/`/`icons` if the new page doesn't use them. (Scope removals to what's clearly dead; don't touch `CNAME`.)

## Verification

- Open the built `index.html` locally and check: hero + photo render, tagline copy exact, all four links resolve (GitHub, LinkedIn, live demo, source), responsive stacking at narrow widths, no `http://` mixed-content references, no console errors, no leftover analytics.
- After deploy (push to `master`): confirm `https://sarahspeartheengineer.com/` serves the new page and the demo/source links work.

## Deferred (optional follow-ups)

- Dark/cosmic theme variant; restyled 404; adding more projects later; a proper favicon/OG card for link previews.
