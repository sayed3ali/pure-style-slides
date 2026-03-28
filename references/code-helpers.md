# Code Helpers Reference

Reusable code patterns for PptxGenJS slide generation. Load this file during Phase 3 (Design & Generate) — not during Blueprint phase.

---

## Layout Sequencer

Programmatic enforcement of layout variety rules. Call this during generation to assign layouts automatically based on slide types.

```javascript
/**
 * Assign varied layouts to a slide sequence, enforcing design rules:
 * - No consecutive repeats
 * - Visual weight alternation (heavy → medium → light → medium)
 * - Bookend structure (Title Hero first, Closing last)
 * - Section dividers every 4-5 content slides
 * - Minimum 5 different layouts for a 10-slide deck
 *
 * @param {Array<{type: string, hasData: boolean, isSection: boolean}>} slides
 *   - type: 'title' | 'content' | 'data' | 'quote' | 'comparison' | 'closing' | 'section' | 'agenda'
 *   - hasData: true if slide contains charts/stats
 *   - isSection: true if this is a section divider
 * @returns {Array<{slideIndex: number, layout: number, layoutName: string}>}
 */
function sequenceLayouts(slides) {
  const LAYOUTS = {
    1:  { name: 'Title Hero',       weight: 'heavy',  types: ['title'] },
    2:  { name: 'Section Divider',  weight: 'medium', types: ['section'] },
    3:  { name: 'Two-Column Split', weight: 'medium', types: ['content', 'comparison'] },
    4:  { name: 'Icon Row',         weight: 'medium', types: ['content'] },
    5:  { name: 'Stat Callout',     weight: 'heavy',  types: ['data'] },
    6:  { name: 'Card Grid',        weight: 'medium', types: ['content'] },
    7:  { name: 'Timeline/Process', weight: 'medium', types: ['content'] },
    8:  { name: 'Chart + Insight',  weight: 'medium', types: ['data'] },
    9:  { name: 'Quote',            weight: 'light',  types: ['quote'] },
    10: { name: 'Image + Overlay',  weight: 'heavy',  types: ['content'] },
    11: { name: 'Table Slide',      weight: 'light',  types: ['data', 'comparison'] },
    12: { name: 'Closing / CTA',    weight: 'heavy',  types: ['closing'] },
    13: { name: 'Agenda / Overview', weight: 'light',  types: ['agenda'] },
    14: { name: 'Before / After',   weight: 'medium', types: ['comparison'] },
    15: { name: 'Multi-Period Table', weight: 'medium', types: ['data', 'comparison'] },
    16: { name: 'KPI Card Row',     weight: 'heavy',  types: ['data'] },
    17: { name: 'Strategy Dashboard', weight: 'heavy', types: ['data', 'content'] },
    18: { name: 'Product Feature',  weight: 'medium', types: ['content'] },
    19: { name: 'Bold Section Divider', weight: 'heavy', types: ['section'] },
    20: { name: 'PoP Bar Comparison', weight: 'medium', types: ['data'] },
  };

  const WEIGHT_ORDER = ['heavy', 'medium', 'light', 'medium']; // repeating cycle
  const result = [];
  let lastLayout = -1;
  let weightIdx = 0;
  const usedLayouts = new Set();

  for (let i = 0; i < slides.length; i++) {
    const slide = slides[i];
    let chosen = null;

    // Fixed assignments
    if (i === 0) { chosen = 1; }
    else if (i === slides.length - 1) { chosen = 12; }
    else if (slide.type === 'section' || slide.isSection) { chosen = 2; }
    else if (slide.type === 'agenda') { chosen = 13; }

    if (!chosen) {
      // Find candidates matching slide type
      const targetWeight = WEIGHT_ORDER[weightIdx % WEIGHT_ORDER.length];
      const candidates = Object.entries(LAYOUTS)
        .filter(([id, l]) => {
          const num = parseInt(id);
          if (num === lastLayout) return false; // no consecutive repeat
          if (num === 1 || num === 12) return false; // reserved
          // Type match
          if (slide.hasData && l.types.includes('data')) return true;
          if (slide.type === 'quote' && l.types.includes('quote')) return true;
          if (slide.type === 'comparison' && l.types.includes('comparison')) return true;
          if (l.types.includes('content')) return true;
          return false;
        })
        .map(([id, l]) => ({ id: parseInt(id), ...l }));

      // Prefer matching weight, then unused layouts
      const weightMatch = candidates.filter(c => c.weight === targetWeight);
      const unused = (weightMatch.length > 0 ? weightMatch : candidates)
        .filter(c => !usedLayouts.has(c.id));
      const pool = unused.length > 0 ? unused : (weightMatch.length > 0 ? weightMatch : candidates);

      chosen = pool.length > 0 ? pool[Math.floor(Math.random() * pool.length)].id : 3;
      weightIdx++;
    }

    usedLayouts.add(chosen);
    lastLayout = chosen;
    result.push({
      slideIndex: i,
      layout: chosen,
      layoutName: LAYOUTS[chosen].name,
    });
  }

  // Validation: ensure minimum layout variety
  const unique = new Set(result.map(r => r.layout));
  if (slides.length >= 10 && unique.size < 5) {
    console.warn(`Warning: Only ${unique.size} unique layouts used for ${slides.length} slides. Consider manual adjustment.`);
  }

  return result;
}
```

