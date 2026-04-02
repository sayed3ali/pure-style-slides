# Layout Library (Compact)

20 slide layout patterns. Use 4+ different layouts per 10-slide deck. Never repeat on consecutive slides. Alternate visual weight: heavy→medium→light.

## Index

| # | Layout | Best For | Weight |
|---|--------|----------|--------|
| 1 | Title Hero | Opening | Heavy |
| 2 | Section Divider | Chapter breaks | Medium |
| 3 | Two-Column Split | Text+visual, comparison | Medium |
| 4 | Icon Row | 3-4 features/benefits | Medium |
| 5 | Stat Callout | Key metrics, KPIs | Heavy |
| 6 | Card Grid | Team, products, categories | Medium |
| 7 | Timeline / Process | Steps, phases, history | Medium |
| 8 | Chart + Insight | Data with commentary | Medium |
| 9 | Quote | Quotes, key messages | Light |
| 10 | Image + Overlay | Visual storytelling | Heavy |
| 11 | Table | Comparisons, specs | Light |
| 12 | Closing / CTA | Final slide | Heavy |
| 13 | Agenda | Table of contents | Light |
| 14 | Before / After | Transformations | Medium |
| 15 | Multi-Period Table | Earnings, guidance vs actuals | Medium |
| 16 | KPI Card Row | Headline metrics w/ icons | Heavy |
| 17 | Strategy Dashboard | Hero chart + mini-metric grid | Heavy |
| 18 | Product Feature | App mockup + stats | Medium |
| 19 | Bold Section Divider | Full-bleed dramatic break | Heavy |
| 20 | PoP Bar Comparison | Side-by-side period bars | Medium |

---

## 1. Title Hero
Dark/themed bg. Title 44pt bold white. Subtitle 20pt accent. Date/author 12pt bottom. Optional accent bar.
```javascript
slide.background = { color: BG_DARK };
slide.addText(title, { x:0.8, y:1.5, w:8.4, h:1.5, fontSize:44, fontFace:HEADER_FONT, color:'FFFFFF', bold:true });
slide.addText(subtitle, { x:0.8, y:3.0, w:8.4, h:0.8, fontSize:20, fontFace:BODY_FONT, color:ACCENT });
slide.addText(dateStr, { x:0.8, y:4.8, w:8.4, h:0.4, fontSize:12, fontFace:BODY_FONT, color:TEXT_MUTED });
```

## 2. Section Divider
Colored bg. Section number or icon top. Title 36pt bold. Brief description 14pt muted.

## 3. Two-Column Split
Left 55% text, right 45% visual (chart/icons/image). Variants: text-left or text-right. Proportions: x:0.5-4.5 left, x:5.0-9.5 right.

## 4. Icon Row
3-4 equal columns. Each: icon in colored circle (0.7" dia, 0.4" icon inside) → bold label → description.
```javascript
slide.addShape(pres.shapes.OVAL, { x:colX+(colW-0.7)/2, y:iconY, w:0.7, h:0.7, fill:{color:ACCENT} });
slide.addImage({ data:iconBase64, x:colX+(colW-0.4)/2, y:iconY+0.15, w:0.4, h:0.4 });
```

## 5. Stat Callout
2-4 stat blocks in row. Number 60-72pt bold accent. Label 14-16pt muted. Optional colored top bar.

## 6. Card Grid
2×2 or 1×3 white cards with shadow on colored bg. Each: optional icon → bold title → description. Optional accent bar on left edge.
```javascript
slide.addShape(pres.shapes.RECTANGLE, { x, y, w, h, fill:{color:'FFFFFF'}, shadow:{type:'outer',blur:3,offset:2,color:'000000',opacity:0.15}, rectRadius:0.1 });
```

## 7. Timeline / Process
Horizontal: numbered circles (OVAL) connected by lines. Labels + descriptions below each.

## 8. Chart + Insight
Chart left 60%. Insight box right 30% (colored bg, large stat, text). Source at bottom.

## 9. Quote
Centered. Large decorative quotes 60pt accent. Quote text 20-24pt. Attribution 14pt muted with em-dash.

## 10. Image + Overlay
Full/half-bleed image + text overlay. For placeholders: gray rectangle with "[Your Image Here]".

## 11. Table
Colored header row (PRIMARY bg, white text). Alternating row colors. Thin borders.
```javascript
const headerRow = [{ text:'Col', options:{ fill:{color:PRIMARY}, color:'FFFFFF', bold:true }}];
```

## 12. Closing / CTA
Mirrors Title Hero. CTA text or "Thank You" + contact details. Same dark bg (bookend).

## 13. Agenda
Numbered list with bold section names + brief descriptions. Clean, spacious.

