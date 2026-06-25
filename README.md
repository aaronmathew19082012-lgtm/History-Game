# History-Game
Great Game
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Sweat Tycoon</title>
<script src="https://cdn.tailwindcss.com"></script>
<link href="https://fonts.googleapis.com/css2?family=Orbitron:wght@400;700;900&family=Space+Grotesk:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.5.0/css/all.min.css">
<script>
tailwind.config={theme:{extend:{fontFamily:{display:['Orbitron','monospace'],body:['Space Grotesk','sans-serif']}}}}
</script>
<style>
:root{--bg:#0d0a06;--card:#1a1510;--card-hover:#231d15;--border:#2d2518;--fg:#f0e6d8;--muted:#8a7e6e;--accent:#FFB300;--accent2:#FF6F00;--purple:#7C4DFF;--purple-light:#B388FF}
*{margin:0;padding:0;box-sizing:border-box}
body{font-family:'Space Grotesk',sans-serif;background:var(--bg);color:var(--fg);min-height:100vh;overflow-x:hidden}
::-webkit-scrollbar{width:6px}::-webkit-scrollbar-track{background:var(--bg)}::-webkit-scrollbar-thumb{background:var(--border);border-radius:3px}

#bg-canvas{position:fixed;inset:0;z-index:0;pointer-events:none}
.bg-glow{position:fixed;border-radius:50%;filter:blur(130px);pointer-events:none;z-index:0}
.bg-glow-1{width:500px;height:500px;background:rgba(255,143,0,0.06);top:-120px;right:-100px}
.bg-glow-2{width:400px;height:400px;background:rgba(255,111,0,0.04);bottom:-80px;left:-80px}

/* Character Container */
.char-scene{position:relative;width:280px;height:380px;cursor:pointer;user-select:none;-webkit-tap-highlight-color:transparent;outline:none}
.char-scene:focus-visible{outline:3px solid var(--accent);outline-offset:8px;border-radius:12px}

/* Whip */
.whip-arm{position:absolute;top:62px;left:168px;width:180px;height:180px;transform-origin:0% 20%;transition:transform 0.06s ease-out;z-index:5}
.whip-arm.cracking{transform:rotate(-55deg)}
.whip-arm.idle{transform:rotate(-15deg);animation:whipIdle 3s ease-in-out infinite}
@keyframes whipIdle{0%,100%{transform:rotate(-12deg)}50%{transform:rotate(-18deg)}}
.whip-handle{position:absolute;top:0;left:0;width:8px;height:50px;background:linear-gradient(90deg,#5D4037,#795548,#5D4037);border-radius:3px;transform:rotate(8deg)}
.whip-cord{position:absolute;top:44px;left:4px;width:3px;height:130px;background:linear-gradient(180deg,#4E342E,#3E2723);border-radius:2px;transform-origin:top center;transform:rotate(3deg)}
.whip-cord::after{content:'';position:absolute;bottom:-4px;left:-3px;width:9px;height:12px;background:#3E2723;border-radius:0 0 50% 50%;transform:rotate(-2deg)}

/* Crack Effect */
.crack-flash{position:absolute;top:30px;right:20px;width:60px;height:60px;opacity:0;pointer-events:none;z-index:6}
.crack-flash.active{animation:crackBurst 0.25s ease-out forwards}
@keyframes crackBurst{0%{opacity:1;transform:scale(0.3)}50%{opacity:0.8;transform:scale(1.2)}100%{opacity:0;transform:scale(1.5)}}

/* Sweat Drops */
.sweat-drop{position:absolute;width:8px;height:12px;background:radial-gradient(ellipse at 40% 30%,rgba(130,210,255,0.85),rgba(100,180,240,0.3));border-radius:50% 50% 50% 50%/60% 60% 40% 40%;opacity:0;pointer-events:none;z-index:4}
.sweat-drop.active{animation:sweatDrop 0.7s ease-in forwards}
@keyframes sweatDrop{0%{opacity:1;transform:translateY(0) scale(1)}100%{opacity:0;transform:translateY(40px) scale(0.5)}}

/* Float Text */
.float-text{position:absolute;pointer-events:none;font-family:'Orbitron',monospace;font-weight:700;color:var(--accent);text-shadow:0 0 10px rgba(255,179,0,0.5);animation:floatUp 1s ease-out forwards;z-index:50;font-size:14px}
@keyframes floatUp{0%{opacity:1;transform:translateY(0) scale(1)}100%{opacity:0;transform:translateY(-80px) scale(0.5)}}

/* Sweat Particles */
.sweat-particle{position:absolute;pointer-events:none;width:5px;height:7px;background:radial-gradient(ellipse,rgba(130,210,255,0.7),transparent);border-radius:50%;z-index:40}
@keyframes particleFly{0%{opacity:1;transform:translate(0,0) scale(1)}100%{opacity:0;transform:translate(var(--dx),var(--dy)) scale(0.1)}}

/* Cards */
.game-card{background:var(--card);border:1px solid var(--border);border-radius:12px;transition:border-color 0.2s,background 0.2s}
.game-card:hover{border-color:rgba(255,179,0,0.15)}

/* Upgrade Card */
.upgrade-card{padding:12px 14px;cursor:pointer;position:relative;overflow:hidden;transition:background 0.15s}
.upgrade-card:not(.locked):hover{background:var(--card-hover)}
.upgrade-card.locked{opacity:0.45;cursor:not-allowed}
.upgrade-card .buy-btn{transition:all 0.15s}
.upgrade-card:not(.locked):hover .buy-btn{background:var(--accent);color:#000}

/* Tabs */
.tab-btn{padding:10px 18px;border-radius:8px 8px 0 0;font-family:'Space Grotesk',sans-serif;font-weight:600;font-size:13px;border:1px solid transparent;border-bottom:none;cursor:pointer;transition:all 0.2s;background:transparent;color:var(--muted)}
.tab-btn:hover{color:var(--fg);background:rgba(255,179,0,0.05)}
.tab-btn.active{color:var(--accent);background:var(--card);border-color:var(--border)}

/* Rebirth */
.rebirth-btn{position:relative;overflow:hidden;background:linear-gradient(135deg,#7C4DFF,#536DFE);border:2px solid rgba(124,77,255,0.4);border-radius:12px;padding:14px 24px;cursor:pointer;transition:all 0.2s;color:#fff;font-family:'Space Grotesk',sans-serif;font-weight:700;font-size:15px}
.rebirth-btn:hover:not(.locked){transform:translateY(-2px);box-shadow:0 8px 30px rgba(124,77,255,0.3)}
.rebirth-btn:active:not(.locked){transform:translateY(0)}
.rebirth-btn.locked{opacity:0.4;cursor:not-allowed;filter:grayscale(0.5)}
.rebirth-btn::before{content:'';position:absolute;top:-50%;left:-50%;width:200%;height:200%;background:conic-gradient(transparent,rgba(255,255,255,0.05),transparent 30%);animation:rbSpin 4s linear infinite}
@keyframes rbSpin{to{transform:rotate(360deg)}}

/* Slot Machine */
.slot-machine{background:linear-gradient(180deg,#1a1510,#231d15,#1a1510);border:3px solid var(--accent);border-radius:16px;padding:20px;box-shadow:0 0 40px rgba(255,179,0,0.1);position:relative}
.slot-machine::before{content:'';position:absolute;top:-2px;left:20%;right:20%;height:4px;background:linear-gradient(90deg,transparent,var(--accent),transparent);border-radius:2px}
.slot-reel-window{width:80px;height:90px;overflow:hidden;background:#0a0806;border:2px solid var(--border);border-radius:8px;position:relative}
.slot-reel-window::before,.slot-reel-window::after{content:'';position:absolute;left:0;right:0;height:25px;z-index:2;pointer-events:none}
.slot-reel-window::before{top:0;background:linear-gradient(#0a0806,transparent)}
.slot-reel-window::after{bottom:0;background:linear-gradient(transparent,#0a0806)}
.slot-reel-strip{display:flex;flex-direction:column;align-items:center;transition:transform 0.1s}
.slot-reel-strip .sym{width:80px;height:90px;display:flex;align-items:center;justify-content:center;font-size:38px;flex-shrink:0}
.spin-btn{background:linear-gradient(180deg,#FFB300,#E65100);border:none;border-radius:10px;padding:12px 36px;font-family:'Orbitron',monospace;font-weight:900;font-size:15px;color:#000;cursor:pointer;transition:all 0.15s;text-transform:uppercase;letter-spacing:2px}
.spin-btn:hover:not(:disabled){transform:translateY(-2px);box-shadow:0 6px 25px rgba(255,179,0,0.4)}
.spin-btn:disabled{opacity:0.4;cursor:not-allowed}

/* Toasts */
.toast{padding:12px 18px;border-radius:10px;font-size:13px;font-weight:500;animation:toastIn 0.3s ease-out,toastOut 0.3s 2.7s ease-in forwards;pointer-events:none;max-width:340px;box-shadow:0 8px 30px rgba(0,0,0,0.5)}
@keyframes toastIn{from{opacity:0;transform:translateX(40px)}to{opacity:1;transform:translateX(0)}}
@keyframes toastOut{from{opacity:1}to{opacity:0;transform:translateX(40px)}}
.toast-success{background:linear-gradient(135deg,#1b5e20,#2e7d32);border:1px solid #4caf50;color:#c8e6c9}
.toast-error{background:linear-gradient(135deg,#b71c1c,#c62828);border:1px solid #ef5350;color:#ffcdd2}
.toast-info{background:linear-gradient(135deg,#e65100,#ff8f00);border:1px solid #ffb300;color:#fff8e1}
.toast-legendary{background:linear-gradient(135deg,#4a148c,#7b1fa2);border:1px solid #ce93d8;color:#f3e5f5}

/* Flash overlays */
.flash-overlay{position:fixed;inset:0;z-index:999;pointer-events:none;animation:flashA 1.5s ease-out forwards}
@keyframes flashA{0%{background:rgba(206,147,216,0.7)}30%{background:rgba(206,147,216,0.2)}100%{background:transparent}}
.rebirth-flash{position:fixed;inset:0;z-index:998;pointer-events:none;animation:rbFlash 1s ease-out forwards}
@keyframes rbFlash{0%{background:rgba(124,77,255,0.5)}40%{background:rgba(124,77,255,0.1)}100%{background:transparent}}

.stat-pulse{animation:sPulse 0.25s ease-out}
@keyframes sPulse{0%{transform:scale(1.08)}100%{transform:scale(1)}}

/* Whip indicator on character */
.equipped-whip-label{position:absolute;bottom:8px;left:50%;transform:translateX(-50%);font-size:10px;font-weight:700;color:var(--accent);background:rgba(0,0,0,0.6);padding:2px 10px;border-radius:20px;white-space:nowrap;z-index:10;border:1px solid rgba(255,179,0,0.3)}

@media(max-width:768px){
  .char-scene{width:220px;height:300px;transform:scale(0.85);transform-origin:top center}
  .slot-reel-window{width:66px;height:74px}
  .slot-reel-strip .sym{width:66px;height:74px;font-size:30px}
  .tab-btn{padding:8px 12px;font-size:11px}
}
@media(prefers-reduced-motion:reduce){*,*::before,*::after{animation-duration:0.01ms!important;transition-duration:0.01ms!important}}
</style>
</head>
<body class="font-body">

<div class="bg-glow bg-glow-1"></div>
<div class="bg-glow bg-glow-2"></div>
<canvas id="bg-canvas"></canvas>

<!-- Header -->
<header class="relative z-10 border-b border-border bg-bg/80 backdrop-blur-md sticky top-0" role="banner">
  <div class="max-w-6xl mx-auto px-4 py-3 flex items-center justify-between flex-wrap gap-2">
    <h1 class="font-display font-900 text-lg md:text-2xl tracking-wider" style="background:linear-gradient(135deg,#FFB300,#FF6F00);-webkit-background-clip:text;-webkit-text-fill-color:transparent">SWEAT TYCOON</h1>
    <div class="flex items-center gap-3 md:gap-5 text-xs md:text-sm flex-wrap">
      <div class="flex items-center gap-1.5"><i class="fa-solid fa-droplet text-sky-400"></i><span class="text-muted">Sweat:</span><span id="stat-sweat" class="font-display font-700 text-sky-300">0</span></div>
      <div class="flex items-center gap-1.5"><i class="fa-solid fa-bolt text-yellow-400"></i><span class="text-muted">/sec:</span><span id="stat-ps" class="font-display font-700 text-yellow-300">0</span></div>
      <div class="flex items-center gap-1.5"><i class="fa-solid fa-hand-pointer text-orange-400"></i><span class="text-muted">/whip:</span><span id="stat-pc" class="font-display font-700 text-orange-300">1</span></div>
      <div class="flex items-center gap-1.5"><i class="fa-solid fa-rotate text-purple-400"></i><span class="text-muted">Rebirths:</span><span id="stat-rebirths" class="font-display font-700 text-purple-300">0</span></div>
    </div>
  </div>
</header>

<!-- Main -->
<main class="relative z-10 max-w-6xl mx-auto px-4 py-5 flex flex-col lg:flex-row gap-6" role="main">

  <!-- Left: Character -->
  <section class="lg:w-5/12 flex flex-col items-center gap-5" aria-label="Whip the character">
    <div class="text-center">
      <p class="text-muted text-xs mb-0.5 font-medium">Total Sweat Points</p>
      <p id="main-sweat" class="font-display font-900 text-3xl md:text-4xl text-sky-300" style="text-shadow:0 0 18px rgba(100,200,255,0.3)">0</p>
    </div>

    <!-- Character Scene -->
    <div class="char-scene" id="char-scene" role="button" tabindex="0" aria-label="Whip to generate sweat points">

      <!-- Character SVG -->
      <svg viewBox="0 0 280 380" width="280" height="380" style="position:absolute;top:0;left:0;z-index:2">
        <!-- Ground shadow -->
        <ellipse cx="140" cy="370" rx="70" ry="8" fill="rgba(0,0,0,0.3)"/>

        <!-- Back leg -->
        <rect x="118" y="290" width="28" height="75" rx="10" fill="#1a1a1a"/>
        <ellipse cx="132" cy="365" rx="20" ry="10" fill="#111"/>

        <!-- Front leg -->
        <rect x="148" y="285" width="28" height="80" rx="10" fill="#1a1a1a"/>
        <ellipse cx="162" cy="365" rx="20" ry="10" fill="#111"/>

        <!-- Shorts -->
        <path d="M105,265 Q100,262 98,280 L96,300 Q96,310 110,310 L130,308 L130,265 Z" fill="#1a1a1a"/>
        <path d="M175,265 Q180,262 182,280 L184,300 Q184,310 170,310 L150,308 L150,265 Z" fill="#1a1a1a"/>
        <rect x="100" y="252" width="80" height="22" rx="8" fill="#1a1a1a"/>

        <!-- Torso -->
        <path d="M108,140 Q104,145 102,180 L100,250 Q100,262 112,262 L168,262 Q180,262 180,250 L178,180 Q176,145 172,140 Z" fill="#8D5524"/>

        <!-- Six pack abs -->
        <line x1="140" y1="170" x2="140" y2="250" stroke="#7A4A1E" stroke-width="1.2" opacity="0.6"/>
        <line x1="118" y1="190" x2="162" y2="190" stroke="#7A4A1E" stroke-width="1" opacity="0.4"/>
        <line x1="118" y1="210" x2="162" y2="210" stroke="#7A4A1E" stroke-width="1" opacity="0.4"/>
        <line x1="118" y1="230" x2="162" y2="230" stroke="#7A4A1E" stroke-width="1" opacity="0.4"/>

        <!-- Pec lines -->
        <path d="M140,155 Q125,168 118,180" fill="none" stroke="#7A4A1E" stroke-width="1" opacity="0.4"/>
        <path d="M140,155 Q155,168 162,180" fill="none" stroke="#7A4A1E" stroke-width="1" opacity="0.4"/>

        <!-- Back arm (behind body) -->
        <path d="M105,150 Q88,165 82,200 L78,230" stroke="#8D5524" stroke-width="22" stroke-linecap="round" fill="none"/>
        <circle cx="76" cy="234" r="12" fill="#6B3E15"/>

        <!-- Neck -->
        <rect x="128" y="118" width="24" height="26" rx="8" fill="#8D5524"/>

        <!-- Head -->
        <ellipse cx="140" cy="90" rx="42" ry="48" fill="#8D5524"/>

        <!-- Ears -->
        <ellipse cx="97" cy="92" rx="8" ry="12" fill="#7A4A1E"/>
        <ellipse cx="183" cy="92" rx="8" ry="12" fill="#7A4A1E"/>

        <!-- Hair -->
        <path d="M98,78 Q100,38 140,32 Q180,38 182,78 L178,65 Q170,48 140,44 Q110,48 102,65 Z" fill="#111"/>
        <!-- Short hair texture -->
        <path d="M105,72 Q115,50 140,46 Q165,50 175,72" fill="none" stroke="#1a1a1a" stroke-width="3"/>

        <!-- Eyebrows -->
        <path d="M116,72 Q124,66 134,70" fill="none" stroke="#3E2723" stroke-width="2.5" stroke-linecap="round"/>
        <path d="M146,70 Q156,66 164,72" fill="none" stroke="#3E2723" stroke-width="2.5" stroke-linecap="round"/>

        <!-- Eyes (white) -->
        <ellipse cx="126" cy="84" rx="12" ry="9" fill="white"/>
        <ellipse cx="154" cy="84" rx="12" ry="9" fill="white"/>

        <!-- Irises -->
        <circle cx="128" cy="85" r="6" fill="#3E2723"/>
        <circle cx="156" cy="85" r="6" fill="#3E2723"/>

        <!-- Pupils -->
        <circle cx="129" cy="86" r="3" fill="#111"/>
        <circle cx="157" cy="86" r="3" fill="#111"/>

        <!-- Eye shine -->
        <circle cx="131" cy="82" r="1.8" fill="white" opacity="0.9"/>
        <circle cx="159" cy="82" r="1.8" fill="white" opacity="0.9"/>

        <!-- Nose -->
        <path d="M137,90 Q135,100 133,106 Q137,110 147,106 Q145,100 143,90" fill="#7A4A1E" opacity="0.7"/>

        <!-- Mouth -->
        <path d="M126,118 Q133,112 140,114 Q147,112 154,118" fill="none" stroke="#5D3A1A" stroke-width="2.2" stroke-linecap="round"/>

        <!-- Goatee / chin hair -->
        <path d="M132,122 Q136,130 140,132 Q144,130 148,122" fill="#1a1a1a" opacity="0.6"/>

        <!-- Front arm (raised - getting whipped) -->
        <path d="M172,150 Q190,140 200,115 L205,95" stroke="#8D5524" stroke-width="22" stroke-linecap="round" fill="none"/>
        <circle cx="206" cy="90" r="12" fill="#6B3E15"/>

        <!-- Sweat drops on body (decorative, animated via CSS) -->
        <g id="svg-sweat-drops">
          <ellipse cx="90" cy="160" rx="3" ry="5" fill="rgba(130,210,255,0.6)" class="svg-sweat" style="animation:svgDrop 1.8s ease-in infinite 0s"/>
          <ellipse cx="195" cy="175" rx="2.5" ry="4" fill="rgba(130,210,255,0.5)" class="svg-sweat" style="animation:svgDrop 2.2s ease-in infinite 0.5s"/>
          <ellipse cx="110" cy="240" rx="3" ry="5" fill="rgba(130,210,255,0.5)" class="svg-sweat" style="animation:svgDrop 2s ease-in infinite 0.9s"/>
          <ellipse cx="175" cy="255" rx="2" ry="3.5" fill="rgba(130,210,255,0.4)" class="svg-sweat" style="animation:svgDrop 2.5s ease-in infinite 1.3s"/>
          <ellipse cx="130" cy="140" rx="2" ry="3" fill="rgba(130,210,255,0.5)" class="svg-sweat" style="animation:svgDrop 1.6s ease-in infinite 0.3s"/>
        </g>
      </svg>

      <!-- Animated whip arm overlay -->
      <div class="whip-arm idle" id="whip-arm">
        <div class="whip-handle"></div>
        <div class="whip-cord"></div>
      </div>

      <!-- Crack flash -->
      <div class="crack-flash" id="crack-flash">
        <svg viewBox="0 0 60 60" width="60" height="60">
          <line x1="30" y1="30" x2="10" y2="5" stroke="rgba(255,255,200,0.9)" stroke-width="2"/>
          <line x1="30" y1="30" x2="55" y2="8" stroke="rgba(255,255,200,0.7)" stroke-width="1.5"/>
          <line x1="30" y1="30" x2="5" y2="35" stroke="rgba(255,255,200,0.6)" stroke-width="1"/>
          <line x1="30" y1="30" x2="50" y2="50" stroke="rgba(255,255,200,0.5)" stroke-width="1"/>
          <line x1="30" y1="30" x2="30" y2="2" stroke="rgba(255,255,200,0.8)" stroke-width="1.5"/>
          <circle cx="30" cy="30" r="4" fill="rgba(255,255,200,0.9)"/>
        </svg>
      </div>

      <!-- Dynamic sweat drops container -->
      <div id="dynamic-sweats"></div>

      <!-- Equipped whip label -->
      <div class="equipped-whip-label" id="whip-label">Rope Whip</div>
    </div>

    <!-- Passive rate display -->
    <div class="game-card px-4 py-2.5 text-center">
      <p class="text-muted text-xs mb-0.5">Passive Sweat</p>
      <p class="font-display font-700 text-base text-sky-400"><span id="passive-display">0</span> <span class="text-xs text-muted font-body">/ sec</span></p>
    </div>

    <!-- Rebirth Multiplier -->
    <div class="game-card px-4 py-2.5 text-center w-full max-w-xs">
      <p class="text-muted text-xs mb-0.5">Rebirth Multiplier</p>
      <p class="font-display font-700 text-base text-purple-400">x<span id="multi-display">1.00</span></p>
    </div>

    <!-- Infinity Whip -->
    <div id="whip-status" class="game-card px-4 py-2.5 text-center w-full max-w-xs hidden">
      <div class="flex items-center justify-center gap-2">
        <i class="fa-solid fa-bolt-lightning text-yellow-300"></i>
        <span class="font-display font-700 text-xs text-yellow-300">Infinity Whip Equipped</span>
      </div>
      <p class="text-muted text-xs mt-0.5">+1,000,000 SP/s (Persistent)</p>
    </div>
  </section>

  <!-- Right: Panels -->
  <section class="lg:w-7/12" aria-label="Game Panels">
    <nav class="flex gap-1" role="tablist" aria-label="Game panels">
      <button class="tab-btn active" data-tab="upgrades" role="tab" aria-selected="true"><i class="fa-solid fa-arrow-up-right-dots mr-1"></i>Whips</button>
      <button class="tab-btn" data-tab="rebirth" role="tab" aria-selected="false"><i class="fa-solid fa-rotate mr-1"></i>Rebirth</button>
      <button class="tab-btn" data-tab="slots" role="tab" aria-selected="false"><i class="fa-solid fa-dice mr-1"></i>Slots</button>
    </nav>

    <!-- Whips Panel -->
    <div id="panel-upgrades" class="game-card rounded-tl-none p-4 md:p-5" role="tabpanel">
      <div class="flex items-center justify-between mb-4">
        <h2 class="font-display font-700 text-sm text-fg">Whips & Gear</h2>
        <p class="text-xs text-muted">Click to purchase</p>
      </div>
      <div id="upgrades-list" class="grid gap-2.5 max-h-[62vh] overflow-y-auto pr-1"></div>
    </div>

    <!-- Rebirth Panel -->
    <div id="panel-rebirth" class="game-card rounded-tl-none p-4 md:p-5 hidden" role="tabpanel">
      <h2 class="font-display font-700 text-sm text-fg mb-4">Rebirth</h2>
      <div class="space-y-4">
        <div class="game-card p-4">
          <h3 class="font-600 text-xs text-muted mb-1.5">How It Works</h3>
          <p class="text-sm text-fg/80 leading-relaxed">Sacrifice all sweat, whips, and progress. Gain a permanent multiplier to all generation. More sweat = higher multiplier.</p>
        </div>
        <div class="game-card p-4">
          <div class="flex items-center justify-between mb-2"><span class="text-sm text-muted">Current Multiplier</span><span class="font-display font-700 text-purple-400">x<span id="rb-current-mult">1.00</span></span></div>
          <div class="flex items-center justify-between mb-2"><span class="text-sm text-muted">Total Rebirths</span><span class="font-display font-700 text-purple-300" id="rb-total-count">0</span></div>
          <div class="flex items-center justify-between"><span class="text-sm text-muted">Next Multiplier</span><span class="font-display font-700 text-purple-300">x<span id="rb-next-mult">1.50</span></span></div>
        </div>
        <div class="game-card p-4" style="border-color:rgba(124,77,255,0.2)">
          <div class="flex items-center justify-between mb-3"><span class="text-sm text-muted">Cost</span><span class="font-display font-700 text-sky-400" id="rb-cost">10,000</span></div>
          <button class="rebirth-btn w-full locked" id="rebirth-btn" aria-label="Rebirth"><span class="relative z-10 flex items-center justify-center gap-2"><i class="fa-solid fa-rotate"></i>Rebirth Now</span></button>
          <p class="text-xs text-muted text-center mt-2" id="rb-hint">Earn 10,000 sweat points to unlock</p>
        </div>
      </div>
    </div>

    <!-- Slots Panel -->
    <div id="panel-slots" class="game-card rounded-tl-none p-4 md:p-5 hidden" role="tabpanel">
      <div class="flex items-center justify-between mb-4">
        <h2 class="font-display font-700 text-sm text-fg">Lucky Slots</h2>
        <p class="text-xs text-muted">Cost: <span class="text-sky-400 font-600">500</span> SP</p>
      </div>
      <div class="flex flex-col items-center gap-4">
        <div class="slot-machine">
          <div class="flex items-center justify-center gap-2.5 mb-4" id="slot-reels">
            <div class="slot-reel-window"><div class="slot-reel-strip" id="reel-0"><div class="sym">🔥</div></div></div>
            <div class="slot-reel-window"><div class="slot-reel-strip" id="reel-1"><div class="sym">💧</div></div></div>
            <div class="slot-reel-window"><div class="slot-reel-strip" id="reel-2"><div class="sym">🏋️</div></div></div>
          </div>
          <div class="flex items-center justify-center gap-2.5 mb-4" style="margin-top:-6px">
            <div class="w-[80px] h-[1.5px] bg-red-500/30"></div>
            <div class="w-[80px] h-[1.5px] bg-red-500/30"></div>
            <div class="w-[80px] h-[1.5px] bg-red-500/30"></div>
          </div>
          <div class="text-center"><button class="spin-btn" id="spin-btn">SPIN</button></div>
        </div>
        <div class="game-card p-3.5 w-full max-w-md">
          <h3 class="font-600 text-xs text-muted mb-2.5 text-center">Payouts</h3>
          <div class="grid grid-cols-2 gap-1.5 text-xs">
            <div class="flex items-center gap-1.5"><span>🔥🔥🔥</span><span class="text-sky-400 font-600">5,000 SP</span></div>
            <div class="flex items-center gap-1.5"><span>💧💧💧</span><span class="text-sky-400 font-600">3,000 SP</span></div>
            <div class="flex items-center gap-1.5"><span>🏋️🏋️🏋️</span><span class="text-sky-400 font-600">2,000 SP</span></div>
            <div class="flex items-center gap-1.5"><span>⚡⚡⚡</span><span class="text-sky-400 font-600">1,000 SP</span></div>
            <div class="flex items-center gap-1.5"><span>💨💨💨</span><span class="text-sky-400 font-600">800 SP</span></div>
            <div class="flex items-center gap-1.5"><span>🥊🥊🥊</span><span class="text-sky-400 font-600">600 SP</span></div>
            <div class="flex items-center gap-1.5"><span>💦💦💦</span><span class="text-sky-400 font-600">400 SP</span></div>
            <div class="flex items-center gap-1.5"><span>⭐⭐⭐</span><span class="text-sky-400 font-600">1,500 SP</span></div>
            <div class="col-span-2 flex items-center justify-center gap-1.5 pt-1.5 border-t border-border">
              <span>⚡🥊⚡</span><span class="text-yellow-300 font-700">INFINITY WHIP — 1,000,000 SP</span>
            </div>
          </div>
        </div>
        <div class="flex gap-4 text-xs text-muted">
          <span>Spins: <span id="slot-spins" class="text-fg font-600">0</span></span>
          <span>Won: <span id="slot-won" class="text-sky-400 font-600">0</span></span>
        </div>
      </div>
    </div>
  </section>
</main>

<!-- Toasts -->
<div id="toast-container" class="fixed top-20 right-4 z-50 flex flex-col gap-2" aria-live="polite"></div>

<!-- SVG sweat drop animation -->
<style>
@keyframes svgDrop{0%{opacity:0.7;transform:translateY(0)}80%{opacity:0.4}100%{opacity:0;transform:translateY(18px)}}
</style>

<script>
/* =============================================
   SWEAT TYCOON — Full Game Logic
   ============================================= */

// ---- State ----
const state = {
  sweat: 0, totalSweat: 0, clickPower: 1, passiveRate: 0,
  rebirths: 0, rebirthMult: 1,
  hasInfinityWhip: false, infinityWhipBonus: 0,
  slotStats: { spins: 0, won: 0 },
  upgrades: {},
  lastTick: Date.now()
};

// ---- Whip Upgrade Definitions ----
const UPGRADE_DEFS = [
  { id:'rope',      name:'Rope Whip',          desc:'A basic rope that stings',          icon:'fa-link',              baseCost:15,      costMult:1.35, baseValue:1,    valueMult:1.2,  type:'click',  whipColor:'#8D6E63' },
  { id:'leather',   name:'Leather Whip',        desc:'Braided leather packs a punch',     icon:'fa-ribbon',            baseCost:80,      costMult:1.4,  baseValue:4,    valueMult:1.22, type:'click',  whipColor:'#5D4037' },
  { id:'chain',     name:'Chain Whip',          desc:'Heavy links leave deep marks',      icon:'fa-link-slash',        baseCost:400,     costMult:1.45, baseValue:15,   valueMult:1.25, type:'click',  whipColor:'#90A4AE' },
  { id:'fans',      name:'Cooling Fans',        desc:'Fans keep the sweat flowing',       icon:'fa-fan',               baseCost:200,     costMult:1.4,  baseValue:2,    valueMult:1.2,  type:'passive', whipColor:null },
  { id:'sauna',     name:'Sauna Suite',         desc:'Industrial heat generation',        icon:'fa-temperature-high',  baseCost:1000,    costMult:1.5,  baseValue:10,   valueMult:1.25, type:'passive', whipColor:null },
  { id:'bullwhip',  name:'Bullwhip',            desc:'The classic — loud and effective',  icon:'fa-bolt',              baseCost:3000,    costMult:1.5,  baseValue:50,   valueMult:1.3,  type:'click',  whipColor:'#4E342E' },
  { id:'gym',       name:'Gym Membership',      desc:'Treadmills and rowing machines',    icon:'fa-dumbbell',          baseCost:5000,    costMult:1.55, baseValue:40,   valueMult:1.3,  type:'passive', whipColor:null },
  { id:'hotyoga',   name:'Hot Yoga Studio',     desc:'Bikram-style heated room',          icon:'fa-spa',               baseCost:18000,   costMult:1.6,  baseValue:150,  valueMult:1.35, type:'passive', whipColor:null },
  { id:'cat9',      name:'Cat o\' Nine Tails',  desc:'Nine strands of pure agony',        icon:'fa-bahai',             baseCost:50000,   costMult:1.6,  baseValue:300,  valueMult:1.35, type:'click',  whipColor:'#3E2723' },
  { id:'volcano',   name:'Volcano Sauna',       desc:'Harness magma-level heat',          icon:'fa-volcano',           baseCost:150000,  costMult:1.65, baseValue:600,  valueMult:1.4,  type:'passive', whipColor:null },
  { id:'scorpion',  name:'Scorpion Whip',       desc:'Flexible and devastating',          icon:'fa-staff-snake',       baseCost:500000,  costMult:1.7,  baseValue:1500, valueMult:1.45, type:'click',  whipColor:'#BF360C' },
  { id:'factory',   name:'Sweat Factory',       desc:'Industrial-scale perspiration',     icon:'fa-industry',          baseCost:800000,  costMult:1.7,  baseValue:2500, valueMult:1.4,  type:'passive', whipColor:null },
  { id:'sunforge',  name:'Sun Forge Harness',   desc:'Channel the power of the sun',      icon:'fa-sun',               baseCost:3000000, costMult:1.75, baseValue:8000, valueMult:1.5,  type:'passive', whipColor:null },
];

// ---- Slot Data ----
const SLOT_SYMBOLS = ['🔥','💧','🏋️','⚡','💨','🥊','💦','⭐'];
const SLOT_PAYOUTS = {
  '🔥🔥🔥':{amount:5000,msg:'Triple Fire! +5,000 SP'},
  '💧💧💧':{amount:3000,msg:'Triple Drip! +3,000 SP'},
  '🏋️🏋️🏋️':{amount:2000,msg:'Triple Workout! +2,000 SP'},
  '⚡⚡⚡':{amount:1000,msg:'Triple Bolt! +1,000 SP'},
  '💨💨💨':{amount:800, msg:'Triple Wind! +800 SP'},
  '🥊🥊🥊':{amount:600, msg:'Triple Punch! +600 SP'},
  '💦💦💦':{amount:400, msg:'Triple Splash! +400 SP'},
  '⭐⭐⭐':{amount:1500,msg:'Triple Star! +1,500 SP'}
};
const INFINITY_PATTERN = '⚡🥊⚡';
const SLOT_COST = 500;

// ---- Utilities ----
function fmt(n){
  if(n<0) return '-'+fmt(-n);
  if(n<1000) return Math.floor(n).toString();
  const u=['','K','M','B','T','Qa','Qi','Sx','Sp','Oc','No','Dc'];
  const t=Math.min(Math.floor(Math.log10(Math.max(1,n))/3),u.length-1);
  if(t===0) return Math.floor(n).toString();
  const s=n/Math.pow(10,t*3);
  return (s%1===0?s.toFixed(0):s.toFixed(1))+u[t];
}
function getCost(d,l){return Math.floor(d.baseCost*Math.pow(d.costMult,l))}
function getVal(d,l){return Math.floor(d.baseValue*Math.pow(d.valueMult,l))}
function getRebirthCost(){return Math.floor(10000*Math.pow(2,state.rebirths))}
function getNextMult(){return 1+(state.rebirths+1)*0.5}
function totalClick(){let p=state.clickPower*state.rebirthMult;if(state.hasInfinityWhip)p+=state.infinityWhipBonus*state.rebirthMult;return p}
function totalPassive(){let r=state.passiveRate*state.rebirthMult;if(state.hasInfinityWhip)r+=state.infinityWhipBonus*state.rebirthMult;return r}

// Get the best whip name for display
function getBestWhipName(){
  let best=null;
  UPGRADE_DEFS.forEach(d=>{
    if(d.type==='click'&&(state.upgrades[d.id]||0)>0){
      if(!best||getCost(d,0)>getCost(best,0)) best=d;
    }
  });
  return best?best.name:'Rope Whip';
}

// Get the whip color for the visual
function getWhipColor(){
  let best=null,bestLvl=0;
  UPGRADE_DEFS.forEach(d=>{
    if(d.type==='click'&&d.whipColor){
      const l=state.upgrades[d.id]||0;
      if(l>bestLvl){bestLvl=l;best=d}
    }
  });
  return best?best.whipColor:'#8D6E63';
}

// ---- Save/Load ----
function saveGame(){try{localStorage.setItem('sweatTycoon',JSON.stringify({...state,lastTick:Date.now()}))}catch(e){}}
function loadGame(){
  try{
    const r=localStorage.getItem('sweatTycoon');
    if(!r)return false;
    const d=JSON.parse(r);
    Object.assign(state,d);
    const elapsed=Math.min((Date.now()-state.lastTick)/1000,3600);
    if(elapsed>1){
      const off=totalPassive()*elapsed;
      if(off>0){state.sweat+=off;state.totalSweat+=off;setTimeout(()=>showToast(`Welcome back! +${fmt(off)} SP earned offline.`,'info'),400)}
    }
    state.lastTick=Date.now();
    return true;
  }catch(e){return false}
}

// ---- Toasts ----
function showToast(msg,type='info'){
  const c=document.getElementById('toast-container');
  const t=document.createElement('div');
  t.className=`toast toast-${type}`;
  t.textContent=msg;
  c.appendChild(t);
  setTimeout(()=>t.remove(),3100);
}

// ---- UI Updates ----
function updateUI(){
  const cp=totalClick(),pr=totalPassive();
  document.getElementById('stat-sweat').textContent=fmt(state.sweat);
  document.getElementById('stat-ps').textContent=fmt(pr);
  document.getElementById('stat-pc').textContent=fmt(cp);
  document.getElementById('stat-rebirths').textContent=state.rebirths;
  document.getElementById('main-sweat').textContent=fmt(state.sweat);
  document.getElementById('passive-display').textContent=fmt(pr);
  document.getElementById('multi-display').textContent=state.rebirthMult.toFixed(2);
  document.getElementById('whip-label').textContent=getBestWhipName();

  // Update whip visual color
  const wc=getWhipColor();
  const cord=document.querySelector('.whip-cord');
  const handle=document.querySelector('.whip-handle');
  if(cord){cord.style.background=`linear-gradient(180deg,${wc},${darken(wc,20)})`}
  if(handle){handle.style.background=`linear-gradient(90deg,${darken(wc,15)},${wc},${darken(wc,15)})`}

  const ws=document.getElementById('whip-status');
  state.hasInfinityWhip?ws.classList.remove('hidden'):ws.classList.add('hidden');

  // Rebirth
  document.getElementById('rb-current-mult').textContent=state.rebirthMult.toFixed(2);
  document.getElementById('rb-total-count').textContent=state.rebirths;
  document.getElementById('rb-next-mult').textContent=getNextMult().toFixed(2);
  const rc=getRebirthCost();
  document.getElementById('rb-cost').textContent=fmt(rc);
  const rb=document.getElementById('rebirth-btn'),rh=document.getElementById('rb-hint');
  if(state.sweat>=rc){rb.classList.remove('locked');rh.textContent='Ready to rebirth!';rh.style.color='#B388FF'}
  else{rb.classList.add('locked');rh.textContent=`Need ${fmt(rc-state.sweat)} more`;rh.style.color=''}

  document.getElementById('slot-spins').textContent=state.slotStats.spins;
  document.getElementById('slot-won').textContent=fmt(state.slotStats.won);

  renderUpgrades();
}

function darken(hex,amt){
  let r=parseInt(hex.slice(1,3),16),g=parseInt(hex.slice(3,5),16),b=parseInt(hex.slice(5,7),16);
  r=Math.max(0,r-amt);g=Math.max(0,g-amt);b=Math.max(0,b-amt);
  return `#${r.toString(16).padStart(2,'0')}${g.toString(16).padStart(2,'0')}${b.toString(16).padStart(2,'0')}`;
}

function renderUpgrades(){
  const c=document.getElementById('upgrades-list');
  if(c.children.length!==UPGRADE_DEFS.length){
    c.innerHTML='';
    UPGRADE_DEFS.forEach(d=>{
      const isWhip=d.type==='click';
      const card=document.createElement('div');
      card.className='upgrade-card game-card';
      card.id=`uc-${d.id}`;
      card.setAttribute('role','button');
      card.setAttribute('tabindex','0');
      card.innerHTML=`
        <div class="flex items-center gap-3">
          <div class="w-10 h-10 rounded-lg flex items-center justify-center text-base flex-shrink-0" style="background:${isWhip&&d.whipColor?d.whipColor+'22':'rgba(255,179,0,0.08)'};color:${isWhip&&d.whipColor?d.whipColor:'var(--accent)'}">
            <i class="fa-solid ${d.icon}"></i>
          </div>
          <div class="flex-1 min-w-0">
            <div class="flex items-center gap-2 flex-wrap">
              <span class="font-600 text-sm text-fg">${d.name}</span>
              ${isWhip?'<span class="text-[10px] font-700 px-1.5 py-0.5 rounded" style="background:rgba(255,111,0,0.15);color:#FF8F00">WHIP</span>':'<span class="text-[10px] font-700 px-1.5 py-0.5 rounded" style="background:rgba(100,200,255,0.1);color:#64B5F6">PASSIVE</span>'}
              <span class="text-[10px] text-muted bg-bg px-1.5 py-0.5 rounded" id="ulv-${d.id}">Lv.0</span>
            </div>
            <p class="text-xs text-muted mt-0.5">${d.desc}</p>
          </div>
          <div class="text-right flex-shrink-0">
            <div class="buy-btn px-2.5 py-1.5 rounded-lg text-xs font-700 border border-border" id="ub-${d.id}">
              <span id="uc2-${d.id}">${fmt(d.baseCost)}</span>
              <i class="fa-solid fa-droplet text-sky-500 ml-0.5" style="font-size:8px"></i>
            </div>
            <p class="text-[10px] text-muted mt-0.5" id="uv-${d.id}">+${fmt(d.baseValue)}${isWhip?'/whip':'/s'}</p>
          </div>
        </div>`;
      card.addEventListener('click',()=>buyUpgrade(d.id));
      card.addEventListener('keydown',e=>{if(e.key==='Enter'||e.key===' '){e.preventDefault();buyUpgrade(d.id)}});
      c.appendChild(card);
    });
  }
  UPGRADE_DEFS.forEach(d=>{
    const l=state.upgrades[d.id]||0;
    const cost=getCost(d,l);
    const val=getVal(d,l+1);
    const card=document.getElementById(`uc-${d.id}`);
    card.classList.toggle('locked',state.sweat<cost);
    document.getElementById(`ulv-${d.id}`).textContent=`Lv.${l}`;
    document.getElementById(`uc2-${d.id}`).textContent=fmt(cost);
    document.getElementById(`uv-${d.id}`).textContent=`+${fmt(val)}${d.type==='click'?'/whip':'/s'}`;
  });
}

// ---- Actions ----
function doWhip(){
  const earned=totalClick();
  state.sweat+=earned;
  state.totalSweat+=earned;
  animateWhip();
  spawnFloat(earned);
  spawnSweatDrops(4);
  const el=document.getElementById('main-sweat');
  el.classList.remove('stat-pulse');void el.offsetWidth;el.classList.add('stat-pulse');
}

function animateWhip(){
  const arm=document.getElementById('whip-arm');
  const flash=document.getElementById('crack-flash');
  arm.classList.remove('idle','cracking');
  void arm.offsetWidth;
  arm.classList.add('cracking');
  flash.classList.remove('active');
  void flash.offsetWidth;
  flash.classList.add('active');
  setTimeout(()=>{arm.classList.remove('cracking');arm.classList.add('idle')},200);
  setTimeout(()=>flash.classList.remove('active'),300);
}

function spawnFloat(amount){
  const c=document.getElementById('char-scene');
  const el=document.createElement('div');
  el.className='float-text';
  el.textContent='+'+fmt(amount);
  el.style.left=(100+Math.random()*80)+'px';
  el.style.top=(30+Math.random()*40)+'px';
  el.style.fontSize=amount>=1000?'16px':'13px';
  c.appendChild(el);
  setTimeout(()=>el.remove(),1000);
}

function spawnSweatDrops(count){
  const c=document.getElementById('char-scene');
  for(let i=0;i<count;i++){
    const d=document.createElement('div');
    d.className='sweat-drop';
    d.style.left=(60+Math.random()*140)+'px';
    d.style.top=(120+Math.random()*140)+'px';
    d.style.width=(5+Math.random()*5)+'px';
    d.style.height=(7+Math.random()*7)+'px';
    d.classList.add('active');
    d.style.animationDelay=(Math.random()*0.2)+'s';
    c.appendChild(d);
    setTimeout(()=>d.remove(),900);
  }
}

function buyUpgrade(id){
  const d=UPGRADE_DEFS.find(u=>u.id===id);
  if(!d)return;
  const l=state.upgrades[id]||0;
  const cost=getCost(d,l);
  if(state.sweat<cost){showToast('Not enough Sweat Points!','error');return}
  state.sweat-=cost;
  state.upgrades[id]=l+1;
  const val=getVal(d,l+1);
  if(d.type==='click') state.clickPower+=val;
  else state.passiveRate+=val;
  showToast(`${d.name} upgraded to Lv.${l+1}!`,'success');
  updateUI();
}

function doRebirth(){
  const cost=getRebirthCost();
  if(state.sweat<cost){showToast('Not enough to rebirth!','error');return}
  const f=document.createElement('div');f.className='rebirth-flash';document.body.appendChild(f);setTimeout(()=>f.remove(),1100);
  state.sweat=0;state.clickPower=1;state.passiveRate=0;state.upgrades={};
  state.rebirths++;state.rebirthMult=getNextMult();
  showToast(`Rebirth #${state.rebirths}! Multiplier: x${state.rebirthMult.toFixed(2)}`,'legendary');
  document.getElementById('upgrades-list').innerHTML='';
  updateUI();saveGame();
}

// ---- Slot Machine ----
let slotBusy=false;
function weightedSym(){
  const w={'🔥':20,'💧':20,'🏋️':15,'⚡':8,'💨':15,'🥊':8,'💦':15,'⭐':12};
  const t=Object.values(w).reduce((a,b)=>a+b,0);
  let r=Math.random()*t;
  for(const[s,v] of Object.entries(w)){r-=v;if(r<=0)return s}
  return '🔥';
}

function spinSlots(){
  if(slotBusy)return;
  if(state.sweat<SLOT_COST){showToast('Need 500 SP to spin!','error');return}
  slotBusy=true;state.sweat-=SLOT_COST;state.slotStats.spins++;
  document.getElementById('spin-btn').disabled=true;
  const finals=[weightedSym(),weightedSym(),weightedSym()];
  const reels=[document.getElementById('reel-0'),document.getElementById('reel-1'),document.getElementById('reel-2')];
  const wh=reels[0].offsetHeight||90;
  reels.forEach((reel,i)=>{
    const pad=15+i*8;
    let h='';
    for(let j=0;j<pad;j++) h+=`<div class="sym">${SLOT_SYMBOLS[Math.floor(Math.random()*SLOT_SYMBOLS.length)]}</div>`;
    h+=`<div class="sym">${finals[i]}</div>`;
    reel.innerHTML=h;
    reel.style.transition='none';reel.style.transform='translateY(0)';void reel.offsetHeight;
  });
  reels.forEach((reel,i)=>{
    const off=(15+i*8)*wh;
    const dur=1.2+i*0.6;
    setTimeout(()=>{reel.style.transition=`transform ${dur}s cubic-bezier(0.15,0.85,0.25,1)`;reel.style.transform=`translateY(-${off}px)`},50);
  });
  const total=(1.2+2*0.6+0.15)*1000+100;
  setTimeout(()=>{checkSlots(finals);slotBusy=false;document.getElementById('spin-btn').disabled=false;updateUI()},total);
  updateUI();
}

function checkSlots(syms){
  const r=syms.join('');let tt='info',msg='';
  if(r===INFINITY_PATTERN){
    state.hasInfinityWhip=true;state.infinityWhipBonus=1000000;
    state.sweat+=1000000;state.totalSweat+=1000000;state.slotStats.won+=1000000;
    tt='legendary';msg='INFINITY WHIP! +1,000,000 SP permanently!';
    const f=document.createElement('div');f.className='flash-overlay';document.body.appendChild(f);setTimeout(()=>f.remove(),1600);
  }else if(SLOT_PAYOUTS[r]){
    const p=SLOT_PAYOUTS[r];const a=Math.floor(p.amount*state.rebirthMult);
    state.sweat+=a;state.totalSweat+=a;state.slotStats.won+=a;
    tt='success';msg=p.msg.replace(/\d[\d,]*/,fmt(a));
  }else if(syms[0]===syms[1]||syms[1]===syms[2]){
    const c=Math.floor(50*state.rebirthMult);
    state.sweat+=c;state.totalSweat+=c;state.slotStats.won+=c;
    tt='info';msg=`Partial match! +${fmt(c)} SP`;
  }else{msg='No match. Try again!';tt='error'}
  showToast(msg,tt);
}

// ---- Tabs ----
document.querySelectorAll('.tab-btn').forEach(b=>{
  b.addEventListener('click',()=>{
    document.querySelectorAll('.tab-btn').forEach(t=>{t.classList.remove('active');t.setAttribute('aria-selected','false')});
    b.classList.add('active');b.setAttribute('aria-selected','true');
    document.querySelectorAll('[role="tabpanel"]').forEach(p=>p.classList.add('hidden'));
    document.getElementById(`panel-${b.dataset.tab}`).classList.remove('hidden');
  });
});

// ---- Events ----
const charScene=document.getElementById('char-scene');
charScene.addEventListener('click',e=>{e.preventDefault();doWhip()});
charScene.addEventListener('keydown',e=>{if(e.key==='Enter'||e.key===' '){e.preventDefault();doWhip()}});
document.getElementById('rebirth-btn').addEventListener('click',doRebirth);
document.getElementById('spin-btn').addEventListener('click',spinSlots);

// ---- Background Particles ----
const bgC=document.getElementById('bg-canvas'),bgX=bgC.getContext('2d');
let bgP=[];
function resizeBg(){bgC.width=innerWidth;bgC.height=innerHeight}
resizeBg();addEventListener('resize',resizeBg);
for(let i=0;i<35;i++) bgP.push({x:Math.random()*innerWidth,y:Math.random()*innerHeight,s:0.8+Math.random()*1.5,sx:(Math.random()-0.5)*0.25,sy:-0.15-Math.random()*0.4,o:0.08+Math.random()*0.2,h:25+Math.random()*20});
function drawBg(){
  bgX.clearRect(0,0,bgC.width,bgC.height);
  bgP.forEach(p=>{
    bgX.beginPath();bgX.arc(p.x,p.y,Math.max(0.5,p.s),0,Math.PI*2);
    bgX.fillStyle=`hsla(${p.h},80%,55%,${p.o})`;bgX.fill();
    p.x+=p.sx;p.y+=p.sy;
    if(p.y<-10){p.y=bgC.height+10;p.x=Math.random()*bgC.width}
    if(p.x<-10)p.x=bgC.width+10;if(p.x>bgC.width+10)p.x=-10;
  });
  requestAnimationFrame(drawBg);
}
drawBg();

// ---- Game Loop ----
function tick(){
  const now=Date.now(),dt=(now-state.lastTick)/1000;state.lastTick=now;
  const pr=totalPassive();
  if(pr>0){const e=pr*dt;state.sweat+=e;state.totalSweat+=e}
  updateUI();
}
setInterval(tick,50);
setInterval(saveGame,15000);
addEventListener('beforeunload',saveGame);

// ---- Init ----
loadGame();
document.getElementById('upgrades-list').innerHTML='';
updateUI();
</script>
</body>
</html>
