<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Man in the Mirror</title>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #ffffff;
    --bg2: #f7f7f6;
    --bg3: #f0f0ee;
    --border: rgba(0,0,0,0.08);
    --border2: rgba(0,0,0,0.14);
    --txt: #1a1a1a;
    --txt2: #6b6b6b;
    --txt3: #a0a0a0;
    --radius: 12px;
    --radius-sm: 8px;
    --fe: #7B72E9;
    --cuerpo: #2DB88F;
    --mente: #3A8FE8;
    --crear: #E8931A;
    --mision: #E05A32;
    --rel: #E05080;
    --car: #888884;
  }

  @media (prefers-color-scheme: dark) {
    :root {
      --bg: #0f0f0f;
      --bg2: #1a1a1a;
      --bg3: #222222;
      --border: rgba(255,255,255,0.07);
      --border2: rgba(255,255,255,0.12);
      --txt: #f0f0f0;
      --txt2: #999999;
      --txt3: #555555;
    }
  }

  body {
    font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif;
    background: var(--bg);
    color: var(--txt);
    font-size: 13px;
    line-height: 1.5;
    padding: 24px 20px 40px;
    max-width: 680px;
    margin: 0 auto;
  }

  /* ── header ── */
  .header {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 24px;
  }
  .header-label {
    font-size: 10px;
    color: var(--txt3);
    letter-spacing: 0.1em;
    text-transform: uppercase;
    margin-bottom: 5px;
  }
  .header-title {
    font-size: 22px;
    font-weight: 500;
    letter-spacing: -0.3px;
    color: var(--txt);
  }
  .header-sub {
    font-size: 12px;
    color: var(--txt3);
    margin-top: 3px;
  }
  .header-score {
    text-align: right;
  }
  .header-score-num {
    font-size: 34px;
    font-weight: 500;
    letter-spacing: -1px;
    line-height: 1;
  }
  .header-score-label {
    font-size: 10px;
    color: var(--txt3);
    margin-top: 3px;
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  /* ── tabs ── */
  .tabs {
    display: flex;
    gap: 4px;
    margin-bottom: 20px;
    padding-bottom: 16px;
    border-bottom: 0.5px solid var(--border);
  }
  .tab {
    padding: 5px 14px;
    border-radius: 20px;
    font-size: 12px;
    font-weight: 400;
    cursor: pointer;
    border: 0.5px solid var(--border2);
    background: transparent;
    color: var(--txt2);
    font-family: inherit;
    transition: all .15s;
  }
  .tab:hover { background: var(--bg2); }
  .tab.active {
    background: var(--txt);
    color: var(--bg);
    border-color: var(--txt);
    font-weight: 500;
  }

  /* ── panels ── */
  .panel { display: none; }
  .panel.visible { display: block; }

  /* ── metric cards ── */
  .metrics {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 8px;
    margin-bottom: 12px;
  }
  .metric {
    background: var(--bg2);
    border-radius: var(--radius-sm);
    padding: 12px 14px;
  }
  .metric-label {
    font-size: 10px;
    color: var(--txt3);
    text-transform: uppercase;
    letter-spacing: 0.06em;
    margin-bottom: 5px;
  }
  .metric-val {
    font-size: 15px;
    font-weight: 500;
    color: var(--txt);
    line-height: 1.3;
  }

  /* ── card ── */
  .card {
    background: var(--bg);
    border: 0.5px solid var(--border);
    border-radius: var(--radius);
    padding: 16px 18px;
    margin-bottom: 10px;
  }
  .card-label {
    font-size: 11px;
    color: var(--txt3);
    margin-bottom: 12px;
    letter-spacing: 0.02em;
  }

  /* ── bar chart ── */
  .bars {
    display: flex;
    align-items: flex-end;
    gap: 5px;
    height: 64px;
  }
  .bar-col {
    flex: 1;
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 3px;
    height: 100%;
    justify-content: flex-end;
  }
  .bar-val { font-size: 9px; color: var(--txt3); font-weight: 500; }
  .bar-rect {
    width: 100%;
    border-radius: 3px 3px 0 0;
    transition: height .4s ease;
    min-height: 2px;
  }
  .bar-day { font-size: 9px; color: var(--txt3); margin-top: 3px; }

  /* ── area cards grid ── */
  .areas-grid {
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(190px, 1fr));
    gap: 8px;
  }
  .area-card {
    background: var(--bg);
    border: 0.5px solid var(--border);
    border-radius: var(--radius);
    padding: 14px 16px;
  }
  .area-top {
    display: flex;
    justify-content: space-between;
    align-items: flex-start;
    margin-bottom: 10px;
  }
  .area-dot {
    width: 7px; height: 7px;
    border-radius: 50%;
    margin-bottom: 5px;
  }
  .area-name {
    font-size: 12px;
    font-weight: 500;
    color: var(--txt);
    max-width: 100px;
    line-height: 1.35;
  }
  .area-trend {
    font-size: 10px;
    margin-top: 3px;
  }

  /* ── ring svg ── */
  .ring-wrap { flex-shrink: 0; }

  /* ── spark ── */
  .spark { margin-top: 4px; }

  /* ── tracker ── */
  .tracker-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 12px;
  }
  .tracker-title {
    font-size: 13px;
    font-weight: 500;
  }
  .btn-add {
    font-size: 11px;
    padding: 4px 12px;
    border-radius: 7px;
    border: 0.5px solid var(--border2);
    background: transparent;
    cursor: pointer;
    color: var(--txt2);
    font-family: inherit;
    transition: background .15s;
  }
  .btn-add:hover { background: var(--bg2); }

  .day-row {
    background: var(--bg);
    border: 0.5px solid var(--border);
    border-radius: var(--radius);
    margin-bottom: 6px;
    overflow: hidden;
  }
  .day-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 12px 16px;
    cursor: pointer;
    user-select: none;
  }
  .day-header:hover { background: var(--bg2); }
  .day-left { display: flex; align-items: center; gap: 10px; }
  .day-name { font-size: 14px; font-weight: 500; }
  .day-dots { display: flex; gap: 3px; align-items: center; }
  .day-dot-bar {
    width: 4px;
    height: 18px;
    border-radius: 2px;
  }
  .day-right { display: flex; align-items: center; gap: 10px; }
  .day-score { font-size: 13px; font-weight: 500; }
  .chevron { font-size: 11px; color: var(--txt3); transition: transform .2s; }
  .chevron.open { transform: rotate(180deg); }

  .day-body {
    display: none;
    padding: 0 16px 14px;
    border-top: 0.5px solid var(--border);
    padding-top: 12px;
  }
  .day-body.open { display: block; }

  .q-row {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 8px;
    margin-bottom: 8px;
  }
  .q-area-dot {
    width: 5px; height: 5px; border-radius: 50%; flex-shrink: 0;
  }
  .q-text {
    flex: 1;
    font-size: 11px;
    color: var(--txt2);
  }
  .q-btns { display: flex; gap: 3px; }
  .vbtn {
    padding: 3px 9px;
    border-radius: 20px;
    font-size: 10px;
    cursor: pointer;
    border: 0.5px solid var(--border);
    background: transparent;
    color: var(--txt2);
    font-family: inherit;
    transition: all .1s;
    white-space: nowrap;
  }
  .vbtn.si.active   { background: #E1F5EE; color: #085041; border-color: #E1F5EE; font-weight: 500; }
  .vbtn.par.active  { background: #FAEEDA; color: #633806; border-color: #FAEEDA; font-weight: 500; }
  .vbtn.no.active   { background: #FCEBEB; color: #791F1F; border-color: #FCEBEB; font-weight: 500; }

  /* new entry form */
  .new-entry {
    background: var(--bg2);
    border-radius: var(--radius);
    padding: 14px 16px;
    margin-bottom: 10px;
    display: none;
  }
  .new-entry.open { display: block; }
  .new-entry-label { font-size: 11px; color: var(--txt3); margin-bottom: 8px; }
  .new-entry input[type=text] {
    width: 100%;
    border: 0.5px solid var(--border2);
    border-radius: 7px;
    padding: 7px 10px;
    font-size: 12px;
    font-family: inherit;
    background: var(--bg);
    color: var(--txt);
    margin-bottom: 12px;
    outline: none;
  }
  .new-entry input[type=text]:focus { border-color: #534AB7; }
  .form-btns { display: flex; gap: 6px; margin-top: 10px; }
  .btn-save {
    padding: 6px 16px;
    border-radius: 7px;
    background: var(--txt);
    color: var(--bg);
    border: none;
    font-size: 12px;
    cursor: pointer;
    font-family: inherit;
    font-weight: 500;
  }
  .btn-cancel {
    padding: 6px 14px;
    border-radius: 7px;
    background: transparent;
    color: var(--txt2);
    border: 0.5px solid var(--border2);
    font-size: 12px;
    cursor: pointer;
    font-family: inherit;
  }

  /* ── heatmap ── */
  .heat-wrap { overflow-x: auto; }
  .heat-inner { min-width: 420px; }
  .heat-days-header {
    display: flex;
    margin-left: 154px;
    gap: 4px;
    margin-bottom: 5px;
  }
  .heat-day-label {
    width: 28px;
    font-size: 9px;
    color: var(--txt3);
    text-align: center;
    flex-shrink: 0;
  }
  .heat-row {
    display: flex;
    align-items: center;
    gap: 8px;
    margin-bottom: 4px;
  }
  .heat-area-label {
    width: 146px;
    font-size: 11px;
    color: var(--txt2);
    text-align: right;
    flex-shrink: 0;
  }
  .heat-cells { display: flex; gap: 4px; }
  .heat-cell {
    width: 28px;
    height: 28px;
    border-radius: 5px;
    flex-shrink: 0;
    transition: background .3s;
    cursor: default;
  }
  .heat-legend {
    display: flex;
    gap: 12px;
    margin-top: 14px;
    margin-left: 154px;
    font-size: 10px;
    color: var(--txt3);
    align-items: center;
  }
  .heat-legend-item { display: flex; align-items: center; gap: 4px; }
  .heat-legend-swatch {
    width: 12px; height: 12px; border-radius: 3px;
  }

  /* ── radar svg ── */
  #radar-svg {
    display: block;
    margin: 0 auto;
    max-width: 210px;
  }
</style>
</head>
<body>

<div class="header">
  <div>
    <div class="header-label">man in the mirror</div>
    <div class="header-title">Estadísticas de vida</div>
    <div class="header-sub" id="sub-label">7 días registrados esta semana</div>
  </div>
  <div class="header-score">
    <div class="header-score-num" id="global-score">—</div>
    <div class="header-score-label">promedio global</div>
  </div>
</div>

<div class="tabs">
  <button class="tab active" data-tab="resumen">Resumen</button>
  <button class="tab" data-tab="areas">Áreas</button>
  <button class="tab" data-tab="tracker">Tracker</button>
  <button class="tab" data-tab="heatmap">Heatmap</button>
</div>

<!-- RESUMEN -->
<div class="panel visible" id="panel-resumen">
  <div class="metrics">
    <div class="metric">
      <div class="metric-label">mejor día</div>
      <div class="metric-val" id="m-best">—</div>
    </div>
    <div class="metric">
      <div class="metric-label">racha</div>
      <div class="metric-val" id="m-racha">—</div>
    </div>
    <div class="metric">
      <div class="metric-label">área débil</div>
      <div class="metric-val" id="m-weak">—</div>
    </div>
  </div>
  <div class="card">
    <div class="card-label">Puntuación diaria</div>
    <div class="bars" id="daily-bars"></div>
  </div>
  <div class="card">
    <div class="card-label">Radar de áreas · promedio semanal</div>
    <svg id="radar-svg" viewBox="0 0 240 240" width="100%"></svg>
  </div>
</div>

<!-- ÁREAS -->
<div class="panel" id="panel-areas">
  <div class="areas-grid" id="areas-grid"></div>
</div>

<!-- TRACKER -->
<div class="panel" id="panel-tracker">
  <div class="tracker-header">
    <div class="tracker-title">Respuestas nocturnas</div>
    <button class="btn-add" id="btn-add-day">+ Nuevo día</button>
  </div>
  <div class="new-entry" id="new-entry-form">
    <div class="new-entry-label">Nuevo registro</div>
    <input type="text" id="new-day-name" placeholder="Ej: Lun 16">
    <div id="new-q-rows"></div>
    <div class="form-btns">
      <button class="btn-save" id="btn-save-new">Guardar</button>
      <button class="btn-cancel" id="btn-cancel-new">Cancelar</button>
    </div>
  </div>
  <div id="tracker-rows"></div>
</div>

<!-- HEATMAP -->
<div class="panel" id="panel-heatmap">
  <div class="card">
    <div class="card-label">Mapa de calor · cada celda es un día, más oscuro es mejor</div>
    <div class="heat-wrap">
      <div class="heat-inner" id="heat-inner"></div>
    </div>
  </div>
</div>

<script>
const AREAS = [
  { k:"fe",     label:"Fe & espíritu",         color:"#534AB7" },
  { k:"cuerpo", label:"Cuerpo & salud",         color:"#0F6E56" },
  { k:"mente",  label:"Mente & aprendizaje",    color:"#185FA5" },
  { k:"crear",  label:"Creatividad & talentos", color:"#854F0B" },
  { k:"mision", label:"Misión & negocio",       color:"#993C1D" },
  { k:"rel",    label:"Relaciones & familia",   color:"#993556" },
  { k:"car",    label:"Carácter & disciplina",  color:"#444441" },
];

const QS = [
  { q:"¿Puse a Dios primero?",      area:"fe"     },
  { q:"¿Entrené mi cuerpo?",        area:"cuerpo" },
  { q:"¿Fortalecí mi mente?",       area:"mente"  },
  { q:"¿Aprendí algo?",             area:"mente"  },
  { q:"¿Creé algo?",                area:"crear"  },
  { q:"¿Serví a alguien?",          area:"rel"    },
  { q:"¿Amé a mi familia?",         area:"rel"    },
  { q:"¿Fui íntegro?",              area:"car"    },
  { q:"¿Cumplí mi palabra?",        area:"car"    },
  { q:"¿Avancé hacia mi misión?",   area:"mision" },
];

const QS_AREA = { fe:["¿Puse a Dios primero?"], cuerpo:["¿Entrené mi cuerpo?"], mente:["¿Fortalecí mi mente?","¿Aprendí algo?"], crear:["¿Creé algo?"], mision:["¿Avancé hacia mi misión?"], rel:["¿Serví a alguien?","¿Amé a mi familia?"], car:["¿Fui íntegro?","¿Cumplí mi palabra?"] };

const VAL = { "Sí":2, "Parcial":1, "No":0 };

let DATA = [
  { d:"Lun", "¿Puse a Dios primero?":"Sí","¿Entrené mi cuerpo?":"Sí","¿Fortalecí mi mente?":"Sí","¿Aprendí algo?":"Sí","¿Creé algo?":"Parcial","¿Serví a alguien?":"Sí","¿Amé a mi familia?":"Sí","¿Fui íntegro?":"Sí","¿Cumplí mi palabra?":"Parcial","¿Avancé hacia mi misión?":"Sí" },
  { d:"Mar", "¿Puse a Dios primero?":"Sí","¿Entrené mi cuerpo?":"No","¿Fortalecí mi mente?":"Sí","¿Aprendí algo?":"Sí","¿Creé algo?":"Sí","¿Serví a alguien?":"Parcial","¿Amé a mi familia?":"Sí","¿Fui íntegro?":"Sí","¿Cumplí mi palabra?":"Sí","¿Avancé hacia mi misión?":"Parcial" },
  { d:"Mié", "¿Puse a Dios primero?":"Sí","¿Entrené mi cuerpo?":"Sí","¿Fortalecí mi mente?":"Parcial","¿Aprendí algo?":"Parcial","¿Creé algo?":"Sí","¿Serví a alguien?":"Sí","¿Amé a mi familia?":"Parcial","¿Fui íntegro?":"Sí","¿Cumplí mi palabra?":"Sí","¿Avancé hacia mi misión?":"Sí" },
  { d:"Jue", "¿Puse a Dios primero?":"Parcial","¿Entrené mi cuerpo?":"Sí","¿Fortalecí mi mente?":"Sí","¿Aprendí algo?":"Sí","¿Creé algo?":"No","¿Serví a alguien?":"Sí","¿Amé a mi familia?":"Sí","¿Fui íntegro?":"Parcial","¿Cumplí mi palabra?":"Sí","¿Avancé hacia mi misión?":"Sí" },
  { d:"Vie", "¿Puse a Dios primero?":"Sí","¿Entrené mi cuerpo?":"Sí","¿Fortalecí mi mente?":"Sí","¿Aprendí algo?":"Sí","¿Creé algo?":"Sí","¿Serví a alguien?":"Sí","¿Amé a mi familia?":"Sí","¿Fui íntegro?":"Sí","¿Cumplí mi palabra?":"Sí","¿Avancé hacia mi misión?":"Sí" },
  { d:"Sáb", "¿Puse a Dios primero?":"Sí","¿Entrené mi cuerpo?":"Parcial","¿Fortalecí mi mente?":"Parcial","¿Aprendí algo?":"Sí","¿Creé algo?":"Parcial","¿Serví a alguien?":"Sí","¿Amé a mi familia?":"Sí","¿Fui íntegro?":"Sí","¿Cumplí mi palabra?":"Parcial","¿Avancé hacia mi misión?":"Parcial" },
  { d:"Dom", "¿Puse a Dios primero?":"Sí","¿Entrené mi cuerpo?":"No","¿Fortalecí mi mente?":"Sí","¿Aprendí algo?":"Sí","¿Creé algo?":"Parcial","¿Serví a alguien?":"Sí","¿Amé a mi familia?":"Sí","¿Fui íntegro?":"Sí","¿Cumplí mi palabra?":"Sí","¿Avancé hacia mi misión?":"Parcial" },
];

// ── Calculations ──
function mean(arr){ const v=arr.filter(x=>x!=null); return v.length ? v.reduce((a,b)=>a+b,0)/v.length : null; }
function p(v){ return v==null?null:Math.round((v/2)*100); }
function dayScore(row){ const vs=QS.map(({q})=>VAL[row[q]]??null).filter(v=>v!=null); return vs.length?Math.round((mean(vs)/2)*100):null; }
function areaScore(row,ak){ const qs=QS_AREA[ak]; const vs=qs.map(q=>VAL[row[q]]??null).filter(v=>v!=null); return vs.length?p(mean(vs)):null; }
function areaAvg(ak){ const vs=DATA.map(r=>areaScore(r,ak)).filter(v=>v!=null); return vs.length?Math.round(mean(vs)):null; }
function globalAvg(){ const vs=DATA.map(dayScore).filter(v=>v!=null); return vs.length?Math.round(mean(vs)):null; }
function scoreColor(s){ return s>=80?"#0F6E56":s>=60?"#854F0B":"#993C1D"; }
function hex2alpha(hex,a){ const r=parseInt(hex.slice(1,3),16),g=parseInt(hex.slice(3,5),16),b=parseInt(hex.slice(5,7),16); return `rgba(${r},${g},${b},${a})`; }

// ── Render helpers ──
function ring(score, color, r){
  r = r||28;
  const sz=(r+5)*2, cx=r+5, circ=2*Math.PI*r;
  const dash = score==null ? 0 : (score/100)*circ;
  return `<svg width="${sz}" height="${sz}" viewBox="0 0 ${sz} ${sz}" class="ring-wrap">
    <circle cx="${cx}" cy="${cx}" r="${r}" fill="none" stroke="rgba(0,0,0,0.07)" stroke-width="3.5"/>
    <circle cx="${cx}" cy="${cx}" r="${r}" fill="none" stroke="${color}" stroke-width="3.5"
      stroke-dasharray="${dash.toFixed(1)} ${circ.toFixed(1)}" stroke-linecap="round"
      transform="rotate(-90 ${cx} ${cx})" style="transition:stroke-dasharray .5s ease"/>
    <text x="${cx}" y="${cx}" text-anchor="middle" dominant-baseline="central"
      font-size="${Math.round(r*.52)}" font-weight="500" fill="#1a1a1a">${score!=null?score+'%':'—'}</text>
  </svg>`;
}

function spark(vals, color){
  const W=130, H=32, pad=2;
  const valid = vals.filter(v=>v!=null);
  if(!valid.length) return `<div style="height:${H}px"></div>`;
  const n=vals.length;
  const xs = vals.map((_,i)=>pad+i*(W-2*pad)/Math.max(n-1,1));
  const ys = vals.map(v=>v==null?null: H-pad-(v/100)*(H-2*pad));
  const pts = xs.reduce((acc,x,i)=> ys[i]!=null ? acc+(acc?`L${x.toFixed(1)},${ys[i].toFixed(1)}`:`M${x.toFixed(1)},${ys[i].toFixed(1)}`) : acc ,'');
  return `<svg width="${W}" height="${H}" viewBox="0 0 ${W} ${H}" class="spark" style="width:100%">
    <path d="${pts}" fill="none" stroke="${color}" stroke-width="1.5" stroke-linecap="round" stroke-linejoin="round" opacity=".9"/>
  </svg>`;
}

// ── Tab switching ──
document.querySelectorAll('.tab').forEach(btn=>{
  btn.addEventListener('click',()=>{
    document.querySelectorAll('.tab').forEach(b=>b.classList.remove('active'));
    document.querySelectorAll('.panel').forEach(p=>p.classList.remove('visible'));
    btn.classList.add('active');
    document.getElementById('panel-'+btn.dataset.tab).classList.add('visible');
    render();
  });
});

function activeTab(){ return document.querySelector('.tab.active').dataset.tab; }

// ── Master render ──
function render(){
  renderHeader();
  const t = activeTab();
  if(t==='resumen') renderResumen();
  if(t==='areas') renderAreas();
  if(t==='tracker') renderTracker();
  if(t==='heatmap') renderHeatmap();
}

function renderHeader(){
  const g = globalAvg();
  const el = document.getElementById('global-score');
  el.textContent = g!=null ? g+'%' : '—';
  el.style.color = g!=null ? scoreColor(g) : '#a0a0a0';
  document.getElementById('sub-label').textContent = DATA.length+' días registrados esta semana';
}

function renderResumen(){
  const scores = DATA.map(dayScore);
  // metrics
  const bestI = scores.reduce((bi,s,i)=>(s??-1)>(scores[bi]??-1)?i:bi, 0);
  const best = DATA[bestI];
  document.getElementById('m-best').textContent = best ? best.d+' · '+dayScore(best)+'%' : '—';
  document.getElementById('m-racha').textContent = DATA.length+' días';
  const weak = AREAS.filter(a=>{ const s=areaAvg(a.k); return s!=null&&s<70; }).sort((a,b)=>areaAvg(a.k)-areaAvg(b.k));
  const wEl = document.getElementById('m-weak');
  wEl.textContent = weak.length ? weak[0].label.split('&')[0].trim() : '—';
  wEl.style.color = weak.length ? weak[0].color : '#1a1a1a';

  // bars
  const barsEl = document.getElementById('daily-bars');
  barsEl.innerHTML = scores.map((s,i)=>{
    const h = s==null ? 2 : Math.max(2, Math.round((s/100)*56));
    return `<div class="bar-col">
      <div class="bar-val">${s!=null?s+'%':''}</div>
      <div class="bar-rect" style="height:${h}px;background:#534AB7;opacity:${s==null?.15:.82}"></div>
      <div class="bar-day">${DATA[i].d}</div>
    </div>`;
  }).join('');

  // radar
  renderRadar();
}

function renderRadar(){
  const svg = document.getElementById('radar-svg');
  const cx=120,cy=120,R=82,n=AREAS.length;
  const angle = i=>(i/n)*2*Math.PI-Math.PI/2;
  const pt = (i,f)=>({ x:cx+R*f*Math.cos(angle(i)), y:cy+R*f*Math.sin(angle(i)) });

  let html = '';
  // grid
  [.25,.5,.75,1].forEach(f=>{
    const pts = AREAS.map((_,i)=>{ const p=pt(i,f); return `${p.x.toFixed(1)},${p.y.toFixed(1)}`; }).join(' ');
    html += `<polygon points="${pts}" fill="none" stroke="rgba(0,0,0,0.07)" stroke-width="0.5"/>`;
  });
  // spokes
  AREAS.forEach((_,i)=>{ const e=pt(i,1); html+=`<line x1="${cx}" y1="${cy}" x2="${e.x.toFixed(1)}" y2="${e.y.toFixed(1)}" stroke="rgba(0,0,0,0.07)" stroke-width="0.5"/>`; });

  // score polygon
  const avgs = AREAS.map(a=>areaAvg(a.k));
  const spoly = AREAS.map((a,i)=>{ const f=(avgs[i]??0)/100; const p=pt(i,f); return `${p.x.toFixed(1)},${p.y.toFixed(1)}`; }).join(' ');
  html += `<polygon points="${spoly}" fill="#534AB7" fill-opacity="0.15" stroke="#534AB7" stroke-width="1.5"/>`;
  AREAS.forEach((a,i)=>{ const f=(avgs[i]??0)/100; const p2=pt(i,f); html+=`<circle cx="${p2.x.toFixed(1)}" cy="${p2.y.toFixed(1)}" r="3" fill="${a.color}"/>`; });

  // labels
  AREAS.forEach((a,i)=>{
    const lp=pt(i,1.22);
    const name=a.label.split('&')[0].trim();
    html+=`<text x="${lp.x.toFixed(1)}" y="${lp.y.toFixed(1)}" text-anchor="middle" dominant-baseline="middle" font-size="10" fill="#a0a0a0">${name}</text>`;
  });

  svg.innerHTML = html;
}

function renderAreas(){
  const grid = document.getElementById('areas-grid');
  grid.innerHTML = AREAS.map(a=>{
    const s = areaAvg(a.k);
    const days = DATA.map(r=>areaScore(r,a.k));
    const last=days[days.length-1], prev=days[days.length-2];
    const trend = (last!=null&&prev!=null) ? last-prev : null;
    const trendHtml = trend!=null
      ? `<div class="area-trend" style="color:${trend>0?'#0F6E56':trend<0?'#993C1D':'#a0a0a0'}">${trend>0?'↑':trend<0?'↓':' '}${Math.abs(trend)}% vs ayer</div>`
      : '';
    return `<div class="area-card">
      <div class="area-top">
        <div>
          <div class="area-dot" style="background:${a.color}"></div>
          <div class="area-name">${a.label}</div>
          ${trendHtml}
        </div>
        ${ring(s, a.color, 24)}
      </div>
      ${spark(days, a.color)}
    </div>`;
  }).join('');
}

function renderTracker(){
  const container = document.getElementById('tracker-rows');
  container.innerHTML = DATA.map((row,di)=>{
    const s = dayScore(row);
    const sColor = s!=null ? scoreColor(s) : '#a0a0a0';
    const dots = AREAS.map(a=>{
      const v = areaScore(row,a.k);
      const op = v!=null ? Math.max(.15, v/100) : .1;
      return `<div class="day-dot-bar" style="background:${a.color};opacity:${op}"></div>`;
    }).join('');
    const qRows = QS.map(({q,area})=>{
      const ar = AREAS.find(a=>a.k===area);
      const cur = row[q];
      return `<div class="q-row">
        <div class="q-area-dot" style="background:${ar.color}"></div>
        <div class="q-text">${q}</div>
        <div class="q-btns">
          <button class="vbtn si${cur==='Sí'?' active':''}" data-di="${di}" data-q="${q}" data-v="Sí">Sí</button>
          <button class="vbtn par${cur==='Parcial'?' active':''}" data-di="${di}" data-q="${q}" data-v="Parcial">Parcial</button>
          <button class="vbtn no${cur==='No'?' active':''}" data-di="${di}" data-q="${q}" data-v="No">No</button>
        </div>
      </div>`;
    }).join('');
    return `<div class="day-row" id="dayrow-${di}">
      <div class="day-header" data-di="${di}">
        <div class="day-left">
          <div class="day-name">${row.d}</div>
          <div class="day-dots">${dots}</div>
        </div>
        <div class="day-right">
          <div class="day-score" style="color:${sColor}">${s!=null?s+'%':'—'}</div>
          <div class="chevron" id="chev-${di}">▾</div>
        </div>
      </div>
      <div class="day-body" id="body-${di}">${qRows}</div>
    </div>`;
  }).join('');

  // header toggle
  container.querySelectorAll('.day-header').forEach(h=>{
    h.addEventListener('click',()=>{
      const di = h.dataset.di;
      const body = document.getElementById('body-'+di);
      const chev = document.getElementById('chev-'+di);
      body.classList.toggle('open');
      chev.classList.toggle('open');
    });
  });

  // vote buttons
  container.querySelectorAll('.vbtn').forEach(btn=>{
    btn.addEventListener('click',e=>{
      e.stopPropagation();
      const {di,q,v} = btn.dataset;
      DATA[+di][q] = v;
      renderTracker();
      renderHeader();
    });
  });
}

function renderHeatmap(){
  const inner = document.getElementById('heat-inner');
  const dayLabels = `<div class="heat-days-header">${DATA.map(r=>`<div class="heat-day-label">${r.d}</div>`).join('')}</div>`;
  const rows = AREAS.map(a=>{
    const cells = DATA.map(r=>{
      const s = areaScore(r,a.k);
      const bg = s==null ? 'rgba(0,0,0,0.06)'
        : s>=90 ? a.color
        : s>=65 ? hex2alpha(a.color,.75)
        : s>=35 ? hex2alpha(a.color,.4)
        : hex2alpha(a.color,.15);
      return `<div class="heat-cell" style="background:${bg}" title="${a.label}: ${s!=null?s+'%':'—'}"></div>`;
    }).join('');
    return `<div class="heat-row">
      <div class="heat-area-label">${a.label}</div>
      <div class="heat-cells">${cells}</div>
    </div>`;
  }).join('');
  const legend = `<div class="heat-legend">
    ${[['bajo','rgba(83,74,183,.15)'],['medio','rgba(83,74,183,.4)'],['alto','rgba(83,74,183,.75)'],['óptimo','#534AB7']].map(([l,bg])=>`<div class="heat-legend-item"><div class="heat-legend-swatch" style="background:${bg}"></div>${l}</div>`).join('')}
  </div>`;
  inner.innerHTML = dayLabels + rows + legend;
}

// ── New entry form ──
const newForm = document.getElementById('new-entry-form');
const newQRows = document.getElementById('new-q-rows');
let newEntry = { d:'' };

function renderNewForm(){
  newQRows.innerHTML = QS.map(({q,area})=>{
    const ar = AREAS.find(a=>a.k===area);
    const cur = newEntry[q];
    return `<div class="q-row">
      <div class="q-area-dot" style="background:${ar.color}"></div>
      <div class="q-text">${q}</div>
      <div class="q-btns">
        <button class="vbtn si${cur==='Sí'?' active':''}" data-q="${q}" data-v="Sí">Sí</button>
        <button class="vbtn par${cur==='Parcial'?' active':''}" data-q="${q}" data-v="Parcial">Parcial</button>
        <button class="vbtn no${cur==='No'?' active':''}" data-q="${q}" data-v="No">No</button>
      </div>
    </div>`;
  }).join('');
  newQRows.querySelectorAll('.vbtn').forEach(btn=>{
    btn.addEventListener('click',()=>{
      newEntry[btn.dataset.q] = btn.dataset.v;
      renderNewForm();
    });
  });
}

document.getElementById('btn-add-day').addEventListener('click',()=>{
  newEntry = { d:'' };
  newForm.classList.add('open');
  renderNewForm();
  document.getElementById('new-day-name').focus();
});

document.getElementById('new-day-name').addEventListener('input',e=>{ newEntry.d = e.target.value; });

document.getElementById('btn-save-new').addEventListener('click',()=>{
  if(!newEntry.d) return;
  DATA.push({...newEntry});
  newEntry = { d:'' };
  newForm.classList.remove('open');
  render();
});

document.getElementById('btn-cancel-new').addEventListener('click',()=>{
  newForm.classList.remove('open');
});

// ── Init ──
render();
</script>
</body>
</html>
