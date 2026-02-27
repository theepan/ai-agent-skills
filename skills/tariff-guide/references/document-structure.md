# Document Structure Reference

Standard chapter framework for tariff guide documents. Adapt to the specific
country or customs territory -- not all sections apply to every system.

## Chapter Templates

### Executive Summary Template

Open with a 2-3 sentence paragraph framing the system relative to the reader's
known system. Then provide a structural comparison table:

| Feature | [Reference System] | [This System] |
|---------|-------------------|---------------|
| Governing law | ... | ... |
| Classification digits | ... | ... |
| Duty rate columns | ... | ... |
| Valuation basis | FOB / CIF | ... |
| Sales tax at border | ... | ... |
| Additional duty layers | ... | ... |
| AD/CVD system | Retrospective / Prospective | ... |
| Customs filing system | ... | ... |
| De minimis threshold | ... | ... |

Follow with "most consequential differences" callout highlighting the 3-5
things that will matter most to importers.

### Tariff Number Anatomy Diagram

Use this format to show the digit breakdown:

```
  XXXX.XX.XX.XX
  |||| || || ||
  |||| || || └└── [National extension] — [Authority]
  |||| || └└──── [National subdivision] — [Authority]
  |||| └└──────── HS subheading (digits 5-6) — WCO
  └└└└──────────── HS heading (digits 1-4) — WCO
```

Follow with a table mapping digits to names, authorities, and purposes.

### Worked Example Template

Each worked example should include:

**Header:** Product description, tariff code, value, destination

**Calculation table:**

| Step | Item | Rate | Calculation | Amount |
|------|------|------|-------------|--------|
| 1 | Starting value | — | ... | $X |
| 2 | Customs duty | X% | $X × X% | $X |
| ... | ... | ... | ... | ... |
| N | Total charges | | | **$X** |

**Result line:** Total charges: $X | Effective rate: X.X%

**Comparison note:** How the same product would be treated in the reference
system.

### Three Standard Examples

1. **Low/zero duty product** -- Shows the base case. Pick a product with Free
   or very low MFN duty (e.g., electronics under ITA). Demonstrates that even
   duty-free goods may face sales tax/VAT.

2. **Moderate duty product** -- Shows the standard calculation. Pick a product
   with meaningful MFN duty (e.g., clothing, footwear, auto parts). Shows the
   full formula in action.

3. **High-duty / additional-duty product** -- Shows the worst case. Pick a
   product subject to AD/CVD, surtax, safeguard, or retaliatory tariff.
   Demonstrates duty compounding and the maximum cost impact.

### Resource Tables

#### Official Websites Table

| Resource | URL | Description |
|----------|-----|-------------|
| Official tariff schedule | ... | Browse/search tariff codes and rates |
| Advance ruling database | ... | Search classification rulings |
| Trade defence measures | ... | Active AD/CVD orders |
| ... | ... | ... |

#### Key Legislation Table

| Law/Regulation | Reference | Description |
|----------------|-----------|-------------|
| Customs Act | ... | Administrative framework |
| Tariff Act | ... | Rate schedule |
| AD/CVD law | ... | Trade remedy authority |
| ... | ... | ... |

### Quick Reference Cheat Sheet

Include at minimum:

1. **Landed cost formula** in a formula box
2. **Key thresholds table** (de minimis, registration, etc.)
3. **Current additional duties summary** (surtaxes, safeguards, etc.)
4. **Cross-system comparison table** (if companion guides exist)

## Section Ordering Principles

- Start with "what" (overview, structure) before "how" (calculation, process)
- Cover the standard case before exceptions and special programs
- Place worked examples immediately after the formula they demonstrate
- Put reference tables and cheat sheets at the end for quick lookup
- Disclaimers are always the final section

## Cross-Reference Conventions

When the reader has a companion guide:

- Use "Key difference from [System]:" callouts (info-type) for structural
  differences
- Use "Comparison with [System]:" callouts (tip-type) for
  similarities/differences in process
- Use "No equivalent in [System]:" callouts (warning-type) for unique features
- Never re-explain concepts covered in the companion guide -- reference it

## Word Count Guidelines

| System Complexity | Target Words | Examples |
|-------------------|-------------|----------|
| Simple (few FTAs, no surtaxes) | 8,000-10,000 | Small economies |
| Moderate (several FTAs, some AD/CVD) | 10,000-13,000 | Canada, Australia, Japan |
| Complex (many FTAs, multiple duty layers, unique features) | 13,000-18,000 | U.S., EU, China |
