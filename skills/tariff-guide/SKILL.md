---
name: tariff-guide
description: >-
  Create comprehensive, printable tariff and customs guide documents for any
  country or customs territory. Use when a user asks to write a tariff guide,
  create a customs handbook, document an import duty system, build a trade
  compliance reference, or explain how tariffs work for a specific country.
  Produces both Markdown and self-contained HTML with professional styling,
  worked examples, comparison tables, and print-ready layout. Accepts a country
  or customs territory name and optional topic outline.
---

# Tariff Guide Generator

Create comprehensive, professional-grade tariff and customs guide documents for
any country or customs territory. Output both Markdown and self-contained HTML
with modern tech-doc styling.

## Input Handling

Determine what the user needs:

1. **Country/territory specified** -- User names a country or customs territory
   (e.g., "Japan", "ASEAN", "UK", "Australia"). Research and write the full
   guide.
2. **Outline provided** -- User provides a chapter outline. Use it as the
   structure and fill in with researched content.
3. **Update existing guide** -- User has an existing guide that needs updating.
   Research current developments and update the relevant sections.
4. **Comparison guide** -- User wants to compare two or more systems. Structure
   as a comparative document with side-by-side analysis.

If the user provides minimal context, ask:

- **Which country or customs territory?**
- **Target audience?** (e.g., U.S. importers entering this market, local
  compliance professionals, general trade audience)
