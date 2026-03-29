# Pure Minimal Design System — Enhanced Edition

"Less, but better" — Dieter Rams. Every element must justify its existence. Whitespace is active, not empty. Typography carries the emotional weight.

This is a Swiss-inspired minimalist design system. Use it when the user asks for minimal, clean, Swiss, Dieter Rams, or elegant slides — or when the audience is design-literate, C-suite, investor-grade, or academic.

**Important**: Pure Minimal uses the same layout library as Magazine mode (`references/layouts.md`, layouts 1–20) but applies its own styling rules, color system, and typography. This file defines HOW each layout should look in Pure Minimal mode — not separate layout patterns.

---

## Color System

Flat fields only. No gradients. No shadows heavier than a single-level card shadow. No outlines except structural dividers.

### Base

```
BLACK:        1A1A1A   (soft black — primary text on light)
WHITE:        FFFFFF   (background, primary text on dark)
WARM_WHITE:   FAFAFA   (alternate background — card container bg)
OFF_WHITE:    F5F5F5   (card/panel fill on white slides)
```

### Grays (Maximum 4 steps)

```
TEXT:          333333   (primary body text)
SECONDARY:    888888   (annotations, labels, muted content)
TERTIARY:     CCCCCC   (rules, dividers, very light elements)
CARD_BORDER:  E8E8E8   (subtle card edge separation — 1px feel)
```

### Accent (Choose ONE primary accent per deck — never mix)

| Accent | Hex | Signal | When To Use |
|--------|-----|--------|-------------|
| Signal Red | FF3B30 | Urgency, warnings, disruption | Pitch decks challenging status quo |
| Electric Blue | 007AFF | Trust, technology, precision | Tech, SaaS, fintech, data analytics |
| Warm Amber | FF9500 | Energy, optimism, warmth | Growth stories, launches |
| Forest Green | 34C759 | Growth, sustainability, health | ESG, healthcare, environment |
| Coral | F96167 | Warm confidence, approachable authority | Investment analysis, market reports |

### Semantic Colors (For Data-Heavy Slides Only)

When presenting graded, categorized, or risk-tiered data, a controlled set of semantic colors is permitted — but only within structured cards or metric rows. These never appear in backgrounds, titles, or body text.

```
GRADE_A:      34C759   (green — top tier, best performance)
GRADE_B:      007AFF   (blue — good, solid)
GRADE_C:      FF9500   (amber — moderate, caution)
GRADE_D:      FF3B30   (red — underperforming, risk)
GRADE_NEUTRAL: 888888  (gray — ungraded, baseline)
```

