---
name: pure-style-slides
description: "Create designer-quality presentation slides that look like a creative agency built them. Triggers on any mention of slides, presentation, deck, pitch, pptx, or requests like make me a presentation. Produces magazine-quality .pptx files with varied layouts (20 patterns), bold typography, data visualizations, icon-driven design (1,392 built-in vector icons), professional color palettes (6 themes), and comprehensive speaker notes. Detects presentation type, researches topic, proposes a Deck Blueprint, then generates polished slides. Also triggers when user wants to redesign or improve an existing .pptx file. Supports Arabic and RTL content with automatic layout mirroring. Does NOT trigger for pptx repair, text extraction without creating new slides, or simple pptx-to-PDF conversion."
---

# Pure-Style Slides Skill

Create presentations that look like a creative agency built them. Every slide is a designed artifact with visual variety, bold typography, color confidence, breathing room, data as design, icon-driven storytelling, comprehensive speaker notes, and RTL awareness.

---

## File Loading Protocol (TOKEN OPTIMIZATION)

**DO NOT read all reference files upfront.** Load files in phases as needed:

### Phase 1 — Always read (this file only)
- `SKILL.md` — you're reading it now. Contains workflow, theme menu, presentation types.

### Phase 2 — Read AFTER user selects theme
- **Theme 1 (Pure Minimal)**: read `references/pure-minimal.md` + `references/layouts-compact.md`
- **Themes 2–5 (Magazine)**: read `references/themes.md` + `references/layouts-compact.md`
- **Theme 6 (Orange)**: read `references/orange.md` + `references/layouts-compact.md`
- **If RTL content detected**: also read `references/rtl-guide.md`

### Phase 3 — Read ONLY when generating code
- `references/icon-keywords.json` — icon lookup (read when picking icons for slides)
- `references/charts.md` — chart recipes (read ONLY if deck has data/chart slides)

### Phase 4 — Read ONLY during QA
- `references/qa-helpers.md` — QA checks + handout generation (read after .pptx is built)

