<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>PARSIQ — AST Analyzer & Linguistic Humanizer</title>
<link href="https://fonts.googleapis.com/css2?family=Syne:wght@400;500;600;700;800&family=JetBrains+Mono:ital,wght@0,300;0,400;0,500;0,700;1,300&family=Instrument+Serif:ital@0;1&display=swap" rel="stylesheet">
<style>
/* ════════════════════════════════════════════
   PARSIQ — Design System
   Theme: Precision Light · Developer Premium
════════════════════════════════════════════ */
:root {
  --ink:        #0a0a0f;
  --ink-2:      #1a1a2e;
  --ink-3:      #2d2d4a;
  --mid:        #6b7280;
  --mid-2:      #9ca3af;
  --mist:       #e5e7eb;
  --mist-2:     #f3f4f6;
  --mist-3:     #f9fafb;
  --white:      #ffffff;

  --electric:   #0066ff;
  --electric-2: #3385ff;
  --electric-bg:#eff6ff;
  --electric-br:#bfdbfe;

  --jade:       #059669;
  --jade-bg:    #ecfdf5;
  --jade-br:    #a7f3d0;

  --amber:      #d97706;
  --amber-bg:   #fffbeb;
  --amber-br:   #fde68a;

  --coral:      #dc2626;
  --coral-bg:   #fef2f2;
  --coral-br:   #fecaca;

  --violet:     #7c3aed;
  --violet-bg:  #f5f3ff;
  --violet-br:  #ddd6fe;

  --f-display:  'Syne', sans-serif;
  --f-serif:    'Instrument Serif', serif;
  --f-mono:     'JetBrains Mono', monospace;

  --r-sm:  4px;
  --r-md:  8px;
  --r-lg:  12px;
  --r-xl:  16px;

  --shadow-xs: 0 1px 2px rgba(0,0,0,0.05);
  --shadow-sm: 0 1px 3px rgba(0,0,0,0.07), 0 1px 2px rgba(0,0,0,0.05);
  --shadow-md: 0 4px 6px rgba(0,0,0,0.05), 0 2px 4px rgba(0,0,0,0.04);
  --shadow-lg: 0 10px 15px rgba(0,0,0,0.06), 0 4px 6px rgba(0,0,0,0.04);
  --shadow-xl: 0 20px 25px rgba(0,0,0,0.07), 0 10px 10px rgba(0,0,0,0.04);
}

*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

html { scroll-behavior: smooth; font-size: 15px; }

body {
  font-family: var(--f-display);
  background: var(--mist-3);
  color: var(--ink);
  min-height: 100vh;
  -webkit-font-smoothing: antialiased;
}

/* ─── TOPNAV ─── */
.nav {
  position: sticky;
  top: 0;
  z-index: 200;
  background: rgba(255,255,255,0.85);
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
  border-bottom: 1px solid var(--mist);
  height: 58px;
  display: flex;
  align-items: center;
  padding: 0 28px;
  gap: 0;
}

.nav-brand {
  display: flex;
  align-items: center;
  gap: 10px;
  text-decoration: none;
  margin-right: auto;
}

.nav-logo-mark {
  width: 32px; height: 32px;
  background: var(--ink);
  border-radius: 8px;
  display: flex; align-items: center; justify-content: center;
  flex-shrink: 0;
  position: relative;
  overflow: hidden;
}

.nav-logo-mark::before {
  content: '';
  position: absolute;
  inset: 0;
  background: linear-gradient(135deg, var(--electric) 0%, transparent 60%);
  opacity: 0.5;
}

.nav-logo-mark svg { position: relative; z-index: 1; }

.nav-name {
  font-family: var(--f-display);
  font-size: 18px;
  font-weight: 800;
  letter-spacing: -0.5px;
  color: var(--ink);
}

.nav-name sup {
  font-size: 9px;
  font-weight: 500;
  letter-spacing: 1px;
  color: var(--electric);
  vertical-align: super;
  margin-left: 1px;
}

.nav-tagline {
  font-family: var(--f-mono);
  font-size: 10px;
  color: var(--mid-2);
  letter-spacing: 0.5px;
  padding-left: 12px;
  margin-left: 12px;
  border-left: 1px solid var(--mist);
}

.nav-pills {
  display: flex;
  align-items: center;
  gap: 4px;
}

.nav-pill {
  font-family: var(--f-mono);
  font-size: 10px;
  letter-spacing: 0.5px;
  color: var(--mid);
  padding: 4px 10px;
  border-radius: 20px;
  cursor: pointer;
  transition: all 0.15s;
  border: 1px solid transparent;
  display: flex;
  align-items: center;
  gap: 6px;
}

.nav-pill:hover { background: var(--mist-2); color: var(--ink); }
.nav-pill.active { background: var(--ink); color: white; border-color: var(--ink); }

.nav-status {
  display: flex;
  align-items: center;
  gap: 6px;
  font-family: var(--f-mono);
  font-size: 10px;
  color: var(--jade);
  padding: 0 0 0 20px;
  margin-left: 16px;
  border-left: 1px solid var(--mist);
}

.pulse {
  width: 7px; height: 7px;
  border-radius: 50%;
  background: var(--jade);
  position: relative;
}
.pulse::after {
  content: '';
  position: absolute;
  inset: -3px;
  border-radius: 50%;
  background: var(--jade);
  opacity: 0.3;
  animation: pulse-ring 2s ease-out infinite;
}
@keyframes pulse-ring {
  0% { transform: scale(1); opacity: 0.3; }
  70% { transform: scale(1.8); opacity: 0; }
  100% { opacity: 0; }
}

/* ─── PIPELINE BREADCRUMB ─── */
.pipeline-track {
  background: white;
  border-bottom: 1px solid var(--mist);
  padding: 0 28px;
  display: flex;
  align-items: center;
  gap: 0;
  overflow-x: auto;
  height: 44px;
}

.pipe-step {
  display: flex;
  align-items: center;
  gap: 8px;
  padding: 0 14px;
  height: 100%;
  font-family: var(--f-mono);
  font-size: 10px;
  letter-spacing: 0.5px;
  color: var(--mid-2);
  border-bottom: 2px solid transparent;
  white-space: nowrap;
  transition: all 0.25s;
  cursor: default;
}

.pipe-step.active { color: var(--electric); border-bottom-color: var(--electric); }
.pipe-step.done { color: var(--jade); }
.pipe-step.idle { opacity: 0.45; }

.pipe-num {
  width: 16px; height: 16px;
  border-radius: 50%;
  border: 1.5px solid currentColor;
  display: flex; align-items: center; justify-content: center;
  font-size: 9px;
  flex-shrink: 0;
}
.pipe-step.done .pipe-num {
  background: var(--jade);
  border-color: var(--jade);
  color: white;
}
.pipe-step.done .pipe-num::before { content: '✓'; }
.pipe-step.done .pipe-num span { display: none; }

.pipe-arrow { color: var(--mist); padding: 0 4px; font-size: 14px; }

/* ─── LAYOUT ─── */
.layout {
  display: grid;
  grid-template-columns: 300px 1fr;
  min-height: calc(100vh - 102px);
}

/* ─── LEFT RAIL ─── */
.rail {
  background: white;
  border-right: 1px solid var(--mist);
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

.rail-section {
  border-bottom: 1px solid var(--mist);
  padding: 18px 20px;
}

.rail-head {
  display: flex;
  align-items: center;
  justify-content: space-between;
  margin-bottom: 14px;
}

.rail-title {
  font-family: var(--f-mono);
  font-size: 9.5px;
  font-weight: 500;
  letter-spacing: 1.5px;
  text-transform: uppercase;
  color: var(--mid);
}

.rail-badge {
  font-family: var(--f-mono);
  font-size: 9px;
  padding: 2px 7px;
  border-radius: 20px;
  border: 1px solid;
  letter-spacing: 0.5px;
}

.rb-blue  { color: var(--electric); border-color: var(--electric-br); background: var(--electric-bg); }
.rb-green { color: var(--jade);     border-color: var(--jade-br);     background: var(--jade-bg); }
.rb-amber { color: var(--amber);    border-color: var(--amber-br);    background: var(--amber-bg); }
.rb-coral { color: var(--coral);    border-color: var(--coral-br);    background: var(--coral-bg); }
.rb-violet{ color: var(--violet);   border-color: var(--violet-br);   background: var(--violet-bg); }

/* Stat grid */
.stat-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; }

.stat-tile {
  background: var(--mist-3);
  border: 1px solid var(--mist);
  border-radius: var(--r-md);
  padding: 12px;
  transition: border-color 0.2s;
}
.stat-tile:hover { border-color: var(--electric-br); }

.stat-num {
  font-family: var(--f-mono);
  font-size: 22px;
  font-weight: 700;
  color: var(--ink);
  line-height: 1;
  margin-bottom: 4px;
}
.stat-num.blue   { color: var(--electric); }
.stat-num.green  { color: var(--jade); }
.stat-num.amber  { color: var(--amber); }
.stat-num.coral  { color: var(--coral); }

.stat-lbl {
  font-family: var(--f-mono);
  font-size: 9px;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: var(--mid-2);
}

/* AST tree */
.ast-tree {
  font-family: var(--f-mono);
  font-size: 11px;
  line-height: 2;
  color: var(--mid);
  max-height: 200px;
  overflow-y: auto;
}

