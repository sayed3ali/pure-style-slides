# Layout Library

A library of slide layout patterns to create varied, magazine-quality presentations. Use at least 4 different layouts across a 10-slide deck. Never repeat the same layout on consecutive slides.

## Layout Index

| # | Layout | Best For | Visual Weight |
|---|--------|----------|---------------|
| 1 | Title Hero | Opening slide | Heavy |
| 2 | Section Divider | Chapter breaks | Medium |
| 3 | Two-Column Split | Comparison, text+visual | Medium |
| 4 | Icon Row | Features, benefits, values | Medium |
| 5 | Stat Callout | Key metrics, KPIs | Heavy |
| 6 | Card Grid | Team, products, categories | Medium |
| 7 | Timeline / Process | Steps, phases, history | Medium |
| 8 | Chart + Insight | Data with commentary | Medium |
| 9 | Quote / Testimonial | Quotes, key messages | Light |
| 10 | Image + Overlay | Visual storytelling | Heavy |
| 11 | Table Slide | Comparisons, specifications | Light |
| 12 | Closing / CTA | Final slide, call to action | Heavy |
| 13 | Agenda / Overview | Table of contents | Light |
| 14 | Before / After | Transformations, improvements | Medium |
| 15 | Multi-Period Comparison Table | Guidance, budget vs actuals, earnings | Medium |
| 16 | KPI Card Row | Headline metrics with icons in dark cards | Heavy |
| 17 | Strategy Dashboard | Hero chart + mini-metric grid | Heavy |
| 18 | Product Feature Showcase | App features, device mockup + stats | Medium |
| 19 | Bold Section Divider | Full-bleed dramatic section break | Heavy |
| 20 | Period-over-Period Bar Comparison | Side-by-side bar panels with growth callouts | Medium |

---

## 1. Title Hero

**Purpose**: Opening slide. Sets the tone for the entire deck.

**Structure**:
- Full dark background (primary color or near-black)
- Large title text (44pt, bold, white/light)
- Subtitle / tagline (18-20pt, muted accent color)
- Optional: date, presenter name (12pt, bottom)
- Optional: thin accent line or shape element

**Layout** (10" × 5.625"):
```
┌──────────────────────────────────┐
│                                  │
│     [TITLE - 44pt bold]          │
│     [Subtitle - 20pt]           │
│                                  │
│     [Date / Author - 12pt]       │
│──────────────────────────────────│
│  [accent bar - full width, 0.05"]│
└──────────────────────────────────┘
```

**Code pattern**:
```javascript
slide.background = { color: PRIMARY_DARK };
slide.addText(title, {
  x: 0.8, y: 1.5, w: 8.4, h: 1.5,
  fontSize: 44, fontFace: HEADER_FONT, color: "FFFFFF",
  bold: true, align: "left"
});
slide.addText(subtitle, {
  x: 0.8, y: 3.0, w: 8.4, h: 0.8,
  fontSize: 20, fontFace: BODY_FONT, color: ACCENT_COLOR
});
```

---

## 2. Section Divider

**Purpose**: Visual break between sections. Creates rhythm in the deck.

**Structure**:
- Colored background (primary or accent)
- Section number or icon (large, top area)
- Section title (36pt, bold)
- Brief description (14pt, muted)

**Layout**:
```
┌──────────────────────────────────┐
│                                  │
│     01                           │
│     ─────                        │
│     [SECTION TITLE]              │
│     [Brief description]          │
│                                  │
└──────────────────────────────────┘
```

---

## 3. Two-Column Split

**Purpose**: Most versatile content layout. Text on one side, visual on the other.

**Variants**:
- Text left, icon/chart right (default)
- Text left, colored block right
- Text right, visual left (alternate for variety)

**Structure**:
```
┌──────────────────────────────────┐
│  [Title - full width]            │
│                                  │
│  ┌──────────┐  ┌──────────────┐ │
│  │  Text     │  │  Visual      │ │
│  │  content  │  │  (chart,     │ │
│  │  bullets  │  │   icon grid, │ │
│  │           │  │   image)     │ │
│  └──────────┘  └──────────────┘ │
└──────────────────────────────────┘
```

**Proportions**: 55/45 or 50/50 split. Left column x: 0.5-4.5, Right column x: 5.0-9.5

---

## 4. Icon Row

**Purpose**: Present 3-4 features, benefits, or values in a horizontal row.

**Structure**:
- Title at top
- 3-4 columns, each with: icon in colored circle → bold label → description
- Equal spacing between columns

