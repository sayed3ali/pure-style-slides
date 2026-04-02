---
name: pure-style-slides
description: "Create designer-quality presentation slides that look like a creative agency built them. Triggers on any mention of slides, presentation, deck, pitch, pptx, or requests like make me a presentation. Produces magazine-quality .pptx files with varied layouts (20 patterns), bold typography, data visualizations, icon-driven design (1,392 built-in vector icons), professional color palettes (6 themes), and comprehensive speaker notes. Detects presentation type, researches topic, proposes a Deck Blueprint, then generates polished slides. Also triggers when user wants to redesign or improve an existing .pptx file. Supports Arabic and RTL content with automatic layout mirroring. Does NOT trigger for pptx repair, text extraction without creating new slides, or simple pptx-to-PDF conversion."
---

# Kimi-Style Slides Skill

Create presentations that look like a creative agency built them. Every deck should feel intentional, polished, and visually striking — never generic or robotic.

## Philosophy

The difference between forgettable slides and memorable ones is **design intelligence**: understanding that a presentation is a visual story, not a text dump. This skill treats every slide as a designed artifact with:

- **Visual variety** — no two consecutive slides share the same layout
- **Bold typography** — large stat callouts, contrasting font weights, intentional hierarchy
- **Color confidence** — a committed palette that reflects the topic, not default blue
- **Breathing room** — generous whitespace, not cramped bullet lists
- **Data as design** — charts, stat cards, and infographics woven into the narrative
- **Icon-driven storytelling** — 1,392 built-in vector icons (dark + white variants) across 8 categories, plus react-icons fallback for anything not covered
- **Speaker notes** — comprehensive presenter scripts for every slide, not afterthoughts
- **RTL-aware** — automatic Arabic/Hebrew/Persian layout mirroring and font selection

---

## Workflow

### Phase 0: Detect Mode — New Deck or Redesign?

Before anything else, determine whether this is a **new deck** or a **redesign of an existing deck**.

#### New Deck (default)
The user wants a presentation created from scratch or from source material (notes, document, topic). Proceed to Phase 1.

#### Redesign / Iterate on Existing Deck
The user uploaded a `.pptx` file and wants it improved, polished, redesigned, or expanded. Follow this workflow:

1. **Extract content** from the uploaded `.pptx` using `markitdown`:
   ```bash
   python -m markitdown uploaded.pptx > extracted.md
   ```
2. **Analyze the existing deck**: count slides, identify structure, assess visual quality, note what works and what doesn't.
3. **Present a Redesign Blueprint** (same format as Phase 2, but mark each slide as `[KEEP]`, `[REDESIGN]`, `[NEW]`, or `[REMOVE]`):
   ```
   ### Slide [N]: [Title] [REDESIGN]
   **Original issue**: Text-heavy, no visual hierarchy, default template
   **Redesign direction**: Convert to Icon Row layout, add stat callouts
   ...
   ```
4. **Wait for approval**, then proceed to Phase 3 with the approved redesign plan.
5. The redesigned deck is generated fresh (not edited in-place) using the extracted content and the user's chosen theme.

---

### Phase 1: Understand & Research

Before touching PptxGenJS, parse the user's request and understand what they need.

#### User Input Parsing

Extract these from the user's request (ask if not provided):

1. **Objective**: Informative, persuasive, or celebratory?
2. **Audience**: Seniority level, domain expertise, cultural context
3. **Time constraint**: Total presentation duration → calculate slides at ~2-3 min per slide
4. **Existing assets**: Data sources, brand guidelines, uploaded documents to reference
5. **Content density**: Executive summary (sparse) vs. deep-dive (detailed)
6. **Design mode**: Magazine or Pure Minimal? (detect or ask — see below)
7. **Theme**: Ask the user to select a theme from the Theme Menu (see below) — do NOT auto-select
8. **Language / direction**: Detect if content is RTL (Arabic, Hebrew, Persian, Urdu). If so, activate RTL mode — read `references/rtl-guide.md` during Phase 3.
9. **Deck variant**: For pitch decks, ask if the user wants an Elevator version (5 slides), a Full version (10-15 slides), or both.
10. **Research sources**: What types of sources should back up the content? Detect from context or ask. See the Research Source Types table below.

#### Design Mode Selection