### Usage in Generation Script

```javascript
// Define slide metadata from the approved Blueprint
const slideMeta = [
  { type: 'title', hasData: false, isSection: false },
  { type: 'agenda', hasData: false, isSection: false },
  { type: 'content', hasData: false, isSection: false },
  { type: 'data', hasData: true, isSection: false },
  { type: 'section', hasData: false, isSection: true },
  { type: 'content', hasData: false, isSection: false },
  { type: 'quote', hasData: false, isSection: false },
  { type: 'data', hasData: true, isSection: false },
  { type: 'comparison', hasData: false, isSection: false },
  { type: 'closing', hasData: false, isSection: false },
];

const layoutPlan = sequenceLayouts(slideMeta);
console.log('Layout plan:', layoutPlan.map(l => `Slide ${l.slideIndex + 1}: ${l.layoutName}`));
// Then use layoutPlan[i].layout to select the layout pattern for each slide
```

---

## Icon Keyword Lookup

Fast topic-to-icon resolution without scanning the full category guide. Load the keyword index from `references/icon-keywords.json`:

```javascript
const iconKeywords = JSON.parse(
  fs.readFileSync(path.join(SKILL_DIR, 'references', 'icon-keywords.json'), 'utf8')
);

/**
 * Find icons by topic keyword.
 * @param {string} keyword - Topic keyword (e.g., 'revenue', 'security', 'travel')
 * @returns {Array<{category: string, index: number, label: string}>}
 */
function findIcons(keyword) {
  const key = keyword.toLowerCase().replace(/\s+/g, '_');
  const matches = iconKeywords[key] || [];
  return matches.map(([cat, idx, label]) => ({ category: cat, index: idx, label }));
}

/**
 * Find the best icon for a topic, with automatic variant selection.
 * @param {string} keyword - Topic keyword
 * @param {boolean} white - true for white variant (dark backgrounds)
 * @returns {Promise<string|null>} Base64 PNG or null if no match
 */
async function findAndGetIcon(keyword, white = false) {
  const matches = findIcons(keyword);
  if (matches.length === 0) return null;
  const best = matches[0]; // first match is highest priority
  return await getIcon(best.category, best.index, white);
}

// Usage:
const revenueIcon = await findAndGetIcon('revenue');        // dark variant
const securityIcon = await findAndGetIcon('security', true); // white variant
```

---

## Icon Extraction Helpers

