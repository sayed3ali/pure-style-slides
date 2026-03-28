<div align="center">

<!-- HERO -->
<br>

```
 ┌─────────────────────────────────────────────────────────────┐
 │                                                             │
 │    ██████╗ ██╗   ██╗██████╗ ███████╗                       │
 │    ██╔══██╗██║   ██║██╔══██╗██╔════╝                       │
 │    ██████╔╝██║   ██║██████╔╝█████╗                         │
 │    ██╔═══╝ ██║   ██║██╔══██╗██╔══╝                         │
 │    ██║     ╚██████╔╝██║  ██║███████╗                       │
 │    ╚═╝      ╚═════╝ ╚═╝  ╚═╝╚══════╝                       │
 │              S T Y L E   S L I D E S                        │
 │                                                             │
 │    Presentations that look like a creative agency built them │
 │                                                             │
 └─────────────────────────────────────────────────────────────┘
```

<br>

**A Claude skill that generates magazine-quality `.pptx` presentations**
**with bold design, real data, and zero design skills required.**

<br>

[`SKILL.md`](./SKILL.md) · [`Layouts`](./references/layouts.md) · [`Themes`](./references/themes.md) · [`Icons`](./references/icons.md) · [`Data Viz`](./references/data-visualization.md)

<br>

---

<br>

</div>

## The Problem

AI-generated slides are **instantly recognizable** — and not in a good way. They share the same sins: repetitive layouts, timid colors, bullet-point overload, missing speaker notes, and that unmistakable "default template" energy.

**Pure Style Slides** exists to fix that. It's a skill for [Claude](https://claude.ai) that treats every slide as a designed artifact, not a text container.

<br>

## What Makes It Different

```
┌──────────────────────────────────────────────────────────────────┐
│                                                                  │
│   GENERIC AI SLIDES              PURE STYLE SLIDES               │
│                                                                  │
│   • Same layout, 15 times        20 layout patterns, sequenced  │
│   • Default blue palette          5 curated color systems        │
│   • Bullet points everywhere      Stat callouts, cards, charts   │
│   • No visual elements            1,392 built-in vector icons    │
│   • "Add speaker notes here"      Full presenter scripts         │
│   • LTR only                      RTL-aware (Arabic, Hebrew)     │
│   • No sources cited               Academic + industry research   │
│   • One size fits all              7 presentation archetypes     │
│                                                                  │
└──────────────────────────────────────────────────────────────────┘
```

<br>

## Architecture

The skill runs a **6-phase pipeline** — each phase builds on the last:

```
                ┌─────────────┐
                │  User Input │
                └──────┬──────┘
                       │
                       ▼
        ┌──────────────────────────────┐
   0    │     DETECT MODE              │   New deck? Redesign?
        └──────────────┬───────────────┘
                       │
                       ▼
        ┌──────────────────────────────┐
   1    │  UNDERSTAND & RESEARCH       │   Parse request, detect type,
        │                              │   gather sources, build
        │  Academic · Industry · Gov   │   Source Registry [S1] [S2]...
        │  News · Company · Technical  │
        └──────────────┬───────────────┘
                       │
                       ▼
        ┌──────────────────────────────┐
   2    │  DECK BLUEPRINT              │   Slide-by-slide outline with
        │                              │   layouts, visuals, sources,
        │  "Here's the plan —          │   speaker notes
        │   approve before I build"    │
        └──────────────┬───────────────┘
                       │  ← user approval
                       ▼
        ┌──────────────────────────────┐
   3    │  DESIGN & GENERATE           │   PptxGenJS rendering with
        │                              │   theme colors, icon sprites,
        │  Layouts · Icons · Charts    │   charts, footnotes
        │  Footnotes · Speaker Notes   │
        └──────────────┬───────────────┘
                       │
                       ▼
        ┌──────────────────────────────┐
   4    │  QA                          │   Automated checks + visual
        │                              │   inspection (PDF → images)
        │  Contrast · Overflow ·       │
        │  Notes · Count · Render      │
        └──────────────┬───────────────┘
                       │
                       ▼
        ┌──────────────────────────────┐
   5    │  DELIVER                     │   .pptx + optional handout PDF
        │                              │   + speaker cheat sheet
        └──────────────────────────────┘
```