**Layout** (3-column version):
```
┌──────────────────────────────────┐
│  [Title]                         │
│                                  │
│  ┌────────┐ ┌────────┐ ┌──────┐ │
│  │  (●)   │ │  (●)   │ │ (●)  │ │
│  │ Label  │ │ Label  │ │Label │ │
│  │ Desc   │ │ Desc   │ │Desc  │ │
│  └────────┘ └────────┘ └──────┘ │
└──────────────────────────────────┘
```

**Icon sizing**: Circle diameter 0.7", icon image 0.4" inside the circle, centered.

**Code pattern for icon in circle**:
```javascript
// Colored circle background
slide.addShape(pres.shapes.OVAL, {
  x: colX + (colW - 0.7) / 2, y: iconY, w: 0.7, h: 0.7,
  fill: { color: ACCENT_COLOR }
});
// Icon PNG on top
slide.addImage({
  data: iconBase64, 
  x: colX + (colW - 0.4) / 2, y: iconY + 0.15, w: 0.4, h: 0.4
});
```

---

## 5. Stat Callout

**Purpose**: Highlight 2-4 key metrics with large numbers.

**Structure**:
- Title at top
- 2-4 stat blocks in a row, each with: large number (48-72pt) → unit/label → context line
- Optional: thin colored bar above each stat for visual separation

**Layout** (3-stat version):
```
┌──────────────────────────────────┐
│  [Title]                         │
│                                  │
│  ┌────────┐ ┌────────┐ ┌──────┐ │
│  │  42%   │ │  $1.2B │ │ 10M  │ │
│  │ Growth │ │Revenue │ │Users │ │
│  │ YoY    │ │ FY2025 │ │ DAU  │ │
│  └────────┘ └────────┘ └──────┘ │
└──────────────────────────────────┘
```

**Typography**: Numbers in 60-72pt bold accent color. Labels in 14-16pt muted.

---

## 6. Card Grid

**Purpose**: Present team members, products, categories, or any collection of items.

**Structure**:
- Title at top
- 2×2 or 1×3 grid of white cards with shadow on colored background
- Each card: optional icon or image → bold title → description
- Optional: thin accent bar on left edge of each card

**Card code pattern**:
```javascript
const makeCardShadow = () => ({
  type: "outer", blur: 6, offset: 2, angle: 135,
  color: "000000", opacity: 0.12
});

slide.addShape(pres.shapes.RECTANGLE, {
  x: cardX, y: cardY, w: cardW, h: cardH,
  fill: { color: "FFFFFF" },
  shadow: makeCardShadow()
});
// Accent bar on left
slide.addShape(pres.shapes.RECTANGLE, {
  x: cardX, y: cardY, w: 0.06, h: cardH,
  fill: { color: ACCENT_COLOR }
});
```

---

## 7. Timeline / Process

**Purpose**: Show sequential steps, phases, or history.

**Structure**:
- Title at top
- Horizontal flow: numbered circles connected by lines
- Labels below each circle
- Brief description below labels

**Layout**:
```
┌──────────────────────────────────┐
│  [Title]                         │
│                                  │
│   (1)──────(2)──────(3)──────(4) │
│   Step 1    Step 2   Step 3  S4  │
│   Desc      Desc     Desc   Desc │
│                                  │
└──────────────────────────────────┘
```

**Code pattern**: Use circles (OVAL) with numbers, connect with lines (LINE shape, w = distance, h = 0).

---

## 8. Chart + Insight

**Purpose**: Present data with a chart AND a key takeaway.

**Structure**:
- Title at top
- Chart on the left (60% width)
- Key insight box on the right (30% width) — colored background, large stat, brief text
- Source line at bottom

**Layout**:
```
┌──────────────────────────────────┐
│  [Title]                         │
│                                  │
│  ┌──────────────┐  ┌──────────┐ │
│  │              │  │ KEY STAT │ │
│  │    CHART     │  │          │ │
│  │              │  │ Insight  │ │
│  │              │  │ text     │ │
│  └──────────────┘  └──────────┘ │
│  Source: ...                     │
└──────────────────────────────────┘
```

---

## 9. Quote / Testimonial

**Purpose**: Highlight a key quote, testimonial, or important message.

**Structure**:
- Centered layout, generous whitespace
- Large quote marks (decorative, accent color, 60pt or shape)
- Quote text (20-24pt, italic or regular)
- Attribution (14pt, muted, below)

**Layout**:
```
┌──────────────────────────────────┐
│                                  │
│           ❝                      │
│     [Quote text, centered,       │
│      20-24pt]                    │
│                                  │
│     — Attribution, 14pt          │
│                                  │
└──────────────────────────────────┘
```

---

## 10. Image + Overlay

**Purpose**: Full or half-bleed image with text overlay. Use when you have or plan to add images.

**Variants**:
- Full image background + dark overlay + white text
- Half image (left or right) + content on other side
- Image with colored overlay bar at bottom

