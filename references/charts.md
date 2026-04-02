# Chart Recipes (Compact)

Read this ONLY when a deck contains data visualization slides. Every chart uses `makeChartStyle()` — never use raw defaults.

## CRITICAL: Never share style objects between `addChart()` calls. Always call `makeChartStyle()` fresh.

## Core Functions

```javascript
function makeChartStyle(theme, overrides = {}) {
  return {
    x: 0.5, y: 1.3, w: 9, h: 3.8,
    plotArea: { fill: { color: theme.BG_LIGHT || 'FFFFFF' } },
    catAxisLabelColor: theme.TEXT_MUTED || '6B7280', catAxisLabelFontSize: 10, catAxisLabelFontFace: theme.BODY_FONT || 'Calibri',
    valAxisLabelColor: theme.TEXT_MUTED || '6B7280', valAxisLabelFontSize: 10, valAxisLabelFontFace: theme.BODY_FONT || 'Calibri',
    valGridLine: { color: theme.SECONDARY ? theme.SECONDARY + '80' : 'E5E7EB', size: 0.5 },
    catGridLine: { style: 'none' },
    catAxisLineShow: false, valAxisLineShow: false,
    chartColors: overrides.chartColors || [theme.ACCENT||'4A90D9', theme.PRIMARY||'1E2761', theme.SECONDARY||'CADCFC', lightenHex(theme.ACCENT||'4A90D9',40), lightenHex(theme.PRIMARY||'1E2761',30)],
    showLegend: overrides.showLegend ?? false, legendPos: overrides.legendPos || 'b', legendColor: theme.TEXT_MUTED||'6B7280', legendFontSize: 9, legendFontFace: theme.BODY_FONT||'Calibri',
    showValue: overrides.showValue ?? false, dataLabelColor: theme.TEXT_DARK||'1F2937', dataLabelFontSize: 9, dataLabelFontFace: theme.BODY_FONT||'Calibri',
    ...overrides,
  };
}

function lightenHex(hex, percent) {
  hex = hex.replace('#','');
  const r = Math.min(255, Math.round(parseInt(hex.slice(0,2),16) + (255-parseInt(hex.slice(0,2),16))*percent/100));
  const g = Math.min(255, Math.round(parseInt(hex.slice(2,4),16) + (255-parseInt(hex.slice(2,4),16))*percent/100));
  const b = Math.min(255, Math.round(parseInt(hex.slice(4,6),16) + (255-parseInt(hex.slice(4,6),16))*percent/100));
  return [r,g,b].map(c=>c.toString(16).padStart(2,'0')).join('');
}

function makeColorRamp(baseHex, count) {
  const ramp = [];
  for (let i=0; i<count; i++) ramp.push(lightenHex(baseHex, (i/(count-1))*60));
  return ramp;
}
```

## Recipes

### Column Bar
```javascript
slide.addChart(pres.charts.BAR, [{ name:'Revenue', labels:['Q1','Q2','Q3','Q4'], values:[4500,5500,6200,7100] }],
  makeChartStyle(theme, { barDir:'col', barGapWidthPct:100, showValue:true, dataLabelPosition:'outEnd', chartColors:[theme.ACCENT] }));
```
Multi-series: add `showLegend:true, chartColors:[theme.SECONDARY, theme.ACCENT]`
Stacked: add `barGrouping:'stacked', dataLabelPosition:'ctr', chartColors:makeColorRamp(theme.PRIMARY,4)`

### Horizontal Bar
```javascript
slide.addChart(pres.charts.BAR, [{ name:'Share', labels:labels, values:values }],
  makeChartStyle(theme, { barDir:'bar', barGapWidthPct:60, showValue:true, dataLabelPosition:'outEnd', catAxisOrientation:'maxMin', chartColors:makeColorRamp(theme.ACCENT,5) }));
```

### Line
```javascript
slide.addChart(pres.charts.LINE, [{ name:'Revenue', labels:months, values:data }],
  makeChartStyle(theme, { lineSize:2.5, lineSmooth:true, lineDataSymbol:'circle', lineDataSymbolSize:6, chartColors:[theme.ACCENT] }));
```
Multi-line: add `showLegend:true, chartColors:[theme.ACCENT, theme.PRIMARY, theme.SECONDARY]`

### Pie (≤6 segments)
```javascript
slide.addChart(pres.charts.PIE, [{ name:'Mix', labels:labels, values:values }],
  makeChartStyle(theme, { x:0.5, y:1.2, w:4.5, h:4.0, showPercent:true, chartColors:makeColorRamp(theme.ACCENT,5) }));
```

### Doughnut
```javascript
slide.addChart(pres.charts.DOUGHNUT, [{ name:'Mix', labels:labels, values:values }],
  makeChartStyle(theme, { x:1, y:1.2, w:4, h:4, holeSize:65, showPercent:true, chartColors:[theme.ACCENT, theme.PRIMARY, theme.SECONDARY, 'E8E8E8'] }));
```
Center callout: `slide.addText(pctText, { x:cx, y:cy, w:1.5, h:0.8, fontSize:36, bold:true, color:theme.ACCENT, align:'center' })`

### Area (stacked composition)
```javascript
slide.addChart(pres.charts.AREA, seriesData,
  makeChartStyle(theme, { chartColors:[theme.ACCENT, theme.PRIMARY, lightenHex(theme.SECONDARY,20)], showLegend:true, legendPos:'b' }));
```

## Insight Patterns

**Adjacent box** (Layout 8): chart left 60%, insight card right 30%
```javascript
slide.addShape(pres.shapes.RECTANGLE, { x:insightX, y:insightY, w:insightW, h:insightH, fill:{color:theme.BG_LIGHT}, rectRadius:0.08 });
slide.addText(statNum, { ...pos, fontSize:36, bold:true, color:theme.ACCENT });
slide.addText(insightText, { ...pos, fontSize:11, color:theme.TEXT_DARK, italic:true });
```

**Inline below chart**: `slide.addText('→ '+insight, { x:chartX, y:chartY+chartH+0.1, w:chartW, h:0.35, fontSize:11, color:theme.ACCENT, italic:true })`

## Rules
- State what data MEANS, not what it SHOWS
- Max 2 sentences per insight
- Use accent color or italic for insights
- No gridlines in minimal/Orange themes
- Pie: max 6 segments. More → horizontal bar
- Always pair charts with context (stat callout or insight text)