This skill supports two design philosophies. Detect which one fits, or ask:

| Mode | Philosophy | When To Use |
|------|-----------|-------------|
| **Magazine** (default) | Bold colors, icon-driven, varied layouts, cards, charts, visual density | Most presentations — training, product overviews, quarterly reviews, conferences |
| **Pure Minimal** | Swiss typography, extreme whitespace, flat color, one accent, "less but better" | C-suite/board, investor pitch, thought leadership, design-literate audiences, keynotes |

**Auto-detect signals for Pure Minimal**: user says "minimal", "clean", "Swiss", "Dieter Rams", "elegant", "simple", "less is more", "Apple-style", "no clutter", or the audience is investors/board/executives.

When **Magazine** mode is selected: read `references/layouts.md` and `references/themes.md`.
When **Pure Minimal** mode is selected: read `references/layouts.md` (same shared layouts) AND `references/pure-minimal.md` for style overrides — Pure Minimal applies its own color system, typography, and styling rules to the shared layout patterns.

#### Theme Selection (Mandatory)

**Always present the full theme list to the user and ask them to pick one.** Do not auto-select a theme silently. If the user already specified a theme by name in their request, confirm it. If they haven't, show the list below and ask them to choose.

Use the `ask_user_input_v0` tool to present the theme choices as a single-select widget when available, otherwise list them in text.

**Available Themes:**

| #  | Theme              | Mood                       | Best For                                      |
|----|--------------------|----------------------------|-----------------------------------------------|
| 1  | Pure Minimal       | Swiss, ultra-clean         | C-suite, board, investor pitch, keynotes      |
| 2  | Midnight Executive | Authoritative, premium     | Finance, consulting, corporate                |
| 3  | Coral Energy       | Warm, dynamic              | Marketing, events, consumer brands, startups  |
| 4  | Ocean Gradient     | Trustworthy, deep          | Technology, SaaS, enterprise, healthcare      |
| 5  | Slate & Gold       | Premium, luxurious         | Investment, real estate, awards, luxury       |
| 6  | Orange             | Bold, warm, growth         | Earnings calls, investor IR, MENA corporate, food/delivery |

After the user selects a theme:
- **Theme 1 (Pure Minimal)**: Read `references/pure-minimal.md` for the style overrides, color system, typography, card system, and forbidden elements. Pure Minimal uses the SAME 20 layouts from `references/layouts.md` but renders them with its own minimalist styling rules (sharp corners, muted colors, single accent, Swiss typography). Also read `references/layouts.md` for the layout patterns.
- **Themes 2–5**: Read the full theme details from `references/themes.md` and apply the selected theme's colors and fonts throughout the deck. Use the layout patterns from `references/layouts.md`.
- **Theme 6 (Orange)**: Read `references/orange.md` for the complete design system including layout overrides, card/badge system, data visualization guidelines (stacked bars, growth pills, guidance tables), and narrative flow. Also read `references/themes.md` for the color summary and `references/layouts.md` for the shared layout patterns. Orange applies its own warm-cream backgrounds, orange section dividers, chocolate text, and IR-optimized chart styling to the shared layouts.

#### Detect Presentation Type

Identify the presentation type from context, or ask. Each type has a proven narrative arc:

| Type | Narrative Arc | Typical Length |
|------|--------------|----------------|
| **Pitch Deck** | Problem → Solution → Market → Traction → Team → Ask | 10-15 slides |
| **Quarterly Review** | Metrics → Wins → Challenges → Roadmap → Financials | 12-20 slides |
| **Training / Educational** | Learning Objective → Concept → Example → Exercise → Assessment | 15-30 slides |
| **Status Update** | Current State → Blockers → Next Steps → Resource Needs | 5-10 slides |
| **Thought Leadership** | Hook → Insight → Data → Implication → CTA | 8-12 slides |
| **Product Overview** | Problem → Solution → Features → Pricing → CTA | 8-12 slides |
| **Conference Talk** | Hook → Context → 3 Key Points → Demo/Story → Takeaway | 15-25 slides |

#### Deck Variants (Pitch Decks)

For pitch decks specifically, offer to generate multiple variants from the same content:

| Variant | Slides | Use Case |
|---------|--------|----------|
| **Elevator** | 3-5 | Quick intro, email attachment, networking |
| **Standard** (default) | 10-15 | Investor meetings, full pitch |
| **Deep Dive** | 18-25 | Due diligence follow-up, detailed appendix |

When the user requests both Elevator and Full variants:
1. Build the Full Blueprint first (Phase 2)
2. After approval, derive the Elevator variant by collapsing the Full Blueprint — combine slides, prioritize the highest-impact content, keep only the single most important stat per section
3. Generate both `.pptx` files from the same theme and visual system

#### Research

Presentations backed by real, cited data are dramatically more credible than generic ones. This phase gathers high-quality sources and structures them for slide integration.

##### Research Source Types

Detect which source types are appropriate from the topic and audience, or ask the user. Multiple types can be combined.

| Source Type | Search Strategy | Best For | Credibility |
|-------------|----------------|----------|-------------|
| **Academic Papers** | Search Google Scholar via `site:scholar.google.com`, arXiv via `site:arxiv.org`, or PubMed via `site:pubmed.ncbi.nlm.nih.gov` | Scientific claims, medical topics, technical deep-dives, education | Very High |
| **Industry Reports** | Search for `[topic] report 2026 filetype:pdf`, or target McKinsey, Gartner, Statista, Deloitte, BCG, PwC | Market sizing, trends, forecasts, strategy decks | High |
| **Government / Institutional** | Target `.gov`, `.edu`, WHO, World Bank, OECD, UN, central banks | Policy, regulation, public health, economics, demographics | Very High |
| **News & Journalism** | Search Reuters, Bloomberg, AP, major newspapers | Current events, earnings, announcements, executive quotes | Medium-High |
| **Company Sources** | Target company blogs, press releases, SEC filings (`site:sec.gov`), investor decks | Product launches, financials, competitive analysis | High (primary) |
| **Technical Docs** | GitHub, official documentation sites, RFCs, standards bodies | Engineering, architecture, developer topics | High |
| **User-Uploaded Material** | Extract from uploaded PDFs, docs, spreadsheets, notes | Any — the user's own data is always primary | Primary |

##### Auto-Detect Source Needs

Match the presentation type to default research sources:

| Presentation Type | Default Sources |
|-------------------|----------------|
| Pitch Deck | Industry Reports + Company Sources + News |
| Quarterly Review | Company Sources (primary) + Industry Reports (benchmarks) |
| Training / Educational | Academic Papers + Technical Docs + Government |
| Thought Leadership | Academic Papers + Industry Reports + News |
| Product Overview | Company Sources + Technical Docs + News |
| Conference Talk | Academic Papers + Industry Reports + News |
| Status Update | Company Sources (usually no external research needed) |

##### Research Workflow

1. **Determine source types** — use the auto-detect table or ask the user
2. **Run targeted searches** — use `web_search` with source-specific queries:
   ```
   # Academic papers
   web_search("[topic] research paper 2025 2026 site:scholar.google.com")
   web_search("[topic] arxiv preprint")
   
   # Industry reports
   web_search("[topic] market report 2026")
   web_search("[topic] industry analysis Gartner OR McKinsey OR Statista")
   
   # Government / institutional data
   web_search("[topic] statistics site:who.int OR site:worldbank.org")
   
   # Company sources
   web_search("[company] investor presentation OR earnings OR press release 2026")
   ```
3. **Use `web_fetch`** to retrieve full content from the most promising results — search snippets are often too thin for accurate data extraction
4. **Extract structured data** — pull out: key statistics, named findings, quotable conclusions, author/org names, publication dates, DOIs or URLs
5. **Build a Source Registry** — a structured list of all sources gathered, used to generate citations throughout the deck

##### Source Registry Format

During research, build and maintain this registry. It feeds into the Blueprint, slide footnotes, speaker notes, and the References slide.

```
SOURCE_REGISTRY:
[S1] Author/Org (Year). "Title." Publication/Publisher. URL or DOI.
     Key data: [extracted stat or finding]
[S2] Author/Org (Year). "Title." Publication/Publisher. URL or DOI.
     Key data: [extracted stat or finding]
...
```

