# UI Redesign — Clean Blue Theme — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Apply the Clean Blue visual theme to the entire site by updating `assets/css/custom.scss` and `index.html`.

**Architecture:** All styling lives in two files — `assets/css/custom.scss` for global tutorial page styles, and the inline `<style>` block in `index.html` for homepage-specific styles. No Jekyll theme files are touched; just-the-docs light sidebar is kept as-is.

**Tech Stack:** Jekyll + just-the-docs remote theme, SCSS, GitHub Pages

---

## File Structure

| File | Change |
|------|--------|
| `assets/css/custom.scss` | Replace existing 4-line file with full blue theme styles |
| `index.html` | Replace existing inline `<style>` block with blue hero + card styles |

---

### Task 1: Apply blue theme to tutorial pages (`assets/css/custom.scss`)

**Files:**
- Modify: `assets/css/custom.scss`

This replaces the existing minimal CSS with the full Clean Blue theme for all tutorial pages. Work from a new branch off main.

- [ ] **Step 1: Create a new branch**

```bash
git checkout main
git pull origin main
git checkout -b feature/ui-clean-blue
```

- [ ] **Step 2: Read the current file**

Read `assets/css/custom.scss` to confirm its current contents (should be ~14 lines of minimal styles).

- [ ] **Step 3: Write the new `assets/css/custom.scss`**

Replace the entire file with:

```scss
---
---

/* ── Base typography ─────────────────────────────── */
body {
  color: #334155;
  line-height: 1.75;
}

h1, h2, h3, h4 {
  color: #0f172a;
  font-weight: 800;
}

.main-content {
  max-width: 860px;
}

/* ── Tutorial section labels (## headings) ────────── */
.main-content h2 {
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #2563eb;
  margin-top: 2rem;
  margin-bottom: 0.6rem;
  border-bottom: none;
  padding-bottom: 0;
}

/* ── KaTeX formula box ───────────────────────────── */
.katex-display {
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 1rem 1.25rem;
  margin: 1rem 0;
  overflow-x: auto;
}

/* ── Plain-English callout (blockquote) ──────────── */
blockquote {
  background: #eff6ff;
  border-left: 4px solid #2563eb;
  border-radius: 0 6px 6px 0;
  padding: 0.75rem 1rem;
  color: #1e3a8a;
  font-style: normal;
  margin: 0.75rem 0;
}

blockquote strong {
  color: #1e40af;
}

blockquote p {
  margin: 0;
}

/* ── Collapsible derivation (<details>) ──────────── */
details {
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 6px;
  padding: 0.5rem 1rem;
  margin: 0.5rem 0;
}

details summary {
  color: #2563eb;
  font-weight: 600;
  font-size: 0.875rem;
  cursor: pointer;
  padding: 0.25rem 0;
}

details p,
details ul,
details ol {
  color: #475569;
  font-size: 0.9rem;
  margin-top: 0.5rem;
}

/* ── Code blocks ─────────────────────────────────── */
.highlight {
  background: #0f172a !important;
  border-radius: 8px;
  margin-bottom: 1.5rem;
}

.highlight pre {
  background: transparent;
  color: #e2e8f0;
  padding: 1rem 1.25rem;
}

/* Inline code */
code.highlighter-rouge {
  background: #f1f5f9;
  color: #1e40af;
  border-radius: 4px;
  padding: 0.1em 0.4em;
  font-size: 0.88em;
}

/* ── Nav buttons (prev / next) ───────────────────── */
.btn {
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  color: #475569 !important;
  border-radius: 6px;
  font-weight: 600;
  font-size: 0.875rem;
  padding: 0.5rem 1.1rem;
  text-decoration: none;
  display: inline-block;
  transition: background 0.15s, color 0.15s;
}

.btn:hover {
  background: #e2e8f0;
  color: #0f172a !important;
}

.btn-primary {
  background: #2563eb !important;
  border-color: #2563eb !important;
  color: #fff !important;
}

.btn-primary:hover {
  background: #1d4ed8 !important;
  border-color: #1d4ed8 !important;
  color: #fff !important;
}
```

- [ ] **Step 4: Verify the file was saved correctly**

Read `assets/css/custom.scss` and confirm the `## Tutorial section labels` block is present and the `.btn-primary` block is at the end.

- [ ] **Step 5: Commit**

```bash
git add assets/css/custom.scss
git commit -m "feat: apply Clean Blue theme to tutorial pages"
```

---

### Task 2: Update homepage styles (`index.html`)

**Files:**
- Modify: `index.html` (inline `<style>` block only — lines 7–134)

Replace the existing `<style>` block with the blue gradient hero and updated card styles. The HTML structure below the `</style>` tag is NOT changed.

