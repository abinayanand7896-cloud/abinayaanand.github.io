# Stripe-Level Theme Revamp — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Revamp the Jekyll ML tutorial site to a Stripe-level premium dark theme — full-width dark homepage with gradient mesh hero, and a dark-sidebar/white-content tutorial reading experience.

**Architecture:** Override just-the-docs' `_layouts/default.html` with a custom shell (topbar + sidebar + content + TOC). Inject all design via `assets/css/custom.css` linked from `_includes/head_custom.html`. Homepage suppresses sidebar/TOC via a `.page-home` body class applied with Liquid. Tutorial `.md` files are untouched — they render inside the new layout automatically.

**Tech Stack:** Jekyll, just-the-docs remote theme, HTML, CSS custom properties, Liquid templates, Inter (Google Fonts, already loaded), vanilla JS (TOC builder).

---

## File Map

| File | Action | Responsibility |
|---|---|---|
| `assets/css/custom.css` | Create | Design tokens, all component styles, just-the-docs overrides |
| `_includes/head_custom.html` | Modify | Link `custom.css`, add Inter weight range |
| `_includes/sidebar_custom.html` | Create | Sidebar nav HTML — all 23 tutorials, 3 tracks |
| `_layouts/default.html` | Create | Page shell: topbar, sidebar, main, TOC, JS |
| `index.html` | Modify | Rewrite `<style>` block + hero/cards HTML |

---

### Task 1: CSS design tokens

**Files:**
- Create: `assets/css/custom.css`

- [ ] **Step 1: Create the file**

Create `assets/css/custom.css` with this full content:

```css
/* ═══════════════════════════════════════════════
   AI FROM THE GROUND UP — Design System
   Stripe-level dark theme
═══════════════════════════════════════════════ */

/* ── Override just-the-docs CSS variables ── */
:root {
  --body-background-color: #060a14;
  --sidebar-color:         #060a14;
  --nav-width:             252px;
  --body-text-color:       #334155;
  --body-heading-color:    #0f172a;
  --link-color:            #6366f1;

  /* ── Design tokens ── */
  --bg-page:    #060a14;
  --bg-surface: #0d1220;
  --bg-content: #ffffff;

  --border-dark:  rgba(255, 255, 255, 0.07);
  --border-light: #e2e8f0;

  --text-primary: #f8fafc;
  --text-muted:   #64748b;
  --text-reading: #334155;

  --indigo:       #6366f1;
  --indigo-light: #a5b4fc;
  --purple:       #a855f7;
  --purple-light: #d8b4fe;
  --rose:         #f43f5e;
  --sky:          #38bdf8;

  --sidebar-active-bg:     rgba(99, 102, 241, 0.08);
  --sidebar-active-border: #6366f1;
  --sidebar-active-text:   #a5b4fc;

  --topbar-height: 56px;
  --ease:          0.18s ease;
}

/* ── Reduced motion ── */
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
  }
}
```

- [ ] **Step 2: Verify**

```bash
ls ~/abinayanand7896-cloud.github.io/assets/css/
```

Expected: `custom.css` listed.

- [ ] **Step 3: Commit**

```bash
cd ~/abinayanand7896-cloud.github.io
git add assets/css/custom.css
git commit -m "feat: add CSS design tokens for Stripe-level theme"
```

---

### Task 2: Link custom.css

**Files:**
- Modify: `_includes/head_custom.html`

- [ ] **Step 1: Replace the full file**

Write `_includes/head_custom.html` with this content (keeps KaTeX, expands Inter weights, adds custom.css):

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Inter:ital,opsz,wght@0,14..32,300;0,14..32,400;0,14..32,500;0,14..32,600;0,14..32,700;0,14..32,800;0,14..32,900;1,14..32,400&display=swap" rel="stylesheet">
<link rel="stylesheet" href="{{ '/assets/css/custom.css' | relative_url }}">
<link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.css" crossorigin="anonymous">
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/katex.min.js" crossorigin="anonymous"></script>
<script defer src="https://cdn.jsdelivr.net/npm/katex@0.16.9/dist/contrib/auto-render.min.js" crossorigin="anonymous"
  onload="renderMathInElement(document.body, {
    delimiters: [
      {left: '$$', right: '$$', display: true},
      {left: '$', right: '$', display: false}
    ],
    throwOnError: false
  });"></script>