Example:
```
SOURCE_REGISTRY:
[S1] McKinsey Global Institute (2026). "The State of AI: 2026 Annual Review." McKinsey & Company.
     Key data: 72% of enterprises now deploy AI in at least one function (up from 55% in 2024)
[S2] Vaswani et al. (2017). "Attention Is All You Need." NeurIPS.
     Key data: Transformer architecture foundational to all modern LLMs
[S3] U.S. Bureau of Labor Statistics (2026). "Occupational Outlook Handbook: Data Scientists."
     Key data: 36% projected job growth 2024-2034, much faster than average
```

##### Source Quality Rules

1. **Recency** — prefer sources from the last 2 years; flag anything older than 3 years as potentially outdated
2. **Primary over secondary** — original research > news summaries > blog posts > social media
3. **Named authors over anonymous** — prefer attributed sources; avoid "according to reports" without specifics
4. **Cross-verify key claims** — if a statistic is central to a slide, try to confirm it from at least 2 sources
5. **Note conflicts** — if sources disagree, flag the discrepancy in speaker notes so the presenter is prepared
6. **Respect copyright** — never reproduce large text blocks; paraphrase findings in your own words and cite the source

##### Integrating Sources into the Deck

Sources flow into three places:

1. **On-slide footnotes** — small (9-10pt) source attribution at the bottom of data slides:
   `Source: McKinsey Global Institute, 2026` or `Source: [S1]`
2. **Speaker notes** — full citation details and additional context from the source, including caveats and methodology notes
3. **References slide** — a dedicated slide at the end (before Closing or as Appendix) listing all cited sources in a clean, numbered format

When a slide uses data from a source, tag it in the Blueprint with `[S1]`, `[S2]`, etc. referencing the Source Registry. This ensures traceability from research → blueprint → final slides.

If the user uploaded a document, PDF, or notes, extract the key content and structure it for slides. User-uploaded material takes precedence as the primary source — external research supplements it.

---

### Phase 2: Deck Blueprint (Always)

Never jump straight to slide generation. Always propose a **Deck Blueprint** first.

#### Blueprint Format

For each slide, provide:

```
### Slide [N]: [Slide Title]
**Type**: [Title / Content / Two-Column / Comparison / Quote / Data / Closing / Section Divider / References]
**Layout**: [layout name from the layout library]
**Visual Direction**: [Specific: chart type, icon suggestions, image placeholders, color blocks]
**Duration**: [Recommended speaking time, e.g., "~2 min"]
**Sources**: [S1], [S3] (or "None" if no external data on this slide)

**On-Slide Content**:
- [Point 1]
- [Point 2]
- [Point 3]
- [Visual element: e.g., "Bar chart showing quarterly revenue growth"]
- [Footnote: "Source: McKinsey, 2026"]

**Speaker Notes**:
[Detailed presenter script: key messages, transition hooks to next slide, additional context, potential Q&A prep. Include full source details for cited data: "This 72% figure comes from McKinsey's 2026 annual AI review — methodology was a survey of 1,400 enterprises globally." 3-5 sentences minimum.]
```

#### Blueprint Rules

1. **One idea per slide** — never exceed 3 main points per slide
2. **Progressive disclosure** — build complex ideas across multiple slides, never cram into one
3. **The "Breadcrumb"** — include transition sentences linking each slide to the previous/next
4. **Data storytelling** — convert raw numbers into insights (e.g., "40% increase" → "2× industry average growth")
5. **Consistent framing** — maintain recurring visual metaphors or narrative threads throughout
6. **Layout sequencing** — use the `sequenceLayouts()` function (see `references/code-helpers.md`) to enforce variety. No consecutive repeats, alternate visual weight, use 5+ unique layouts in a 10-slide deck.
7. **Source traceability** — every data claim in the Blueprint must reference a Source Registry entry (`[S1]`, `[S2]`, etc.). If a slide has external data, list its sources in the `**Sources**` field and include a footnote in On-Slide Content. Speaker notes should contain full citation details.

#### References Slide

When the deck cites 3+ external sources, include a **References slide** as the second-to-last slide (before the Closing/CTA slide), or as the first Appendix slide. Format:

```
### Slide [N]: References
**Type**: References
**Layout**: Table Slide or Card Grid
**Visual Direction**: Clean numbered list, small text (10-12pt), organized by type or order of appearance
**Duration**: "~0 min (skip in presentation, included for credibility)"
**Sources**: All

**On-Slide Content**:
1. [Full citation from S1]
2. [Full citation from S2]
3. [Full citation from S3]
...

**Speaker Notes**:
This slide is included for reference and credibility. You don't need to present it — simply mention that full sources are available at the end of the deck if anyone wants to review them.
```