- [ ] **Step 1: Read `index.html`**

Read the file to locate the opening `<style>` tag (line 7) and closing `</style>` tag (line 134). Confirm the HTML content (hero div, level sections, cards) starts at line 136.

- [ ] **Step 2: Replace the `<style>` block**

Replace lines 7–134 (the entire `<style>...</style>` block) with:

```html
<style>
/* ── Hero ─────────────────────────────────────────── */
.page-hero {
  background: linear-gradient(135deg, #1e40af 0%, #2563eb 100%);
  text-align: center;
  padding: 48px 24px 40px;
  border-bottom: none;
  border-radius: 0 0 12px 12px;
  margin-bottom: 36px;
}
.page-hero h1 {
  font-size: 28px;
  font-weight: 800;
  color: #fff;
  margin: 0 0 10px;
}
.page-hero p {
  font-size: 15px;
  color: #bfdbfe;
  margin: 0 0 20px;
}
.hero-cta {
  display: inline-block;
  background: #fff;
  color: #1d4ed8;
  padding: 10px 24px;
  border-radius: 8px;
  font-size: 14px;
  font-weight: 700;
  text-decoration: none;
  transition: background 0.2s;
}
.hero-cta:hover { background: #eff6ff; color: #1d4ed8; }

/* ── Level sections ───────────────────────────────── */
.level-section { margin-bottom: 40px; }

.level-header {
  display: flex;
  align-items: center;
  gap: 12px;
  margin-bottom: 16px;
  padding-bottom: 10px;
  border-bottom: 2px solid #e2e8f0;
}
.level-badge {
  font-size: 11px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  padding: 3px 10px;
  border-radius: 20px;
  color: #fff;
}
.badge-l1 { background: #2563eb; }
.badge-l2 { background: #1d4ed8; }
.badge-l3 { background: #1e40af; }

.level-title {
  font-size: 17px;
  font-weight: 700;
  color: #0f172a;
}
.level-count {
  font-size: 12px;
  color: #94a3b8;
  margin-left: auto;
}

/* ── Card grid ────────────────────────────────────── */
.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(150px, 1fr));
  gap: 14px;
}

/* ── Card ─────────────────────────────────────────── */
.tcard {
  border-radius: 10px;
  overflow: hidden;
  cursor: pointer;
  border: 1px solid #e2e8f0;
  border-top: 3px solid #2563eb;
  box-shadow: 0 1px 4px rgba(37,99,235,0.07);
  transition: box-shadow 0.2s, transform 0.2s;
  text-decoration: none;
  display: block;
  max-width: 200px;
  background: #f8fafc;
}
.tcard:hover {
  box-shadow: 0 6px 20px rgba(37,99,235,0.15);
  transform: translateY(-2px);
}
.tcard-header {
  padding: 18px 14px 14px;
  display: flex;
  flex-direction: column;
  align-items: center;
  text-align: center;
  min-height: 96px;
  justify-content: center;
}
.tcard-icon { font-size: 26px; margin-bottom: 7px; }
.tcard-name {
  font-weight: 700;
  font-size: 12px;
  color: #0f172a;
  line-height: 1.35;
}

/* Level colour modifiers */
.l1 .tcard-header { background: linear-gradient(135deg, #dbeafe, #eff6ff); }
.l2 .tcard-header { background: linear-gradient(135deg, #bfdbfe, #dbeafe); }
.l3 .tcard-header { background: linear-gradient(135deg, #93c5fd, #bfdbfe); }

/* Peek strip */
.tcard-peek {
  background: #fff;
  color: #475569;
  font-size: 11px;
  line-height: 1.5;
  padding: 0 12px;
  max-height: 0;
  overflow: hidden;
  transition: max-height 0.35s ease, padding 0.25s ease;
}
.tcard:hover .tcard-peek {
  max-height: 100px;
  padding: 10px 12px;
  border-top: 1px solid #e2e8f0;
}
</style>
```

- [ ] **Step 3: Verify the HTML below the style block is unchanged**

Read lines 135–145 of `index.html` and confirm the hero div and level sections are still intact and unchanged.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: apply Clean Blue theme to homepage hero and cards"
```

---

### Task 3: Push branch and merge

- [ ] **Step 1: Push the branch**

```bash
git push -u origin feature/ui-clean-blue
```

- [ ] **Step 2: Verify on GitHub**

Open `https://github.com/abinayanand7896-cloud/abinayanand7896-cloud.github.io` and confirm the branch appears under "branches".

- [ ] **Step 3: Create PR and merge**

Create a pull request from `feature/ui-clean-blue` → `main` and merge it. GitHub Pages will rebuild automatically (1–2 minutes). Visit `https://abinayanand7896-cloud.github.io` to verify the blue theme is live.
