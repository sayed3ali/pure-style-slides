# Data Visualization Reference

A complete guide to creating professional, theme-matched data visualizations in PptxGenJS. Every chart should look like a designer built it — not a default Excel export.

---

## Design Philosophy

1. **Charts tell stories, not just show data** — every chart needs a headline insight, not just a title
2. **Less is more** — strip gridlines, borders, and axis clutter; let the data breathe
3. **Theme-matched colors** — never use PptxGenJS defaults; always pass `chartColors` from your theme
4. **Consistent styling** — every chart in a deck should share the same axis style, font, and grid treatment
5. **Pair with context** — a chart alone is forgettable; pair it with a stat callout, insight box, or annotation
6. **Every analysis slide MUST include a written insight** — non-negotiable. Charts show evidence; insights tell the audience what to think. Add an insight card, annotation box, or callout text on every slide that contains a chart. The insight should state the "so what?" — not just describe the chart but explain what it means for the business. Use `makeCard()` with accent-left bar or a muted-fill panel.

### Mandatory Insight Annotation Pattern

Every slide with charts or data visualizations MUST include at least one visible insight annotation. Use one of these patterns:

```javascript
// Pattern A: Insight card with accent-left bar (preferred for Pure Minimal)
makeCard(slide, pres, x, y, w, h, { fill: 'F5F5F5', accentSide: 'left', accentColor: ACCENT });
slide.addText('KEY INSIGHT', {
  x: x + 0.2, y: y + 0.08, w: w - 0.4, h: 0.25,
  fontSize: 10, fontFace: HEADER_FONT, color: '888888',
  bold: true, charSpacing: 3,
});
slide.addText('Your insight text here — the "so what?" of the chart.', {
  x: x + 0.2, y: y + 0.35, w: w - 0.4, h: h - 0.5,
  fontSize: 11, fontFace: BODY_FONT, color: '333333',
  align: 'left', valign: 'top', lineSpacingMultiple: 1.3,
});

// Pattern B: Inline insight below chart title (compact)
slide.addText('→  Beverages drive 77% of revenue but only 71% of profit — Food margin needs attention.', {
  x: chartX, y: chartY + chartH + 0.1, w: chartW, h: 0.35,
  fontSize: 11, fontFace: BODY_FONT, color: ACCENT,
  italic: true, align: 'left',
});

// Pattern C: Stat callout cards alongside chart (for multi-metric slides)
// See KPI Card Row and Strategy Dashboard layouts for patterns
```

**Rules for insight text:**
- State what the data MEANS, not what it SHOWS ("Coffee margins outpace Hot Chocolate 3:1" not "Coffee margin is 80%")
- Maximum 2 sentences per insight card
- Use accent color or italic to visually distinguish insights from data labels
- Position insights adjacent to the chart they annotate — not at the bottom of the slide as an afterthought

---

## Chart Types Available

PptxGenJS supports these chart types via `pres.charts.*`:

| Type | Constant | Best For |
|------|----------|----------|
| Bar / Column | `pres.charts.BAR` | Comparing categories, rankings, period comparisons |
| Line | `pres.charts.LINE` | Trends over time, growth trajectories |
| Pie | `pres.charts.PIE` | Part-of-whole (≤6 segments) |
| Doughnut | `pres.charts.DOUGHNUT` | Part-of-whole with center callout |
| Scatter | `pres.charts.SCATTER` | Correlation, distribution |
| Bubble | `pres.charts.BUBBLE` | Three-variable comparison |
| Radar | `pres.charts.RADAR` | Multi-axis comparison (skills, ratings) |
| Area | `pres.charts.AREA` | Volume over time, stacked composition |

---

## Chart Styling System

### The `makeChartStyle()` Factory

Create a reusable factory function at the top of your generation script. This ensures every chart in the deck shares consistent styling matched to the selected theme:

```javascript
/**
 * Generate theme-matched chart options.
 * Call this for every chart — never share objects between addChart() calls.
 * 
 * @param {object} theme - The active theme object with color constants
 * @param {object} overrides - Per-chart overrides (chartColors, showLegend, etc.)
 * @returns {object} Fresh chart options object
 */
function makeChartStyle(theme, overrides = {}) {
  return {
    // Position & size (override per chart)
    x: 0.5, y: 1.3, w: 9, h: 3.8,
    
    // Background
    plotArea: { fill: { color: theme.BG_LIGHT || 'FFFFFF' } },
    
    // Axis labels
    catAxisLabelColor: theme.TEXT_MUTED || '6B7280',
    catAxisLabelFontSize: 10,
    catAxisLabelFontFace: theme.BODY_FONT || 'Calibri',
    valAxisLabelColor: theme.TEXT_MUTED || '6B7280',
    valAxisLabelFontSize: 10,
    valAxisLabelFontFace: theme.BODY_FONT || 'Calibri',
    
    // Grid lines — subtle value grid only, no category grid
    valGridLine: { color: theme.SECONDARY ? theme.SECONDARY + '80' : 'E5E7EB', size: 0.5 },
    catGridLine: { style: 'none' },
    
    // Axis lines — hide for clean look
    catAxisLineShow: false,
    valAxisLineShow: false,
    
    // Colors — use theme palette
    chartColors: overrides.chartColors || [
      theme.ACCENT || '4A90D9',
      theme.PRIMARY || '1E2761',
      theme.SECONDARY || 'CADCFC',
      lightenHex(theme.ACCENT || '4A90D9', 40),
      lightenHex(theme.PRIMARY || '1E2761', 30),
    ],
    
    // Legend
    showLegend: overrides.showLegend ?? false,
    legendPos: overrides.legendPos || 'b',
    legendColor: theme.TEXT_MUTED || '6B7280',
    legendFontSize: 9,
    legendFontFace: theme.BODY_FONT || 'Calibri',
    
    // Data labels
    showValue: overrides.showValue ?? false,
    dataLabelColor: theme.TEXT_DARK || '1F2937',
    dataLabelFontSize: 9,
    dataLabelFontFace: theme.BODY_FONT || 'Calibri',
    
    // Apply overrides last
    ...overrides,
  };
}

/**
 * Lighten a hex color by a percentage.
 * Useful for generating chart color ramps from theme colors.
 */
function lightenHex(hex, percent) {
  hex = hex.replace('#', '');
  const r = Math.min(255, Math.round(parseInt(hex.slice(0, 2), 16) + (255 - parseInt(hex.slice(0, 2), 16)) * percent / 100));
  const g = Math.min(255, Math.round(parseInt(hex.slice(2, 4), 16) + (255 - parseInt(hex.slice(2, 4), 16)) * percent / 100));
  const b = Math.min(255, Math.round(parseInt(hex.slice(4, 6), 16) + (255 - parseInt(hex.slice(4, 6), 16)) * percent / 100));
  return [r, g, b].map(c => c.toString(16).padStart(2, '0')).join('');
}

/**
 * Generate a color ramp of N shades from a base color.
 * Use for multi-series charts, pie segments, or heatmap gradients.
 */
function makeColorRamp(baseHex, count) {
  const ramp = [];
  for (let i = 0; i < count; i++) {
    const pct = (i / (count - 1)) * 60; // 0% to 60% lightened
    ramp.push(lightenHex(baseHex, pct));
  }
  return ramp;
}
```

