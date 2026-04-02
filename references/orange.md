# Orange Theme — Warm Corporate IR Design System

A bold, warm, investor-grade theme designed for earnings calls, corporate IR decks, capital markets days, and growth-oriented presentations. Combines vibrant orange energy with warm cream backgrounds, deep chocolate text, and a structured multi-card layout system optimized for financial data visualization.

---

## Origin & Design DNA

Inspired by top-tier MENA on-demand platform IR design language. The system uses a distinctive warm-on-warm palette: vivid orange for energy and brand confidence, warm cream/sand backgrounds for approachability, and deep chocolate brown for anchoring text — a deliberate departure from the typical cold blue/navy IR deck.

---

## Color System

### Primary Palette

```
PRIMARY:       FF5722   (vivid orange — hero backgrounds, section dividers, accent bars)
SECONDARY:     3D1F00   (chocolate brown — headings, badges, dark accents, number labels)
ACCENT:        7AB648   (leaf green — positive indicators, checkmarks, growth arrows)
BG_WARM:       F5EDE3   (warm cream — default slide background, the "paper" feel)
BG_ORANGE:     FF5722   (full-bleed orange — section divider slides, hero statements)
BG_WHITE:      FFFFFF   (white — card fills, table cell backgrounds)
TEXT_DARK:     3D1F00   (chocolate brown — primary headings)
TEXT_BODY:     4A3728   (warm dark brown — body text, descriptions)
TEXT_LIGHT:    FFFFFF   (white — text on orange or dark backgrounds)
TEXT_MUTED:    9B8E82   (warm gray — annotations, footnotes, source citations)
```

### Secondary / Data Colors

```
ORANGE_LIGHT:  FF8A50   (light orange — stacked bar secondary, chart accent 2)
BROWN_MID:     6B4226   (medium brown — stacked bar segment 3, delivery fees)
GREEN_DATA:    7AB648   (green — advertising & listing fees in revenue stack, growth badges)
OLIVE_DATA:    A8B05C   (olive — subscription fees in revenue stack)
CREAM_CARD:    FFF3E6   (light peach — card highlight fills, KPI card backgrounds)
PEACH_TINT:    FFE8D6   (peach tint — alternating table rows, soft badge backgrounds)
BADGE_BG:      F5EDE3   (cream — pill-shaped badges for y/y growth labels)
GUIDANCE_BG:   3D1F00   (chocolate — guidance callout boxes)
```

### Chart Palette (ordered for stacked bars and multi-series)

```
CHART_1:       3D1F00   (chocolate — commission fees / base layer)
CHART_2:       6B4226   (brown — subscription & other income)
CHART_3:       FF5722   (orange — delivery & service fees)
CHART_4:       7AB648   (green — advertising & listing fees / top layer)
CHART_5:       FF8A50   (light orange — secondary series)
```

### Semantic / Grade Colors

```
EXCEEDED:      7AB648   (green — performance exceeded guidance)
IN_LINE:       FF5722   (orange — performance in line with guidance)
BELOW:         CC3300   (dark red — underperformance, use sparingly)
NEUTRAL:       9B8E82   (warm gray — ungraded / baseline)
```

---

## Typography System

### Font Pairing

| Voice | Header Font | Body Font | Monospace |
|-------|-------------|-----------|-----------|
| **Primary** | Trebuchet MS | Calibri | Consolas |
| **Fallback** | Arial Black | Arial | Consolas |

### Type Scale

| Element | Size | Weight | Color | Notes |
|---------|------|--------|-------|-------|
| Slide title | 36-44pt | Bold | 3D1F00 | On cream bg; FFFFFF on orange bg |
| Category label (above title) | 12pt | Bold | FF5722 | Uppercase, tracked (charSpacing: 3) |
| Subtitle | 22-24pt | Normal | 4A3728 | Below title, lighter weight |
| Section divider title | 56-72pt | Extra Bold | FFFFFF | On FF5722 orange full-bleed background |
| Body text | 14-16pt | Normal | 4A3728 | Left-aligned, generous line spacing |
| KPI stat number | 48-72pt | Bold | FF5722 or 3D1F00 | Large callout numbers |
| KPI label | 14pt | Bold | 4A3728 | Below stat number |
| KPI sublabel | 12pt | Normal | 9B8E82 | Supporting detail below label |
| Growth badge text | 16-20pt | Bold | FF5722 | Inside rounded pill badges |
| Chart title bar | 18pt | Bold | FFFFFF | Inside orange header bar above chart |
| Table header | 14pt | Bold | FFFFFF | On FF5722 or 3D1F00 background |
| Table body | 13pt | Normal | 4A3728 | On white or peach alternating rows |
| Footnote / source | 9-10pt | Normal | 9B8E82 | Bottom of slide, left-aligned |
| Page number | 10pt | Normal | 9B8E82 | Bottom-right corner |