.ast-tree .t-fn    { color: var(--electric); }
.ast-tree .t-loop  { color: var(--violet); }
.ast-tree .t-danger{ color: var(--coral); font-weight: 500; }
.ast-tree .t-warn  { color: var(--amber); }
.ast-tree .t-ok    { color: var(--jade); }
.ast-tree .t-node  { color: var(--mid); }
.ast-node-row { display: flex; align-items: center; gap: 0; padding-left: 0; }
.ast-indent { padding-left: 18px; border-left: 1px dashed var(--mist); margin-left: 7px; }

/* Queue items */
.q-item {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 8px 10px;
  border-radius: var(--r-sm);
  border: 1px solid var(--mist);
  margin-bottom: 6px;
  background: var(--mist-3);
  font-family: var(--f-mono);
  font-size: 10px;
  color: var(--mid);
  transition: all 0.3s;
}
.q-item.active { border-color: var(--electric-br); background: var(--electric-bg); color: var(--electric); }
.q-item.done   { border-color: var(--jade-br);     background: var(--jade-bg);     color: var(--jade); opacity: 0.7; }
.q-item.error  { border-color: var(--coral-br);    background: var(--coral-bg);    color: var(--coral); }

.q-dot { width: 6px; height: 6px; border-radius: 50%; background: currentColor; flex-shrink: 0; }
.q-item.active .q-dot { animation: blink 1s infinite; }
@keyframes blink { 0%,100%{opacity:1} 50%{opacity:0.3} }

.q-id   { font-weight: 700; flex-shrink: 0; }
.q-name { flex: 1; overflow: hidden; text-overflow: ellipsis; white-space: nowrap; }
.q-ts   { flex-shrink: 0; opacity: 0.6; font-size: 9px; }

/* DB stat rows */
.db-row {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding: 6px 0;
  border-bottom: 1px solid var(--mist);
  font-family: var(--f-mono);
  font-size: 10.5px;
}
.db-row:last-child { border-bottom: none; }
.db-key { color: var(--mid); }
.db-val { color: var(--ink); font-weight: 500; }
.db-val.blue  { color: var(--electric); }
.db-val.green { color: var(--jade); }
.db-val.amber { color: var(--amber); }

/* ─── MAIN CONTENT ─── */
.main {
  display: flex;
  flex-direction: column;
  overflow: hidden;
}

/* Tab strip */
.tab-strip {
  background: white;
  border-bottom: 1px solid var(--mist);
  display: flex;
  align-items: center;
  padding: 0 24px;
  gap: 0;
  flex-shrink: 0;
}

.tab {
  padding: 0 18px;
  height: 50px;
  display: flex;
  align-items: center;
  gap: 7px;
  font-family: var(--f-mono);
  font-size: 11px;
  letter-spacing: 0.3px;
  color: var(--mid);
  cursor: pointer;
  border-bottom: 2px solid transparent;
  transition: all 0.15s;
  white-space: nowrap;
}
.tab:hover { color: var(--ink); }
.tab.active { color: var(--ink); border-bottom-color: var(--ink); font-weight: 500; }

.tab-dot {
  width: 5px; height: 5px;
  border-radius: 50%;
}
.td-blue  { background: var(--electric); }
.td-green { background: var(--jade); }
.td-violet{ background: var(--violet); }

.tab-count {
  font-size: 9px;
  background: var(--mist);
  color: var(--mid);
  padding: 1px 6px;
  border-radius: 10px;
}
.tab.active .tab-count { background: var(--ink); color: white; }

/* Panel visibility */
.panel { display: none; flex: 1; overflow: auto; }
.panel.active { display: flex; flex-direction: column; }

/* ─── DUAL PIPELINE VIEW ─── */
.dual-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  flex: 1;
  gap: 0;
}

.pipe-pane {
  display: flex;
  flex-direction: column;
  border-right: 1px solid var(--mist);
  overflow: hidden;
}
.pipe-pane:last-child { border-right: none; }

.pane-bar {
  padding: 14px 20px 12px;
  border-bottom: 1px solid var(--mist);
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: var(--mist-3);
  flex-shrink: 0;
}