### CRITICAL: Object Mutation Safety

PptxGenJS **mutates option objects in-place** (converting values to EMU internally). **Never share a single style object across multiple `addChart()` calls.** Always call `makeChartStyle()` fresh for each chart:

```javascript
// ❌ WRONG — second chart gets corrupted values
const style = makeChartStyle(theme);
slide1.addChart(pres.charts.BAR, data1, style);
slide2.addChart(pres.charts.BAR, data2, style);

// ✅ CORRECT — fresh object each time
slide1.addChart(pres.charts.BAR, data1, makeChartStyle(theme));
slide2.addChart(pres.charts.BAR, data2, makeChartStyle(theme));
```

---

## Chart Code Recipes

### 1. Column Chart (Vertical Bars)

The workhorse. Use for category comparisons, period-over-period, rankings.

```javascript
slide.addChart(pres.charts.BAR, [
  {
    name: 'Revenue',
    labels: ['Q1', 'Q2', 'Q3', 'Q4'],
    values: [4500, 5500, 6200, 7100],
  }
], makeChartStyle(theme, {
  x: 0.5, y: 1.4, w: 5.5, h: 3.6,
  barDir: 'col',
  barGapWidthPct: 100,       // Gap between bars (100 = bar width)
  showValue: true,
  dataLabelPosition: 'outEnd',
  chartColors: [theme.ACCENT],
}));
```

**Multi-series column chart** (grouped bars):

```javascript
slide.addChart(pres.charts.BAR, [
  { name: '2024', labels: ['Q1','Q2','Q3','Q4'], values: [3200, 3800, 4100, 4500] },
  { name: '2025', labels: ['Q1','Q2','Q3','Q4'], values: [4500, 5500, 6200, 7100] },
], makeChartStyle(theme, {
  barDir: 'col',
  barGapWidthPct: 80,
  showLegend: true,
  legendPos: 'b',
  chartColors: [theme.SECONDARY, theme.ACCENT],
}));
```

**Stacked column chart**:

```javascript
slide.addChart(pres.charts.BAR, multiSeriesData, makeChartStyle(theme, {
  barDir: 'col',
  barGrouping: 'stacked',
  showValue: true,
  dataLabelPosition: 'ctr',
  chartColors: makeColorRamp(theme.PRIMARY, 4),
}));
```

### 2. Horizontal Bar Chart

Use for rankings, league tables, or when category labels are long.

```javascript
slide.addChart(pres.charts.BAR, [
  {
    name: 'Market Share',
    labels: ['Company A', 'Company B', 'Company C', 'Company D', 'Others'],
    values: [35, 25, 18, 12, 10],
  }
], makeChartStyle(theme, {
  barDir: 'bar',            // Horizontal bars
  barGapWidthPct: 60,
  showValue: true,
  dataLabelPosition: 'outEnd',
  catAxisOrientation: 'maxMin',  // Top to bottom (largest first)
  chartColors: makeColorRamp(theme.ACCENT, 5),
}));
```

### 3. Line Chart

Use for trends, time series, growth trajectories.

```javascript
slide.addChart(pres.charts.LINE, [
  {
    name: 'Monthly Revenue',
    labels: ['Jan','Feb','Mar','Apr','May','Jun','Jul','Aug','Sep','Oct','Nov','Dec'],
    values: [12, 14, 15, 18, 22, 25, 28, 31, 29, 33, 36, 42],
  }
], makeChartStyle(theme, {
  lineSize: 2.5,
  lineSmooth: true,          // Curved line
  lineDataSymbol: 'circle',
  lineDataSymbolSize: 6,
  showValue: false,           // Too cluttered for 12 points
  chartColors: [theme.ACCENT],
}));
```

**Multi-line comparison**:

```javascript
slide.addChart(pres.charts.LINE, [
  { name: 'Product A', labels: months, values: productA },
  { name: 'Product B', labels: months, values: productB },
  { name: 'Product C', labels: months, values: productC },
], makeChartStyle(theme, {
  lineSize: 2,
  lineSmooth: true,
  showLegend: true,
  legendPos: 'b',
  chartColors: [theme.ACCENT, theme.PRIMARY, theme.SECONDARY],
}));
```

### 4. Pie Chart

Use for part-of-whole with **6 or fewer segments**. More than 6 → use horizontal bar instead.