---

## Layout Patterns (Orange-Specific Treatments)

Orange theme uses the same 20 shared layouts from `layouts.md` but applies its own distinctive treatments. Below are the per-layout overrides.

### Layout 1: Title Hero → "Brand Splash"

**Structure**: Left 45% is solid orange (FF5722) with white title text. Right 55% is a full-bleed photo zone (city skyline, riders, operations imagery). A thin green accent line (7AB648) separates the two zones vertically.

```
Slide background: split — left FF5722, right image
Title: 36pt Bold FFFFFF, left-aligned on orange zone
Subtitle: 18pt Normal FFFFFF, below title
Date: 14pt Normal FFFFFF, bottom of orange zone
Logo: top-left on orange zone (white variant)
```

### Layout 2: Section Divider → "Orange Curtain"

**Structure**: Full-bleed FF5722 background. Section title in 56-72pt Extra Bold FFFFFF, left-aligned at vertical center. Logo badge in top-right corner (white, slightly tilted for brand personality). No other elements.

```
Background: FF5722 (solid orange)
Title: 56pt Bold FFFFFF, x: 0.7", y: 2.0"
Logo: top-right corner, white variant, 1.2" wide
```

### Layout 3: Two-Column Split → "Warm Split"

**Structure**: Left 50% on F5EDE3 cream background with text content. Right 50% on FF5722 orange background with numbered highlight cards or key company bullets. Each bullet uses a numbered chocolate (3D1F00) badge circle.

```
Left zone: cream (F5EDE3), title in 3D1F00, body bullets in 4A3728
Right zone: orange (FF5722), numbered points with FFFFFF text
Number badges: 3D1F00 circles with FFFFFF numbers
```

### Layout 4: Icon Row → "KPI Scorecard"

**Structure**: 4 equal-width cards on cream (F5EDE3) background. Each card has a top icon area with a brown (3D1F00) circular icon, a metric label, a large stat number in FF5722, and a supporting label. Cards separated by 0.2" gaps. Below the cards, a full-width accent banner (FF5722) with dividend or key announcement text.

```
Background: F5EDE3 (cream)
Card fill: FFF3E6 (light peach) with subtle shadow
Icon circles: 3D1F00 with white icon inside
Stat numbers: 36-48pt Bold FF5722
Labels: 16pt Bold 4A3728
Bottom banner: FF5722 bg, FFFFFF text, diamond icon left
```

### Layout 5: Stat Callout → "Big Number Reveal"

**Structure**: Single dominant metric centered. Number in 96-120pt Bold FF5722. Label above in 12pt tracked uppercase 9B8E82. Context line below in 16pt 4A3728. Optional: secondary smaller stats flanking in muted cards.

### Layout 6: Card Grid → "Three-Pillar Strategy"

**Structure**: 3 cards side by side on cream background. Each card has a thin orange top-bar accent, an orange outline icon at top, a bold 20pt 3D1F00 heading, and bullet points in 4A3728. Below the cards, a peach (FFE8D6) banner with macro tailwind callouts and a paper-plane icon.

```
Card construction: FFFFFF fill, 4px FF5722 top bar, sharp corners
Icon: FF5722 outline style, centered above heading
Heading: 20pt Bold 3D1F00
Bullet points: orange dot markers, 14pt 4A3728 text
Bottom banner: FFE8D6 fill, 14pt 4A3728 text with orange bullets
```

### Layout 7: Timeline / Process → "Journey Arrow"

**Structure**: Horizontal timeline with ascending GMV growth arrow (FF5722). Year markers along bottom. Key milestones annotated above with icon badges and labels. GMV bar chart underneath using orange (FF5722) bars. CAGR callout in a large circle badge.

