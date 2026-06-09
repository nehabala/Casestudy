# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A single, self-contained HTML portfolio piece: `ikiq-case-study-v4_3.html` — a UX/product-design case study for the "IKIQ Resume Analyzer" (author: Neha Balasundaram). There is no build step, package manager, framework, or test suite. To view it, open the file directly in a browser (e.g. `start ikiq-case-study-v4_3.html` on Windows).

## Structure of the single file

Everything lives in one file, in three contiguous blocks:

1. **`<style>` (top of `<head>`)** — All CSS. The design system is driven by CSS custom properties in `:root` (color palette `--navy`/`--blue`/`--teal`, ink grays `--ink`..`--ink4`, radii `--r`). Reusable utility classes follow: typography (`.hero-h`, `.h1`/`.h2`/`.h3`, `.lead`, `.cap`), layout grids (`.g2`/`.g3`/`.g4`, `.g-55`/`.g-60`/`.g-40` for asymmetric two-column splits), cards (`.card`, `.card-blue`, `.card-teal`, `.card-surface`), tags (`.tag` + `.t-blue`/`.t-teal`/`.t-ink`/`.t-navy`), and texture backgrounds (`.tex-grid`, `.tex-dot`).

2. **`<body>`** — Fixed `#prog` scroll-progress bar, a fixed `<nav>` whose anchor links map 1:1 to the page sections, then the content as a sequence of `<section id="...">` blocks in order: `title`, `brief`, `output`, `process`, `design`, `impl`, `impact`, `next`, `close`. Each section is wrapped in `.wrap` (max-width container) and decorated with ASCII comment banners.

3. **Two `<script>` blocks** — One mid-page (~line 652) drives a Chrome-dino-style canvas game embedded in the `design` section (`#dgCanvas`, `#dgc`, `#dg-start`/`#dg-over`). The final block (~line 1090) handles three site-wide behaviors: the scroll progress bar, IntersectionObserver-based nav highlighting (adds `.act` to the in-view section's link), and scroll-reveal animations.

## Conventions to preserve when editing

- **Scroll-reveal**: any element that should animate in on scroll gets class `rev`; stagger delays use `d1`..`d4`. The observer adds `.on` once and unobserves. New animated content must use these classes or it won't appear/animate consistently.
- **Adding/removing a section**: keep the `<nav>` links (around line 243) in sync with section `id`s — the active-link highlighting and smooth-scroll anchors depend on this mapping.
- **Placeholders**: image slots use `.ph` blocks with a `.ph-note` like "Replace with 1.png …", marking where real assets are intended to go.
- Styling is utility-class first; reach for existing CSS variables and utility classes rather than inline styles or new rules where possible.