<br>

## Features

### 🎨 5 Curated Themes

| Theme | Mood | Best For |
|-------|------|----------|
| **Pure Minimal** | Swiss, ultra-clean | C-suite, board, investor pitch, keynotes |
| **Midnight Executive** | Authoritative, premium | Finance, consulting, corporate |
| **Coral Energy** | Warm, dynamic | Marketing, events, consumer brands, startups |
| **Ocean Gradient** | Trustworthy, deep | Technology, SaaS, enterprise, healthcare |
| **Slate & Gold** | Premium, luxurious | Investment, real estate, awards, luxury |

### 📐 20 Layout Patterns

No two consecutive slides share the same layout. The built-in sequencer enforces variety by alternating visual weight (heavy → medium → light), preventing repeats, and ensuring the "sandwich" structure (dark bookends, lighter content).

<details>
<summary><b>View all 20 layouts</b></summary>

| # | Layout | Weight | Best For |
|---|--------|--------|----------|
| 1 | Title Hero | Heavy | Opening slide |
| 2 | Section Divider | Medium | Topic transitions |
| 3 | Two-Column Split | Medium | Content, comparisons |
| 4 | Icon Row | Medium | Feature lists, processes |
| 5 | Stat Callout | Heavy | Key metrics |
| 6 | Card Grid | Medium | Multi-point content |
| 7 | Timeline / Process | Medium | Sequential content |
| 8 | Chart + Insight | Medium | Data with narrative |
| 9 | Quote | Light | Testimonials, key quotes |
| 10 | Image + Overlay | Heavy | Visual storytelling |
| 11 | Table Slide | Light | Structured data, comparisons |
| 12 | Closing / CTA | Heavy | Final slide |
| 13 | Agenda / Overview | Light | Table of contents |
| 14 | Before / After | Medium | Transformations |
| 15 | Multi-Period Table | Medium | Financial data |
| 16 | KPI Card Row | Heavy | Dashboard metrics |
| 17 | Strategy Dashboard | Heavy | Strategic overviews |
| 18 | Product Feature Showcase | Medium | Product demos |
| 19 | Bold Section Divider | Heavy | Major transitions |
| 20 | Period-over-Period Bars | Medium | Growth comparisons |

</details>

### 🔬 Research-Backed Content

The skill doesn't just arrange text — it **researches your topic** and cites real sources.

```
SOURCE TYPES SUPPORTED
──────────────────────────────────────
  Academic Papers    → Google Scholar, arXiv, PubMed
  Industry Reports   → McKinsey, Gartner, Statista, Deloitte
  Government Data    → .gov, WHO, World Bank, OECD
  News & Journalism  → Reuters, Bloomberg, AP
  Company Sources    → SEC filings, press releases, investor decks
  Technical Docs     → GitHub, RFCs, official documentation
  User Uploads       → Your PDFs, docs, spreadsheets
```

Sources are tracked in a **Source Registry** (`[S1]`, `[S2]`...) that flows through to on-slide footnotes, speaker notes, and a dedicated References slide.

### 🎯 1,392 Built-in Vector Icons

Eight categories of professional icons in dark + white variants, extracted from sprite sheets at runtime:

| Category | Count | What's Inside |
|----------|-------|---------------|
| General | 137 | Chat, devices, media, connectivity, code |
| Arrow | 58 | Directional, navigation, chevrons |
| Business | 138 | Charts, mail, documents, finance |
| Interface | 255 | Checkmarks, toggles, search, lock, cloud |
| Buildings | 38 | Houses, offices, churches, airports |
| User | 118 | Emoji faces, hand gestures |
| Other | 246 | Weather, animals, vehicles, food, nature |
| Brand | 402 | Company logos |

Fast keyword lookup: `findAndGetIcon('revenue')` → best matching icon as base64 PNG.

### 📊 Data Visualization