### Layout 8: Chart + Insight → "Dual Chart Dashboard"

**Structure**: Two side-by-side chart panels on cream background. Each panel has an orange (FF5722) header bar with white title text. Left panel: bar chart (orange bars). Right panel: stacked bar chart (4-color stack using CHART_1 through CHART_4). Below panels: bullet-point insights in 14pt body text. Growth badges (rounded pills in BADGE_BG with FF5722 text) annotate y/y growth above each chart.

```
Panel header: FF5722 fill, FFFFFF text, 18pt Bold
Chart bars: single-color FF5722 for simple bars
Stacked bars: 3D1F00, 6B4226, FF5722, 7AB648 (bottom to top)
Growth badges: F5EDE3 rounded pill, FF5722 text, "17%", "23%"
Axis labels: 9B8E82, 11pt
Gridlines: none (clean axis only)
```

### Layout 9: Quote → "Evolution Timeline"

**Adapted**: Instead of a traditional quote slide, this becomes a 3-stage product evolution timeline (e.g., 2016 → 2021 → 2025) with phone mockup screenshots and tagline quotes. Each stage has a gray/brown/orange header pill and a descriptive tagline in italics.

### Layout 10: Image + Overlay → "App Showcase"

**Structure**: Left side shows a phone mockup with app screenshot. Center-right shows descriptive text with "What's changed?" header in FF5722. Far right shows large stat callout badges (e.g., "+24%" in FF5722 on cream circle, "+2%" in 3D1F00 on cream circle) with metric labels.

### Layout 11: Table Slide → "Guidance Tracker"

**Structure**: Full-width table on cream background. Header row uses FF5722 background with FFFFFF text. Data rows alternate between FFFFFF and FFF3E6 (peach). Performance checkmark column uses 7AB648 green circles with white checkmarks. "Exceeded" and "In line" labels in green text. Guidance column uses the same orange header style.

```
Table header: FF5722 bg, FFFFFF text, 14pt Bold
Row 1 (odd): FFFFFF bg
Row 2 (even): FFF3E6 bg (light peach)
Checkmarks: 7AB648 circle, FFFFFF checkmark
Status text: "Exceeded" in 7AB648 Bold, "In line" in 4A3728
Column dividers: 1pt E8E0D6 (warm gray)
```

### Layout 12: Closing / CTA → "Thank You Brand"

**Structure**: Full-bleed FF5722 background. "Thank you!" in 72pt Extra Bold FFFFFF centered. Logo centered below in large format (dark/brown variant on orange). Contact email in 14pt Normal FFFFFF below logo.

### Layout 13: Agenda / Overview → "Presenter Grid"

**Structure**: Left side shows presenter headshots in circular frames with name/title. Right side shows numbered agenda items in rounded cream (F5EDE3) pill bars with orange number badges. Vertical dotted line separates the two zones.

```
Photo frames: circular crop, 1.5" diameter
Name: 16pt Bold 3D1F00
Title: 14pt Normal FF5722
Agenda pills: F5EDE3 fill, rounded corners, 3D1F00 text
Number badges: FF5722 circle (for current section), 3D1F00 circle (for future sections)
Dotted separator: 9B8E82 dots, vertical
```

### Layout 14: Before / After → "GCC vs Non-GCC Split"

**Structure**: Two stacked bar charts side by side. Left shows vertical breakdown (Food vs G&R). Right shows geographical breakdown (GCC vs Non-GCC). Both use orange and brown/dark color coding. Growth badges annotate percentage changes above each bar cluster.

### Layout 15: Multi-Period Table → "Guidance Matrix"

**Structure**: Multi-column comparison table. Left column has metric labels in 3D1F00 on peach (FFF3E6) background. Middle columns show guidance vs actual with checkmark badges. Right column shows forward guidance. All in alternating warm rows.

### Layout 16: KPI Card Row → "Financial Highlights"

**Structure**: 4 KPI cards in a row on cream background. Each card has a brown icon circle at top, a metric name, a large orange stat number, and margin/growth context below. Inspired by the "Key highlights for 2024" slide pattern.

```
Card fill: FFF3E6 (light peach)
Icon area: 3D1F00 circle with white icon
Metric name: 16pt Bold 3D1F00
Stat number: 48pt Bold FF5722
Growth text: "+23% y/y" in FF5722
Context: "(USD 7.4bn)" in 9B8E82
```

