# HTML Template Reference

Complete CSS design system and HTML structure for tariff guide documents. All
styles are embedded -- no external dependencies.

## CSS Design System

```css
/* ===== RESET & BASE ===== */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

:root {
  --slate-50: #f8fafc;
  --slate-100: #f1f5f9;
  --slate-200: #e2e8f0;
  --slate-300: #cbd5e1;
  --slate-400: #94a3b8;
  --slate-500: #64748b;
  --slate-600: #475569;
  --slate-700: #334155;
  --slate-800: #1e293b;
  --slate-900: #0f172a;
  --blue-50: #eff6ff;
  --blue-100: #dbeafe;
  --blue-200: #bfdbfe;
  --blue-400: #60a5fa;
  --blue-500: #3b82f6;
  --blue-600: #2563eb;
  --blue-700: #1d4ed8;
  --amber-50: #fffbeb;
  --amber-100: #fef3c7;
  --amber-400: #fbbf24;
  --amber-500: #f59e0b;
  --amber-600: #d97706;
  --green-50: #f0fdf4;
  --green-100: #dcfce7;
  --green-400: #4ade80;
  --green-500: #22c55e;
  --green-600: #16a34a;
  --red-50: #fef2f2;
  --red-100: #fee2e2;
  --red-400: #f87171;
  --red-500: #ef4444;
  --red-600: #dc2626;
  --purple-50: #faf5ff;
  --purple-500: #a855f7;
  --purple-600: #9333ea;
  --content-width: 800px;
  --sidebar-width: 280px;
}

html { scroll-behavior: smooth; font-size: 16px; }

body {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto,
    'Helvetica Neue', Arial, sans-serif;
  color: var(--slate-800);
  background: #fff;
  line-height: 1.7;
  -webkit-font-smoothing: antialiased;
}
```

## Layout

```css
.page-wrapper { display: flex; min-height: 100vh; }

/* Sidebar: fixed left, full height, scrollable */
.sidebar {
  position: fixed; top: 0; left: 0;
  width: var(--sidebar-width); height: 100vh;
  overflow-y: auto; background: var(--slate-50);
  border-right: 1px solid var(--slate-200);
  padding: 24px 16px; font-size: 0.82rem; z-index: 100;
}

/* Main content: offset by sidebar width */
.main-content {
  margin-left: var(--sidebar-width); flex: 1;
  padding: 48px 48px 96px;
  max-width: calc(var(--content-width) + 96px + var(--sidebar-width));
}
```

## Sidebar Navigation

```css
.sidebar-title {
  font-size: 0.75rem; font-weight: 700; text-transform: uppercase;
  letter-spacing: 0.08em; color: var(--slate-400);
  margin-bottom: 16px; padding-bottom: 12px;
  border-bottom: 1px solid var(--slate-200);
}
.sidebar a {
  display: block; color: var(--slate-600); text-decoration: none;
  padding: 5px 8px; border-radius: 4px; transition: all 0.15s;
  line-height: 1.4;
}
.sidebar a:hover { color: var(--blue-600); background: var(--blue-50); }
.sidebar .nav-section {
  font-weight: 600; color: var(--slate-800);
  margin-top: 14px; padding-top: 10px;
  border-top: 1px solid var(--slate-200);
}
.sidebar .nav-section:first-of-type {
  border-top: none; margin-top: 0; padding-top: 0;
}
.sidebar .nav-sub { padding-left: 12px; font-size: 0.78rem; }
```

## Typography

```css
h1 {
  font-size: 2.25rem; font-weight: 800; color: var(--slate-900);
  line-height: 1.2; margin-bottom: 8px; letter-spacing: -0.02em;
}
.subtitle {
  font-size: 1.1rem; color: var(--slate-500);
  margin-bottom: 32px; line-height: 1.5;
}
.meta {
  display: flex; gap: 24px; flex-wrap: wrap;
  font-size: 0.85rem; color: var(--slate-500);
  margin-bottom: 48px; padding-bottom: 32px;
  border-bottom: 1px solid var(--slate-200);
}
h2 {
  font-size: 1.65rem; font-weight: 700; color: var(--slate-900);
  margin: 56px 0 20px; padding-top: 32px;
  border-top: 1px solid var(--slate-200); letter-spacing: -0.01em;
}
h2:first-of-type { border-top: none; padding-top: 0; margin-top: 0; }
h3 {
  font-size: 1.25rem; font-weight: 600; color: var(--slate-900);
  margin: 40px 0 12px;
}
h4 {
  font-size: 1.05rem; font-weight: 600; color: var(--slate-700);
  margin: 28px 0 10px;
}
.section-intro {
  font-size: 1.05rem; color: var(--slate-600);
  margin-bottom: 32px; line-height: 1.7;
}
```