```javascript
slide.addChart(pres.charts.PIE, [
  {
    name: 'Revenue by Region',
    labels: ['North America', 'Europe', 'Asia Pacific', 'Other'],
    values: [42, 28, 22, 8],
  }
], makeChartStyle(theme, {
  x: 0.5, y: 1.2, w: 4.5, h: 4.0,
  showPercent: true,
  showTitle: false,
  showLegend: true,
  legendPos: 'b',
  dataLabelColor: 'FFFFFF',
  dataLabelFontSize: 11,
  chartColors: [theme.ACCENT, theme.PRIMARY, theme.SECONDARY, lightenHex(theme.ACCENT, 40)],
}));
```

### 5. Doughnut Chart

Like pie, but with a hollow center — perfect for putting a key number in the middle.

```javascript
// Doughnut chart
slide.addChart(pres.charts.DOUGHNUT, [
  {
    name: 'Budget Allocation',
    labels: ['Engineering', 'Marketing', 'Sales', 'Operations'],
    values: [40, 25, 20, 15],
  }
], makeChartStyle(theme, {
  x: 1.0, y: 1.2, w: 4.0, h: 3.8,
  showPercent: true,
  holeSize: 55,              // Inner hole percentage
  showLegend: true,
  legendPos: 'r',
  chartColors: makeColorRamp(theme.PRIMARY, 4),
}));

// Center label inside the doughnut hole
slide.addText([
  { text: '40%', options: { fontSize: 36, bold: true, color: theme.ACCENT, breakLine: true } },
  { text: 'Engineering', options: { fontSize: 12, color: theme.TEXT_MUTED } },
], {
  x: 2.0, y: 2.5, w: 2.0, h: 1.2,
  align: 'center', valign: 'middle',
});
```

### 6. Area Chart

Use for volume over time or stacked composition trends.

```javascript
slide.addChart(pres.charts.AREA, [
  { name: 'Organic', labels: quarters, values: organic },
  { name: 'Paid', labels: quarters, values: paid },
  { name: 'Referral', labels: quarters, values: referral },
], makeChartStyle(theme, {
  barGrouping: 'stacked',
  lineSmooth: true,
  showLegend: true,
  legendPos: 'b',
  chartColors: [theme.ACCENT, theme.PRIMARY, lightenHex(theme.SECONDARY, 20)],
  opacity: 80,               // Slight transparency for stacked areas
}));
```

### 7. Scatter / Bubble Chart

Use for correlation analysis or multi-variable comparison.

```javascript
// Scatter plot
slide.addChart(pres.charts.SCATTER, [
  {
    name: 'Listings',
    values: [
      { x: 2.5, y: 85 },
      { x: 3.2, y: 72 },
      { x: 4.1, y: 91 },
      { x: 1.8, y: 65 },
      { x: 5.0, y: 95 },
    ],
  }
], makeChartStyle(theme, {
  showLegend: false,
  lineSize: 0,               // No connecting line
  lineDataSymbol: 'circle',
  lineDataSymbolSize: 8,
  chartColors: [theme.ACCENT],
  catAxisTitle: 'Rating',
  valAxisTitle: 'Occupancy %',
  showCatAxisTitle: true,
  showValAxisTitle: true,
}));
```

### 8. Radar Chart

Use for multi-axis comparison (skills, product features, competitive analysis).

```javascript
slide.addChart(pres.charts.RADAR, [
  { name: 'Our Product', labels: ['Speed','Reliability','Price','Support','Features'], values: [90, 85, 70, 95, 80] },
  { name: 'Competitor', labels: ['Speed','Reliability','Price','Support','Features'], values: [75, 80, 85, 60, 90] },
], makeChartStyle(theme, {
  x: 2.0, y: 1.0, w: 6.0, h: 4.2,
  radarStyle: 'filled',
  showLegend: true,
  legendPos: 'b',
  chartColors: [theme.ACCENT, theme.SECONDARY],
}));
```

---

## Slide Layout Patterns for Data

### Layout D1: Chart + Insight Panel

The primary data layout. Chart on the left, key takeaway panel on the right.

```
┌──────────────────────────────────────┐
│  [Headline Insight — not just title] │
│                                      │
│  ┌────────────────┐  ┌────────────┐  │
│  │                │  │  ┌──────┐  │  │
│  │   CHART        │  │  │ 42%  │  │  │
│  │                │  │  │ KPI  │  │  │
│  │                │  │  └──────┘  │  │
│  │                │  │  context   │  │
│  └────────────────┘  └────────────┘  │
│  Source: ...                         │
└──────────────────────────────────────┘
```

```javascript
function layoutChartInsight(slide, pres, theme, {
  title, chartType, chartData, chartOverrides,
  statValue, statLabel, insightText, source, speakerNotes
}) {
  // Title — phrased as insight, not label
  slide.addText(title, {
    x: 0.5, y: 0.4, w: 9.0, h: 0.6,
    fontSize: 22, fontFace: theme.HEADER_FONT, color: theme.TEXT_DARK,
    bold: true, margin: 0,
  });

  // Chart (left 60%)
  slide.addChart(chartType, chartData, makeChartStyle(theme, {
    x: 0.5, y: 1.3, w: 5.5, h: 3.6,
    ...chartOverrides,
  }));

  // Insight panel (right 35%)
  slide.addShape(pres.shapes.RECTANGLE, {
    x: 6.3, y: 1.3, w: 3.2, h: 3.6,
    fill: { color: theme.BG_LIGHT || 'F5F5F5' },
    rectRadius: 0.08,
  });

  // Big stat number
  slide.addText(statValue, {
    x: 6.5, y: 1.6, w: 2.8, h: 1.0,
    fontSize: 48, fontFace: theme.HEADER_FONT, color: theme.ACCENT,
    bold: true, align: 'center', valign: 'middle',
  });

  // Stat label
  slide.addText(statLabel, {
    x: 6.5, y: 2.6, w: 2.8, h: 0.4,
    fontSize: 13, fontFace: theme.BODY_FONT, color: theme.TEXT_MUTED,
    align: 'center',
  });

  // Insight text
  slide.addText(insightText, {
    x: 6.6, y: 3.2, w: 2.6, h: 1.4,
    fontSize: 11, fontFace: theme.BODY_FONT, color: theme.TEXT_DARK,
    align: 'left', valign: 'top',
  });

  // Source
  if (source) {
    slide.addText(`Source: ${source}`, {
      x: 0.5, y: 5.15, w: 9.0, h: 0.3,
      fontSize: 8, color: theme.TEXT_MUTED, fontFace: theme.BODY_FONT,
    });
  }

  // Speaker notes (mandatory on every data slide)
  if (speakerNotes) {
    slide.addNotes(speakerNotes);
  }
}
```