## 14. Before / After
Two equal columns: "Before" (muted/gray) and "After" (vibrant/accent). Visual distinction via color.

## 15. Multi-Period Table
Full-width table, 3-4 columns with distinct header colors per period. Dual-line cells (headline + detail).
```javascript
const makeCell = (headline, detail, isLabel=false) => ({
  text: [
    { text: headline+'\n', options:{ fontSize:13, bold:true, color:TEXT_DARK }},
    { text: detail, options:{ fontSize:10, color:TEXT_MUTED }}
  ],
  options: { fill:{color: isLabel?'F5F5F5':'FFFFFF'}, align: isLabel?'left':'center', valign:'middle' }
});
slide.addTable([headerRow, ...dataRows], { x:0.4, y:1.2, w:9.2, border:{type:'solid',pt:0.5,color:'E0E0E0'} });
```

## 16. KPI Card Row
3-4 dark cards in row. Each: icon (white variant) → unit label → large stat (36pt bold white) → growth % (accent) → metric label.
```javascript
const cardW=2.05, cardH=3.0, gap=0.2;
const startX = (10 - (kpis.length*cardW + (kpis.length-1)*gap)) / 2;
kpis.forEach((kpi, i) => {
  const cx = startX + i*(cardW+gap), cy = 1.2;
  slide.addShape(pres.shapes.RECTANGLE, { x:cx, y:cy, w:cardW, h:cardH, fill:{color:BG_DARK}, rectRadius:0.1 });
  // icon, unit, value (36pt), growth, label inside card
});
```

## 17. Strategy Dashboard
Left 45%: hero chart with trend callout. Right 55%: 2×3 mini-metric card grid. Each mini-card: title, subtitle, growth badge (oval), mini bar pair.
```javascript
function addMiniMetricCard(slide, pres, { x, y, w, h, title, subtitle, values, growth, theme }) {
  slide.addShape(pres.shapes.RECTANGLE, { x, y, w, h, fill:{color:theme.BG_LIGHT}, rectRadius:0.06 });
  slide.addText(title, { x:x+0.1, y:y+0.08, w:w-0.2, h:0.25, fontSize:10, bold:true, color:theme.TEXT_DARK });
  // growth badge oval, mini bar pair
}
```

## 18. Product Feature
Left 35%: phone mockup (rounded rect frame). Center 35%: feature list with icon bullets. Right 30%: stat callouts.
```javascript
function addPhoneMockup(slide, x, y, w, h, theme) {
  slide.addShape(pres.shapes.ROUNDED_RECTANGLE, { x, y, w, h, rectRadius:0.25, fill:{color:'1A1A2E'} });
  slide.addShape(pres.shapes.ROUNDED_RECTANGLE, { x:x+0.08, y:y+0.25, w:w-0.16, h:h-0.35, rectRadius:0.15, fill:{color:'FFFFFF'} });
}
```

## 19. Bold Section Divider
Full-bleed primary/accent color. Single word/phrase 80-120pt ultra-bold white. Maximum impact.
```javascript
slide.background = { color: ACCENT };
slide.addText(title, { x:0.8, y:1.5, w:8.4, h:2.5, fontSize:96, fontFace:HEADER_FONT, color:'FFFFFF', bold:true, italic:true });
```

## 20. PoP Bar Comparison
2-3 panels side by side. Each: colored header bar → two bars (period A vs B) → growth callout bubble (oval) → insight text.
```javascript
function addPoPPanel(slide, pres, { x, y, w, h, headerText, headerColor, bars, growth, insight, theme }) {
  // header bar, panel bg, bars scaled to max, growth oval between, insight box below
  const maxVal = Math.max(...bars.map(b=>b.value));
  bars.forEach((bar, i) => {
    const barH = (bar.value/maxVal) * barAreaH;
    slide.addShape(pres.shapes.RECTANGLE, { x:bx, y:by, w:barW, h:barH, fill:{color: i===0?theme.ACCENT:theme.PRIMARY} });
  });
  slide.addShape(pres.shapes.OVAL, { x:bubbleX, y:bubbleY, w:0.7, h:0.35, fill:{color:theme.BG_DARK} });
  slide.addText(growth, { x:bubbleX, y:bubbleY, w:0.7, h:0.35, fontSize:10, bold:true, color:'FFFFFF', align:'center', valign:'middle' });
}
```

---

## Sequencing Rules
1. Never repeat same layout consecutively
2. Start: Layout 1. End: Layout 12
3. Section dividers (2 or 19) break deck into 2-3 sections
4. Alternate visual weight: heavy→medium→light→medium→heavy
5. Data slides followed by lighter slides
6. 10-slide deck = minimum 5 layouts
7. Financial decks: emphasize 15, 16, 20. Product decks: include 18. Strategy decks: use 17
