# QA Helpers (Load during Phase 4-5 only)

## Slide Setup (CRITICAL)
```javascript
const pres = new PptxGenJS();
pres.layout = 'LAYOUT_16x9'; // 10" × 5.625" — required for all layouts
// NEVER use LAYOUT_WIDE (13.33" × 7.5" — breaks positioning)
// defineLayout requires `width`/`height`, NOT `w`/`h`
```

## Option Object Safety
PptxGenJS mutates objects in-place. Never share option objects between calls.
```javascript
const makeCardShadow = () => ({ type:"outer", blur:6, offset:2, angle:135, color:"000000", opacity:0.15 });
// ✅ Fresh: slide.addShape(..., { shadow: makeCardShadow() });
```

## Layout Sequencer
```javascript
function sequenceLayouts(slides) {
  const LAYOUTS = {
    1:{name:'Title Hero',weight:'heavy',types:['title']}, 2:{name:'Section Divider',weight:'medium',types:['section']},
    3:{name:'Two-Column Split',weight:'medium',types:['content','comparison']}, 4:{name:'Icon Row',weight:'medium',types:['content']},
    5:{name:'Stat Callout',weight:'heavy',types:['data']}, 6:{name:'Card Grid',weight:'medium',types:['content']},
    7:{name:'Timeline',weight:'medium',types:['content']}, 8:{name:'Chart+Insight',weight:'medium',types:['data']},
    9:{name:'Quote',weight:'light',types:['quote']}, 10:{name:'Image+Overlay',weight:'heavy',types:['content']},
    11:{name:'Table',weight:'light',types:['data','comparison']}, 12:{name:'Closing',weight:'heavy',types:['closing']},
    13:{name:'Agenda',weight:'light',types:['agenda']}, 14:{name:'Before/After',weight:'medium',types:['comparison']},
    15:{name:'Multi-Period Table',weight:'medium',types:['data','comparison']}, 16:{name:'KPI Cards',weight:'heavy',types:['data']},
    17:{name:'Strategy Dashboard',weight:'heavy',types:['data','content']}, 18:{name:'Product Feature',weight:'medium',types:['content']},
    19:{name:'Bold Section Divider',weight:'heavy',types:['section']}, 20:{name:'PoP Bar Comparison',weight:'medium',types:['data']},
  };
  const WEIGHT_ORDER = ['heavy','medium','light','medium'];
  const result = []; let lastLayout = null, weightIdx = 0;
  slides.forEach((s, i) => {
    let layout;
    if (s.type==='title') layout=1; else if (s.type==='closing') layout=12; else if (s.type==='section') layout= i>slides.length*0.5?19:2;
    else if (s.type==='agenda') layout=13;
    else {
      const targetWeight = WEIGHT_ORDER[weightIdx % 4];
      const candidates = Object.entries(LAYOUTS).filter(([id,l])=> l.types.includes(s.type||'content') && l.weight===targetWeight && Number(id)!==lastLayout);
      layout = candidates.length ? Number(candidates[Math.floor(Math.random()*candidates.length)][0]) : Number(Object.entries(LAYOUTS).filter(([id,l])=> l.types.includes(s.type||'content') && Number(id)!==lastLayout)[0]?.[0]||3);
      weightIdx++;
    }
    result.push({ slideIndex:i, layout, layoutName: LAYOUTS[layout]?.name });
    lastLayout = layout;
  });
  return result;
}
```

## RTL Helpers
```javascript
function mirrorTextOptions(options) { return { ...options, align: options.align==='left'?'right':options.align==='right'?'left':options.align }; }
function rtlX(x, w, slideW=10) { return slideW - x - w; }
function isRTL(text) { return ((text.match(/[\u0600-\u06FF\u0590-\u05FF\uFB50-\uFDFF\uFE70-\uFEFF]/g)||[]).length) > text.length*0.3; }
```

## Automated QA Checks
```javascript
function runQAChecks(pptxPath, deckMeta) {
  const issues = [];
  let content;
  try { content = require('child_process').execSync(`python -m markitdown "${pptxPath}"`, {encoding:'utf8'}); }
  catch(e) { return [{slide:0, severity:'error', issue:'markitdown extraction failed'}]; }
  const slideHeaders = content.match(/^## Slide \d+/gm) || [];
  if (slideHeaders.length !== deckMeta.slideCount) issues.push({slide:0, severity:'error', issue:`Expected ${deckMeta.slideCount} slides, found ${slideHeaders.length}`});
  const notesCount = (content.match(/### Notes:/g)||[]).length;
  if (notesCount < slideHeaders.length) issues.push({slide:0, severity:'warning', issue:`${slideHeaders.length-notesCount} slide(s) missing notes`});
  content.split(/^## Slide \d+/gm).slice(1).forEach((s,i) => { if (s.replace(/### Notes:[\s\S]*/m,'').trim().length<20) issues.push({slide:i+1, severity:'warning', issue:'Very little visible text'}); });
  if (deckMeta.durationMinutes) { const exp=Math.round(deckMeta.durationMinutes/2.5); const r=slideHeaders.length/exp; if(r>1.5||r<0.5) issues.push({slide:0,severity:'warning',issue:`${slideHeaders.length} slides for ${deckMeta.durationMinutes}min (expected ~${exp})`}); }
  return issues;
}
function estimateTextFit(text, width, height, fontSize) {
  const cpl=Math.floor(width*(72/fontSize)*1.8), la=Math.floor((height*72)/(fontSize*1.4)), ln=Math.ceil(text.length/cpl);
  return { fits: ln<=la, linesNeeded:ln, linesAvailable:la };
}
function checkContrast(fg, bg) {
  const lum=h=>{const[r,g,b]=[0,2,4].map(i=>parseInt(h.slice(i,i+2),16)/255).map(c=>c<=0.03928?c/12.92:Math.pow((c+0.055)/1.055,2.4));return 0.2126*r+0.7152*g+0.0722*b;};
  const l1=lum(fg.replace('#','')), l2=lum(bg.replace('#',''));
  const ratio=(Math.max(l1,l2)+0.05)/(Math.min(l1,l2)+0.05);
  return { ratio:Math.round(ratio*100)/100, passAA:ratio>=4.5, passAAA:ratio>=7 };
}
```

## Visual QA
```bash
python scripts/office/soffice.py --headless --convert-to pdf output.pptx
pdftoppm -jpeg -r 150 output.pdf slide
ls -1 "$PWD"/slide-*.jpg
```

## Handout PDF
```javascript
function generateHandout(pptxPath, outputPath, options={}) {
  const content = require('child_process').execSync(`python -m markitdown "${pptxPath}"`, {encoding:'utf8'});
  const slides = content.split(/^## Slide /gm).slice(1);
  let md = `# ${options.title||'Handout'}\n\n${options.author?'**Presenter**: '+options.author+'\n':''}${options.date?'**Date**: '+options.date+'\n':''}\n---\n\n`;
  slides.forEach((s,i) => {
    const title = s.trim().split('\n')[0] || `Slide ${i+1}`;
    md += `## ${i+1}. ${title}\n\n`;
    const ni = s.indexOf('### Notes:');
    if (ni>=0) md += s.slice(ni+'### Notes:'.length).trim()+'\n\n';
    md += '---\n\n';
  });
  require('fs').writeFileSync(outputPath, md, 'utf8');
}
```
```bash
pandoc handout.md -o handout.pdf --pdf-engine=xelatex -V geometry:margin=1in
```