### Layout D2: Full-Width Chart

Use when the chart IS the story and needs maximum space.

```
┌──────────────────────────────────────┐
│  [Headline Insight]                  │
│                                      │
│  ┌──────────────────────────────┐    │
│  │                              │    │
│  │         FULL-WIDTH CHART     │    │
│  │                              │    │
│  │                              │    │
│  └──────────────────────────────┘    │
│  Source: ...                         │
└──────────────────────────────────────┘
```

```javascript
function layoutFullChart(slide, pres, theme, {
  title, chartType, chartData, chartOverrides, source, speakerNotes
}) {
  slide.addText(title, {
    x: 0.5, y: 0.4, w: 9.0, h: 0.6,
    fontSize: 22, fontFace: theme.HEADER_FONT, color: theme.TEXT_DARK,
    bold: true, margin: 0,
  });

  slide.addChart(chartType, chartData, makeChartStyle(theme, {
    x: 0.5, y: 1.2, w: 9.0, h: 3.8,
    ...chartOverrides,
  }));

  if (source) {
    slide.addText(`Source: ${source}`, {
      x: 0.5, y: 5.15, w: 9.0, h: 0.3,
      fontSize: 8, color: theme.TEXT_MUTED,
    });
  }

  if (speakerNotes) {
    slide.addNotes(speakerNotes);
  }
}
```

### Layout D3: Dual Chart Comparison

Two charts side by side for direct comparison.

```
┌──────────────────────────────────────┐
│  [Comparison Title]                  │
│                                      │
│  ┌───────────────┐ ┌──────────────┐  │
│  │  CHART A      │ │  CHART B     │  │
│  │  (before /    │ │  (after /    │  │
│  │   segment 1)  │ │   segment 2) │  │
│  └───────────────┘ └──────────────┘  │
│  [Label A]          [Label B]        │
└──────────────────────────────────────┘
```

```javascript
function layoutDualChart(slide, pres, theme, {
  title, leftChart, rightChart, leftLabel, rightLabel, speakerNotes
}) {
  slide.addText(title, {
    x: 0.5, y: 0.4, w: 9.0, h: 0.6,
    fontSize: 22, fontFace: theme.HEADER_FONT, color: theme.TEXT_DARK,
    bold: true, margin: 0,
  });

  // Left chart
  slide.addChart(leftChart.type, leftChart.data, makeChartStyle(theme, {
    x: 0.3, y: 1.3, w: 4.5, h: 3.2,
    ...leftChart.overrides,
  }));
  slide.addText(leftLabel, {
    x: 0.3, y: 4.6, w: 4.5, h: 0.4,
    fontSize: 14, fontFace: theme.HEADER_FONT, color: theme.TEXT_DARK,
    bold: true, align: 'center',
  });

  // Right chart
  slide.addChart(rightChart.type, rightChart.data, makeChartStyle(theme, {
    x: 5.2, y: 1.3, w: 4.5, h: 3.2,
    ...rightChart.overrides,
  }));
  slide.addText(rightLabel, {
    x: 5.2, y: 4.6, w: 4.5, h: 0.4,
    fontSize: 14, fontFace: theme.HEADER_FONT, color: theme.TEXT_DARK,
    bold: true, align: 'center',
  });

  if (speakerNotes) {
    slide.addNotes(speakerNotes);
  }
}
```

### Layout D4: KPI Dashboard

3-4 stat cards across the top + a supporting chart below. The "executive dashboard" slide.

```
┌──────────────────────────────────────┐
│  [Dashboard Title]                   │
│                                      │
│  ┌────────┐ ┌────────┐ ┌────────┐   │
│  │  $1.2M │ │  +23%  │ │  4.8★  │   │
│  │Revenue │ │Growth  │ │Rating  │   │
│  └────────┘ └────────┘ └────────┘   │
│                                      │
│  ┌──────────────────────────────┐    │
│  │       SUPPORTING CHART       │    │
│  └──────────────────────────────┘    │
└──────────────────────────────────────┘
```

```javascript
function layoutKPIDashboard(slide, pres, theme, {
  title, kpis, chartType, chartData, chartOverrides, speakerNotes
}) {
  const makeCardShadow = () => ({
    type: 'outer', blur: 4, offset: 1, angle: 135,
    color: '000000', opacity: 0.08,
  });

  slide.addText(title, {
    x: 0.5, y: 0.3, w: 9.0, h: 0.5,
    fontSize: 20, fontFace: theme.HEADER_FONT, color: theme.TEXT_DARK,
    bold: true, margin: 0,
  });

  // KPI cards
  const cardW = (9.0 - (kpis.length - 1) * 0.3) / kpis.length;
  kpis.forEach((kpi, i) => {
    const cx = 0.5 + i * (cardW + 0.3);

    // Card background
    slide.addShape(pres.shapes.RECTANGLE, {
      x: cx, y: 0.95, w: cardW, h: 1.15,
      fill: { color: 'FFFFFF' },
      shadow: makeCardShadow(),
    });

    // Accent top bar
    slide.addShape(pres.shapes.RECTANGLE, {
      x: cx, y: 0.95, w: cardW, h: 0.04,
      fill: { color: kpi.color || theme.ACCENT },
    });

    // Value
    slide.addText(kpi.value, {
      x: cx + 0.15, y: 1.05, w: cardW - 0.3, h: 0.6,
      fontSize: 28, fontFace: theme.HEADER_FONT,
      color: kpi.color || theme.ACCENT,
      bold: true, align: 'center', valign: 'middle',
    });

    // Label
    slide.addText(kpi.label, {
      x: cx + 0.15, y: 1.65, w: cardW - 0.3, h: 0.35,
      fontSize: 10, fontFace: theme.BODY_FONT,
      color: theme.TEXT_MUTED,
      align: 'center', valign: 'top',
    });
  });

  // Supporting chart below
  slide.addChart(chartType, chartData, makeChartStyle(theme, {
    x: 0.5, y: 2.35, w: 9.0, h: 2.8,
    ...chartOverrides,
  }));

  if (speakerNotes) {
    slide.addNotes(speakerNotes);
  }
}
```