For decks with fewer than 3 external sources, skip the dedicated References slide and rely on individual slide footnotes instead.

#### Present and Wait

Present the Deck Blueprint to the user for review. They should be able to add, remove, or reorder slides. Only proceed after approval (explicit or implicit "looks good").

---

### Phase 3: Design & Generate

Read the **pptx skill** at `/mnt/skills/public/pptx/SKILL.md` first, then read `/mnt/skills/public/pptx/pptxgenjs.md` for the PptxGenJS API reference.

Also read these references from this skill's directory:
- **Layout library**: `references/layouts.md` — 20 slide layout patterns (including KPI Card Row, Strategy Dashboard, Product Feature Showcase, Period-over-Period Bars, Multi-Period Table, Bold Section Divider)
- **Theme library**: `references/themes.md` — color palettes and font pairings
- **Icon library**: `references/icons.md` — 1,392 built-in vector icons, usage patterns, topic mapping
- **Icon keyword lookup**: `references/icon-keywords.json` — fast topic→icon resolution (load with `findIcons()` from code-helpers)
- **Data visualization**: `references/data-visualization.md` — chart recipes, D1–D7 layouts, stat callouts, theme styling
- **Code helpers**: `references/code-helpers.md` — layout sequencer, icon keyword lookup, RTL helpers, QA automation, handout generator, option object safety
- **RTL guide** (if RTL content detected): `references/rtl-guide.md` — layout mirroring, font selection, bidirectional content

#### Design Principles

1. **Use the user-selected theme** — apply the color palette and font pairing the user chose during Phase 1 Theme Selection. If the user hasn't selected a theme yet, stop and ask them to pick one from the Theme Menu before proceeding. Never auto-select or default to a theme without user confirmation.

2. **Vary layouts aggressively** — the #1 sin of AI slides is repetition. Use the `sequenceLayouts()` function from `references/code-helpers.md` to programmatically assign and enforce layout variety. The layout library has 20 patterns. Minimum 5 different layouts for a 10-slide deck. For financial/earnings decks, prefer layouts 15 (Multi-Period Table), 16 (KPI Card Row), and 20 (Period-over-Period Bars). For product decks, use layout 18 (Product Feature Showcase). For strategy presentations, use layout 17 (Strategy Dashboard).

3. **The "sandwich" structure** — dark background for title + closing slides, lighter backgrounds for content slides. Creates visual bookends.

4. **Visual hierarchy** on every slide:
   - Title (H1): 36-44pt, bold, header font
   - Key Message (H2): 20-24pt, bold
   - Supporting Details (body): 14-16pt, body font
   - Stat callouts: 48-72pt, bold, accent color
   - Captions / sources: 10-12pt, muted color

5. **Every slide needs a visual element** — non-negotiable. Options:
   - Icon in colored circle (prefer built-in vector icons; fall back to react-icons → sharp → PNG pipeline)
   - Chart (bar, line, pie, doughnut via PptxGenJS)
   - Large stat number with label
   - Colored shape blocks for layout structure
   - `[IMAGE: description]` placeholder with descriptive text

6. **Icons are your secret weapon** — This skill ships with **1,392 professional vector icons** as transparent PNGs in `icons/` (see `references/icons.md`). Use the keyword lookup in `references/icon-keywords.json` for fast topic→icon resolution. Use the built-in icons first; only fall back to react-icons for icons not in the library.

   **Icon usage workflow**:
   1. Use `findIcons('keyword')` from `references/code-helpers.md` for fast lookup by topic
   2. Or consult `references/icons.md` for the full category guide
   3. Use dark variants on light backgrounds, white variants on dark/colored backgrounds
   4. Use `addIconInCircle()` for the polished colored-circle look
   5. If no built-in icon fits, fall back to react-icons → sharp → PNG

   **Icon sizing guide**:
   - Inline accent (next to heading): 0.35"–0.45"
   - Feature card icon: 0.5"–0.6"
   - Hero/callout icon: 0.8"–1.2"