Core icon extraction functions (from sprite sheets):

```javascript
const sharp = require('sharp');
const path = require('path');
const fs = require('fs');

const SKILL_DIR = '/mnt/skills/user/pure-style-slides';
const ICON_DIR = path.join(SKILL_DIR, 'icons');
const manifest = JSON.parse(fs.readFileSync(path.join(ICON_DIR, 'manifest.json'), 'utf8'));
const iconCache = {};

/**
 * Extract a single icon from a sprite sheet and return as base64 PNG.
 * @param {string} category - One of: general, arrow, business, interface, buildings, user, other, brand
 * @param {number} index - Icon number (1-based)
 * @param {boolean} white - true for white variant (dark backgrounds)
 */
async function getIcon(category, index, white = false) {
  const catKey = white ? `${category}_white` : category;
  const cacheKey = `${catKey}_${index}`;
  if (iconCache[cacheKey]) return iconCache[cacheKey];
  
  const meta = manifest[catKey];
  if (!meta) throw new Error(`Unknown icon category: ${catKey}`);
  const iconInfo = meta.icons.find(i => i.index === index);
  if (!iconInfo) throw new Error(`Icon ${catKey}#${index} not found (max: ${meta.count})`);
  
  const spritePath = path.join(ICON_DIR, meta.sprite);
  const pngBuffer = await sharp(spritePath)
    .extract({ left: iconInfo.x, top: iconInfo.y, width: iconInfo.w, height: iconInfo.h })
    .png()
    .toBuffer();
  
  const base64 = 'image/png;base64,' + pngBuffer.toString('base64');
  iconCache[cacheKey] = base64;
  return base64;
}

/**
 * Add an icon inside a colored circle — the signature look of this skill.
 */
async function addIconInCircle(slide, pres, category, index, x, y, iconSize, circleColor) {
  const circleSize = iconSize * 1.6;
  const offset = (circleSize - iconSize) / 2;
  
  slide.addShape(pres.shapes.OVAL, {
    x: x - offset, y: y - offset,
    w: circleSize, h: circleSize,
    fill: { color: circleColor },
  });
  
  const iconData = await getIcon(category, index, true);
  slide.addImage({ data: iconData, x: x, y: y, w: iconSize, h: iconSize });
}
```

---

## RTL Text Helpers

For Arabic, Hebrew, and other RTL content. See `references/rtl-guide.md` for the full design system.

```javascript
/**
 * Create RTL-aware text options.
 * Flips alignment and adds RTL mode for PptxGenJS.
 */
function rtlText(options) {
  return {
    ...options,
    rtlMode: true,
    align: options.align === 'left' ? 'right' : (options.align === 'right' ? 'left' : options.align),
  };
}

/**
 * Mirror x-position for RTL layout (flip left↔right on 10" slide).
 * @param {number} x - Original x position
 * @param {number} w - Element width
 * @param {number} slideW - Slide width (default 10")
 */
function rtlX(x, w, slideW = 10) {
  return slideW - x - w;
}

/**
 * Detect if text content is primarily RTL.
 * Checks for Arabic, Hebrew, Persian, Urdu character ranges.
 */
function isRTL(text) {
  const rtlChars = /[\u0600-\u06FF\u0750-\u077F\u08A0-\u08FF\uFB50-\uFDFF\uFE70-\uFEFF\u0590-\u05FF\uFB1D-\uFB4F]/;
  const matches = (text.match(new RegExp(rtlChars.source, 'g')) || []).length;
  return matches > text.length * 0.3; // >30% RTL characters → RTL mode
}
```

---

## QA Automation Helpers

Programmatic checks to run during Phase 5 before visual inspection.

```javascript
const { execSync } = require('child_process');