**For image placeholders** (when user will add images later):
```javascript
// Placeholder rectangle
slide.addShape(pres.shapes.RECTANGLE, {
  x: 0, y: 0, w: 5, h: 5.625,
  fill: { color: "E0E0E0" }
});
slide.addText("[Your Image Here]", {
  x: 0.5, y: 2.5, w: 4, h: 0.6,
  fontSize: 14, color: "999999", align: "center"
});
```

---

## 11. Table Slide

**Purpose**: Structured comparison or specifications.

**Structure**:
- Title at top
- Styled table with colored header row
- Alternating row colors for readability

**Table styling**:
```javascript
const headerRow = [
  { text: "Feature", options: { fill: { color: PRIMARY }, color: "FFFFFF", bold: true } },
  { text: "Basic", options: { fill: { color: PRIMARY }, color: "FFFFFF", bold: true } },
  { text: "Pro", options: { fill: { color: PRIMARY }, color: "FFFFFF", bold: true } }
];
```

---

## 12. Closing / CTA

**Purpose**: Final slide. Call to action, contact info, or thank you.

**Structure**: Similar to Title Hero but with:
- "Thank You" or CTA text
- Contact details / next steps
- Same dark background as title slide (bookend effect)

---

## 13. Agenda / Overview

**Purpose**: Table of contents or overview of what the deck covers.

**Structure**:
- Title: "Agenda" or "What We'll Cover"
- Numbered list with bold section names and brief descriptions
- Clean, spacious layout

---

## 14. Before / After

**Purpose**: Show transformation, improvement, or comparison.

**Structure**:
- Title at top
- Two equal columns: "Before" (muted/gray tones) and "After" (vibrant/accent colors)
- Each column: label + content/stats
- Visual distinction through color coding

---

## 15. Multi-Period Comparison Table

**Purpose**: Compare performance across time periods with multiple metrics. Common in earnings calls, quarterly reviews, and guidance updates. Inspired by financial results tables showing Original Guidance → Actuals → Revised Guidance.

**Structure**:
- Title at top
- Full-width table with 3-4 columns, each with distinct header color
- Each row: metric label (left) + values per period (right)
- Each cell can contain a headline number + supporting detail below it
- Column headers use different background colors to distinguish periods

**Layout**:
```
┌──────────────────────────────────────────┐
│  [Title]                                 │
│                                          │
│  ┌──────────┬──────────┬──────────┬─────┐│
│  │ Metric   │ Period 1 │ Period 2 │ P3  ││
│  │          │ (muted)  │ (accent) │(pri)││
│  ├──────────┼──────────┼──────────┼─────┤│
│  │ GMV      │ 17-18%   │ 33%      │27%  ││
│  │          │ $8.7bn   │ $4.6bn   │$9.4 ││
│  ├──────────┼──────────┼──────────┼─────┤│
│  │ Revenue  │ 18-20%   │ 37%      │29%  ││
│  │          │ $3.5bn   │ $1.9bn   │$3.8 ││
│  └──────────┴──────────┴──────────┴─────┘│
│  [Source / footnotes - 9pt]              │
└──────────────────────────────────────────┘
```

**Code pattern**:
```javascript
// Column headers with different colors per period
const headerColors = [TEXT_MUTED, SECONDARY, ACCENT_COLOR, PRIMARY];
const headers = ['Performance Measure', 'Guidance FY25', 'Actuals H1', 'Revised FY25'];

const headerRow = headers.map((text, i) => ({
  text, options: {
    fill: { color: i === 0 ? 'F5F5F5' : headerColors[i] },
    color: i === 0 ? TEXT_DARK : 'FFFFFF',
    bold: true, fontSize: 11, fontFace: HEADER_FONT,
    align: i === 0 ? 'left' : 'center',
  }
}));

// Data rows with dual-line cells (headline + detail)
const makeCell = (headline, detail, isLabel = false) => ({
  text: [
    { text: headline + '\n', options: { fontSize: 13, bold: true, color: TEXT_DARK } },
    { text: detail, options: { fontSize: 10, color: TEXT_MUTED } }
  ],
  options: {
    fill: { color: isLabel ? 'F5F5F5' : 'FFFFFF' },
    align: isLabel ? 'left' : 'center',
    valign: 'middle',
  }
});

slide.addTable([headerRow, ...dataRows], {
  x: 0.4, y: 1.2, w: 9.2,
  border: { type: 'solid', pt: 0.5, color: 'E0E0E0' },
  rowH: [0.45, 0.7, 0.7, 0.7, 0.7, 0.7],
  colW: [2.8, 2.1, 2.1, 2.1],
});
```

