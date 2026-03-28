# Vector Icon Library Reference

This skill ships with **1,392 professional vector icons** organized into 8 categories with both dark (for light backgrounds) and white (for dark backgrounds) variants, stored as sprite sheet PNGs.

## Icon Library Structure

```
icons/
├── general_sprite.png          # 137 icons — chat, device, media, connectivity
├── general_white_sprite.png    # 137 icons — white variants for dark slides
├── arrow_sprite.png            # 58 icons — directional, navigation, chevrons
├── arrow_white_sprite.png
├── business_sprite.png         # 138 icons — office, finance, communication, documents
├── business_white_sprite.png
├── interface_sprite.png        # 255 icons — UI controls, toggles, checks, settings
├── interface_white_sprite.png
├── buildings_sprite.png        # 38 icons — architecture, travel, hospitality
├── buildings_white_sprite.png
├── user_sprite.png             # 118 icons — emoticons, gestures, gender symbols
├── user_white_sprite.png
├── other_sprite.png            # 246 icons — weather, nature, vehicles, food, animals
├── other_white_sprite.png
├── brand_sprite.png            # 402 icons — company logos and brand marks
├── brand_white_sprite.png
└── manifest.json               # Sprite sheet metadata (grid positions, counts)
```

Each sprite sheet arranges icons in a 15-column grid. Each cell is 128x128px with 4px padding. The `manifest.json` contains exact pixel coordinates for every icon.

## How to Use in PptxGenJS

### Step 1: Icon Extraction Helper (add to your generation script)

Icons are stored in sprite sheets. Use `sharp` to extract individual icons at generation time:

```javascript
const sharp = require('sharp');
const path = require('path');
const fs = require('fs');

const SKILL_DIR = '/mnt/skills/user/pure-style-slides';
const ICON_DIR = path.join(SKILL_DIR, 'icons');
const manifest = JSON.parse(fs.readFileSync(path.join(ICON_DIR, 'manifest.json'), 'utf8'));

// Cache for extracted icons (avoid re-extracting the same icon)
const iconCache = {};

/**
 * Extract a single icon from a sprite sheet and return as base64 PNG.
 * 
 * @param {string} category - One of: general, arrow, business, interface, buildings, user, other, brand
 * @param {number} index - Icon number (1-based)
 * @param {boolean} white - true for white variant (dark backgrounds), false for dark variant
 * @returns {Promise<string>} Base64 PNG data string for PptxGenJS addImage({ data: ... })
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
```

### Step 2: Use icons in slides

```javascript
// Dark icon on light background
const chartIcon = await getIcon('business', 16);
slide.addImage({ data: chartIcon, x: 1.0, y: 2.0, w: 0.5, h: 0.5 });

// White icon on dark background
const gearIcon = await getIcon('general', 16, true);
slide.addImage({ data: gearIcon, x: 1.0, y: 2.0, w: 0.5, h: 0.5 });
```

### Step 3: Icon in colored circle (recommended pattern)

```javascript
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

// Usage:
await addIconInCircle(slide, pres, 'business', 16, 2.0, 3.0, 0.5, 'F96167');
```

### Step 4: Icon row layout (3-column with icons)

```javascript
const items = [
  { cat: 'business', idx: 16, title: 'Revenue', desc: '+23% YoY growth' },
  { cat: 'business', idx: 43, title: 'Markets', desc: 'Expanded to 12 regions' },
  { cat: 'interface', idx: 45, title: 'Uptime', desc: '99.97% SLA achieved' },
];

for (let i = 0; i < items.length; i++) {
  const item = items[i];
  const x = 0.8 + i * 3.8;
  await addIconInCircle(slide, pres, item.cat, item.idx, x + 0.2, 2.5, 0.5, accentColor);
  slide.addText(item.title, { x: x - 0.1, y: 3.3, w: 1.2, h: 0.4, fontSize: 18, bold: true, align: 'center' });
  slide.addText(item.desc, { x: x - 0.3, y: 3.7, w: 1.6, h: 0.4, fontSize: 12, align: 'center' });
}
```

## Choosing the Right Variant

| Slide Background | Icon Variant | How to Get |
|-----------------|-------------|------------|
| White / light colors | Dark (default) | `await getIcon('business', 16)` |
| Dark / navy / black | White | `await getIcon('business', 16, true)` |
| Colored circle accent | White | `await getIcon('business', 16, true)` |
| Light colored card | Dark | `await getIcon('business', 16)` |

## Icon Category Guide — What's Where

### GENERAL (137 icons) — `getIcon('general', 1)` to `getIcon('general', 137)`
Technology, communication, and device icons.

- **1-14**: Chat bubbles, speech, messaging, comments, video, QR
- **15-28**: Barcode, settings gear, tools, code brackets, keyboard, laptop, film, shield, terminal
- **29-42**: Screen/display, music notes, accessibility, bell, bluetooth, hotspot, mail, archive, save, download
- **43-56**: Warning, scissors, mic, microscope, phone, paper plane, RSS, contactless, infinity, upload
- **57-70**: Mouse, plug, power, antenna, server, tag, monitor, chip, drawer, headphones, keyboard, screens, wifi
- **71-137**: Second page continues with more variants

