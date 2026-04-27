# UI Redesign — Clean Blue Theme

**Date:** 2026-04-27
**Scope:** Visual restyling of the entire site — homepage and all tutorial pages. No content changes. No structural changes to Jekyll or just-the-docs.

---

## Design Decisions

| Decision | Choice |
|----------|--------|
| Style direction | Clean Blue — white background, blue (#2563eb) accent |
| Sidebar | Light (default just-the-docs), active item highlighted blue |
| Homepage | Blue gradient hero + track cards with blue top-border |
| Tutorial sections | Uppercase blue labels, not H2 bold headers |
| Formula box | Light grey background with border, centred |
| Plain-English callout | Blue left-border on light-blue background |
| Code blocks | Dark background (#0f172a) with blue syntax highlights |
| Nav buttons | Secondary = grey outline, Primary = solid blue |

---

## Colour Palette

| Token | Hex | Usage |
|-------|-----|-------|
| `--blue-600` | `#2563eb` | Primary accent — badges, labels, borders, buttons |
| `--blue-700` | `#1d4ed8` | Hover states, gradient end |
| `--blue-800` | `#1e40af` | Gradient start in hero |
| `--blue-50` | `#eff6ff` | Callout background |
| `--blue-100` | `#dbeafe` | Light accent areas |
| `--blue-200` | `#bfdbfe` | Hero subtitle text |
| `--blue-300` | `#93c5fd` | Code block keyword colour |
| `--slate-50` | `#f8fafc` | Card/formula box background |
| `--slate-200` | `#e2e8f0` | Borders, dividers |
| `--slate-500` | `#64748b` | Secondary text |
| `--slate-700` | `#334155` | Body text |
| `--slate-900` | `#0f172a` | Headings, code block background |

---

## Implementation — `assets/css/custom.scss`

All changes live in one file. No theme files are modified.

### 1. Global typography & body

```scss
body {
  color: #334155;
  line-height: 1.75;
}

h1, h2, h3, h4 {
  color: #0f172a;
  font-weight: 800;
}
```

### 2. Tutorial section labels

Target each `## Section Name` heading inside `.main-content`:

```scss
.main-content h2 {
  font-size: 11px;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: #2563eb;
  margin-top: 2rem;
  margin-bottom: 0.6rem;
  border-bottom: none;
}
```

### 3. Formula box

The KaTeX display blocks (`.katex-display`) get a styled container:

```scss
.katex-display {
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 8px;
  padding: 1rem 1.25rem;
  margin: 1rem 0;
}
```

### 4. Plain-English callout (blockquote)

```scss
blockquote {
  background: #eff6ff;
  border-left: 4px solid #2563eb;
  border-radius: 0 6px 6px 0;
  padding: 0.75rem 1rem;
  color: #1e3a8a;
  font-style: normal;
  margin: 0.75rem 0;

  strong { color: #1e40af; }
}
```

### 5. Collapsible derivation (`<details>`)

```scss
details {
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  border-radius: 6px;
  padding: 0.5rem 1rem;
  margin: 0.5rem 0;

  summary {
    color: #2563eb;
    font-weight: 600;
    font-size: 0.85rem;
    cursor: pointer;
  }

  p, ul, ol { color: #475569; font-size: 0.9rem; }
}
```

### 6. Code blocks

```scss
.highlight {
  background: #0f172a !important;
  border-radius: 8px;
  margin: 0.75rem 0 1.5rem;

  pre { background: transparent; color: #e2e8f0; }
}

code.highlighter-rouge {
  background: #f1f5f9;
  color: #1e40af;
  border-radius: 4px;
  padding: 0.1em 0.4em;
  font-size: 0.88em;
}
```

### 7. Track badge (parent label in tutorials)

```scss
.label-blue {
  background: #2563eb;
  color: #fff;
  font-size: 10px;
  font-weight: 700;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  padding: 3px 10px;
  border-radius: 20px;
}
```

### 8. Nav buttons (prev/next at bottom of tutorials)

```scss
.btn {
  background: #f8fafc;
  border: 1px solid #e2e8f0;
  color: #475569;
  border-radius: 6px;
  font-weight: 600;
  font-size: 0.85rem;
  padding: 0.5rem 1.1rem;

  &:hover { background: #e2e8f0; color: #0f172a; }
}

.btn-primary {
  background: #2563eb;
  border-color: #2563eb;
  color: #fff;

  &:hover { background: #1d4ed8; border-color: #1d4ed8; color: #fff; }
}
```

### 9. Homepage hero

Applied via inline `<style>` in `index.html` (already exists there):

```css
.page-hero {
  background: linear-gradient(135deg, #1e40af 0%, #2563eb 100%);
  color: #fff;
  padding: 48px 24px 40px;
  border-bottom: none;
  border-radius: 0 0 12px 12px;
  margin-bottom: 36px;
}
.page-hero h1 { color: #fff; font-size: 28px; }
.page-hero p  { color: #bfdbfe; }
.hero-cta {
  background: #fff;
  color: #1d4ed8;
  font-weight: 700;
  border-radius: 8px;
  padding: 10px 24px;
}
.hero-cta:hover { background: #eff6ff; color: #1d4ed8; }
```

### 10. Track cards on homepage

```css
.tutorial-card {
  border-top: 3px solid #2563eb;
  border-radius: 8px;
  background: #f8fafc;
  border: 1px solid #e2e8f0;
}
.tutorial-card:hover {
  border-top-color: #1d4ed8;
  box-shadow: 0 4px 12px rgba(37,99,235,0.1);
}
.badge-active { background: #2563eb; }
.badge-coming { background: #94a3b8; }
```

---

## Out of Scope

- Dark mode toggle
- Font changes (system font stack is fine)
- Sidebar structural changes (just-the-docs light sidebar kept as-is)
- Any content changes
- Mobile layout changes beyond what the CSS naturally handles