**When to use**: Quarterly reviews, earnings calls, budget vs. actuals, guidance revisions, any multi-period financial comparison.

---

## 16. KPI Card Row

**Purpose**: Present 3-4 key performance indicators in dark cards with icons, large stat numbers, growth indicators, and labels. Inspired by earnings presentations' "key highlights" slides.

**Structure**:
- Title at top
- 3-4 dark-background cards in a horizontal row
- Each card: small icon (top) → unit label → large stat number → growth percentage → metric label
- Optional: footer bar below cards for additional context (e.g., dividends, notes)

**Layout** (4-card version):
```
┌──────────────────────────────────────────┐
│  [Title]                                 │
│                                          │
│  ┌────────┐ ┌────────┐ ┌────────┐ ┌───┐ │
│  │  [icon]│ │  [icon]│ │  [icon]│ │[i]│ │
│  │  USD   │ │  USD   │ │  USD   │ │USD│ │
│  │  2.5bn │ │  986mn │ │  166mn │ │116│ │
│  │ +33%   │ │ +36%   │ │ +31%   │ │+25│ │
│  │  GMV   │ │Revenue │ │EBITDA  │ │NI │ │
│  └────────┘ └────────┘ └────────┘ └───┘ │
│                                          │
│  ┌──────────────────────────────────────┐│
│  │ [Footer context bar — optional]      ││
│  └──────────────────────────────────────┘│
└──────────────────────────────────────────┘
```

**Code pattern**:
```javascript
const kpis = [
  { icon: { cat: 'business', idx: 15 }, unit: 'USD (cFX)', value: '2.5bn', growth: '+33% y/y', label: 'GMV' },
  { icon: { cat: 'business', idx: 16 }, unit: 'USD (cFX)', value: '986mn', growth: '+36% y/y', label: 'Management Revenue' },
  { icon: { cat: 'business', idx: 19 }, unit: 'USD', value: '166mn', growth: '6.8% of GMV', label: 'Adjusted EBITDA' },
  { icon: { cat: 'interface', idx: 41 }, unit: 'USD', value: '116mn', growth: '4.8% of GMV', label: 'Adjusted Net Income' },
];

const cardW = 2.05;
const cardH = 3.0;
const gap = 0.2;
const startX = (10 - (kpis.length * cardW + (kpis.length - 1) * gap)) / 2;

for (let i = 0; i < kpis.length; i++) {
  const kpi = kpis[i];
  const cx = startX + i * (cardW + gap);
  const cy = 1.2;

  // Dark card background
  slide.addShape(pres.shapes.RECTANGLE, {
    x: cx, y: cy, w: cardW, h: cardH,
    fill: { color: BG_DARK },
    rectRadius: 0.1,
  });

  // Icon (white variant on dark card)
  await addIconInCircle(slide, pres, kpi.icon.cat, kpi.icon.idx, cx + (cardW - 0.4) / 2, cy + 0.25, 0.35, ACCENT_COLOR);

  // Unit label
  slide.addText(kpi.unit, {
    x: cx, y: cy + 0.85, w: cardW, h: 0.3,
    fontSize: 10, color: TEXT_MUTED, align: 'center', fontFace: BODY_FONT,
  });

  // Large stat number
  slide.addText(kpi.value, {
    x: cx, y: cy + 1.1, w: cardW, h: 0.7,
    fontSize: 36, bold: true, color: 'FFFFFF', align: 'center', fontFace: HEADER_FONT,
  });

  // Growth indicator
  slide.addText(kpi.growth, {
    x: cx, y: cy + 1.75, w: cardW, h: 0.3,
    fontSize: 12, color: ACCENT_COLOR, align: 'center', fontFace: BODY_FONT,
  });

  // Metric label
  slide.addText(kpi.label, {
    x: cx + 0.1, y: cy + 2.2, w: cardW - 0.2, h: 0.5,
    fontSize: 13, bold: true, color: 'FFFFFF', align: 'center', fontFace: BODY_FONT,
  });
}

// Optional footer bar
slide.addShape(pres.shapes.RECTANGLE, {
  x: 0.5, y: 4.5, w: 9.0, h: 0.65,
  fill: { color: SECONDARY },
  rectRadius: 0.06,
});
slide.addText(footerText, {
  x: 0.7, y: 4.55, w: 8.6, h: 0.55,
  fontSize: 11, color: TEXT_DARK, fontFace: BODY_FONT,
});
```

**When to use**: Any slide that presents 3-4 headline KPIs — earnings highlights, quarterly reviews, executive dashboards, investment summaries.

---

## 17. Strategy Dashboard

**Purpose**: Present a hero chart alongside a grid of supporting mini-metrics, organized by strategic pillars. Inspired by growth strategy slides showing MAU trends alongside selection, experience, value, and ecosystem metrics.

