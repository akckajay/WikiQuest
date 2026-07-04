# WikiQuest
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>WIKIQUEST — Trail to the Word</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=Spectral:ital,wght@0,400;0,500;0,600;0,700;1,400&family=JetBrains+Mono:wght@400;500;600;700&display=swap" rel="stylesheet">
<style>
  :root{
    --paper: #EDE6D6;
    --paper-deep: #E2D9C4;
    --ink: #2A2420;
    --ink-soft: #5B5348;
    --moss: #4F6B4A;
    --moss-deep: #3A5136;
    --rust: #BD5B38;
    --rust-deep: #9C4529;
    --gold: #C99A3E;
    --line: rgba(42,36,32,0.16);
  }
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    background: var(--paper);
    background-image: radial-gradient(circle at 1px 1px, rgba(42,36,32,0.05) 1px, transparent 0);
    background-size: 22px 22px;
    color: var(--ink);
    font-family: 'Spectral', Georgia, serif;
    min-height: 100vh;
  }
  .mono{ font-family: 'JetBrains Mono', monospace; }

  .topbar{
    position: sticky; top:0; z-index: 20;
    background: var(--paper);
    border-bottom: 2px solid var(--ink);
    padding: 10px 14px 12px;
  }
  .brand{ display:flex; align-items:baseline; justify-content:space-between; gap:10px; margin-bottom:8px; }
  .brand h1{
    font-size: 1.05rem; letter-spacing: 0.12em; margin:0;
    font-family:'JetBrains Mono', monospace; font-weight:700; text-transform: uppercase;
  }
  .brand h1 span{ color: var(--rust); }
  .clicks{ font-family:'JetBrains Mono', monospace; font-size:0.72rem; color: var(--ink-soft); white-space:nowrap; }

  .quest-row{ display:flex; align-items:stretch; gap:8px; flex-wrap: wrap; }
  .quest-card{ flex:1; min-width: 140px; border:1.5px solid var(--ink); border-radius:3px; padding:7px 10px; background: var(--paper-deep); position:relative; }
  .quest-label{ font-family:'JetBrains Mono', monospace; font-size:0.6rem; letter-spacing:0.14em; text-transform:uppercase; color: var(--ink-soft); display:block; margin-bottom:2px; }
  .quest-value{ font-weight:600; font-size:0.95rem; line-height:1.15; }
  .quest-card.target{ background: var(--rust); border-color: var(--rust-deep); }
  .quest-card.target .quest-label{ color: rgba(255,255,255,0.75); }
  .quest-card.target .quest-value{ color:#fff; }
  .quest-card.target::after{ content:"⚑"; position:absolute; right:8px; top:6px; font-size:1rem; color:#fff; }

  .trail-strip{ margin-top:8px; display:flex; align-items:center; gap:0; overflow-x:auto; padding-bottom:4px; scrollbar-width: thin; }
  .trail-chip{ font-family:'JetBrains Mono', monospace; font-size:0.68rem; background: var(--paper); border:1px solid var(--ink); border-radius: 999px; padding:3px 9px; white-space:nowrap; color: var(--ink-soft); flex-shrink:0; }
  .trail-chip.current{ background: var(--moss); border-color: var(--moss-deep); color:#fff; font-weight:600; }
  .trail-dots{ width:16px; height:1px; border-top: 2px dotted var(--ink-soft); flex-shrink:0; margin: 0 2px; }

  #article-wrap{ padding: 16px 16px 90px; max-width: 720px; margin: 0 auto; }
  #article{ font-size: 1.05rem; line-height: 1.7; }
  #article h1.article-title{ font-size:1.7rem; font-weight:700; margin: 0 0 0.5em; }
  #article p{ margin: 0.9em 0; }
  #article a{ color: var(--rust-deep); text-decoration: underline; text-decoration-color: rgba(189,91,56,0.4); text-underline-offset:2px; cursor:pointer; font-weight:600; }
  #article a:hover{ text-decoration-color: var(--rust-deep); }

  .center-screen{ min-height: 70vh; display:flex; flex-direction:column; align-items:center; justify-content:center; text-align:center; padding: 24px; }
  .compass{ font-size: 2.4rem; margin-bottom: 6px; }
  .center-screen h2{ font-size:1.5rem; margin: 0 0 8px; font-weight:700; }
  .center-screen p{ max-width: 420px; color: var(--ink-soft); line-height:1.6; margin: 0 0 20px; font-size:0.95rem; }
  .btn{
    font-family:'JetBrains Mono', monospace; font-weight:600; font-size:0.85rem; letter-spacing:0.04em; text-transform:uppercase;
    background: var(--ink); color: var(--paper); border:none; padding: 12px 22px; border-radius:3px; cursor:pointer;
    box-shadow: 3px 3px 0 var(--rust); transition: transform 0.1s ease;
  }
  .btn:active{ transform: translate(2px,2px); box-shadow: 1px 1px 0 var(--rust); }
  .btn-row{ display:flex; gap:10px; flex-wrap:wrap; justify-content:center; }

  .win-overlay{ position: fixed; inset:0; background: rgba(42,36,32,0.55); display:flex; align-items:center; justify-content:center; z-index:50; padding: 20px; }
  .win-card{ background: var(--paper); border: 2px solid var(--ink); border-radius:5px; max-width: 420px; width:100%; padding: 26px 22px; text-align:center; box-shadow: 6px 6px 0 var(--rust); }
  .win-card .flag{ font-size:2.2rem; }
  .win-card h2{ margin:6px 0 4px; font-size:1.4rem; }
  .win-card p{ color: var(--ink-soft); font-size:0.9rem; margin: 0 0 14px; }
  .win-stats{ font-family:'JetBrains Mono', monospace; font-size:0.85rem; background: var(--paper-deep); border:1px solid var(--line); border-radius:3px; padding: 10px; margin-bottom: 18px; text-align:left; }
  .win-stats .row{ display:flex; justify-content:space-between; margin: 3px 0; }
  .win-trail{ font-size:0.78rem; color: var(--ink-soft); word-break: break-word; }

  .toast{
    position: fixed; bottom: 18px; left:50%; transform: translateX(-50%);
    background: var(--ink); color: var(--paper); font-family:'JetBrains Mono', monospace;
    font-size: 0.75rem; padding: 8px 14px; border-radius: 4px; z-index: 60;
    opacity:0; pointer-events:none; transition: opacity 0.25s ease;
  }
  .toast.show{ opacity:1; }
  ::selection{ background: var(--gold); color: var(--ink); }
</style>
</head>
<body>

<div id="topbar" class="topbar" style="display:none;">
  <div class="brand">
    <h1>WIKI<span>QUEST</span></h1>
    <div class="clicks mono" id="click-count">0 clicks</div>
  </div>
  <div class="quest-row">
    <div class="quest-card">
      <span class="quest-label">Now reading</span>
      <span class="quest-value" id="current-title">—</span>
    </div>
    <div class="quest-card target">
      <span class="quest-label">Destination</span>
      <span class="quest-value" id="target-title">—</span>
    </div>
  </div>
  <div class="trail-strip" id="trail-strip"></div>
</div>

<div id="article-wrap"></div>
<div class="toast" id="toast"></div>

<script>
// Self-contained topic graph — no network calls needed, works fully offline.
const GRAPH = {
  "Elephant": "The elephant is the largest living land animal, known for its long trunk and curved [[Ivory]] tusks. Wild herds roam the [[Savanna]] and forests of [[Africa]], led by an experienced matriarch. Their closest ancient relative, the woolly [[Mammoth]], died out thousands of years ago.",
  "Ivory": "Ivory is the dense white material that makes up tusks and teeth, prized for centuries for carving and old [[Piano]] keys. Most commercial ivory historically came from [[Elephant]] tusks, though [[Mammoth]] tusks preserved in permafrost are also a legal source today.",
  "Africa": "Africa is the second-largest continent, home to vast deserts, dense [[Rainforest]], and the open grassland called [[Savanna]]. It supports some of the planet's most iconic wildlife, including the [[Elephant]] and the [[Lion]].",
  "Savanna": "A savanna is a wide grassland dotted with scattered trees, found across much of [[Africa]]. It supports grazing herds and their predators, most famously the [[Lion]], and borders both desert and [[Rainforest]] regions.",
  "Lion": "The lion is a large, social big cat and one of nature's most recognizable [[Predator]]s. Lions live in prides across the [[Savanna]] and grasslands of [[Africa]], hunting cooperatively for large prey.",
  "Predator": "A predator is any animal that hunts and feeds on other animals to survive. Predators like the [[Lion]] on land and the [[Shark]] in the [[Ocean]] play a key role in balancing ecosystems.",
  "Piano": "The piano is a keyboard instrument that produces sound when hammers strike internal strings. Its keys were traditionally topped with [[Ivory]], and it remains a central instrument in both solo [[Music]] and the full [[Orchestra]].",
  "Music": "Music is the art of arranging sound, rhythm, and pitch to create expression. It's produced by instruments ranging from the [[Piano]] and [[Guitar]] to full ensembles like the [[Orchestra]], and travels to our ears as a [[Sound Wave]].",
  "Orchestra": "An orchestra is a large ensemble combining string, wind, brass, and percussion instruments to perform complex [[Music]]. The [[Piano]] and [[String Instrument]] families are core to its sound.",
  "Volcano": "A volcano is an opening in the Earth's surface where molten rock, or [[Lava]], escapes from below. Volcanoes form at boundaries between [[Tectonic Plates]] and can trigger powerful [[Earthquake]]s nearby.",
  "Lava": "Lava is molten rock that reaches the Earth's surface during a [[Volcano]] eruption, cooling into hardened rock over time. Its movement is driven by pressure building beneath shifting [[Tectonic Plates]].",
  "Tectonic Plates": "Tectonic plates are massive slabs of the Earth's crust that slowly drift over time. Their collisions and separations cause [[Earthquake]]s, build mountains, and fuel [[Volcano]] activity, and their boundaries often lie beneath the [[Ocean]].",
  "Earthquake": "An earthquake is a sudden shaking of the ground caused by the movement of [[Tectonic Plates]] deep underground. Large earthquakes can also trigger eruptions from a nearby [[Volcano]] or waves across the [[Ocean]].",
  "Ocean": "The ocean is the vast body of saltwater covering most of the planet, home to the [[Coral Reef]], countless [[Fish]], and predators like the [[Shark]]. Its floor is shaped by shifting [[Tectonic Plates]].",
  "Coral Reef": "A coral reef is a colorful underwater structure built by tiny living organisms over centuries. Reefs shelter an enormous variety of [[Fish]] and other [[Ocean]] life, though they can attract the occasional [[Shark]].",
  "Shark": "The shark is a cartilage-skeletoned [[Predator]] that has patrolled the world's [[Ocean]]s for millions of years. Sharks often hunt near a [[Coral Reef]], where [[Fish]] gather in large numbers.",
  "Fish": "Fish are cold-blooded animals that breathe through gills and live throughout the [[Ocean]] and freshwater [[River]]s. Many species shelter among a [[Coral Reef]], while others are hunted by the [[Shark]].",
  "Guitar": "The guitar is a stringed instrument played by plucking or strumming, central to countless styles of [[Music]]. As a [[String Instrument]], it produces tone through vibration that travels as a [[Sound Wave]].",
  "String Instrument": "A string instrument produces sound through vibrating strings, stretched across a body and plucked, bowed, or struck. The [[Guitar]] and [[Piano]] both belong to this broad family used throughout [[Music]] and the [[Orchestra]].",
  "Sound Wave": "A sound wave is a vibration that travels through air or water, carrying the tones we hear as [[Music]]. Instruments like the [[Guitar]] and [[String Instrument]] family generate these waves through physical vibration.",
  "Chocolate": "Chocolate is a food made from roasted and ground [[Cacao]] beans, prized worldwide for its rich flavor. Cacao trees grow naturally in humid [[Rainforest]] regions near the equator.",
  "Cacao": "Cacao is the tropical tree bean used to make [[Chocolate]], traditionally grown in the shaded understory of the [[Rainforest]] along rivers like the [[Amazon River]].",
  "Rainforest": "A rainforest is a dense, warm, high-rainfall forest teeming with biodiversity, found in parts of [[Africa]] and along the [[Amazon River]]. It's the natural home of the [[Cacao]] tree, source of [[Chocolate]].",
  "Amazon River": "The Amazon River is the largest river by water volume on Earth, winding through the South American [[Rainforest]]. Its basin supports countless species and feeds smaller streams that lead to a [[Waterfall]] or join the wider [[River]] network.",
  "River": "A river is a natural flowing body of freshwater that carves through landscapes on its way to the sea. Rivers like the [[Amazon River]] can plunge over cliffs to form a [[Waterfall]], and are often fed by melting [[Glacier]] ice.",
  "Waterfall": "A waterfall forms where a [[River]] drops suddenly over a cliff or steep ledge, often carved by erosion over thousands of years. Many are fed by glacial melt from a retreating [[Glacier]].",
  "Glacier": "A glacier is a massive, slow-moving body of ice formed from compacted snow over centuries. Glaciers carve valleys, feed rivers, and are a lasting remnant of the last [[Ice Age]].",
  "Ice Age": "An ice age is a long period of the Earth's history marked by widespread [[Glacier]] growth and cooler global temperatures. The most recent ice age saw the rise of the woolly [[Mammoth]], whose remains are still found today as [[Fossil]] evidence.",
  "Mammoth": "The mammoth was a large, tusked relative of the modern [[Elephant]] that thrived during the [[Ice Age]]. Frozen remains preserved in ice have yielded remarkably intact [[Fossil]] specimens, including tusks made of [[Ivory]].",
  "Fossil": "A fossil is the preserved remains or trace of an ancient organism, often found in rock or ice from the [[Ice Age]]. Fossils of creatures like the [[Mammoth]] help scientists understand life on ancient [[Africa]] and beyond."
};

const TOPICS = Object.keys(GRAPH);

let state = { target: null, current: null, trail: [], clicks: 0, won: false };

const topbar = document.getElementById('topbar');
const articleWrap = document.getElementById('article-wrap');
const toastEl = document.getElementById('toast');

function showToast(msg){
  toastEl.textContent = msg;
  toastEl.classList.add('show');
  setTimeout(()=> toastEl.classList.remove('show'), 1800);
}

function renderStartScreen(){
  topbar.style.display = 'none';
  articleWrap.innerHTML = `
    <div class="center-screen">
      <div class="compass">🧭</div>
      <h2>WIKIQUEST</h2>
      <p>You'll land on a random topic page. Click through its linked words, subject to subject,
      until you reach the destination word. Every click counts — find the shortest trail.</p>
      <div class="btn-row">
        <button class="btn" id="start-btn">Begin quest</button>
      </div>
    </div>
  `;
  document.getElementById('start-btn').addEventListener('click', startGame);
}

function renderTrail(){
  const strip = document.getElementById('trail-strip');
  strip.innerHTML = '';
  state.trail.forEach((title, i)=>{
    const chip = document.createElement('span');
    chip.className = 'trail-chip' + (i === state.trail.length - 1 ? ' current' : '');
    chip.textContent = title;
    strip.appendChild(chip);
    if(i < state.trail.length - 1){
      const dots = document.createElement('span');
      dots.className = 'trail-dots';
      strip.appendChild(dots);
    }
  });
  strip.scrollLeft = strip.scrollWidth;
}

function parseLinks(text){
  return text.replace(/\[\[(.+?)\]\]/g, (m, title) => `<a data-wiki-title="${title}">${title}</a>`);
}

function renderArticle(title){
  document.getElementById('current-title').textContent = title;
  document.getElementById('target-title').textContent = state.target;
  document.getElementById('click-count').textContent = state.clicks + (state.clicks === 1 ? ' click' : ' clicks');
  renderTrail();

  const body = parseLinks(GRAPH[title]);
  articleWrap.innerHTML = `<div id="article"><h1 class="article-title">${title}</h1><p>${body}</p></div>`;

  document.getElementById('article').addEventListener('click', function(e){
    const link = e.target.closest('a');
    if(!link) return;
    e.preventDefault();
    const wikiTitle = link.getAttribute('data-wiki-title');
    if(!wikiTitle || !GRAPH[wikiTitle]){
      showToast("That link isn't part of the trail");
      return;
    }
    handleNavigate(wikiTitle);
  });
}

function handleNavigate(title){
  if(state.won) return;
  state.clicks += 1;
  state.current = title;
  state.trail.push(title);
  renderArticle(title);

  if(title === state.target){
    state.won = true;
    showWin();
  }
}

function showWin(){
  const overlay = document.createElement('div');
  overlay.className = 'win-overlay';
  overlay.innerHTML = `
    <div class="win-card">
      <div class="flag">🏁</div>
      <h2>You reached ${state.target}!</h2>
      <p>Trail complete — nicely navigated.</p>
      <div class="win-stats">
        <div class="row"><span>Clicks taken</span><span>${state.clicks}</span></div>
        <div class="row"><span>Pages visited</span><span>${state.trail.length}</span></div>
      </div>
      <div class="win-trail mono">${state.trail.join(' → ')}</div>
      <div class="btn-row" style="margin-top:18px;">
        <button class="btn" id="again-btn">Play again</button>
      </div>
    </div>
  `;
  document.body.appendChild(overlay);
  document.getElementById('again-btn').addEventListener('click', ()=>{
    overlay.remove();
    startGame();
  });
}

function startGame(){
  const startTitle = TOPICS[Math.floor(Math.random() * TOPICS.length)];
  let target = TOPICS[Math.floor(Math.random() * TOPICS.length)];
  while(target === startTitle){
    target = TOPICS[Math.floor(Math.random() * TOPICS.length)];
  }
  state = { target, current: startTitle, trail: [startTitle], clicks: 0, won: false };
  topbar.style.display = 'block';
  renderArticle(startTitle);
}

renderStartScreen();
</script>
</body>
</html>
