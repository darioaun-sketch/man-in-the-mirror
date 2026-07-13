<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Man in the Mirror - Ultimate Edition</title>
<link href="https://fonts.googleapis.com/css2?family=Cormorant+Garamond:ital,wght@0,300;0,400;0,500;0,600;1,300;1,400&family=Inter:wght@300;400;500;600&display=swap" rel="stylesheet">
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --bg: #ffffff; --bg2: #f7f7f6; --bg3: #f0f0ee;
    --border: rgba(0,0,0,0.08); --border2: rgba(0,0,0,0.14);
    --txt: #1a1a1a; --txt2: #6b6b6b; --txt3: #a0a0a0;
    --radius: 12px; --radius-sm: 8px;
    --fe: #7B72E9; --cuerpo: #2DB88F; --mente: #3A8FE8;
    --mision: #E05A32; --car: #888884; --sos: #D32F2F;
  }

  body.dark-mode {
    --bg: #0f0f0f; --bg2: #1a1a1a; --bg3: #222222;
    --border: rgba(255,255,255,0.07); --border2: rgba(255,255,255,0.12);
    --txt: #f0f0f0; --txt2: #999999; --txt3: #555555;
  }

  body { font-family: 'Inter', sans-serif; background: var(--bg); color: var(--txt); font-size: 13px; font-weight: 300; line-height: 1.5; padding: 24px 20px 80px; max-width: 680px; margin: 0 auto; transition: background 0.3s, color 0.3s; }

  /* ── Header & Tabs ── */
  .header { display: flex; justify-content: space-between; align-items: flex-start; margin-bottom: 20px; }
  .header-label { font-size: 10px; color: var(--txt3); letter-spacing: 0.18em; text-transform: uppercase; margin-bottom: 5px; }
  .header-title { font-size: 36px; font-weight: 300; letter-spacing: -0.5px; font-family: 'Cormorant Garamond', serif; line-height: 1.1; }
  .header-score { text-align: right; }
  .header-score-num { font-size: 42px; font-weight: 300; letter-spacing: -2px; line-height: 1; font-family: 'Cormorant Garamond', serif; }
  
  .top-btns { display: flex; gap: 8px; margin-top: 8px; }
  .icon-btn { background: var(--bg2); border: 0.5px solid var(--border); border-radius: 50%; width: 32px; height: 32px; display: flex; align-items: center; justify-content: center; cursor: pointer; color: var(--txt); font-size: 14px; }

  .tabs { display: flex; gap: 4px; margin-bottom: 20px; padding-bottom: 16px; border-bottom: 0.5px solid var(--border); overflow-x: auto; scrollbar-width: none; }
  .tabs::-webkit-scrollbar { display: none; }
  .tab { padding: 5px 14px; border-radius: 20px; font-size: 11px; cursor: pointer; border: 0.5px solid var(--border2); background: transparent; color: var(--txt2); font-family: inherit; white-space: nowrap; }
  .tab.active { background: var(--txt); color: var(--bg); border-color: var(--txt); font-weight: 500; }

  .panel { display: none; }
  .panel.visible { display: block; }

  /* ── Cards & UI ── */
  .card { background: var(--bg); border: 0.5px solid var(--border); border-radius: var(--radius); padding: 16px 18px; margin-bottom: 10px; }
  .card-label { font-size: 10px; color: var(--txt3); margin-bottom: 12px; letter-spacing: 0.08em; text-transform: uppercase; }
  .metrics { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; margin-bottom: 12px; }
  .metric { background: var(--bg2); border-radius: var(--radius-sm); padding: 12px 14px; }
  .metric-label { font-size: 10px; color: var(--txt3); text-transform: uppercase; letter-spacing: 0.08em; margin-bottom: 6px; }
  .metric-val { font-size: 20px; font-weight: 300; font-family: 'Cormorant Garamond', serif; }

  /* ── Charts (Radar & Heatmap) ── */
  #radar-svg { display: block; margin: 0 auto; max-width: 210px; }
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

  /* ── Habits & Trackers ── */
  .day-row { background: var(--bg); border: 0.5px solid var(--border); border-radius: var(--radius); margin-bottom: 6px; }
  .day-header { display: flex; justify-content: space-between; align-items: center; padding: 12px 16px; cursor: pointer; }
  .day-header:hover { background: var(--bg2); }
  .day-body { display: none; padding: 0 16px 14px; border-top: 0.5px solid var(--border); }
  .day-body.open { display: block; }
  .section-title { font-size: 10px; color: var(--txt3); text-transform: uppercase; letter-spacing: 0.08em; margin: 16px 0 10px; font-weight: 500; }

  .habit-row-wrap { margin-bottom: 8px; border-bottom: 0.5px solid var(--border); padding-bottom: 8px; }
  .habit-top { display: flex; align-items: center; gap: 8px; margin-bottom: 6px; }
  .habit-chk { width: 16px; height: 16px; border-radius: 4px; border: 1.5px solid var(--border2); cursor: pointer; display: flex; align-items: center; justify-content: center; }
  .habit-name { font-size: 12px; flex: 1; cursor: pointer; user-select: none; }
  .habit-streak { font-size: 10px; color: #E8931A; font-weight: 600; display: flex; align-items: center; gap: 2px; }
  .btn-timer { background: var(--bg2); border: 0.5px solid var(--border); border-radius: 4px; padding: 2px 6px; font-size: 10px; cursor: pointer; color: var(--txt); }
  
  .text-input { width: 100%; border: 0.5px solid var(--border2); border-radius: var(--radius-sm); padding: 8px 10px; font-size: 11px; font-family: inherit; background: var(--bg); color: var(--txt); outline: none; margin-bottom: 4px; }
  .text-input:focus { border-color: var(--fe); }

  .photo-upload { font-size: 11px; margin-bottom: 10px; }
  .photo-preview { max-width: 100%; border-radius: 8px; margin-top: 8px; border: 0.5px solid var(--border); display: none; }
  .btn-add { font-size: 11px; padding: 5px 14px; border-radius: 7px; border: none; background: var(--txt); color: var(--bg); cursor: pointer; font-weight: 500; }

  /* ── SOS Button ── */
  .sos-btn { position: fixed; bottom: 20px; right: 20px; width: 50px; height: 50px; border-radius: 25px; background: var(--sos); color: white; font-weight: 600; border: none; box-shadow: 0 4px 12px rgba(211,47,47,0.4); cursor: pointer; display: flex; align-items: center; justify-content: center; font-size: 14px; z-index: 100; transition: transform 0.2s; }
  .sos-btn:active { transform: scale(0.9); }

  /* ── Modals ── */
  .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0,0,0,0.85); backdrop-filter: blur(5px); display: none; align-items: center; justify-content: center; z-index: 1000; padding: 20px; }
  .modal-overlay.active { display: flex; }
  .modal-box { background: var(--bg); border-radius: var(--radius); padding: 24px; max-width: 400px; width: 100%; text-align: center; border: 1px solid var(--border); }
  
  .timer-display { font-size: 48px; font-family: 'Cormorant Garamond', serif; margin: 10px 0; font-variant-numeric: tabular-nums; }
  .sos-title { color: var(--sos); font-size: 24px; font-weight: 600; text-transform: uppercase; margin-bottom: 15px; }
  .sos-list { text-align: left; font-size: 14px; line-height: 1.8; margin-bottom: 20px; }
  .mirror-title { font-size: 20px; color: var(--mision); margin-bottom: 10px; font-weight: 600; }
  .modal-btn { background: var(--txt); color: var(--bg); border: none; padding: 10px 20px; border-radius: 8px; cursor: pointer; font-family: inherit; font-weight: 500; margin-top: 10px; width: 100%; }
