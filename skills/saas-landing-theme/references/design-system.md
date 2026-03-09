# Design System Reference

Complete design token specification for the SaaS Landing Theme. All values are canonical — use them exactly as specified unless the user provides brand overrides.

## Table of Contents
1. [Color Palette](#color-palette)
2. [Typography](#typography)
3. [Spacing & Layout](#spacing--layout)
4. [Shadows & Borders](#shadows--borders)
5. [Animations](#animations)
6. [Component Specs](#component-specs)
7. [Responsive Breakpoints](#responsive-breakpoints)

---

## Color Palette

### CSS Variables

```css
:root {
  /* Backgrounds */
  --bg: #FAFAF7;            /* Primary page background — warm off-white, NOT pure white */
  --bg-warm: #F5F0EB;       /* Accent background for highlighted sections */
  --bg-card: #FFFFFF;        /* Card surfaces */

  /* Text */
  --text-primary: #1A1A1A;  /* Headlines, nav, strong text */
  --text-secondary: #6B6B6B; /* Body text, descriptions */
  --text-muted: #9B9B9B;    /* Labels, metadata, captions */

  /* Accent — Vermillion Red (default, override with user's brand color) */
  --accent: #E8553D;
  --accent-soft: #FFF0ED;   /* Accent at ~8% opacity on white */
  --accent-hover: #D14830;  /* Accent darkened ~10% for hover states */

  /* Semantic Colors */
  --success: #2D8A56;
  --purple: #7C5CFC;
  --purple-soft: #F3F0FF;
  --blue: #3B82F6;
  --blue-soft: #EFF6FF;

  /* Borders */
  --border: #E8E4DF;        /* Standard borders */
  --border-light: #F0ECE7;  /* Subtle dividers */
}
```

### Color Usage Rules
- **Never use pure white `#FFFFFF` as a page background.** Always use `--bg: #FAFAF7` for warmth.
- **Cards use `#FFFFFF`** to create subtle lift against the warm background.
- **Accent color appears sparingly:** CTAs, badges, hover states, the hero italic underline. It should pop, not overwhelm.
- **Text hierarchy is strict:** `--text-primary` for headings, `--text-secondary` for body, `--text-muted` for metadata.

### Deriving Custom Accent Palettes
When the user provides a brand color, generate three variants:
```
--accent:       [provided color]
--accent-soft:  [provided color] mixed with white at 8-12% opacity
--accent-hover: [provided color] darkened by 10-15%
```

### Tag/Badge Color Map
Use these for status indicators, category tags, and badges:
```css
.tag-primary   { background: var(--accent-soft); color: var(--accent); }
.tag-purple    { background: var(--purple-soft); color: var(--purple); }
.tag-blue      { background: var(--blue-soft); color: var(--blue); }
.tag-green     { background: #ECFDF5; color: var(--success); }
.tag-yellow    { background: #FFF8E1; color: #D4A017; }
```

---

## Typography

### Font Stack
```css
/* Headlines — editorial serif with character */
font-family: 'Instrument Serif', serif;

/* Body — clean geometric sans */
font-family: 'General Sans', -apple-system, BlinkMacSystemFont, sans-serif;
```

### Google Fonts Import
```html
<link href="https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=General+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
```

### Type Scale

| Element              | Font Family        | Size                        | Weight | Letter-Spacing | Line-Height |
|----------------------|--------------------|-----------------------------|--------|----------------|-------------|
| Hero H1              | Instrument Serif   | `clamp(48px, 7vw, 82px)`   | 400    | -2px           | 1.05        |
| Section H2           | Instrument Serif   | `clamp(36px, 4vw, 52px)`   | 400    | -1.5px         | 1.1         |
| Bento Card H3        | Instrument Serif   | 28px                        | 400    | -0.5px         | 1.2         |
| Feature Card H3      | General Sans       | 18px                        | 700    | -0.3px         | 1.3         |
| Body Text            | General Sans       | 18px (hero), 14px (cards)   | 400    | 0              | 1.7         |
| Nav Links            | General Sans       | 14px                        | 500    | 0              | 1           |
| Section Label        | General Sans       | 12px                        | 700    | 2px            | 1           |
| Metadata / Captions  | General Sans       | 12-13px                     | 500-600| 0.5-1px        | 1           |
| Button Text          | General Sans       | 14px (std), 16px (large)    | 600    | 0              | 1           |

### Typography Rules
- **Hero headlines use Instrument Serif with italic `<em>` for the accent word.** The italic word gets colored with `--accent` and has a soft underline highlight behind it.
- **Section labels are all-caps General Sans** with a decorative line before them: `::before { width: 20px; height: 1.5px; background: var(--accent); }`
- **Never use Instrument Serif below 18px.** It's a display face — use General Sans for anything small.
- **Tight letter-spacing on large text** (-2px on hero, -1.5px on H2) gives headlines editorial weight.

---

## Spacing & Layout

### Max Widths
- Page container: `max-width: 1240px; margin: 0 auto;`
- Hero text: `max-width: 900px` (headline), `max-width: 560px` (subhead)
- CTA text: `max-width: 500px`
- Footer brand description: `max-width: 280px`

### Section Padding
```
Nav:            16px 40px
Hero:           160px 40px 100px (top accounts for fixed nav)
Marquee:        60px 0
Features:       120px 40px
Bento:          0 40px 120px
Pricing:        120px 40px
Testimonials:   80px 40px 120px
CTA:            0 40px 120px
Footer:         60px 40px 40px
```

### Grid Specs
```
Features:     grid-template-columns: repeat(3, 1fr); gap: 20px;
Bento:        grid-template-columns: repeat(12, 1fr); gap: 20px;
Pricing:      grid-template-columns: repeat(3, 1fr); gap: 20px;
Testimonials: grid-template-columns: repeat(3, 1fr); gap: 20px;
Footer:       grid-template-columns: 2fr repeat(3, 1fr); gap: 48px;
Kanban:       grid-template-columns: repeat(4, 1fr); gap: 14px;
```

### Border Radius Scale
```css
--radius: 16px;      /* Cards, screenshots */
--radius-sm: 10px;   /* Kanban cards, inner elements */
--radius-xs: 6px;    /* Tags, small chips */
/* Buttons and badges: border-radius: 100px (pill shape) */
```

---

## Shadows & Borders

### Shadow Scale
```css
--shadow-sm: 0 1px 2px rgba(0,0,0,0.04);        /* Subtle lift */
--shadow-md: 0 4px 20px rgba(0,0,0,0.06);        /* Card hover */
--shadow-lg: 0 12px 40px rgba(0,0,0,0.08);       /* Featured cards, floating elements */
--shadow-xl: 0 20px 60px rgba(0,0,0,0.1);        /* Hero screenshot */
```

### Border Rules
- Standard card border: `1px solid var(--border-light)`
- Stronger separator: `1px solid var(--border)`
- Featured pricing card: `border-color: var(--accent); box-shadow: 0 0 0 1px var(--accent), var(--shadow-lg);`
- Nav glass border (on scroll): `1px solid var(--border-light)`

### Noise Texture Overlay
Always include on `body::before`:
```css
body::before {
  content: '';
  position: fixed;
  inset: 0;
  opacity: 0.025;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='noise'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23noise)'/%3E%3C/svg%3E");
  pointer-events: none;
  z-index: 9999;
}
```

---

## Animations

### Keyframes

```css
@keyframes fadeUp {
  from { opacity: 0; transform: translateY(30px); }
  to { opacity: 1; transform: translateY(0); }
}

@keyframes fadeIn {
  from { opacity: 0; }
  to { opacity: 1; }
}

@keyframes scaleIn {
  from { opacity: 0; transform: scale(0.95); }
  to { opacity: 1; transform: scale(1); }
}

@keyframes float {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-8px); }
}

@keyframes pulse {
  0%, 100% { opacity: 1; }
  50% { opacity: 0.6; }
}

@keyframes marquee {
  0% { transform: translateX(0); }
  100% { transform: translateX(-50%); }
}
```

### Animation Usage

| Element                | Animation     | Duration | Easing   | Trigger              |
|------------------------|---------------|----------|----------|----------------------|
| Hero badge, H1, P, CTA| fadeUp        | 0.8s     | ease-out | Page load (staggered)|
| Hero screenshot        | scaleIn       | 0.8s     | ease-out | Page load (0.5s delay)|
| Feature/pricing cards  | fadeUp        | 0.6s     | ease-out | Scroll (IntersectionObserver) |
| Floating stat cards    | float         | 4s       | ease-in-out | Infinite loop     |
| Status dot (badge)     | pulse         | 2s       | ease     | Infinite loop        |
| Logo marquee           | marquee       | 30s      | linear   | Infinite loop        |

### Stagger Pattern for Hero
```css
.animate-up     { animation: fadeUp 0.8s ease-out both; }
.animate-up-1   { animation-delay: 0.1s; }
.animate-up-2   { animation-delay: 0.2s; }
.animate-up-3   { animation-delay: 0.3s; }
.animate-up-4   { animation-delay: 0.4s; }
```

### Hover Transitions
- **All interactive elements**: `transition: all 0.3s ease;`
- **Card hover**: `transform: translateY(-4px); box-shadow: var(--shadow-lg);`
- **Button hover**: `transform: translateY(-1px); box-shadow: var(--shadow-md);`
- **Feature card top border reveal**: `::before { transform: scaleX(0) → scaleX(1); }`
- **Nav link underline**: `::after { width: 0 → 100%; }`
- **Workflow steps**: `transform: translateX(4px);` on hover

### Scroll Animation JavaScript
```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.style.animationPlayState = 'running';
      entry.target.classList.add('animate-up');
    }
  });
}, { threshold: 0.1 });

document.querySelectorAll('.feature-card, .bento-card, .pricing-card, .testimonial-card')
  .forEach((el, i) => {
    el.style.opacity = '0';
    el.style.animation = `fadeUp 0.6s ease-out ${i * 0.08}s both`;
    el.style.animationPlayState = 'paused';
    observer.observe(el);
  });
```

### Nav Scroll Detection
```javascript
const nav = document.getElementById('nav');
window.addEventListener('scroll', () => {
  nav.classList.toggle('scrolled', window.scrollY > 40);
});
```

---

## Component Specs

### Buttons

| Variant      | Background           | Text Color           | Border                     | Hover Effect                              |
|--------------|----------------------|----------------------|----------------------------|-------------------------------------------|
| btn-primary  | `--text-primary`     | white                | none                       | bg: #333, translateY(-1px), shadow-md     |
| btn-accent   | `--accent`           | white                | none                       | bg: accent-hover, translateY(-1px), glow  |
| btn-outline  | transparent          | `--text-primary`     | 1.5px solid `--border`     | border-color: text-primary, translateY(-1px) |
| btn-white    | white                | `--text-primary`     | none                       | bg: #f0f0f0, translateY(-1px), shadow-lg  |
| btn-ghost    | transparent          | rgba(255,255,255,0.8)| 1.5px solid rgba(255,255,255,0.2) | border-color: 0.5, color: white |

All buttons: `border-radius: 100px; font-weight: 600; font-size: 14px; padding: 10px 22px;`
Large variant: `font-size: 16px; padding: 16px 36px;`

### Cards

**Feature Card:**
- Background: `--bg-card`, border: `1px solid --border-light`, radius: `--radius`
- Padding: 36px
- Top accent bar: `::before` with `height: 3px`, `scaleX(0)→scaleX(1)` on hover
- Icon container: 48×48px, radius 12px, with category-specific soft background

**Pricing Card:**
- Same base as feature card, padding: 40px
- Featured variant: accent border + `0 0 0 1px var(--accent)` box-shadow
- "Most Popular" badge: absolute positioned, `top: -12px`, pill shape, accent background

**Testimonial Card:**
- Same base, padding: 36px
- Stars: `#F5A623` color, 14px, `letter-spacing: 2px`
- Quote text: italic, `--text-secondary`
- Author: avatar (40×40 circle with initials) + name + role

**Bento Card:**
- 12-column grid, cards span 4-8 columns
- Uses Instrument Serif for H3 (28px)
- Can contain: mini charts, stat rows, workflow steps, or descriptive text

### Hero Screenshot (Product Mockup)
- Container: perspective 1200px, card has `rotateX(2deg)` → `rotateX(0)` on hover
- Browser bar: 3 dots (red/yellow/green) + centered URL bar
- Body: 220px sidebar + flexible main area
- Sidebar: navigation items with emoji icons
- Main: kanban board with 4 columns, cards with tags

### Nav
- Fixed, `z-index: 1000`
- Default: transparent background
- Scrolled: `rgba(250,250,247,0.85)` + `backdrop-filter: blur(20px) saturate(1.5)` + bottom border

---

## Responsive Breakpoints

### 1024px (Tablet)
```
Features/Pricing/Testimonial grids: 2 columns
Bento cards: all span 6 columns
Screenshot sidebar: hidden
Floating cards: hidden
Footer: 2 columns
```

### 640px (Mobile)
```
Nav links: hidden (could add hamburger menu)
Section padding: 20px horizontal
All grids: 1 column
Bento cards: all span 1 (full width)
Kanban board: 2 columns
CTA inner: 48px 24px padding
Hero actions: flex-direction column (stack buttons)
Footer: 1 column
```