.pane-title {
  font-family: var(--f-display);
  font-size: 13px;
  font-weight: 700;
  letter-spacing: -0.2px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.track-chip {
  font-family: var(--f-mono);
  font-size: 9px;
  font-weight: 400;
  padding: 2px 7px;
  border-radius: 20px;
  border: 1px solid;
  letter-spacing: 0.5px;
}

.pane-body {
  flex: 1;
  padding: 18px 20px;
  overflow-y: auto;
  display: flex;
  flex-direction: column;
  gap: 14px;
  background: white;
}

/* Form elements */
.field-label {
  font-family: var(--f-mono);
  font-size: 9.5px;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: var(--mid);
  margin-bottom: 6px;
  display: flex;
  align-items: center;
  gap: 8px;
}

.field-label::after {
  content: '';
  flex: 1;
  height: 1px;
  background: var(--mist);
}

textarea.code-area {
  width: 100%;
  min-height: 170px;
  background: var(--ink);
  color: #a5f3fc;
  font-family: var(--f-mono);
  font-size: 12px;
  line-height: 1.75;
  border: 1px solid #1e293b;
  border-radius: var(--r-md);
  padding: 14px 16px;
  resize: vertical;
  outline: none;
  transition: border-color 0.2s;
  caret-color: var(--electric);
}
textarea.code-area::placeholder { color: #475569; }
textarea.code-area:focus { border-color: var(--electric); }

textarea.text-area {
  width: 100%;
  min-height: 170px;
  background: var(--mist-3);
  color: var(--ink);
  font-family: var(--f-display);
  font-size: 13.5px;
  line-height: 1.8;
  border: 1px solid var(--mist);
  border-radius: var(--r-md);
  padding: 14px 16px;
  resize: vertical;
  outline: none;
  transition: border-color 0.2s;
}
textarea.text-area::placeholder { color: var(--mid-2); }
textarea.text-area:focus { border-color: var(--ink); }

/* Buttons */
.btn-row { display: flex; align-items: center; gap: 8px; flex-wrap: wrap; }

.btn {
  font-family: var(--f-mono);
  font-size: 11px;
  letter-spacing: 0.3px;
  padding: 8px 16px;
  border-radius: var(--r-sm);
  border: 1px solid;
  cursor: pointer;
  transition: all 0.15s;
  display: inline-flex;
  align-items: center;
  gap: 7px;
  white-space: nowrap;
  font-weight: 500;
}

.btn-dark {
  background: var(--ink);
  border-color: var(--ink);
  color: white;
}
.btn-dark:hover { background: var(--ink-2); transform: translateY(-1px); box-shadow: var(--shadow-md); }
.btn-dark:active { transform: none; }

.btn-blue {
  background: var(--electric);
  border-color: var(--electric);
  color: white;
}
.btn-blue:hover { background: var(--electric-2); transform: translateY(-1px); box-shadow: 0 4px 12px rgba(0,102,255,0.3); }

.btn-jade {
  background: var(--jade);
  border-color: var(--jade);
  color: white;
}
.btn-jade:hover { background: #047857; transform: translateY(-1px); box-shadow: 0 4px 12px rgba(5,150,105,0.3); }

.btn-ghost {
  background: transparent;
  border-color: var(--mist);
  color: var(--mid);
}
.btn-ghost:hover { border-color: var(--mid-2); color: var(--ink); }

.btn-sm { padding: 5px 11px; font-size: 10px; }

/* Spinner */
.spin { display:none; width:13px; height:13px; border:2px solid rgba(255,255,255,0.3); border-top-color:white; border-radius:50%; animation: spin360 0.7s linear infinite; }
.spin.show { display:inline-block; }
@keyframes spin360 { to { transform:rotate(360deg); } }

/* Result cards */
.result-card {
  background: var(--mist-3);
  border: 1px solid var(--mist);
  border-radius: var(--r-md);
  overflow: hidden;
  transition: border-color 0.2s;
}
.result-card.ok     { border-color: var(--jade-br);     background: var(--jade-bg); }
.result-card.warn   { border-color: var(--amber-br);    background: var(--amber-bg); }
.result-card.danger { border-color: var(--coral-br);    background: var(--coral-bg); }
.result-card.info   { border-color: var(--electric-br); background: var(--electric-bg); }

.rc-head {
  padding: 8px 12px;
  display: flex;
  align-items: center;
  justify-content: space-between;
  border-bottom: 1px solid inherit;
  border-bottom-color: var(--mist);
}
.result-card.ok     .rc-head { border-bottom-color: var(--jade-br); }
.result-card.warn   .rc-head { border-bottom-color: var(--amber-br); }
.result-card.danger .rc-head { border-bottom-color: var(--coral-br); }
.result-card.info   .rc-head { border-bottom-color: var(--electric-br); }

.rc-label { font-family: var(--f-mono); font-size: 10px; letter-spacing: 0.8px; text-transform: uppercase; font-weight: 500; }
.result-card.ok     .rc-label { color: var(--jade); }
.result-card.warn   .rc-label { color: var(--amber); }
.result-card.danger .rc-label { color: var(--coral); }
.result-card.info   .rc-label { color: var(--electric); }
.result-card .rc-label { color: var(--mid); }

.rc-body { padding: 12px; }

/* Empty state */
.empty-state {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 22px;
  gap: 6px;
  opacity: 0.5;
}
.empty-icon { font-size: 22px; }
.empty-text { font-family: var(--f-mono); font-size: 10.5px; color: var(--mid); text-align: center; }

/* Complexity meter */
.cplx-row {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 8px;
  margin-bottom: 10px;
}
.cplx-item {
  text-align: center;
  padding: 10px 8px;
  border-radius: var(--r-sm);
  background: white;
  border: 1px solid var(--mist);
}
.cplx-val { font-family: var(--f-mono); font-size: 20px; font-weight: 700; color: var(--ink); line-height: 1; }
.cplx-key { font-family: var(--f-mono); font-size: 8.5px; letter-spacing: 1px; color: var(--mid); margin-top: 4px; text-transform: uppercase; }

.meter-bar {
  height: 6px;
  background: var(--mist);
  border-radius: 3px;
  overflow: hidden;
  margin: 8px 0 2px;
}
.meter-fill {
  height: 100%;
  border-radius: 3px;
  transition: width 0.7s cubic-bezier(0.4,0,0.2,1);
}
.mf-low  { background: linear-gradient(90deg, var(--jade), #34d399); }
.mf-mid  { background: linear-gradient(90deg, var(--amber), #fbbf24); }
.mf-high { background: linear-gradient(90deg, var(--coral), #f87171); }

/* Flag items */
.flag-line {
  display: flex;
  align-items: flex-start;
  gap: 9px;
  padding: 8px 10px;
  border-radius: var(--r-sm);
  margin-bottom: 5px;
  font-family: var(--f-mono);
  font-size: 11px;
  line-height: 1.5;
}
.flag-line:last-child { margin-bottom: 0; }
.flag-danger { background: var(--coral-bg); border: 1px solid var(--coral-br); color: var(--coral); }
.flag-warn   { background: var(--amber-bg); border: 1px solid var(--amber-br); color: var(--amber); }
.flag-ok     { background: var(--jade-bg);  border: 1px solid var(--jade-br);  color: var(--jade); }
.flag-ico    { flex-shrink: 0; margin-top: 1px; font-size: 12px; }

/* Token table */
.tok-row {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 5px 0;
  border-bottom: 1px solid var(--mist);
  font-family: var(--f-mono);
  font-size: 10.5px;
}
.tok-row:last-child { border-bottom: none; }
.tok-k { color: var(--mid); }
.tok-v { font-weight: 500; }
.tok-v.red   { color: var(--coral); }
.tok-v.green { color: var(--jade); }
.tok-v.blue  { color: var(--electric); }

/* Humanized text output */
.human-out {
  font-family: var(--f-serif);
  font-size: 15px;
  line-height: 1.9;
  color: var(--ink);
  font-style: normal;
}

/* Linguistic score cards */
.ling-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 8px; }
.ling-card {
  background: white;
  border: 1px solid var(--mist);
  border-radius: var(--r-md);
  padding: 12px;
  text-align: center;
  transition: box-shadow 0.2s;
}
.ling-card:hover { box-shadow: var(--shadow-md); }
.ling-val { font-family: var(--f-mono); font-size: 22px; font-weight: 700; line-height: 1; margin-bottom: 5px; }
.ling-key { font-family: var(--f-mono); font-size: 8.5px; letter-spacing: 1px; color: var(--mid); text-transform: uppercase; }
.ling-bar { height: 3px; border-radius: 2px; margin-top: 8px; }

/* ─── DASHBOARD ─── */
.dash { padding: 28px; overflow-y: auto; flex: 1; display: flex; flex-direction: column; gap: 24px; }

.dash-header {
  display: flex;
  align-items: flex-end;
  justify-content: space-between;
}

.dash-h1 {
  font-family: var(--f-display);
  font-size: 32px;
  font-weight: 800;
  letter-spacing: -1px;
  line-height: 1;
  color: var(--ink);
}

.dash-h1 em {
  font-style: normal;
  color: var(--electric);
}

.dash-sub {
  font-family: var(--f-mono);
  font-size: 10.5px;
  color: var(--mid);
  letter-spacing: 0.5px;
  margin-top: 6px;
}

.kpi-row { display: grid; grid-template-columns: repeat(4, 1fr); gap: 14px; }

.kpi {
  background: white;
  border: 1px solid var(--mist);
  border-radius: var(--r-lg);
  padding: 20px;
  box-shadow: var(--shadow-xs);
  transition: all 0.2s;
  position: relative;
  overflow: hidden;
}
.kpi::after {
  content: '';
  position: absolute;
  bottom: 0; left: 0; right: 0;
  height: 2px;
}
.kpi.kpi-blue::after  { background: var(--electric); }
.kpi.kpi-green::after { background: var(--jade); }
.kpi.kpi-amber::after { background: var(--amber); }
.kpi.kpi-dark::after  { background: var(--ink); }

.kpi:hover { box-shadow: var(--shadow-md); transform: translateY(-1px); }

.kpi-label {
  font-family: var(--f-mono);
  font-size: 9.5px;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: var(--mid);
  margin-bottom: 10px;
}

.kpi-value {
  font-family: var(--f-mono);
  font-size: 36px;
  font-weight: 700;
  line-height: 1;
  color: var(--ink);
  margin-bottom: 6px;
}
.kpi-blue  .kpi-value { color: var(--electric); }
.kpi-green .kpi-value { color: var(--jade); }
.kpi-amber .kpi-value { color: var(--amber); }

.kpi-note {
  font-family: var(--f-mono);
  font-size: 9.5px;
  color: var(--mid-2);
}

/* Chart section */
.chart-row { display: grid; grid-template-columns: 2fr 1fr; gap: 14px; }

.chart-card {
  background: white;
  border: 1px solid var(--mist);
  border-radius: var(--r-lg);
  padding: 20px;
  box-shadow: var(--shadow-xs);
}

.chart-title {
  font-family: var(--f-mono);
  font-size: 10px;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: var(--mid);
  margin-bottom: 16px;
  display: flex;
  align-items: center;
  gap: 8px;
}
.chart-title::before {
  content: '';
  width: 3px; height: 14px;
  background: var(--electric);
  border-radius: 2px;
  flex-shrink: 0;
}

/* Bar chart */
.bar-chart-wrap { display: flex; align-items: flex-end; gap: 10px; height: 130px; }
.bar-col { flex: 1; display: flex; flex-direction: column; align-items: center; gap: 6px; height: 100%; }
.bar-track { flex: 1; width: 100%; display: flex; flex-direction: column; justify-content: flex-end; }
.bar-el {
  width: 100%;
  border-radius: 4px 4px 0 0;
  min-height: 4px;
  transition: height 0.6s cubic-bezier(0.4,0,0.2,1);
}
.be-blue  { background: linear-gradient(0deg, var(--electric), #60a5fa); }
.be-green { background: linear-gradient(0deg, var(--jade), #34d399); }
.bar-lbl  { font-family: var(--f-mono); font-size: 8.5px; color: var(--mid-2); }

/* Donut */
.donut-wrap { display: flex; flex-direction: column; align-items: center; gap: 14px; }
.donut-svg-wrap { position: relative; width: 110px; height: 110px; }
.donut-svg-wrap svg { transform: rotate(-90deg); }
.donut-center {
  position: absolute;
  inset: 0;
  display: flex; flex-direction: column; align-items: center; justify-content: center;
}
.donut-pct { font-family: var(--f-mono); font-size: 16px; font-weight: 700; color: var(--ink); }
.donut-sub { font-family: var(--f-mono); font-size: 8px; color: var(--mid-2); }
.donut-legend { display: flex; flex-direction: column; gap: 7px; width: 100%; }
.dl-item { display: flex; align-items: center; gap: 8px; font-family: var(--f-mono); font-size: 10px; color: var(--mid); }
.dl-swatch { width: 8px; height: 8px; border-radius: 2px; flex-shrink: 0; }

/* Jobs table */
.jobs-card {
  background: white;
  border: 1px solid var(--mist);
  border-radius: var(--r-lg);
  overflow: hidden;
  box-shadow: var(--shadow-xs);
}

.jobs-thead {
  display: grid;
  grid-template-columns: 76px 1fr 110px 90px 85px 100px;
  padding: 10px 16px;
  background: var(--mist-3);
  border-bottom: 1px solid var(--mist);
  font-family: var(--f-mono);
  font-size: 9px;
  letter-spacing: 1px;
  text-transform: uppercase;
  color: var(--mid);
}

.jobs-row {
  display: grid;
  grid-template-columns: 76px 1fr 110px 90px 85px 100px;
  padding: 12px 16px;
  border-bottom: 1px solid var(--mist);
  font-family: var(--f-mono);
  font-size: 11px;
  color: var(--mid);
  align-items: center;
  transition: background 0.1s;
}
.jobs-row:last-child { border-bottom: none; }
.jobs-row:hover { background: var(--mist-3); }

.jid { color: var(--electric); font-size: 10px; }
.jtype { color: var(--ink); }
.jstatus-pill {
  display: inline-flex; align-items: center; gap: 5px;
  padding: 2px 8px; border-radius: 20px;
  font-size: 9px; letter-spacing: 0.5px; border: 1px solid;
}
.jsp-done    { color: var(--jade);  background: var(--jade-bg);  border-color: var(--jade-br); }
.jsp-running { color: var(--electric); background: var(--electric-bg); border-color: var(--electric-br); }
.jsp-fail    { color: var(--coral); background: var(--coral-bg); border-color: var(--coral-br); }

.jscore { font-weight: 700; }
.jscore.low  { color: var(--jade); }
.jscore.mid  { color: var(--amber); }
.jscore.high { color: var(--coral); }

/* ─── ARCHITECTURE VIEW ─── */
.arch-view {
  padding: 28px;
  overflow-y: auto;
  flex: 1;
}

.arch-intro {
  margin-bottom: 28px;
}

.arch-h1 {
  font-family: var(--f-display);
  font-size: 28px;
  font-weight: 800;
  letter-spacing: -0.5px;
  color: var(--ink);
  margin-bottom: 6px;
}

.arch-sub {
  font-family: var(--f-mono);
  font-size: 10.5px;
  color: var(--mid);
  letter-spacing: 0.5px;
}

.arch-flow {
  display: flex;
  flex-direction: column;
  gap: 0;
  max-width: 860px;
}

.arch-layer {
  display: flex;
  flex-direction: column;
  gap: 0;
}

.arch-box {
  background: white;
  border: 1px solid var(--mist);
  border-radius: var(--r-lg);
  padding: 16px 20px;
  display: flex;
  align-items: center;
  gap: 14px;
  box-shadow: var(--shadow-xs);
  cursor: default;
  transition: all 0.2s;
  max-width: 320px;
  margin: 0 auto;
}
.arch-box:hover { box-shadow: var(--shadow-md); border-color: var(--electric-br); }

.arch-box-icon {
  width: 38px; height: 38px;
  border-radius: var(--r-md);
  display: flex; align-items: center; justify-content: center;
  font-size: 18px;
  flex-shrink: 0;
}
.abi-blue   { background: var(--electric-bg); }
.abi-green  { background: var(--jade-bg); }
.abi-amber  { background: var(--amber-bg); }
.abi-violet { background: var(--violet-bg); }
.abi-dark   { background: var(--mist-2); }

.arch-box-info {}
.arch-box-title { font-family: var(--f-display); font-size: 14px; font-weight: 700; color: var(--ink); }
.arch-box-sub   { font-family: var(--f-mono); font-size: 9.5px; color: var(--mid); margin-top: 3px; letter-spacing: 0.3px; }

.arch-connector {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 8px 0;
  color: var(--mid-2);
  font-size: 18px;
  gap: 2px;
}
.arch-connector-label {
  font-family: var(--f-mono);
  font-size: 9px;
  letter-spacing: 0.5px;
  color: var(--mid-2);
}

.arch-split-wrap {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 14px;
  margin: 0;
}

.arch-branch {
  border: 1px dashed var(--mist);
  border-radius: var(--r-lg);
  padding: 14px;
  display: flex;
  flex-direction: column;
  gap: 0;
}

.arch-branch-header {
  font-family: var(--f-mono);
  font-size: 9px;
  letter-spacing: 1px;
  text-transform: uppercase;
  padding: 4px 10px;
  border-radius: 20px;
  border: 1px solid;
  text-align: center;
  margin-bottom: 12px;
  align-self: center;
}

.abh-blue  { color: var(--electric); border-color: var(--electric-br); background: var(--electric-bg); }
.abh-green { color: var(--jade); border-color: var(--jade-br); background: var(--jade-bg); }

.arch-branch .arch-box { max-width: 100%; margin: 0; }
.arch-branch .arch-connector { padding: 6px 0; font-size: 14px; }

.arch-merge { display: flex; justify-content: center; align-items: center; gap: 8px; margin: 8px 0; color: var(--mid-2); font-size: 14px; letter-spacing: 4px; }

.tech-tags { display: flex; flex-wrap: wrap; gap: 7px; margin-top: 24px; }
.tech-tag {
  font-family: var(--f-mono);
  font-size: 10px;
  padding: 4px 10px;
  border-radius: 20px;
  border: 1px solid;
  letter-spacing: 0.5px;
}

/* ─── TOAST ─── */
.toast-stack {
  position: fixed;
  bottom: 24px; right: 24px;
  z-index: 999;
  display: flex;
  flex-direction: column;
  gap: 8px;
  pointer-events: none;
}

.toast {
  display: flex;
  align-items: center;
  gap: 10px;
  padding: 11px 16px;
  background: var(--ink);
  color: white;
  border-radius: var(--r-md);
  font-family: var(--f-mono);
  font-size: 11.5px;
  box-shadow: var(--shadow-xl);
  animation: toast-in 0.3s cubic-bezier(0.4,0,0.2,1), toast-out 0.3s ease 2.7s forwards;
  pointer-events: all;
  max-width: 320px;
}
.toast.t-success { background: var(--jade); }
.toast.t-error   { background: var(--coral); }
.toast.t-info    { background: var(--electric); }

@keyframes toast-in  { from { transform: translateY(10px) scale(0.97); opacity: 0; } to { transform: none; opacity: 1; } }
@keyframes toast-out { to   { transform: translateX(20px); opacity: 0; } }

/* ─── UTILITY ─── */
.fade-in { animation: fadeIn 0.35s ease forwards; }
@keyframes fadeIn { from { opacity:0; transform:translateY(6px); } to { opacity:1; transform:none; } }

::-webkit-scrollbar { width: 5px; height: 5px; }
::-webkit-scrollbar-track { background: transparent; }
::-webkit-scrollbar-thumb { background: var(--mist); border-radius: 3px; }
::-webkit-scrollbar-thumb:hover { background: var(--mid-2); }

@media (max-width: 1100px) {
  .layout { grid-template-columns: 260px 1fr; }
  .kpi-row { grid-template-columns: 1fr 1fr; }
}
@media (max-width: 860px) {
  .layout { grid-template-columns: 1fr; }
  .rail { display: none; }
  .dual-grid { grid-template-columns: 1fr; }
  .jobs-thead, .jobs-row { grid-template-columns: 70px 1fr 90px 70px; }
  .jobs-thead > *:nth-child(5),.jobs-thead > *:nth-child(6),
  .jobs-row > *:nth-child(5),.jobs-row > *:nth-child(6) { display: none; }
}
</style>
</head>
<body>

<!-- ══ NAV ══ -->
<nav class="nav">
  <div class="nav-brand">
    <div class="nav-logo-mark">
      <svg width="18" height="18" viewBox="0 0 18 18" fill="none">
        <path d="M4 9h3M11 9h3M9 4v3M9 11v3" stroke="white" stroke-width="1.8" stroke-linecap="round"/>
        <circle cx="9" cy="9" r="2" fill="white"/>
        <rect x="1" y="1" width="16" height="16" rx="3" stroke="rgba(255,255,255,0.3)" stroke-width="1"/>
      </svg>
    </div>
    <div>
      <div class="nav-name">PARSIQ<sup>™</sup></div>
    </div>
    <div class="nav-tagline">Parse · Analyze · Humanize</div>
  </div>

  <nav class="nav-pills">
    <div class="nav-pill active" data-tab="analyzer">
      <span class="tab-dot td-blue"></span> Dual Pipeline
    </div>
    <div class="nav-pill" data-tab="dashboard">
      <span class="tab-dot td-green"></span> Dashboard
    </div>
    <div class="nav-pill" data-tab="architecture">
      <span class="tab-dot td-violet"></span> Architecture
    </div>
  </nav>

  <div class="nav-status">
    <div class="pulse"></div>
    All Systems Operational
  </div>
</nav>

<!-- ══ PIPELINE BREADCRUMB ══ -->
<div class="pipeline-track">
  <div class="pipe-step active" id="ps1">
    <div class="pipe-num"><span>1</span></div>Submission
  </div>
  <div class="pipe-arrow">›</div>
  <div class="pipe-step idle" id="ps2">
    <div class="pipe-num"><span>2</span></div>AST Parse
  </div>
  <div class="pipe-arrow">›</div>
  <div class="pipe-step idle" id="ps3">
    <div class="pipe-num"><span>3</span></div>Tree Traverse
  </div>
  <div class="pipe-arrow">›</div>
  <div class="pipe-step idle" id="ps4">
    <div class="pipe-num"><span>4</span></div>Regex Sanitize
  </div>
  <div class="pipe-arrow">›</div>
  <div class="pipe-step idle" id="ps5">
    <div class="pipe-num"><span>5</span></div>Redis Queue
  </div>
  <div class="pipe-arrow">›</div>
  <div class="pipe-step idle" id="ps6">
    <div class="pipe-num"><span>6</span></div>Celery Worker
  </div>
  <div class="pipe-arrow">›</div>
  <div class="pipe-step idle" id="ps7">
    <div class="pipe-num"><span>7</span></div>LLM Humanize
  </div>
  <div class="pipe-arrow">›</div>
  <div class="pipe-step idle" id="ps8">
    <div class="pipe-num"><span>8</span></div>DB Write
  </div>
</div>

<!-- ══ LAYOUT ══ -->
<div class="layout">

  <!-- ── LEFT RAIL ── -->
  <aside class="rail">

    <!-- Live Metrics -->
    <div class="rail-section">
      <div class="rail-head">
        <div class="rail-title">Live Metrics</div>
        <div class="rail-badge rb-green">LIVE</div>
      </div>
      <div class="stat-grid">
        <div class="stat-tile">
          <div class="stat-num blue" id="mTotal">0</div>
          <div class="stat-lbl">Jobs Total</div>
        </div>
        <div class="stat-tile">
          <div class="stat-num green" id="mDone">0</div>
          <div class="stat-lbl">Completed</div>
        </div>
        <div class="stat-tile">
          <div class="stat-num amber" id="mComplexity">0.0</div>
          <div class="stat-lbl">Avg Complexity</div>
        </div>
        <div class="stat-tile">
          <div class="stat-num" id="mChars">0</div>
          <div class="stat-lbl">Chars Modified</div>
        </div>
      </div>
    </div>

    <!-- AST Tree -->
    <div class="rail-section">
      <div class="rail-head">
        <div class="rail-title">Syntax Tree</div>
        <div class="rail-badge rb-blue">AST</div>
      </div>
      <div class="ast-tree" id="astTree">
        <div style="color:var(--mid-2);font-size:10px;padding:6px 0;">Submit code to render tree…</div>
      </div>
    </div>

    <!-- Celery Queue -->
    <div class="rail-section">
      <div class="rail-head">
        <div class="rail-title">Task Queue</div>
        <div class="rail-badge rb-violet">REDIS · CELERY</div>
      </div>
      <div id="queueList">
        <div style="color:var(--mid-2);font-family:var(--f-mono);font-size:10px;text-align:center;padding:10px 0;">Queue is empty</div>
      </div>
    </div>

    <!-- DB Aggregations -->
    <div class="rail-section" style="flex:1;">
      <div class="rail-head">
        <div class="rail-title">SQL Aggregations</div>
        <div class="rail-badge rb-amber">POSTGRESQL</div>
      </div>
      <div>
        <div class="db-row"><span class="db-key">AVG(complexity)</span><span class="db-val blue" id="dbAvg">—</span></div>
        <div class="db-row"><span class="db-key">SUM(chars_delta)</span><span class="db-val green" id="dbSum">—</span></div>
        <div class="db-row"><span class="db-key">MAX(nest_depth)</span><span class="db-val" id="dbMax">—</span></div>
        <div class="db-row"><span class="db-key">COUNT(dangerous)</span><span class="db-val amber" id="dbDanger">—</span></div>
        <div class="db-row"><span class="db-key">atomic_rollbacks</span><span class="db-val green">0</span></div>
      </div>
    </div>
  </aside>

  <!-- ── MAIN AREA ── -->
  <main class="main">

    <!-- Tab Strip (desktop hidden, nav pills used instead via JS) -->

    <!-- ══ PANEL: DUAL PIPELINE ══ -->
    <div class="panel active" id="panel-analyzer">
      <div class="dual-grid">

        <!-- TRACK A -->
        <div class="pipe-pane">
          <div class="pane-bar">
            <div class="pane-title">
              AST Code Analyzer
              <span class="track-chip rb-blue">TRACK A · CPU · LOCAL</span>
            </div>
            <button class="btn btn-ghost btn-sm" onclick="loadSampleCode()">Load Sample</button>
          </div>
          <div class="pane-body">

            <div>
              <div class="field-label">Python Source Code</div>
              <textarea id="codeInput" class="code-area" spellcheck="false" placeholder="# Paste your Python code here…
def process(items):
    for item in items:
        for sub in item.children:
            eval(sub.query)
            db.execute(sub.sql)"></textarea>
            </div>

            <div class="btn-row">
              <button class="btn btn-dark" onclick="runAST()">
                <span class="spin" id="astSpin"></span>
                ⬡ Parse AST
              </button>
              <button class="btn btn-ghost btn-sm" onclick="clearAST()">Clear</button>
            </div>

            <!-- Complexity -->
            <div>
              <div class="field-label">Complexity Analysis</div>
              <div id="complexityOut">
                <div class="result-card"><div class="empty-state"><div class="empty-icon">🌳</div><div class="empty-text">Parse code to compute complexity</div></div></div>
              </div>
            </div>

            <!-- Flags -->
            <div>
              <div class="field-label">Security Flags</div>
              <div id="flagsOut">
                <div class="result-card"><div class="empty-state"><div class="empty-icon">🔍</div><div class="empty-text">No flags detected</div></div></div>
              </div>
            </div>

            <!-- Token reduction -->
            <div>
              <div class="field-label">Pre-Filter Token Analysis</div>
              <div class="result-card" id="tokenOut">
                <div class="empty-state"><div class="empty-icon">✂️</div><div class="empty-text">Regex sanitizer awaiting input</div></div>
              </div>
            </div>

          </div>
        </div>

        <!-- TRACK B -->
        <div class="pipe-pane">
          <div class="pane-bar">
            <div class="pane-title">
              Linguistic Humanizer
              <span class="track-chip rb-green">TRACK B · ASYNC · LLM</span>
            </div>
            <button class="btn btn-ghost btn-sm" onclick="loadSampleText()">Load Sample</button>
          </div>
          <div class="pane-body">

            <div>
              <div class="field-label">AI-Generated Text Input</div>
              <textarea id="textInput" class="text-area" spellcheck="false" placeholder="Paste AI-generated text here…

Furthermore, it is important to delve into the multifaceted nature of this phenomenon. The implementation of this solution leverages state-of-the-art methodologies and demonstrates a robust framework."></textarea>
            </div>

            <div class="btn-row">
              <button class="btn btn-jade" onclick="runHumanizer()">
                <span class="spin" id="humanSpin"></span>
                ◈ Humanize Text
              </button>
              <div style="margin-left:auto; display:flex; align-items:center; gap:6px; font-family:var(--f-mono); font-size:10px; color:var(--mid);">
                <div class="pulse" id="workerPulse" style="width:6px;height:6px;"></div>
                <span id="workerLabel">Worker Idle</span>
              </div>
            </div>

            <!-- Pre-filter -->
            <div>
              <div class="field-label">Pre-Filter Sanitization</div>
              <div class="result-card" id="prefilterOut">
                <div class="empty-state"><div class="empty-icon">🧹</div><div class="empty-text">Regex pre-filter awaiting text</div></div>
              </div>
            </div>

            <!-- Humanized output -->
            <div>
              <div class="field-label">Humanized Output</div>
              <div class="result-card" id="humanOut">
                <div class="empty-state"><div class="empty-icon">✍️</div><div class="empty-text">LLM output will appear here after async processing</div></div>
              </div>
            </div>

            <!-- Linguistic metrics -->
            <div>
              <div class="field-label">Linguistic Metrics</div>
              <div id="lingOut">
                <div class="result-card"><div class="empty-state"><div class="empty-icon">📊</div><div class="empty-text">Perplexity · Burstiness scores pending</div></div></div>
              </div>
            </div>

          </div>
        </div>
      </div>
    </div>

    <!-- ══ PANEL: DASHBOARD ══ -->
    <div class="panel" id="panel-dashboard">
      <div class="dash">
        <div class="dash-header">
          <div>
            <div class="dash-h1">Analytics <em>Dashboard</em></div>
            <div class="dash-sub">AGGREGATED SQL · REAL-TIME · POSTGRESQL · AVG() · SUM()</div>
          </div>
          <button class="btn btn-ghost btn-sm" onclick="refreshDash()">↻ Refresh</button>
        </div>

        <div class="kpi-row">
          <div class="kpi kpi-blue">
            <div class="kpi-label">Total Submissions</div>
            <div class="kpi-value" id="dTotal">0</div>
            <div class="kpi-note">All-time PostgreSQL records</div>
          </div>
          <div class="kpi kpi-amber">
            <div class="kpi-label">Avg Complexity — AVG()</div>
            <div class="kpi-value" id="dComplexity">0.0</div>
            <div class="kpi-note">Nested loop depth score</div>
          </div>
          <div class="kpi kpi-green">
            <div class="kpi-label">Chars Modified — SUM()</div>
            <div class="kpi-value" id="dChars">0</div>
            <div class="kpi-note">Humanizer Δ, SQL aggregation</div>
          </div>
          <div class="kpi kpi-dark">
            <div class="kpi-label">Success Rate</div>
            <div class="kpi-value" id="dRate">100%</div>
            <div class="kpi-note">Atomic transaction health</div>
          </div>
        </div>

        <div class="chart-row">
          <div class="chart-card">
            <div class="chart-title">Complexity & Char Delta per Job</div>
            <div class="bar-chart-wrap" id="barChart">
              <div style="font-family:var(--f-mono);font-size:10px;color:var(--mid-2);margin:auto;">Run jobs to populate chart</div>
            </div>
            <div style="display:flex;gap:14px;margin-top:10px;">
              <div style="display:flex;align-items:center;gap:6px;font-family:var(--f-mono);font-size:9.5px;color:var(--mid);">
                <div style="width:10px;height:10px;border-radius:2px;background:var(--electric)"></div> Complexity
              </div>
              <div style="display:flex;align-items:center;gap:6px;font-family:var(--f-mono);font-size:9.5px;color:var(--mid);">
                <div style="width:10px;height:10px;border-radius:2px;background:var(--jade)"></div> Chars Δ
              </div>
            </div>
          </div>
          <div class="chart-card">
            <div class="chart-title">Task Outcomes</div>
            <div class="donut-wrap">
              <div class="donut-svg-wrap">
                <svg viewBox="0 0 100 100" width="110" height="110">
                  <circle cx="50" cy="50" r="38" fill="none" stroke="var(--mist)" stroke-width="10"/>
                  <circle cx="50" cy="50" r="38" fill="none" stroke="var(--jade)"     stroke-width="10" stroke-dasharray="200 239" stroke-linecap="round" id="donArc1"/>
                  <circle cx="50" cy="50" r="38" fill="none" stroke="var(--electric)" stroke-width="10" stroke-dasharray="35 239"  stroke-dashoffset="-203" stroke-linecap="round" id="donArc2"/>
                  <circle cx="50" cy="50" r="38" fill="none" stroke="var(--coral)"    stroke-width="10" stroke-dasharray="4 239"   stroke-dashoffset="-238" stroke-linecap="round" id="donArc3"/>
                </svg>
                <div class="donut-center">
                  <div class="donut-pct" id="donPct">100%</div>
                  <div class="donut-sub">success</div>
                </div>
              </div>
              <div class="donut-legend">
                <div class="dl-item"><div class="dl-swatch" style="background:var(--jade)"></div>Completed</div>
                <div class="dl-item"><div class="dl-swatch" style="background:var(--electric)"></div>Processing</div>
                <div class="dl-item"><div class="dl-swatch" style="background:var(--coral)"></div>Failed</div>
              </div>
            </div>
          </div>
        </div>

        <!-- Jobs Table -->
        <div class="jobs-card">
          <div class="jobs-thead">
            <div>Job ID</div>
            <div>Type</div>
            <div>Status</div>
            <div>Complexity</div>
            <div>Chars Δ</div>
            <div>Timestamp</div>
          </div>
          <div id="jobsBody">
            <div style="padding:24px;text-align:center;font-family:var(--f-mono);font-size:10.5px;color:var(--mid-2);">No records. Run the analyzer to populate.</div>
          </div>
        </div>
      </div>
    </div>

    <!-- ══ PANEL: ARCHITECTURE ══ -->
    <div class="panel" id="panel-architecture">
      <div class="arch-view">
        <div class="arch-intro">
          <div class="arch-h1">System Architecture</div>
          <div class="arch-sub">DUAL-PIPELINE · DECOUPLED · ASYNCHRONOUS · ATOMIC</div>
        </div>

        <div class="arch-flow">

          <!-- User -->
          <div style="display:flex;justify-content:center;">
            <div class="arch-box">
              <div class="arch-box-icon abi-blue">👤</div>
              <div class="arch-box-info">
                <div class="arch-box-title">User Submission</div>
                <div class="arch-box-sub">HTTP POST → Django View → transaction.atomic()</div>
              </div>
            </div>
          </div>

          <div class="arch-connector">↓<div class="arch-connector-label">SPLIT INTO DUAL PIPELINES</div></div>

          <!-- Split -->
          <div class="arch-split-wrap">

            <!-- Track A -->
            <div class="arch-branch">
              <div class="arch-branch-header abh-blue">TRACK A — LOCAL CPU</div>

              <div class="arch-box">
                <div class="arch-box-icon abi-blue">🌳</div>
                <div class="arch-box-info">
                  <div class="arch-box-title">ast.parse()</div>
                  <div class="arch-box-sub">Convert source → syntax tree graph</div>
                </div>
              </div>
              <div class="arch-connector">↓</div>
              <div class="arch-box">
                <div class="arch-box-icon abi-blue">🔁</div>
                <div class="arch-box-info">
                  <div class="arch-box-title">Recursive Traversal</div>
                  <div class="arch-box-sub">Count nested loops · Flag eval() · Flag db calls</div>
                </div>
              </div>
              <div class="arch-connector">↓</div>
              <div class="arch-box">
                <div class="arch-box-icon abi-dark">⚡</div>
                <div class="arch-box-info">
                  <div class="arch-box-title">Instant JSON Result</div>
                  <div class="arch-box-sub">Zero network calls · CPU-only · &lt;50ms</div>
                </div>
              </div>
            </div>

            <!-- Track B -->
            <div class="arch-branch">
              <div class="arch-branch-header abh-green">TRACK B — ASYNC WORKER</div>

              <div class="arch-box">
                <div class="arch-box-icon abi-green">🧹</div>
                <div class="arch-box-info">
                  <div class="arch-box-title">Regex Pre-Filter</div>
                  <div class="arch-box-sub">Strip AI fingerprints · Reduce token payload</div>
                </div>
              </div>
              <div class="arch-connector">↓</div>
              <div class="arch-box">
                <div class="arch-box-icon abi-violet">📥</div>
                <div class="arch-box-info">
                  <div class="arch-box-title">Redis Queue</div>
                  <div class="arch-box-sub">202 Accepted returned immediately to UI</div>
                </div>
              </div>
              <div class="arch-connector">↓</div>
              <div class="arch-box">
                <div class="arch-box-icon abi-violet">⚙️</div>
                <div class="arch-box-info">
                  <div class="arch-box-title">Celery Worker</div>
                  <div class="arch-box-sub">Background task · LLM API call · Retry logic</div>
                </div>
              </div>
              <div class="arch-connector">↓</div>
              <div class="arch-box">
                <div class="arch-box-icon abi-green">🧠</div>
                <div class="arch-box-info">
                  <div class="arch-box-title">LLM Humanizer</div>
                  <div class="arch-box-sub">Perplexity ↑ · Burstiness ↑ · System prompt</div>
                </div>
              </div>
            </div>
          </div>

          <div class="arch-merge">↘ &nbsp;&nbsp; ↙</div>

          <!-- PostgreSQL -->
          <div style="display:flex;justify-content:center;">
            <div class="arch-box">
              <div class="arch-box-icon abi-amber">🗄️</div>
              <div class="arch-box-info">
                <div class="arch-box-title">PostgreSQL</div>
                <div class="arch-box-sub">Atomic write · AVG() · SUM() · Single aggregated row returned</div>
              </div>
            </div>
          </div>
        </div>

        <div class="tech-tags">
          <div class="tech-tag rb-blue">Python AST</div>
          <div class="tech-tag rb-blue">Django ORM</div>
          <div class="tech-tag rb-green">Celery</div>
          <div class="tech-tag rb-green">Redis</div>
          <div class="tech-tag rb-amber">PostgreSQL</div>
          <div class="tech-tag rb-amber">transaction.atomic()</div>
          <div class="tech-tag rb-violet">LLM API</div>
          <div class="tech-tag rb-violet">Regex Sanitizer</div>
          <div class="tech-tag" style="color:var(--mid);border-color:var(--mist);background:var(--mist-2);">202 Accepted</div>
          <div class="tech-tag" style="color:var(--mid);border-color:var(--mist);background:var(--mist-2);">AVG() · SUM()</div>
        </div>
      </div>
    </div>

  </main>
</div>

<!-- TOAST -->
<div class="toast-stack" id="toasts"></div>

<script>
/* ══════════════════════════════════════════
   STATE
══════════════════════════════════════════ */
const S = {
  jobs: [],
  total: 0,
  done: 0,
  avgCplx: 0,
  totalChars: 0,
  maxDepth: 0,
  dangerCount: 0,
  queueItems: [],
};

/* ══════════════════════════════════════════
   NAV / TABS
══════════════════════════════════════════ */
document.querySelectorAll('.nav-pill').forEach(p => {
  p.addEventListener('click', () => {
    document.querySelectorAll('.nav-pill').forEach(x => x.classList.remove('active'));
    document.querySelectorAll('.panel').forEach(x => x.classList.remove('active'));
    p.classList.add('active');
    document.getElementById('panel-' + p.dataset.tab).classList.add('active');
    if (p.dataset.tab === 'dashboard') refreshDash();
  });
});

/* ══════════════════════════════════════════
   PIPELINE STEPPER
══════════════════════════════════════════ */
function sleep(ms) { return new Promise(r => setTimeout(r, ms)); }

async function animatePipeline(steps, delay = 380) {
  for (let i = 1; i <= 8; i++) {
    const el = document.getElementById('ps' + i);
    el.classList.remove('active', 'done');
    el.classList.add('idle');
  }
  for (let i = 1; i <= steps; i++) {
    const el = document.getElementById('ps' + i);
    el.classList.remove('idle', 'done');
    el.classList.add('active');
    await sleep(delay);
    el.classList.remove('active');
    el.classList.add('done');
  }
  const last = document.getElementById('ps' + steps);
  if (last) { last.classList.remove('done'); last.classList.add('active'); }
}

/* ══════════════════════════════════════════
   SAMPLES
══════════════════════════════════════════ */
function loadSampleCode() {
  document.getElementById('codeInput').value =
`def process_pipeline(records, cfg):
    results = []
    for record in records:
        for field in record.get_fields():
            if field.is_active:
                for rule in cfg.validation_rules:
                    if rule.applies(field):
                        outcome = eval(field.expr)
                        conn = db.get_connection()
                        conn.execute(rule.raw_sql)
                        results.append(outcome)
    return results

def safe_transform(data):
    return [x * 2 for x in data if x > 0]`;
}

function loadSampleText() {
  document.getElementById('textInput').value =
`Furthermore, it is important to delve into the multifaceted nature of this phenomenon. The implementation of this solution involves a comprehensive approach that leverages state-of-the-art methodologies. It is worth noting that the integration of these components demonstrates a robust framework for addressing the aforementioned challenges. In conclusion, the utilization of these advanced techniques showcases the potential for significant improvements in overall system performance and reliability.`;
}

/* ══════════════════════════════════════════
   AST ANALYSIS
══════════════════════════════════════════ */
function runAST() {
  const code = document.getElementById('codeInput').value.trim();
  if (!code) { toast('Paste Python code first', 'error'); return; }

  document.getElementById('astSpin').classList.add('show');
  animatePipeline(3, 320);

  setTimeout(() => {
    const a = analyzeCode(code);
    renderComplexity(a);
    renderFlags(a);
    renderTokens(code);
    renderASTTree(a);
    updateRailMetrics(a);
    recordJob('AST_ANALYSIS', a.complexity, 0, 'done');
    document.getElementById('astSpin').classList.remove('show');
    toast(`AST parsed · ${a.nodeCount} nodes · complexity ${a.complexity}`, 'success');
  }, 700);
}

function analyzeCode(code) {
  const lines = code.split('\n');
  let nestDepth = 0, maxD = 0, evalCount = 0, dbCount = 0, loops = 0, fns = 0, nodes = 0;

  lines.forEach(line => {
    const indent = line.length - line.trimStart().length;
    const d = Math.floor(indent / 4);
    maxD = Math.max(maxD, d);

    if (/^\s*(for|while)\s/.test(line))  { loops++; nestDepth = Math.max(nestDepth, d + 1); nodes++; }
    if (/^\s*def\s/.test(line))           { fns++; nodes++; }
    if (/\beval\s*\(/.test(line))         evalCount++;
    if (/\bdb\.\w+|\.execute\s*\(/.test(line)) dbCount++;
    if (line.trim() && !line.trim().startsWith('#')) nodes++;
  });

  const cplx = Math.round((nestDepth * 2.5 + loops * 1.2 + evalCount * 3) * 10) / 10;
  return {
    nestDepth, maxD, evalCount, dbCount, loops, fns, nodeCount: nodes,
    complexity: cplx,
    level: cplx < 5 ? 'low' : cplx < 12 ? 'mid' : 'high'
  };
}

function renderComplexity(a) {
  const pct = Math.min(100, (a.complexity / 20) * 100);
  const cls = a.level === 'low' ? 'low' : a.level === 'mid' ? 'mid' : 'high';
  const col = a.level === 'low' ? 'var(--jade)' : a.level === 'mid' ? 'var(--amber)' : 'var(--coral)';

  document.getElementById('complexityOut').innerHTML = `
    <div class="result-card ${a.level === 'low' ? 'ok' : a.level === 'mid' ? 'warn' : 'danger'} fade-in">
      <div class="rc-head">
        <span class="rc-label">Complexity Score — ${a.complexity} · ${a.level.toUpperCase()}</span>
        <span class="rail-badge ${a.level === 'low' ? 'rb-green' : a.level === 'mid' ? 'rb-amber' : 'rb-coral'}">${a.level.toUpperCase()}</span>
      </div>
      <div class="rc-body">
        <div class="cplx-row">
          <div class="cplx-item"><div class="cplx-val">${a.nestDepth}</div><div class="cplx-key">Max Nesting</div></div>
          <div class="cplx-item"><div class="cplx-val">${a.loops}</div><div class="cplx-key">Loops</div></div>
          <div class="cplx-item"><div class="cplx-val">${a.fns}</div><div class="cplx-key">Functions</div></div>
        </div>
        <div class="meter-bar">
          <div class="meter-fill mf-${cls}" style="width:${pct}%"></div>
        </div>
        <div style="font-family:var(--f-mono);font-size:9px;color:var(--mid-2);text-align:right;">${Math.round(pct)}% of threshold</div>
      </div>
    </div>`;
}

function renderFlags(a) {
  let html = '';
  if (a.evalCount > 0)
    html += `<div class="flag-line flag-danger"><span class="flag-ico">⛔</span><span>eval() detected — ${a.evalCount} call(s) — unauthorized dynamic execution risk</span></div>`;
  if (a.dbCount > 0)
    html += `<div class="flag-line flag-warn"><span class="flag-ico">⚠️</span><span>Unhandled DB connection — ${a.dbCount} instance(s) — missing context manager</span></div>`;
  if (a.nestDepth >= 3)
    html += `<div class="flag-line flag-warn"><span class="flag-ico">⚠️</span><span>Deep nesting (${a.nestDepth} levels) — refactor recommended</span></div>`;
  if (!html)
    html = `<div class="flag-line flag-ok"><span class="flag-ico">✓</span><span>No dangerous patterns detected — code looks clean</span></div>`;

  document.getElementById('flagsOut').innerHTML = `
    <div class="result-card fade-in">
      <div class="rc-head"><span class="rc-label">Security & Quality Flags</span></div>
      <div class="rc-body" style="padding:8px 12px;">${html}</div>
    </div>`;
}

function renderTokens(code) {
  const orig = code.length;
  const patterns = ['furthermore','delve','multifaceted','aforementioned','utilize','robust','leverage','comprehensive'];
  let removed = 0, stripped = code;
  patterns.forEach(p => {
    const m = (stripped.match(new RegExp(p, 'gi')) || []).length;
    removed += m;
    stripped = stripped.replace(new RegExp(p, 'gi'), '');
  });
  const cleaned = stripped.replace(/\n{3,}/g, '\n\n').trim();
  const saved = orig - cleaned.length;

  document.getElementById('tokenOut').innerHTML = `
    <div class="rc-head"><span class="rc-label">Regex Token Sanitizer</span></div>
    <div class="rc-body">
      <div class="tok-row"><span class="tok-k">Original payload</span><span class="tok-v">${orig.toLocaleString()} chars</span></div>
      <div class="tok-row"><span class="tok-k">AI patterns stripped</span><span class="tok-v red">${removed} match(es)</span></div>
      <div class="tok-row"><span class="tok-k">Chars removed</span><span class="tok-v green">−${saved}</span></div>
      <div class="tok-row"><span class="tok-k">Token reduction (est.)</span><span class="tok-v green">~${Math.ceil(saved/4)} tokens</span></div>
      <div class="tok-row"><span class="tok-k">Status</span><span class="tok-v blue">SANITIZED ✓</span></div>
    </div>`;
}

function renderASTTree(a) {
  document.getElementById('astTree').innerHTML = `
    <div class="ast-node-row"><span class="t-node">Module</span></div>
    <div class="ast-indent">
      <div class="ast-node-row"><span class="t-fn">FunctionDef</span><span class="t-node"> × ${a.fns}</span></div>
      <div class="ast-indent">
        <div class="ast-node-row"><span class="t-loop">For / While</span><span class="t-node"> × ${a.loops}</span></div>
        ${a.nestDepth > 1 ? `<div class="ast-indent">
          <div class="ast-node-row"><span class="t-loop">For [nested]</span></div>
          <div class="ast-indent">` : ''}
            ${a.evalCount > 0 ? `<div class="ast-node-row"><span class="t-danger">⛔ Call[eval]</span><span class="t-node"> × ${a.evalCount}</span></div>` : ''}
            ${a.dbCount > 0  ? `<div class="ast-node-row"><span class="t-warn">⚠ Call[db]</span><span class="t-node"> × ${a.dbCount}</span></div>` : ''}
            <div class="ast-node-row"><span class="t-ok">✓ Assign / Return</span></div>
          ${a.nestDepth > 1 ? '</div></div>' : ''}
      </div>
    </div>
    <div style="margin-top:8px;font-size:9px;color:var(--mid-2);">${a.nodeCount} nodes traversed</div>`;
}

function updateRailMetrics(a) {
  S.maxDepth    = Math.max(S.maxDepth, a.nestDepth);
  S.dangerCount += a.evalCount + a.dbCount;
  S.avgCplx     = S.total > 0
    ? ((S.avgCplx * (S.total - 1) + a.complexity) / S.total)
    : a.complexity;

  document.getElementById('dbAvg').textContent    = S.avgCplx.toFixed(2);
  document.getElementById('dbSum').textContent    = S.totalChars.toLocaleString();
  document.getElementById('dbMax').textContent    = S.maxDepth;
  document.getElementById('dbDanger').textContent = S.dangerCount;
}

function clearAST() {
  document.getElementById('codeInput').value = '';
  document.getElementById('complexityOut').innerHTML = '<div class="result-card"><div class="empty-state"><div class="empty-icon">🌳</div><div class="empty-text">Parse code to compute complexity</div></div></div>';
  document.getElementById('flagsOut').innerHTML      = '<div class="result-card"><div class="empty-state"><div class="empty-icon">🔍</div><div class="empty-text">No flags detected</div></div></div>';
  document.getElementById('tokenOut').innerHTML      = '<div class="empty-state"><div class="empty-icon">✂️</div><div class="empty-text">Regex sanitizer awaiting input</div></div>';
  document.getElementById('astTree').innerHTML       = '<div style="color:var(--mid-2);font-size:10px;padding:6px 0;">Submit code to render tree…</div>';
}

/* ══════════════════════════════════════════
   HUMANIZER
══════════════════════════════════════════ */
function runHumanizer() {
  const text = document.getElementById('textInput').value.trim();
  if (!text) { toast('Paste text to humanize first', 'error'); return; }

  document.getElementById('humanSpin').classList.add('show');
  document.getElementById('workerLabel').textContent = 'Sanitizing…';
  const wp = document.getElementById('workerPulse');
  wp.style.background = 'var(--amber)';

  animatePipeline(8, 360);

  setTimeout(() => {
    const pf = prefilter(text);
    renderPrefilter(pf);
    document.getElementById('workerLabel').textContent = 'Queued → Redis';

    addQueue({ task: 'humanize_text', status: 'active' });

    setTimeout(() => {
      document.getElementById('workerLabel').textContent = 'Worker Processing…';
      wp.style.background = 'var(--electric)';

      setTimeout(() => {
        const out = simulateHumanize(pf.cleaned);
        renderHumanized(out, text.length);
        document.getElementById('humanSpin').classList.remove('show');
        document.getElementById('workerLabel').textContent = 'Worker Idle';
        wp.style.background = 'var(--jade)';
        updateQueueFirst('done');
        const delta = Math.abs(out.length - text.length);
        S.totalChars += delta;
        document.getElementById('mChars').textContent  = S.totalChars.toLocaleString();
        document.getElementById('dbSum').textContent   = S.totalChars.toLocaleString();
        recordJob('HUMANIZE_TEXT', 0, delta, 'done');
        toast('Text humanized · perplexity ↑ · burstiness ↑', 'success');
      }, 2400);
    }, 800);
  }, 500);
}

function prefilter(text) {
  const pats = [
    /furthermore,?\s*/gi, /it is (important|worth noting) to\s*/gi,
    /delve into\s*/gi, /multifaceted\s*/gi, /aforementioned\s*/gi,
    /leverag(e|ing|es)\s*/gi, /\brobust\b\s*/gi, /\bcomprehensive\b\s*/gi,
    /in conclusion,?\s*/gi, /state-of-the-art\s*/gi, /\bshowcases?\b\s*/gi,
    /utiliz(e|ation|ing)\s*/gi, /\bhereby\b\s*/gi,
  ];
  let cleaned = text, removed = 0;
  pats.forEach(re => {
    const m = (cleaned.match(re)||[]).length;
    removed += m;
    cleaned = cleaned.replace(re, '');
  });
  cleaned = cleaned.replace(/\s{2,}/g, ' ').trim();
  return { original: text, cleaned, removed, saved: text.length - cleaned.length };
}

function renderPrefilter(p) {
  document.getElementById('prefilterOut').innerHTML = `
    <div class="rc-head"><span class="rc-label">Regex Pre-Filter Results</span></div>
    <div class="rc-body">
      <div class="tok-row"><span class="tok-k">AI fingerprints stripped</span><span class="tok-v red">${p.removed} pattern(s)</span></div>
      <div class="tok-row"><span class="tok-k">Payload before</span><span class="tok-v">${p.original.length} chars</span></div>
      <div class="tok-row"><span class="tok-k">Payload after</span><span class="tok-v green">${p.cleaned.length} chars</span></div>
      <div class="tok-row"><span class="tok-k">Tokens saved (est.)</span><span class="tok-v blue">~${Math.ceil(p.saved/4)}</span></div>
    </div>`;
}

function simulateHumanize(text) {
  const sents = text.split(/(?<=[.!?])\s+/).filter(Boolean);
  return sents.map((s, i) => {
    let t = s
      .replace(/\bimportant\b/gi, 'critical')
      .replace(/\bsolution\b/gi, 'fix')
      .replace(/\bimplementation\b/gi, 'approach')
      .replace(/\bcomponents\b/gi, 'parts')
      .replace(/\bperformance\b/gi, 'speed')
      .replace(/\breliability\b/gi, 'stability')
      .replace(/\bintegration\b/gi, 'connection')
      .replace(/^The /i, i % 2 ? 'This ' : 'That ');
    // Burstiness: shorten every 3rd sentence
    if (i % 3 === 0 && t.length > 70)
      t = t.substring(0, Math.ceil(t.length * 0.55)).trimEnd() + '.';
    return t.trim();
  }).join(' ');
}

function renderHumanized(text, origLen) {
  document.getElementById('humanOut').innerHTML = `
    <div class="rc-head"><span class="rc-label">Humanized Output — 202 Accepted · Async Complete</span></div>
    <div class="rc-body"><div class="human-out">${text}</div></div>`;

  const perp = (Math.random() * 14 + 72).toFixed(1);
  const burst = (Math.random() * 0.28 + 0.62).toFixed(2);
  const read  = (Math.random() * 10 + 67).toFixed(1);

  document.getElementById('lingOut').innerHTML = `
    <div class="result-card ok fade-in">
      <div class="rc-head"><span class="rc-label">Linguistic Quality Scores</span></div>
      <div class="rc-body">
        <div class="ling-grid">
          <div class="ling-card">
            <div class="ling-val" style="color:var(--jade);">${perp}</div>
            <div class="ling-key">Perplexity</div>
            <div class="ling-bar" style="background:linear-gradient(90deg,var(--jade),#34d399);width:${perp}%"></div>
          </div>
          <div class="ling-card">
            <div class="ling-val" style="color:var(--electric);">${burst}</div>
            <div class="ling-key">Burstiness</div>
            <div class="ling-bar" style="background:linear-gradient(90deg,var(--electric),#60a5fa);width:${burst*100}%"></div>
          </div>
          <div class="ling-card">
            <div class="ling-val" style="color:var(--amber);">${read}</div>
            <div class="ling-key">Readability</div>
            <div class="ling-bar" style="background:linear-gradient(90deg,var(--amber),#fbbf24);width:${read}%"></div>
          </div>
        </div>
      </div>
    </div>`;
}

/* ══════════════════════════════════════════
   QUEUE
══════════════════════════════════════════ */
function addQueue(item) {
  item.id = 'TK-' + Math.random().toString(36).substring(2,7).toUpperCase();
  item.ts = new Date().toLocaleTimeString([], {hour:'2-digit',minute:'2-digit',second:'2-digit'});
  S.queueItems.unshift(item);
  if (S.queueItems.length > 4) S.queueItems.pop();
  renderQueue();
}
function updateQueueFirst(status) {
  if (S.queueItems[0]) S.queueItems[0].status = status;
  renderQueue();
}
function renderQueue() {
  const el = document.getElementById('queueList');
  if (!S.queueItems.length) {
    el.innerHTML = '<div style="color:var(--mid-2);font-family:var(--f-mono);font-size:10px;text-align:center;padding:10px 0;">Queue is empty</div>';
    return;
  }
  el.innerHTML = S.queueItems.map(q => `
    <div class="q-item ${q.status}">
      <div class="q-dot"></div>
      <div class="q-id">${q.id}</div>
      <div class="q-name">${q.task}</div>
      <div class="q-ts">${q.ts}</div>
    </div>`).join('');
}

/* ══════════════════════════════════════════
   JOBS
══════════════════════════════════════════ */
function recordJob(type, cplx, chars, status) {
  S.total++;
  if (status === 'done') S.done++;
  if (cplx > 0) S.avgCplx = ((S.avgCplx * (S.total-1)) + cplx) / S.total;

  S.jobs.unshift({
    id: 'JB-' + Math.random().toString(36).substring(2,6).toUpperCase(),
    type, cplx, chars, status,
    ts: new Date().toLocaleTimeString([], {hour:'2-digit',minute:'2-digit',second:'2-digit'})
  });

  document.getElementById('mTotal').textContent     = S.total;
  document.getElementById('mDone').textContent      = S.done;
  document.getElementById('mComplexity').textContent= S.avgCplx.toFixed(1);
}

/* ══════════════════════════════════════════
   DASHBOARD
══════════════════════════════════════════ */
function refreshDash() {
  document.getElementById('dTotal').textContent     = S.total;
  document.getElementById('dComplexity').textContent= S.avgCplx.toFixed(1);
  document.getElementById('dChars').textContent     = S.totalChars.toLocaleString();
  const rate = S.total === 0 ? '100%' : Math.round((S.done/S.total)*100)+'%';
  document.getElementById('dRate').textContent = rate;
  document.getElementById('donPct').textContent = rate;
  renderBarChart();
  renderJobsTable();
}

function renderBarChart() {
  const el = document.getElementById('barChart');
  if (!S.jobs.length) {
    el.innerHTML = '<div style="font-family:var(--f-mono);font-size:10px;color:var(--mid-2);margin:auto;">Run jobs to populate chart</div>';
    return;
  }
  const recent = S.jobs.slice(0,8).reverse();
  const maxC   = Math.max(...recent.map(j=>j.cplx), 1);
  const maxCh  = Math.max(...recent.map(j=>j.chars), 1);

  el.innerHTML = recent.map(j => `
    <div class="bar-col">
      <div class="bar-track">
        <div class="bar-el be-blue" style="height:${Math.max(4,(j.cplx/maxC)*120)}px" title="Complexity: ${j.cplx}"></div>
      </div>
      <div class="bar-lbl">${j.id.replace('JB-','')}</div>
    </div>
    <div class="bar-col">
      <div class="bar-track">
        <div class="bar-el be-green" style="height:${Math.max(4,(j.chars/maxCh)*120)}px" title="Chars: ${j.chars}"></div>
      </div>
      <div class="bar-lbl">Δ${j.chars}</div>
    </div>`).join('');
}

function renderJobsTable() {
  const el = document.getElementById('jobsBody');
  if (!S.jobs.length) {
    el.innerHTML = '<div style="padding:24px;text-align:center;font-family:var(--f-mono);font-size:10.5px;color:var(--mid-2);">No records. Run the analyzer to populate.</div>';
    return;
  }
  el.innerHTML = S.jobs.slice(0,10).map(j => {
    const lvl = j.cplx > 12 ? 'high' : j.cplx > 5 ? 'mid' : 'low';
    return `
    <div class="jobs-row fade-in">
      <div class="jid">${j.id}</div>
      <div class="jtype">${j.type}</div>
      <div><span class="jstatus-pill jsp-${j.status === 'done' ? 'done' : 'running'}">${j.status.toUpperCase()}</span></div>
      <div class="jscore ${lvl}">${j.cplx || '—'}</div>
      <div style="color:var(--jade);">+${j.chars}</div>
      <div style="color:var(--mid-2);font-size:10px;">${j.ts}</div>
    </div>`;
  }).join('');
}

/* ══════════════════════════════════════════
   TOAST
══════════════════════════════════════════ */
function toast(msg, type='info') {
  const area = document.getElementById('toasts');
  const t = document.createElement('div');
  t.className = `toast t-${type}`;
  const icons = { success: '✓', error: '✕', info: '◈' };
  t.innerHTML = `<span>${icons[type]||'◈'}</span> ${msg}`;
  area.appendChild(t);
  setTimeout(() => t.remove(), 3100);
}

/* ══════════════════════════════════════════
   INIT
══════════════════════════════════════════ */
renderQueue();
renderJobsTable();
renderBarChart();
</script>
</body>
</html>
