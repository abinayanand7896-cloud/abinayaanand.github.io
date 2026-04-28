# Stripe-Level Theme Revamp — Design Spec
**Date:** 2026-04-28
**Project:** AI From the Ground Up (abinayanand7896-cloud.github.io)
**Status:** Approved

---

## Overview

Full-site visual revamp of the Jekyll-based ML tutorial site to a Stripe-level premium dark theme. The homepage becomes a rich dark experience with gradient mesh hero; tutorial reading pages use a light content area with a persistent dark sidebar — the same pattern as Stripe Docs and Tailwind Docs.

---

## Scope

| Area | In Scope |
|---|---|
| Homepage (`index.html`) | Full CSS + HTML rewrite |
| Tutorial pages (`tutorials/*.md`) | Shell only — layout, sidebar, topbar, content wrapper |
| Tutorial content | Not touched — Markdown content unchanged |
| `_config.yml` / `Gemfile` | Not touched |
| `just-the-docs` theme | Kept as base; overridden via local `_layouts` and `_includes` |

---

## Architecture

### Files Created

```
_layouts/
  default.html          ← Overrides just-the-docs shell
                           (topbar + sidebar + content wrapper + TOC panel)

_includes/
  sidebar_custom.html   ← Sidebar HTML injected by default.html

assets/css/
  custom.css            ← All design tokens + component overrides
```

### Files Modified

```
_includes/head_custom.html   ← Add custom.css link + Inter font weights
index.html                   ← Rewrite CSS block + hero/card HTML
```

### How Jekyll Override Works

Jekyll always prefers files in the project over theme files. By placing `_layouts/default.html` in the project root, it replaces the theme's version. All `tutorials/*.md` files render inside our layout with zero per-file changes.

---

## Homepage Design

### Hero Section

- **Background:** `#060a14` (deep navy) with a 3-orb radial gradient mesh:
  - Indigo orb (`rgba(99,102,241,0.35)`) centered at top
  - Sky blue orb (`rgba(56,189,248,0.12)`) at bottom-right
  - Pink orb (`rgba(236,72,153,0.10)`) at bottom-left
- **Grid overlay:** Faint 64×64px perspective grid at 6% indigo opacity, masked to fade toward the bottom
- **Noise texture:** SVG fractalNoise at 3% opacity — same technique Stripe uses
- **Eyebrow pill:** `rgba(99,102,241,0.12)` background, indigo border, pulsing dot animation
- **H1:** Inter 900, `clamp(36px, 6vw, 64px)`, -0.04em tracking, gradient text on key phrase (indigo → violet → sky via `background-clip: text`)
- **Lead text:** 17px, `#94a3b8`, max-width 480px
- **Primary CTA:** `#6366f1` background, inner glow box-shadow, arrow icon
- **Ghost CTA:** `rgba(255,255,255,0.04)` background, 8% white border
- **Stats row:** 3 stats pinned at hero bottom with hairline separator at 6% white opacity; stat numbers use gradient text

### Tutorial Cards

- **Grid:** `repeat(auto-fill, minmax(160px, 1fr))`, 12px gap
- **Card background:** `rgba(255,255,255,0.025)` fill, `rgba(255,255,255,0.07)` border, 14px radius
- **Hover state:** Border brightens to track color, gradient inset glow via `::before` pseudo-element, 2px lift, `box-shadow` with track-colored glow
- **Icon container:** 40×40px, 10px radius, track-tinted background (indigo tint for ML, purple tint for DL)
- **Description:** Always visible (no hover-reveal), 2-line clamp, `#475569`
- **Track colors:**
  - ML Basics → indigo (`#6366f1`)
  - Deep Learning → purple (`#a855f7`)
  - Coming Soon → rose (`#f43f5e`), 40% opacity, pointer-events none

### Spacing

- Hero: 80px top padding, 64px bottom padding
- Between track sections: 48px
- Card gap: 12px
- Track header bottom margin: 20px

---

## Tutorial Page Shell Design

### Top Bar (dark, fixed, 56px)

- Background: `#060a14`, 1px bottom border at 6% white opacity
- Left: Site name — "AI **Ground Up**" with gradient on "Ground Up"
- Center: Search input styled to match dark theme, ⌘K hint
- Right: GitHub text link + indigo "Start →" pill button
- Spans full viewport width above the sidebar+content layout

### Sidebar (dark, 252px, sticky)