**Structure**:
- Title at top (spanning full width)
- Two sub-headers: left panel label + right panel label (dark background bars)
- Left panel (45%): hero bar/line chart with trend callout
- Right panel (55%): 2×3 or 2×2 grid of mini-metric cards, each with a mini bar chart or stat + growth badge

**Layout**:
```
┌──────────────────────────────────────────────┐
│  [Title - full width]                        │
│                                              │
│  ┌─[Sub-header A]───┐  ┌─[Sub-header B]────┐│
│  │                   │  │ ┌─────┐ ┌─────┐   ││
│  │                   │  │ │Mini │ │Mini │   ││
│  │   HERO CHART      │  │ │Stat1│ │Stat2│   ││
│  │   (bar/line)      │  │ └─────┘ └─────┘   ││
│  │                   │  │ ┌─────┐ ┌─────┐   ││
│  │   [+25% y/y       │  │ │Mini │ │Mini │   ││
│  │    callout]       │  │ │Stat3│ │Stat4│   ││
│  │                   │  │ └─────┘ └─────┘   ││
│  └───────────────────┘  └───────────────────┘│
│  [Source / footnotes]                        │
└──────────────────────────────────────────────┘
```

**Code pattern for mini-metric card**:
```javascript
function addMiniMetricCard(slide, pres, { x, y, w, h, title, subtitle, values, growth, theme }) {
  // Card background
  slide.addShape(pres.shapes.RECTANGLE, {
    x, y, w, h,
    fill: { color: theme.BG_LIGHT || 'FFF8F0' },
    rectRadius: 0.06,
  });

  // Title + subtitle
  slide.addText(title, {
    x: x + 0.1, y: y + 0.08, w: w - 0.2, h: 0.25,
    fontSize: 10, bold: true, color: theme.TEXT_DARK, fontFace: theme.BODY_FONT,
  });
  if (subtitle) {
    slide.addText(subtitle, {
      x: x + 0.1, y: y + 0.28, w: w - 0.2, h: 0.2,
      fontSize: 8, color: theme.TEXT_MUTED, fontFace: theme.BODY_FONT,
    });
  }

  // Growth badge (circle with percentage)
  const badgeSize = 0.35;
  slide.addShape(pres.shapes.OVAL, {
    x: x + 0.15, y: y + h - 0.85, w: badgeSize, h: badgeSize,
    fill: { color: theme.BG_DARK || '3D1C00' },
  });
  slide.addText(growth, {
    x: x + 0.15, y: y + h - 0.85, w: badgeSize, h: badgeSize,
    fontSize: 8, bold: true, color: 'FFFFFF', align: 'center', valign: 'middle',
  });

  // Mini bar pair (before/after)
  if (values && values.length === 2) {
    const barW = 0.55;
    const maxH = 0.6;
    const max = Math.max(...values.map(v => v.num));
    const barX = x + w - barW * 2 - 0.35;
    values.forEach((v, i) => {
      const bh = (v.num / max) * maxH;
      slide.addShape(pres.shapes.RECTANGLE, {
        x: barX + i * (barW + 0.1), y: y + h - 0.15 - bh, w: barW, h: bh,
        fill: { color: i === 0 ? theme.ACCENT : theme.PRIMARY },
        rectRadius: 0.03,
      });
      slide.addText(v.label, {
        x: barX + i * (barW + 0.1), y: y + h - 0.15, w: barW, h: 0.18,
        fontSize: 7, color: theme.TEXT_MUTED, align: 'center',
      });
    });
  }
}

// Usage: 2×3 grid of mini-metrics
const metrics = [
  { title: 'Selection', subtitle: 'Active Partners', values: [{num: 63, label: "Jun '24"}, {num: 77, label: "Jun '25"}], growth: '21%' },
  { title: 'Multi-verticality', subtitle: '% GMV share', values: [{num: 30, label: "Jun '24"}, {num: 34, label: "Jun '25"}], growth: '4pp' },
  // ... more metrics
];
const cols = 2, miniW = 2.1, miniH = 1.35, miniGap = 0.15;
const gridX = 4.8;
metrics.forEach((m, i) => {
  const col = i % cols, row = Math.floor(i / cols);
  addMiniMetricCard(slide, pres, {
    x: gridX + col * (miniW + miniGap),
    y: 1.6 + row * (miniH + miniGap),
    w: miniW, h: miniH, ...m, theme,
  });
});
```

**When to use**: Growth strategy overviews, multi-pillar strategic updates, any slide that needs to show one primary trend alongside 4-6 supporting metrics.

---

## 18. Product Feature Showcase

**Purpose**: Present a product or app feature with a device mockup, feature descriptions, and key stats. Inspired by product launch and app feature update slides.