- **Should it cross-reference existing guides?** (e.g., "assume reader knows
  the U.S. system")
- **Output format?** (default: both Markdown + self-contained HTML)

## Research Phase

Before writing, gather comprehensive information. Launch parallel research
agents covering these topic areas:

### Research Area 1: Structure and Institutions

- Governing legislation (customs act, tariff act, trade law)
- Key agencies (customs authority, trade tribunal, trade policy body, tax
  authority)
- Classification system (HS-based structure, national digits, how many digits)
- Tariff schedule structure (rate columns, preferential treatments)
- How to access the official tariff schedule (URLs, databases, tools)

### Research Area 2: Rates, Valuation, and Origin

- Valuation basis (FOB, CIF, or other -- this is a critical distinguishing
  feature)
- MFN tariff profile (average rates, rate ranges by sector)
- Free trade agreements (list all FTAs with partner names, years, codes)
- Rules of origin (non-preferential, preferential, proof of origin mechanisms)
- Preferential programs (GSP equivalents, developing country preferences)

### Research Area 3: Additional Duties and Taxes

- Anti-dumping and countervailing duty system (retrospective vs. prospective,
  current major measures)
- Safeguard measures (any active safeguards, TRQs)
- Special tariffs/surtaxes (retaliatory tariffs, national security tariffs,
  any equivalent to U.S. Section 301/232/IEEPA)
- Sales tax / VAT / GST at the border (rate, how calculated, recoverable?)
- Excise duties (alcohol, tobacco, fuel, luxury goods)
- Any unique mechanisms (e.g., EU CBAM, entry price systems)

### Research Area 4: Procedures and Compliance

- Customs declaration system (electronic system name, process)
- De minimis thresholds (duty-free, tax-free, e-commerce)
- Duty relief programs (drawback, bonded warehouses, temporary importation,
  processing zones)
- Trusted trader programs (AEO equivalent, mutual recognition)
- Binding classification rulings (process, validity, searchable database?)
- Penalties and enforcement (audit lookback period, penalty structure,
  voluntary disclosure, appeal process)

### Research Quality Requirements

- Use current data (specify the date in the guide)
- Include specific rates, thresholds, legal references, regulation numbers
- Include official website URLs and database links
- Verify data against official government sources where possible
- Note any upcoming changes (pending legislation, scheduled rate changes)

## Document Structure

Read [references/document-structure.md](references/document-structure.md) for
the standard chapter framework.

Every guide follows this core structure, adapted to the specific system:

### Required Sections

1. **Executive Summary** -- Structural comparison table vs. U.S. (or another
   reference system if specified). Highlight the 3-5 most consequential
   differences. This section tells the reader what makes this system unique.

2. **System Overview** -- Legislative foundation, key agencies with roles and
   website URLs, how the system is organized.

3. **Classification** -- How the tariff number works, digit breakdown diagram,
   national extensions, how to access the official schedule.

4. **Tariff Rates and Treatments** -- MFN rates, preferential rates, all FTA
   treatment codes, the "best rate" rule if applicable.

5. **Customs Valuation** -- FOB vs. CIF (critical distinction), transaction
   value method, additions/deductions, currency conversion.

6. **Rules of Origin** -- Non-preferential and preferential, key FTA rules of
   origin, proof of origin mechanisms.

7. **Total Landed Cost Calculation** -- The complete formula from customs value
   to total charges. Must include **3 worked examples** with step-by-step
   calculations showing realistic products and amounts.

8. **Additional Duties** -- AD/CVD system, safeguards, surtaxes, retaliatory
   tariffs, any unique mechanisms. Include current major measures with rates.

9. **Sales Tax / VAT / GST** -- Tax at the border (if applicable), rates,
   recovery mechanism, interaction with duty calculation.

10. **De Minimis** -- Thresholds for duty-free, tax-free, and e-commerce.
    Compare with U.S. $800 threshold.

11. **Duty Relief and Special Programs** -- Drawback, bonded warehousing,
    temporary importation, processing zones, trusted trader programs.

12. **Compliance and Enforcement** -- Audit lookback period, penalties,
    voluntary disclosure, appeal process.

13. **Key Resources** -- All official URLs, legislation references, key
    guidance documents organized in tables.

14. **Quick Reference Cheat Sheet** -- Condensed formula, key thresholds table,
    current additional duties summary, comparison table with other systems.

15. **Disclaimers** -- Not legal advice, rates change frequently, verify
    against official sources.

### Required Elements

Every guide must include:

- **Structural comparison table** (this system vs. U.S. and/or other systems)
- **Tariff number anatomy diagram** (showing digit breakdown with labels)
- **Complete landed cost formula** (step-by-step, showing all layers)
- **3 worked examples** with realistic products, tariff codes, and amounts:
  - Example 1: A duty-free or low-duty product (to show base case)
  - Example 2: A product with moderate duty (to show standard calculation)
  - Example 3: A product with additional duties/surtaxes (to show worst case)
- **Key thresholds table** (de minimis, registration thresholds, etc.)
- **FTA/preferential treatment code table** (all codes with partners and years)
- **Official resource URLs table**
- **Key legislation reference table**

### Cross-References

When the audience has read a companion guide (e.g., the U.S. guide), use
callout boxes to highlight differences:

- "Key difference from U.S." callouts for structural differences
- "Comparison with U.S." callouts for procedural similarities/differences
- Do NOT re-explain foundational concepts (HS basics, GRI rules, WTO
  Valuation Agreement) -- reference the companion guide

## Writing Style

### Tone

- **Authoritative but accessible** -- Write for professionals, not academics
- **Framework-focused** -- Teach how to look things up, not snapshot current
  rates (rates change; the framework is durable)
- **Practical** -- Include worked examples, decision trees, and step-by-step
  processes
- **Comparative** -- Constantly relate back to the reader's known system

### Formatting

- Use markdown headers for hierarchy (## for chapters, ### for sections,
  #### for subsections)
- Use tables for structured data (rates, codes, comparisons)
- Use blockquotes (>) for key differences and comparisons
- Use code blocks for formulas, tariff number diagrams, and URLs
- Use bold for key terms on first use
- Keep paragraphs focused -- one concept per paragraph
- Target 8,000-15,000 words depending on system complexity

## HTML Generation

Read [references/html-template.md](references/html-template.md) for the
complete CSS design system and HTML template.

After writing the Markdown version, create a self-contained HTML version with:

### Design System

- **System font stack** (no external font dependencies)
- **Color palette**: Slate-900 text, blue-500 accents, slate-50 backgrounds
- **Sticky sidebar TOC** (280px, fixed left, scrollable)
- **Content area** (800px max-width, left margin for sidebar)
- **4 callout types**: Info (blue), Warning (amber), Tip (green), Important (red)
- **Dark code blocks** (slate-800 background)
- **Styled tables** (dark header, alternating rows, hover highlight)
- **Formula boxes** (dark gradient background, monospace font)
- **Worked example boxes** (dark header, body, result footer with red amount)
- **Tariff anatomy diagrams** (monospace, bordered box)
- **Badge/chips** (for status indicators: Active, Suspended, Repealed)
- **Print stylesheet** (hide sidebar, B&W-friendly, page breaks before h2)
- **Responsive** (hide sidebar below 1024px)

### HTML Requirements

- **Fully self-contained** -- all CSS embedded in `<style>`, no external
  dependencies of any kind
- **All internal anchor links must resolve** -- every `href="#id"` must have a
  corresponding `id="..."` element
- **Print-ready** -- must look professional when printed to PDF
- **No JavaScript** -- pure HTML + CSS

## Verification

After generating both files, verify:

1. **File sizes** -- MD should be 30-80KB, HTML should be 40-100KB
2. **Anchor links** -- Extract all `href="#..."` and `id="..."` from HTML;
   confirm every href has a matching id
3. **External dependencies** -- Confirm zero `<link>`, `<script src>`, or
   external `url()` references in HTML
4. **Content completeness** -- Confirm all required sections are present
5. **Worked examples** -- Confirm 3 examples with correct arithmetic

## Output

Deliver two files in a dedicated directory:

```
{country}-tariff/
├── {country}-tariff-guide.md
└── {country}-tariff-guide.html
```

Both files should contain identical content -- the HTML is a styled rendering
of the Markdown.

## Guidelines

- Research thoroughly before writing. Launch parallel research agents to gather
  comprehensive data on all topic areas simultaneously.
- Be specific. Include actual tariff codes, regulation numbers, SOR/CFR
  references, and official URLs.
- Verify arithmetic in worked examples. Show each step so the reader can follow
  and verify independently.
- Use consistent terminology. Define terms on first use and use them
  consistently throughout.
- Date the document. Include the version date prominently and note that
  information reflects conditions as of that date.
- When uncertain about a current rate or status, state what was known and
  direct the reader to the official source for verification.
- Do not speculate about future policy. State current facts and pending
  legislation separately.
- Keep the HTML visually consistent with companion guides if they exist. Reuse
  the same CSS design system for a cohesive collection.