## Tables

```css
.table-wrap { overflow-x: auto; margin: 16px 0 24px; }
table { width: 100%; border-collapse: collapse; font-size: 0.9rem; }
th {
  background: var(--slate-800); color: #fff; font-weight: 600;
  text-align: left; padding: 10px 14px; white-space: nowrap;
  font-size: 0.82rem; text-transform: uppercase; letter-spacing: 0.04em;
}
td {
  padding: 10px 14px; border-bottom: 1px solid var(--slate-200);
  vertical-align: top;
}
tr:nth-child(even) td { background: var(--slate-50); }
tr:hover td { background: var(--blue-50); }
```

## Callout Boxes

Four types with consistent structure:

```css
.callout {
  border-left: 4px solid; border-radius: 0 8px 8px 0;
  padding: 16px 20px; margin: 20px 0 24px; font-size: 0.92rem;
}
.callout-title {
  font-weight: 700; font-size: 0.8rem; text-transform: uppercase;
  letter-spacing: 0.06em; margin-bottom: 6px;
  display: flex; align-items: center; gap: 6px;
}
.callout-info    { border-color: var(--blue-500);  background: var(--blue-50); }
.callout-warning { border-color: var(--amber-500); background: var(--amber-50); }
.callout-tip     { border-color: var(--green-500); background: var(--green-50); }
.callout-important { border-color: var(--red-500); background: var(--red-50); }
```

HTML usage:
```html
<div class="callout callout-info">
  <div class="callout-title">Key Difference from U.S.</div>
  <p>Description of the difference.</p>
</div>
```

Use info (blue) for structural differences, warning (amber) for cautions and
upcoming changes, tip (green) for comparisons and practical advice, important
(red) for critical differences and gotchas.

## Formula Box

Dark gradient background for formulas and calculations:

```css
.formula-box {
  background: linear-gradient(135deg, var(--slate-800), var(--slate-900));
  color: #fff; padding: 24px 28px; border-radius: 8px;
  margin: 20px 0 24px;
  font-family: 'SF Mono', 'Fira Code', 'Consolas', monospace;
  font-size: 0.88rem; line-height: 1.8;
}
.formula-box .formula-title {
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  font-weight: 700; font-size: 0.8rem; text-transform: uppercase;
  letter-spacing: 0.06em; color: var(--blue-400); margin-bottom: 12px;
}
```

## Worked Example Box

```css
.example-box {
  border: 1px solid var(--slate-200); border-radius: 8px;
  margin: 20px 0 28px; overflow: hidden;
}
.example-header {
  background: var(--slate-800); color: #fff;
  padding: 12px 20px; font-weight: 600; font-size: 0.92rem;
}
.example-body { padding: 20px; }
.example-result {
  background: var(--slate-50); border-top: 1px solid var(--slate-200);
  padding: 16px 20px; font-weight: 600; font-size: 0.95rem;
}
.example-result .amount { color: var(--red-600); font-size: 1.1rem; }
```

HTML usage:
```html
<div class="example-box">
  <div class="example-header">Example 1: Product Description</div>
  <div class="example-body">
    <p><strong>Tariff Code:</strong> XXXX.XX.XX | <strong>Value:</strong> $X</p>
    <div class="table-wrap">
      <table>...</table>
    </div>
  </div>
  <div class="example-result">
    Total: <span class="amount">$X</span> | Effective: <span class="amount">X%</span>
  </div>
</div>
```

## Tariff Anatomy Diagram

```css
.hts-diagram {
  background: var(--slate-50); border: 1px solid var(--slate-200);
  border-radius: 8px; padding: 24px; margin: 20px 0 24px;
  font-family: 'SF Mono', 'Fira Code', 'Consolas', monospace;
  font-size: 0.85rem; line-height: 1.5;
  white-space: pre; overflow-x: auto;
}
```

## Badges / Status Chips

```css
.badge {
  display: inline-block; font-size: 0.72rem; font-weight: 600;
  padding: 2px 8px; border-radius: 999px;
  text-transform: uppercase; letter-spacing: 0.04em;
}
.badge-blue   { background: var(--blue-100);   color: var(--blue-700); }
.badge-amber  { background: var(--amber-100);  color: var(--amber-600); }
.badge-green  { background: var(--green-100);  color: var(--green-600); }
.badge-red    { background: var(--red-100);    color: var(--red-600); }
.badge-purple { background: var(--purple-50);  color: var(--purple-600); }
```