**Structure**:
- Title at top
- Left (35%): phone/device mockup placeholder (rounded rectangle frame with content area)
- Center (35%): feature descriptions with icon bullets
- Right (30%): stat callouts or key updates panel

**Layout**:
```
┌──────────────────────────────────────────────┐
│  [Title - full width]                        │
│                                              │
│  ┌─────────┐  ┌──────────────┐  ┌──────────┐│
│  │ ┌─────┐ │  │ Feature desc │  │  ~60%    ││
│  │ │     │ │  │ with icons:  │  │  Key stat││
│  │ │PHONE│ │  │              │  │          ││
│  │ │MOCK │ │  │ ● Feature A  │  │  >40%    ││
│  │ │ UP  │ │  │ ● Feature B  │  │  Key stat││
│  │ │     │ │  │ ● Feature C  │  │          ││
│  │ └─────┘ │  │              │  │  [badge] ││
│  └─────────┘  └──────────────┘  └──────────┘│
└──────────────────────────────────────────────┘
```

**Code pattern — device mockup**:
```javascript
/**
 * Draw a phone mockup frame with placeholder content area.
 * Creates a rounded rectangle "phone" with a lighter inner screen.
 */
function addPhoneMockup(slide, x, y, w, h, theme) {
  const bezelTop = 0.25;
  const bezelSide = 0.08;
  const bezelBottom = 0.15;
  const screenW = w - bezelSide * 2;
  const screenH = h - bezelTop - bezelBottom;

  // Phone body (dark rounded rect)
  slide.addShape(pres.shapes.RECTANGLE, {
    x, y, w, h,
    fill: { color: '1A1A1A' },
    rectRadius: 0.2,
  });

  // Screen area (white/light)
  slide.addShape(pres.shapes.RECTANGLE, {
    x: x + bezelSide, y: y + bezelTop, w: screenW, h: screenH,
    fill: { color: 'FFFFFF' },
    rectRadius: 0.08,
  });

  // Notch indicator (small pill at top center)
  const notchW = 0.5;
  slide.addShape(pres.shapes.RECTANGLE, {
    x: x + (w - notchW) / 2, y: y + 0.08, w: notchW, h: 0.06,
    fill: { color: '333333' },
    rectRadius: 0.03,
  });

  // Placeholder text inside screen
  slide.addText('[App Screenshot]', {
    x: x + bezelSide + 0.1, y: y + bezelTop + screenH / 2 - 0.15,
    w: screenW - 0.2, h: 0.3,
    fontSize: 10, color: 'BBBBBB', align: 'center',
  });
}

// Usage:
addPhoneMockup(slide, 0.5, 0.9, 2.2, 4.0, theme);

// Feature list (center column)
const features = [
  { icon: { cat: 'user', idx: 57 }, title: '4 Family Members', desc: 'Share benefits with up to 3 additional members' },
  { icon: { cat: 'business', idx: 69 }, title: 'Family Allowances', desc: 'Set spend limits and manage budgets' },
];
for (let i = 0; i < features.length; i++) {
  const fy = 1.4 + i * 1.4;
  await addIconInCircle(slide, pres, features[i].icon.cat, features[i].icon.idx, 3.2, fy, 0.35, ACCENT_COLOR);
  slide.addText(features[i].title, {
    x: 3.8, y: fy - 0.05, w: 2.8, h: 0.3,
    fontSize: 14, bold: true, color: TEXT_DARK, fontFace: HEADER_FONT,
  });
  slide.addText(features[i].desc, {
    x: 3.8, y: fy + 0.25, w: 2.8, h: 0.4,
    fontSize: 11, color: TEXT_MUTED, fontFace: BODY_FONT,
  });
}

// Stat callouts (right column)
const stats = [
  { value: '~60%', label: 'Cost saving per person' },
  { value: '>40%', label: 'Higher retention with family plan' },
];
for (let i = 0; i < stats.length; i++) {
  const sy = 1.4 + i * 1.5;
  // Accent circle with stat
  slide.addShape(pres.shapes.OVAL, {
    x: 7.3, y: sy, w: 0.7, h: 0.7,
    fill: { color: ACCENT_COLOR },
  });
  slide.addText(stats[i].value, {
    x: 7.3, y: sy, w: 0.7, h: 0.7,
    fontSize: 14, bold: true, color: 'FFFFFF', align: 'center', valign: 'middle',
  });
  slide.addText(stats[i].label, {
    x: 8.1, y: sy + 0.1, w: 1.6, h: 0.5,
    fontSize: 12, bold: true, color: TEXT_DARK, fontFace: BODY_FONT,
  });
}
```