</style>
</head>
<body>

<div class="header">
  <div>
    <div class="header-label">man in the mirror</div>
    <div class="header-title">Estadísticas de Vida</div>
    <div class="top-btns">
      <button class="icon-btn" id="btn-theme" title="Modo Oscuro">🌙</button>
      <button class="icon-btn" id="btn-future" title="Tú del Futuro">👁️</button>
    </div>
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
  <button class="tab" data-tab="boveda">Bóveda del Chef</button>
</div>

<!-- RESUMEN -->
<div class="panel visible" id="panel-resumen">
  <div class="metrics">
    <div class="metric"><div class="metric-label">mejor día</div><div class="metric-val" id="m-best">—</div></div>
    <div class="metric"><div class="metric-label">racha</div><div class="metric-val" id="m-racha">—</div></div>
    <div class="metric"><div class="metric-label">área débil</div><div class="metric-val" id="m-weak">—</div></div>
  </div>
  
  <div class="card" id="insights-card">
    <div class="card-label">🧠 Inteligencia & Correlaciones</div>
    <div id="insights-content" style="font-size:12px; color:var(--txt2);">Analizando datos...</div>
  </div>

  <div class="card" id="weekly-card">
    <div class="card-label">📅 Resumen Últimos 7 Días</div>
    <div id="weekly-content" style="font-size:12px;"></div>
  </div>

  <div class="card">
    <div class="card-label">Puntuación diaria (Hábitos 75% + Reflexión 25%)</div>
    <div id="daily-bars" style="display:flex; gap:4px; height:60px; align-items:flex-end; margin-top:10px;"></div>
  </div>

  <div class="card">
    <div class="card-label">Radar de áreas · promedio general</div>
    <svg id="radar-svg" viewBox="0 0 240 240" width="100%"></svg>
  </div>