### ARROW (58 icons) — `getIcon('arrow', 1)` to `getIcon('arrow', 58)`
- **1-14**: Double chevrons, chevron pairs, single chevrons, circled arrows
- **15-28**: Circled arrows (up variants), plain arrows, directional triangles
- **29-42**: Play triangles, caret indicators, boxed chevrons/arrows
- **43-58**: Boxed arrows, circled chevrons, exchange, expand/collapse, cursor, shuffle, repeat

### BUSINESS (138 icons) — `getIcon('business', 1)` to `getIcon('business', 138)`
- **1-14**: ID card, badge, briefcase, scales/justice, balance, suitcase, megaphone, target, camera, clipboard, calendar
- **15-28**: Award/medal, trending chart, bar chart, mixed chart, line chart, pie chart, clipboard, book, document, bookmark, copyright, scissors, compose, mail
- **29-42**: Envelope variants, tag, fax, document variants, notepad, glasses, pen, fountain pen
- **43-56**: Globe variants, map pin, attachment, paste, pencil variants, edit
- **57-70**: Flow chart, org chart, registered mark, phone variants, phonebook, piggy bank, printer

### INTERFACE (255 icons) — `getIcon('interface', 1)` to `getIcon('interface', 255)`
- **1-14**: Prohibited, hamburger menu, back/undo, checkmark variants
- **15-28**: Circle, cloud variants, close/X, crop, crosshair, cursor, cut, dashboard, database
- **29-42**: Delete, disc, eye show/hide, filter, flag, flash, grid, hash, heart, home
- **43-56**: Hourglass, image, info, key, layers, layout, link, list, lock/unlock, log in/out
- **57+**: Map, menu, moon, music, notification, sliders, package, paperclip, pause, play, plus, printer, question, refresh, search, send, share, shield, shopping, star, sun, table, terminal, timer, toggle, trash, trending, trophy, user, volume, wifi, zoom

### BUILDINGS (38 icons) — `getIcon('buildings', 1)` to `getIcon('buildings', 38)`
- **1-14**: Gate/arch, office buildings, church, apartment, hospital, bridge, castle, house
- **15-28**: Garage, factory, stadium, courthouse, monument, chapel, barn, shops, temple, torii, pagoda
- **29-38**: Warehouse, passport, room service, bathtub, luggage, airplane, hotel, beach, shower

### USER (118 icons) — `getIcon('user', 1)` to `getIcon('user', 118)`
- **1-56**: Full emoji set, thumbs up/down, high five, wave
- **57-70**: Hand gestures (open hands, handshake, peace, ok, fist, pointing)
- **71-118**: Gender symbols, more gesture variants

### OTHER (246 icons) — `getIcon('other', 1)` to `getIcon('other', 246)`
- **Weather (1-14)**: Lightning, rain, cloud, storm, fog, snowflake, rainbow
- **Nature (15-28)**: Apple, barn, dolphin, running, tractor, pine tree, first aid, battery
- **Animals (29-42)**: Cat, dog, horse, bird, fish, rabbit, elephant, camel, pig, spider
- **Vehicles (43-56)**: Ambulance, bus, van, car, bicycle, fire truck, taxi, truck
- **Food & Drink (57-76)**: Beer, blender, cocktail, coffee, champagne, martini, wine, teacup
- **77-246**: Clothing, sports, education, medical, music, tools

### BRAND (402 icons) — `getIcon('brand', 1)` to `getIcon('brand', 402)`
Company logos. **Only use when discussing those brands.**

## Topic-to-Icon Quick Reference

| Topic | Recommended Call |
|-------|-----------------|
| **Revenue / Finance** | `getIcon('business', 15)` trending, `getIcon('business', 16)` bar chart, `getIcon('business', 19)` pie |
| **Growth / Metrics** | `getIcon('arrow', 48)` up arrow, `getIcon('business', 14)` target |
| **Communication** | `getIcon('general', 1)` chat, `getIcon('business', 28)` mail |
| **Technology** | `getIcon('general', 15)` code, `getIcon('general', 23)` laptop |
| **Security** | `getIcon('general', 27)` shield, `getIcon('interface', 55)` lock, `getIcon('interface', 51)` key |
| **Settings** | `getIcon('general', 16)` gear, `getIcon('interface', 67)` sliders |
| **Travel** | `getIcon('buildings', 29)` passport, `getIcon('business', 43)` globe, `getIcon('buildings', 35)` airplane |
| **Data / Analytics** | `getIcon('business', 16)` bar chart, `getIcon('interface', 37)` database |
| **Documents** | `getIcon('business', 33)` document, `getIcon('business', 38)` notepad |
| **Health** | `getIcon('other', 26)` first aid, `getIcon('buildings', 6)` hospital |
| **Education** | `getIcon('business', 22)` book, `getIcon('interface', 48)` info circle |
| **Real Estate** | `getIcon('buildings', 9)` house, `getIcon('buildings', 2)` office |
| **Food** | `getIcon('other', 57)` beer, `getIcon('other', 61)` coffee |

## Best Practices

1. **Always match variant to background**: Dark icons on light, white icons on dark/colored
2. **Use icons in colored circles**: `addIconInCircle()` for the polished look
3. **Consistent sizing**: 0.4"–0.6" inline, 0.8"–1.2" for feature callouts
4. **Don't overuse**: 2-4 icons per slide maximum
5. **Brand icons require care**: Only when discussing those brands
6. **All getIcon() calls are async**: Use `await` or batch with `Promise.all()`
7. **Icons are cached**: First call extracts from sprite sheet, subsequent calls return cached base64
