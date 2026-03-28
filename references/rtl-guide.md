# RTL & Multi-Language Support Guide

Design and implementation guide for Arabic, Hebrew, Persian, Urdu, and other RTL (right-to-left) content in PptxGenJS presentations.

---

## When RTL Mode Activates

RTL support activates automatically when:
- The user's content contains >30% RTL characters (Arabic, Hebrew, Persian, Urdu)
- The user explicitly requests Arabic/RTL slides
- The user provides content in an RTL language

**Detection function** (see `references/code-helpers.md` for implementation):
```javascript
function isRTL(text) {
  const rtlChars = /[\u0600-\u06FF\u0750-\u077F\u08A0-\u08FF\uFB50-\uFDFF\uFE70-\uFEFF\u0590-\u05FF\uFB1D-\uFB4F]/;
  const matches = (text.match(new RegExp(rtlChars.source, 'g')) || []).length;
  return matches > text.length * 0.3;
}
```

---

## Layout Mirroring Rules

When RTL mode is active, mirror the horizontal layout of every slide:

### What Gets Mirrored
- Text alignment: `left` → `right`, `right` → `left` (center stays center)
- Two-column layouts: visual on right → visual on left, text on left → text on right
- Icon rows: reading order reverses (rightmost = first item)
- Timeline/process flows: right-to-left direction
- Chart + Insight panels: chart moves right, insight panel moves left
- Card grids: reading order reverses

### What Does NOT Get Mirrored
- Centered elements (titles, quotes)
- Full-width charts (they fill the slide regardless)
- Background shapes and decorative elements
- Vertical positioning (y-axis stays the same)

### Position Mirroring Helper

```javascript
/**
 * Mirror an x-position for RTL layout.
 * @param {number} x - Original x position (inches from left)
 * @param {number} w - Element width (inches)
 * @param {number} slideW - Slide width (default 10")
 * @returns {number} Mirrored x position
 */
function rtlX(x, w, slideW = 10) {
  return slideW - x - w;
}

// Example: element at x=0.8, w=4.0 on a 10" slide
// rtlX(0.8, 4.0) = 10 - 0.8 - 4.0 = 5.2
```

---

## PptxGenJS RTL Text Configuration

### Basic RTL Text

```javascript
slide.addText('النص العربي هنا', {
  x: 0.8, y: 1.5, w: 8.4, h: 0.8,
  rtlMode: true,
  fontSize: 24, fontFace: 'Arial', // Arial has excellent Arabic support
  color: TEXT_COLOR,
  align: 'right', // RTL text should right-align
});
```

### Mixed Content (Arabic + English)

When slides contain both RTL and LTR text (common in technical presentations):

```javascript
// Title in Arabic (RTL)
slide.addText('تحليل البيانات المالية', {
  x: 0.8, y: 0.5, w: 8.4, h: 0.8,
  rtlMode: true,
  fontSize: 36, fontFace: HEADER_FONT_AR, bold: true,
  color: 'FFFFFF', align: 'right',
});

// Subtitle in English (LTR) — still right-aligned for visual consistency
slide.addText('Financial Data Analysis — Q3 2026', {
  x: 0.8, y: 1.3, w: 8.4, h: 0.5,
  rtlMode: false,
  fontSize: 16, fontFace: BODY_FONT,
  color: ACCENT_COLOR, align: 'right',
});
```

---

## Font Selection for RTL

### Safe Arabic/RTL Fonts in PptxGenJS

PptxGenJS uses system fonts. These are reliably available:

| Font | Quality | Best For |
|------|---------|----------|
| **Arial** | Excellent Arabic | Body text, most content |
| **Calibri** | Good Arabic | Body text (Windows) |
| **Tahoma** | Very good Arabic | Headings, bold text |
| **Segoe UI** | Excellent Arabic | Modern look |
| **Times New Roman** | Good Arabic | Formal/academic |

### Recommended Font Pairings for Arabic Decks

| Voice | Header Font | Body Font |
|-------|-------------|-----------|
| **Corporate / Professional** | Tahoma | Arial |
| **Modern / Clean** | Segoe UI | Arial |
| **Academic / Formal** | Times New Roman | Arial |
| **Technical** | Calibri | Calibri |

### Font Constants for RTL Decks

```javascript
// Add these alongside your theme constants
const HEADER_FONT_AR = 'Tahoma';   // Bold, clear Arabic headers
const BODY_FONT_AR = 'Arial';       // Clean Arabic body text
```

---

## RTL Layout Patterns

### Mirrored Two-Column Split (Layout 3)

Original (LTR):
```
┌──────────────────────────────────┐
│  [Title - full width, right]     │
│  ┌──────────┐  ┌──────────────┐ │
│  │  Visual   │  │  Text        │ │
│  │  (chart)  │  │  content     │ │
│  └──────────┘  └──────────────┘ │
└──────────────────────────────────┘
```

```javascript
// RTL: Visual left, Text right (reading starts right)
const textX = rtlEnabled ? 5.0 : 0.5;
const textW = 4.0;
const visualX = rtlEnabled ? 0.5 : 5.0;
const visualW = 4.5;

slide.addText(content, {
  x: textX, y: 1.5, w: textW, h: 3.5,
  rtlMode: rtlEnabled,
  align: rtlEnabled ? 'right' : 'left',
  fontSize: 14, fontFace: rtlEnabled ? BODY_FONT_AR : BODY_FONT,
});
```