Seven data-specific layout patterns (D1–D7) with theme-matched chart styling via `makeChartStyle()`. Chart titles state insights ("Revenue Grew 58%"), not labels ("Quarterly Revenue").

### 🌍 RTL Support

Full Arabic, Hebrew, Persian, and Urdu support with automatic layout mirroring, RTL-safe font selection (Tahoma + Arial), and bidirectional content handling.

### 🎤 Speaker Notes

Every slide gets a full presenter script — not "add notes here" placeholders. Notes include talking points, transition hooks, source details, and Q&A prep.

<br>

## Project Structure

```
pure-style-slides/
├── SKILL.md                          # Main skill definition (618 lines)
├── references/
│   ├── layouts.md                    # 20 slide layout patterns
│   ├── themes.md                     # 5 color palettes + font pairings
│   ├── pure-minimal.md               # Swiss-style design overrides
│   ├── data-visualization.md         # Chart recipes, D1–D7 layouts
│   ├── code-helpers.md               # Layout sequencer, QA, RTL, icons
│   ├── icons.md                      # Full icon catalog + usage guide
│   ├── icon-keywords.json            # Fast topic → icon keyword index
│   └── rtl-guide.md                  # Arabic/Hebrew layout mirroring
├── icons/
│   ├── manifest.json                 # Icon metadata (positions, sizes)
│   ├── {category}_sprite.png         # Dark icon sprite sheets (×8)
│   └── {category}_white_sprite.png   # White icon sprite sheets (×8)
└── README.md
```

<br>

## Usage

This is a **Claude skill** — it runs inside [Claude.ai](https://claude.ai) projects with computer use enabled.

### Quick Start

1. Create a new Claude project
2. Upload the `pure-style-slides/` folder as a skill
3. Ask Claude to make a presentation:

```
"Create a pitch deck about our AI-powered logistics platform.
 We're targeting Series A investors."
```

Claude will:
- Detect this as a **Pitch Deck** → suggest the Problem → Solution → Market → Traction → Team → Ask arc
- Ask you to **pick a theme** from the 5 options
- **Research** the logistics/AI market for real data points
- Present a **Deck Blueprint** for your approval
- Generate a polished `.pptx` with varied layouts, icons, charts, and full speaker notes
- Run **QA checks** and deliver the final file

### Supported Presentation Types

| Type | Typical Length |
|------|---------------|
| Pitch Deck | 10–15 slides (or 3–5 elevator variant) |
| Quarterly Review | 12–20 slides |
| Training / Educational | 15–30 slides |
| Status Update | 5–10 slides |
| Thought Leadership | 8–12 slides |
| Product Overview | 8–12 slides |
| Conference Talk | 15–25 slides |

<br>

## Dependencies

| Package | Purpose |
|---------|---------|
| `pptxgenjs` | Slide generation |
| `sharp` | Icon sprite extraction |
| `markitdown` | Text extraction for QA |
| LibreOffice | PDF conversion for visual QA |
| Poppler (`pdftoppm`) | PDF → image for visual inspection |

<br>

## Design Philosophy

> *The difference between forgettable slides and memorable ones is **design intelligence**: understanding that a presentation is a visual story, not a text dump.*

This skill is opinionated. It believes:

- **Variety is non-negotiable** — layout repetition kills engagement
- **Color requires commitment** — pick a palette and own it
- **Data belongs on slides** — real stats beat generic claims
- **Whitespace is a feature** — breathing room > bullet density
- **Speaker notes are first-class** — the deck is incomplete without them
- **Sources build trust** — cited data beats "according to industry reports"

<br>

## Contributing

This skill is actively maintained. To contribute:

1. Fork the repository
2. Make changes to `SKILL.md` or the reference files
3. Test by uploading to a Claude project and generating presentations
4. Submit a PR with before/after examples

<br>

## License

MIT

<br>

---

<div align="center">

**Built for [Claude](https://claude.ai) · Powered by [PptxGenJS](https://gitbrent.github.io/PptxGenJS/)**

*Stop making slides. Start designing decks.*

</div>
