<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Human Unity Era</title>
<style>
:root{
  --bg:#0b0b0b; --fg:#f2f2f2; --accent:#00ffe7; --danger:#ff2e2e; --muted:#9a9a9a;
  --loop:40s; /* master timeline */
}
*{box-sizing:border-box}
html,body{margin:0;background:var(--bg);color:var(--fg);font-family:Inter,system-ui,Arial,sans-serif;line-height:1.6}
a{color:var(--accent);text-decoration:none} a:hover{text-decoration:underline}
.container{max-width:960px;margin:0 auto;padding:24px}

/* ======= HERO (VHS emergency broadcast) ======= */
.hero{position:relative;aspect-ratio:16/9;background:#000;overflow:hidden;border-bottom:1px solid #171717}
.layer{position:absolute;inset:0;display:grid;place-items:center;opacity:0}
canvas#noise{position:absolute;inset:0;opacity:.12;mix-blend-mode:screen}
.scanlines{position:absolute;inset:0;pointer-events:none;opacity:.18;
  background:repeating-linear-gradient(0deg,rgba(255,255,255,.16) 0 1px, transparent 1px 3px)}
.deadMask{position:absolute;inset:0;background:#000;opacity:0}

/* Text styles */
.text h1,.text p{margin:0;text-align:center;color:#111}
.text h1{
  font-size:clamp(44px,10vw,140px);font-weight:900;letter-spacing:.06em;text-transform:uppercase;
  text-shadow:0 0 0 #111,0 0 14px rgba(203,62,39,.65),0 0 32px rgba(203,62,39,.38);
  animation:jitter 3s steps(2,end) infinite;
}
.text p{
  font-size:clamp(18px,3.8vw,30px);font-weight:800;letter-spacing:.01em;padding:0 6vw;
  text-shadow:0 0 0 #111,0 0 12px rgba(203,62,39,.58),0 0 26px rgba(203,62,39,.32);
  animation:jitter 3s steps(2,end) infinite;
}
@keyframes jitter{
  0%,100%{transform:translate(0,0)}
  25%{transform:translate(.35px,-.4px)}
  50%{transform:translate(-.45px,.25px)}
  75%{transform:translate(.25px,.35px)}
}

/* Ghost emblem behind text (very faint) */
.ghostBehind img{
  width:min(68vw,820px); height:auto; opacity:.065;
  filter:grayscale(100%) contrast(120%) brightness(.7);
  animation:ghostBreath var(--loop) linear infinite;
}
@keyframes ghostBreath{
  0%,10%{opacity:.04} 12%,20%{opacity:.08} 22%,36%{opacity:.06} 38%,48%{opacity:.09}
  50%,60%{opacity:.05} 62%,70%{opacity:.07} 72%,80%{opacity:.05} 82%,100%{opacity:.04}
}

/* Final emblem (breakthrough) */
.emblemMain img, .ghostEmblem img{
  width:min(70vw,860px); height:auto; object-fit:contain;
  filter:contrast(115%) saturate(115%) brightness(.9);
}
/* Warm pulses + warp/flicker during breakthrough (~16–22s) */
@keyframes emblemPulse{
  0%,35% {filter:grayscale(60%) saturate(.7) brightness(.85); opacity:.55; transform:scale(1)}
  37%,52%{filter:saturate(1.20) brightness(1.05); opacity:.95; transform:scale(1.008) rotate(0.15deg)}
  54%,69%{filter:grayscale(45%) saturate(.85) brightness(.9); opacity:.7; transform:scale(.998)}
  71%,86%{filter:saturate(1.25) brightness(1.08); opacity:1; transform:scale(1.012) rotate(-0.12deg)}
  88%,100%{filter:saturate(1.05) brightness(.98); opacity:.85; transform:scale(1)}
}
@keyframes emblemGlitch{
  0%,100%{transform:translate(0,0)}
  45%{transform:translate(-.4px,.4px)}
  55%{transform:translate(.6px,-.5px)}
}

/* Dead static window (22–29s) with ghost emblem: weak → strong x3 → weak */
@keyframes ghostSeq{
  0%,8%   {opacity:.07; filter:grayscale(100%) saturate(.7)}
  10%,26% {opacity:.18; filter:grayscale(70%) saturate(.9)}
  28%,44% {opacity:.24; filter:saturate(1.1) brightness(1.02)}
  46%,62% {opacity:.3;  filter:saturate(1.15)}
  64%,76% {opacity:.08; filter:grayscale(100%) saturate(.7)}
  100%    {opacity:.07}
}

/* Master timeline windows */
.winTS   { animation: winTS   var(--loop) linear infinite }   /* ~0–4s */
.winT1   { animation: winT1   var(--loop) linear infinite }   /* ~4–9s */
.winT2   { animation: winT2   var(--loop) linear infinite }   /* ~9–16s (7s hold) */
.winEm   { animation: winEm   var(--loop) linear infinite }   /* ~16–22s */
.winDead { animation: winDead var(--loop) linear infinite }   /* ~22–29s */

@keyframes winTS   { 0%,10%{opacity:1}  10.1%,100%{opacity:0} }
@keyframes winT1   { 10%,22.5%{opacity:1} 0%,9.9%,22.6%,100%{opacity:0} }
@keyframes winT2   { 22.5%,40%{opacity:1} 0%,22.4%,40.1%,100%{opacity:0} }
@keyframes winEm   { 40%,55%{opacity:1} 0%,39.9%,55.1%,100%{opacity:0} }
@keyframes winDead { 55%,72.5%{opacity:1} 0%,54.9%,72.6%,100%{opacity:0} }

/* Text entrance timing within windows */
.ts h1{ animation: tsIn var(--loop) linear infinite, jitter 3s steps(2,end) infinite }
@keyframes tsIn{ 0%,1%{opacity:0; filter:blur(6px)} 2%,10%{opacity:1; filter:blur(0)} 10.1%,100%{opacity:0} }

.t1 p{ animation: t1In var(--loop) linear infinite }
@keyframes t1In{ 10%,12%{opacity:0; filter:blur(6px)} 12.5%,22.5%{opacity:1; filter:blur(0)} 22.6%,100%{opacity:0} }

.t2 p{ animation: t2In var(--loop) linear infinite }
@keyframes t2In{ 22.5%,24%{opacity:0; filter:blur(6px)} 24.5%,40%{opacity:1; filter:blur(0)} 40.1%,100%{opacity:0} }

/* Emblem breakthrough effects */
.emblemMain img{ animation: emblemPulse var(--loop) linear infinite, emblemGlitch 2.8s steps(2,end) infinite }

/* Darker end static during dead window */
.deadMask{ animation: deadWin var(--loop) linear infinite }
@keyframes deadWin{ 55%,72.5%{opacity:.8} 0%,54.9%,72.6%,100%{opacity:0} }

/* Ghost emblem during dead static */
.ghostEmblem img{ animation: ghostSeq var(--loop) linear infinite }

/* Basic site framing below */
header.site{position:sticky;top:0;background:linear-gradient(180deg,#0b0b0be6,#0b0b0bb0 55%,#0b0b0b00);border-bottom:1px solid #151515;z-index:2}
header .brand{display:flex;align-items:center;gap:10px}
.badge{display:inline-block;padding:.15rem .5rem;border:1px solid #2a2a2a;border-radius:999px;font-size:.75rem;color:var(--muted);margin-left:.5rem}
.nav{display:flex;gap:14px;flex-wrap:wrap;margin-top:8px}
.nav a{position:relative}
.nav a::after{content:'';position:absolute;left:0;bottom:-2px;width:100%;height:2px;background:var(--accent);transform:scaleX(0);transform-origin:left;transition:transform .2s}
.nav a:hover::after{transform:scaleX(1)}
h1,h2{font-family:'Courier New',monospace;text-transform:uppercase;letter-spacing:.5px}
h2{color:var(--accent);margin:24px 0 12px}
ul.toc{list-style:none;padding-left:0}
ul.toc>li{margin:12px 0;padding:12px;border:1px solid #1f1f1f;border-radius:10px;background:#0f0f0fbd;backdrop-filter:saturate(120%) blur(2px)}
.btn{display:inline-block;padding:.5rem .85rem;border:1px solid var(--accent);border-radius:8px;color:var(--accent);transition:transform .15s ease, background .15s ease, color .15s ease}
.btn:hover{transform:translateY(-1px);background:var(--accent);color:#041a18;text-decoration:none}
footer{border-top:1px solid #151515;color:var(--muted);margin-top:32px;padding:18px}
</style>
</head>
<body>

<header class="site">
  <div class="container">
    <div class="brand">
      <h1>🌍 Human Unity Era</h1>
      <span class="badge">Civilian System Override</span>
    </div>
    <nav class="nav small">
      <a href="#">Home</a>
      <a href="#">Declaration</a>
      <a href="#">Systems</a>
      <a href="#">Framework</a>
      <a href="#">Tools</a>
    </nav>
  </div>
</header>

<section class="hero" aria-label="Emergency broadcast">
  <!-- Retro static -->
  <canvas id="noise"></canvas>
  <div class="scanlines"></div>

  <!-- Ghost emblem behind text (very faint) -->
  <div class="layer ghostBehind" style="opacity:1">
    <img src="images/emblem-burnt.png" alt="HUE emblem (ghost)">
  </div>

  <!-- TIMES UP -->
  <div class="layer winTS ts">
    <div class="text"><h1>TIME’S UP</h1></div>
  </div>

  <!-- Message slide 1 -->
  <div class="layer winT1 t1">
    <div class="text"><p>We are the people. We are the majority.<br/>We are the ones that matter.</p></div>
  </div>

  <!-- Message slide 2 (7s hold) -->
  <div class="layer winT2 t2">
    <div class="text"><p>The time of allowing ego-driven “world leaders” to lead us into war for profit is over.<br/>We, the people of the world, will have the final say.</p></div>
  </div>

  <!-- Emblem breakthrough pulses -->
  <div class="layer winEm emblemMain">
    <img src="images/emblem-burnt.png" alt="Human Unity Era emblem">
  </div>

  <!-- Dead static window (7s) -->
  <div class="deadMask"></div>
  <div class="layer winDead ghostEmblem">
    <img src="images/emblem-burnt.png" alt="HUE emblem (ghost flicker)">
  </div>
</section>

<main class="container">
  <p class="small">Civilian-led. Peace-driven. AI-readable.</p>
  <p><b>Created by Kaileb Abrey-Blackmore</b><br><i>Civilian Architect of Global Coordination</i></p>

  <h2>🏠 Welcome</h2>
  <p>Welcome to the official Human Unity Era digital platform — a transparent, accessible framework designed to rebuild global cooperation, dignity, and peace from the ground up.</p>
  <p><b>This is not a petition. It is a <span style="color:var(--accent)">blueprint</span>.</b></p>

  <h2>📚 Explore the Framework</h2>
  <p>Click a section below to explore the core components of the Human Unity Era. Each section links to a separate subpage.</p>
  <ul class="toc">
    <li><b>🕊️ The Human Unity Era Declaration</b><br><span class="small">• Founding Declaration • Author Statement • One-Page Overview</span><div><a class="btn" href="#">Open Section</a></div></li>
    <li><b>⚙️ Systems of Civilian Power</b><br><span class="small">• Governance Charter • CEWIC Proposal • Treaty-born Institutions (e.g., MPOB)</span><div><a class="btn" href="#">Open Section</a></div></li>
    <li><b>🌍 Ukraine–Russia Peace Framework</b><br><span class="small">• UN-style Treaty • Post-Treaty Agenda • Peace Dividend • Sanctions & Reconstruction</span><div><a class="btn" href="#">Open Section</a></div></li>
    <li><b>💡 Civilian Involvement Tools</b><br><span class="small">• Join the Movement • Protest Toolkit • Posters, Flyers, Templates</span><div><a class="btn" href="#">Open Section</a></div></li>
  </ul>

  <h2>📬 Contact + Media</h2>
  <p>📧 Email: <a href="mailto:humanunityera@gmail.com">humanunityera@gmail.com</a><br>
     🎥 TikTok: <i>(Paste video or link here)</i><br>
     📰 Press Kit: <i>(Link or upload your PDF)</i></p>

  <h2>✊ Join the Movement</h2>
  <ul>
    <li>Translate or share the documents</li>
    <li>Test the CEWIC model</li>
    <li>Join our Founding Civilian circle</li>
  </ul>
  <p><a class="btn" href="#">[Sign-Up Form Link Here]</a></p>
</main>

<footer>
  <div class="container">© 2025 Human Unity Era — Built by civilians, for civilians.</div>
</footer>

<script>
/* Retro static inside hero (no external files) */
(function(){
  const c = document.getElementById('noise');
  const ctx = c.getContext('2d');
  function fit(){ const r=c.parentElement.getBoundingClientRect(); c.width=r.width; c.height=r.height }
  addEventListener('resize', fit, {passive:true}); fit();
  function frame(){
    const w=c.width, h=c.height;
    if (!w || !h) { requestAnimationFrame(frame); return; }
    const id = ctx.createImageData(w,h);
    const d = id.data;
    for (let i=0;i<d.length;i+=4){
      const v = (Math.random()*255)|0;
      d[i]=d[i+1]=d[i+2]=v; d[i+3]=215;
    }
    ctx.putImageData(id,0,0);
    if (Math.random()<.3){
      const y=(Math.random()*h)|0;
      ctx.fillStyle='rgba(255,255,255,.12)';
      ctx.fillRect(0,y,w,2);
    }
    requestAnimationFrame(frame);
  }
  frame();
})();
</script>
</body>
</html>