### Layout 17: Strategy Dashboard → "Profitability Dashboard"

**Structure**: Two chart panels side by side (e.g., Adj. EBITDA and Adj. FCF). Each has an orange header bar. Charts use solid orange bars. Guidance callout box in 3D1F00 (chocolate) with FFFFFF text at bottom. Bullet insight text below charts.

### Layout 18: Product Feature → "Subscription Showcase"

**Structure**: Left phone mockup with app screenshot. Center shows subscription program details with purple/orange deal cards. Right shows large metric badges (2.1x, +28%) with descriptive labels. "Key updates" badge tilted in top-right.

### Layout 19: Bold Section Divider → "Orange Curtain (Alt)"

Same as Layout 2 but can optionally use FFFFFF background with 3D1F00 massive text for contrast variety.

### Layout 20: PoP Bar Comparison → "Vertical Mix Evolution"

**Structure**: Two stacked bar charts (by vertical and by segment) with growth badges above. Uses the full stacked bar palette. Orange header bars label each chart panel.

---

## Data Visualization Guidelines

### Bar Charts

- **Simple bars**: solid FF5722 (orange) with no outline
- **Comparison bars**: FF5722 (current) + E8E0D6 (prior/gray) side by side
- **Bar gap**: 75% for breathing room
- **No gridlines**: clean axis only
- **Value labels**: 14pt Bold 3D1F00, positioned above or inside bars
- **Axis labels**: 11pt 9B8E82, no axis lines

### Stacked Bar Charts (Revenue Breakdown)

- **Stack order (bottom to top)**: 3D1F00 (commission) → 6B4226 (subscription) → FF5722 (delivery/service) → 7AB648 (advertising)
- **Legend**: horizontal below chart, small colored squares + 11pt 4A3728 labels
- **Total value label**: above each stack in 14pt Bold 3D1F00
- **Growth badge**: rounded pill (F5EDE3 fill, FF5722 text) floating above each year

### Growth Badges / Pills

Distinctive pattern: rounded pill shapes that float above chart bars to show y/y growth.

```javascript
// Growth badge
slide.addShape(pres.shapes.ROUNDED_RECTANGLE, {
  x, y, w: 0.9, h: 0.35,
  rectRadius: 0.15,
  fill: { color: "F5EDE3" }
});
slide.addText(growthPct, {
  x, y, w: 0.9, h: 0.35,
  fontSize: 12, fontFace: "Trebuchet MS", color: "3D1F00",
  bold: true, align: "center", valign: "middle"
});
```

### Guidance Callout Boxes

Dark chocolate (3D1F00) rounded rectangle with white text showing guidance target. Positioned below charts with dotted border connecting to the relevant bar.

```javascript
slide.addShape(pres.shapes.ROUNDED_RECTANGLE, {
  x, y, w: 2.5, h: 0.6,
  rectRadius: 0.1,
  fill: { color: "3D1F00" }
});
slide.addText(guidanceText, {
  x, y, w: 2.5, h: 0.6,
  fontSize: 12, fontFace: "Calibri", color: "FFFFFF",
  bold: true, align: "center", valign: "middle"
});
```

### Performance Tables (Guidance vs Actual)

- Header row: FF5722 background, FFFFFF text
- Alternating rows: FFFFFF and FFF3E6
- Checkmark column: 7AB648 filled circles with FFFFFF checkmarks
- "Exceeded" text: 7AB648 Bold
- "In line" text: 4A3728 with "(high end of range)" in 9B8E82

### Concentric Circle Diagrams (TAM/SAM/SOM)

- Outer ring: FF5722 10% opacity fill
- Middle ring: FF5722 25% opacity fill  
- Inner ring: FF5722 solid
- Labels in 3D1F00 Bold with dashed lines to rings
- CAGR indicators on right side in 7AB648

### Network Effect / Flywheel Diagrams

- Central node: large FF5722 circle with white icon
- Satellite nodes: 3D1F00 circles (smaller) with white labels
- Connecting arrows: F5EDE3 (cream) curved paths
- Metric callouts: orange stat numbers adjacent to each node

---

## Slide Background Rules