</div>

<!-- ÁREAS -->
<div class="panel" id="panel-areas"><div id="areas-grid" style="display:grid; gap:8px;"></div></div>

<!-- HÁBITOS (HOY) -->
<div class="panel" id="panel-habitos">
  <div id="habitos-grid"></div>
</div>

<!-- TRACKER REFLEXIÓN -->
<div class="panel" id="panel-tracker">
  <div style="display:flex;justify-content:space-between;margin-bottom:12px;">
    <div class="header-title" style="font-size:24px;">Historial</div>
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

<!-- BÓVEDA DEL CHEF -->
<div class="panel" id="panel-boveda">
  <div class="card">
    <div class="card-label">Bóveda de Conocimiento</div>
    <input type="text" id="vault-search" class="text-input" placeholder="Buscar aprendizajes, recetas, notas..." style="margin-bottom:12px;">
    <div id="vault-content" style="display:flex; flex-direction:column; gap:8px;"></div>
  </div>
</div>

<button class="sos-btn" id="btn-sos">SOS</button>

<!-- MODALS -->
<div class="modal-overlay" id="modal-timer">
  <div class="modal-box">
    <div class="card-label" id="timer-title">Modo Enfoque</div>
    <div class="timer-display" id="timer-display">25:00</div>
    <button class="modal-btn" id="btn-timer-action">Iniciar</button>
    <button class="modal-btn" style="background:transparent; color:var(--txt); border:1px solid var(--border); margin-top:8px;" id="btn-timer-close">Cancelar</button>
  </div>
</div>

<div class="modal-overlay" id="modal-sos">
  <div class="modal-box" style="border: 2px solid var(--sos);">
    <div class="sos-title">Protocolo de Reseteo</div>
    <div class="sos-list">
      1. Suelta el teléfono y ponte de pie.<br>
      2. Bebe 1 vaso grande de agua ahora.<br>
      3. Lávate la cara con agua helada.<br>
      4. Haz 20 flexiones estrictas.<br>
      5. "No te detengas cuando estés cansado, detente cuando hayas terminado."
    </div>
    <button class="modal-btn" style="background:var(--sos);" onclick="document.getElementById('modal-sos').classList.remove('active')">Entendido. A trabajar.</button>
  </div>
</div>

<div class="modal-overlay" id="modal-mirror">
  <div class="modal-box">
    <div class="mirror-title">Confrontación del Espejo</div>
    <p style="font-size:13px; color:var(--txt2); margin-bottom:16px;" id="mirror-text"></p>
    <input type="text" id="mirror-input" class="text-input" placeholder="Escribe 'NO ME RENDIRÉ' para continuar" style="text-align:center; font-weight:bold;">
    <button class="modal-btn" id="btn-mirror-unlock" style="background:var(--txt3); cursor:not-allowed;" disabled>Desbloquear</button>
  </div>