**Rules for semantic colors**:
- Only inside card borders, badge circles, or metric value text
- Maximum 5 semantic colors on a single slide
- Must have clear legend or self-evident labeling (A+ / B / C etc.)
- Never used for backgrounds larger than a badge (0.5" circle or 1.5" × 0.3" pill)

### Contrast Modes

| Mode | Background | Text | Accent Use |
|------|-----------|------|------------|
| **Light** (default) | FFFFFF or FAFAFA | 1A1A1A / 333333 | Sparingly — stat numbers, single keyword highlights |
| **Dark** | 1A1A1A | FFFFFF | Accent for data or key statement only |
| **Accent Hero** | Accent color | FFFFFF | One slide maximum per deck (thesis statement or verdict) |
| **Panel** | F5F5F5 | 333333 | For card-grid and metric-panel slides — structured data feel |

---

## Typography System (Swiss Hierarchy)

### Available Fonts

| Voice | Header Font | Body Font | Monospace |
|-------|-------------|-----------|-----------|
| **Technical / Geometric** | Calibri | Calibri Light | Consolas |
| **Humanist / Elegant** | Trebuchet MS | Calibri | Consolas |
| **Classic Swiss** | Arial | Arial | Consolas |

### Type Scale (8pt baseline grid)

| Element | Size | Weight | Tracking | Color |
|---------|------|--------|----------|-------|
| Slide title | 28-32pt | Bold | Normal | 1A1A1A |
| Category label (above title) | 11pt | Bold | charSpacing: 4 | 888888 |
| Subtitle / header | 24pt | Normal | Normal | 333333 |
| Section header (in-card) | 20pt | Bold | Normal | 1A1A1A |
| Body text | 16pt | Normal | Normal | 333333 |
| Data labels | 14pt | Bold | charSpacing: 3 | 888888 |
| Annotations / source | 11pt | Normal | Normal | 888888 |
| Giant data number | 120pt | Bold | Normal | Accent or 1A1A1A |
| Card stat number | 48-60pt | Bold | Normal | Accent or 1A1A1A |
| Metric value (in rows) | 28-36pt | Bold | Normal | 1A1A1A |
| Badge text | 14pt | Bold | Normal | FFFFFF |

### Title / Category Label Positioning (Anti-Overlap)

**CRITICAL**: Category labels and slide titles overlap when positioned too close. Use these exact positions:

```javascript
// Category label — small caps above the title
function addCategoryLabel(slide, text, y = 0.35) {
  slide.addText(text.toUpperCase(), {
    x: 0.7, y, w: 8.6, h: 0.25,
    fontSize: 11, fontFace: HEADER_FONT, color: '888888',
    bold: true, charSpacing: 4, align: 'left', valign: 'top',
  });
}

// Slide title — below category label with breathing room
function addSlideTitle(slide, text, y = 0.65) {
  slide.addText(text, {
    x: 0.7, y, w: 8.6, h: 0.7,
    fontSize: 28, fontFace: HEADER_FONT, color: '1A1A1A',
    bold: true, align: 'left', valign: 'top',
  });
}
```

**Key spacing rules:**
- Category label: y=0.35, h=0.25 → bottom at y=0.60
- Title: y=0.65, h=0.70 → bottom at y=1.35
- Gap between label bottom and title top: **0.05"** (minimum)
- First content element: y≥1.45 → **0.10" gap** below title bottom
- Slide title max size: **28-32pt** (44pt causes overflow on long titles)
- Never use `fontSize: 44` for slide titles unless the title is ≤3 words

### Typography Rules

- **Alignment**: Strict left-align or centered. Never justify.
- **Maximum 2 font weights per slide** (e.g., Bold + Regular)
- **No underlines** — use space or accent color for emphasis
- **Category labels**: ALL CAPS, tracked wide (charSpacing: 3-4), placed ABOVE the slide title in 888888. Max 3 words.
- **Bold inline keywords**: Bold the key term at the start of a point. Use accent color for inline data highlights.

---

## Layout Grid

### Dimensions (10" × 5.625")

```
MARGIN:         0.7"
CONTENT_X:      0.7"
CONTENT_W:      8.6"
CONTENT_Y:      0.5"
CONTENT_H:      4.6"
SPACING:        0.24"  (base unit — use multiples: 0.24, 0.48, 0.72)
CARD_PADDING:   0.3"
CARD_GAP:       0.2"
CARD_RADIUS:    0       (sharp corners only — 90° is canonical)
```

### The 35% Rule

**Standard slides**: Minimum 40% of the slide must remain untouched.
**Data / metric slides**: Minimum 35% — structured cards and metric grids need more real estate.

---

## Card System

Cards are the fundamental container for structured data. They provide visual grouping without decoration.

### Card Construction

```javascript
const makeCard = (slide, pres, x, y, w, h, opts = {}) => {
  const { fill = "FFFFFF", accentSide = null, accentColor = "007AFF" } = opts;
  
  slide.addShape(pres.shapes.RECTANGLE, {
    x, y, w, h,
    fill: { color: fill },
    shadow: { type: "outer", blur: 4, offset: 1, angle: 270, color: "000000", opacity: 0.08 }
  });
  
  if (accentSide === "left") {
    slide.addShape(pres.shapes.RECTANGLE, {
      x, y, w: 0.04, h,
      fill: { color: accentColor }
    });
  }
  if (accentSide === "top") {
    slide.addShape(pres.shapes.RECTANGLE, {
      x, y, w, h: 0.04,
      fill: { color: accentColor }
    });
  }
};
```

### Card Variants

| Variant | Fill | Border | When |
|---------|------|--------|------|
| **Default** | FFFFFF | shadow only | Most uses |
| **Muted** | F5F5F5 | none | Secondary info |
| **Accent-left** | FFFFFF | 4px left bar | Categories, numbered items |
| **Accent-top** | FFFFFF | 4px top bar | Grid items, features |
| **Hero card** | Accent color | none | Verdict, key decision (max 1 per slide) |

---

## Badge System

### Circle Badge

```javascript
const makeBadge = (slide, pres, x, y, label, color = "007AFF") => {
  slide.addShape(pres.shapes.OVAL, {
    x, y, w: 0.45, h: 0.45,
    fill: { color }
  });
  slide.addText(label, {
    x, y, w: 0.45, h: 0.45,
    fontSize: 14, fontFace: HEADER_FONT, color: "FFFFFF",
    bold: true, align: "center", valign: "middle", margin: 0
  });
};
```

### Grade Pill

```javascript
const makeGradePill = (slide, pres, x, y, grade, color) => {
  slide.addShape(pres.shapes.ROUNDED_RECTANGLE, {
    x, y, w: 0.6, h: 0.32,
    fill: { color },
    rectRadius: 0.16
  });
  slide.addText(grade, {
    x, y, w: 0.6, h: 0.32,
    fontSize: 13, fontFace: HEADER_FONT, color: "FFFFFF",
    bold: true, align: "center", valign: "middle", margin: 0
  });
};
```

**Exception**: Rounded corners ONLY on badges and pills (radius ≤ 0.2"). All other shapes use sharp 90° corners.

---

## Pure Minimal Style Overrides for Shared Layouts

All 20 layouts from `references/layouts.md` are available in Pure Minimal mode. Apply these style transformations when rendering them:

### Global Overrides (Apply to ALL layouts)

| Property | Magazine Mode | Pure Minimal Override |
|----------|-------------|----------------------|
| Background | Theme-colored (PRIMARY, BG_DARK, etc.) | FFFFFF, FAFAFA, or 1A1A1A only |
| Card corners | `rectRadius: 0.1` | `rectRadius: 0` (sharp 90°) |
| Card shadow | `blur: 6, opacity: 0.12-0.15` | `blur: 4, offset: 1, opacity: 0.08` |
| Text color | Theme TEXT_DARK / TEXT_LIGHT | 1A1A1A / 333333 / 888888 only |
| Accent color | Theme ACCENT (multi-color OK) | Single chosen accent — never mix |
| Header font | Theme HEADER_FONT | Calibri, Trebuchet MS, or Arial |
| Body font | Theme BODY_FONT | Calibri Light, Calibri, or Arial |
| Icon circles | Filled with ACCENT_COLOR | Filled with single accent or F5F5F5 |
| Decorative shapes | Color blocks, accent strips | Remove — let whitespace do the work |
| Category label | Not used | ALL CAPS, charSpacing: 4, placed ABOVE title |

### Per-Layout Overrides

| Layout | Pure Minimal Treatment |
|--------|----------------------|
| **1. Title Hero** | White bg, not dark. Typography only. No accent bar. No shapes. Category label above title. Date/author at bottom in CCCCCC. This becomes the "Void" slide. |
| **2. Section Divider** | White bg with category label + bold title. Number in accent badge, not large text. No colored background. |
| **3. Two-Column Split** | Use golden ratio (61.8/38.2) not 50/50 or 55/45. Left text, right visual zone in muted card. |
| **4. Icon Row** | Icons in F5F5F5 circles (not bold accent circles). Labels left-aligned or centered. Wrap in a single `makeCard()`. |
| **5. Stat Callout** | Becomes "Data Reveal" — ONE giant number (120pt) centered. Context label in tracked caps. Max 1 stat per slide. If 2-4 stats needed, use card-based Metric Row Panel approach. |
| **6. Card Grid** | Sharp-cornered cards with `makeCard()`. Accent-top bars for categorization. Use grade colors for tier comparisons (Graded Card Row). |
| **7. Timeline / Process** | Numbered circle badges in accent. Connecting lines in CCCCCC (1pt). Labels below in 333333. |
| **8. Chart + Insight** | Chart: no gridlines, no axis lines, accent color only. Insight panel in makeCard() with muted fill. |
| **9. Quote** | Centered on white. No decorative quote marks. Italic text, 24pt. Attribution with em-dash prefix in 888888. |
| **10. Image + Overlay** | Use makeCard() for the image placeholder zone. B&W or duotone images only — no color stock photography on standard slides. |
| **11. Table Slide** | No alternating row colors. Use thin 1pt CCCCCC row dividers. Header row in 1A1A1A with FFFFFF text. |
| **12. Closing / CTA** | Mirrors Title Hero (Void). White bg. Thesis restatement — NEVER "Thank You". Contact info in CCCCCC. This is the "Echo" slide. |
| **13. Agenda / Overview** | Use numbered badge cards in a 3×3 grid. Each card with accent-left bar. |
| **14. Before / After** | Becomes "Dichotomy" — left half in 1A1A1A (dark), right half in FFFFFF (light). Let color contrast do the comparison. |
| **15. Multi-Period Table** | Sharp corners. No colored column headers — use 1A1A1A header with FFFFFF text for all columns. Differentiate periods with subtle fills (FFFFFF vs F5F5F5 vs FAFAFA). |
| **16. KPI Card Row** | Cards in FFFFFF (not dark). Stat numbers in accent or 1A1A1A. No colored growth indicators — use plain text with ↑/↓ arrows. Max 4 cards. |
| **17. Strategy Dashboard** | Hero chart uses accent color only, no gridlines. Mini-metric cards in makeCard() with accent-top bars. Muted fills (F5F5F5) for card backgrounds. |
| **18. Product Feature** | Phone mockup uses 1A1A1A frame (no gradients). Feature list with accent circle bullets. Stats in plain text — no colored stat circles. |
| **19. Bold Section Divider** | In Pure Minimal, either use (a) white bg with massive 1A1A1A text, or (b) 1A1A1A bg with FFFFFF text. Never use accent color as full-bleed background except on ONE Accent Hero slide per deck. |
| **20. PoP Bar Comparison** | Bars in accent + CCCCCC (not two bold theme colors). Growth callout uses 1A1A1A oval with FFFFFF text. Panels have no colored headers — use 1A1A1A header bar. Insight text in plain cards. |

---

## Additional Pure Minimal Slide Types

These are Pure Minimal–specific patterns that don't have a direct Magazine layout equivalent. Use them alongside the shared layouts:

### Single Statement (Thesis Slide)

A full-screen sentence at the golden ratio line. Use instead of (or in addition to) Layout 9 (Quote) for internal thesis statements.

```javascript
slide.background = { color: "FFFFFF" };
slide.addText(thesis, {
  x: 1.0, y: 1.7, w: 8, h: 2.0,
  fontSize: 36, fontFace: HEADER_FONT, color: "1A1A1A",
  bold: true, align: "center", margin: 0
});
// Accent Hero variant (ONE per deck max):
// slide.background = { color: ACCENT };
// color: "FFFFFF"
```

### Verdict / Decision Slide

For the "go / no-go" moment. One hero-colored banner at top, structured details below.

```javascript
slide.background = { color: "FFFFFF" };
// Hero verdict banner (accent bg, full width)
slide.addShape(pres.shapes.RECTANGLE, {
  x: 0.7, y: 1.6, w: 8.6, h: 1.0,
  fill: { color: "34C759" }  // green=GO, FF3B30=NO-GO, FF9500=CONDITIONAL
});
slide.addText(verdictLabel, {
  x: 1.0, y: 1.7, w: 5, h: 0.7,
  fontSize: 28, fontFace: HEADER_FONT, color: "FFFFFF",
  bold: true, align: "left", margin: 0
});
// Below: two-column detail in cards with accent-left bars
```

**Rules**: Only ONE verdict slide per deck. The banner is the ONLY element on this slide that uses a colored fill larger than a badge.

### Stacked Alert Cards (Limitations / Risks)

For presenting 3-5 limitations, risks, or caveats in vertically stacked cards with icon accent.

```javascript
slide.background = { color: "FFFFFF" };
// Left zone: stacked cards (55% width)
limitations.forEach((lim, i) => {
  const ly = 1.7 + i * 0.85;
  makeCard(slide, pres, 0.7, ly, 5.5, 0.7, { fill: "FFF8F0" });
  slide.addImage({ data: lim.iconBase64, x: 0.9, y: ly + 0.15, w: 0.35, h: 0.35 });
  slide.addText(lim.title, {
    x: 1.4, y: ly + 0.08, w: 4.5, h: 0.3,
    fontSize: 14, fontFace: HEADER_FONT, color: "1A1A1A",
    bold: true, align: "left", margin: 0
  });
  slide.addText(lim.desc, {
    x: 1.4, y: ly + 0.38, w: 4.5, h: 0.25,
    fontSize: 11, fontFace: BODY_FONT, color: "888888",
    align: "left", margin: 0
  });
});
// Right zone: image placeholder or supporting visual (40% width)
```

---

## Narrative Flow

### Standard (Non-Data) Decks

1. **Layout 1 (Void)** — Title slide, typography only
2. **Single Statement** — Problem in one sentence
3. **Layout 5 (Data Reveal)** — The number that changes everything
4. **Layout 3 (Golden Split)** — 3 supporting points, one per slide
5. **Layout 12 (Echo)** — Closing statement, thesis restatement

### Analytical / Investment Decks

1. **Layout 1 (Void)** — Title
2. **Layout 13 (Agenda Grid)** — Numbered sections
3. **Layout 3 (Golden Split)** — Introduction / Context
4. **Layout 3 (Two-Zone)** — Problem + Objectives
5. **Layout 16 (KPI Card Row)** — Dataset overview / Key figures
6. **Layout 3 or 17** — Methodology / Model selection
7. **Layout 6 (Graded Card Row)** — Results / Tier comparison
8. **Layout 16 (KPI Card Row)** — Financial opportunity
9. **Stacked Alert Cards** — Biases / Limitations
10. **Verdict** — Recommendation
11. **Layout 12 (Echo)** — Closing

---

## Forbidden Elements (Auto-reject)

These elements violate the Pure Minimal philosophy and must never appear:

- Drop shadows heavier than `opacity: 0.08`
- Glow effects or gradients
- Rounded corners (except badges/pills ≤ 0.2" radius)
- Bullet points with more than 6 words each
- Stock photography in color on standard slides (B&W or duotone only)
- Underlines or decorative shapes
- Page numbers (unless essential)
- "Thank you" slides — end with thesis restatement
- More than ONE primary accent color per deck
- Accent lines under titles
- Colored backgrounds (only Dark mode, Accent Hero, and Verdict slides may use fills)

---

## Permitted Elements

| Element | Condition |
|---------|-----------|
| Card shadows | `opacity: 0.08` max, `blur: 4`, `offset: 1` |
| Colored badges / pills | ≤ 0.6" wide, rounded corners ≤ 0.2" radius |
| Icons in colored circles | Within cards or feature rows only |
| Accent-colored left bars on cards | 4px wide maximum |
| Color-coded hero banner | ONE per deck maximum |
| Semantic grade colors | Within card borders only |
| Charts (bar, line, doughnut) | No gridlines, muted axis labels, accent colors only |

---

## Minimal Chart Guidelines

### Line Chart

```javascript
slide.addChart(pres.charts.LINE, [{ name: "Metric", labels: years, values: dataPoints }], {
  x: 1.5, y: 1.5, w: 7, h: 3.5,
  lineSize: 3, chartColors: [ACCENT],
  showLegend: false, showTitle: false,
  catAxisLabelColor: "888888", valAxisLabelColor: "888888",
  catGridLine: { style: "none" }, valGridLine: { style: "none" },
  catAxisLineShow: false, valAxisLineShow: false, showValue: false,
});
```

### Bar Chart Rules

- Maximum 5 bars, single color (accent) or semantic grade colors
- No gridlines, no axis lines, label directly on or near each bar
- Bar gap: 75% (generous spacing)

### Doughnut Chart

- `holeSize: 65`, accent + E8E8E8 (gray for remainder)
- No more than 4 segments

---

## Content Discipline

### The "One Breath" Rule

Speaker notes: 25 words max per bullet, 50 words max total per slide note.

### Pacing

Pure Minimal decks are slower — 45 to 90 seconds per slide. A 10-minute talk needs ~8-12 slides. Data-heavy analytical decks may run 12-15 slides for 15 minutes.
