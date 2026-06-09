# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A collection of self-contained, single-file HTML portfolio pieces — UX/product-design case studies (author: Neha Balasundaram). There is no build step, package manager, framework, or test suite. Each `.html` file is the entire deliverable: open it directly in a browser to view (e.g. `start ResumeAnalyser.html` on Windows).

The two case studies:

- **`ResumeAnalyser.html`** — "IKIQ Resume Analyzer". ~1170 lines. References real assets from the `images/` folder via `<img src="images/*.png">`.
- **`DashboardRedesign.html`** — "uplevel" dashboard redesign. ~775 lines but ~3MB on disk because all images are inlined as `data:image/...;base64` (no external dependency on `images/`). Reading it whole will exceed tool size limits — use Grep or read with `offset`/`limit`.

The two files do **not** share a design system or CSS — each is independent. Don't assume a class or variable from one exists in the other.

## Shared structure of each file

Everything lives in one file, in three contiguous blocks:

1. **`<style>` in `<head>`** — All CSS, driven by CSS custom properties in `:root`.
   - `ResumeAnalyser`: navy/blue/teal palette (`--navy`/`--blue`/`--teal`), ink grays `--ink`..`--ink4`, radii `--r`. Utility classes: typography (`.hero-h`, `.h1`/`.h2`/`.h3`, `.lead`, `.cap`), grids (`.g2`/`.g3`/`.g4`, `.g-55`/`.g-60`/`.g-40`), cards (`.card`, `.card-blue`, `.card-teal`, `.card-surface`), tags (`.tag` + `.t-blue`/`.t-teal`/`.t-ink`/`.t-navy`), textures (`.tex-grid`, `.tex-dot`).
   - `DashboardRedesign`: warm cream/sage/amber palette (`--cream*`, `--ink*`, `--accent*`, `--sage*`, `--amber*`, `--border*`, `--shadow*`).

2. **`<body>`** — A fixed scroll-progress bar, a fixed `<nav>` whose anchor links map 1:1 to `<section id="...">` blocks in document order, then the sections.
   - `ResumeAnalyser` sections: `title`, `brief`, `output`, `process`, `design`, `impl`, `impact`, `next`, `close`. Sections wrapped in `.wrap` and decorated with ASCII comment banners.
   - `DashboardRedesign` sections: `hero`, `problem`, `research`, `phases` (containing `phase1`/`phase2`/`phase3`), `shipped`, `metrics`, `reflection`.

3. **`<script>` block(s) at the end** — Site-wide scroll behaviors.
   - Both: scroll-progress bar + IntersectionObserver-based scroll-reveal.
   - `ResumeAnalyser` has an extra mid-page `<script>` (~line 719) driving a Chrome-dino-style canvas game in the `design` section (`#dgCanvas`).
   - `DashboardRedesign` has a scroll-spy: side dots (`.sp-dot`, observing `[data-section]`) and a floating phase bar (`#phase-bar`, `#ptab1..3`) that tracks `[data-phase]` overlap to highlight the dominant phase.

## Conventions to preserve when editing

- **Scroll-reveal**: elements that animate in on scroll get a reveal class — `rev` (with stagger delays `d1`..`d4`) in `ResumeAnalyser`, `reveal` (→ `.in` when visible) in `DashboardRedesign`. The observer adds the active class once and unobserves. New animated content must use the file's existing convention or it won't appear/animate.
- **Adding/removing a section**: keep the `<nav>` links in sync with section `id`s — active-link highlighting and smooth-scroll anchors depend on the mapping. In `DashboardRedesign` also keep `[data-section]` indices and `.sp-dot` count aligned, and `[data-phase]` values aligned with the phase tabs.
- **Images**: `ResumeAnalyser` points at `images/*.png` (e.g. `HeroImage.png`, `WhatShipped.png`). `DashboardRedesign` inlines images as base64 — to swap one, replace the `data:` string in place rather than adding an external reference. Some legacy `.ph` placeholder blocks (with a `.ph-note` like "Replace with 1.png …") may remain; these mark intended asset slots.
- Styling is utility-class first — reach for existing CSS variables and utility classes rather than inline styles or new rules where possible. Variables and classes are per-file; verify a name exists in the file you're editing.