7. **Data visualization** — when content has numbers:
   - Use PptxGenJS charts with theme-matched colors via `makeChartStyle()` factory (see `references/data-visualization.md`)
   - Modern chart styling (muted grid lines, clean labels, no axis lines)
   - Single key stats → large number callouts (60-72pt) with trend indicators instead of charts
   - Use data-specific layouts D1–D7 from the data visualization reference
   - Chart titles should state the insight ("Revenue Grew 58%"), not just label the data ("Quarterly Revenue")

8. **Shapes for structure** — colored rectangles and overlays create:
   - Card layouts (white cards with shadow on colored backgrounds)
   - Section dividers, accent strips, background blocks

9. **Source footnotes on data slides** — when a slide cites external data, add a small source line at the bottom:
   ```javascript
   // Source footnote — bottom of slide, muted styling
   slide.addText('Source: McKinsey Global Institute, 2026', {
     x: 0.5, y: 5.1, w: 9, h: 0.3,
     fontSize: 9, color: theme.mutedText || '999999',
     fontFace: BODY_FONT, italic: true,
     align: 'left', valign: 'bottom',
   });
   ```
   Keep footnotes to one line. Use short-form attribution (Org, Year) on-slide; save full citations for speaker notes and the References slide.

10. **RTL support** — when RTL content is detected:
   - Read `references/rtl-guide.md` for the full design system
   - Use `isRTL()` to detect RTL content, `rtlText()` for text options, `rtlX()` for position mirroring
   - Use `createRTLContext()` wrapper from `references/code-helpers.md` for automatic mirroring
   - Select RTL-safe fonts: Tahoma (headers) + Arial (body) for Arabic
   - Mirror two-column layouts, icon rows, and timelines for right-to-left reading

#### Speaker Notes Implementation

Add speaker notes to every slide using PptxGenJS:

```javascript
slide.addNotes("Detailed presenter script here. Include: key messages to emphasize, " +
  "transition to the next slide, additional context not shown on-slide, " +
  "potential audience questions and suggested responses.");
```

Speaker notes should contain:
- **Key talking points** (the story behind the slide)
- **Transition hook** (how to bridge to the next slide)
- **Additional context** (data sources, caveats, deeper detail)
- **Q&A prep** (anticipated questions and responses for critical slides)

#### Slide Construction Order

For each slide:
1. Set background (solid color or gradient image)
2. Add structural shapes (cards, dividers, accent strips)
3. Add text content (titles, body, labels) — apply RTL settings if needed
4. Add visual elements (icons, charts, stat numbers)
5. Add footer/source if needed
6. Add speaker notes (`slide.addNotes(...)`)

#### Code References

All reusable code patterns (icon helpers, layout sequencer, RTL helpers, QA functions, option object safety) are in `references/code-helpers.md`. Read that file for implementation details. Key functions:

| Function | Purpose |
|----------|---------|
| `sequenceLayouts(slides)` | Programmatic layout variety enforcement |
| `getIcon(category, index, white)` | Extract icon from sprite sheet |
| `addIconInCircle(slide, pres, ...)` | Icon in colored circle |
| `findIcons(keyword)` | Fast topic→icon lookup from keyword index |
| `findAndGetIcon(keyword, white)` | Find + extract icon in one call |
| `isRTL(text)` | Detect RTL content |
| `rtlText(options)` | RTL-aware text options |
| `rtlX(x, w)` | Mirror x-position for RTL |
| `createRTLContext(rtlEnabled)` | Full RTL context wrapper |
| `runQAChecks(pptxPath, deckMeta)` | Automated QA checks |
| `estimateTextFit(text, w, h, fontSize)` | Text overflow estimation |
| `checkContrast(fg, bg)` | WCAG contrast ratio check |
| `generateHandout(pptxPath, outputPath)` | Markdown handout from deck |
| `makeCardShadow()` | Safe shadow factory (avoids PptxGenJS mutation) |
| `addPoPPanel(slide, pres, {...})` | Period-over-Period bar panel with growth callout (Layout 20) |
| `addPhoneMockup(slide, x, y, w, h)` | Phone device frame with placeholder screen (Layout 18) |
| `addMiniMetricCard(slide, pres, {...})` | Mini bar chart card for Strategy Dashboard (Layout 17) |

---

### Phase 4: Advanced Features

After the core deck is generated, consider adding these based on context:

#### Backup / Appendix Slides

For pitch decks, quarterly reviews, and thought leadership decks, generate 2-3 appendix slides covering anticipated tough questions. Label them clearly as "Appendix" with a section divider.

#### Alternative Layouts

For critical slides (title, key data, closing), consider offering 2-3 layout options if the user expressed uncertainty about the look.

#### Accessibility Notes

When generating, keep in mind:
- Maintain sufficient color contrast (4.5:1 minimum for text) — use `checkContrast()` from code-helpers to verify
- Avoid conveying meaning through color alone
- Include alt-text descriptions for icons and charts via the `altText` property on images

#### Formatting Conventions in the Blueprint

When writing the Deck Blueprint, use these markers so the user and generator can clearly identify element types:
- **Bold** for key terms to be visually emphasized in final design
- `Code blocks` for specific data points or figures to be charted
- `[IMAGE: description]` for visual asset placeholders
- `[ANIMATION: sequence]` where build sequences enhance understanding
- `[CHART: type — data description]` for chart specifications

---

### Phase 5: QA (Mandatory)

#### Automated QA (run first)

Before visual inspection, run the automated QA checks from `references/code-helpers.md`:

```javascript
const issues = runQAChecks('/home/claude/output.pptx', {
  slideCount: 12,          // expected from Blueprint
  durationMinutes: 30,     // from user's time constraint
  theme: selectedTheme,    // for contrast checking
});

if (issues.length > 0) {
  console.log('QA issues found:');
  issues.forEach(i => console.log(`  [${i.severity}] Slide ${i.slide}: ${i.issue}`));
}
```

**Automated checks performed:**
- Slide count matches Blueprint
- Speaker notes present on every slide
- Empty slide detection (too little visible text)
- Slide count vs. duration sanity check
- Text overflow estimation (via `estimateTextFit()`)
- Color contrast validation (via `checkContrast()`)

#### Manual QA (run after automated checks pass)

1. **Content QA**: `python -m markitdown output.pptx` — check for missing/wrong text
2. **Speaker Notes QA**: Verify notes are present on each slide (markitdown outputs notes under `### Notes:`)
3. **Visual QA**: Convert to images and inspect every slide:
   ```bash
   python scripts/office/soffice.py --headless --convert-to pdf output.pptx
   rm -f slide-*.jpg
   pdftoppm -jpeg -r 150 output.pdf slide
   ls -1 "$PWD"/slide-*.jpg
   ```
4. **RTL QA** (if applicable): Check text direction, layout mirroring, font rendering, mixed content alignment
5. **Fix issues** — overlaps, cut-off text, low contrast, uneven spacing
6. **Re-verify** after fixes — one fix often creates another problem
7. **Repeat** until clean

---

### Phase 6: Deliver

Copy the final .pptx to `/mnt/user-data/outputs/` and present to the user. Include a brief summary of what's in the deck (slide count, theme used, total estimated duration).

If deck variants were generated (Elevator + Full), deliver both files with clear naming:
- `presentation-elevator.pptx` (3-5 slides)
- `presentation-full.pptx` (10-15 slides)

After delivery, offer:
- **Speaker Cheat Sheet** — condensed 1-page notes with key talking points per slide (markdown or PDF)
- **Handout PDF** — dense text format of the deck content suitable for printing or sharing

#### Handout PDF Generation

Use the `generateHandout()` function from `references/code-helpers.md` to create a markdown handout, then convert to PDF:

```bash
# Generate markdown handout from the deck
# (see references/code-helpers.md for the generateHandout() implementation)

# Convert to PDF (pick the method that's available)
# Option 1: pandoc
pandoc handout.md -o handout.pdf --pdf-engine=xelatex -V geometry:margin=1in

# Option 2: Python markdown + wkhtmltopdf
pip install markdown pdfkit --break-system-packages
python3 -c "
import markdown, pdfkit
with open('handout.md') as f:
    html = markdown.markdown(f.read(), extensions=['tables', 'fenced_code'])
styled = f'<html><head><style>body{{font-family:Calibri,sans-serif;max-width:700px;margin:auto;padding:40px;line-height:1.6}}h1{{color:#1A1A1A}}h2{{color:#333;border-bottom:1px solid #ddd;padding-bottom:4px}}hr{{border:none;border-top:1px solid #eee}}</style></head><body>{html}</body></html>'
pdfkit.from_string(styled, 'handout.pdf')
"

# Option 3: LibreOffice (fallback)
libreoffice --headless --convert-to pdf handout.md
```

