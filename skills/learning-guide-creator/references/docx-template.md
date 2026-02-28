# Document Template Reference

This is the JavaScript template for generating learning guide documents using docx-js.
**Copy this entire template as your starting point**, then replace the placeholder content
with actual researched content.

## Complete Template

```javascript
const fs = require("fs");
const {
  Document, Packer, Paragraph, TextRun, Table, TableRow, TableCell, ImageRun,
  Header, Footer, AlignmentType, PageOrientation, LevelFormat, ExternalHyperlink,
  TableOfContents, HeadingLevel, BorderStyle, WidthType, ShadingType,
  VerticalAlign, PageNumber, PageBreak, TabStopType, TabStopPosition
} = require("docx");

// ============================================================
// CONFIGURATION — Update these for each document
// ============================================================
const CONFIG = {
  title: "DOCUMENT TITLE HERE",
  subtitle: "A Beginner's Guide to Understanding [Topic]",
  date: new Date().toLocaleDateString("en-US", { year: "numeric", month: "long" }),
  headerText: "DOCUMENT TITLE HERE",
};

// ============================================================
// COLORS — Consistent theme
// ============================================================
const COLORS = {
  darkBlue: "1A5276",
  medBlue: "2E86C1",
  lightBlue: "D5E8F0",
  red: "E74C3C",
  green: "27AE60",
  lightGreen: "E8F8F5",
  gray: "CCCCCC",
  lightGray: "F2F3F4",
  darkGray: "2C3E50",
  white: "FFFFFF",
};

// ============================================================
// HELPER FUNCTIONS — Use these throughout the document
// ============================================================

// Standard body paragraph
function para(text, opts = {}) {
  return new Paragraph({
    spacing: { after: 120, line: 276 },
    alignment: opts.align,
    children: [new TextRun({ text, font: "Arial", size: 21, color: opts.color, bold: opts.bold, italics: opts.italics })],
  });
}

// Paragraph with bold label followed by normal text
function boldPara(label, text) {
  return new Paragraph({
    spacing: { after: 120, line: 276 },
    children: [
      new TextRun({ text: label, font: "Arial", size: 21, bold: true }),
      new TextRun({ text, font: "Arial", size: 21 }),
    ],
  });
}

// Multi-run paragraph (for mixed formatting within one paragraph)
function multiPara(runs) {
  return new Paragraph({
    spacing: { after: 120, line: 276 },
    children: runs.map(r => new TextRun({ font: "Arial", size: 21, ...r })),
  });
}

// Chapter title (Heading 1) — always starts new page except first
function chapterTitle(text, firstChapter = false) {
  const children = [new TextRun({ text, font: "Arial", size: 32, bold: true, color: COLORS.darkBlue })];
  return new Paragraph({
    heading: HeadingLevel.HEADING_1,
    spacing: { before: firstChapter ? 240 : 360, after: 200 },
    pageBreakBefore: !firstChapter,
    children,
  });
}

// Section heading (Heading 2)
function sectionHeading(text) {
  return new Paragraph({
    heading: HeadingLevel.HEADING_2,
    spacing: { before: 280, after: 160 },
    children: [new TextRun({ text, font: "Arial", size: 26, bold: true, color: COLORS.medBlue })],
  });
}

// Sub-section heading (Heading 3 level, but styled manually to not clutter TOC)
function subHeading(text) {
  return new Paragraph({
    spacing: { before: 200, after: 120 },
    children: [new TextRun({ text, font: "Arial", size: 22, bold: true, color: COLORS.darkBlue })],
  });
}

// "What You'll Learn" opener for each chapter
function chapterOpener(text) {
  return new Paragraph({
    spacing: { before: 80, after: 160, line: 276 },
    border: { bottom: { style: BorderStyle.SINGLE, size: 4, color: COLORS.medBlue, space: 6 } },
    children: [
      new TextRun({ text: "What you'll learn: ", font: "Arial", size: 21, bold: true, italics: true, color: COLORS.medBlue }),
      new TextRun({ text, font: "Arial", size: 21, italics: true, color: COLORS.darkGray }),
    ],
  });
}

// Tip box (blue left border) — practical advice
function tipBox(title, text) {
  return new Paragraph({
    spacing: { before: 160, after: 160 },
    border: { left: { style: BorderStyle.SINGLE, size: 12, color: COLORS.medBlue, space: 8 } },
    indent: { left: 240 },
    children: [
      new TextRun({ text: "TIP: " + title + " ", font: "Arial", size: 21, bold: true, color: COLORS.medBlue }),
      new TextRun({ text, font: "Arial", size: 21 }),
    ],
  });
}

// Warning box (red left border) — common mistakes, critical info
function warningBox(title, text) {
  return new Paragraph({
    spacing: { before: 160, after: 160 },
    border: { left: { style: BorderStyle.SINGLE, size: 12, color: COLORS.red, space: 8 } },
    indent: { left: 240 },
    children: [
      new TextRun({ text: "WARNING: " + title + " ", font: "Arial", size: 21, bold: true, color: COLORS.red }),
      new TextRun({ text, font: "Arial", size: 21 }),
    ],
  });
}

// Example box (green left border) — real-world scenarios
function exampleBox(title, text) {
  return new Paragraph({
    spacing: { before: 160, after: 160 },
    border: { left: { style: BorderStyle.SINGLE, size: 12, color: COLORS.green, space: 8 } },
    indent: { left: 240 },
    children: [
      new TextRun({ text: "EXAMPLE: " + title + " ", font: "Arial", size: 21, bold: true, color: COLORS.green }),
      new TextRun({ text, font: "Arial", size: 21 }),
    ],
  });
}

// Key Takeaways header (used at end of each chapter)
function keyTakeawaysHeader() {
  return new Paragraph({
    spacing: { before: 240, after: 120 },
    border: { top: { style: BorderStyle.SINGLE, size: 4, color: COLORS.medBlue, space: 6 } },
    children: [new TextRun({ text: "Key Takeaways", font: "Arial", size: 24, bold: true, color: COLORS.darkBlue })],
  });
}

// Standard table cell
const cellMargins = { top: 80, bottom: 80, left: 120, right: 120 };
const border = { style: BorderStyle.SINGLE, size: 1, color: COLORS.gray };
const borders = { top: border, bottom: border, left: border, right: border };

function cell(text, opts = {}) {
  return new TableCell({
    borders,
    width: opts.width ? { size: opts.width, type: WidthType.DXA } : undefined,
    shading: opts.shading ? { fill: opts.shading, type: ShadingType.CLEAR } : undefined,
    margins: cellMargins,
    verticalAlign: VerticalAlign.CENTER,
    children: [new Paragraph({
      children: [new TextRun({ text, font: "Arial", size: 20, bold: opts.bold, color: opts.color })],
    })],
  });
}

// Header cell (blue background, bold white text)
function headerCell(text, width) {
  return cell(text, { width, shading: COLORS.darkBlue, bold: true, color: COLORS.white });
}

// Alternating row cell
function dataCell(text, width, isAlt = false) {
  return cell(text, { width, shading: isAlt ? COLORS.lightGray : COLORS.white });
}

// Build a complete table with headers and data rows
// headers: [{text, width}], rows: [[cell texts...]]
function buildTable(headers, rows) {
  const totalWidth = headers.reduce((sum, h) => sum + h.width, 0);
  const columnWidths = headers.map(h => h.width);

  return new Table({
    width: { size: totalWidth, type: WidthType.DXA },
    columnWidths,
    rows: [
      new TableRow({
        children: headers.map(h => headerCell(h.text, h.width)),
      }),
      ...rows.map((rowData, i) =>
        new TableRow({
          children: rowData.map((cellText, j) => dataCell(cellText, columnWidths[j], i % 2 === 1)),
        })
      ),
    ],
  });
}

// Spacer paragraph
function spacer(size = 120) {
  return new Paragraph({ spacing: { after: size }, children: [] });
}

// ============================================================
// NUMBERING CONFIG — Bullets and numbered lists
// ============================================================
const numberingConfig = {
  config: [
    {
      reference: "bullets",
      levels: [{
        level: 0, format: LevelFormat.BULLET, text: "\u2022",
        alignment: AlignmentType.LEFT,
        style: { paragraph: { indent: { left: 720, hanging: 360 } } },
      }],
    },
    {
      reference: "bullets2",
      levels: [{
        level: 0, format: LevelFormat.BULLET, text: "\u2022",
        alignment: AlignmentType.LEFT,
        style: { paragraph: { indent: { left: 720, hanging: 360 } } },
      }],
    },
    // Add more bullet references as needed (bullets3, bullets4...)
    // Each reference creates an INDEPENDENT list
    {
      reference: "numbers",
      levels: [{
        level: 0, format: LevelFormat.DECIMAL, text: "%1.",
        alignment: AlignmentType.LEFT,
        style: { paragraph: { indent: { left: 720, hanging: 360 } } },
      }],
    },
    {
      reference: "numbers2",
      levels: [{
        level: 0, format: LevelFormat.DECIMAL, text: "%1.",
        alignment: AlignmentType.LEFT,
        style: { paragraph: { indent: { left: 720, hanging: 360 } } },
      }],
    },
  ],
};

// Helper to create a bullet item
function bullet(text, ref = "bullets") {
  return new Paragraph({
    numbering: { reference: ref, level: 0 },
    spacing: { after: 80 },
    children: [new TextRun({ text, font: "Arial", size: 21 })],
  });
}

// Helper to create a numbered item
function numbered(text, ref = "numbers") {
  return new Paragraph({
    numbering: { reference: ref, level: 0 },
    spacing: { after: 80 },
    children: [new TextRun({ text, font: "Arial", size: 21 })],
  });
}

// Bullet with bold lead-in
function bulletBold(label, text, ref = "bullets") {
  return new Paragraph({
    numbering: { reference: ref, level: 0 },
    spacing: { after: 80 },
    children: [
      new TextRun({ text: label, font: "Arial", size: 21, bold: true }),
      new TextRun({ text, font: "Arial", size: 21 }),
    ],
  });
}

// ============================================================
// TITLE PAGE
// ============================================================
function buildTitlePage() {
  return [
    spacer(2400),
    new Paragraph({
      alignment: AlignmentType.CENTER,
      spacing: { after: 200 },
      children: [new TextRun({ text: CONFIG.title, font: "Arial", size: 52, bold: true, color: COLORS.darkBlue })],
    }),
    new Paragraph({
      alignment: AlignmentType.CENTER,
      border: { top: { style: BorderStyle.SINGLE, size: 6, color: COLORS.medBlue, space: 12 } },
      spacing: { before: 200, after: 400 },
      children: [new TextRun({ text: CONFIG.subtitle, font: "Arial", size: 28, color: COLORS.medBlue })],
    }),
    spacer(600),
    new Paragraph({
      alignment: AlignmentType.CENTER,
      spacing: { after: 120 },
      children: [new TextRun({ text: CONFIG.date, font: "Arial", size: 24, color: COLORS.darkGray })],
    }),
    new Paragraph({
      alignment: AlignmentType.CENTER,
      children: [new TextRun({ text: "A Comprehensive Learning Guide", font: "Arial", size: 22, italics: true, color: COLORS.darkGray })],
    }),
  ];
}

// ============================================================
// DOCUMENT ASSEMBLY
// ============================================================

// Build all chapter content here — this is where your researched content goes
const chapters = [

  // --- CHAPTER 1 ---
  chapterTitle("Chapter 1: [Title]", true), // true = first chapter, no page break
  chapterOpener("[What the reader will learn in this chapter]"),

  sectionHeading("1.1 [Section Title]"),
  para("[Body text with researched content...]"),
  tipBox("[Tip Title]", "[Practical advice...]"),

  sectionHeading("1.2 [Section Title]"),
  para("[More content...]"),
  exampleBox("[Example Title]", "[Real-world scenario...]"),

  keyTakeawaysHeader(),
  bullet("[Takeaway 1]", "bullets"),
  bullet("[Takeaway 2]", "bullets"),
  bullet("[Takeaway 3]", "bullets"),

  // --- CHAPTER 2 ---
  chapterTitle("Chapter 2: [Title]"),
  chapterOpener("[What the reader will learn]"),

  sectionHeading("2.1 [Section Title]"),
  para("[Content...]"),
  warningBox("[Warning Title]", "[Critical information...]"),

  // ... Continue for all chapters ...

  // --- GLOSSARY (last chapter) ---
  chapterTitle("Glossary of Terms"),
  // Use a table for glossary
  buildTable(
    [{ text: "Term", width: 2800 }, { text: "Definition", width: 6560 }],
    [
      ["[Term 1]", "[Plain English definition]"],
      ["[Term 2]", "[Plain English definition]"],
    ]
  ),

  // --- REFERENCES ---
  chapterTitle("References & Further Reading"),
  para("[Source 1 — Title, URL, accessed date]"),
  para("[Source 2 — Title, URL, accessed date]"),
];

// ============================================================
// ASSEMBLE DOCUMENT
// ============================================================
const doc = new Document({
  styles: {
    default: { document: { run: { font: "Arial", size: 21 } } },
    paragraphStyles: [
      {
        id: "Heading1", name: "Heading 1", basedOn: "Normal", next: "Normal", quickFormat: true,
        run: { size: 32, bold: true, font: "Arial", color: COLORS.darkBlue },
        paragraph: { spacing: { before: 360, after: 200 }, outlineLevel: 0 },
      },
      {
        id: "Heading2", name: "Heading 2", basedOn: "Normal", next: "Normal", quickFormat: true,
        run: { size: 26, bold: true, font: "Arial", color: COLORS.medBlue },
        paragraph: { spacing: { before: 280, after: 160 }, outlineLevel: 1 },
      },
    ],
  },
  numbering: numberingConfig,
  sections: [
    // Section 1: Title page (no header/footer)
    {
      properties: {
        page: {
          size: { width: 12240, height: 15840 },
          margin: { top: 1440, right: 1440, bottom: 1440, left: 1440 },
        },
      },
      children: buildTitlePage(),
    },
    // Section 2: TOC page
    {
      properties: {
        page: {
          size: { width: 12240, height: 15840 },
          margin: { top: 1440, right: 1440, bottom: 1440, left: 1440 },
        },
      },
      headers: {
        default: new Header({
          children: [new Paragraph({
            alignment: AlignmentType.RIGHT,
            children: [new TextRun({ text: CONFIG.headerText, font: "Arial", size: 18, color: COLORS.darkGray, italics: true })],
          })],
        }),
      },
      footers: {
        default: new Footer({
          children: [new Paragraph({
            alignment: AlignmentType.CENTER,
            children: [
              new TextRun({ text: "Page ", font: "Arial", size: 18, color: COLORS.darkGray }),
              new TextRun({ children: [PageNumber.CURRENT], font: "Arial", size: 18, color: COLORS.darkGray }),
            ],
          })],
        }),
      },
      children: [
        new Paragraph({
          spacing: { after: 300 },
          children: [new TextRun({ text: "Table of Contents", font: "Arial", size: 32, bold: true, color: COLORS.darkBlue })],
        }),
        new TableOfContents("Table of Contents", { hyperlink: true, headingStyleRange: "1-2" }),
        new Paragraph({ children: [new PageBreak()] }),
        // "Who This Guide Is For" section
        new Paragraph({
          spacing: { after: 200 },
          children: [new TextRun({ text: "Who This Guide Is For", font: "Arial", size: 28, bold: true, color: COLORS.darkBlue })],
        }),
        para("This guide is written for [describe target audience]. No prior knowledge of [topic] is assumed. Whether you are [scenario 1], [scenario 2], or simply curious about how [topic] works, this guide will take you from zero to a solid working understanding."),
        para("Each chapter builds on the previous one, starting with the basics and progressively introducing more detailed concepts. Practical examples, tips, and warnings are included throughout to help you connect theory to real-world application."),
        spacer(200),
      ],
    },
    // Section 3: Main content
    {
      properties: {
        page: {
          size: { width: 12240, height: 15840 },
          margin: { top: 1440, right: 1440, bottom: 1440, left: 1440 },
        },
      },
      headers: {
        default: new Header({
          children: [new Paragraph({
            alignment: AlignmentType.RIGHT,
            children: [new TextRun({ text: CONFIG.headerText, font: "Arial", size: 18, color: COLORS.darkGray, italics: true })],
          })],
        }),
      },
      footers: {
        default: new Footer({
          children: [new Paragraph({
            alignment: AlignmentType.CENTER,
            children: [
              new TextRun({ text: "Page ", font: "Arial", size: 18, color: COLORS.darkGray }),
              new TextRun({ children: [PageNumber.CURRENT], font: "Arial", size: 18, color: COLORS.darkGray }),
            ],
          })],
        }),
      },
      children: chapters,
    },
  ],
});

// ============================================================
// WRITE FILE
// ============================================================
Packer.toBuffer(doc).then(buffer => {
  const filename = CONFIG.title.toLowerCase().replace(/[^a-z0-9]+/g, "-").replace(/-+/g, "-") + "-guide.docx";
  fs.writeFileSync(filename, buffer);
  console.log(`Created: ${filename}`);
});
```

## Usage Notes

1. **Always create unique numbering references** — If you need multiple independent bullet lists, use "bullets", "bullets2", "bullets3", etc. Same reference = continues numbering.

2. **Content width** — With 1-inch margins on US Letter, content width is 9360 DXA. All full-width tables should use this value.

3. **Table column widths must sum to table width** — e.g., for a 2-column table: 4680 + 4680 = 9360.

4. **Aim for ~30 pages** — A rough guide:
   - 1 page ≈ 300-350 words of body text
   - Tables, boxes, and spacing add ~30% more pages
   - So target 8,000-10,000 words of actual content

5. **Generate enough bullet references** — Create at least 15-20 separate bullet references (bullets, bullets2, ... bullets20) and number references (numbers, numbers2, ... numbers10) in the numbering config. Each independent list section needs its own reference.

6. **Validate after creation:**
   ```bash
   python /mnt/skills/public/docx/scripts/office/validate.py output.docx
   ```