### Mirrored Icon Row (Layout 4)

```javascript
// Icons read right-to-left in RTL mode
const items = [...]; // 3 items
const rtlItems = rtlEnabled ? [...items].reverse() : items;

for (let i = 0; i < rtlItems.length; i++) {
  const x = 0.8 + i * 3.2; // Same x positions, items reversed
  await addIconInCircle(slide, pres, rtlItems[i].cat, rtlItems[i].idx, x + 0.3, 2.5, 0.5, accentColor);
  slide.addText(rtlItems[i].title, {
    x: x, y: 3.4, w: 1.5, h: 0.4,
    rtlMode: rtlEnabled,
    fontSize: 16, bold: true, align: 'center',
    fontFace: rtlEnabled ? BODY_FONT_AR : BODY_FONT,
  });
}
```

### Mirrored Timeline (Layout 7)

In RTL, timelines flow right-to-left:

```javascript
const steps = [...]; // e.g., 4 steps
const rtlSteps = rtlEnabled ? [...steps].reverse() : steps;
// Same rendering code, items already reversed
```

### Mirrored Chart + Insight (Layout 8)

```javascript
const chartX = rtlEnabled ? 3.5 : 0.5;
const chartW = 6.0;
const insightX = rtlEnabled ? 0.5 : 7.0;
const insightW = 2.5;

slide.addChart(pres.charts.BAR, data, {
  x: chartX, y: 1.3, w: chartW, h: 3.8,
  ...makeChartStyle(theme),
});

// Insight panel
slide.addShape(pres.shapes.RECTANGLE, {
  x: insightX, y: 1.3, w: insightW, h: 3.8,
  fill: { color: theme.ACCENT },
});
slide.addText(insightText, {
  x: insightX + 0.2, y: 1.5, w: insightW - 0.4, h: 3.4,
  rtlMode: rtlEnabled,
  align: rtlEnabled ? 'right' : 'left',
  fontSize: 14, color: 'FFFFFF',
  fontFace: rtlEnabled ? BODY_FONT_AR : BODY_FONT,
});
```

---

## RTL-Aware Generation Wrapper

Wrap your entire slide generation in an RTL-aware context:

```javascript
/**
 * Create an RTL-aware slide factory.
 * Automatically applies RTL settings to all text and mirrors positions.
 */
function createRTLContext(rtlEnabled) {
  return {
    rtl: rtlEnabled,
    headerFont: rtlEnabled ? HEADER_FONT_AR : HEADER_FONT,
    bodyFont: rtlEnabled ? BODY_FONT_AR : BODY_FONT,
    
    // Text helper — auto-applies RTL settings
    text(content, options) {
      return {
        ...options,
        rtlMode: rtlEnabled,
        fontFace: options.fontFace || (options.bold ? this.headerFont : this.bodyFont),
        align: rtlEnabled 
          ? (options.align === 'left' ? 'right' : (options.align === 'right' ? 'left' : options.align))
          : options.align,
      };
    },
    
    // Position helper — mirrors x for RTL
    x(x, w) {
      return rtlEnabled ? rtlX(x, w) : x;
    },
    
    // Array helper — reverses item order for RTL reading
    order(items) {
      return rtlEnabled ? [...items].reverse() : items;
    },
  };
}

// Usage:
const ctx = createRTLContext(isRTL(userContent));

slide.addText(title, ctx.text(title, {
  x: 0.8, y: 0.5, w: 8.4, h: 0.8,
  fontSize: 36, bold: true, color: 'FFFFFF',
}));
```

---

## Bidirectional Content Considerations

1. **Numbers in Arabic text**: Arabic text uses Western numerals (0-9) or Eastern Arabic numerals (٠-٩). PptxGenJS handles this correctly with `rtlMode: true`.

2. **Parentheses and brackets**: These may render in unexpected positions in mixed content. Test with `rtlMode: true` and verify visually.

3. **Brand names and URLs**: Keep these in LTR even within RTL paragraphs. PptxGenJS handles this automatically for Latin characters within RTL text blocks.

4. **Charts**: Chart axes remain LTR (numbers read left-to-right universally). Category labels can be RTL — PptxGenJS renders them correctly with RTL font.

5. **Speaker notes**: Apply `rtlMode` to notes if they're in Arabic:
   ```javascript
   slide.addNotes('ملاحظات المتحدث هنا...');
   // Notes don't have rtlMode, but the text will render correctly
   // in PowerPoint's notes panel which auto-detects RTL
   ```

---

## QA Checklist for RTL Decks

During Phase 5 QA, additionally check:
- [ ] All text blocks have `rtlMode: true` where content is RTL
- [ ] Two-column layouts are mirrored (text right, visual left)
- [ ] Icon rows and timelines read right-to-left
- [ ] Font renders Arabic/Hebrew characters correctly (no boxes or missing glyphs)
- [ ] Mixed content (Arabic + English) has correct alignment per text block
- [ ] Chart labels are readable (category labels may need RTL font)
- [ ] Numbers display correctly (Western or Eastern Arabic as appropriate)
- [ ] Speaker notes are present and readable