| Slide Type | Background | When |
|-----------|-----------|------|
| **Content slides** | F5EDE3 (warm cream) | Default for most slides |
| **Section dividers** | FF5722 (solid orange) | Chapter transitions, bold statements |
| **Financial tables** | F5EDE3 with FFFFFF card areas | Data-heavy slides |
| **Title / Closing** | Split (orange + photo) or full orange | First and last slides |
| **Dark emphasis** | 3D1F00 (chocolate) | Rare — verdict or critical decision |

---

## Card System (Orange Variant)

### Card Construction

```javascript
const makeOrangeCard = (slide, pres, x, y, w, h, opts = {}) => {
  const { fill = "FFFFFF", accentTop = false } = opts;
  
  slide.addShape(pres.shapes.RECTANGLE, {
    x, y, w, h,
    fill: { color: fill },
    shadow: { type: "outer", blur: 4, offset: 1, angle: 270, color: "000000", opacity: 0.06 }
  });
  
  if (accentTop) {
    slide.addShape(pres.shapes.RECTANGLE, {
      x, y, w, h: 0.04,
      fill: { color: "FF5722" }
    });
  }
};
```

### Card Variants

| Variant | Fill | Accent | When |
|---------|------|--------|------|
| **Default** | FFFFFF | shadow only | General content cards |
| **Warm** | FFF3E6 | none | KPI cards, highlight boxes |
| **Accent-top** | FFFFFF | 4px orange top bar | Strategy pillars, feature grid |
| **Icon card** | FFF3E6 | 3D1F00 circle icon at top | Financial highlights row |
| **Guidance box** | 3D1F00 | none | Guidance callout (dark) |
| **Banner** | FF5722 | none | Bottom announcement bar |

---

## Badge System

### Number Badge (Agenda / Steps)

```javascript
const makeNumberBadge = (slide, pres, x, y, num, isActive = true) => {
  slide.addShape(pres.shapes.OVAL, {
    x, y, w: 0.5, h: 0.5,
    fill: { color: isActive ? "FF5722" : "3D1F00" }
  });
  slide.addText(String(num), {
    x, y, w: 0.5, h: 0.5,
    fontSize: 18, fontFace: "Trebuchet MS", color: "FFFFFF",
    bold: true, align: "center", valign: "middle"
  });
};
```

### Checkmark Badge (Performance Table)

```javascript
const makeCheckBadge = (slide, pres, x, y) => {
  slide.addShape(pres.shapes.OVAL, {
    x, y, w: 0.35, h: 0.35,
    fill: { color: "7AB648" }
  });
  slide.addText("✓", {
    x, y, w: 0.35, h: 0.35,
    fontSize: 16, fontFace: "Calibri", color: "FFFFFF",
    bold: true, align: "center", valign: "middle"
  });
};
```

---

## Logo Placement

- **On cream/white backgrounds**: orange logo variant, top-right, ~1.2" wide, slightly tilted (brand signature)
- **On orange backgrounds**: white logo variant, top-right, same size and tilt
- **On closing slides**: large centered dark (3D1F00) logo on orange background

---

## Forbidden Elements (Orange Theme)

- Cold blues or navy as primary colors (use warm browns instead)
- Gray/silver corporate backgrounds (use warm cream F5EDE3)
- Gradients on backgrounds (flat colors only)
- Thin hairline fonts (use bold, confident weights)
- Bullet-heavy slides without visual structure (use cards)
- More than 4 data colors in a single chart
- Stock photography without warm/orange color grading

---

## Choosing Orange Theme

**By industry**: Food delivery, logistics, MENA corporate, consumer platforms, marketplace businesses
**By audience**: Investors, analysts, board members, public market stakeholders  
**By tone**: Confident, warm, growth-oriented, approachable yet professional
**By context**: Earnings calls, investor presentations, capital markets days, IPO roadshows, annual reports

---

## Narrative Flow (Investor Presentation)