</div>

<div class="modal-overlay" id="modal-future">
  <div class="modal-box">
    <div class="card-label">Proyección a 1 Año 👁️</div>
    <div id="future-content" style="font-size:14px; line-height:1.6; margin-bottom:16px;"></div>
    <button class="modal-btn" onclick="document.getElementById('modal-future').classList.remove('active')">Volver al presente</button>
  </div>
</div>

<script>
const STORAGE_KEY = 'man_in_the_mirror_ultimate';
const AREAS = [
  { k:"fe",     label:"Fe & espíritu",         color:"#7B72E9" },
  { k:"mente",  label:"Mente & aprendizaje",   color:"#3A8FE8" },
  { k:"mision", label:"Misión & negocio",      color:"#E05A32" },
  { k:"cuerpo", label:"Cuerpo & salud",        color:"#2DB88F" },
  { k:"car",    label:"Carácter & disciplina", color:"#888884" },
];
const HABITOS = {
  fe: ["Leer la Biblia 30 min", "Reflexión nocturna", "Ver Smallville"],
  mente: ["Leer You Can't Hurt Me 30 min", "Estudiar Heroes Academia 1 hora", "Medir cortisol (1–10)"],
  mision: ["aplicación chef", "aprendizaje chef", "el diario del chef", "Ayudar en casa (cocinar o arreglar)"],
  cuerpo: ["Entrenar", "3 litros de agua", "140g proteína", "240g carbohidratos", "70g grasas", "Ducha fría", "Dormir 8–9 horas", "Medir energía (1–10)"],
  car: ["Cumplir horario del día", "Mantener espacio limpio", "Evitar distracciones innecesarias"]
};
const QS = [
  { q:"¿Conecté con Dios hoy?", area:"fe" }, { q:"¿Aprendí algo nuevo hoy?", area:"mente" },
  { q:"¿Fui constante como chef?", area:"mision" }, { q:"¿Entrené con intensidad?", area:"cuerpo" },
  { q:"¿Cumplí mis horarios?", area:"car" }
];
const VAL = { "Sí":2, "Parcial":1, "No":0 };
let DATA = [];

// TIMERS PREDEFINIDOS (minutos)
const TIMERS = {
  "Leer la Biblia 30 min": 30,
  "Leer You Can't Hurt Me 30 min": 30,
  "Estudiar Heroes Academia 1 hora": 60,
  "Entrenar": 45
};

// INITIALIZATION
function init() {
  const saved = localStorage.getItem(STORAGE_KEY);
  if (saved) DATA = JSON.parse(saved);
  if(localStorage.getItem('theme') === 'dark') document.body.classList.add('dark-mode');
  checkMirror();
  render();
}

function saveData() { localStorage.setItem(STORAGE_KEY, JSON.stringify(DATA)); }
function defaultHabits() {
  let h = {};
  Object.values(HABITOS).flat().forEach(hab => h[hab] = { done: false, note: '' });
  return h;
}

// SCORING
function getHabScore(r, ak) {
  let habs = ak ? HABITOS[ak] : Object.values(HABITOS).flat();
  let done = habs.filter(h => r.habits && r.habits[h]?.done).length;
  return habs.length ? (done/habs.length)*100 : null;
}
function getRefScore(r, ak) {
  let qsList = ak ? QS.filter(q=>q.area===ak).map(q=>q.q) : QS.map(q=>q.q);
  let vals = qsList.map(q => VAL[r.qs?.[q]]).filter(v => v!=null);
  return vals.length ? (vals.reduce((a,b)=>a+b,0)/(vals.length*2))*100 : null;
}
function dayScore(r) { 
  let h=getHabScore(r), rf=getRefScore(r); 
  if(h==null && rf==null) return null;
  return Math.round(((h||0)*0.75) + ((rf||0)*0.25)); 
}
function areaScore(r, ak) {
  let h = getHabScore(r, ak), rf = getRefScore(r, ak);
  if(h == null && rf == null) return null;
  return Math.round(((h || 0) * 0.75) + ((rf || 0) * 0.25));
}
function areaAvg(ak){ 
  const vs = DATA.map(r=>areaScore(r,ak)).filter(v=>v!=null); 
  return vs.length ? Math.round(vs.reduce((a,b)=>a+b,0)/vs.length) : null; 
}
function getStreak(habit) {
  let s = 0;
  for(let i=DATA.length-1; i>=0; i--) { if(DATA[i].habits?.[habit]?.done) s++; else break; }
  return s;
}