**Never read**: `references/icons.md` (the keyword index covers 95% of lookups; only read icons.md if you can't find an icon via keywords)

---

## Workflow

### Phase 0: Detect Mode

**New deck** (default) → Phase 1.
**Redesign**: user uploaded `.pptx` → extract with `python -m markitdown uploaded.pptx > extracted.md`, analyze, present Redesign Blueprint marking each slide `[KEEP]`/`[REDESIGN]`/`[NEW]`/`[REMOVE]`, wait for approval, then Phase 3.

### Phase 1: Understand & Research

Extract from user request (ask if missing):

| Input | What |
|-------|------|
| Objective | Informative / persuasive / celebratory |
| Audience | Seniority, domain, cultural context |
| Duration | Total minutes → ~2-3 min per slide |
| Assets | Data sources, brand docs, uploads |
| Density | Executive (sparse) vs deep-dive (detailed) |
| Design mode | Magazine (default) or Pure Minimal |
| Theme | Ask user to select from Theme Menu below |
| Language | RTL? → activate RTL mode |
| Variant | Pitch decks: Elevator (5) / Standard (10-15) / Deep Dive (18-25) |

#### Theme Menu (ALWAYS ask user to choose)

| # | Theme | Mood | Best For |
|---|-------|------|---------|
| 1 | Pure Minimal | Swiss, ultra-clean | C-suite, board, investor pitch, keynotes |
| 2 | Midnight Executive | Authoritative, premium | Finance, consulting, corporate |
| 3 | Coral Energy | Warm, dynamic | Marketing, events, consumer brands, startups |
| 4 | Ocean Gradient | Trustworthy, deep | Technology, SaaS, enterprise, healthcare |
| 5 | Slate & Gold | Premium, luxurious | Investment, real estate, awards, luxury |
| 6 | Orange | Bold, warm, growth | Earnings calls, investor IR, MENA corporate, food/delivery |

**After selection → read the appropriate theme file(s) per the File Loading Protocol above.**

#### Presentation Types

| Type | Arc | Slides |
|------|-----|--------|
| Pitch Deck | Problem→Solution→Market→Traction→Team→Ask | 10-15 |
| Quarterly Review | Metrics→Wins→Challenges→Roadmap→Financials | 12-20 |
| Training | Objective→Concept→Example→Exercise→Assessment | 15-30 |
| Status Update | Current→Blockers→Next Steps→Resources | 5-10 |
| Thought Leadership | Hook→Insight→Data→Implication→CTA | 8-12 |
| Product Overview | Problem→Solution→Features→Pricing→CTA | 8-12 |
| Conference Talk | Hook→Context→3 Points→Demo/Story→Takeaway | 15-25 |

#### Research

Use web search to gather real data. Target 3-8 high-quality sources. Build a Source Registry:

```
[S1] Source Name — "key finding" (URL, date)
[S2] Source Name — "key finding" (URL, date)
```

Add source footnotes on data slides. Include a References slide if 3+ sources used.

### Phase 2: Deck Blueprint

Present the outline for approval:

```
### Slide [N]: [Title]
**Layout**: [layout name from layouts-compact.md]
**Visual**: [what appears on screen]
**Data**: [S1] reference if applicable
**Notes**: [2-3 sentence speaker script]
```

Rules: no two consecutive slides share the same layout. Minimum 4 different layouts per 10 slides. Wait for user approval before generating.

### Phase 3: Generate

Read the theme + layout files per the File Loading Protocol. If charts are needed, read `references/charts.md`. Generate the deck using PptxGenJS.

**Layout sequencer** — track used layouts, never repeat consecutively:
```javascript
let lastLayout = null;
function pickLayout(preferred) {
  if (preferred === lastLayout) { /* shift to alternate */ }
  lastLayout = preferred;
  return preferred;
}
```

**Speaker notes**: every slide gets 2-4 sentence presenter notes via `slide.addNotes(text)`.

### Phase 4: QA

Read `references/qa-helpers.md`. Run automated + visual QA. Fix issues, re-verify.

### Phase 5: Deliver

Copy .pptx to `/mnt/user-data/outputs/`. Offer Speaker Cheat Sheet or Handout PDF.

---

## Quick Reference

| What | Where |
|------|-------|
| PptxGenJS API | `/mnt/skills/public/pptx/pptxgenjs.md` |
| Layouts (compact, all 20) | `references/layouts-compact.md` |
| Themes 2–5 (Magazine palettes) | `references/themes.md` |
| Orange theme (full IR system) | `references/orange.md` |
| Pure Minimal style overrides | `references/pure-minimal.md` |
| Chart recipes (bar/line/pie/etc) | `references/charts.md` |
| Icon keyword lookup | `references/icon-keywords.json` |
| Icon full catalog (rarely needed) | `references/icons.md` |
| RTL / Arabic guide | `references/rtl-guide.md` |
| QA checks + handout generation | `references/qa-helpers.md` |
| Icon sprites + manifest | `icons/{category}_sprite.png` + `icons/manifest.json` |

## Dependencies

- `npm install -g pptxgenjs` — slide generation
- `npm install -g react-icons react react-dom sharp` — icons (optional fallback)
- `pip install "markitdown[pptx]"` — text extraction for QA
- LibreOffice + Poppler — PDF conversion for visual QA

## Built-in Icon Library (1,392 icons)

| Category | Count | What's In It |
|----------|-------|-------------|
| `general` | 137 | Chat, devices, media, connectivity, code, settings |
| `arrow` | 58 | Directional, navigation, chevrons |
| `business` | 138 | Charts, mail, documents, phone, globe, finance |
| `interface` | 255 | Checkmarks, toggles, search, lock, home, cloud |
| `buildings` | 38 | Houses, offices, churches, hotels, airports |
| `user` | 118 | Emoji faces, hand gestures, gender symbols |
| `other` | 246 | Weather, animals, vehicles, food & drink, nature |
| `brand` | 402 | Company logos (only when discussing those brands) |

Use `references/icon-keywords.json` for fast topic→icon lookup. Load with:
```javascript
const keywords = JSON.parse(fs.readFileSync('references/icon-keywords.json','utf8'));
function findIcon(topic) {
  const entry = keywords[topic] || Object.entries(keywords).find(([k]) => k.includes(topic))?.[1];
  return entry ? entry[0] : null; // [category, index, name]
}
```