- Background: `#060a14` — visually continuous with topbar
- Section groups with tiny all-caps labels + track icon
- Each link: 12.5px, `#64748b`, 7px vertical padding, left-border accent
- **Active state:** `rgba(99,102,241,0.08)` background, `#6366f1` left border (2px), `#a5b4fc` text, 600 weight
- Dot indicator (5px circle) per link, indigo when active
- Coming Soon links: 40% opacity, not interactive
- Scrolls independently from content
- Section dividers: 1px hairline at 4% white opacity

### Content Area (white, fluid)

- Background: `#ffffff`
- Max-width: 680px, centered, 40px horizontal padding, 48px top padding
- **Breadcrumb:** 11.5px, `#94a3b8`, `›` separator
- **Track badge:** Colored pill (indigo for ML, purple for DL) matching homepage
- **H1:** 36px, 800 weight, -0.04em tracking, `#0f172a`
- **Lead paragraph:** 16.5px, `#64748b`, 1.75 line-height
- **Body text:** 15.5px, `#334155`, 1.8 line-height — optimized for reading
- **H2:** 22px, 700 weight, `#0f172a`, 40px top margin
- **Callout boxes:** `#f8fafc` background, `#e2e8f0` border, 3px `#6366f1` left border, 10px radius
- **Code blocks:** `#0f172a` background, 12px radius, syntax highlighted
- **Prev/Next nav:** Two bordered cards at page bottom, hover reveals indigo glow

### Right TOC Panel (light, 200px)

- Background: `#fafafa`, 1px left border `#f1f5f9`
- "On this page" label: 9.5px all-caps, `#94a3b8`
- TOC links: 11.5px, left-border indicator, active state in indigo
- Sticky to top while content scrolls

---

## Color System

```css
/* Backgrounds */
--bg-page:     #060a14;
--bg-surface:  #0d1220;
--bg-content:  #ffffff;

/* Borders */
--border-dark:  rgba(255,255,255,0.07);
--border-light: #e2e8f0;

/* Text */
--text-primary: #f8fafc;
--text-muted:   #64748b;
--text-reading: #334155;

/* Tracks */
--indigo: #6366f1;   /* ML Basics, primary CTA */
--purple: #a855f7;   /* Deep Learning */
--rose:   #f43f5e;   /* Coming Soon */
--sky:    #38bdf8;   /* gradient highlight */

/* Sidebar active */
--sidebar-active-bg:     rgba(99,102,241,0.08);
--sidebar-active-border: #6366f1;
--sidebar-active-text:   #a5b4fc;
```

---

## Typography

Font: **Inter** (already loaded). Add weights 300, 400, 500, 600, 700, 800, 900.

| Role | Size | Weight | Tracking | Color |
|---|---|---|---|---|
| Hero H1 | clamp(36px, 6vw, 64px) | 900 | -0.04em | `#f8fafc` + gradient |
| Content H1 | 36px | 800 | -0.04em | `#0f172a` |
| H2 (content) | 22px | 700 | -0.025em | `#0f172a` |
| Card title | 12px | 600 | -0.005em | `#cbd5e1` |
| Body (reading) | 15.5px | 400 | normal | `#334155` |
| Lead paragraph | 16.5px | 400 | normal | `#64748b` |
| Labels / badges | 10–11px | 700 | +0.1em | varies |
| Sidebar links | 12.5px | 400 active:600 | normal | `#64748b` active:`#a5b4fc` |

---

## Animations

All transitions: `0.15–0.22s ease`. No motion beyond:
- Card hover: `transform: translateY(-2px)`, border/shadow transition
- Sidebar link hover: color + background fade
- Button hover: slight brightness shift
- Eyebrow dot: `opacity` pulse at 2s infinite

`prefers-reduced-motion` respected — all transitions disabled.

---

## Accessibility

- All text contrast meets WCAG AA (4.5:1 minimum)
- `#a5b4fc` on `#060a14` = 8.1:1 — AAA
- `#334155` on `#ffffff` = 9.4:1 — AAA
- Focus rings preserved on all interactive elements
- Sidebar links are `<a>` elements with proper href
- Active page marked with `aria-current="page"`
- Cards are `<a>` elements (already the case in index.html)

---

## Constraints & Non-Goals

- We do NOT replace just-the-docs search functionality — we style it
- We do NOT implement a dark/light toggle on tutorial pages (light only)
- We do NOT modify tutorial Markdown content
- We do NOT change the URL structure or navigation order
- Mobile responsive at 375px, 768px, 1024px, 1440px breakpoints — sidebar collapses below 768px (just-the-docs handles this natively)