// TABS
document.querySelectorAll('.tab').forEach(btn => {
  btn.addEventListener('click', () => {
    document.querySelectorAll('.tab').forEach(b=>b.classList.remove('active'));
    document.querySelectorAll('.panel').forEach(p=>p.classList.remove('visible'));
    btn.classList.add('active');
    document.getElementById('panel-'+btn.dataset.tab).classList.add('visible');
    render();
  });
});

// RENDERING
function render() {
  const g = DATA.map(dayScore).filter(v=>v!=null);
  document.getElementById('global-score').textContent = g.length ? Math.round(g.reduce((a,b)=>a+b,0)/g.length)+'%' : '—';
  
  if(document.getElementById('panel-resumen').classList.contains('visible')) {
    const scores = DATA.map(dayScore);
    document.getElementById('m-best').textContent = DATA.length ? Math.max(...scores.filter(v=>v!=null))+'%' : '—';
    document.getElementById('m-racha').textContent = DATA.length+' d';
    
    generateInsights();
    generateWeekly();
    
    document.getElementById('daily-bars').innerHTML = scores.map((s,i) => `
      <div style="flex:1; display:flex; flex-direction:column; align-items:center; justify-content:flex-end; gap:2px;">
        <div style="font-size:9px; color:var(--txt3)">${s!=null?s+'%':''}</div>
        <div style="width:100%; background:var(--fe); height:${s==null?2:Math.max(2, (s/100)*40)}px; opacity:${s==null?.15:.82}; border-radius:2px 2px 0 0;"></div>
        <div style="font-size:9px; color:var(--txt3)">${DATA[i].d}</div>
      </div>`).join('');
      
    renderRadar();
  }
  
  if(document.getElementById('panel-areas').classList.contains('visible')) {
    document.getElementById('areas-grid').innerHTML = AREAS.map(a => {
      let avg = areaAvg(a.k) || 0;
      let trophy = (a.k==='mision' && avg>=80) ? '🏆' : '';
      return `<div class="card" style="display:flex; justify-content:space-between; align-items:center;">
        <div><div style="font-weight:600; color:${a.color}">${a.label} ${trophy}</div><div style="font-size:11px; color:var(--txt3)">Promedio histórico</div></div>
        <div style="font-size:24px; font-family:'Cormorant Garamond', serif;">${avg}%</div>
      </div>`;
    }).join('');
  }

  if(document.getElementById('panel-habitos').classList.contains('visible')) {
    if(!DATA.length) return document.getElementById('habitos-grid').innerHTML = 'Crea un día primero.';
    document.getElementById('habitos-grid').innerHTML = renderHabitList(DATA.length-1);
  }

  if(document.getElementById('panel-tracker').classList.contains('visible')) {
    document.getElementById('tracker-rows').innerHTML = DATA.map((r, di) => `
      <div class="day-row">
        <div class="day-header" onclick="toggleBody(${di})">
          <div style="font-weight:600;">${r.d}</div>
          <div style="font-weight:500;">${dayScore(r)!=null?dayScore(r)+'%':'—'}</div>
        </div>
        <div class="day-body" id="body-${di}">
          <input type="text" class="text-input note-main" data-di="${di}" placeholder="Mentalidad / Objetivo del día..." value="${r.note||''}">
          
          <div class="section-title">Foto del Día (Opcional)</div>
          <input type="file" class="photo-upload" accept="image/*" onchange="handleImage(this, ${di})">
          <img class="photo-preview" id="img-${di}" src="${r.img||''}" style="display:${r.img?'block':'none'}">

          <div class="section-title">Hábitos</div>
          ${renderHabitList(di, true)}
        </div>
      </div>
    `).reverse().join('');
  }
  
  if(document.getElementById('panel-heatmap').classList.contains('visible')) {
    renderHeatmap();
  }

  if(document.getElementById('panel-boveda').classList.contains('visible')) {
    renderVault();
  }
}