Use for status indicators: `<span class="badge badge-red">Active</span>`,
`<span class="badge badge-green">Repealed</span>`,
`<span class="badge badge-amber">Pending</span>`.

## Print Stylesheet

```css
@media print {
  .sidebar { display: none !important; }
  .main-content {
    margin-left: 0 !important; padding: 0 !important;
    max-width: 100% !important;
  }
  body { font-size: 11pt; color: #000; }
  h1 { font-size: 22pt; }
  h2 {
    font-size: 16pt; page-break-before: always;
    border-top: 2px solid #000; padding-top: 12pt;
  }
  h2:first-of-type { page-break-before: avoid; }
  h3 { font-size: 13pt; }
  pre, .formula-box {
    background: #f5f5f5 !important; color: #000 !important;
    border: 1px solid #ccc;
  }
  .callout { border-left-width: 3px; }
  .callout-info { background: #f0f4ff !important; border-color: #333 !important; }
  .callout-warning { background: #fffbe6 !important; border-color: #666 !important; }
  .callout-tip { background: #f0fff0 !important; border-color: #333 !important; }
  .callout-important { background: #fff0f0 !important; border-color: #666 !important; }
  table { font-size: 9pt; }
  th {
    background: #333 !important; color: #fff !important;
    -webkit-print-color-adjust: exact; print-color-adjust: exact;
  }
  tr:nth-child(even) td {
    background: #f5f5f5 !important;
    -webkit-print-color-adjust: exact; print-color-adjust: exact;
  }
  a { color: #000 !important; text-decoration: underline; }
  .example-header {
    background: #333 !important;
    -webkit-print-color-adjust: exact; print-color-adjust: exact;
  }
  .badge { border: 1px solid #999; }
}
```

## Responsive

```css
@media (max-width: 1024px) {
  .sidebar { display: none; }
  .main-content { margin-left: 0; padding: 24px 20px 64px; }
}
```

## HTML Document Structure

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>The Complete Guide to [Country] Import Tariffs</title>
<style>
  /* All CSS from above embedded here */
</style>
</head>
<body>

<nav class="sidebar">
  <div class="sidebar-title">Table of Contents</div>
  <a href="#executive-summary" class="nav-section">Executive Summary</a>
  <a href="#ch1" class="nav-section">Ch 1: Overview</a>
  <a href="#ch1-sub" class="nav-sub">Subsection</a>
  <!-- ... more nav items ... -->
</nav>

<div class="main-content">
  <h1>The Complete Guide to [Country] Import Tariffs</h1>
  <p class="subtitle">How [Country] Charges Duties on Imported Goods</p>
  <div class="meta">
    <span><strong>Version:</strong>&nbsp;[Month Year]</span>
    <span><strong>Purpose:</strong>&nbsp;Educational reference</span>
    <span><strong>Prerequisite:</strong>&nbsp;[if applicable]</span>
  </div>

  <h2 id="executive-summary">Executive Summary</h2>
  <!-- Content -->

  <h2 id="ch1">Chapter 1: [Title]</h2>
  <h3 id="ch1-sub">[Subsection]</h3>
  <!-- Content -->

  <!-- ... more chapters ... -->

  <h2 id="disclaimers">Disclaimers</h2>
  <!-- Disclaimers -->

  <p style="margin-top: 48px; padding-top: 24px;
     border-top: 1px solid var(--slate-200);
     color: var(--slate-400); font-size: 0.85rem;">
    For the latest information, consult: [links]
  </p>
</div>

</body>
</html>
```

## Sidebar Navigation Conventions

- Use `class="nav-section"` for chapter-level links (bold, with top border)
- Use `class="nav-sub"` for subsection links (indented, smaller font)
- Keep sidebar labels short (max ~30 characters)
- Use abbreviations: "Ch 1:", "Ex 1:", "&amp;" for "and"
- First nav-section gets no top border (`:first-of-type` rule)

## Anchor ID Conventions

- Chapters: `id="ch1"`, `id="ch2"`, etc.
- Subsections: `id="ch1-topic"`, `id="ch2-topic"`, etc.
- Special sections: `id="executive-summary"`, `id="cheat-sheet"`,
  `id="disclaimers"`
- Worked examples: `id="ch7-ex1"`, `id="ch7-ex2"`, `id="ch7-ex3"`
- No spaces in IDs -- use hyphens