1. **Layout 1 (Brand Splash)** — Title with company photo
2. **Layout 13 (Presenter Grid)** — Agenda with speaker lineup
3. **Layout 2 (Orange Curtain)** — Section divider: "Business Update"
4. **Layout 3 (Warm Split)** — Company overview with key highlights
5. **Layout 16 (Financial Highlights)** — 4 KPI cards: GMV, Revenue, EBITDA, Net Income
6. **Layout 6 (Three-Pillar Strategy)** — Growth levers / strategic pillars
7. **Layout 10 (App Showcase)** — Product / platform evolution
8. **Layout 2 (Orange Curtain)** — Section divider: "Financials"
9. **Layout 8 (Dual Chart Dashboard)** — GMV + Revenue charts
10. **Layout 14 (GCC vs Non-GCC Split)** — Segment breakdown
11. **Layout 17 (Profitability Dashboard)** — EBITDA + FCF charts
12. **Layout 5 (Big Number Reveal)** — Net income + dividend
13. **Layout 2 (Orange Curtain)** — Section divider: "Outlook"
14. **Layout 15 (Guidance Matrix)** — Guidance vs actual vs forward
15. **Layout 12 (Thank You Brand)** — Closing with logo and contact

---

## JavaScript Theme Object

Use this constant block at the top of your generation script when Orange theme is selected:

```javascript
// ── Orange Theme Constants ──
const theme = {
  // Core palette
  PRIMARY:     'FF5722',
  SECONDARY:   '3D1F00',
  ACCENT:      '7AB648',
  BG_DARK:     '3D1F00',
  BG_LIGHT:    'F5EDE3',
  BG_WHITE:    'FFFFFF',
  TEXT_DARK:    '3D1F00',
  TEXT_LIGHT:   'FFFFFF',
  TEXT_MUTED:   '9B8E82',
  TEXT_BODY:    '4A3728',

  // Fonts
  HEADER_FONT: 'Trebuchet MS',
  BODY_FONT:   'Calibri',

  // Chart palette (ordered for stacked bars)
  CHART_COLORS: ['3D1F00', '6B4226', 'FF5722', '7AB648', 'FF8A50'],

  // Extended
  CREAM_CARD:   'FFF3E6',
  PEACH_TINT:   'FFE8D6',
  BADGE_BG:     'F5EDE3',
  ORANGE_LIGHT: 'FF8A50',
  BROWN_MID:    '6B4226',
  OLIVE_DATA:   'A8B05C',
  EXCEEDED:     '7AB648',
  CARD_BORDER:  'E8E0D6',
};

// Default slide background — warm cream (override per slide type)
const DEFAULT_BG = { color: theme.BG_LIGHT };  // F5EDE3
const SECTION_BG = { color: theme.PRIMARY };     // FF5722
const DARK_BG    = { color: theme.BG_DARK };     // 3D1F00
```

### Orange-Specific Helpers

