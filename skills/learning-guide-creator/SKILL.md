---
name: learning-guide-creator
description: >
  Create comprehensive, research-backed ~30-page learning guide documents (.docx) on any topic.
  Designed for beginners who want to study a new area from scratch with background information,
  real-world examples, and practical takeaways. Use this skill whenever the user asks to create
  a learning guide, study guide, research document, educational document, guidance document,
  beginner's guide, "teach me about X" document, training material, or any request that involves
  researching a topic and producing a printable educational Word document. Also trigger when the
  user says things like "I want to learn about...", "create a guide on...", "write a document
  explaining...", "research and write about...", "help me understand... as a document",
  "make me a study guide for...", or "create learning materials for...". This skill combines
  web research with professional document creation to produce polished, easy-to-read guides.
---

# Learning Guide Creator

## Overview

This skill creates professional ~30-page .docx learning guides by:
1. Researching the topic thoroughly via web search (current data, regulations, best practices)
2. Structuring content for a beginner audience with progressive complexity
3. Generating a polished Word document with consistent professional formatting

## Workflow

### Phase 1 — Understand the Topic

Ask the user (if not already clear):
- **What topic** do you want to learn about?
- **What's your current knowledge level?** (complete beginner, some familiarity, need a refresher)
- **Any specific focus areas** within the topic? (optional — if not specified, cover broadly)
- **Who is the audience?** (default: someone new to the topic who needs practical understanding)

### Phase 2 — Research

Conduct 8-15 web searches to gather current, authoritative information:

1. **Foundational searches** (3-4): Core concepts, definitions, how the system/topic works
2. **Current state searches** (3-4): Latest regulations, updates, trends, recent changes (include year)
3. **Practical searches** (2-3): Real-world examples, common mistakes, best practices, how-to
4. **Deep-dive searches** (2-3): Specific sub-topics the user mentioned or that are essential

**Research rules:**
- Always include the current year in searches for regulatory/policy/technology topics
- Prefer official/government/academic sources over blogs
- Note conflicting information — address it in the document
- Track sources for the References section
- Use `web_fetch` on the most authoritative results to get full details

### Phase 3 — Build Outline

Before writing, construct a chapter outline with approximately this structure (adapt to topic):

```
FRONT MATTER
  - Title Page (topic name, subtitle, date)
  - Table of Contents
  - "Who This Guide Is For" (1 paragraph)

PART 1: FOUNDATIONS (Chapters 1-3) — ~8 pages
  - Chapter 1: What Is [Topic]? — Big picture, why it matters, brief history
  - Chapter 2: Key Concepts & Terminology — Core vocabulary explained simply
  - Chapter 3: How [Topic] Works — The fundamental mechanics/process

PART 2: DEEP DIVE (Chapters 4-7) — ~12 pages
  - Chapter 4-7: Major sub-topics, each with:
    - Explanation of the concept
    - How it works in practice
    - Real-world example or case study
    - Common mistakes or misconceptions
    - Tip boxes and warning boxes where appropriate

PART 3: PRACTICAL APPLICATION (Chapters 8-9) — ~6 pages
  - Chapter 8: Putting It All Together — Step-by-step walkthrough
  - Chapter 9: Common Scenarios & Troubleshooting

BACK MATTER — ~4 pages
  - Quick Reference Summary (key points table)
  - Glossary of Terms
  - Resources & Further Reading (with URLs)
  - References (sources used in research)
```

Aim for 8-12 chapters total. Adjust chapter count based on topic complexity.

### Phase 4 — Write the Document

Read the docx skill at `/mnt/skills/public/docx/SKILL.md` before generating the document.

Then read the document template reference at:
`/mnt/skills/user/learning-guide-creator/references/docx-template.md`

This contains the exact JavaScript code template with all helper functions and formatting
that matches the established document style. **Use this template as the base for every document.**

**Writing guidelines:**
- Write for someone who has ZERO background — explain everything from scratch
- Use plain English first, then introduce technical terms with definitions
- Every chapter should open with a 1-2 sentence "What you'll learn" statement
- Use analogies and real-world comparisons to explain complex ideas
- Include "Tip" boxes (blue left border) for practical advice
- Include "Warning" boxes (red left border) for common mistakes or critical info
- Include "Example" boxes (green left border) for real-world scenarios
- Use tables to compare options, summarize data, or present structured info
- End each chapter with a "Key Takeaways" bullet list (3-5 points)
- Aim for ~30 pages (approximately 8,000-10,000 words of content)
- Include page breaks between chapters
- Use specific numbers, dates, thresholds, and names from research (not vague generalities)

**Tone:**
- Conversational but professional — like a knowledgeable mentor explaining to a colleague
- "Here's how this works..." not "This section will discuss..."
- Active voice, direct sentences
- Avoid jargon without explanation
- When jargon is necessary, format as: "term (plain English explanation)"

### Phase 5 — Generate the .docx

Use `npm install -g docx` and the docx-js library to create the document.
Follow ALL critical rules from the docx skill (page size, no unicode bullets, dual table widths, etc.).

After creating the file:
```bash
python /mnt/skills/public/docx/scripts/office/validate.py output.docx
```

Copy to outputs and present to user:
```bash
cp output.docx /mnt/user-data/outputs/
```

## Document Formatting Standards

These match the established style from previous research/guidance documents:

| Element | Specification |
|---------|--------------|
| Font | Arial throughout |
| Body text | 10.5pt (size: 21 in docx-js) |
| Heading 1 | 16pt, bold, dark blue (#1A5276) |
| Heading 2 | 13pt, bold, medium blue (#2E86C1) |
| Heading 3 | 11pt, bold, dark blue (#1A5276) |
| Page size | US Letter (12240 x 15840 DXA) |
| Margins | 1 inch all sides (1440 DXA) |
| Line spacing | 1.15x (276 twips) on body text |
| Tip boxes | Blue left border (#2E86C1), 12pt weight, indented |
| Warning boxes | Red left border (#E74C3C), 12pt weight, indented |
| Example boxes | Green left border (#27AE60), 12pt weight, indented |
| Tables | Light blue header (#D5E8F0), gray borders (#CCCCCC) |
| Title page | Large centered title, subtitle, date, dark blue accent |
| Headers | Document title, right-aligned |
| Footers | "Page X" centered |
| Chapter breaks | Page break before each new chapter |

## Quality Checklist

Before delivering, verify:
- [ ] All research findings are incorporated with specific data points
- [ ] Every technical term is explained in plain English on first use
- [ ] Tip/Warning/Example boxes are distributed throughout (at least 2 per chapter)
- [ ] Tables are used for comparisons and structured data
- [ ] Key Takeaways appear at end of each chapter
- [ ] Glossary covers all domain-specific terms used
- [ ] References section lists actual sources from research
- [ ] Document validates without errors
- [ ] Page count is approximately 25-35 pages
- [ ] TOC is included and heading hierarchy is correct