**When to use**: Product launches, app feature updates, SaaS feature showcases, mobile app reviews, any slide where a device screenshot or UI preview is central.

---

## 19. Bold Section Divider (Full-Bleed)

**Purpose**: Maximum-impact section break using full-bleed brand color with a single massive word. Creates dramatic rhythm in the deck. Variant of Layout 2 but with extreme visual weight.

**Structure**:
- Full slide covered in primary/accent color
- Single word or short phrase in ultra-bold (80-120pt)
- Centered vertically, left-aligned or centered
- Optional: subtle brand logo watermark in top-right corner

**Layout**:
```
┌──────────────────────────────────┐
│█████████████████████████████████ │
│█                               █│
│█                               █│
│█   FINANCIALS                  █│
│█                               █│
│█                               █│
│█████████████████████████████████ │
└──────────────────────────────────┘
```

**Code pattern**:
```javascript
slide.background = { color: ACCENT_COLOR }; // or PRIMARY

slide.addText(sectionTitle, {
  x: 0.8, y: 1.5, w: 8.4, h: 2.5,
  fontSize: 96, fontFace: HEADER_FONT, color: 'FFFFFF',
  bold: true, italic: true, align: 'left', valign: 'middle',
});

// Optional: subtle brand watermark (top-right, rotated)
// Use a semi-transparent white text or small logo image
```

**Difference from Layout 2**: Layout 2 includes section numbers, descriptions, and moderate font sizes. Layout 19 is pure impact — one word, massive type, full-bleed color. Use 19 for dramatic rhythm (common in investor decks and conference talks), use 2 for informational section breaks.

**When to use**: Earnings presentations, keynotes, conference talks — anywhere you want a dramatic visual pause between sections.

---

## 20. Period-over-Period Bar Comparison

**Purpose**: Present 2-3 side-by-side bar chart comparisons, each showing two periods with a growth callout bubble. The most common pattern in earnings and quarterly review presentations.

**Structure**:
- Title at top
- 2-3 chart panels in a horizontal row
- Each panel: colored header bar → two bars (period A vs period B) → dashed growth callout bubble between bars → insight text below
- Source line at bottom

**Layout** (3-panel version):
```
┌──────────────────────────────────────────────┐
│  [Title]                                     │
│                                              │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐   │
│  │[Header A]│  │[Header B]│  │[Header C]│   │
│  │% margin  │  │% margin  │  │% margin  │   │
│  │          │  │   ╭+25%╮ │  │   ╭+47%╮ │   │
│  │  ╭+31%╮  │  │   ╰────╯ │  │   ╰────╯ │   │
│  │  ╰────╯  │  │  ▐█  ▐█  │  │  ▐█  ▐█  │   │
│  │  ▐█  ▐█  │  │  93  116 │  │  129 190  │   │
│  │ 126  166 │  │          │  │           │   │
│  │Q2'24 Q25 │  │Q2'24 Q25 │  │Q2'24 Q25 │   │
│  │[Insight] │  │[Insight] │  │[Insight]  │   │
│  └──────────┘  └──────────┘  └──────────┘   │
│  Source: ...                                 │
└──────────────────────────────────────────────┘
```

