# Template Structure Reference

Canonical HTML structure for each section of the SaaS Landing Theme. When generating a page, follow these patterns exactly — they encode the design system's spacing, hierarchy, and interaction patterns.

## Table of Contents
1. [Page Shell](#page-shell)
2. [Navigation](#navigation)
3. [Hero Section](#hero-section)
4. [Logo Marquee](#logo-marquee)
5. [Features Section](#features-section)
6. [Bento Grid Section](#bento-grid-section)
7. [Pricing Section](#pricing-section)
8. [Testimonials Section](#testimonials-section)
9. [CTA Section](#cta-section)
10. [Footer](#footer)
11. [Product Screenshot Mockup](#product-screenshot-mockup)

---

## Page Shell

Every page starts with this shell. Include the Google Fonts link and all CSS variables from the design system.

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>[Product Name] — [Tagline]</title>
  <link href="https://fonts.googleapis.com/css2?family=Instrument+Serif:ital@0;1&family=General+Sans:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    /* All CSS variables from design-system.md */
    /* Reset: * { margin:0; padding:0; box-sizing:border-box; } */
    /* Body: font-family, background, color, overflow-x, -webkit-font-smoothing */
    /* Noise texture: body::before */
    /* All keyframe animations */
    /* All component styles */
  </style>
</head>
<body>
  <!-- Sections go here in canonical order -->
  <script>
    /* Nav scroll detection */
    /* Intersection observer for scroll animations */
  </script>
</body>
</html>
```

---

## Navigation

```html
<nav id="nav">
  <div class="nav-inner">
    <a href="#" class="logo">[brandname]<span>.</span></a>
    <ul class="nav-links">
      <li><a href="#features">Features</a></li>
      <li><a href="#pricing">Pricing</a></li>
      <li><a href="#">Docs</a></li>
      <li><a href="#">Blog</a></li>
      <li><a href="#" class="btn btn-primary">Start Free Trial</a></li>
    </ul>
  </div>
</nav>
```

**Key behaviors:**
- Logo uses Instrument Serif, the period (`.`) after the name is colored with `--accent`
- Nav links get an animated underline on hover (`::after` pseudo-element)
- The last item is a CTA button (btn-primary)
- On scroll past 40px, add `.scrolled` class for glass-morphism effect
- Customize nav links based on user's product sections

---

## Hero Section

```html
<section class="hero">
  <!-- Badge -->
  <div class="hero-badge animate-up">
    <span class="dot"></span>
    [Status text, e.g., "Now in Public Beta — v0.9"]
  </div>

  <!-- Headline — one key word wrapped in <em> for accent styling -->
  <h1 class="animate-up animate-up-1">
    [Headline with one <em>accent word</em>]
  </h1>

  <!-- Subhead -->
  <p class="animate-up animate-up-2">[Value proposition in 1-2 sentences]</p>

  <!-- CTA buttons -->
  <div class="hero-actions animate-up animate-up-3">
    <a href="#" class="btn btn-accent btn-large">[Primary CTA] →</a>
    <a href="#" class="btn btn-outline btn-large">
      <!-- Optional play icon SVG for demo button -->
      [Secondary CTA]
    </a>
  </div>

  <!-- Social proof -->
  <div class="hero-social-proof animate-up animate-up-4">
    <div class="avatar-stack">
      <!-- 4 colored avatar circles with initials -->
      <div class="avatar" style="background:[color]">[XX]</div>
      <!-- ... -->
    </div>
    <span class="stars">★★★★★</span>
    <span>[Social proof text, e.g., "Loved by 500+ firms"]</span>
  </div>

  <!-- Product visual -->
  <div class="hero-visual animate-scale" style="animation-delay:0.5s">
    <!-- Floating stat cards (see below) -->
    <!-- Product screenshot mockup (see dedicated section) -->
  </div>
</section>
```

**Floating stat cards** — Position 2 cards absolutely relative to `.hero-visual`:
```html
<div class="float-card float-card-1">
  <div class="fc-label">[METRIC NAME]</div>
  <div class="fc-value green">[VALUE]</div>
  <div class="fc-change">↑ [change description]</div>
</div>
```
- `float-card-1`: `top: 40px; left: -30px;`
- `float-card-2`: `top: 80px; right: -30px; animation-delay: 1.5s;`

**Headline accent pattern:** Exactly one word in the H1 should be wrapped in `<em>` which renders in italic Instrument Serif with `--accent` color and a soft highlight bar behind it via `::after`.

---

## Logo Marquee

```html
<div class="marquee-section">
  <div class="marquee-label">Trusted by forward-thinking [industry]</div>
  <div class="marquee-track">
    <!-- 8 items + 8 duplicates for seamless loop -->
    <div class="marquee-item">
      <span class="m-icon" style="background:[color]22;color:[color]">[symbol]</span>
      [Company Name]
    </div>
    <!-- Repeat all 8 items for seamless loop -->
  </div>
</div>
```

**Important:** The marquee track must contain items duplicated exactly once (16 total for 8 brands) to create a seamless infinite scroll. Use Unicode geometric shapes for brand icons: ⬡ ◈ ◎ ◆ ▲ ● ◇ ■

---

## Features Section

```html
<section class="features" id="features">
  <div class="section-label">Features</div>
  <h2>[Section headline — keep under 8 words]</h2>
  <div class="features-grid">
    <!-- 6 feature cards -->
    <div class="feature-card">
      <div class="feature-icon fi-[1-6]">[emoji]</div>
      <h3>[Feature name]</h3>
      <p>[Feature description — 1-2 sentences, focus on benefit not feature]</p>
    </div>
    <!-- Repeat 5 more -->
  </div>
</section>
```

**Feature icon backgrounds** cycle through 6 soft colors:
```
fi-1: accent-soft    fi-2: purple-soft    fi-3: blue-soft
fi-4: #FFF8E1        fi-5: #ECFDF5        fi-6: #FFF0F6
```

Use contextual emojis that match the feature, not generic ones. For a CPA product: 🚀 📋 🤖 👥 📊 🔗. For a dev tool: ⚡ 🔧 🛡️ 📦 🔄 📈.

---

## Bento Grid Section

The bento grid uses a 12-column CSS grid with cards spanning variable widths.

```html
<section class="bento-section">
  <div class="bento-grid">
    <!-- Row 1: 7 + 5 -->
    <div class="bento-card span-7">
      <div class="section-label">[Category]</div>
      <h3>[Card headline]</h3>
      <p>[Description]</p>
      <!-- Visual content: mini-chart, screenshot, or illustration -->
    </div>
    <div class="bento-card span-5">
      <div class="section-label">[Category]</div>
      <h3>[Card headline]</h3>
      <p>[Description]</p>
      <!-- Visual content: stat-row or metrics -->
    </div>

    <!-- Row 2: 5 + 7 (reversed for visual interest) -->
    <div class="bento-card span-5">
      <!-- Workflow steps or list content -->
    </div>
    <div class="bento-card span-7" style="background: var(--bg-warm);">
      <!-- Use bg-warm for one card to add variety -->
    </div>
  </div>
</section>
```

### Bento Content Types

**Mini Chart:**
```html
<div class="mini-chart">
  <!-- 12 bars with varying heights, some with .active class for accent color -->
  <div class="chart-bar" style="height:40%"></div>
  <div class="chart-bar active" style="height:85%"></div>
  <!-- ... -->
</div>
```

**Stat Row:**
```html
<div class="stat-row">
  <div class="stat-item">
    <div class="stat-num" style="color:var(--accent)">4×</div>
    <div class="stat-label">[Label]</div>
  </div>
  <!-- 2-3 stat items -->
</div>
```

**Workflow Steps:**
```html
<div class="workflow-steps">
  <div class="workflow-step">
    <span class="step-num">1</span>
    [Step description]
    <span class="step-arrow">→</span>
  </div>
  <!-- Last step uses ✓ instead of → -->
</div>
```

---

## Pricing Section

```html
<section class="pricing" id="pricing">
  <div class="section-label" style="justify-content:center">Pricing</div>
  <h2>[Pricing headline]</h2>
  <p>[Pricing subhead — emphasize simplicity/transparency]</p>
  <div class="pricing-grid">
    <!-- Tier 1: Starter -->
    <div class="pricing-card">
      <div class="plan-name">[TIER NAME]</div>
      <div class="plan-price">[Price]<span>/user/mo</span></div>
      <div class="plan-desc">[Who this tier is for]</div>
      <ul class="plan-features">
        <li>[Feature — each gets a green ✓ via ::before]</li>
        <!-- 4-6 features -->
      </ul>
      <a href="#" class="btn btn-outline">Start Free Trial</a>
    </div>

    <!-- Tier 2: Professional (featured) -->
    <div class="pricing-card featured">
      <!-- Same structure, uses btn-accent instead of btn-outline -->
      <!-- Gets "Most Popular" badge via ::before pseudo-element -->
    </div>

    <!-- Tier 3: Enterprise -->
    <div class="pricing-card">
      <!-- Price can be "Custom" for enterprise -->
      <!-- CTA: "Contact Sales" -->
    </div>
  </div>
</section>
```

The middle card always gets the `.featured` class. It should have 1-2 more features than the starter tier.

---

## Testimonials Section

```html
<section class="testimonials">
  <div class="section-label" style="justify-content:center">[Label]</div>
  <h2>[Testimonials headline]</h2>
  <div class="testimonial-grid">
    <div class="testimonial-card">
      <div class="tc-stars">★★★★★</div>
      <div class="tc-quote">"[Testimonial text — 2-3 sentences, specific and credible]"</div>
      <div class="tc-author">
        <div class="tc-avatar" style="background:var(--accent)">[XX]</div>
        <div>
          <div class="tc-name">[Full Name]</div>
          <div class="tc-role">[Title, Company]</div>
        </div>
      </div>
    </div>
    <!-- 2 more testimonial cards -->
  </div>
</section>
```

**Testimonial writing guidelines:**
- Each quote should mention a specific benefit or metric (e.g., "set up in under a day", "no more status meetings")
- Vary the avatar background colors: use `--accent`, `--purple`, `--success`
- Make author names diverse and realistic
- Roles should be relevant to the target audience

---

## CTA Section

```html
<section class="cta">
  <div class="cta-inner">
    <!-- Radial gradient glow via ::before pseudo-element -->
    <h2>[Strong closing headline]</h2>
    <p>[Final push — mention free trial, no credit card, social proof count]</p>
    <div class="cta-actions">
      <a href="#" class="btn btn-white btn-large">[Primary CTA] →</a>
      <a href="#" class="btn btn-ghost btn-large">[Secondary CTA]</a>
    </div>
  </div>
</section>
```

The `.cta-inner` has a dark (`--text-primary`) background with rounded corners (24px) and a radial gradient glow in the accent color at low opacity. This creates a dramatic section break before the footer.

---

## Footer

```html
<footer>
  <div class="footer-top">
    <div class="footer-brand">
      <a href="#" class="logo">[brandname]<span>.</span></a>
      <p>[One-line brand description]</p>
    </div>
    <div class="footer-col">
      <h4>Product</h4>
      <a href="#">Features</a>
      <a href="#">Pricing</a>
      <a href="#">Integrations</a>
      <a href="#">Changelog</a>
      <a href="#">Roadmap</a>
    </div>
    <div class="footer-col">
      <h4>Resources</h4>
      <a href="#">Documentation</a>
      <a href="#">Blog</a>
      <a href="#">Guides</a>
      <a href="#">Webinars</a>
      <a href="#">API Reference</a>
    </div>
    <div class="footer-col">
      <h4>Company</h4>
      <a href="#">About</a>
      <a href="#">Careers</a>
      <a href="#">Contact</a>
      <a href="#">Privacy</a>
      <a href="#">Terms</a>
    </div>
  </div>
  <div class="footer-bottom">
    <span>© [Year] [Product Name]. All rights reserved.</span>
    <span>Made in [Location] [Flag Emoji]</span>
  </div>
</footer>
```

Customize footer columns based on the product. A dev tool might have "Developers" instead of "Resources". An enterprise product might have "Security" and "Compliance" links.

---

## Product Screenshot Mockup

This is the hero's main visual — a realistic browser-window mockup showing the product UI.

```html
<div class="hero-screenshot">
  <!-- Browser chrome -->
  <div class="screenshot-bar">
    <span class="screenshot-dot r"></span>
    <span class="screenshot-dot y"></span>
    <span class="screenshot-dot g"></span>
    <div class="screenshot-url">app.[domain].com/[page]</div>
  </div>
  <!-- App content -->
  <div class="screenshot-body">
    <div class="screenshot-sidebar">
      <!-- 6-8 nav items with emoji icons -->
      <div class="sidebar-item active"><span class="icon">[emoji]</span> [Active Page]</div>
      <div class="sidebar-item"><span class="icon">[emoji]</span> [Page Name]</div>
      <!-- ... -->
    </div>
    <div class="screenshot-main">
      <div class="main-header">
        <h3>[Page Title]</h3>
        <div class="filters">
          <span class="filter-chip active">[Active View]</span>
          <span class="filter-chip">[View 2]</span>
          <span class="filter-chip">[View 3]</span>
        </div>
      </div>
      <!-- Main content — adapt to product type -->
      <!-- For project management: kanban board -->
      <!-- For analytics: chart + data table -->
      <!-- For dev tools: code editor + terminal -->
    </div>
  </div>
</div>
```

The screenshot body should be contextual to the product. The default kanban board works for project management, practice management, and workflow tools. For other product types, adapt the main content area:

- **Analytics product:** Show a chart (mini-chart pattern) + summary cards
- **Dev tool:** Show a code block with syntax highlighting + output panel
- **CRM:** Show a contact list + detail sidebar
- **Communication tool:** Show an inbox/message list + compose area

The sidebar remains consistent across types — it's the universal SaaS navigation pattern.