Copy the handout to `/mnt/user-data/outputs/handout.pdf` and present alongside the deck.

---

## Response Protocol (Summary)

1. **Detect mode**: New deck or redesign of existing `.pptx`?
2. Parse user input → detect presentation type, audience, time constraint, language direction, **research source types**
3. **Ask user to select a theme** from the Theme Menu (show the 5 themes with mood/best-for context)
4. For pitch decks, offer **Deck Variants** (Elevator / Standard / Deep Dive)
5. **Research topic** using targeted source types (academic, industry, government, etc.) → build **Source Registry**
6. Present **Deck Blueprint** (outline with slide types, visual direction, source references, speaker notes)
7. Wait for user approval or modifications
8. Generate full slide deck with the **user-selected theme** applied, using layout sequencer for variety; add **source footnotes** on data slides and a **References slide** if 3+ sources
9. Run **automated QA** (slide count, notes, contrast, overflow) then **visual QA**
10. Deliver .pptx (+ variants if requested) + offer Speaker Cheat Sheet or **Handout PDF**

---

## Quick Reference

| What | Where |
|------|-------|
| PptxGenJS API | `/mnt/skills/public/pptx/pptxgenjs.md` |
| Layout library — Magazine mode (20 patterns) | `references/layouts.md` (this skill) |
| Theme library — Magazine mode (5 palettes) | `references/themes.md` (this skill) |
| **Orange theme — IR/earnings design system** | **`references/orange.md` (this skill)** |
| **Icon library — 1,392 vector icons** | **`references/icons.md` (this skill)** |
| **Icon keyword lookup — fast topic→icon** | **`references/icon-keywords.json` (this skill)** |
| **Data visualization — charts, layouts, styling** | **`references/data-visualization.md` (this skill)** |
| **Code helpers — sequencer, RTL, QA, handout** | **`references/code-helpers.md` (this skill)** |
| **RTL / multi-language guide** | **`references/rtl-guide.md` (this skill)** |
| **Icon sprite sheets + manifest** | **`icons/{category}_sprite.png` + `icons/manifest.json`** |
| Pure Minimal style overrides | `references/pure-minimal.md` (this skill) |
| PPTX skill (QA, deps) | `/mnt/skills/public/pptx/SKILL.md` |
| Theme factory | `/mnt/skills/examples/theme-factory/SKILL.md` |

## Dependencies

Same as the pptx skill:
- `npm install -g pptxgenjs` — slide generation
- `npm install -g react-icons react react-dom sharp` — icons (optional fallback; built-in icon library covers most needs)
- `pip install "markitdown[pptx]"` — text extraction for QA
- LibreOffice + Poppler — PDF conversion for visual QA

## Built-in Icon Library

This skill includes **1,392 professional vector icons** as transparent 128×128 PNGs, extracted from a curated icon set. Icons are pre-rendered in two variants:

- **Dark** (black on transparent) — for light slide backgrounds
- **White** (white on transparent) — for dark slide backgrounds and colored accent circles

### Categories at a Glance

| Category | Count | What's In It |
|----------|-------|-------------|
| `general` | 137 | Chat, devices, media, connectivity, code, settings |
| `arrow` | 58 | Directional, navigation, chevrons, expand/collapse |
| `business` | 138 | Charts, mail, documents, phone, globe, finance |
| `interface` | 255 | Checkmarks, toggles, search, lock, home, cloud, heart |
| `buildings` | 38 | Houses, offices, churches, hotels, airports, travel |
| `user` | 118 | Emoji faces, hand gestures, gender symbols |
| `other` | 246 | Weather, animals, vehicles, food & drink, nature |
| `brand` | 402 | Company logos (use only when discussing those brands) |

### Fast Icon Lookup

Use the keyword index at `references/icon-keywords.json` for instant topic→icon resolution without scanning the full catalog. Load it with `findIcons()` or `findAndGetIcon()` from `references/code-helpers.md`.

See `references/icons.md` for the complete catalog, code examples, and topic-to-icon quick reference.