/**
 * Run automated QA checks on a generated .pptx file.
 * Returns an array of issues found.
 *
 * @param {string} pptxPath - Path to the generated .pptx
 * @param {object} deckMeta - Metadata about the deck
 *   - slideCount: expected number of slides
 *   - durationMinutes: total presentation duration
 *   - theme: theme object with color values
 * @returns {Array<{slide: number, severity: string, issue: string}>}
 */
function runQAChecks(pptxPath, deckMeta) {
  const issues = [];

  // 1. Extract text content
  let content;
  try {
    content = execSync(`python -m markitdown "${pptxPath}"`, { encoding: 'utf8' });
  } catch (e) {
    issues.push({ slide: 0, severity: 'error', issue: 'Failed to extract text with markitdown' });
    return issues;
  }

  // 2. Count slides
  const slideHeaders = content.match(/^## Slide \d+/gm) || [];
  if (slideHeaders.length !== deckMeta.slideCount) {
    issues.push({
      slide: 0, severity: 'error',
      issue: `Expected ${deckMeta.slideCount} slides, found ${slideHeaders.length}`
    });
  }

  // 3. Check speaker notes presence
  const notesCount = (content.match(/### Notes:/g) || []).length;
  if (notesCount < slideHeaders.length) {
    const missing = slideHeaders.length - notesCount;
    issues.push({
      slide: 0, severity: 'warning',
      issue: `${missing} slide(s) missing speaker notes`
    });
  }

  // 4. Check for empty slides (slides with very little text)
  const slideSections = content.split(/^## Slide \d+/gm).slice(1);
  slideSections.forEach((section, i) => {
    const textOnly = section.replace(/### Notes:[\s\S]*?(?=^##|\Z)/gm, '').trim();
    if (textOnly.length < 20) {
      issues.push({
        slide: i + 1, severity: 'warning',
        issue: 'Slide has very little visible text content'
      });
    }
  });

  // 5. Slide count vs duration sanity check
  if (deckMeta.durationMinutes) {
    const expectedSlides = Math.round(deckMeta.durationMinutes / 2.5); // ~2.5 min per slide
    const ratio = slideHeaders.length / expectedSlides;
    if (ratio > 1.5) {
      issues.push({
        slide: 0, severity: 'warning',
        issue: `${slideHeaders.length} slides for ${deckMeta.durationMinutes} min may be too many (expected ~${expectedSlides})`
      });
    } else if (ratio < 0.5) {
      issues.push({
        slide: 0, severity: 'warning',
        issue: `${slideHeaders.length} slides for ${deckMeta.durationMinutes} min may be too few (expected ~${expectedSlides})`
      });
    }
  }

  return issues;
}

/**
 * Estimate text overflow risk.
 * Rough heuristic: characters per line ≈ box_width_inches × (72 / fontSize) × 1.8
 *
 * @param {string} text - Text content
 * @param {number} width - Text box width in inches
 * @param {number} height - Text box height in inches
 * @param {number} fontSize - Font size in points
 * @returns {{fits: boolean, linesNeeded: number, linesAvailable: number}}
 */
function estimateTextFit(text, width, height, fontSize) {
  const charsPerLine = Math.floor(width * (72 / fontSize) * 1.8);
  const lineHeight = fontSize * 1.4; // ~1.4× line spacing
  const linesAvailable = Math.floor((height * 72) / lineHeight);
  const linesNeeded = Math.ceil(text.length / charsPerLine);
  return {
    fits: linesNeeded <= linesAvailable,
    linesNeeded,
    linesAvailable,
  };
}

/**
 * Check WCAG color contrast ratio between two hex colors.
 * @param {string} fg - Foreground hex (e.g., 'FFFFFF')
 * @param {string} bg - Background hex (e.g., '1A1A1A')
 * @returns {{ratio: number, passAA: boolean, passAAA: boolean}}
 */
function checkContrast(fg, bg) {
  const luminance = (hex) => {
    const r = parseInt(hex.slice(0, 2), 16) / 255;
    const g = parseInt(hex.slice(2, 4), 16) / 255;
    const b = parseInt(hex.slice(4, 6), 16) / 255;
    const linearize = (c) => c <= 0.03928 ? c / 12.92 : Math.pow((c + 0.055) / 1.055, 2.4);
    return 0.2126 * linearize(r) + 0.7152 * linearize(g) + 0.0722 * linearize(b);
  };
  const l1 = luminance(fg.replace('#', ''));
  const l2 = luminance(bg.replace('#', ''));
  const ratio = (Math.max(l1, l2) + 0.05) / (Math.min(l1, l2) + 0.05);
  return {
    ratio: Math.round(ratio * 100) / 100,
    passAA: ratio >= 4.5,    // WCAG AA for normal text
    passAAA: ratio >= 7.0,   // WCAG AAA for normal text
  };
}
```

---

## Handout PDF Generator

Generate a printable handout from the completed deck.

```javascript
/**
 * Generate a markdown handout from the deck content.
 * Extracts slide titles, key content, and speaker notes into a printable format.
 *
 * @param {string} pptxPath - Path to the .pptx file
 * @param {string} outputPath - Path for the output markdown file
 * @param {object} options - { title, author, date }
 */
function generateHandout(pptxPath, outputPath, options = {}) {
  const content = execSync(`python -m markitdown "${pptxPath}"`, { encoding: 'utf8' });
  const slides = content.split(/^## Slide /gm).slice(1);

  let handout = `# ${options.title || 'Presentation Handout'}\n\n`;
  if (options.author) handout += `**Presenter**: ${options.author}\n`;
  if (options.date) handout += `**Date**: ${options.date}\n`;
  handout += '\n---\n\n';

  slides.forEach((slide, i) => {
    const lines = slide.trim().split('\n');
    const title = lines[0] || `Slide ${i + 1}`;
    handout += `## ${i + 1}. ${title}\n\n`;

    // Extract notes section
    const notesIdx = slide.indexOf('### Notes:');
    if (notesIdx >= 0) {
      const notesContent = slide.slice(notesIdx + '### Notes:'.length).trim();
      handout += notesContent + '\n\n';
    }
    handout += '---\n\n';
  });

  fs.writeFileSync(outputPath, handout, 'utf8');
}
```

### Converting Handout to PDF

```bash
# Using Pandoc (if available)
pandoc handout.md -o handout.pdf --pdf-engine=xelatex -V geometry:margin=1in

# Using LibreOffice (fallback)
libreoffice --headless --convert-to pdf handout.md

# Using Python + markdown + pdfkit (always available)
pip install markdown pdfkit --break-system-packages
python3 -c "
import markdown, pdfkit
with open('handout.md') as f:
    html = markdown.markdown(f.read(), extensions=['tables', 'fenced_code'])
html = f'<html><head><style>body{{font-family:Calibri,sans-serif;max-width:700px;margin:auto;padding:40px;line-height:1.6}}h1{{color:#1A1A1A}}h2{{color:#333;border-bottom:1px solid #ddd;padding-bottom:4px}}hr{{border:none;border-top:1px solid #eee}}</style></head><body>{html}</body></html>'
pdfkit.from_string(html, 'handout.pdf')
"
```

---

## Option Object Safety

**CRITICAL**: PptxGenJS mutates option objects in-place. Never share a single options object across multiple calls. Use factory functions:

```javascript
const makeCardShadow = () => ({
  type: "outer", blur: 6, offset: 2, angle: 135,
  color: "000000", opacity: 0.15
});

const makeTextStyle = (fontSize, color, bold = false) => ({
  fontSize, color, bold,
  fontFace: BODY_FONT,
  align: 'left',
  valign: 'top',
});

// ✅ Fresh object each time
slide.addShape(pres.shapes.RECTANGLE, { ..., shadow: makeCardShadow() });
slide.addShape(pres.shapes.RECTANGLE, { ..., shadow: makeCardShadow() });
```
