<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Man in the Mirror</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,300;1,400&family=Inter:wght@300;400;500&display=swap" rel="stylesheet">
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
    font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
    background: var(--bg);
    color: var(--txt);
    font-size: 13px;
    font-weight: 300;
    line-height: 1.5;
    padding: 24px 20px 40px;
    max-width: 680px;
    margin: 0 auto;
  }

  /* ── header ── */
  .header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 24px; }
  .header-label { font-size: 10px; color: var(--txt3); letter-spacing: 0.18em; text-transform: uppercase; margin-bottom: 5px; font-family: 'Inter', sans-serif; }
  .header-title { font-size: 36px; font-weight: 300; letter-spacing: -0.5px; color: var(--txt); font-family: 'Cormorant Garamond', Georgia, serif; line-height: 1.1; }
  .header-sub { font-size: 11px; color: var(--txt3); margin-top: 5px; font-weight: 300; letter-spacing: 0.02em; }
  .header-score { text-align: right; }
  .header-score-num { font-size: 42px; font-weight: 300; letter-spacing: -2px; line-height: 1; font-family: 'Cormorant Garamond', Georgia, serif; }
  .header-score-label { font-size: 10px; color: var(--txt3); margin-top: 3px; text-transform: uppercase; letter-spacing: 0.1em; }

  /* ── tabs ── */
  .tabs { display: flex; gap: 4px; margin-bottom: 20px; padding-bottom: 16px; border-bottom: 0.5px solid var(--border); }
  .tab { padding: 5px 14px; border-radius: 20px; font-size: 11px; font-weight: 400; cursor: pointer; border: 0.5px solid var(--border2); background: transparent; color: var(--txt2); font-family: 'Inter', sans-serif; transition: all .15s; }
  .tab:hover { background: var(--bg2); }
  .tab.active { background: var(--txt); color: var(--bg); border-color: var(--txt); font-weight: 500; }

  /* ── panels ── */
  .panel { display: none; }
  .panel.visible { display: block; }

  /* ── metric cards ── */
  .metrics { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; margin-bottom: 12px; }
  .metric { background: var(--bg2); border-radius: var(--radius-sm); padding: 12px 14px; }
  .metric-label { font-size: 10px; color: var(--txt3); text-transform: uppercase; letter-spacing: 0.08em; margin-bottom: 6px; }
  .metric-val { font-size: 20px; font-weight: 300; color: var(--txt); line-height: 1.3; font-family: 'Cormorant Garamond', Georgia, serif; }

  /* ── card & charts ── */
  .card { background: var(--bg); border: 0.5px solid var(--border); border-radius: var(--radius); padding: 16px 18px; margin-bottom: 10px; }
  .card-label { font-size: 10px; color: var(--txt3); margin-bottom: 12px; letter-spacing: 0.08em; text-transform: uppercase; }
  
  .bars { display: flex; align-items: flex-end; gap: 5px; height: 64px; }
  .bar-col { flex: 1; display: flex; flex-direction: column; align-items: center; gap: 3px; height: 100%; justify-content: flex-end; }
  .bar-val { font-size: 9px; color: var(--txt3); font-weight: 500; }
  .bar-rect { width: 100%; border-radius: 3px 3px 0 0; min-height: 2px; }
  .bar-day { font-size: 9px; color: var(--txt3); margin-top: 3px; }

  /* ── areas grid ── */
  .areas-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(190px, 1fr)); gap: 8px; }
  .area-card { background: var(--bg); border: 0.5px solid var(--border); border-radius: var(--radius); padding: 14px 16px; }
  .area-top { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 10px; }
  .area-dot { width: 7px; height: 7px; border-radius: 50%; margin-bottom: 5px; }
  .area-name { font-size: 14px; font-family: 'Cormorant Garamond', Georgia, serif; color: var(--txt); max-width: 100px; line-height: 1.35; }
  .area-trend { font-size: 10px; margin-top: 3px; }
  .ring-wrap { flex-shrink: 0; }
  .spark { margin-top: 4px; }

  /* ── tracker & form ── */
  .tracker-header { display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; }
  .tracker-title { font-size: 20px; font-family: 'Cormorant Garamond', Georgia, serif; }
  .btn-add { font-size: 11px; padding: 5px 14px; border-radius: 7px; border: none; background: var(--txt); color: var(--bg); cursor: pointer; font-family: inherit; font-weight: 500; }

  .day-row { background: var(--bg); border: 0.5px solid var(--border); border-radius: var(--radius); margin-bottom: 6px; overflow: hidden; }
  .day-header { display: flex; justify-content: space-between; align-items: center; padding: 12px 16px; cursor: pointer; user-select: none; }
  .day-header:hover { background: var(--bg2); }
  .day-left { display: flex; align-items: center; gap: 10px; }
  .day-name { font-size: 18px; font-family: 'Cormorant Garamond', Georgia, serif; }
  .day-dots { display: flex; gap: 3px; align-items: center; }
  .day-dot-bar { width: 4px; height: 18px; border-radius: 2px; }
  .day-right { display: flex; align-items: center; gap: 10px; }
  .day-score { font-size: 13px; font-weight: 500; }
  .chevron { font-size: 11px; color: var(--txt3); transition: transform .2s; }
  .chevron.open { transform: rotate(180deg); }
  
  .day-body { display: none; padding: 0 16px 14px; border-top: 0.5px solid var(--border); }
  .day-body.open { display: block; }
  
  .section-title { font-size: 10px; color: var(--txt3); text-transform: uppercase; letter-spacing: 0.08em; margin: 16px 0 10px; font-weight: 500; }
  
  /* Inputs de texto */
  .day-note-input { width: 100%; border: 0.5px solid var(--border2); border-radius: var(--radius-sm); padding: 10px 12px; font-size: 12px; font-family: inherit; background: var(--bg); color: var(--txt); resize: vertical; min-height: 60px; outline: none; margin-bottom: 4px; }
  .day-note-input:focus { border-color: var(--fe); }

  .habit-row-wrap { margin-bottom: 8px; border-bottom: 0.5px solid var(--border); padding-bottom: 8px; }
  .habit-top { display: flex; align-items: center; gap: 8px; cursor: pointer; user-select: none; margin-bottom: 6px; }
  .habit-note-input { width: calc(100% - 24px); border: 0.5px solid var(--border2); border-radius: 4px; padding: 5px 8px; font-size: 11px; font-family: inherit; background: var(--bg); color: var(--txt); outline: none; margin-left: 24px; }
  .habit-note-input::placeholder { color: var(--txt3); }
  .habit-note-input:focus { border-color: var(--fe); }

  .q-row { display: flex; justify-content: space-between; align-items: center; gap: 8px; margin-bottom: 8px; }
  .q-area-dot { width: 5px; height: 5px; border-radius: 50%; flex-shrink: 0; }
  .q-text { flex: 1; font-size: 11px; color: var(--txt2); }
  .q-btns { display: flex; gap: 3px; }
  .vbtn { padding: 3px 9px; border-radius: 20px; font-size: 10px; cursor: pointer; border: 0.5px solid var(--border); background: transparent; color: var(--txt2); font-family: inherit; white-space: nowrap; }
  .vbtn.si.active { background: #E1F5EE; color: #085041; border-color: #E1F5EE; font-weight: 500; }
  .vbtn.par.active { background: #FAEEDA; color: #633806; border-color: #FAEEDA; font-weight: 500; }
  .vbtn.no.active { background: #FCEBEB; color: #791F1F; border-color: #FCEBEB; font-weight: 500; }

  /* ── heatmap ── */
  .heat-wrap { overflow-x: auto; }
  .heat-inner { min-width: 420px; }
  .heat-days-header { display: flex; margin-left: 154px; gap: 4px; margin-bottom: 5px; }
  .heat-day-label { width: 28px; font-size: 9px; color: var(--txt3); text-align: center; flex-shrink: 0; }
  .heat-row { display: flex; align-items: center; gap: 8px; margin-bottom: 4px; }
  .heat-area-label { width: 146px; font-size: 11px; color: var(--txt2); text-align: right; flex-shrink: 0; }
  .heat-cells { display: flex; gap: 4px; }
  .heat-cell { width: 28px; height: 28px; border-radius: 5px; flex-shrink: 0; }
  .heat-legend { display: flex; gap: 12px; margin-top: 14px; margin-left: 154px; font-size: 10px; color: var(--txt3); }
  .heat-legend-swatch { width: 12px; height: 12px; border-radius: 3px; margin-right: 4px; display: inline-block; vertical-align: middle; }

  #radar-svg { display: block; margin: 0 auto; max-width: 210px; }
</style>
</head>
<body>

<div class="header">
  <div>
    <div class="header-label">man in the mirror</div>
    <div class="header-title">Estadísticas de vida</div>
    <div class="header-sub" id="sub-label">0 días registrados</div>
  </div>
  <div class="header-score">
    <div class="header-score-num" id="global-score">—</div>
    <div class="header-score-label">promedio global</div>
  </div>
</div>

<div class="tabs">
  <button class="tab active" data-tab="resumen">Resumen</button>
  <button class="tab" data-tab="areas">Áreas</button>
  <button class="tab" data-tab="habitos">Hábitos (Hoy)</button>
  <button class="tab" data-tab="tracker">Reflexión</button>
  <button class="tab" data-tab="heatmap">Heatmap</button>
</div>

<!-- RESUMEN -->
<div class="panel visible" id="panel-resumen">
  <div class="metrics">
    <div class="metric"><div class="metric-label">mejor día</div><div class="metric-val" id="m-best">—</div></div>
    <div class="metric"><div class="metric-label">racha</div><div class="metric-val" id="m-racha">—</div></div>
    <div class="metric"><div class="metric-label">área débil</div><div class="metric-val" id="m-weak">—</div></div>
  </div>
  <div class="card">
    <div class="card-label">Puntuación diaria (Hábitos 75% + Reflexión 25%)</div>
    <div class="bars" id="daily-bars"></div>
  </div>
  <div class="card">
    <div class="card-label">Radar de áreas · promedio general</div>
    <svg id="radar-svg" viewBox="0 0 240 240" width="100%"></svg>
  </div>
</div>

<!-- ÁREAS -->
<div class="panel" id="panel-areas"><div class="areas-grid" id="areas-grid"></div></div>

<!-- HÁBITOS (HOY) -->
<div class="panel" id="panel-habitos">
  <div class="tracker-header">
    <div class="tracker-title">Hábitos del último día</div>
  </div>
  <div id="habitos-grid"></div>
</div>

<!-- TRACKER REFLEXIÓN -->
<div class="panel" id="panel-tracker">
  <div class="tracker-header">
    <div class="tracker-title">Historial y Reflexión</div>
    <button class="btn-add" id="btn-add-day">+ Nuevo día</button>
  </div>
  <div id="tracker-rows"></div>
</div>

<!-- HEATMAP -->
<div class="panel" id="panel-heatmap">
  <div class="card">
    <div class="card-label">Mapa de calor · cada celda es un día</div>
    <div class="heat-wrap"><div class="heat-inner" id="heat-inner"></div></div>
  </div>
</div>

<script>
const STORAGE_KEY = 'man_in_the_mirror_v2'; // Clave para estructura limpia

const AREAS = [
  { k:"fe",     label:"Fe & espíritu",         color:"#7B72E9" },
  { k:"mente",  label:"Mente & aprendizaje",   color:"#3A8FE8" },
  { k:"mision", label:"Misión & negocio",      color:"#E05A32" },
  { k:"cuerpo", label:"Cuerpo & salud",        color:"#2DB88F" },
  { k:"car",    label:"Carácter & disciplina", color:"#888884" },
];

const HABITOS = {
  fe: [
    "Leer la Biblia 30 min",
    "Reflexión nocturna (agradecimientos + qué mejorar)",
    "Ver Smallville"
  ],
  mente: [
    "Leer You Can't Hurt Me 30 min",
    "Estudiar Heroes Academia 1 hora",
    "Medir cortisol (1–10, 10 = bajo cortisol)"
  ],
  mision: [
    "aplicación chef",
    "aprendizaje chef",
    "el diario del chef",
    "Ayudar en casa (cocinar o arreglar)"
  ],
  cuerpo: [
    "Entrenar",
    "3 litros de agua",
    "140g proteína",
    "240g carbohidratos",
    "70g grasas",
    "Ducha fría",
    "Dormir 8–9 horas",
    "Medir energía (1–10)"
  ],
  car: [
    "Cumplir horario del día",
    "Mantener espacio limpio y ordenado",
    "Evitar distracciones innecesarias"
  ]
};

// Preguntas corregidas sin Hopecast y enfocadas en la misión actual
const QS = [
  { q:"¿Conecté con Dios hoy?",                  area:"fe" },
  { q:"¿Fui agradecido durante el día?",         area:"fe" },
  { q:"¿Viví con propósito y paz interior?",     area:"fe" },
  { q:"¿Aprendí algo nuevo hoy?",                area:"mente" },
  { q:"¿Sostuve mi cortisol bajo?",              area:"mente" },
  { q:"¿Mi mente estuvo enfocada y clara?",      area:"mente" },
  { q:"¿Fui constante en mi camino como chef?",  area:"mision" },
  { q:"¿Avancé en mi diario y aprendizajes?",    area:"mision" },
  { q:"¿Aportó valor mi trabajo de hoy?",        area:"mision" },
  { q:"¿Entrené con intensidad?",                area:"cuerpo" },
  { q:"¿Cumplí mi nutrición del día?",           area:"cuerpo" },
  { q:"¿Mi energía fue alta hoy (7+/10)?",       area:"cuerpo" },
  { q:"¿Cumplí mis horarios?",                   area:"car" },
  { q:"¿Fui íntegro cuando nadie miraba?",       area:"car" },
  { q:"¿Cumplí todo lo que prometí?",            area:"car" }
];

const QS_AREA = {
  fe: QS.filter(q=>q.area==="fe").map(q=>q.q),
  mente: QS.filter(q=>q.area==="mente").map(q=>q.q),
  mision: QS.filter(q=>q.area==="mision").map(q=>q.q),
  cuerpo: QS.filter(q=>q.area==="cuerpo").map(q=>q.q),
  car: QS.filter(q=>q.area==="car").map(q=>q.q),
};

const VAL = { "Sí":2, "Parcial":1, "No":0 };
let DATA = [];

function loadData() {
  try {
    const saved = localStorage.getItem(STORAGE_KEY);
    if (saved) {
      DATA = JSON.parse(saved);
      // Migración si vienen del formato viejo
      if(DATA.data) {
         DATA = DATA.data.map(r => {
             let nr = { d: r.d, note: '', qs: {}, habits: defaultHabits() };
             QS.forEach(q => { if(r[q.q]) nr.qs[q.q] = r[q.q]; });
             return nr;
         });
      }
    }
  } catch(e) {}
  render();
}

function saveData() { localStorage.setItem(STORAGE_KEY, JSON.stringify(DATA)); }

function defaultHabits() {
  let h = {};
  Object.values(HABITOS).flat().forEach(hab => { h[hab] = { done: false, note: '' }; });
  return h;
}

// ── Calculadoras de 75% Hábitos / 25% Reflexión ──
function mean(arr){ return arr.length ? arr.reduce((a,b)=>a+b,0)/arr.length : 0; }

function getHabScore(row, ak) {
  if(!row.habits) return null;
  let habs = ak ? HABITOS[ak] : Object.values(HABITOS).flat();
  if(!habs.length) return null;
  let done = 0;
  habs.forEach(h => { if(row.habits[h] && row.habits[h].done) done++; });
  return (done / habs.length) * 100;
}

function getRefScore(row, ak) {
  let qsList = ak ? QS_AREA[ak] : QS.map(q=>q.q);
  let vals = qsList.map(q => VAL[row.qs ? row.qs[q] : null]).filter(v => v != null);
  return vals.length ? (mean(vals)/2)*100 : null;
}

function calcTotal(hS, rS) {
  if(hS == null && rS == null) return null;
  if(hS == null) return Math.round(rS);
  if(rS == null) return Math.round(hS);
  return Math.round((hS * 0.75) + (rS * 0.25)); // 75% hábitos, 25% reflexión
}

function dayScore(row) { return calcTotal(getHabScore(row), getRefScore(row)); }
function areaScore(row, ak) { return calcTotal(getHabScore(row, ak), getRefScore(row, ak)); }
function areaAvg(ak){ const vs=DATA.map(r=>areaScore(r,ak)).filter(v=>v!=null); return vs.length?Math.round(mean(vs)):null; }
function globalAvg(){ const vs=DATA.map(dayScore).filter(v=>v!=null); return vs.length?Math.round(mean(vs)):null; }
function scoreColor(s){ return s>=80?"#2DB88F":s>=60?"#E8931A":"#E05A32"; }

// ── UI Components ──
function ring(score, color, r=28){
  const sz=(r+5)*2, cx=r+5, circ=2*Math.PI*r, dash=score==null?0:(score/100)*circ;
  const tCol = window.matchMedia('(prefers-color-scheme: dark)').matches ? '#f0f0f0' : '#1a1a1a';
  const trCol = window.matchMedia('(prefers-color-scheme: dark)').matches ? 'rgba(255,255,255,0.08)' : 'rgba(0,0,0,0.07)';
  return `<svg width="${sz}" height="${sz}" viewBox="0 0 ${sz} ${sz}" class="ring-wrap">
    <circle cx="${cx}" cy="${cx}" r="${r}" fill="none" stroke="${trCol}" stroke-width="3.5"/>
    <circle cx="${cx}" cy="${cx}" r="${r}" fill="none" stroke="${color}" stroke-width="3.5" stroke-dasharray="${dash.toFixed(1)} ${circ.toFixed(1)}" transform="rotate(-90 ${cx} ${cx})" stroke-linecap="round"/>
    <text x="${cx}" y="${cx}" text-anchor="middle" dominant-baseline="central" font-size="${Math.round(r*.52)}" font-weight="500" fill="${tCol}">${score!=null?score+'%':'—'}</text>
  </svg>`;
}
function spark(vals, color){
  const W=130, H=32, pad=2, valid=vals.filter(v=>v!=null);
  if(!valid.length) return `<div style="height:${H}px"></div>`;
  const xs=vals.map((_,i)=>pad+i*(W-2*pad)/Math.max(vals.length-1,1));
  const ys=vals.map(v=>v==null?null:H-pad-(v/100)*(H-2*pad));
  const pts=xs.reduce((a,x,i)=>ys[i]!=null?a+(a?`L${x},${ys[i]}`:`M${x},${ys[i]}`):a,'');
  return `<svg width="${W}" height="${H}" class="spark"><path d="${pts}" fill="none" stroke="${color}" stroke-width="1.5" stroke-linecap="round" opacity=".9"/></svg>`;
}

// ── TABS ──
document.querySelectorAll('.tab').forEach(btn => {
  btn.addEventListener('click', () => {
    document.querySelectorAll('.tab').forEach(b=>b.classList.remove('active'));
    document.querySelectorAll('.panel').forEach(p=>p.classList.remove('visible'));
    btn.classList.add('active');
    document.getElementById('panel-'+btn.dataset.tab).classList.add('visible');
    render();
  });
});
function activeTab(){ return document.querySelector('.tab.active').dataset.tab; }

// ── Renders ──
function render(){
  renderHeader();
  const t = activeTab();
  if(t==='resumen') renderResumen();
  if(t==='areas') renderAreas();
  if(t==='habitos') renderHabitos();
  if(t==='tracker') renderTracker();
  if(t==='heatmap') renderHeatmap();
}

function renderHeader(){
  const g = globalAvg();
  const el = document.getElementById('global-score');
  el.textContent = g!=null ? g+'%' : '—';
  el.style.color = g!=null ? scoreColor(g) : 'var(--txt3)';
  document.getElementById('sub-label').textContent = DATA.length+' días registrados';
}

function renderResumen(){
  if(!DATA.length) { document.getElementById('daily-bars').innerHTML='<div style="color:var(--txt3);font-size:12px;padding:10px 0">Añade un día en "Reflexión".</div>'; return; }
  const scores = DATA.map(dayScore);
  const bestI = scores.reduce((bi,s,i)=>(s??-1)>(scores[bi]??-1)?i:bi, 0);
  document.getElementById('m-best').textContent = DATA[bestI] ? DATA[bestI].d+' · '+dayScore(DATA[bestI])+'%' : '—';
  document.getElementById('m-racha').textContent = DATA.length+' días';
  const weak = AREAS.filter(a=>areaAvg(a.k)!=null).sort((a,b)=>areaAvg(a.k)-areaAvg(b.k))[0];
  const wEl = document.getElementById('m-weak');
  wEl.textContent = weak ? weak.label.split('&')[0].trim() : '—';
  wEl.style.color = weak ? weak.color : 'var(--txt)';
  
  document.getElementById('daily-bars').innerHTML = scores.map((s,i) => {
    const h = s==null ? 2 : Math.max(2, (s/100)*56);
    return `<div class="bar-col"><div class="bar-val">${s!=null?s+'%':''}</div><div class="bar-rect" style="height:${h}px;background:#7B72E9;opacity:${s==null?.15:.82}"></div><div class="bar-day">${DATA[i].d}</div></div>`;
  }).join('');
  renderRadar();
}

function renderRadar(){
  const svg = document.getElementById('radar-svg');
  const cx=120,cy=120,R=82, avgs=AREAS.map(a=>areaAvg(a.k)), isDark=window.matchMedia('(prefers-color-scheme: dark)').matches;
  const p = (i,f) => { const a=(i/AREAS.length)*2*Math.PI-Math.PI/2; return `${(cx+R*f*Math.cos(a)).toFixed(1)},${(cy+R*f*Math.sin(a)).toFixed(1)}`; };
  let html = [.25,.5,.75,1].map(f => `<polygon points="${AREAS.map((_,i)=>p(i,f)).join(' ')}" fill="none" stroke="${isDark?'rgba(255,255,255,0.08)':'rgba(0,0,0,0.07)'}" stroke-width="0.5"/>`).join('');
  AREAS.forEach((_,i)=>html+=`<line x1="${cx}" y1="${cy}" x2="${p(i,1).split(',')[0]}" y2="${p(i,1).split(',')[1]}" stroke="${isDark?'rgba(255,255,255,0.08)':'rgba(0,0,0,0.07)'}" stroke-width="0.5"/>`);
  const poly = AREAS.map((_,i)=>p(i,(avgs[i]??0)/100)).join(' ');
  html += `<polygon points="${poly}" fill="#7B72E9" fill-opacity="0.15" stroke="#7B72E9" stroke-width="1.5"/>`;
  AREAS.forEach((a,i)=> {
    const coords = p(i,(avgs[i]??0)/100).split(',');
    html+=`<circle cx="${coords[0]}" cy="${coords[1]}" r="3" fill="${a.color}"/>`;
    const lc = p(i,1.22).split(',');
    html+=`<text x="${lc[0]}" y="${lc[1]}" text-anchor="middle" dominant-baseline="middle" font-size="10" fill="${isDark?'#555':'#a0a0a0'}">${a.label.split('&')[0].trim()}</text>`;
  });
  svg.innerHTML = html;
}

function renderAreas(){
  document.getElementById('areas-grid').innerHTML = !DATA.length ? '<div style="color:var(--txt3);font-size:12px">Sin datos</div>' : AREAS.map(a=>{
    const s = areaAvg(a.k), days = DATA.map(r=>areaScore(r,a.k)), last=days[days.length-1], prev=days[days.length-2], tr = (last!=null&&prev!=null) ? last-prev : null;
    return `<div class="area-card"><div class="area-top"><div><div class="area-dot" style="background:${a.color}"></div><div class="area-name">${a.label}</div>
      ${tr!=null?`<div class="area-trend" style="color:${tr>0?'#2DB88F':tr<0?'#E05A32':'var(--txt3)'}">${tr>0?'↑':tr<0?'↓':''} ${Math.abs(tr)}% vs ayer</div>`:''}
    </div>${ring(s,a.color,24)}</div>${spark(days,a.color)}</div>`;
  }).join('');
}

// Renderiza los hábitos del último día creado (acceso rápido)
function renderHabitos(){
  const grid = document.getElementById('habitos-grid');
  if(!DATA.length){ grid.innerHTML = '<div style="color:var(--txt3);font-size:12px;padding:20px 0">Ve a Reflexión y crea un nuevo día primero.</div>'; return; }
  
  const di = DATA.length - 1; // Índice del último día
  const row = DATA[di];
  
  grid.innerHTML = `<div style="margin-bottom:12px;font-size:11px;color:var(--txt3)">Viendo los hábitos de: <strong style="color:var(--txt)">${row.d}</strong></div>` + 
  AREAS.map(a => {
    const list = HABITOS[a.k];
    let completados = 0;
    const items = list.map(h => {
      if(!row.habits) row.habits = defaultHabits();
      const hd = row.habits[h] || {done:false, note:''};
      if(hd.done) completados++;
      return `
        <div class="habit-row-wrap">
          <div class="habit-top" onclick="toggleHabit(${di}, '${h}')">
             <div style="width:16px;height:16px;border-radius:4px;border:1.5px solid ${hd.done?a.color:'var(--border2)'};background:${hd.done?a.color:'transparent'};display:flex;align-items:center;justify-content:center;">
                ${hd.done?'<svg width="10" height="10"><polyline points="1.5,5 4,7.5 8.5,2.5" stroke="white" stroke-width="1.5" fill="none"/></svg>':''}
             </div>
             <div style="font-size:12px;color:${hd.done?'var(--txt3)':'var(--txt)'};text-decoration:${hd.done?'line-through':'none'}">${h}</div>
          </div>
          <input type="text" class="habit-note-input" data-di="${di}" data-h="${h}" value="${hd.note||''}" placeholder="Escribe un detalle de este hábito...">
        </div>`;
    }).join('');
    const pct = list.length ? Math.round((completados/list.length)*100) : 0;
    return `<div class="card" style="margin-bottom:8px">
      <div style="display:flex;justify-content:space-between;margin-bottom:10px;"><div style="display:flex;align-items:center;gap:8px;"><div style="width:7px;height:7px;border-radius:50%;background:${a.color}"></div><div style="font-weight:500">${a.label}</div></div><div style="font-size:11px;color:${pct===100?a.color:'var(--txt3)'}">${completados}/${list.length}</div></div>
      <div style="height:3px;background:var(--bg3);border-radius:2px;margin-bottom:12px;"><div style="height:3px;background:${a.color};border-radius:2px;width:${pct}%"></div></div>
      ${items}
    </div>`;
  }).join('');
}

function renderTracker(){
  const container = document.getElementById('tracker-rows');
  container.innerHTML = DATA.map((row, di) => {
    const s = dayScore(row), sCol = s!=null ? scoreColor(s) : 'var(--txt3)';
    const dots = AREAS.map(a=>{ const v=areaScore(row,a.k); return `<div class="day-dot-bar" style="background:${a.color};opacity:${v!=null?Math.max(.15,v/100):.1}"></div>`; }).join('');
    
    // Hábitos HTML
    const habsHtml = AREAS.map(a => {
      return HABITOS[a.k].map(h => {
        const hd = (row.habits && row.habits[h]) ? row.habits[h] : {done:false, note:''};
        return `<div class="habit-row-wrap">
          <div class="habit-top" onclick="toggleHabit(${di}, '${h}')">
             <div style="width:16px;height:16px;border-radius:4px;border:1.5px solid ${hd.done?a.color:'var(--border2)'};background:${hd.done?a.color:'transparent'};display:flex;align-items:center;justify-content:center;">
                ${hd.done?'<svg width="10" height="10"><polyline points="1.5,5 4,7.5 8.5,2.5" stroke="white" stroke-width="1.5" fill="none"/></svg>':''}
             </div>
             <div style="font-size:12px;color:${hd.done?'var(--txt3)':'var(--txt)'};text-decoration:${hd.done?'line-through':'none'}">${h}</div>
          </div>
          <input type="text" class="habit-note-input" data-di="${di}" data-h="${h}" value="${hd.note||''}" placeholder="Nota de este hábito...">
        </div>`;
      }).join('');
    }).join('');

    // Reflexión HTML
    const qsHtml = QS.map(({q,area})=>{
      const aCol = AREAS.find(a=>a.k===area).color;
      const c = row.qs ? row.qs[q] : null;
      return `<div class="q-row"><div class="q-area-dot" style="background:${aCol}"></div><div class="q-text">${q}</div><div class="q-btns">
        <button class="vbtn si${c==='Sí'?' active':''}" onclick="setQ(${di},'${q}','Sí')">Sí</button>
        <button class="vbtn par${c==='Parcial'?' active':''}" onclick="setQ(${di},'${q}','Parcial')">Parcial</button>
        <button class="vbtn no${c==='No'?' active':''}" onclick="setQ(${di},'${q}','No')">No</button>
      </div></div>`;
    }).join('');

    return `<div class="day-row" id="dayrow-${di}">
      <div class="day-header" onclick="toggleDay(${di})">
        <div class="day-left"><div class="day-name">${row.d}</div><div class="day-dots">${dots}</div></div>
        <div class="day-right"><div class="day-score" style="color:${sCol}">${s!=null?s+'%':'—'}</div><div class="chevron" id="chev-${di}">▾</div></div>
      </div>
      <div class="day-body" id="body-${di}">
        <div class="section-title">Nota del Día</div>
        <textarea class="day-note-input" data-di="${di}" placeholder="¿Cómo estuvo tu día? Escribe aquí tus pensamientos...">${row.note||''}</textarea>
        
        <div class="section-title">Hábitos (75% del puntaje)</div>
        ${habsHtml}

        <div class="section-title" style="margin-top:20px">Reflexión Nocturna (25% del puntaje)</div>
        ${qsHtml}
      </div>
    </div>`;
  }).reverse().join('');
}

function renderHeatmap(){
  const inner = document.getElementById('heat-inner');
  if(!DATA.length) { inner.innerHTML='<div style="color:var(--txt3);font-size:12px">Sin datos</div>'; return; }
  inner.innerHTML = `<div class="heat-days-header">${DATA.map(r=>`<div class="heat-day-label">${r.d}</div>`).join('')}</div>` +
    AREAS.map(a=>`<div class="heat-row"><div class="heat-area-label">${a.label}</div><div class="heat-cells">
      ${DATA.map(r=>{ const s=areaScore(r,a.k); const c=s==null?'var(--bg3)':s>=90?a.color:s>=65?a.color+'bf':s>=35?a.color+'66':a.color+'26'; return `<div class="heat-cell" style="background:${c}"></div>`;}).join('')}
    </div></div>`).join('') +
    `<div class="heat-legend">${[['bajo','rgba(123,114,233,.15)'],['medio','rgba(123,114,233,.4)'],['alto','rgba(123,114,233,.75)'],['óptimo','#7B72E9']].map(([l,c])=>`<div><div class="heat-legend-swatch" style="background:${c}"></div>${l}</div>`).join('')}</div>`;
}

// ── Interacciones ──
function toggleDay(di){ 
  document.getElementById('body-'+di).classList.toggle('open'); 
  document.getElementById('chev-'+di).classList.toggle('open'); 
}
function toggleHabit(di, h){
  if(!DATA[di].habits) DATA[di].habits = defaultHabits();
  DATA[di].habits[h].done = !DATA[di].habits[h].done;
  saveData(); render();
}
function setQ(di, q, v){
  if(!DATA[di].qs) DATA[di].qs = {};
  DATA[di].qs[q] = v;
  saveData(); render();
}

// Nuevo Día (Simple y directo)
document.getElementById('btn-add-day').addEventListener('click', () => {
  const d = prompt("Nombre del nuevo día (Ej: Lun 16):");
  if(!d) return;
  DATA.push({ d: d.trim(), note: '', qs: {}, habits: defaultHabits() });
  saveData(); render();
  setTimeout(() => { toggleDay(DATA.length - 1); }, 50); // Abre el nuevo día para llenarlo
});

// Autoguardado sin perder foco para los inputs de texto
document.addEventListener('input', e => {
  if(e.target.classList.contains('day-note-input')){
    DATA[e.target.dataset.di].note = e.target.value;
    saveData();
  }
  if(e.target.classList.contains('habit-note-input')){
    DATA[e.target.dataset.di].habits[e.target.dataset.h].note = e.target.value;
    saveData();
  }
});

// Iniciar
document.addEventListener('DOMContentLoaded', loadData);
</script>
</body>
</html>
