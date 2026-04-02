# pure-style-slides

**PptxGenJS presentation skill for Claude — 20 layouts, 6 themes (incl. Orange IR theme), 1,392 vector icons, token-optimized layered loading (63–76% reduction)**

Create designer-quality PowerPoint presentations that look like a creative agency built them. Every deck has visual variety, bold typography, color confidence, breathing room, data-as-design, icon-driven storytelling, and comprehensive speaker notes.

---

## Features

- **20 layout patterns** — Title Hero, Section Divider, Two-Column Split, Icon Row, Stat Callout, Card Grid, Timeline, Chart + Insight, Quote, Image + Overlay, Table, Closing/CTA, Agenda, Before/After, Multi-Period Table, KPI Card Row, Strategy Dashboard, Product Feature, Bold Section Divider, PoP Bar Comparison
- **6 themes** — Pure Minimal (Swiss), Midnight Executive (corporate), Coral Energy (marketing), Ocean Gradient (tech), Slate & Gold (luxury), **Orange** (investor IR / earnings)
- **1,392 built-in vector icons** — 8 categories (general, arrow, business, interface, buildings, user, other, brand) in dark + white variants
- **Token-optimized** — layered file loading reads only what's needed per phase (63–76% reduction vs monolithic approach)
- **RTL support** — automatic Arabic/Hebrew/Persian layout mirroring
- **Data visualization** — bar, line, pie, doughnut, area, scatter charts with theme-matched styling
- **Speaker notes** — comprehensive presenter scripts on every slide
- **QA automation** — slide count, notes presence, text overflow, contrast checks

---

## File Map

```
pure-style-slides/
├── SKILL.md                          # Main skill — workflow, theme menu, loading protocol
├── references/
│   ├── layouts-compact.md            # 20 layout patterns (compact, code-focused)
│   ├── themes.md                     # Magazine themes 1–5 + Orange color specs
│   ├── orange.md                     # Orange theme full design system (IR/earnings)
│   ├── pure-minimal.md               # Pure Minimal style overrides (Swiss design)
│   ├── charts.md                     # Chart recipes — makeChartStyle + all chart types
│   ├── qa-helpers.md                 # QA checks, sequencer, RTL helpers, handout gen
│   ├── icons.md                      # Icon catalog documentation
│   ├── icon-keywords.json            # Fast topic→icon keyword lookup
│   └── rtl-guide.md                  # Arabic/Hebrew/RTL layout guide
└── icons/
    ├── manifest.json                 # Icon index (category, position, name)
    ├── {category}_sprite.png         # Dark icon sprite sheets
    └── {category}_white_sprite.png   # White icon sprite sheets
```

---

## Token-Optimized Loading Protocol

The skill uses **layered loading** — files are read per phase, not all upfront:

| Phase | Files Read | Tokens | When |
|-------|-----------|--------|------|
| 1 — Parse | `SKILL.md` only | ~2,000 | Always |
| 2 — Theme | `layouts-compact.md` + theme file | ~3,400–7,900 | After user picks theme |
| 3 — Generate | `charts.md` + `icon-keywords.json` | ~4,200 | Only if deck has charts |
| 4 — QA | `qa-helpers.md` | ~1,550 | After .pptx is built |

### Savings vs previous monolithic approach

| Scenario | Before | After | Reduction |
|----------|--------|-------|-----------|
| Magazine (no charts) | 41K | 10K | **76%** |
| Magazine (with charts) | 41K | 11K | **73%** |
| Orange theme | 48K | 18K | **63%** |
| Pure Minimal | 46K | 16K | **65%** |

---

## Themes

| # | Theme | Mood | Best For |
|---|-------|------|---------|
| 1 | Pure Minimal | Swiss, ultra-clean | C-suite, board, investor pitch, keynotes |
| 2 | Midnight Executive | Authoritative, premium | Finance, consulting, corporate |
| 3 | Coral Energy | Warm, dynamic | Marketing, events, consumer brands, startups |
| 4 | Ocean Gradient | Trustworthy, deep | Technology, SaaS, enterprise, healthcare |
| 5 | Slate & Gold | Premium, luxurious | Investment, real estate, awards, luxury |
| 6 | Orange | Bold, warm, growth | Earnings calls, investor IR, MENA corporate |

### Orange Theme

Extracted from talabat Holding plc investor presentations (Capital Markets Day, Q4/FY24 Earnings, Investor Presentation). Features warm cream backgrounds (F5EDE3), vivid orange accents (FF5722), chocolate brown text (3D1F00), and leaf green growth indicators (7AB648). Includes IR-specific patterns: KPI card rows, dual chart dashboards, guidance tracker tables, growth pill badges, and stacked revenue bar charts.

---

## Dependencies

```bash
npm install -g pptxgenjs
npm install -g react-icons react react-dom sharp  # optional icon fallback
pip install "markitdown[pptx]"                     # QA text extraction
# LibreOffice + Poppler for PDF visual QA
```

---

## Usage

This is a **Claude skill** — install it in Claude's skill directory and it triggers automatically when you ask for slides, presentations, decks, or pitches.

```
"Create a 10-slide investor presentation about our Q3 results using the Orange theme"
"Make me a pitch deck for a SaaS startup"
"Redesign this uploaded .pptx with the Pure Minimal theme"
```

---

## Author

**Sayed Ali** — Financial Analyst & Python Instructor

---

## License

See [LICENSE](LICENSE) for details.