```

- [ ] **Step 2: Commit**

```bash
git add _includes/head_custom.html
git commit -m "feat: link custom.css and extend Inter font weights"
```

---

### Task 3: Sidebar include

**Files:**
- Create: `_includes/sidebar_custom.html`

- [ ] **Step 1: Create the file with all 23 tutorials**

Create `_includes/sidebar_custom.html`:

```html
<div class="sidebar-section">
  <div class="sidebar-section-label sidebar-label--ml">
    <svg width="9" height="9" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polyline points="22 7 13.5 15.5 8.5 10.5 2 17"/><polyline points="16 7 22 7 22 13"/></svg>
    ML Basics
  </div>
  <a class="sidebar-link{% if page.url contains 'what-is-ml' %} active{% endif %}" href="{{ '/tutorials/what-is-ml' | relative_url }}"{% if page.url contains 'what-is-ml' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>What is Machine Learning?</a>
  <a class="sidebar-link{% if page.url contains 'foundations' %} active{% endif %}" href="{{ '/tutorials/foundations' | relative_url }}"{% if page.url contains 'foundations' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>ML Foundations</a>
  <a class="sidebar-link{% if page.url contains 'linear-regression' %} active{% endif %}" href="{{ '/tutorials/linear-regression' | relative_url }}"{% if page.url contains 'linear-regression' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>Linear Regression</a>
  <a class="sidebar-link{% if page.url contains 'logistic-regression' %} active{% endif %}" href="{{ '/tutorials/logistic-regression' | relative_url }}"{% if page.url contains 'logistic-regression' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>Logistic Regression</a>
  <a class="sidebar-link{% if page.url contains 'decision-tree' %} active{% endif %}" href="{{ '/tutorials/decision-tree' | relative_url }}"{% if page.url contains 'decision-tree' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>Decision Trees</a>
  <a class="sidebar-link{% if page.url contains 'random-forest' %} active{% endif %}" href="{{ '/tutorials/random-forest' | relative_url }}"{% if page.url contains 'random-forest' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>Random Forests</a>
  <a class="sidebar-link{% if page.url contains 'gradient-boosting' %} active{% endif %}" href="{{ '/tutorials/gradient-boosting' | relative_url }}"{% if page.url contains 'gradient-boosting' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>Gradient Boosting</a>
  <a class="sidebar-link{% if page.url contains 'xgboost' %} active{% endif %}" href="{{ '/tutorials/xgboost' | relative_url }}"{% if page.url contains 'xgboost' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>XGBoost</a>
  <a class="sidebar-link{% if page.url contains '/svm' %} active{% endif %}" href="{{ '/tutorials/svm' | relative_url }}"{% if page.url contains '/svm' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>Support Vector Machines</a>
  <a class="sidebar-link{% if page.url contains '/knn' %} active{% endif %}" href="{{ '/tutorials/knn' | relative_url }}"{% if page.url contains '/knn' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>K-Nearest Neighbors</a>
  <a class="sidebar-link{% if page.url contains 'naive-bayes' %} active{% endif %}" href="{{ '/tutorials/naive-bayes' | relative_url }}"{% if page.url contains 'naive-bayes' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>Naive Bayes</a>
  <a class="sidebar-link{% if page.url contains '/pca' %} active{% endif %}" href="{{ '/tutorials/pca' | relative_url }}"{% if page.url contains '/pca' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>PCA</a>
  <a class="sidebar-link{% if page.url contains 'kmeans' %} active{% endif %}" href="{{ '/tutorials/kmeans' | relative_url }}"{% if page.url contains 'kmeans' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>K-Means Clustering</a>
  <a class="sidebar-link{% if page.url contains 'intermediate' %} active{% endif %}" href="{{ '/tutorials/intermediate' | relative_url }}"{% if page.url contains 'intermediate' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>What's Next?</a>
</div>

<div class="sidebar-divider"></div>

<div class="sidebar-section">
  <div class="sidebar-section-label sidebar-label--dl">
    <svg width="9" height="9" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><polygon points="12 2 2 7 12 12 22 7 12 2"/><polyline points="2 17 12 22 22 17"/><polyline points="2 12 12 17 22 12"/></svg>
    Deep Learning
  </div>
  <a class="sidebar-link{% if page.url contains 'neural-networks-intro' %} active{% endif %}" href="{{ '/tutorials/neural-networks-intro' | relative_url }}"{% if page.url contains 'neural-networks-intro' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>What are Neural Networks?</a>
  <a class="sidebar-link{% if page.url contains '/mlp' %} active{% endif %}" href="{{ '/tutorials/mlp' | relative_url }}"{% if page.url contains '/mlp' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>MLP</a>
  <a class="sidebar-link{% if page.url contains '/deep-learning' and page.url contains 'deep-learning-track' == false %} active{% endif %}" href="{{ '/tutorials/deep-learning' | relative_url }}"><span class="sidebar-dot"></span>What is Deep Learning?</a>
  <a class="sidebar-link{% if page.url contains '/cnn' %} active{% endif %}" href="{{ '/tutorials/cnn' | relative_url }}"{% if page.url contains '/cnn' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>CNN</a>
  <a class="sidebar-link{% if page.url contains '/rnn' %} active{% endif %}" href="{{ '/tutorials/rnn' | relative_url }}"{% if page.url contains '/rnn' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>RNN</a>
  <a class="sidebar-link{% if page.url contains '/lstm' %} active{% endif %}" href="{{ '/tutorials/lstm' | relative_url }}"{% if page.url contains '/lstm' %} aria-current="page"{% endif %}><span class="sidebar-dot"></span>LSTM</a>
</div>

<div class="sidebar-divider"></div>

<div class="sidebar-section">
  <div class="sidebar-section-label sidebar-label--soon">
    <svg width="9" height="9" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><circle cx="12" cy="12" r="10"/><polyline points="12 6 12 12 16 14"/></svg>
    Coming Soon
  </div>
  <span class="sidebar-link sidebar-link--disabled"><span class="sidebar-dot"></span>NLP</span>
  <span class="sidebar-link sidebar-link--disabled"><span class="sidebar-dot"></span>Transformers</span>
  <span class="sidebar-link sidebar-link--disabled"><span class="sidebar-dot"></span>LLMs &amp; GPT</span>
</div>
```

- [ ] **Step 2: Commit**

```bash
git add _includes/sidebar_custom.html
git commit -m "feat: add sidebar navigation include with all 23 tutorials"
```

---

### Task 4: Default layout override

**Files:**
- Create: `_layouts/default.html`

- [ ] **Step 1: Create `_layouts/` directory**

```bash
mkdir -p ~/abinayanand7896-cloud.github.io/_layouts
```

- [ ] **Step 2: Create `_layouts/default.html`**

```html
---
---
<!DOCTYPE html>
<html lang="{{ site.lang | default: 'en-US' }}">
  {% include head.html %}
  <body class="{% if page.url == '/' or page.url == '/index.html' %}page-home{% endif %}">

    <header class="site-topbar">
      <a class="topbar-logo" href="{{ '/' | relative_url }}">AI&nbsp;<span>Ground&nbsp;Up</span></a>
      <div class="topbar-search" role="search" aria-label="Search tutorials">
        <svg width="13" height="13" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" aria-hidden="true"><circle cx="11" cy="11" r="8"/><line x1="21" y1="21" x2="16.65" y2="16.65"/></svg>
        <span>Search tutorials…</span>
        <kbd>⌘K</kbd>
      </div>
      <nav class="topbar-nav" aria-label="Site navigation">
        <a class="topbar-link" href="https://github.com/abinayanand7896-cloud" target="_blank" rel="noopener noreferrer">GitHub</a>
        <a class="topbar-cta" href="{{ '/tutorials/what-is-ml' | relative_url }}">Start →</a>
      </nav>
    </header>

    <div class="page-shell">
      <nav class="sidebar-nav" aria-label="Tutorial navigation">
        {% include sidebar_custom.html %}
      </nav>

      <main class="main-content" id="main-content">
        {{ content }}
      </main>

      <aside class="toc-panel" aria-label="On this page">
        <div class="toc-label">On this page</div>
        <nav class="toc-nav" id="toc-nav"></nav>
      </aside>
    </div>

    <script>
      (function () {
        var tocNav = document.getElementById('toc-nav');
        if (!tocNav) return;
        var headings = document.querySelectorAll('.main-content h2, .main-content h3');
        if (headings.length < 2) {
          var panel = document.querySelector('.toc-panel');
          if (panel) panel.style.display = 'none';
          return;
        }
        headings.forEach(function (h) {
          if (!h.id) h.id = h.textContent.toLowerCase().replace(/[^a-z0-9]+/g, '-').replace(/(^-|-$)/g, '');
          var a = document.createElement('a');
          a.className = 'toc-link' + (h.tagName === 'H3' ? ' toc-link--h3' : '');
          a.href = '#' + h.id;
          a.textContent = h.textContent;
          tocNav.appendChild(a);
        });
        var obs = new IntersectionObserver(function (entries) {
          entries.forEach(function (e) {
            if (e.isIntersecting) {
              document.querySelectorAll('.toc-link').forEach(function (l) { l.classList.remove('active'); });
              var active = tocNav.querySelector('[href="#' + e.target.id + '"]');
              if (active) active.classList.add('active');
            }
          });
        }, { rootMargin: '-20% 0px -70% 0px' });
        headings.forEach(function (h) { obs.observe(h); });
      })();
    </script>

  </body>
</html>
```

- [ ] **Step 3: Commit**

```bash
git add _layouts/default.html
git commit -m "feat: add custom layout override with topbar, sidebar, content, TOC"
```

---

### Task 5: Topbar + sidebar + shell CSS

**Files:**
- Modify: `assets/css/custom.css` (append)

- [ ] **Step 1: Append the following to `assets/css/custom.css`**

```css
/* ═══════════════════════════════════════════════
   TOPBAR
═══════════════════════════════════════════════ */
.site-topbar {
  position: fixed;
  top: 0; left: 0; right: 0;
  z-index: 100;
  height: var(--topbar-height);
  background: var(--bg-page);
  border-bottom: 1px solid rgba(255,255,255,0.06);
  display: flex;
  align-items: center;
  padding: 0 20px;
  gap: 16px;
}

.topbar-logo {
  font-size: 14px;
  font-weight: 800;
  color: #f8fafc;
  letter-spacing: -0.03em;
  text-decoration: none;
  white-space: nowrap;
  flex-shrink: 0;
}
.topbar-logo span {
  background: linear-gradient(135deg, #818cf8, #c084fc);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.topbar-search {
  flex: 1;
  max-width: 280px;
  height: 32px;
  background: rgba(255,255,255,0.06);
  border: 1px solid rgba(255,255,255,0.08);
  border-radius: 8px;
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 0 12px;
  color: #475569;
  font-size: 12px;
  cursor: text;
  margin-left: auto;
}
.topbar-search kbd {
  margin-left: auto;
  background: rgba(255,255,255,0.08);
  border-radius: 4px;
  padding: 1px 6px;
  font-size: 10px;
  font-family: inherit;
  color: #334155;
  border: none;
}

.topbar-nav { display: flex; align-items: center; gap: 4px; flex-shrink: 0; }

.topbar-link {
  font-size: 12px; color: #64748b;
  padding: 6px 12px; text-decoration: none;
  border-radius: 7px;
  transition: color var(--ease);
}
.topbar-link:hover { color: #cbd5e1; }

.topbar-cta {
  font-size: 12px; font-weight: 600;
  color: var(--indigo-light);
  background: rgba(99,102,241,0.12);
  border: 1px solid rgba(99,102,241,0.2);
  padding: 6px 14px; border-radius: 7px;
  text-decoration: none;
  transition: all var(--ease);
}
.topbar-cta:hover { background: rgba(99,102,241,0.22); color: #fff; }

/* ═══════════════════════════════════════════════
   PAGE SHELL
═══════════════════════════════════════════════ */
.page-shell {
  display: flex;
  min-height: 100vh;
  padding-top: var(--topbar-height);
}

/* ═══════════════════════════════════════════════
   SIDEBAR
═══════════════════════════════════════════════ */
.sidebar-nav {
  width: var(--nav-width);
  flex-shrink: 0;
  background: var(--bg-page);
  border-right: 1px solid rgba(255,255,255,0.06);
  position: sticky;
  top: var(--topbar-height);
  height: calc(100vh - var(--topbar-height));
  overflow-y: auto;
  padding: 20px 0;
}
.sidebar-nav::-webkit-scrollbar { width: 4px; }
.sidebar-nav::-webkit-scrollbar-thumb { background: #1e293b; border-radius: 4px; }

.sidebar-section { margin-bottom: 4px; }

.sidebar-section-label {
  font-size: 9.5px;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  padding: 12px 20px 6px;
  display: flex;
  align-items: center;
  gap: 6px;
}
.sidebar-label--ml   { color: rgba(99,102,241,0.75); }
.sidebar-label--dl   { color: rgba(168,85,247,0.75); }
.sidebar-label--soon { color: rgba(244,63,94,0.5); }

.sidebar-link {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 7px 20px;
  font-size: 12.5px;
  color: #64748b;
  text-decoration: none;
  border-left: 2px solid transparent;
  transition: color var(--ease), background var(--ease), border-color var(--ease);
  line-height: 1.4;
}
.sidebar-link:hover { color: #cbd5e1; background: rgba(255,255,255,0.03); }
.sidebar-link.active {
  color: var(--sidebar-active-text);
  background: var(--sidebar-active-bg);
  border-left-color: var(--sidebar-active-border);
  font-weight: 600;
}
.sidebar-link--disabled { opacity: 0.38; pointer-events: none; cursor: default; }

.sidebar-dot {
  width: 5px; height: 5px;
  border-radius: 50%;
  background: currentColor;
  opacity: 0.35;
  flex-shrink: 0;
}
.sidebar-link.active .sidebar-dot { background: var(--indigo); opacity: 1; }

.sidebar-divider {
  height: 1px;
  background: rgba(255,255,255,0.04);
  margin: 12px 20px;
}

/* ═══════════════════════════════════════════════
   TOC PANEL
═══════════════════════════════════════════════ */
.toc-panel {
  width: 200px;
  flex-shrink: 0;
  background: #fafafa;
  border-left: 1px solid var(--border-light);
  padding: 56px 20px 20px;
  position: sticky;
  top: var(--topbar-height);
  height: calc(100vh - var(--topbar-height));
  overflow-y: auto;
}
.toc-label {
  font-size: 9.5px;
  font-weight: 700;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: #94a3b8;
  margin-bottom: 12px;
}
.toc-link {
  display: block;
  font-size: 11.5px;
  color: #94a3b8;
  padding: 5px 0 5px 10px;
  border-left: 2px solid #e2e8f0;
  text-decoration: none;
  line-height: 1.4;
  transition: all var(--ease);
}
.toc-link:hover { color: #475569; border-left-color: #c7d2fe; }
.toc-link.active { color: #4f46e5; border-left-color: var(--indigo); font-weight: 600; }
.toc-link--h3 { padding-left: 20px; font-size: 11px; }

/* ── Homepage: no sidebar / TOC ── */
.page-home .sidebar-nav,
.page-home .toc-panel { display: none; }
.page-home .main-content { max-width: 100%; padding: 0; background: var(--bg-page); }

/* ── Responsive ── */
@media (max-width: 768px) {
  .sidebar-nav, .toc-panel { display: none; }
  .topbar-search { display: none; }
}
```

- [ ] **Step 2: Commit**

```bash
git add assets/css/custom.css
git commit -m "feat: add topbar, sidebar, TOC, shell, and responsive CSS"
```

---

### Task 6: Content area CSS (tutorial reading pages)

**Files:**
- Modify: `assets/css/custom.css` (append)

- [ ] **Step 1: Append content area styles**

```css
/* ═══════════════════════════════════════════════
   MAIN CONTENT — tutorial reading area
═══════════════════════════════════════════════ */
.main-content {
  flex: 1;
  min-width: 0;
  background: var(--bg-content);
}

/* just-the-docs wraps rendered content in .page-content */
.main-content .page-content,
.main-content > article {
  max-width: 680px;
  margin: 0 auto;
  padding: 48px 40px 80px;
}

/* Headings */
.main-content h1 {
  font-size: 34px;
  font-weight: 800;
  color: #0f172a;
  letter-spacing: -0.035em;
  line-height: 1.12;
  margin: 0 0 14px;
}
.main-content h2 {
  font-size: 22px;
  font-weight: 700;
  color: #0f172a;
  letter-spacing: -0.025em;
  margin: 40px 0 12px;
  line-height: 1.3;
}
.main-content h3 {
  font-size: 17px;
  font-weight: 700;
  color: #1e293b;
  letter-spacing: -0.015em;
  margin: 28px 0 10px;
}

/* Body text */
.main-content p {
  font-size: 15.5px;
  line-height: 1.8;
  color: var(--text-reading);
  margin-bottom: 16px;
}
/* Lead — first p after h1 */
.main-content h1 + p {
  font-size: 16.5px;
  color: #64748b;
  line-height: 1.75;
  margin-bottom: 32px;
}

/* Links */
.main-content a {
  color: var(--indigo);
  text-decoration: none;
  border-bottom: 1px solid rgba(99,102,241,0.25);
  transition: border-color var(--ease);
}
.main-content a:hover { border-bottom-color: var(--indigo); }

/* Blockquote / callout */
.main-content blockquote {
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-left: 3px solid var(--indigo);
  border-radius: 10px;
  padding: 16px 18px;
  margin: 24px 0;
}
.main-content blockquote p { font-size: 14px; color: #475569; margin: 0; line-height: 1.65; }
.main-content blockquote strong { color: #0f172a; }

/* Inline code */
.main-content code {
  background: #f1f5f9;
  border: 1px solid #e2e8f0;
  border-radius: 4px;
  padding: 1px 6px;
  font-size: 13px;
  color: #c026d3;
}

/* Code blocks */
.main-content pre {
  background: #0f172a !important;
  border-radius: 12px !important;
  padding: 20px !important;
  margin: 20px 0 !important;
  border: 1px solid rgba(255,255,255,0.06) !important;
}
.main-content pre code {
  background: none !important;
  border: none !important;
  padding: 0 !important;
  color: #e2e8f0 !important;
  font-size: 13px;
  line-height: 1.7;
}

/* HR */
.main-content hr { border: none; height: 1px; background: #f1f5f9; margin: 32px 0; }

/* Tables */
.main-content table { width: 100%; border-collapse: collapse; font-size: 14px; margin: 24px 0; }
.main-content th { background: #f8fafc; font-weight: 600; color: #0f172a; padding: 10px 14px; border-bottom: 2px solid #e2e8f0; text-align: left; }
.main-content td { padding: 9px 14px; border-bottom: 1px solid #f1f5f9; color: #334155; }
.main-content tr:hover td { background: #fafafa; }

/* Lists */
.main-content ul, .main-content ol { padding-left: 22px; margin-bottom: 16px; }
.main-content li { font-size: 15.5px; line-height: 1.75; color: var(--text-reading); margin-bottom: 4px; }

@media (max-width: 768px) {
  .main-content .page-content,
  .main-content > article { padding: 32px 20px 60px; }
  .main-content h1 { font-size: 26px; }
}
```

- [ ] **Step 2: Commit**

```bash
git add assets/css/custom.css
git commit -m "feat: add content area and tutorial reading page CSS"
```

---

### Task 7: Homepage hero rewrite

**Files:**
- Modify: `index.html`

- [ ] **Step 1: Replace the entire `<style>` block**

In `index.html`, replace everything from `<style>` through `</style>` with:

```html
<style>
/* ── Homepage hero ── */
.page-hero {
  position: relative;
  overflow: hidden;
  text-align: center;
  padding: 100px 28px 0;
}
.page-hero::before {
  content: '';
  position: absolute;
  width: 900px; height: 900px;
  top: -300px; left: 50%;
  transform: translateX(-50%);
  background: radial-gradient(ellipse at center,
    rgba(99,102,241,0.38) 0%,
    rgba(139,92,246,0.20) 30%,
    transparent 70%);
  pointer-events: none;
}
.page-hero::after {
  content: '';
  position: absolute;
  width: 600px; height: 600px;
  bottom: -60px; right: -80px;
  background: radial-gradient(ellipse at center, rgba(56,189,248,0.12) 0%, transparent 70%);
  pointer-events: none;
}
.hero-orb-left {
  position: absolute;
  width: 400px; height: 400px;
  bottom: 0; left: -80px;
  background: radial-gradient(ellipse at center, rgba(244,63,94,0.09) 0%, transparent 70%);
  pointer-events: none;
}
.hero-grid {
  position: absolute; inset: 0;
  background-image:
    linear-gradient(rgba(99,102,241,0.06) 1px, transparent 1px),
    linear-gradient(90deg, rgba(99,102,241,0.06) 1px, transparent 1px);
  background-size: 64px 64px;
  mask-image: radial-gradient(ellipse 80% 60% at 50% 0%, black, transparent);
  -webkit-mask-image: radial-gradient(ellipse 80% 60% at 50% 0%, black, transparent);
  pointer-events: none;
}
.hero-inner { position: relative; z-index: 2; }

.hero-eyebrow {
  display: inline-flex;
  align-items: center;
  gap: 7px;
  background: rgba(99,102,241,0.10);
  border: 1px solid rgba(99,102,241,0.22);
  color: #a5b4fc;
  font-size: 11px; font-weight: 600;
  letter-spacing: 0.07em; text-transform: uppercase;
  padding: 5px 14px; border-radius: 100px;
  margin-bottom: 28px;
}
.eyebrow-pulse {
  width: 5px; height: 5px;
  border-radius: 50%;
  background: #818cf8;
  flex-shrink: 0;
  animation: pulse-dot 2s ease-in-out infinite;
}
@keyframes pulse-dot {
  0%, 100% { opacity: 1; }
  50%       { opacity: 0.3; }
}

.page-hero h1 {
  font-size: clamp(34px, 6vw, 64px);
  font-weight: 900;
  color: #f8fafc;
  margin: 0 0 18px;
  letter-spacing: -0.04em;
  line-height: 1.06;
}
.gradient-text {
  background: linear-gradient(135deg, #818cf8 0%, #c084fc 50%, #38bdf8 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}

.page-hero > .hero-inner > p {
  font-size: 17px; color: #94a3b8;
  margin: 0 auto 36px;
  max-width: 480px; line-height: 1.7;
}

.hero-actions {
  display: flex; gap: 12px;
  justify-content: center; flex-wrap: wrap;
}
.hero-cta-primary {
  display: inline-flex; align-items: center; gap: 8px;
  background: #6366f1; color: #fff;
  padding: 13px 26px; border-radius: 10px;
  font-size: 14px; font-weight: 600;
  text-decoration: none;
  box-shadow: 0 0 0 1px rgba(99,102,241,.5),
              0 4px 16px rgba(99,102,241,.35),
              inset 0 1px 0 rgba(255,255,255,.15);
  transition: all 0.15s ease;
}
.hero-cta-primary:hover {
  background: #4f46e5;
  box-shadow: 0 0 0 1px rgba(99,102,241,.6),
              0 8px 24px rgba(99,102,241,.45),
              inset 0 1px 0 rgba(255,255,255,.15);
  transform: translateY(-1px);
}
.hero-cta-ghost {
  display: inline-flex; align-items: center;
  background: rgba(255,255,255,0.04); color: #94a3b8;
  padding: 12px 22px; border-radius: 10px;
  font-size: 14px; font-weight: 500;
  text-decoration: none;
  border: 1px solid rgba(255,255,255,0.08);
  transition: all 0.15s ease;
}
.hero-cta-ghost:hover { background: rgba(255,255,255,0.07); color: #cbd5e1; }

.hero-stats {
  display: flex; justify-content: center;
  margin-top: 56px;
  border-top: 1px solid rgba(255,255,255,0.06);
  position: relative; z-index: 2;
}
.hero-stat {
  padding: 24px 36px;
  border-right: 1px solid rgba(255,255,255,0.06);
  text-align: center;
}
.hero-stat:last-child { border-right: none; }
.hero-stat-num {
  font-size: 28px; font-weight: 800;
  letter-spacing: -0.04em; line-height: 1;
  margin-bottom: 6px;
  background: linear-gradient(135deg, #f1f5f9, #94a3b8);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  background-clip: text;
}
.hero-stat-label {
  font-size: 10.5px; color: #475569;
  text-transform: uppercase; letter-spacing: 0.08em;
  font-weight: 500;
}

/* ── Track sections ── */
.homepage-content { max-width: 1000px; margin: 0 auto; padding: 56px 24px 80px; }
.track-section { margin-bottom: 56px; }
.track-header {
  display: flex; align-items: center; gap: 12px;
  margin-bottom: 16px; padding-bottom: 14px;
  border-bottom: 1px solid rgba(255,255,255,0.06);
}
.track-badge {
  display: inline-flex; align-items: center; gap: 5px;
  font-size: 10px; font-weight: 700;
  text-transform: uppercase; letter-spacing: 0.1em;
  padding: 4px 12px; border-radius: 100px; border: 1px solid;
}
.badge-ml   { background: rgba(99,102,241,.10); border-color: rgba(99,102,241,.25); color: #a5b4fc; }
.badge-dl   { background: rgba(168,85,247,.10);  border-color: rgba(168,85,247,.25);  color: #d8b4fe; }
.badge-soon { background: rgba(244,63,94,.08);   border-color: rgba(244,63,94,.20);   color: #fda4af; }

.track-title { font-size: 14px; font-weight: 600; color: #94a3b8; letter-spacing: -0.01em; }
.track-count { font-size: 12px; color: #334155; margin-left: auto; font-weight: 500; }

/* ── Cards ── */
.cards-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(160px, 1fr));
  gap: 12px;
}
.tcard {
  border-radius: 14px;
  background: rgba(255,255,255,0.025);
  border: 1px solid rgba(255,255,255,0.07);
  text-decoration: none;
  display: block;
  padding: 20px 16px 16px;
  position: relative;
  overflow: hidden;
  transition: border-color 0.2s ease, transform 0.2s ease, box-shadow 0.2s ease;
  cursor: pointer;
}
.tcard::before {
  content: '';
  position: absolute; inset: 0;
  border-radius: inherit;
  opacity: 0;
  transition: opacity 0.2s ease;
  pointer-events: none;
}
.tcard.ml::before   { background: linear-gradient(135deg, rgba(99,102,241,.08) 0%, transparent 60%); }
.tcard.dl::before   { background: linear-gradient(135deg, rgba(168,85,247,.08) 0%, transparent 60%); }
.tcard.soon::before { background: linear-gradient(135deg, rgba(244,63,94,.05) 0%, transparent 60%); }
.tcard:hover::before { opacity: 1; }

.tcard.ml:hover   { border-color: rgba(99,102,241,.35); box-shadow: 0 8px 32px rgba(0,0,0,.3), 0 0 0 1px rgba(99,102,241,.15); transform: translateY(-2px); }
.tcard.dl:hover   { border-color: rgba(168,85,247,.35); box-shadow: 0 8px 32px rgba(0,0,0,.3), 0 0 0 1px rgba(168,85,247,.15); transform: translateY(-2px); }
.tcard.soon:hover { border-color: rgba(244,63,94,.25);  box-shadow: 0 8px 32px rgba(0,0,0,.3); transform: translateY(-2px); }
.tcard.soon { opacity: 0.5; pointer-events: none; }

.tcard-icon {
  width: 40px; height: 40px;
  border-radius: 10px;
  display: flex; align-items: center; justify-content: center;
  margin-bottom: 12px; flex-shrink: 0;
}
.tcard.ml   .tcard-icon { background: rgba(99,102,241,.15); color: #818cf8; }
.tcard.dl   .tcard-icon { background: rgba(168,85,247,.15); color: #c084fc; }
.tcard.soon .tcard-icon { background: rgba(244,63,94,.10);  color: #fda4af; }
.tcard-icon svg { width: 18px; height: 18px; stroke-width: 1.75; }

.tcard-name {
  font-weight: 600; font-size: 12px;
  color: #cbd5e1; line-height: 1.4;
  letter-spacing: -0.005em; margin-bottom: 6px;
}
.tcard-peek {
  font-size: 11px; color: #475569; line-height: 1.55;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

/* ── Removed old peek hover — always visible now ── */
.tcard-header { display: contents; }

@media (max-width: 600px) {
  .hero-stat { padding: 20px 16px; }
  .hero-actions { flex-direction: column; align-items: center; }
  .homepage-content { padding: 40px 16px 60px; }
}
</style>
```

- [ ] **Step 2: Replace the hero HTML**

In `index.html`, replace the block `<!-- ═══ HERO ═══ -->` through the closing `</div>` of `.page-hero` with:

```html
<!-- ═══ HERO ═══ -->
<div class="page-hero">
  <div class="hero-orb-left"></div>
  <div class="hero-grid"></div>
  <div class="hero-inner">
    <div class="hero-eyebrow">
      <div class="eyebrow-pulse"></div>
      Free · Open Source · No fluff
    </div>
    <h1>AI, explained<br><span class="gradient-text">from the ground up</span></h1>
    <p>Machine learning tutorials with real formulas, honest explanations, and working code. No hand-waving.</p>
    <div class="hero-actions">
      <a class="hero-cta-primary" href="{{ '/tutorials/what-is-ml' | relative_url }}">
        Start from scratch
        <svg width="14" height="14" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2.5" stroke-linecap="round" stroke-linejoin="round"><line x1="5" y1="12" x2="19" y2="12"/><polyline points="12 5 19 12 12 19"/></svg>
      </a>
      <a class="hero-cta-ghost" href="{{ '/tutorials/foundations' | relative_url }}">Browse all tutorials</a>
    </div>
    <div class="hero-stats">
      <div class="hero-stat"><div class="hero-stat-num">23</div><div class="hero-stat-label">Tutorials</div></div>
      <div class="hero-stat"><div class="hero-stat-num">3</div><div class="hero-stat-label">Tracks</div></div>
      <div class="hero-stat"><div class="hero-stat-num">100%</div><div class="hero-stat-label">Free</div></div>
    </div>
  </div>
</div>

<div class="homepage-content">
```

- [ ] **Step 3: Add closing div at the very end of index.html** (after the last `</div>` closing the last `.track-section`):

```html
</div><!-- /.homepage-content -->
```

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: rewrite homepage hero with Stripe gradient mesh design"
```

---

### Task 8: Homepage cards HTML cleanup

**Files:**
- Modify: `index.html`

The old cards use a `.tcard-header` wrapper div that is now unnecessary (new CSS uses `display: contents` on it). The peek is also no longer hover-only. The HTML structure already works with `display: contents` added in Task 7's CSS, so no per-card changes are required.

- [ ] **Step 1: Verify build is clean**

```bash
cd ~/abinayanand7896-cloud.github.io
bundle exec jekyll build 2>&1 | grep -E "error|warning|done"
```

Expected: line ending in `done in X seconds.` with no `error` lines.

- [ ] **Step 2: Serve locally and verify homepage**

```bash
bundle exec jekyll serve --livereload
```

Open `http://localhost:4000` and check:
- Background is dark navy (`#060a14`)
- Hero gradient mesh orbs are visible (indigo glow at top)
- H1 "from the ground up" has indigo→violet→sky gradient text
- Stats row sits below hero with hairline separator
- Cards are dark glass (dark background, faint border)
- Hovering a card shows the track-colored glow and 2px lift
- No sidebar on the homepage

- [ ] **Step 3: Verify a tutorial page**

Open `http://localhost:4000/tutorials/linear-regression` and check:
- Dark topbar with "AI Ground Up" gradient logo
- Dark sidebar (252px) with all tutorials listed, "Linear Regression" highlighted in indigo
- White content area, 680px wide, comfortable reading typography
- Right TOC panel with "On this page" populated from h2/h3 headings

- [ ] **Step 4: Commit**

```bash
git add -A
git commit -m "feat: verified Stripe-level theme — homepage and tutorial pages"
```

---

### Task 9: Push to GitHub Pages

- [ ] **Step 1: Final status check**

```bash
cd ~/abinayanand7896-cloud.github.io
git status
```

Expected: `nothing to commit, working tree clean`

- [ ] **Step 2: Push**

```bash
git push origin main
```

- [ ] **Step 3: Verify live site**

Wait ~60 seconds for GitHub Pages to rebuild, then open `https://abinayanand7896-cloud.github.io` and verify the dark theme is live.