**Code pattern**:
```javascript
/**
 * Draw a period-over-period bar comparison panel with growth callout.
 */
function addPoPPanel(slide, pres, {
  x, y, w, h, headerText, headerColor, marginLabel,
  bars, // [{value: 126, label: "Q2'24"}, {value: 166, label: "Q2'25"}]
  growth, // "+31% y/y"
  insight, // "EBITDA grew 31% y/y..."
  theme,
}) {
  const panelPad = 0.12;
  const headerH = 0.35;
  const marginH = 0.25;
  const barAreaTop = y + headerH + marginH + 0.15;
  const barAreaH = h - headerH - marginH - 1.2; // leave room for labels + insight
  const insightY = y + h - 0.85;

  // Panel background
  slide.addShape(pres.shapes.RECTANGLE, {
    x, y: y + headerH + marginH, w, h: h - headerH - marginH,
    fill: { color: 'FFFFFF' },
    line: { color: 'E8E8E8', width: 0.5 },
    rectRadius: 0.06,
  });

  // Header bar
  slide.addShape(pres.shapes.RECTANGLE, {
    x, y, w, h: headerH,
    fill: { color: headerColor || theme.PRIMARY },
    rectRadius: 0.06,
  });
  slide.addText(headerText, {
    x: x + 0.1, y, w: w - 0.2, h: headerH,
    fontSize: 11, bold: true, color: 'FFFFFF', align: 'center', valign: 'middle',
    fontFace: theme.HEADER_FONT,
  });

  // Margin label (e.g., "% of GMV  6.8%  6.8%")
  if (marginLabel) {
    slide.addText(marginLabel, {
      x: x + 0.1, y: y + headerH + 0.05, w: w - 0.2, h: marginH,
      fontSize: 8, color: theme.TEXT_MUTED, align: 'center', fontFace: theme.BODY_FONT,
    });
  }

  // Bars
  const maxVal = Math.max(...bars.map(b => b.value));
  const barW = w * 0.25;
  const barGap = w * 0.15;
  const barsStartX = x + (w - (bars.length * barW + (bars.length - 1) * barGap)) / 2;

  bars.forEach((bar, i) => {
    const barH = (bar.value / maxVal) * barAreaH;
    const bx = barsStartX + i * (barW + barGap);
    const by = barAreaTop + barAreaH - barH;

    // Bar
    slide.addShape(pres.shapes.RECTANGLE, {
      x: bx, y: by, w: barW, h: barH,
      fill: { color: i === 0 ? theme.ACCENT : theme.PRIMARY },
      rectRadius: 0.04,
    });

    // Value inside/above bar
    slide.addText(String(bar.value), {
      x: bx - 0.1, y: by + barH * 0.3, w: barW + 0.2, h: 0.3,
      fontSize: 14, bold: true, color: 'FFFFFF', align: 'center',
      fontFace: theme.HEADER_FONT,
    });

    // Period label below bar
    slide.addText(bar.label, {
      x: bx - 0.15, y: barAreaTop + barAreaH + 0.05, w: barW + 0.3, h: 0.2,
      fontSize: 8, color: theme.TEXT_MUTED, align: 'center', fontFace: theme.BODY_FONT,
    });
  });

  // Growth callout bubble (dashed oval between bars)
  const bubbleW = 0.7;
  const bubbleH = 0.35;
  const bubbleX = x + w / 2 - bubbleW / 2;
  const bubbleY = barAreaTop - 0.1;

  slide.addShape(pres.shapes.OVAL, {
    x: bubbleX, y: bubbleY, w: bubbleW, h: bubbleH,
    fill: { color: theme.BG_DARK || '3D1C00' },
  });
  slide.addText(growth, {
    x: bubbleX, y: bubbleY, w: bubbleW, h: bubbleH,
    fontSize: 10, bold: true, color: 'FFFFFF', align: 'center', valign: 'middle',
    fontFace: theme.HEADER_FONT,
  });

  // Insight text below bars
  slide.addShape(pres.shapes.RECTANGLE, {
    x: x + 0.05, y: insightY, w: w - 0.1, h: 0.7,
    fill: { color: theme.BG_LIGHT || 'FFF8F0' },
    rectRadius: 0.05,
  });
  slide.addText(insight, {
    x: x + 0.15, y: insightY + 0.05, w: w - 0.3, h: 0.6,
    fontSize: 9, color: theme.TEXT_DARK, fontFace: theme.BODY_FONT,
    valign: 'top',
  });
}

// Usage: 3 panels side by side
const panels = [
  { headerText: 'Adj. EBITDA (USD mn)', bars: [{value: 126, label: "Q2'24"}, {value: 166, label: "Q2'25"}], growth: '+31%\ny/y', marginLabel: '% of GMV    6.8%    6.8%', insight: 'EBITDA grew 31% y/y, maintaining stable 6.8% margin', headerColor: theme.ACCENT },
  // ... more panels
];

const panelW = 2.9, panelGap = 0.15;
const panelStartX = (10 - (panels.length * panelW + (panels.length - 1) * panelGap)) / 2;
panels.forEach((p, i) => {
  addPoPPanel(slide, pres, {
    x: panelStartX + i * (panelW + panelGap),
    y: 1.0, w: panelW, h: 4.2,
    ...p, theme,
  });
});
```

**When to use**: Quarterly earnings, any period-over-period comparison, financial results overview, performance benchmarking across time periods.

---

## Layout Sequencing Rules

1. **Never repeat** the same layout on consecutive slides
2. **Start with** Title Hero (layout 1)
3. **End with** Closing/CTA (layout 12)
4. **Use Section Dividers** (layout 2 or 19) to break the deck into 2-3 sections. Use 19 (Bold) for dramatic impact in investor/keynote decks, 2 for informational section breaks.
5. **Alternate visual weight**: heavy → medium → light → medium → heavy
6. **Data-heavy slides** (charts, tables) should be followed by lighter slides (quotes, icon rows)
7. A **10-slide deck** should use minimum 5 different layouts
8. **Financial decks** should use layouts 15, 16, 20 heavily — these are designed for earnings/quarterly review patterns
9. **Product decks** should include layout 18 for feature showcases
10. **Strategy decks** should use layout 17 for multi-metric dashboards