// RADAR CHART
function renderRadar() {
  const svg = document.getElementById('radar-svg');
  const cx=120, cy=120, R=82, avgs=AREAS.map(a=>areaAvg(a.k)), isDark=window.matchMedia('(prefers-color-scheme: dark)').matches;
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

// HEATMAP
function renderHeatmap() {
  const inner = document.getElementById('heat-inner');
  if(!DATA.length) { inner.innerHTML='<div style="color:var(--txt3);font-size:12px">Sin datos</div>'; return; }
  inner.innerHTML = `<div class="heat-days-header">${DATA.map(r=>`<div class="heat-day-label">${r.d}</div>`).join('')}</div>` +
    AREAS.map(a=>`<div class="heat-row"><div class="heat-area-label">${a.label}</div><div class="heat-cells">
      ${DATA.map(r=>{ const s=areaScore(r,a.k); const c=s==null?'var(--bg3)':s>=90?a.color:s>=65?a.color+'bf':s>=35?a.color+'66':a.color+'26'; return `<div class="heat-cell" style="background:${c}"></div>`;}).join('')}
    </div></div>`).join('') +
    `<div class="heat-legend">${[['bajo','rgba(123,114,233,.15)'],['medio','rgba(123,114,233,.4)'],['alto','rgba(123,114,233,.75)'],['óptimo','#7B72E9']].map(([l,c])=>`<div><div class="heat-legend-swatch" style="background:${c}"></div>${l}</div>`).join('')}</div>`;
}

// HABITS RENDERER
function renderHabitList(di, isTracker = false) {
  const row = DATA[di];
  if(!row.habits) row.habits = defaultHabits();
  return AREAS.map(a => {
    let html = `<div style="font-weight:600; font-size:11px; color:${a.color}; margin:10px 0 5px;">${a.label.toUpperCase()}</div>`;
    HABITOS[a.k].forEach(h => {
      let hd = row.habits[h] || {done:false, note:''};
      let streak = getStreak(h);
      let fire = streak > 2 ? `<span class="habit-streak">🔥${streak}</span>` : '';
      let timerBtn = TIMERS[h] && !hd.done ? `<button class="btn-timer" onclick="openTimer('${h}', ${di}, ${TIMERS[h]})">⏱️</button>` : '';
      let bg = hd.done ? a.color : 'transparent';
      let chk = hd.done ? `<svg width="10" height="10"><polyline points="1.5,5 4,7.5 8.5,2.5" stroke="white" stroke-width="1.5" fill="none"/></svg>` : '';
      
      html += `
      <div class="habit-row-wrap">
        <div class="habit-top">
          <div class="habit-chk" style="background:${bg}; border-color:${hd.done?a.color:'var(--border2)'}" onclick="toggleHabit(${di}, '${h}')">${chk}</div>
          <div class="habit-name" style="text-decoration:${hd.done?'line-through':'none'}; color:${hd.done?'var(--txt3)':'var(--txt)'}" onclick="toggleHabit(${di}, '${h}')">${h} ${fire}</div>
          ${timerBtn}
        </div>
        <input type="text" class="text-input habit-note" data-di="${di}" data-h="${h}" value="${hd.note||''}" placeholder="Notas / Detalles...">
      </div>`;
    });
    return html;
  }).join('');
}

// INTERACTIONS & DATA
function toggleBody(di) { document.getElementById('body-'+di).classList.toggle('open'); }
function toggleHabit(di, h) { DATA[di].habits[h].done = !DATA[di].habits[h].done; saveData(); render(); }

document.getElementById('btn-add-day').addEventListener('click', () => {
  let d = prompt("Nombre del día (Ej: Lun 16):");
  if(d) { DATA.push({ d, note:'', qs:{}, habits:defaultHabits() }); saveData(); render(); }
});

document.addEventListener('input', e => {
  if(e.target.classList.contains('note-main')) { DATA[e.target.dataset.di].note = e.target.value; saveData(); }
  if(e.target.classList.contains('habit-note')) { DATA[e.target.dataset.di].habits[e.target.dataset.h].note = e.target.value; saveData(); renderVault(); }
  if(e.target.id === 'vault-search') renderVault(e.target.value);
});

function handleImage(input, di) {
  if(!input.files || !input.files[0]) return;
  const reader = new FileReader();
  reader.onload = e => {
    const img = new Image();
    img.onload = () => {
      const cvs = document.createElement('canvas');
      const MAX_W = 400; const scale = MAX_W / img.width;
      cvs.width = MAX_W; cvs.height = img.height * scale;
      cvs.getContext('2d').drawImage(img, 0, 0, cvs.width, cvs.height);
      const b64 = cvs.toDataURL('image/jpeg', 0.6);
      DATA[di].img = b64; saveData();
      document.getElementById('img-'+di).src = b64;
      document.getElementById('img-'+di).style.display = 'block';
    }
    img.src = e.target.result;
  }
  reader.readAsDataURL(input.files[0]);
}

// INSIGHTS & WEEKLY
function generateInsights() {
  const el = document.getElementById('insights-content');
  if(DATA.length < 3) return el.innerHTML = "Faltan datos para crear correlaciones (mínimo 3 días).";
  
  let sleepDays = DATA.filter(r => r.habits?.['Dormir 8–9 horas']?.done);
  let sleepScoreAvg = sleepDays.length ? Math.round(sleepDays.reduce((a,b)=>a+dayScore(b),0)/sleepDays.length) : 0;
  
  let noSleepDays = DATA.filter(r => !r.habits?.['Dormir 8–9 horas']?.done);
  let noSleepScoreAvg = noSleepDays.length ? Math.round(noSleepDays.reduce((a,b)=>a+dayScore(b),0)/noSleepDays.length) : 0;
  
  let txt = `Cuando duermes 8 horas, tu puntuación promedio es <b>${sleepScoreAvg}%</b>. `;
  if(noSleepDays.length) txt += `Cuando no lo haces, cae a <b>${noSleepScoreAvg}%</b>. `;
  
  el.innerHTML = txt;
}

function generateWeekly() {
  const el = document.getElementById('weekly-content');
  const last7 = DATA.slice(-7);
  if(last7.length === 0) return el.innerHTML = "Sin datos.";
  let validDays = last7.filter(r => dayScore(r) != null);
  let avg = validDays.length ? Math.round(validDays.reduce((a,b)=>a+dayScore(b),0)/validDays.length) : 0;
  el.innerHTML = `Promedio últimos días: <b>${avg}%</b>. <br>Mantén el enfoque en tu misión.`;
}

// VAULT
function renderVault(filter = "") {
  const el = document.getElementById('vault-content');
  let html = ""; let f = filter.toLowerCase();
  DATA.slice().reverse().forEach(r => {
    if(!r.habits) return;
    HABITOS['mision'].forEach(h => {
      let note = r.habits[h]?.note;
      if(note && note.toLowerCase().includes(f)) {
        html += `<div style="background:var(--bg2); padding:10px; border-radius:6px; border:1px solid var(--border);">
          <div style="font-size:10px; color:var(--mision); font-weight:600; margin-bottom:4px;">${r.d} · ${h}</div>
          <div style="font-size:12px;">${note}</div>
        </div>`;
      }
    });
  });
  el.innerHTML = html || "<div style='color:var(--txt3); font-size:12px;'>No hay notas que coincidan.</div>";
}

// SOS & TIMERS & MODALS
document.getElementById('btn-theme').addEventListener('click', () => {
  document.body.classList.toggle('dark-mode');
  localStorage.setItem('theme', document.body.classList.contains('dark-mode') ? 'dark' : 'light');
});

document.getElementById('btn-sos').addEventListener('click', () => {
  document.getElementById('modal-sos').classList.add('active');
});

let timerInterval;
function openTimer(habit, di, mins) {
  const modal = document.getElementById('modal-timer');
  const disp = document.getElementById('timer-display');
  const btn = document.getElementById('btn-timer-action');
  
  document.getElementById('timer-title').textContent = `Enfoque: ${habit}`;
  disp.textContent = `${mins}:00`;
  modal.classList.add('active');
  
  let time = mins * 60; let running = false;
  
  btn.onclick = () => {
    if(running) return;
    running = true; btn.textContent = "Trabajando...";
    timerInterval = setInterval(() => {
      time--;
      let m = Math.floor(time/60).toString().padStart(2,'0');
      let s = (time%60).toString().padStart(2,'0');
      disp.textContent = `${m}:${s}`;
      if(time <= 0) {
        clearInterval(timerInterval);
        DATA[di].habits[habit].done = true;
        saveData(); render();
        modal.classList.remove('active');
        alert("¡Sesión completada! Hábito marcado.");
      }
    }, 1000);
  };
  
  document.getElementById('btn-timer-close').onclick = () => {
    clearInterval(timerInterval);
    modal.classList.remove('active');
  };
}

function checkMirror() {
  if(DATA.length < 3) return;
  const last3 = DATA.slice(-3);
  let valid = last3.filter(r => dayScore(r) != null);
  if(valid.length === 0) return;
  
  let avg = Math.round(valid.reduce((a,b)=>a+dayScore(b),0)/valid.length);
  
  if(avg < 60 && !sessionStorage.getItem('mirrorShown')) {
    const modal = document.getElementById('modal-mirror');
    const input = document.getElementById('mirror-input');
    const btn = document.getElementById('btn-mirror-unlock');
    
    document.getElementById('mirror-text').textContent = `Tu promedio de los últimos 3 días es ${avg}%. ¿Qué excusa estás usando para no dar el 100%? Reconócelo y avanza.`;
    modal.classList.add('active');
    
    input.addEventListener('input', e => {
      if(e.target.value === 'NO ME RENDIRÉ') {
        btn.disabled = false; btn.style.background = 'var(--txt)';
      } else {
        btn.disabled = true; btn.style.background = 'var(--txt3)';
      }
    });
    
    btn.onclick = () => {
      sessionStorage.setItem('mirrorShown', 'true');
      modal.classList.remove('active');
    };
  }
}

document.getElementById('btn-future').addEventListener('click', () => {
  const modal = document.getElementById('modal-future');
  const content = document.getElementById('future-content');
  
  if(!DATA.length) { content.textContent = "Necesitas registrar días para predecir el futuro."; modal.classList.add('active'); return; }
  
  let totalDays = DATA.length;
  let pChef = DATA.filter(r => r.habits?.['aprendizaje chef']?.done).length;
  let pGym = DATA.filter(r => r.habits?.['Entrenar']?.done).length;
  
  let rateChef = pChef / totalDays;
  let rateGym = pGym / totalDays;
  
  let projChef = Math.round(rateChef * 365);
  let projGym = Math.round(rateGym * 365);
  
  let msg = `Basado en tu disciplina actual, de aquí a un año habrás acumulado:<br><br>`;
  msg += `<b>👨‍🍳 ${projChef} días de aprendizaje chef</b><br>`;
  msg += `<b>💪 ${projGym} días de entrenamiento</b><br><br>`;
  
  if(rateChef > 0.8) msg += `<span style="color:var(--cuerpo)">Estás en camino a ser imparable en la cocina.</span>`;
  else msg += `<span style="color:var(--mision)">Si quieres ser un gran chef, este número debe subir. No negocies tus metas.</span>`;
  
  content.innerHTML = msg;
  modal.classList.add('active');
});

init();
</script>
</body>
</html>