### Layout D5: Doughnut + Breakdown

Doughnut chart with a center stat, paired with a vertical breakdown list.

```
┌──────────────────────────────────────┐
│  [Title]                             │
│                                      │
│      ┌──────────┐    Category A ██ 40%│
│      │          │    Category B ██ 25%│
│      │   42%    │    Category C ██ 20%│
│      │  Total   │    Category D ██ 15%│
│      │          │                    │
│      └──────────┘                    │
└──────────────────────────────────────┘
```

```javascript
function layoutDoughnutBreakdown(slide, pres, theme, {
  title, chartData, centerValue, centerLabel, colors, speakerNotes
}) {
  slide.addText(title, {
    x: 0.5, y: 0.4, w: 9.0, h: 0.6,
    fontSize: 22, fontFace: theme.HEADER_FONT, color: theme.TEXT_DARK,
    bold: true, margin: 0,
  });

  const chartColors = colors || makeColorRamp(theme.ACCENT, chartData[0].values.length);

  // Doughnut chart (left)
  slide.addChart(pres.charts.DOUGHNUT, chartData, makeChartStyle(theme, {
    x: 0.5, y: 1.2, w: 4.5, h: 3.8,
    holeSize: 55,
    showPercent: false,
    showLegend: false,
    chartColors: chartColors,
  }));

  // Center label
  slide.addText([
    { text: centerValue, options: { fontSize: 36, bold: true, color: theme.ACCENT, breakLine: true } },
    { text: centerLabel, options: { fontSize: 11, color: theme.TEXT_MUTED } },
  ], {
    x: 1.5, y: 2.5, w: 2.5, h: 1.2,
    align: 'center', valign: 'middle',
  });

  // Breakdown list (right)
  const labels = chartData[0].labels;
  const values = chartData[0].values;
  const total = values.reduce((a, b) => a + b, 0);

  labels.forEach((label, i) => {
    const ly = 1.5 + i * 0.7;
    const pct = Math.round(values[i] / total * 100);

    // Color swatch
    slide.addShape(pres.shapes.RECTANGLE, {
      x: 5.5, y: ly + 0.05, w: 0.25, h: 0.25,
      fill: { color: chartColors[i] },
    });

    // Label
    slide.addText(label, {
      x: 5.95, y: ly, w: 2.5, h: 0.35,
      fontSize: 13, fontFace: theme.BODY_FONT, color: theme.TEXT_DARK,
      bold: true,
    });

    // Percentage
    slide.addText(`${pct}%`, {
      x: 8.5, y: ly, w: 1.0, h: 0.35,
      fontSize: 13, fontFace: theme.BODY_FONT, color: theme.TEXT_MUTED,
      align: 'right',
    });
  });

  if (speakerNotes) {
    slide.addNotes(speakerNotes);
  }
}
```

### Layout D6: Data Table with Highlights

Styled table for structured data — use when exact numbers matter more than visual shape.

```
┌──────────────────────────────────────┐
│  [Title]                             │
│                                      │
│  ┌──────────────────────────────┐    │
│  │ Header │ Col A │ Col B │ Δ   │    │
│  │────────┼───────┼───────┼─────│    │
│  │ Row 1  │  123  │  145  │ +18%│    │
│  │ Row 2  │  456  │  501  │ +10%│    │
│  │ Row 3  │  789  │  720  │ -9% │    │
│  └──────────────────────────────┘    │
│  [Key insight or footnote]           │
└──────────────────────────────────────┘
```

```javascript
function makeStyledTable(slide, pres, theme, {
  title, headers, rows, highlightCol, footnote, speakerNotes
}) {
  slide.addText(title, {
    x: 0.5, y: 0.4, w: 9.0, h: 0.6,
    fontSize: 22, fontFace: theme.HEADER_FONT, color: theme.TEXT_DARK,
    bold: true, margin: 0,
  });

  // Build header row
  const headerRow = headers.map(h => ({
    text: h,
    options: {
      fill: { color: theme.PRIMARY },
      color: 'FFFFFF',
      bold: true,
      fontSize: 11,
      fontFace: theme.BODY_FONT,
      align: 'center',
      valign: 'middle',
    }
  }));

  // Build data rows with alternating colors
  const tableRows = rows.map((row, ri) => {
    return row.map((cell, ci) => ({
      text: String(cell),
      options: {
        fill: { color: ri % 2 === 0 ? 'FFFFFF' : (theme.BG_LIGHT || 'F9FAFB') },
        color: ci === highlightCol ? theme.ACCENT : theme.TEXT_DARK,
        bold: ci === highlightCol,
        fontSize: 11,
        fontFace: theme.BODY_FONT,
        align: ci === 0 ? 'left' : 'center',
        valign: 'middle',
      }
    }));
  });

  const allRows = [headerRow, ...tableRows];
  const colW = headers.map((_, i) => i === 0 ? 2.5 : (6.5 / (headers.length - 1)));

  slide.addTable(allRows, {
    x: 0.5, y: 1.2, w: 9.0,
    colW: colW,
    rowH: 0.45,
    border: { pt: 0.5, color: 'E5E7EB' },
  });

  if (footnote) {
    slide.addText(footnote, {
      x: 0.5, y: 5.15, w: 9.0, h: 0.3,
      fontSize: 9, color: theme.TEXT_MUTED, italic: true,
    });
  }

  if (speakerNotes) {
    slide.addNotes(speakerNotes);
  }
}
```