```javascript
// ── Growth Pill Badge ──
function addGrowthPill(slide, pres, x, y, label) {
  slide.addShape(pres.shapes.ROUNDED_RECTANGLE, {
    x, y, w: 0.95, h: 0.32,
    rectRadius: 0.14,
    fill: { color: theme.BADGE_BG }
  });
  slide.addText(label, {
    x, y, w: 0.95, h: 0.32,
    fontSize: 11, fontFace: theme.HEADER_FONT, color: theme.SECONDARY,
    bold: true, align: 'center', valign: 'middle', margin: 0
  });
}

// ── KPI Card (Financial Highlights Row) ──
function addKpiCard(slide, pres, x, y, w, h, { iconBase64, metricName, statValue, growthLabel, contextLabel }) {
  // Card background
  slide.addShape(pres.shapes.RECTANGLE, {
    x, y, w, h,
    fill: { color: theme.CREAM_CARD },
    shadow: { type: 'outer', blur: 4, offset: 1, angle: 270, color: '000000', opacity: 0.06 }
  });
  // Icon circle
  slide.addShape(pres.shapes.OVAL, {
    x: x + w/2 - 0.3, y: y + 0.2, w: 0.6, h: 0.6,
    fill: { color: theme.SECONDARY }
  });
  if (iconBase64) {
    slide.addImage({
      data: iconBase64,
      x: x + w/2 - 0.2, y: y + 0.3, w: 0.4, h: 0.4
    });
  }
  // Metric name
  slide.addText(metricName, {
    x: x + 0.1, y: y + 0.9, w: w - 0.2, h: 0.35,
    fontSize: 14, fontFace: theme.HEADER_FONT, color: theme.TEXT_DARK,
    bold: true, align: 'center', valign: 'middle', margin: 0
  });
  // Stat value
  slide.addText(statValue, {
    x: x + 0.1, y: y + 1.25, w: w - 0.2, h: 0.5,
    fontSize: 20, fontFace: theme.HEADER_FONT, color: theme.PRIMARY,
    bold: true, align: 'center', valign: 'middle', margin: 0
  });
  // Growth label
  if (growthLabel) {
    addGrowthPill(slide, pres, x + w/2 - 0.47, y + 1.8, growthLabel);
  }
  // Context
  if (contextLabel) {
    slide.addText(contextLabel, {
      x: x + 0.1, y: y + 2.15, w: w - 0.2, h: 0.3,
      fontSize: 11, fontFace: theme.BODY_FONT, color: theme.TEXT_MUTED,
      align: 'center', valign: 'middle', margin: 0
    });
  }
}

// ── Guidance Callout Box ──
function addGuidanceBox(slide, pres, x, y, w, label) {
  slide.addShape(pres.shapes.ROUNDED_RECTANGLE, {
    x, y, w, h: 0.55,
    rectRadius: 0.08,
    fill: { color: theme.SECONDARY }
  });
  slide.addText(label, {
    x: x + 0.1, y, w: w - 0.2, h: 0.55,
    fontSize: 12, fontFace: theme.BODY_FONT, color: theme.TEXT_LIGHT,
    bold: true, align: 'center', valign: 'middle', margin: 0
  });
}

// ── Orange Section Divider ──
function addSectionDivider(slide, pres, title) {
  slide.background = SECTION_BG;
  slide.addText(title, {
    x: 0.7, y: 1.8, w: 8.6, h: 2.0,
    fontSize: 56, fontFace: theme.HEADER_FONT, color: theme.TEXT_LIGHT,
    bold: true, align: 'left', valign: 'middle', margin: 0, italic: true
  });
}

// ── Orange Chart Header Bar ──
function addChartHeaderBar(slide, pres, x, y, w, title) {
  slide.addShape(pres.shapes.ROUNDED_RECTANGLE, {
    x, y, w, h: 0.45,
    rectRadius: 0.06,
    fill: { color: theme.PRIMARY }
  });
  slide.addText(title, {
    x: x + 0.15, y, w: w - 0.3, h: 0.45,
    fontSize: 14, fontFace: theme.HEADER_FONT, color: theme.TEXT_LIGHT,
    bold: true, align: 'center', valign: 'middle', margin: 0
  });
}

// ── Checkmark Badge (Guidance Tables) ──
function addCheckBadge(slide, pres, x, y) {
  slide.addShape(pres.shapes.OVAL, {
    x, y, w: 0.3, h: 0.3,
    fill: { color: theme.EXCEEDED }
  });
  slide.addText('✓', {
    x, y, w: 0.3, h: 0.3,
    fontSize: 14, fontFace: theme.BODY_FONT, color: theme.TEXT_LIGHT,
    bold: true, align: 'center', valign: 'middle', margin: 0
  });
}
```

### Stacked Bar Chart (Revenue Breakdown)

```javascript
// Revenue breakdown stacked bar
const revenueData = [
  { name: 'Commission fees',           labels: years, values: commissionData },
  { name: 'Subscription & Other',      labels: years, values: subscriptionData },
  { name: 'Delivery & Service Fees',   labels: years, values: deliveryData },
  { name: 'Advertising & listing fees', labels: years, values: advertisingData },
];

slide.addChart(pres.charts.BAR, revenueData, {
  x: chartX, y: chartY, w: chartW, h: chartH,
  barGrouping: 'stacked',
  chartColors: theme.CHART_COLORS.slice(0, 4),  // 3D1F00, 6B4226, FF5722, 7AB648
  showLegend: true,
  legendPos: 'b',
  legendColor: theme.TEXT_MUTED,
  legendFontSize: 9,
  catAxisLabelColor: theme.TEXT_MUTED,
  valAxisLabelColor: theme.TEXT_MUTED,
  catAxisLabelFontSize: 10,
  valAxisLabelFontSize: 10,
  catGridLine: { style: 'none' },
  valGridLine: { style: 'none' },
  catAxisLineShow: false,
  valAxisLineShow: false,
  showValue: false,
  barGapWidthPct: 80,
});
```

