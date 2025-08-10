<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8" />
<meta name="viewport" content="width=device-width,initial-scale=1" />
<title>Human Unity Era — Broadcast</title>
<meta name="color-scheme" content="dark">
<style>
/* ===== Vars & base ===== */
:root{
  --bg:#0a0a0a; --fg:#eaeaea;
  --glowR: rgba(203,62,39,.65);
  --glowR2: rgba(203,62,39,.35);
  --scan: rgba(255,255,255,.16);
  --loop: 40s; /* master timeline length */
}
*{box-sizing:border-box}
html,body{margin:0;background:var(--bg);color:var(--fg);font:400 16px/1.5 system-ui, -apple-system, Segoe UI, Roboto, Arial, sans-serif}
a{color:#00ffe7;text-decoration:none}
a:hover{text-decoration:underline}

/* ===== HERO stage ===== */
.hero{
  position:relative;
  height:100vh; /* full viewport */
  background:#000;
  overflow:hidden;
  border-bottom:1px solid #121212;
}

/* Layers occupy the full hero; timeline windows toggle their opacity */
.layer{position:absolute; inset:0; display:grid; place-items:center; opacity:0}

/* Static + scanlines */
canvas#noise{position:absolute; inset:0; opacity:.12; mix-blend-mode:screen}
.scanlines{
  position:absolute; inset:0; pointer-events:none; opacity:.18;
  background:repeating-linear-gradient(0deg, var(--scan) 0 1px, transparent 1px 3px);
}

/* Dark mask during “signal lost” window */
.deadMask{position:absolute; inset:0; background:#000; opacity:0}

/* ===== Text styles ===== */
.text h1, .text p{margin:0; text-align:center; color:#111}
.text h1{
  font-size:clamp(44px,10vw,140px);
  font-weight:900; letter-spacing:.06em; text-transform:uppercase;
  text-shadow: 0 0 0 #111, 0 0 14px var(--glowR), 0 0 32px var(--glowR2);
  animation:jitter 3s steps(2,end) infinite;
}
.text p{
  font-size:clamp(18px,3.8vw,30px);
  font-weight:800; letter-spacing:.01em; padding:0 6vw;
  text-shadow: 0 0 0 #111, 0 0 12px var(--glowR), 0 0 26px var(--glowR2);
  animation:jitter 3s steps(2,end) infinite;
}

/* tiny “old CRT” micro-drift */
@keyframes jitter{
  0%,100%{transform:translate(0,0)}
  25%{transform:translate(.35px,-.4px)}
  50%{transform:translate(-.45px,.25px)}
  75%{transform:translate(.25px,.35px)}
}

/* ===== Emblem images ===== */

/* faint emblem behind all text (subtle breathing) */
.ghostBehind img{
  width:min(68vw,820px); height:auto; opacity:.065;
  filter:grayscale(100%) contrast(120%) brightness(.7);
  animation:ghostBreath var(--loop) linear infinite;
}
@keyframes ghostBreath{
  0%,10%{opacity:.04} 12%,20%{opacity:.08} 22%,36%{opacity:.06}
  38%,48%{opacity:.09} 50%,60%{opacity:.05} 62%,70%{opacity:.07}
  72%,80%{opacity:.05} 82%,100%{opacity:.04}
}

/* main emblem and ghost emblem sizing */
.emblemMain img, .ghostEmblem img{
  width:min(70vw,860px); height:auto; object-fit:contain;
  filter:contrast(115%) saturate(115%) brightness(.9);
}

/* breakthrough feels like power surging with slight warp */
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

/* dead-static window: weak→strong x3→weak ghost */
@keyframes ghostSeq{
  0%,8%   {opacity:.07; filter:grayscale(100%) saturate(.7)}
  10%,26% {opacity:.18; filter:grayscale(70%) saturate(.9)}
  28%,44% {opacity:.24; filter:saturate(1.1) brightness(1.02)}
  46%,62% {opacity:.30; filter:saturate(1.15)}
  64%,76% {opacity:.08; filter:grayscale(100%) saturate(.7)}
  100%    {opacity:.07}
}

/* ===== Master timeline windows (seconds are approximate) =====
   0–4:  TIME’S UP
   4–9:  Slide 1
   9–16: Slide 2 (7s hold)
  16–22: Emblem breakthrough
  22–29: Dead-static + ghost emblem (7s), then loop
*/
.winTS   { animation: winTS   var(--loop) linear infinite }
.winS1   { animation: winS1   var(--loop) linear infinite }
.winS2   { animation: winS2   var(--loop) linear infinite }
.winEm   { animation: winEm   var(--loop) linear infinite }
.winDead { animation: winDead var(--loop) linear infinite }

@keyframes winTS   { 0%,10%{opacity:1}   10.1%,100%{opacity:0} }
@keyframes winS1   { 10%,22.5%{opacity:1} 0%,9.9%,22.6%,100%{opacity:0} }
@keyframes winS2   { 22.5%,40%{opacity:1} 0%,22.4%,40.1%,100%{opacity:0} }
@keyframes winEm   { 40%,55%{opacity:1}  0%,39.9%,55.1%,100%{opacity:0} }
@keyframes winDead { 55%,72.5%{opacity:1} 0%,54.9%,72.6%,100%{opacity:0} }

/* text fade-ins inside their windows (slightly slower → “breaking through”) */
.ts h1{ animation: tsIn var(--loop) linear infinite, jitter 3s steps(2,end) infinite }
@keyframes tsIn{ 0%,1%{opacity:0; filter:blur(6px)} 2%,10%{opacity:1; filter:blur(0)} 10.1%,100%{opacity:0} }

.s1 p{ animation: s1In var(--loop) linear infinite }
@keyframes s1In{ 10%,12%{opacity:0; filter:blur(6px)} 12.6%,22.5%{opacity:1; filter:blur(0)} 22.6%,100%{opacity:0} }

.s2 p{ animation: s2In var(--loop) linear infinite }
@keyframes s2In{ 22.5%,24%{opacity:0; filter:blur(6px)} 24.6%,40%{opacity:1; filter:blur(0)} 40.1%,100%{opacity:0} }

/* emblem breakthrough animation applied */
.emblemMain img{ animation: emblemPulse var(--loop) linear infinite, emblemGlitch 2.8s steps(2,end) infinite }

/* darker static during the dead window */
.deadMask{ animation: deadWin var(--loop) linear infinite }
@keyframes deadWin{ 55%,72.5%{opacity:.85} 0%,54.9%,72.6%,100%{opacity:0} }

/* ghost emblem during dead static */
.ghostEmblem img{ animation: ghostSeq var(--loop) linear infinite }

/* ===== Optional: tiny footer so page scroll feels normal ===== */
footer{position:relative;padding:14px 18px;color:#9a9a9a;border-top:1px solid #121212}
</style>
</head>
<body>

<!-- ===== HERO ===== -->
<section class="hero" aria-label="Emergency broadcast">
  <!-- live static -->
  <canvas id="noise"></canvas>
  <div class="scanlines"></div>

  <!-- faint emblem behind everything -->
  <div class="layer ghostBehind" style="opacity:1">
    <img src="images/emblem-burnt.jpeg" alt="HUE emblem (ghost background)">
  </div>

  <!-- 0–4s: TIME'S UP -->
  <div class="layer winTS ts">
    <div class="text"><h1>TIME’S UP</h1></div>
  </div>

  <!-- 4–9s: slide 1 -->
  <div class="layer winS1 s1">
    <div class="text"><p>We are the people. We are the majority.<br/>We are the ones that matter.</p></div>
  </div>

  <!-- 9–16s: slide 2 (7s hold) -->
  <div class="layer winS2 s2">
    <div class="text"><p>The time of allowing ego-driven “world leaders” to lead us into war for profit is over.<br/>We, the people of the world, will have the final say.</p></div>
  </div>

  <!-- 16–22s: emblem breakthrough (warm pulses + micro warp) -->
  <div class="layer winEm emblemMain">
    <img src="images/emblem-burnt.jpeg" alt="Human Unity Era emblem">
  </div>

  <!-- 22–29s: dead static + ghost emblem sequence -->
  <div class="deadMask" aria-hidden="true"></div>
  <div class="layer winDead ghostEmblem">
    <img src="images/emblem-burnt.jpeg" alt="HUE emblem (ghost in static)">
  </div>
</section>

<footer>© 2025 Human Unity Era</footer>

<script>
/* ===== Retro static generator (no external assets) ===== */
(function(){
  const c = document.getElementById('noise');
  const ctx = c.getContext('2d', { willReadFrequently: false });

  function fit(){
    const r = c.parentElement.getBoundingClientRect();
    c.width = Math.max(320, r.width|0);
    c.height = Math.max(180, r.height|0);
  }
  addEventListener('resize', fit, {passive:true});
  fit();

  function frame(){
    const w=c.width, h=c.height;
    if (!w || !h) { requestAnimationFrame(frame); return; }

    const id = ctx.createImageData(w,h);
    const d = id.data;
    // noisy luminance with light alpha, fast enough for mobile
    for (let i=0;i<d.length;i+=4){
      const v = (Math.random()*255)|0;
      d[i]=d[i+1]=d[i+2]=v; d[i+3]=215;
    }
    ctx.putImageData(id,0,0);

    // occasional brighter scanline
    if (Math.random()<.30){
      const y=(Math.random()*h)|0;
      ctx.fillStyle='rgba(255,255,255,.12)';
      ctx.fillRect(0,y,w,2);
    }
    requestAnimationFrame(frame);
  }
  frame();
})();

/* Debug version tag so we know which build is live */
console.log('HUE broadcast build v=101');
</script>
</body>
</html>