### Layout D7: Mini-Chart Grid

2×2 grid of small charts — great for quarterly review dashboards.

```
┌──────────────────────────────────────┐
│  [Dashboard Title]                   │
│  ┌──────────────┐ ┌──────────────┐   │
│  │  Mini Chart 1│ │  Mini Chart 2│   │
│  │  [label]     │ │  [label]     │   │
│  └──────────────┘ └──────────────┘   │
│  ┌──────────────┐ ┌──────────────┐   │
│  │  Mini Chart 3│ │  Mini Chart 4│   │
│  └──────────────┘ └──────────────┘   │
└──────────────────────────────────────┘
```

```javascript
function layoutMiniChartGrid(slide, pres, theme, { title, charts, speakerNotes }) {
  slide.addText(title, {
    x: 0.5, y: 0.3, w: 9.0, h: 0.5,
    fontSize: 20, fontFace: theme.HEADER_FONT, color: theme.TEXT_DARK,
    bold: true, margin: 0,
  });

  const positions = [
    { x: 0.3, y: 0.95, w: 4.6, h: 2.15 },
    { x: 5.1, y: 0.95, w: 4.6, h: 2.15 },
    { x: 0.3, y: 3.25, w: 4.6, h: 2.15 },
    { x: 5.1, y: 3.25, w: 4.6, h: 2.15 },
  ];

  charts.forEach((chart, i) => {
    if (i >= 4) return;
    const pos = positions[i];

    // Card background
    slide.addShape(pres.shapes.RECTANGLE, {
      x: pos.x, y: pos.y, w: pos.w, h: pos.h,
      fill: { color: 'FFFFFF' },
      shadow: { type: 'outer', blur: 3, offset: 1, angle: 135, color: '000000', opacity: 0.06 },
    });

    // Mini chart label
    slide.addText(chart.label, {
      x: pos.x + 0.15, y: pos.y + 0.08, w: pos.w - 0.3, h: 0.3,
      fontSize: 11, fontFace: theme.HEADER_FONT, color: theme.TEXT_DARK,
      bold: true,
    });

    // Chart
    slide.addChart(chart.type, chart.data, makeChartStyle(theme, {
      x: pos.x + 0.1, y: pos.y + 0.4, w: pos.w - 0.2, h: pos.h - 0.55,
      showLegend: false,
      catAxisLabelFontSize: 8,
      valAxisLabelFontSize: 8,
      ...chart.overrides,
    }));
  });

  if (speakerNotes) {
    slide.addNotes(speakerNotes);
  }
}
```

---

## Stat Callout Patterns

When you have a single key number, a large stat callout is often more impactful than a chart.

### Big Number + Context

```javascript
function addStatCallout(slide, theme, { value, label, context, x, y, w }) {
  // Value
  slide.addText(value, {
    x, y, w, h: 1.0,
    fontSize: 64, fontFace: theme.HEADER_FONT,
    color: theme.ACCENT, bold: true, align: 'center',
  });

  // Label
  slide.addText(label.toUpperCase(), {
    x, y: y + 1.0, w, h: 0.4,
    fontSize: 13, fontFace: theme.BODY_FONT,
    color: theme.TEXT_DARK, bold: true, align: 'center',
    charSpacing: 2,
  });

  // Context
  slide.addText(context, {
    x, y: y + 1.4, w, h: 0.4,
    fontSize: 11, fontFace: theme.BODY_FONT,
    color: theme.TEXT_MUTED, align: 'center',
  });
}
```

### Stat with Trend Indicator

Pair a stat with an up/down arrow icon from the built-in library:

```javascript
function addStatWithTrend(slide, pres, theme, { value, label, trend, trendPositive, x, y, w }) {
  const SKILL_DIR = '/mnt/skills/user/pure-style-slides';
  
  // Value
  slide.addText(value, {
    x, y, w: w - 0.6, h: 0.9,
    fontSize: 48, fontFace: theme.HEADER_FONT,
    color: theme.ACCENT, bold: true, align: 'center',
  });

  // Trend arrow (use built-in arrow icons)
  const arrowIcon = trendPositive
    ? `${SKILL_DIR}/icons/arrow/arrow_048.png`    // up arrow
    : `${SKILL_DIR}/icons/arrow/arrow_013.png`;   // down arrow

  const trendColor = trendPositive ? '16A34A' : 'DC2626';

  // Trend indicator circle + icon
  slide.addShape(pres.shapes.OVAL, {
    x: x + w - 0.55, y: y + 0.2, w: 0.45, h: 0.45,
    fill: { color: trendColor },
  });
  slide.addImage({
    path: `${SKILL_DIR}/icons/arrow_white/arrow_white_${trendPositive ? '048' : '013'}.png`,
    x: x + w - 0.48, y: y + 0.27, w: 0.3, h: 0.3,
  });

  // Trend text
  slide.addText(trend, {
    x: x + w - 0.8, y: y + 0.7, w: 1.0, h: 0.3,
    fontSize: 10, color: trendColor, align: 'center', bold: true,
  });

  // Label
  slide.addText(label, {
    x, y: y + 1.0, w, h: 0.35,
    fontSize: 12, fontFace: theme.BODY_FONT,
    color: theme.TEXT_MUTED, align: 'center',
  });
}
```

---

## Chart-Specific Best Practices

### When to Use Which Chart

| Data Story | Recommended Chart | Avoid |
|-----------|-------------------|-------|
| Compare categories | Column bar | Pie (hard to compare lengths vs. angles) |
| Show trend over time | Line | Bar (doesn't imply continuity) |
| Part of whole (≤6) | Pie or doughnut | Pie with 7+ slices |
| Part of whole (>6) | Horizontal bar (sorted) | Pie (too many slivers) |
| Correlation | Scatter | Line (implies sequence) |
| Ranking | Horizontal bar (sorted) | Vertical bar (label readability) |
| Before/After | Grouped column (2 series) | Pie |
| KPI spotlight | Stat callout (no chart) | Chart (overcomplicates one number) |
| Multi-metric dashboard | Mini-chart grid (D7) | One giant chart |
| Budget/allocation | Doughnut + breakdown (D5) | Table |

### Color Rules for Data

1. **Single series**: Use `theme.ACCENT` as the only bar/line color
2. **Two series (comparison)**: Use `theme.ACCENT` + `theme.SECONDARY`
3. **3-5 series**: Use `makeColorRamp(theme.ACCENT, n)` for harmonious gradient
4. **Positive/negative**: Green (`16A34A`) for positive, red (`DC2626`) for negative — don't rely on theme colors for semantic meaning
5. **Highlight one bar**: Color all bars `theme.SECONDARY`, then the highlighted bar `theme.ACCENT`

### Title Writing Rules for Charts

- ❌ **Bad**: "Quarterly Revenue" (just a label)
- ✅ **Good**: "Revenue Grew 58% in 2025" (the insight)
- ❌ **Bad**: "Market Share by Region"
- ✅ **Good**: "North America Drives 42% of Revenue"
- ❌ **Bad**: "Customer Satisfaction Scores"
- ✅ **Good**: "Satisfaction Hit an All-Time High at 4.8★"

The slide title should always answer "So what?" — the chart shows the evidence.

### Source Attribution

Always cite data sources on data slides. Place at the bottom left:

```javascript
slide.addText('Source: Company Q4 2025 earnings report', {
  x: 0.5, y: 5.2, w: 6.0, h: 0.25,
  fontSize: 8, color: theme.TEXT_MUTED, fontFace: theme.BODY_FONT,
});
```

### Number Formatting Tips

Format numbers for readability on slides:

```javascript
function formatStat(value) {
  if (value >= 1e9) return `$${(value / 1e9).toFixed(1)}B`;
  if (value >= 1e6) return `$${(value / 1e6).toFixed(1)}M`;
  if (value >= 1e3) return `$${(value / 1e3).toFixed(0)}K`;
  return `$${value}`;
}

function formatPct(value) {
  return `${value > 0 ? '+' : ''}${value.toFixed(1)}%`;
}
```

---

## Putting It All Together

### Example: Quarterly Revenue Review Slide Sequence

A data-heavy section of a quarterly review deck might use these layouts in sequence:

| Slide | Layout | Content |
|-------|--------|---------|
| 1 | **D4 — KPI Dashboard** | Revenue, growth, margin, customers → with a 12-month trend line below |
| 2 | **D1 — Chart + Insight** | Revenue by product line (stacked bar) + key insight about top performer |
| 3 | **D5 — Doughnut + Breakdown** | Revenue by region with center showing total |
| 4 | **D3 — Dual Chart** | This year vs. last year (side-by-side column charts) |
| 5 | **D6 — Data Table** | Detailed breakdown with change column highlighted |

**Key principle**: Vary the chart types AND the layouts. Never show two column charts on consecutive slides. Alternate between layouts D1–D7 just as you alternate between the main layout library patterns.

---

## Speaker Notes for Data Slides

Data slides need the **strongest speaker notes** in the deck. The chart shows evidence — the notes tell the presenter what story to tell, what caveats to mention, and how to handle questions. Every data layout function (D1–D7) accepts a `speakerNotes` parameter. **Never leave it empty.**

### What Data Slide Notes Must Contain

Every data slide's speaker notes should cover these five elements:

1. **The headline story** — the "so what?" in plain language. What should the audience walk away remembering from this slide? Start with this.
2. **Walk-through guidance** — how to narrate the chart. Which data point to call out first, where to draw the audience's eye, what comparison to highlight.
3. **Context & caveats** — methodology notes, data source, time period, any asterisks. Things the audience might ask about that don't belong on-slide.
4. **Transition hook** — one sentence that bridges to the next slide. "Now that we've seen the revenue picture, let's look at what's driving this growth..."
5. **Q&A prep** — 1-2 anticipated tough questions and suggested answers. Critical for KPI dashboards and financial data slides.

### Speaker Notes Templates by Layout

Use these as starting templates — customize for each slide's specific content.

#### D1 (Chart + Insight) Notes Template

```
STORY: [One sentence: what this chart proves]

WALK-THROUGH: Direct the audience's attention to [specific data point/trend]. 
Note that [key comparison or change]. The insight panel highlights [stat] 
which represents [context for why this matters].

CONTEXT: Data covers [time period]. Source: [source]. Note that [any caveat 
or methodology detail — e.g., "excludes one-time items" or "uses constant 
currency"].

TRANSITION: [Bridge to next slide — e.g., "This growth is impressive, but 
where is it coming from? Let's break it down by region on the next slide."]

Q&A PREP:
- "Why did [metric] dip in [period]?" → [Prepared answer]
- "How does this compare to [competitor/benchmark]?" → [Prepared answer]
```

#### D4 (KPI Dashboard) Notes Template

```
STORY: [Executive summary — one sentence covering the overall health picture]

WALK-THROUGH: Start with [most important KPI] at [value] — this is 
[up/down X%] from [comparison period]. Then highlight [second KPI] which 
shows [trend]. The supporting chart below reveals [pattern].

CONTEXT: All KPIs are as of [date]. [KPI 1] is calculated as [definition]. 
[Any KPI that might be unfamiliar needs a one-line explanation here.]

TRANSITION: [Bridge — e.g., "The headline numbers look strong. Let's dig 
into the product-level breakdown to understand what's driving them."]

Q&A PREP:
- "Why is [KPI] measured this way?" → [Definition and rationale]
- "What's the target for [KPI]?" → [Target and how we're tracking]
- "This looks different from last quarter's report." → [Explain any 
  methodology changes or restatements]
```

#### D3 (Dual Chart) Notes Template

```
STORY: [The comparison headline — what the side-by-side reveals]

WALK-THROUGH: On the left, [describe Chart A in one sentence]. On the 
right, [describe Chart B]. The key takeaway from comparing them is 
[insight — e.g., "growth accelerated in the second half" or "the new 
market is outpacing the established one"].

CONTEXT: [Any differences in how the two charts should be read — e.g., 
different scales, different time periods, different populations.]

TRANSITION: [Bridge to next slide]
```

#### D5 (Doughnut + Breakdown) Notes Template

```
STORY: [What the allocation/distribution reveals]

WALK-THROUGH: The total [center value] breaks down as follows: [largest 
segment] dominates at [X%], followed by [second] at [Y%]. Pay attention 
to [notable segment — fastest growing, surprisingly large/small].

CONTEXT: [How segments are defined. Any segments that were combined or 
excluded. Whether this represents revenue, units, headcount, etc.]

TRANSITION: [Bridge — e.g., "Now let's look at how these segments have 
shifted over time."]
```

#### D6 (Data Table) Notes Template

```
STORY: [What the table reveals when you read across/down]

WALK-THROUGH: The highlighted column shows [metric]. Draw attention to 
[Row X] which shows [notable value]. Compare [Row A] vs [Row B] to see 
[insight]. The overall pattern is [trend].

CONTEXT: [Column definitions if not self-evident. Units. Time period. 
Any rows that are estimates vs. actuals.]

TRANSITION: [Bridge to next slide]

Q&A PREP:
- "Can you send this data?" → [Yes/no and how]
- "Why is [Row X] so different?" → [Explanation]
```

#### D7 (Mini-Chart Grid) Notes Template

```
STORY: [Dashboard-level summary — one sentence covering all four charts]

WALK-THROUGH: Top-left shows [Chart 1 insight]. Top-right shows [Chart 2 
insight]. Bottom-left reveals [Chart 3 insight]. Bottom-right highlights 
[Chart 4 insight]. The most important takeaway across all four is [overall 
pattern].

CONTEXT: [Time period. Any charts using different scales or metrics. Data 
freshness — "as of [date]".]

TRANSITION: [Bridge — typically transitions out of the data section into 
recommendations or next steps]
```

### Helper: Auto-Generate Notes Skeleton

Use this helper to generate a notes skeleton from chart metadata, then customize:

```javascript
/**
 * Generate speaker notes skeleton for a data slide.
 * Returns a string ready for slide.addNotes().
 * 
 * @param {object} opts
 * @param {string} opts.insight - The "so what?" headline
 * @param {string} opts.walkthrough - How to narrate the visual
 * @param {string} opts.source - Data source citation
 * @param {string} opts.caveat - Methodology note or asterisk (optional)
 * @param {string} opts.transition - Bridge sentence to next slide
 * @param {string[]} opts.qaPrep - Array of "Q → A" strings (optional)
 */
function makeDataSlideNotes({ insight, walkthrough, source, caveat, transition, qaPrep }) {
  let notes = `KEY INSIGHT: ${insight}\n\n`;
  notes += `WALK-THROUGH: ${walkthrough}\n\n`;
  notes += `SOURCE: ${source}`;
  if (caveat) {
    notes += ` | NOTE: ${caveat}`;
  }
  notes += '\n\n';
  notes += `TRANSITION: ${transition}`;
  if (qaPrep && qaPrep.length > 0) {
    notes += '\n\nQ&A PREP:\n';
    qaPrep.forEach(qa => {
      notes += `• ${qa}\n`;
    });
  }
  return notes;
}

// Usage example:
slide.addNotes(makeDataSlideNotes({
  insight: 'Revenue grew 58% YoY, driven primarily by the APAC region which tripled its contribution.',
  walkthrough: 'Start with the overall bar height showing $7.1M in Q4. Then point to the APAC segment (green) which grew from 8% to 22% of total. North America remains the largest segment but growth there has plateaued.',
  source: 'Internal finance dashboard, Q4 2025 close',
  caveat: 'APAC figures include the Tanaka Corp acquisition completed in July. Organic APAC growth was approximately 180%.',
  transition: 'This regional shift has big implications for our 2026 hiring plan, which we will cover next.',
  qaPrep: [
    'Q: "Is the APAC growth sustainable without more acquisitions?" → A: Organic pipeline is 3× larger than pre-acquisition, suggesting yes.',
    'Q: "Why did EMEA decline?" → A: Currency headwinds. In constant currency, EMEA grew 4%.',
  ],
}));
```

### Rules for Data Slide Notes

1. **Never skip notes on data slides** — these are the slides where presenters need the most help
2. **Write in presenter voice** — notes should read like a script, not a report. Use "you" and "we" and "notice that..."
3. **Front-load the insight** — the first sentence should be the headline takeaway. If the presenter only glances at notes, they see the most important thing
4. **Include the actual numbers** — don't make the presenter squint at the chart during the presentation. Repeat the key values in the notes
5. **Anticipate skepticism** — data slides attract the most questions. Always include at least one Q&A prep item
6. **Keep transition hooks specific** — "Let's move on" is lazy. "Now that we've seen the revenue picture, let's look at what's driving the margin expansion" tells the presenter exactly how to bridge
