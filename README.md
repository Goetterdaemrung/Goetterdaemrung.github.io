<!DOCTYPE html>

<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>NeuroWard</title>
<link href="https://fonts.googleapis.com/css2?family=IBM+Plex+Mono:wght@400;600&family=IBM+Plex+Sans:wght@300;400;500;600;700&display=swap" rel="stylesheet">
<style>
:root{
  --bg:#0b0f14;--surface:#111720;--surface2:#161e2a;--surface3:#1c2639;
  --border:#1e2d3d;--border2:#253447;
  --accent:#3b82f6;--accent2:#0ea5e9;
  --green:#22c55e;--yellow:#f59e0b;--red:#ef4444;--purple:#a78bfa;
  --text:#e2e8f0;--text2:#94a3b8;--text3:#475569;
  --mono:'IBM Plex Mono',monospace;--sans:'IBM Plex Sans',sans-serif;
  --s-not:#334155;--s-ord:#854d0e;--s-pend:#5b21b6;--s-done:#0f766e;--s-res:#166534;
  --sc-not:#64748b;--sc-ord:#fbbf24;--sc-pend:#c084fc;--sc-done:#2dd4bf;--sc-res:#4ade80;
}
*{box-sizing:border-box;margin:0;padding:0}
body{font-family:var(--sans);background:var(--bg);color:var(--text);height:100vh;display:flex;flex-direction:column;overflow:hidden}
#hdr{background:var(--surface);border-bottom:1px solid var(--border);padding:10px 20px;display:flex;align-items:center;justify-content:space-between;flex-shrink:0;gap:12px;flex-wrap:wrap}
.logo{font-family:var(--mono);font-size:16px;font-weight:600;color:var(--accent2)}
.logo span{color:var(--text2);font-weight:400}
.daybadge{font-family:var(--mono);font-size:11px;color:var(--text2);background:var(--surface2);border:1px solid var(--border);border-radius:6px;padding:4px 10px}
#layout{display:flex;flex:1;overflow:hidden}
#sidebar{width:240px;flex-shrink:0;background:var(--surface);border-right:1px solid var(--border);display:flex;flex-direction:column;overflow:hidden;transition:width 0.22s}
#sidebar.col{width:46px}
#sbhdr{padding:10px;border-bottom:1px solid var(--border);display:flex;gap:6px;align-items:center}
#sbsrch{flex:1;background:var(--bg);border:1px solid var(--border);border-radius:6px;padding:6px 8px;color:var(--text);font-size:12px;font-family:var(--sans);outline:none;min-width:0;transition:opacity 0.2s}
#sidebar.col #sbsrch{opacity:0;pointer-events:none;width:0;padding:0;border:none}
#sidebar.col .pti,.sidebar.col .ptd{display:none}
#tsb{background:none;border:1px solid var(--border);border-radius:6px;color:var(--text2);cursor:pointer;padding:4px 8px;font-size:14px;flex-shrink:0;line-height:1}
#ptlist{flex:1;overflow-y:auto}
.ptrow{padding:9px 10px;border-bottom:1px solid var(--border);cursor:pointer;transition:background 0.1s;display:flex;align-items:center;gap:8px}
.ptrow:hover{background:var(--surface2)}
.ptrow.act{background:#0f2040;border-left:3px solid var(--accent)}
.ptdot{width:9px;height:9px;border-radius:50%;flex-shrink:0}
.pti{flex:1;min-width:0}
.ptn{font-size:13px;font-weight:600;color:var(--text);white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.pts{font-size:11px;color:var(--text2);margin-top:1px;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
.ptd{font-family:var(--mono);font-size:10px;color:var(--accent2);flex-shrink:0}
#sbbtns{display:flex;gap:6px;padding:10px}
.sbBtn{flex:1;padding:8px;border:none;border-radius:6px;font-size:12px;font-weight:600;cursor:pointer;font-family:var(--sans)}
#main{flex:1;overflow-y:auto;padding:20px 28px}
#empty{display:flex;flex-direction:column;align-items:center;justify-content:center;height:100%;color:var(--text3);gap:12px}
.sec{margin-bottom:12px;background:var(--surface);border:1px solid var(--border);border-radius:10px;overflow:hidden}
.sechdr{display:flex;align-items:center;gap:8px;padding:11px 16px;cursor:pointer;user-select:none}
.sechdr:hover{background:var(--surface2)}
.sectit{font-family:var(--mono);font-size:10px;font-weight:600;color:var(--accent2);text-transform:uppercase;letter-spacing:2px;flex:1}
.secchev{color:var(--text3);font-size:11px;transition:transform 0.2s;flex-shrink:0}
.sechdr.open .secchev{transform:rotate(180deg)}
.secact{display:flex;gap:5px;align-items:center}
.secbody{padding:16px;display:none}
.secbody.open{display:block}
.g2{display:grid;grid-template-columns:1fr 1fr;gap:10px}
.g3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:10px}
.g4{display:grid;grid-template-columns:1fr 1fr 1fr 1fr;gap:10px}
.f{display:flex;flex-direction:column;gap:4px;margin-bottom:10px}
.f label{font-size:10px;font-weight:600;color:var(--text3);text-transform:uppercase;letter-spacing:0.8px}
input[type=text],input[type=date],input[type=number],select,textarea{background:var(--bg);border:1px solid var(--border2);border-radius:6px;padding:8px 10px;color:var(--text);font-size:13px;font-family:var(--sans);outline:none;width:100%;transition:border-color 0.15s}
input:focus,select:focus,textarea:focus{border-color:var(--accent)}
textarea{resize:vertical}
select option{background:var(--surface2)}
.numInput{-moz-appearance:textfield}
.numInput::-webkit-outer-spin-button,.numInput::-webkit-inner-spin-button{-webkit-appearance:none}
#pthdr{background:var(--surface);border:1px solid var(--border);border-radius:10px;padding:16px;margin-bottom:12px}
.chip{font-family:var(--mono);font-size:11px;background:var(--surface2);border:1px solid var(--border2);border-radius:5px;padding:3px 8px;color:var(--text2)}
.chip.hl{color:var(--accent2);border-color:var(--accent)}
.chip.diag{color:var(--purple);border-color:#6d28d9;background:#1a0f33}
#astrip{display:flex;flex-wrap:wrap;gap:5px;margin-bottom:10px}
.ab{font-size:11px;font-weight:600;border-radius:5px;padding:4px 10px;display:inline-flex;align-items:center;gap:4px;cursor:pointer;border:1px solid;transition:opacity 0.15s}
.ab:hover{opacity:0.75}
.ab-r{background:#2d0f0f;border-color:var(--red);color:#fca5a5}
.ab-y{background:#2d1f00;border-color:var(--yellow);color:#fcd34d}
.ab-b{background:#0f1f3d;border-color:var(--accent);color:#93c5fd}
.ab-t{background:#042f2e;border-color:#0f766e;color:#5eead4}
.irow{padding:6px;border-bottom:1px solid var(--border);border-left:3px solid transparent;border-radius:3px;margin-bottom:2px}
.irow:hover{background:var(--surface2)}
.irow.st-not-ordered{border-left-color:var(--s-not)}
.irow.st-ordered{border-left-color:var(--sc-ord);background:#1c150a}
.irow.st-pending{border-left-color:var(--sc-pend);background:#150f22}
.irow.st-done-pending-result{border-left-color:var(--sc-done);background:#031917}
.irow.st-resulted{border-left-color:var(--sc-res);background:#052210}
.iname{background:transparent;border:none;color:var(--text);font-size:12px;font-weight:500;outline:none;width:100%;padding:0}
.iname:focus{border-bottom:1px dashed var(--accent2)}
.sbdg{font-size:9px;font-weight:700;font-family:var(--mono);border-radius:4px;padding:2px 7px;white-space:nowrap;cursor:pointer;border:none;outline:none}
.bd-not-ordered{background:var(--s-not);color:var(--sc-not)}
.bd-ordered{background:var(--s-ord);color:var(--sc-ord)}
.bd-pending{background:var(--s-pend);color:var(--sc-pend)}
.bd-done-pending-result{background:var(--s-done);color:var(--sc-done)}
.bd-resulted{background:var(--s-res);color:var(--sc-res)}
.labt{width:100%;border-collapse:collapse;font-size:12px}
.labt th{background:var(--surface2);color:var(--text2);padding:5px 8px;text-align:left;font-family:var(--mono);font-size:10px;border-bottom:1px solid var(--border);white-space:nowrap}
.labt td{padding:4px 8px;border-bottom:1px solid var(--border);color:var(--text);white-space:nowrap}
.labt tr:hover td{background:var(--surface2)}
.lgrp td{background:var(--surface);color:var(--accent2);font-weight:700;font-size:10px;text-transform:uppercase;letter-spacing:1px;padding:6px 8px}
.acell{background:#2d0f0f!important}
.aval{color:#fca5a5!important;font-weight:700}
.txcard{background:var(--surface2);border:1px solid var(--border2);border-radius:8px;padding:14px;margin-bottom:10px}
.txhdr{display:flex;justify-content:space-between;align-items:center;margin-bottom:10px;gap:8px;flex-wrap:wrap}
.txname{font-weight:700;color:var(--accent2);font-size:14px}
.txday{font-family:var(--mono);font-size:11px;background:#0f2040;border:1px solid var(--accent);color:var(--accent2);border-radius:4px;padding:2px 8px}
.pbar{height:6px;background:var(--border);border-radius:3px;overflow:hidden;margin-top:6px}
.pfill{height:100%;background:var(--accent);border-radius:3px;transition:width 0.3s}
.pfill.done{background:var(--green)}
.cc{background:var(--surface2);border:1px solid var(--border2);border-radius:8px;padding:12px;margin-bottom:8px}
.cc.rsp{border-color:var(--green)}
.cc.awt{border-color:var(--yellow)}
.abcard{background:var(--surface2);border:1px solid var(--border2);border-radius:8px;padding:10px;margin-bottom:8px}
.abcard.exp{border-color:var(--red);background:#1a0808}
.abcard.exp2{border-color:var(--yellow)}
.devrow{display:grid;grid-template-columns:160px 140px 1fr 1fr 140px auto;gap:8px;align-items:end;padding:8px 0;border-bottom:1px solid var(--border)}
.evcard{background:var(--surface2);border-left:3px solid var(--accent);border-radius:0 8px 8px 0;padding:12px;margin-bottom:8px}
.fuentry{background:var(--surface2);border:1px solid var(--border2);border-radius:8px;padding:10px;margin-bottom:8px}
.guide-box{border-radius:8px;padding:12px;margin-bottom:14px;cursor:pointer}
.guide-hist{background:#1a1500;border:1px solid #713f12}
.guide-exam{background:#0d1a2d;border:1px solid #1e3a5f}
.guide-title{font-size:11px;font-weight:700;display:flex;align-items:center;justify-content:space-between}
.guide-content{margin-top:10px;font-size:12px;color:var(--text2);line-height:1.7;display:none}
.guide-content.open{display:block}
.guide-content ul{padding-left:16px}
.guide-content li{margin-bottom:3px}
/* Concurrent meds */
.medcard{border-radius:8px;padding:12px;margin-bottom:8px;border:1px solid var(--border2);background:var(--surface2)}
.medcard.stopped{border-color:var(--red);background:#1a0808;opacity:0.8}
.medcard.onhold{border-color:var(--yellow);background:#1a1000}
.btn{border:none;border-radius:6px;padding:7px 14px;cursor:pointer;font-size:12px;font-weight:600;font-family:var(--sans)}
.btnp{background:var(--accent);color:#fff}
.btng{background:var(--surface2);color:var(--text2);border:1px solid var(--border2)}
.btnd{background:#2d0f0f;color:#fca5a5;border:1px solid var(--red)}
.btnsm{padding:4px 10px;font-size:11px}
.btngrn{background:#0d2a0d;color:#86efac;border:1px solid var(--green)}
.addb{font-family:var(--sans);font-size:11px;background:var(--surface2);border:1px solid var(--border2);color:var(--accent2);border-radius:4px;padding:3px 9px;cursor:pointer}
.rmbtn{background:none;border:none;color:var(--text3);cursor:pointer;font-size:14px;padding:2px 5px;border-radius:3px}
.rmbtn:hover{color:var(--red);background:#2d0f0f}
.mvbtn{background:none;border:none;color:var(--text3);cursor:pointer;font-size:11px;padding:1px 4px;border-radius:3px}
.mvbtn:hover{background:var(--surface3);color:var(--text)}
.mov{position:fixed;inset:0;background:rgba(0,0,0,0.87);z-index:1000;display:flex;align-items:center;justify-content:center;padding:20px}
.mobox{background:var(--surface);border:1px solid var(--border2);border-radius:12px;padding:24px;width:100%;max-width:560px;max-height:90vh;overflow-y:auto}
.mobox h3{font-size:16px;font-weight:700;margin-bottom:16px;color:var(--accent2)}
.mofot{display:flex;justify-content:flex-end;gap:8px;margin-top:16px}
.mobox.wide{max-width:720px}
.tpl-btn{background:var(--surface2);border:1px solid var(--border2);border-radius:8px;padding:10px 14px;cursor:pointer;text-align:left;transition:border-color 0.15s;font-family:var(--sans);width:100%}
.tpl-btn:hover{border-color:var(--accent2)}
.tpl-name{font-weight:700;font-size:13px;color:var(--accent2)}
.tpl-sub{font-size:11px;color:var(--text3);margin-top:2px}
.scanbox{border:2px dashed var(--border2);border-radius:12px;padding:32px;text-align:center}
/* Network warning */
.net-warn{background:#1a0f00;border:1px solid var(--yellow);border-radius:8px;padding:14px;margin-bottom:12px;font-size:12px;color:#fcd34d;line-height:1.6}
::-webkit-scrollbar{width:5px;height:5px}
::-webkit-scrollbar-track{background:transparent}
::-webkit-scrollbar-thumb{background:var(--border2);border-radius:4px}
.mt8{margin-top:8px}.mb8{margin-bottom:8px}.mb12{margin-bottom:12px}
.sa{scroll-margin-top:16px}
@media print{
  #sidebar,#hdr,.btn-row,#astrip,#sbbtns{display:none!important}
  #layout{display:block}#main{padding:0;overflow:visible}
  .sec{break-inside:avoid;border:1px solid #ccc;margin-bottom:8px;page-break-inside:avoid}
  .sechdr{background:#f0f4ff}
  .secbody{display:block!important;padding:10px}
  body{background:#fff;color:#000}
  .chip{border:1px solid #ccc;background:#f5f5f5;color:#333}
}
</style>
</head>
<body>
<div id="hdr">
  <div class="logo">⚕ Neuro<span>Ward</span></div>
  <div style="display:flex;gap:8px;align-items:center;flex-wrap:wrap">
    <button class="btn btng btnsm" onclick="exportData()">⬇ Export</button>
    <label class="btn btng btnsm" style="cursor:pointer">⬆ Import<input type="file" accept=".json" onchange="importData(this)" style="display:none"></label>
    <button class="btn btng btnsm" onclick="printPt()">🖨 Print Patient</button>
    <button class="btn btng btnsm" onclick="openPrintAll()">🖨 Print All</button>
    <button class="btn btng btnsm" onclick="exportMedFil48()">📄 Med-Fil-48 DOCX</button>
    <button class="btn btng btnsm" onclick="exportFullDischarge()">📋 Full Discharge DOCX</button>
    <div class="daybadge" id="daybadge"></div>
  </div>
</div>
<div id="layout">
  <div id="sidebar">
    <div id="sbhdr">
      <input id="sbsrch" type="text" placeholder="Search…" oninput="renderList()">
      <button id="tsb" onclick="toggleSB()">‹</button>
    </div>
    <div id="ptlist"></div>
    <div id="sbbtns">
      <button class="sbBtn" style="background:var(--accent);color:#fff" onclick="openAdd()">+ Admit</button>
      <button class="sbBtn" style="background:var(--surface2);color:var(--accent2);border:1px solid var(--border2)" onclick="openTplModal()">📋 Template</button>
    </div>
  </div>
  <div id="main">
    <div id="empty"><div style="font-size:48px">🏥</div><p style="font-size:15px;color:var(--text3)">Select a patient or admit a new one</p></div>
    <div id="pview" style="display:none"></div>
  </div>
</div>

<!-- ADD MODAL -->

<div id="addmod" class="mov" style="display:none">
  <div class="mobox" style="max-width:640px">
    <h3 id="addtit">➕ Admit New Patient</h3>
    <div class="g2 mb8">
      <div class="f"><label>Full Name</label><input id="n-name" type="text"></div>
      <div class="f"><label>Arabic Name</label><input id="n-nar" type="text" dir="rtl"></div>
    </div>
    <div class="g4 mb8">
      <div class="f"><label>Age</label><input id="n-age" type="number" inputmode="numeric" class="numInput"></div>
      <div class="f"><label>Sex</label><select id="n-sex"><option>M</option><option>F</option></select></div>
      <div class="f"><label>DOA</label><input id="n-doa" type="date"></div>
      <div class="f"><label>DOS</label><input id="n-dos" type="date"></div>
    </div>
    <div class="g2 mb8">
      <div class="f"><label>Diagnosis</label><input id="n-diag" type="text"></div>
      <div class="f"><label>DDx</label><input id="n-ddx" type="text"></div>
    </div>
    <div class="g3 mb8">
      <div class="f"><label>Referring Staff</label><input id="n-ref" type="text"></div>
      <div class="f"><label>Handedness</label><select id="n-hand"><option>Right</option><option>Left</option><option>Ambidextrous</option></select></div>
      <div class="f"><label>Residence</label><input id="n-res" type="text"></div>
    </div>
    <div class="g2 mb8">
      <div class="f"><label>Occupation</label><input id="n-occ" type="text"></div>
      <div class="f"><label>Marital</label><select id="n-mar"><option>Married</option><option>Single</option><option>Divorced</option><option>Widowed</option></select></div>
    </div>
    <div id="addfot" class="mofot">
      <button class="btn btng" onclick="closeM('addmod')">Cancel</button>
      <button class="btn btnp" onclick="admitPt()">Admit</button>
    </div>
  </div>
</div>

<!-- TEMPLATE MODAL -->

<div id="tplmod" class="mov" style="display:none">
  <div class="mobox wide">
    <h3>📋 Admit from Template</h3>
    <p style="font-size:12px;color:var(--text3);margin-bottom:16px">Investigations, treatments, and consults will be pre-configured. Fill in demographics next.</p>
    <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;margin-bottom:16px" id="tpl-grid"></div>
    <div class="mofot"><button class="btn btng" onclick="closeM('tplmod')">Cancel</button></div>
  </div>
</div>

<!-- LABS MODAL -->

<div id="labmod" class="mov" style="display:none">
  <div class="mobox wide">
    <h3>🧪 Add Lab Results</h3>
    <div style="font-size:11px;color:var(--text3);margin-bottom:12px">Add <b>!</b> after any abnormal value (e.g. <b>7.2!</b>) — triggers red alert until corrected.</div>
    <div class="f mb8"><label>Date</label><input id="lab-date" type="date" style="max-width:200px"></div>
    <div style="font-weight:700;font-size:11px;color:var(--accent2);margin-bottom:8px;text-transform:uppercase;letter-spacing:1px">CBC</div>
    <div class="g3 mb8">
      <div class="f"><label>Hb</label><input type="text" inputmode="decimal" id="lab-Hb"></div>
      <div class="f"><label>WBC</label><input type="text" inputmode="decimal" id="lab-WBC"></div>
      <div class="f"><label>PLT</label><input type="text" inputmode="decimal" id="lab-PLT"></div>
    </div>
    <div style="font-weight:700;font-size:11px;color:var(--accent2);margin-bottom:8px;text-transform:uppercase;letter-spacing:1px">Coagulation</div>
    <div class="g3 mb8">
      <div class="f"><label>INR</label><input type="text" inputmode="decimal" id="lab-INR"></div>
      <div class="f"><label>PTT</label><input type="text" inputmode="decimal" id="lab-PTT"></div>
      <div class="f"><label>Fibrinogen</label><input type="text" inputmode="decimal" id="lab-Fibrinogen"></div>
    </div>
    <div style="font-weight:700;font-size:11px;color:var(--accent2);margin-bottom:8px;text-transform:uppercase;letter-spacing:1px">Chemistry</div>
    <div class="g4 mb8">
      <div class="f"><label>Na</label><input type="text" inputmode="decimal" id="lab-Na"></div>
      <div class="f"><label>K</label><input type="text" inputmode="decimal" id="lab-K"></div>
      <div class="f"><label>Creatinine</label><input type="text" inputmode="decimal" id="lab-Creatinine"></div>
      <div class="f"><label>Urea</label><input type="text" inputmode="decimal" id="lab-Urea"></div>
      <div class="f"><label>ALT</label><input type="text" inputmode="decimal" id="lab-ALT"></div>
      <div class="f"><label>AST</label><input type="text" inputmode="decimal" id="lab-AST"></div>
      <div class="f"><label>Albumin</label><input type="text" inputmode="decimal" id="lab-Albumin"></div>
      <div class="f"><label>CRP</label><input type="text" inputmode="decimal" id="lab-CRP"></div>
      <div class="f"><label>Ca</label><input type="text" inputmode="decimal" id="lab-Ca"></div>
      <div class="f"><label>Mg</label><input type="text" inputmode="decimal" id="lab-Mg"></div>
      <div class="f"><label>Bilirubin</label><input type="text" inputmode="decimal" id="lab-Bilirubin"></div>
      <div class="f"><label>ALP</label><input type="text" inputmode="decimal" id="lab-ALP"></div>
    </div>
    <div class="mofot">
      <button class="btn btng" onclick="closeM('labmod')">Cancel</button>
      <button class="btn btnp" onclick="saveLabs()">Save Results</button>
    </div>
  </div>
</div>

<!-- VITALS MODAL -->

<div id="vitmod" class="mov" style="display:none">
  <div class="mobox">
    <h3>📊 Add Vital Signs</h3>
    <div class="f mb8"><label>Date</label><input id="vit-date" type="date" style="max-width:200px"></div>
    <div class="g3 mb8">
      <div class="f"><label>Temp (°C)</label><input type="text" inputmode="decimal" id="vit-temp"></div>
      <div class="f"><label>HR (bpm)</label><input type="text" inputmode="numeric" id="vit-hr"></div>
      <div class="f"><label>BP (mmHg)</label><input type="text" id="vit-bp" placeholder="120/80"></div>
    </div>
    <div class="g3 mb8">
      <div class="f"><label>RR (/min)</label><input type="text" inputmode="numeric" id="vit-rr"></div>
      <div class="f"><label>O₂ Sat (%)</label><input type="text" inputmode="numeric" id="vit-o2"></div>
      <div class="f"><label>Weight (kg)</label><input type="text" inputmode="decimal" id="vit-wt"></div>
    </div>
    <div class="mofot">
      <button class="btn btng" onclick="closeM('vitmod')">Cancel</button>
      <button class="btn btnp" onclick="saveVitals()">Save</button>
    </div>
  </div>
</div>

<!-- EVENT MODAL -->

<div id="evmod" class="mov" style="display:none">
  <div class="mobox">
    <h3>📋 Add Hospital Course Event</h3>
    <div class="f mb8"><label>Date</label><input id="ev-date" type="date" style="max-width:200px"></div>
    <div class="f"><label>Event / Note</label><textarea id="ev-text" rows="6" placeholder="Clinical event, procedure, response to treatment…"></textarea></div>
    <div class="mofot">
      <button class="btn btng" onclick="closeM('evmod')">Cancel</button>
      <button class="btn btnp" onclick="saveEv()">Add Event</button>
    </div>
  </div>
</div>

<!-- SCAN MODAL -->

<div id="scanmod" class="mov" style="display:none">
  <div class="mobox wide">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:14px">
      <h3 id="scan-title">📷 Scan Document</h3>
      <button class="rmbtn" onclick="closeM('scanmod')" style="font-size:18px">✕</button>
    </div>
    <div id="net-warn-scan" class="net-warn" style="display:none">
      ⚠️ <b>Network required for scanning.</b><br>
      If you opened this file directly (file://), scanning won't work in the browser for security reasons.<br>
      <b>Fix:</b> In your terminal run: <code style="background:#000;padding:2px 6px;border-radius:3px">python3 -m http.server 8080</code> then open <code style="background:#000;padding:2px 6px;border-radius:3px">http://localhost:8080</code> and use the file from there.
    </div>
    <div id="scan-mode-sel" style="display:flex;gap:8px;flex-wrap:wrap;margin-bottom:14px">
      <span style="font-size:11px;color:var(--text3);align-self:center">Type:</span>
      <button class="addb scan-mbtn" data-mode="admission" style="border-color:var(--accent2)">Admission Sheet</button>
      <button class="addb scan-mbtn" data-mode="labs">Lab Result</button>
      <button class="addb scan-mbtn" data-mode="radiology">Radiology Report</button>
      <button class="addb scan-mbtn" data-mode="consult">Consult Reply</button>
    </div>
    <div class="scanbox" id="scan-drop">
      <div style="font-size:40px;margin-bottom:10px">📸</div>
      <div style="color:var(--accent2);font-size:15px;margin-bottom:6px">Photograph or upload document</div>
      <div style="color:var(--text3);font-size:12px;margin-bottom:16px">Arabic, English, handwritten or printed — all supported</div>
      <input type="file" id="scan-file" accept="image/*" capture="environment" style="display:none" onchange="handleScanFile(this)">
      <button class="btn btnp" onclick="document.getElementById('scan-file').click()">📷 Take Photo / Upload</button>
    </div>
    <div id="scan-preview" style="display:none">
      <img id="scan-img" style="width:100%;max-height:260px;object-fit:contain;border-radius:8px;background:#000;margin-bottom:10px">
      <div style="display:flex;gap:8px;margin-bottom:12px">
        <button class="btn btng" onclick="resetScan()">🔄 Retake</button>
        <button class="btn btnp" id="scan-go-btn" onclick="runScan()" style="flex:1">🔍 Extract Data</button>
      </div>
    </div>
    <div id="scan-result" style="display:none">
      <div style="background:#0d2a1a;border:1px solid var(--green);border-radius:8px;padding:14px;margin-bottom:12px">
        <div style="color:var(--green);font-weight:700;font-size:13px;margin-bottom:8px">✅ Extracted — Review Before Saving</div>
        <div id="scan-unc" style="display:none;background:#2d2200;border:1px solid var(--yellow);border-radius:6px;padding:8px;margin-bottom:10px;font-size:12px;color:#fcd34d"></div>
        <div id="scan-prev-data" style="max-height:260px;overflow-y:auto;font-size:12px"></div>
      </div>
      <div style="display:flex;gap:8px">
        <button class="btn btng" onclick="resetScan()">🔄 Rescan</button>
        <button class="btn btnp" onclick="applyScan()" style="flex:1;font-weight:700">✅ Confirm & Save</button>
      </div>
    </div>
    <div id="scan-err" style="display:none;background:#2d0f0f;border:1px solid var(--red);border-radius:8px;padding:10px;color:#fca5a5;font-size:13px;margin-top:10px"></div>
  </div>
</div>

<!-- AI SUMMARY MODAL -->

<div id="aimod" class="mov" style="display:none">
  <div class="mobox wide">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:14px">
      <h3>🤖 AI Clinical Summary</h3>
      <button class="rmbtn" onclick="closeM('aimod')" style="font-size:18px">✕</button>
    </div>
    <div id="net-warn-ai" class="net-warn" style="display:none">
      ⚠️ <b>Network required for AI summary.</b><br>
      If you opened this file directly (file://), API calls are blocked by the browser.<br>
      <b>Fix:</b> Run <code style="background:#000;padding:2px 6px;border-radius:3px">python3 -m http.server 8080</code> then open via <code style="background:#000;padding:2px 6px;border-radius:3px">http://localhost:8080</code>
    </div>
    <div id="ai-loading" style="text-align:center;padding:30px;color:var(--text2);display:none">
      <div style="font-size:32px;margin-bottom:12px">⏳</div><div>Generating summary…</div>
    </div>
    <div id="ai-result" style="display:none">
      <div style="background:var(--surface2);border:1px solid var(--border2);border-radius:8px;padding:16px;white-space:pre-wrap;font-size:13px;line-height:1.7;max-height:60vh;overflow-y:auto" id="ai-text"></div>
      <div style="display:flex;gap:8px;margin-top:12px">
        <button class="btn btng" onclick="copyAI()">📋 Copy</button>
        <button class="btn btng" onclick="closeM('aimod')">Close</button>
      </div>
    </div>
  </div>
</div>

<!-- ATTACHMENTS MODAL -->

<div id="attmod" class="mov" style="display:none">
  <div class="mobox wide">
    <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:14px">
      <h3>📎 Patient Attachments</h3>
      <button class="rmbtn" onclick="closeM('attmod')" style="font-size:18px">✕</button>
    </div>
    <div style="font-size:12px;color:var(--text3);margin-bottom:14px">Attach images, PDFs, reports, or any documents. Files are stored in your browser (localStorage) — export/import to back them up.</div>
    <label class="btn btnp btnsm" style="cursor:pointer;display:inline-flex;align-items:center;gap:6px;margin-bottom:16px">
      ➕ Add File(s)
      <input type="file" id="att-file-input" multiple accept="image/*,.pdf,.doc,.docx,.txt" onchange="handleAttachFiles(this)" style="display:none">
    </label>
    <div id="att-list"></div>
  </div>
</div>

<script>
// ── PERMANENT STORAGE KEY ─────────────────────────────────────────────────────
// This key never changes between versions — your data is always preserved
const STORAGE_KEY = 'neuroward_patients';

// ── CONSTANTS ─────────────────────────────────────────────────────────────────
const DEF_SPEC=['Vasculitic Profile','CSF — Opening Pressure','CSF — Cells','CSF — Protein','CSF — Glucose','CSF — OCB','CSF — IgG Index','AntAQP4','Anti-MOG','Virology Screen','Quantiferon','Tumor Markers','Thyroid Profile','Thrombophilia Profile','Cultures'];
const DEF_IMG=['MRI Brain + Spine (C+)','CT Chest (C+)','CT Angio','Echo','Duplex'];
const DEF_NP=['EMG & NCS','VEP','EEG'];
const LAB_G={'CBC':['Hb','WBC','PLT'],'Coagulation':['INR','PTT','Fibrinogen'],'Chemistry':['Na','K','Creatinine','Urea','ALT','AST','Albumin','CRP','Ca','Mg','Bilirubin','ALP']};
const LAB_KEYS=Object.values(LAB_G).flat();
const VIT_KEYS=['temp','hr','bp','rr','o2','wt'];
const VIT_LBL={temp:'Temp °C',hr:'HR bpm',bp:'BP mmHg',rr:'RR /min',o2:'O₂ Sat %',wt:'Weight kg'};
const DEV_T=['Mahurkar Catheter','CVL','Urinary Catheter',"Ryle's Tube",'Other'];
const ST_CYC=['not-ordered','ordered','pending','done-pending-result','resulted'];
const ST_LBL={'not-ordered':'Not Ordered','ordered':'Ordered','pending':'Pending','done-pending-result':'Done / Awaiting Report','resulted':'Resulted'};
const MED_STATUS={active:{label:'Active',color:'var(--green)',bg:'#052210'},onhold:{label:'On Hold',color:'var(--yellow)',bg:'#1a1200'},stopped:{label:'Stopped',color:'var(--red)',bg:'#1a0808'}};

// ── TEMPLATES ─────────────────────────────────────────────────────────────────
const TEMPLATES={
  ms:{name:'MS Relapse',icon:'🧠',diagnosis:'Multiple Sclerosis — Acute Relapse',
    histGuide:['Onset, progression, and character of current symptoms','Previous relapses — frequency, severity, recovery','Visual symptoms — loss, blur, pain on movement (optic neuritis)?','Sensory — level, distribution, Lhermitte sign?','Motor — weakness, spasticity, falls','Bladder and bowel dysfunction','Cognitive symptoms — memory, processing speed','Fatigue character and pattern','Preceding infection or fever (Uhthoff phenomenon)?','Current disease-modifying therapy — name, duration, compliance','Last MRI and when lesions were last assessed'],
    examGuide:['Visual acuity and colour vision (Ishihara) — both eyes','RAPD (relative afferent pupillary defect)','Internuclear ophthalmoplegia — medial rectus lag on horizontal gaze','Nystagmus — direction, type','Cerebellar signs — dysmetria, dysdiadochokinesia, intention tremor','Pyramidal signs — tone, power, reflexes, Babinski','Sensory level if myelopathy suspected','Bladder — suprapubic tenderness, retention signs','EDSS estimation'],
    specOrdered:['AntAQP4','Anti-MOG','CSF — OCB','CSF — IgG Index','CSF — Cells','CSF — Protein','CSF — Glucose','Vasculitic Profile'],imgOrdered:['MRI Brain + Spine (C+)'],npOrdered:['VEP'],ophthOrdered:true,treatments:['IVMP'],consults:['Ophthalmology']},
  mg:{name:'MG Crisis',icon:'💪',diagnosis:'Myasthenia Gravis — Crisis',
    histGuide:['Onset of weakness — ocular first, then bulbar or generalized?','Ptosis — unilateral or bilateral, variability, worse at end of day?','Diplopia — direction, fatigable?','Dysphagia, dysarthria, dysphonia — aspiration risk?','Respiratory symptoms — dyspnea on exertion, orthopnea','Current pyridostigmine dose and timing of last dose','Precipitating factors — infection, new medication, surgery, stress','Previous crisis — how many, ICU admissions?','Thymoma history — prior CT chest, thymectomy?','Distinguish myasthenic vs cholinergic crisis'],
    examGuide:['Ptosis — measure palpebral aperture, ice pack test','Fatigable ophthalmoplegia — sustained upgaze 60 seconds','Bulbar — palatal movement, gag reflex, nasal speech','Respiratory — single breath count, bedside FVC (critical if <1L)','Proximal limb weakness — arm abduction, hip flexion','Fatigability testing — 10 repetitions of arm abduction','Reflexes — typically preserved in MG','Cholinergic signs — secretions, bradycardia, miosis, diarrhea'],
    specOrdered:['Thyroid Profile','Tumor Markers'],imgOrdered:['CT Chest (C+)'],npOrdered:['EMG & NCS'],treatments:['PLEX','IVIG'],consults:['Ophthalmology'],otherLabs:['AchR Antibody','Anti-MuSK Antibody']},
  nmo:{name:'NMO / NMOSD',icon:'👁',diagnosis:'NMOSD — Acute Attack',
    histGuide:['Visual symptoms — acute painless vs painful loss, which eye, time course','Myelopathy symptoms — level, bilateral or asymmetric, progression rate','Intractable hiccups or nausea (area postrema syndrome)','Previous attacks — phenotype, severity, recovery','Prior AQP4 or MOG antibody status','Associated autoimmune conditions — SLE, Sjögren, thyroid disease','Bladder dysfunction — retention, urgency'],
    examGuide:['Visual acuity each eye, colour vision','RAPD — critical in optic neuritis','Fundus — disc pallor if prior optic neuritis','Sensory level — compare C and thoracic levels','Symmetric vs asymmetric weakness — NMO often severe and bilateral','Bladder assessment','Area postrema signs — persistent nausea and hiccups'],
    specOrdered:['AntAQP4','Anti-MOG','CSF — Cells','CSF — Protein','CSF — OCB','Vasculitic Profile'],imgOrdered:['MRI Brain + Spine (C+)'],npOrdered:['VEP'],ophthOrdered:true,treatments:['IVMP','PLEX'],consults:['Ophthalmology']},
  enceph:{name:'Autoimmune Encephalitis',icon:'🔥',diagnosis:'Autoimmune Encephalitis',
    histGuide:['Subacute onset — days to weeks of progressive confusion, psychiatric, or seizure symptoms','Psychiatric prodrome — anxiety, psychosis, behavioural change before neurological symptoms','Seizure type, frequency, focal vs generalized','Memory disturbance — anterograde amnesia, temporal lobe involvement','Movement disorder — orofacial dyskinesias, choreoathetosis (NMDAR)','Autonomic instability — heart rate, BP, temperature dysregulation','Preceding infection — HSV? Fever?','Recent malignancy or paraneoplastic workup'],
    examGuide:['Level of consciousness and fluctuation','Cognitive assessment — orientation, memory, attention','Psychiatric features — paranoia, hallucinations, catatonia','Orofacial dyskinesias, limb movements (NMDAR)','Faciobrachial dystonic seizures (LGI1)','Autonomic — HR, BP, temperature, diaphoresis'],
    specOrdered:['Vasculitic Profile','Virology Screen','CSF — Cells','CSF — Protein','CSF — OCB','CSF — IgG Index','Tumor Markers','Thyroid Profile'],imgOrdered:['MRI Brain + Spine (C+)'],npOrdered:['EEG'],treatments:['IVMP','IVIG'],otherLabs:['NMDAR Ab (serum+CSF)','LGI1 Ab','CASPR2 Ab','GABA-B Ab']},
  gbs:{name:'GBS',icon:'⚡',diagnosis:'Guillain-Barré Syndrome',
    histGuide:['Preceding infection — diarrhea (Campylobacter), URTI — timing before weakness onset','Pattern of weakness — ascending, proximal, distal, facial','Pace — nadir typically 2–4 weeks','Pain — back pain, radicular pain common early','Autonomic symptoms — bowel/bladder change, palpitations, BP swings','Respiratory symptoms — dyspnea, orthopnea (critical to assess)','Vaccination in preceding 6 weeks'],
    examGuide:['Motor — pattern of weakness, proximal vs distal, facial','Reflexes — areflexia or hyporeflexia is hallmark','Respiratory — bedside FVC (intubate if <20 mL/kg), single breath count','Rule of 20-30-40 — FVC <20, MIP <30, MEP <40','Autonomic — HR variability, BP lying vs standing','Sensory — glove and stocking pattern'],
    specOrdered:['CSF — Cells','CSF — Protein','CSF — Glucose','Vasculitic Profile','Virology Screen'],imgOrdered:['MRI Brain + Spine (C+)'],npOrdered:['EMG & NCS'],treatments:['IVIG','PLEX'],otherLabs:['Anti-ganglioside panel (GM1/GQ1b/GD1a)','Campylobacter serology']},
  stroke:{name:'Stroke / TIA',icon:'🩸',diagnosis:'Ischaemic Stroke / TIA',
    histGuide:['Exact time of symptom onset — last known well time','Symptom character — sudden onset vs stuttering','Focal deficits — face, arm, leg, speech, vision','Vascular risk factors — HTN, DM, AF, hyperlipidaemia, smoking','Antiplatelet or anticoagulation history','Recent procedures — catheterization, surgery','Neck pain or trauma — dissection?'],
    examGuide:['NIHSS scoring — do full assessment and document score','Blood pressure — both arms','Cardiac — AF on pulse, murmurs, carotid bruits','Visual fields — homonymous hemianopia','Neglect — extinction, inattention','Aphasia type — Broca, Wernicke, global'],
    specOrdered:['Vasculitic Profile','Thrombophilia Profile'],imgOrdered:['MRI Brain + Spine (C+)','CT Angio','Echo','Duplex'],npOrdered:[],consults:['Cardiology'],otherLabs:['Fasting lipids','HbA1c','Antiphospholipid antibodies']},
  infection:{name:'CNS Infection',icon:'🦠',diagnosis:'CNS Infection',
    histGuide:['Fever — onset, character, rigors','Headache — onset, severity, character, photophobia, phonophobia','Neck stiffness','Altered consciousness — confusion, drowsiness, GCS trend','Seizures — focal or generalized','Rash — meningococcal purpura?','Immunosuppression — HIV, steroids, chemotherapy','Contact history — TB exposure, travel, animal contact'],
    examGuide:['GCS and trend — most important','Meningism — Kernig, Brudzinski, neck stiffness','Fundoscopy — papilloedema before LP','Fever and hemodynamic status','Rash — petechiae, purpura, distribution','Signs of raised ICP — Cushing triad'],
    specOrdered:['CSF — Opening Pressure','CSF — Cells','CSF — Protein','CSF — Glucose','Virology Screen','Quantiferon','Cultures'],imgOrdered:['MRI Brain + Spine (C+)'],npOrdered:['EEG'],otherLabs:['Blood cultures x2','Procalcitonin','Serum Cryptococcal Ag']},
  epilepsy:{name:'Epilepsy / Status',icon:'🌩',diagnosis:'Epilepsy / Status Epilepticus',
    histGuide:['Seizure description — eyewitness account, duration, postictal phase','First seizure ever vs known epilepsy','Seizure type — focal aware, focal impaired, bilateral tonic-clonic','Aura — type, duration, consistency','Provoking factors — sleep deprivation, alcohol, missed medications','Medication compliance — current AED regimen, recent changes','Todd paresis — focal weakness after seizure'],
    examGuide:['GCS post-ictally — duration of impaired consciousness','Todd paresis — focal weakness, how long','Tongue biting — lateral tongue is epilepsy, tip is non-epileptic','Urinary incontinence','Focal cortical signs — structural cause?','Neurocutaneous stigmata — neurofibromatosis, tuberous sclerosis'],
    specOrdered:['Vasculitic Profile','Virology Screen','Thyroid Profile'],imgOrdered:['MRI Brain + Spine (C+)'],npOrdered:['EEG'],otherLabs:['AED levels (if on treatment)','Ammonia','Lactate']}
};

// ── DATA ──────────────────────────────────────────────────────────────────────
let patients=[],currentId=null,scanImgData=null,scannedResult=null,activeScanMode='admission';

function uid(){return Math.random().toString(36).slice(2,9)}
function mkInv(name){return{id:uid(),name,status:'not-ordered',orderedDate:'',doneDate:'',result:''}}

function newPt(d,tplKey){
  const p={
    id:Date.now(),templateKey:tplKey||null,
    name:d.name||'',nameAr:d.nameAr||'',age:d.age||'',sex:d.sex||'M',
    occupation:d.occupation||'',marital:d.marital||'Married',
    residence:d.residence||'',handedness:d.handedness||'Right',
    dos:d.dos||'',doa:d.doa||td(),
    diagnosis:d.diagnosis||'',ddx:d.ddx||'',referringStaff:d.ref||'',
    complaint:'',hpi:'',pastHistory:'',personalHistory:'',familyHistory:'',
    exam:{general:'',cn:'',tone:'',RUL:'',LUL:'',RLL:'',LLL:'',reflexesSup:'',reflexesDeep:'',plantar:'',sensSup:'',sensDeep:'',coordination:'',gait:'',cognition:''},
    specificLabs:DEF_SPEC.map(mkInv),otherLabs:[],imaging:DEF_IMG.map(mkInv),npStudies:DEF_NP.map(mkInv),
    routineLabs:[],vitals:[],concurrentMeds:[],
    ophthalmology:{va_r:'',va_l:'',oct:mkInv('OCT'),perimetry:mkInv('Perimetry'),notes:'',fundusEntries:[]},
    consultations:[],treatments:[],devices:[],antibiotics:[],hospitalCourse:[],
    collapsedSections:{}
  };
  if(tplKey&&TEMPLATES[tplKey])applyTpl(p,TEMPLATES[tplKey]);
  return p;
}

function applyTpl(p,T){
  if(T.diagnosis&&!p.diagnosis)p.diagnosis=T.diagnosis;
  const t=td();
  p.specificLabs.forEach(l=>{if((T.specOrdered||[]).some(n=>l.name.toLowerCase().includes(n.toLowerCase()))){l.status='ordered';l.orderedDate=t;}});
  p.imaging.forEach(i=>{if((T.imgOrdered||[]).some(n=>i.name.toLowerCase().includes(n.toLowerCase()))){i.status='ordered';i.orderedDate=t;}});
  p.npStudies.forEach(n=>{if((T.npOrdered||[]).some(x=>n.name.toLowerCase().includes(x.toLowerCase()))){n.status='ordered';n.orderedDate=t;}});
  (T.otherLabs||[]).forEach(name=>p.otherLabs.push({...mkInv(name),status:'ordered',orderedDate:t}));
  (T.treatments||[]).forEach(type=>{
    if(type==='IVMP')p.treatments.push({type:'IVMP',startDate:'',daysTotal:5});
    if(type==='PLEX')p.treatments.push({type:'PLEX',startDate:'',sessionsTotal:5,frequency:'every2days',labsDone:false,mahurkarDone:false,mahurkarDate:'',mahurkarSide:'Right',mahurkarSite:''});
    if(type==='IVIG')p.treatments.push({type:'IVIG',startDate:'',daysTotal:5,dose:'0.4g/kg/day'});
  });
  (T.consults||[]).forEach(s=>p.consultations.push({specialty:s,dateSent:t,responded:false,doctor:'',recommendations:''}));
  if(T.ophthOrdered){p.ophthalmology.oct.status='ordered';p.ophthalmology.oct.orderedDate=t;p.ophthalmology.perimetry.status='ordered';p.ophthalmology.perimetry.orderedDate=t;}
}

// ── STORAGE — PERMANENT KEY ───────────────────────────────────────────────────
function loadD(){
  try{
    // Load from permanent key
    let d=localStorage.getItem(STORAGE_KEY);
    // Migration: also check old keys from previous versions
    if(!d){
      for(const old of['nw_v5','neuroward_v4','neuroward_html_v1','neuroward_v3']){
        const od=localStorage.getItem(old);
        if(od){d=od;console.log('Migrated data from',old);break;}
      }
    }
    patients=d?JSON.parse(d):[];
  }catch{patients=[]}
}
function saveD(){try{localStorage.setItem(STORAGE_KEY,JSON.stringify(patients))}catch{}}
function getP(){return patients.find(p=>p.id===currentId)}
function upd(fn){const p=getP();if(!p)return;fn(p);saveD()}

// ── EXPORT / IMPORT ───────────────────────────────────────────────────────────
function exportData(){
  const blob=new Blob([JSON.stringify({version:'neuroward_v6',date:td(),patients},null,2)],{type:'application/json'});
  const a=document.createElement('a');
  a.href=URL.createObjectURL(blob);
  a.download='neuroward_backup_'+td()+'.json';
  a.click();URL.revokeObjectURL(a.href);
}

function importData(input){
  const f=input.files[0];if(!f)return;
  const reader=new FileReader();
  reader.onload=ev=>{
    try{
      const parsed=JSON.parse(ev.target.result);
      const imported=parsed.patients||parsed;
      if(!Array.isArray(imported)){alert('Invalid backup file.');return;}
      if(!confirm('Import '+imported.length+' patient(s)? This will MERGE with your current '+patients.length+' patient(s). Existing patients are kept.'))return;
      // Merge — avoid duplicates by id
      const existingIds=new Set(patients.map(p=>p.id));
      const newOnes=imported.filter(p=>!existingIds.has(p.id));
      patients=[...patients,...newOnes];
      saveD();renderList();
      alert('Imported '+newOnes.length+' new patient(s). '+( imported.length-newOnes.length)+' duplicate(s) skipped.');
    }catch{alert('Could not read file. Make sure it is a valid NeuroWard backup.');}
  };
  reader.readAsText(f);
  input.value='';
}

// ── HELPERS ───────────────────────────────────────────────────────────────────
function td(){return new Date().toISOString().split('T')[0]}
function dSince(d){if(!d)return null;return Math.floor((Date.now()-new Date(d))/86400000)}
function admDay(doa){const d=dSince(doa);return d!==null?'Day '+(d+1):'—'}
function fmtD(d){return d?new Date(d).toLocaleDateString('en-GB',{day:'2-digit',month:'short'}):'—'}
function esc(s){return String(s||'').replace(/&/g,'&amp;').replace(/</g,'&lt;').replace(/>/g,'&gt;').replace(/"/g,'&quot;')}
function isAbn(v){return typeof v==='string'&&v.trim().endsWith('!')}
function stripB(v){return String(v||'').replace(/!$/,'').trim()}
function isFileProto(){return location.protocol==='file:'}

// ── ALERTS ────────────────────────────────────────────────────────────────────
function getAlerts(p){
  const A=[];
  (p.antibiotics||[]).forEach(ab=>{
    if(!ab.startDate||!ab.days)return;
    const exp=new Date(ab.startDate);exp.setDate(exp.getDate()+ +ab.days);
    const diff=Math.floor((exp-Date.now())/86400000);
    if(diff<0)A.push({lv:'r',msg:ab.drug+' — EXPIRED',anc:'treatment'});
    else if(diff===0)A.push({lv:'r',msg:ab.drug+' — renewal TODAY',anc:'treatment'});
    else if(diff===1)A.push({lv:'y',msg:ab.drug+' — expires tomorrow',anc:'treatment'});
  });
  (p.consultations||[]).filter(c=>!c.responded).forEach(c=>A.push({lv:'y',msg:c.specialty+' — awaiting response',anc:'consults'}));
  const invA=(list,anc)=>list.forEach(l=>{
    if(l.status==='ordered')A.push({lv:'y',msg:l.name+' — Ordered',anc});
    else if(l.status==='pending'){const s=l.orderedDate?' · ordered '+fmtD(l.orderedDate):'';A.push({lv:'y',msg:l.name+' — Pending'+s,anc})}
    if(l.status==='done-pending-result'){const s=l.doneDate?' · done '+fmtD(l.doneDate):'';A.push({lv:'t',msg:l.name+' — awaiting report'+s,anc})}
  });
  invA([...(p.specificLabs||[]),...(p.otherLabs||[])],'investigations');
  invA(p.imaging||[],'investigations');
  invA(p.npStudies||[],'investigations');
  LAB_KEYS.forEach(key=>{
    const ent=[...(p.routineLabs||[])].sort((a,b)=>a.date.localeCompare(b.date));
    let lastAbn=null,fixed=false;
    ent.forEach(e=>{const v=e.values?.[key];if(!v)return;if(isAbn(v))lastAbn={v,date:e.date};else if(lastAbn)fixed=true;});
    if(lastAbn&&!fixed)A.push({lv:'r',msg:key+': '+stripB(lastAbn.v)+' ⚠ ('+fmtD(lastAbn.date)+')',anc:'labs'});
  });
  // Concurrent meds stopped
  (p.concurrentMeds||[]).filter(m=>m.status==='stopped').forEach(m=>A.push({lv:'y',msg:(m.name||'Medication')+' — STOPPED',anc:'treatment'}));
  (p.treatments||[]).forEach(tx=>{
    if(tx.type==='PLEX'){
      if(!tx.labsDone)A.push({lv:'r',msg:'PLEX — pre-treatment labs not confirmed',anc:'treatment'});
      if(!tx.mahurkarDone)A.push({lv:'r',msg:'PLEX — Mahurkar not confirmed',anc:'treatment'});
      if(tx.startDate){const info=plexInfo(tx);A.push({lv:'b',msg:'PLEX — '+info,anc:'treatment'});}
    }
    if(tx.type==='IVMP'&&tx.startDate){const d=dSince(tx.startDate)+1;if(d<=(tx.daysTotal||5))A.push({lv:'b',msg:'IVMP Day '+d+' of '+(tx.daysTotal||5),anc:'treatment'});}
    if(tx.type==='IVIG'&&tx.startDate){const d=dSince(tx.startDate)+1;if(d<=(tx.daysTotal||5))A.push({lv:'b',msg:'IVIG Day '+d+' of '+(tx.daysTotal||5),anc:'treatment'});}
  });
  return A;
}

function plexInfo(tx){
  if(!tx.startDate)return'not started';
  const freq=tx.frequency==='daily'?1:2;
  const dayNum=dSince(tx.startDate)+1;
  const sessNum=Math.ceil(dayNum/freq);
  const tot=tx.sessionsTotal||5;
  if(sessNum>tot)return'course complete';
  return'Session ~'+Math.min(sessNum,tot)+' of '+tot+' (Day '+dayNum+')';
}

function dotClr(p){const a=getAlerts(p);if(a.some(x=>x.lv==='r'))return'#ef4444';if(a.some(x=>x.lv==='y'||x.lv==='t'))return'#f59e0b';return'#22c55e'}

// ── SIDEBAR ───────────────────────────────────────────────────────────────────
let sbCol=false;
function toggleSB(){sbCol=!sbCol;document.getElementById('sidebar').classList.toggle('col',sbCol);document.getElementById('tsb').textContent=sbCol?'›':'‹'}
function renderList(){
  const q=(document.getElementById('sbsrch').value||'').toLowerCase();
  const fil=patients.filter(p=>!q||p.name.toLowerCase().includes(q)||(p.nameAr||'').includes(q)||(p.diagnosis||'').toLowerCase().includes(q));
  document.getElementById('ptlist').innerHTML=fil.map(p=>`
    <div class="ptrow ${p.id===currentId?'act':''}" onclick="selPt(${p.id})">
      <div class="ptdot" style="background:${dotClr(p)}"></div>
      <div class="pti"><div class="ptn">${esc(p.name)||'Unnamed'}</div><div class="pts">${esc(p.diagnosis)||'No diagnosis'}</div></div>
      <div class="ptd">${admDay(p.doa)}</div>
    </div>`).join('');
}
function selPt(id){currentId=id;renderList();renderPt()}

// ── SECTIONS ──────────────────────────────────────────────────────────────────
function togSec(key){
  upd(p=>{p.collapsedSections=p.collapsedSections||{};p.collapsedSections[key]=!p.collapsedSections[key]});
  const b=document.getElementById('sb-'+key),h=document.getElementById('sh-'+key);
  if(!b||!h)return;
  const open=b.classList.toggle('open');h.classList.toggle('open',open);
}
function secOpen(p,key){return!(p.collapsedSections||{})[key]}
function mkSec(p,key,title,content,actions){
  const open=secOpen(p,key);
  return`<div class="sec sa" id="s-${key}">
    <div class="sechdr ${open?'open':''}" id="sh-${key}" onclick="togSec('${key}')">
      <span class="sectit">${title}</span>
      ${actions?`<div class="secact" onclick="event.stopPropagation()">${actions}</div>`:''}
      <span class="secchev">▼</span>
    </div>
    <div class="secbody ${open?'open':''}" id="sb-${key}">${content}</div>
  </div>`;
}

function jumpTo(id){
  const el=document.getElementById(id);if(!el)return;
  el.scrollIntoView({behavior:'smooth',block:'start'});
  const key=id.replace('s-','');
  const body=document.getElementById('sb-'+key);
  if(body&&!body.classList.contains('open'))togSec(key);
}

// ── RENDER PATIENT ────────────────────────────────────────────────────────────
function renderPt(){
  const p=getP();if(!p)return;
  document.getElementById('empty').style.display='none';
  const pv=document.getElementById('pview');pv.style.display='block';
  const alerts=getAlerts(p);
  const ac=alerts.map(a=>`<span class="ab ab-${a.lv}" onclick="jumpTo('s-${a.anc}')">${a.msg}</span>`).join('');
  const tpl=p.templateKey?TEMPLATES[p.templateKey]:null;
  pv.innerHTML=`
    <div id="pthdr">
      <div style="display:flex;align-items:baseline;gap:12px;flex-wrap:wrap;margin-bottom:6px">
        <div style="font-size:22px;font-weight:700;color:var(--text)">${esc(p.name)||'Unnamed'}</div>
        ${p.nameAr?`<div style="font-size:16px;color:var(--text2);direction:rtl">${esc(p.nameAr)}</div>`:''}
      </div>
      <div style="display:flex;gap:6px;flex-wrap:wrap;margin:8px 0 10px">
        <span class="chip">${esc(p.age)}y ${esc(p.sex)}</span>
        ${p.handedness?`<span class="chip">${esc(p.handedness)}-handed</span>`:''}
        ${p.residence?`<span class="chip">📍${esc(p.residence)}</span>`:''}
        <span class="chip hl">DOA: ${fmtD(p.doa)} · ${admDay(p.doa)}</span>
        ${p.dos?`<span class="chip">DOS: ${fmtD(p.dos)}</span>`:''}
        ${p.diagnosis?`<span class="chip diag">${esc(p.diagnosis)}</span>`:''}
        ${p.ddx?`<span class="chip">DDx: ${esc(p.ddx)}</span>`:''}
        ${p.referringStaff?`<span class="chip">Ref: ${esc(p.referringStaff)}</span>`:''}
        ${tpl?`<span class="chip" style="color:#fbbf24;border-color:#854d0e;background:#1c1200">📋 ${tpl.name}</span>`:''}
      </div>
      ${ac?`<div id="astrip">${ac}</div>`:'<div style="font-size:12px;color:var(--green);margin-bottom:10px">✅ No active alerts</div>'}
      <div class="btn-row" style="display:flex;gap:8px;margin-top:10px;flex-wrap:wrap">
        <button class="btn btng btnsm" onclick="editInfo()">✏️ Edit Info</button>
        <button class="btn btng btnsm" onclick="openScan()">📷 Scan</button>
        <button class="btn btng btnsm" onclick="openAttachments()">📎 Attachments</button>
        <button class="btn btngrn btnsm" onclick="genAISummary()">🤖 AI Summary</button>
        <button class="btn btnd btnsm" onclick="delPt()">🗑 Remove</button>
      </div>
    </div>
    ${mkSec(p,'history','History',rHistory(p,tpl))}
    ${mkSec(p,'exam','Neurological Examination',rExam(p,tpl))}
    ${mkSec(p,'investigations','Investigations',rInv(p),`<button class="addb" onclick="addSL()">+Spec Lab</button> <button class="addb" onclick="addOL()">+Other Lab</button> <button class="addb" onclick="addImg()">+Imaging</button> <button class="addb" onclick="addNP()">+NP</button>`)}
    ${mkSec(p,'labs','Routine Labs — Trend',rLabs(p),`<button class="addb" onclick="openLabM()">+ Add Results</button>`)}
    ${mkSec(p,'vitals','Vital Signs — Trend',rVitals(p),`<button class="addb" onclick="openVitM()">+ Add Vitals</button>`)}
    ${mkSec(p,'ophthalmology','Ophthalmology',rOphth(p),`<button class="addb" onclick="addFundus()">+ Fundus</button>`)}
    ${mkSec(p,'consults','Consultations',rConsults(p),`<button class="addb" onclick="addConsult()">+ Add</button>`)}
    ${mkSec(p,'treatment','Treatment & Medications',rTreatment(p),`<button class="addb" onclick="addTx('IVMP')">+IVMP</button> <button class="addb" onclick="addTx('PLEX')">+PLEX</button> <button class="addb" onclick="addTx('IVIG')">+IVIG</button> <button class="addb" onclick="addAb()">+Antibiotic</button> <button class="addb" onclick="addMed()">+Medication</button>`)}
    ${mkSec(p,'devices','Devices & Lines',rDevices(p),`<button class="addb" onclick="addDev()">+ Add</button>`)}
    ${mkSec(p,'course','Hospital Course / Events',rCourse(p),`<button class="addb" onclick="openEvM()">+ Add Event</button>`)}
    <div style="height:40px"></div>`;
}

// ── HISTORY ───────────────────────────────────────────────────────────────────
function rHistory(p,tpl){
  const g=tpl?`<div class="guide-box guide-hist" onclick="this.querySelector('.gc').classList.toggle('open');this.querySelector('.gt').textContent=this.querySelector('.gc').classList.contains('open')?'▲':'▼'">
    <div class="guide-title"><span style="color:#fbbf24;font-weight:700;font-size:12px">📋 History Guide — ${tpl.name}</span><span class="gt" style="color:#f59e0b">▼</span></div>
    <div class="gc guide-content"><ul>${(tpl.histGuide||[]).map(x=>`<li>${x}</li>`).join('')}</ul></div>
  </div>`:''
  return`${g}<div style="display:grid;grid-template-columns:1.5fr 1fr;gap:20px">
    <div>
      <div class="f"><label>Chief Complaint</label><input type="text" value="${esc(p.complaint)}" onchange="updF('complaint',this.value)" style="font-size:14px;padding:10px 12px"></div>
      <div class="f"><label>History of Present Illness</label><textarea rows="11" onchange="updF('hpi',this.value)" style="font-size:14px;line-height:1.75;padding:10px 12px">${esc(p.hpi)}</textarea></div>

```
</div>
<div>
  <div class="f"><label>Past Medical History</label><textarea rows="4" onchange="updF('pastHistory',this.value)" style="font-size:13px;line-height:1.6">${esc(p.pastHistory)}</textarea></div>
  <div class="f"><label>Personal History</label><textarea rows="3" onchange="updF('personalHistory',this.value)" style="font-size:13px;line-height:1.6">${esc(p.personalHistory)}</textarea></div>
  <div class="f"><label>Family History</label><textarea rows="3" onchange="updF('familyHistory',this.value)" style="font-size:13px;line-height:1.6">${esc(p.familyHistory)}</textarea></div>
</div>
```

  </div>`;}

// ── EXAM ──────────────────────────────────────────────────────────────────────
function rExam(p,tpl){
const e=p.exam;
const ef=(k,lbl,rows)=>`<div class="f"><label>${lbl}</label><textarea rows="${rows||2}" onchange="updExam('${k}',this.value)" style="font-size:13px;line-height:1.6">${esc(e[k])}</textarea></div>`;
const g=tpl?`<div class="guide-box guide-exam" onclick="this.querySelector('.gc').classList.toggle('open');this.querySelector('.gt').textContent=this.querySelector('.gc').classList.contains('open')?'▲':'▼'"> <div class="guide-title"><span style="color:#93c5fd;font-weight:700;font-size:12px">🔍 Exam Guide — ${tpl.name}</span><span class="gt" style="color:var(--accent2)">▼</span></div> <div class="gc guide-content"><ul>${(tpl.examGuide||[]).map(x=>`<li>${x}</li>`).join(’’)}</ul></div>

  </div>`:''
  return`${g}
    <div class="g2 mb12">${ef('general','General Appearance / Consciousness',3)}${ef('cn','Cranial Nerves (CN I–XII)',3)}</div>
    <div class="f mb12"><label>Motor — Tone</label><textarea rows="2" onchange="updExam('tone',this.value)" style="font-size:13px">${esc(e.tone)}</textarea></div>
    <div style="margin-bottom:14px">
      <label style="font-size:10px;font-weight:600;color:var(--text3);text-transform:uppercase;letter-spacing:0.8px;display:block;margin-bottom:8px">Motor Power (MRC Grade)</label>
      <div style="display:grid;grid-template-columns:1fr 1fr;gap:10px;max-width:400px">
        ${['RUL','LUL','RLL','LLL'].map(k=>`<div class="f"><label>${{RUL:'R Upper Limb',LUL:'L Upper Limb',RLL:'R Lower Limb',LLL:'L Lower Limb'}[k]}</label><input type="text" value="${esc(e[k])}" onchange="updExam('${k}',this.value)" placeholder="e.g. 4+/5" style="font-size:14px;padding:9px 10px"></div>`).join('')}
      </div>
    </div>
    <div class="g3 mb12">${ef('reflexesSup','Superficial Reflexes',2)}${ef('reflexesDeep','Deep Reflexes',2)}${ef('plantar','Plantar Response',2)}</div>
    <div class="g4 mb12">${ef('sensSup','Sensory — Superficial',2)}${ef('sensDeep','Sensory — Deep',2)}${ef('coordination','Coordination',2)}${ef('gait','Gait',2)}</div>
    <div class="f">${ef('cognition','Cognition (MMSE / MoCA / Bedside Assessment)',3)}</div>`;}

// ── INVESTIGATIONS ────────────────────────────────────────────────────────────
function iRow(inv,i,lt){
const st=inv.status,showR=st===‘done-pending-result’||st===‘resulted’||inv.result,showD=st===‘pending’||st===‘done-pending-result’||st===‘resulted’;
return`<div class="irow st-${st}" id="ir-${inv.id}"> <div style="display:flex;gap:5px;align-items:center;justify-content:space-between"> <input class="iname" value="${esc(inv.name)}" onchange="updInv('${lt}','${inv.id}','name',this.value)" title="Rename"> <div style="display:flex;gap:3px;align-items:center;flex-shrink:0"> ${i>0?`<button class="mvbtn" onclick="movInv('${lt}','${inv.id}',-1)">↑</button>`:''} <button class="mvbtn" onclick="movInv('${lt}','${inv.id}',1)">↓</button> <button class="sbdg bd-${st}" onclick="cycSt('${lt}','${inv.id}')">${ST_LBL[st]}</button> <button class="rmbtn" onclick="rmInv('${lt}','${inv.id}')">✕</button> </div> </div> ${showD?`<div style="display:flex;gap:8px;margin-top:4px;flex-wrap:wrap">
<div style="display:flex;align-items:center;gap:4px;font-size:10px;color:var(--text3)">Ordered:<input type="date" value="${esc(inv.orderedDate)}" onchange="updInv('${lt}','${inv.id}','orderedDate',this.value)" style="font-size:11px;padding:2px 5px;width:128px"></div>
${st===‘done-pending-result’||st===‘resulted’?`<div style="display:flex;align-items:center;gap:4px;font-size:10px;color:var(--text3)">Done:<input type="date" value="${esc(inv.doneDate)}" onchange="updInv('${lt}','${inv.id}','doneDate',this.value)" style="font-size:11px;padding:2px 5px;width:128px"></div>`:’’}
</div>`:''} ${showR?`<div style="margin-top:5px"><textarea rows="2" placeholder="Result / report…" onchange="updInv('${lt}','${inv.id}','result',this.value)" style="font-size:12px;padding:5px 8px;background:var(--bg)">${esc(inv.result)}</textarea></div>`:’’}

  </div>`;}

function rInv(p){
const s=(p.specificLabs||[]).map((l,i)=>iRow(l,i,‘specificLabs’)).join(’’);
const o=(p.otherLabs||[]).map((l,i)=>iRow(l,i,‘otherLabs’)).join(’’);
const img=(p.imaging||[]).map((l,i)=>iRow(l,i,‘imaging’)).join(’’);
const np=(p.npStudies||[]).map((l,i)=>iRow(l,i,‘npStudies’)).join(’’);
return`<div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:20px">
<div>
<div style="font-size:10px;font-weight:700;color:var(--text2);text-transform:uppercase;letter-spacing:1px;margin-bottom:8px">Specific Labs</div>${s}
<div style="margin-top:12px;border-top:1px solid var(--border);padding-top:10px">
<div style="font-size:10px;font-weight:700;color:var(--text2);text-transform:uppercase;letter-spacing:1px;margin-bottom:6px">Other / Additional Labs</div>${o}
</div>
</div>
<div><div style="font-size:10px;font-weight:700;color:var(--text2);text-transform:uppercase;letter-spacing:1px;margin-bottom:8px">Imaging</div>${img}</div>
<div><div style="font-size:10px;font-weight:700;color:var(--text2);text-transform:uppercase;letter-spacing:1px;margin-bottom:8px">Neurophysiology</div>${np}</div>

  </div>
  <div style="display:flex;gap:5px;flex-wrap:wrap;border-top:1px solid var(--border);padding-top:10px;margin-top:10px">
    ${ST_CYC.map(s=>`<span class="sbdg bd-${s}">${ST_LBL[s]}</span>`).join('')}
  </div>`;}

// ── LABS TREND ────────────────────────────────────────────────────────────────
function rLabs(p){
const labs=p.routineLabs||[];
if(!labs.length)return`<div style="color:var(--text3);font-size:13px">No results yet.</div>`;
const sorted=[…labs].sort((a,b)=>a.date.localeCompare(b.date));
const dates=sorted.map(l=>fmtD(l.date));
let h=`<div style="overflow-x:auto"><table class="labt"><thead><tr><th style="min-width:110px">Lab</th><th style="min-width:52px">Trend</th>${dates.map(d=>`<th>${d}</th>`).join('')}</tr></thead><tbody>`;
Object.entries(LAB_G).forEach(([grp,keys])=>{
h+=`<tr class="lgrp"><td colspan="${dates.length+2}">${grp}</td></tr>`;
keys.forEach(key=>{
const vals=sorted.map(e=>e.values?.[key]||’’);
if(vals.every(v=>!v))return;
const nums=vals.map(v=>parseFloat(stripB(v))).filter(v=>!isNaN(v));
const spark=nums.length>1?mkSpark(nums):’’;
const lastIdx=vals.map((v,i)=>v?i:-1).filter(i=>i>=0).pop();
const lastAbn=lastIdx>=0&&isAbn(vals[lastIdx]);
h+=`<tr><td style="color:${lastAbn?'#fca5a5':'var(--text2)'};font-weight:500">${key}${lastAbn?' ⚠':''}</td><td>${spark}</td>`;
h+=vals.map(v=>{const a=isAbn(v);return`<td class="${a?'acell':''}">${v?`<span class="${a?'aval':''}">${stripB(v)}${a?’<sup style="color:var(--red)">!</sup>’:’’}</span>`:'—'}</td>`}).join(’’);
h+=`</tr>`;
});
});
h+=`</tbody></table></div><button class="addb mt8" onclick="rmLastLab()">Remove Last Entry</button>`;
return h;}

function mkSpark(vals){
const W=60,H=20,pd=2,mn=Math.min(…vals),mx=Math.max(…vals),rng=mx-mn||1;
const pts=vals.map((v,i)=>(pd+(i/Math.max(vals.length-1,1))*(W-2*pd))+’,’+(pd+(1-(v-mn)/rng)*(H-2*pd)));
const last=vals[vals.length-1],prev=vals[vals.length-2];
const c=prev!==undefined?(last>prev?’#ef4444’:last<prev?’#60a5fa’:’#22c55e’):’#22c55e’;
const lp=pts[pts.length-1].split(’,’);
return`<svg width="${W}" height="${H}" style="display:inline-block;vertical-align:middle"><polyline points="${pts.join(' ')}" fill="none" stroke="#334155" stroke-width="1.5"/><polyline points="${pts.join(' ')}" fill="none" stroke="${c}" stroke-width="1.5" opacity="0.9"/><circle cx="${lp[0]}" cy="${lp[1]}" r="2.5" fill="${c}"/></svg>`;}

// ── VITALS ────────────────────────────────────────────────────────────────────
function rVitals(p){
const vits=p.vitals||[];
if(!vits.length)return`<div style="color:var(--text3);font-size:13px">No vitals entered yet.</div>`;
const sorted=[…vits].sort((a,b)=>a.date.localeCompare(b.date));
let h=`<div style="overflow-x:auto"><table class="labt"><thead><tr><th>Parameter</th>${sorted.map(v=>`<th>${fmtD(v.date)}</th>`).join('')}</tr></thead><tbody>`;
VIT_KEYS.forEach(k=>{
const vals=sorted.map(e=>e[k]||’’);
if(vals.every(v=>!v))return;
h+=`<tr><td style="color:var(--text2);font-weight:500">${VIT_LBL[k]}</td>${vals.map(v=>`<td>${v||’—’}</td>`).join('')}</tr>`;
});
h+=`</tbody></table></div><button class="addb mt8" onclick="rmLastVit()">Remove Last Entry</button>`;
return h;}

// ── OPHTHALMOLOGY ─────────────────────────────────────────────────────────────
function rOphth(p){
const o=p.ophthalmology;
const oct=o.oct||mkInv(‘OCT’),per=o.perimetry||mkInv(‘Perimetry’);
const fu=(o.fundusEntries||[]).map((f,i)=>`<div class="fuentry">
<div style="display:flex;gap:10px;align-items:center;margin-bottom:6px">
<div class="f" style="flex:0 0 auto;margin:0"><label>Date</label><input type="date" value="${esc(f.date)}" onchange="updFu(${i},'date',this.value)" style="width:140px"></div>
<div class="f" style="flex:1;margin:0"><label>Findings</label><textarea rows="2" onchange="updFu(${i},'findings',this.value)">${esc(f.findings)}</textarea></div>
<button class="rmbtn" onclick="rmFu(${i})" style="align-self:flex-end;margin-bottom:4px">✕</button>
</div>

  </div>`).join('');
  return`<div class="g4 mb12">
    <div class="f"><label>VA — Right</label><input type="text" value="${esc(o.va_r)}" onchange="updOp('va_r',this.value)" placeholder="6/6"></div>
    <div class="f"><label>VA — Left</label><input type="text" value="${esc(o.va_l)}" onchange="updOp('va_l',this.value)" placeholder="6/6"></div>
  </div>
  <div class="g2 mb12">
    <div><div style="font-size:10px;font-weight:700;color:var(--text2);text-transform:uppercase;letter-spacing:1px;margin-bottom:6px">OCT</div>${iRow(oct,0,'ophth_oct')}</div>
    <div><div style="font-size:10px;font-weight:700;color:var(--text2);text-transform:uppercase;letter-spacing:1px;margin-bottom:6px">Perimetry</div>${iRow(per,0,'ophth_perimetry')}</div>
  </div>
  <div style="margin-bottom:12px">
    <div style="font-size:10px;font-weight:700;color:var(--text2);text-transform:uppercase;letter-spacing:1px;margin-bottom:8px">Fundus Examinations</div>
    ${fu||'<div style="color:var(--text3);font-size:12px;margin-bottom:8px">None recorded.</div>'}
  </div>
  <div class="f"><label>Notes</label><textarea rows="2" onchange="updOp('notes',this.value)">${esc(o.notes)}</textarea></div>`;}

// ── CONSULTATIONS ─────────────────────────────────────────────────────────────
function rConsults(p){
const h=(p.consultations||[]).map((c,i)=>`<div class="cc ${c.responded?'rsp':'awt'}">
<div style="display:flex;gap:8px;align-items:center;margin-bottom:8px;flex-wrap:wrap">
<input type="text" value="${esc(c.specialty)}" onchange="updCons(${i},'specialty',this.value)" placeholder="Specialty…" style="font-size:14px;font-weight:700;color:var(--accent2);background:transparent;border:none;outline:none;flex:1;min-width:120px;border-bottom:1px solid var(--border2)">
<input type="date" value="${esc(c.dateSent)}" onchange="updCons(${i},'dateSent',this.value)" style="font-size:12px;padding:3px 8px;width:140px">
<input type="text" value="${esc(c.doctor)}" onchange="updCons(${i},'doctor',this.value)" placeholder="Doctor…" style="font-size:12px;padding:3px 8px;width:140px">
<label style="display:flex;align-items:center;gap:5px;font-size:12px;cursor:pointer;color:${c.responded?'var(--green)':'var(--yellow)'}">
<input type=“checkbox” ${c.responded?‘checked’:’’} onchange=“updCons(${i},‘responded’,this.checked)” style=“accent-color:var(–green)”>
${c.responded?‘✓ Responded’:‘⚠ Awaiting’}
</label>
<button class="rmbtn" onclick="rmCons(${i})">✕</button>
</div>
<div class="f"><label>Recommendations</label><textarea rows="3" onchange="updCons(${i},'recommendations',this.value)">${esc(c.recommendations)}</textarea></div>

  </div>`).join('');
  return h||'<div style="color:var(--text3);font-size:12px">No consultations yet.</div>';}

// ── TREATMENT (IVMP + PLEX + IVIG + Antibiotics + Concurrent Meds) ────────────
function rTreatment(p){
const txH=(p.treatments||[]).map((tx,i)=>{
// IVMP
if(tx.type===‘IVMP’){
const d=tx.startDate?dSince(tx.startDate)+1:null,tot=tx.daysTotal||5;
const pct=d?Math.min((d/tot)*100,100):0,done=d&&d>tot;
return`<div class="txcard"><div class="txhdr"> <span class="txname">IVMP — Methylprednisolone 1g IV</span> ${d?`<span class="txday">${done?‘✅ Complete’:‘Day ‘+d+’ of ‘+tot}</span>`:''} <button class="rmbtn" onclick="rmTx(${i})">✕</button> </div> <div style="display:flex;gap:10px;flex-wrap:wrap"> <div class="f"><label>Start Date</label><input type="date" value="${esc(tx.startDate)}" onchange="updTx(${i},'startDate',this.value)" style="width:150px"></div> <div class="f"><label>Total Days</label><input type="number" inputmode="numeric" class="numInput" value="${tot}" onchange="updTxR(${i},'daysTotal',+this.value)" style="width:70px"></div> </div> <div class="pbar"><div class="pfill ${done?'done':''}" style="width:${pct}%"></div></div> <div style="font-size:11px;color:var(--text3);margin-top:4px">${d?(done?'Course complete':tot-d+1+' day(s) remaining'):'Enter start date to track'}</div> </div>`;
}
// IVIG — same auto-counter
if(tx.type===‘IVIG’){
const d=tx.startDate?dSince(tx.startDate)+1:null,tot=tx.daysTotal||5;
const pct=d?Math.min((d/tot)*100,100):0,done=d&&d>tot;
return`<div class="txcard"><div class="txhdr"> <span class="txname">IVIG</span> ${d?`<span class="txday">${done?‘✅ Complete’:‘Day ‘+d+’ of ‘+tot}</span>`:''} <button class="rmbtn" onclick="rmTx(${i})">✕</button> </div> <div style="display:flex;gap:10px;flex-wrap:wrap"> <div class="f"><label>Dose</label><input type="text" value="${esc(tx.dose||'')}" onchange="updTx(${i},'dose',this.value)" placeholder="e.g. 0.4g/kg/day" style="width:180px"></div> <div class="f"><label>Start Date</label><input type="date" value="${esc(tx.startDate)}" onchange="updTx(${i},'startDate',this.value)" style="width:150px"></div> <div class="f"><label>Total Days</label><input type="number" inputmode="numeric" class="numInput" value="${tot}" onchange="updTxR(${i},'daysTotal',+this.value)" style="width:70px"></div> </div> <div class="pbar"><div class="pfill ${done?'done':''}" style="width:${pct}%"></div></div> <div style="font-size:11px;color:var(--text3);margin-top:4px">${d?(done?'Course complete':tot-d+1+' day(s) remaining'):'Enter start date to track'}</div> </div>`;
}
// PLEX
if(tx.type===‘PLEX’){
const freq=tx.frequency===‘daily’?1:2;
const dayNum=tx.startDate?dSince(tx.startDate)+1:null;
const sessNum=dayNum?Math.ceil(dayNum/freq):null;
const tot=tx.sessionsTotal||5;
const curSess=sessNum?Math.min(sessNum,tot):null;
const done=sessNum&&sessNum>tot;
const pct=sessNum?Math.min((sessNum/tot)*100,100):0;
return`<div class="txcard"><div class="txhdr"> <span class="txname">PLEX — Plasma Exchange</span> ${curSess?`<span class="txday">${done?‘✅ Complete’:‘Session ~‘+curSess+’ of ‘+tot+’ · Day ‘+dayNum}</span>`:''} <button class="rmbtn" onclick="rmTx(${i})">✕</button> </div> <div style="display:flex;gap:10px;flex-wrap:wrap;margin-bottom:12px"> <div class="f"><label>Start Date</label><input type="date" value="${esc(tx.startDate)}" onchange="updTx(${i},'startDate',this.value)" style="width:150px"></div> <div class="f"><label>Sessions</label><input type="number" inputmode="numeric" class="numInput" value="${tot}" onchange="updTxR(${i},'sessionsTotal',+this.value)" style="width:70px"></div> <div class="f"><label>Frequency</label><select onchange="updTxR(${i},'frequency',this.value)" style="width:180px"> <option value="every2days" ${tx.frequency!=='daily'?'selected':''}>Every 2 days</option> <option value="daily" ${tx.frequency==='daily'?'selected':''}>Daily</option> </select></div> </div> <div style="font-size:11px;color:var(--text3);margin-bottom:10px">${tot} sessions × ${freq===1?'1':'2'} days = ~${tot*freq} days total</div> <div class="pbar" style="margin-bottom:12px"><div class="pfill ${done?'done':''}" style="width:${pct}%"></div></div> <div style="display:grid;grid-template-columns:1fr 1fr;gap:12px"> <div style="background:var(--surface);border:1px solid ${tx.labsDone?'var(--green)':'var(--red)'};border-radius:8px;padding:12px"> <label style="display:flex;gap:8px;align-items:flex-start;cursor:pointer;font-size:13px;color:${tx.labsDone?'var(--green)':'#fca5a5'}"> <input type="checkbox" ${tx.labsDone?'checked':''} onchange="updTxR(${i},'labsDone',this.checked)" style="accent-color:var(--green);width:16px;height:16px;margin-top:2px;flex-shrink:0"> <div><div style="font-weight:600">Pre-treatment Labs Done</div> <div style="font-size:11px;color:var(--text3);margin-top:2px">CBC · Coag · Ca ionized · K+</div></div> </label> ${!tx.labsDone?`<div style="color:var(--red);font-size:11px;margin-top:8px">🔴 Required before first session</div>`:'<div style="color:var(--green);font-size:11px;margin-top:8px">✅ Confirmed</div>'} </div> <div style="background:var(--surface);border:1px solid ${tx.mahurkarDone?'var(--green)':'var(--red)'};border-radius:8px;padding:12px"> <label style="display:flex;gap:8px;align-items:flex-start;cursor:pointer;font-size:13px;color:${tx.mahurkarDone?'var(--green)':'#fca5a5'}"> <input type="checkbox" ${tx.mahurkarDone?'checked':''} onchange="updTxR(${i},'mahurkarDone',this.checked)" style="accent-color:var(--green);width:16px;height:16px;margin-top:2px;flex-shrink:0"> <div style="font-weight:600">Mahurkar Catheter Inserted</div> </label> ${tx.mahurkarDone?`<div style="display:flex;gap:6px;margin-top:8px;flex-wrap:wrap">
<input type="date" value="${esc(tx.mahurkarDate||'')}" onchange="updTx(${i},'mahurkarDate',this.value)" style="font-size:11px;padding:3px 6px;width:140px">
<select onchange="updTx(${i},'mahurkarSide',this.value)" style="font-size:11px;padding:3px 6px;width:90px">
<option ${!tx.mahurkarSide||tx.mahurkarSide===‘Right’?‘selected’:’’}>Right</option>
<option ${tx.mahurkarSide===‘Left’?‘selected’:’’}>Left</option>
</select>
<input type="text" value="${esc(tx.mahurkarSite||'')}" placeholder="Site e.g. Femoral" onchange="updTx(${i},'mahurkarSite',this.value)" style="font-size:11px;padding:3px 6px;flex:1;min-width:100px">
</div>`:`<div style="color:var(--red);font-size:11px;margin-top:8px">🔴 Required before sessions</div>`} </div> </div> ${dayNum&&!done?`<div style="margin-top:10px;background:var(--surface);border:1px solid var(--border2);border-radius:8px;padding:10px;font-size:12px;color:var(--text2)">📅 Started ${fmtD(tx.startDate)} · Day ${dayNum} → ~Session ${curSess} of ${tot}${tot-curSess>0?’ · ~‘+(tot-curSess)+’ remaining’:’ · Final reached’}</div>`:''} ${done?`<div style="margin-top:10px;background:#052210;border:1px solid var(--green);border-radius:8px;padding:10px;font-size:12px;color:var(--green);font-weight:700;text-align:center">✅ PLEX complete — ${tot} sessions</div>`:''} </div>`;
}
return’’;
}).join(’’);

// Antibiotics
const abH=(p.antibiotics||[]).map((ab,i)=>{
const exp=ab.startDate&&ab.days?(()=>{const d=new Date(ab.startDate);d.setDate(d.getDate()+ +ab.days);return d.toISOString().split(‘T’)[0]})():null;
const diff=exp?Math.floor((new Date(exp)-Date.now())/86400000):null;
const expired=diff!==null&&diff<0,expiring=diff===0;
return`<div class="abcard ${expired?'exp':expiring?'exp2':''}"> <div style="display:grid;grid-template-columns:1fr 140px 100px 110px auto;gap:8px;align-items:end"> <div class="f" style="margin:0"><label>Antibiotic</label><input type="text" value="${esc(ab.drug)}" onchange="updAb(${i},'drug',this.value)"></div> <div class="f" style="margin:0"><label>Start Date</label><input type="date" value="${esc(ab.startDate)}" onchange="updAbR(${i},'startDate',this.value)"></div> <div class="f" style="margin:0"><label>Days Approved</label><input type="number" inputmode="numeric" class="numInput" value="${esc(ab.days)}" onchange="updAbR(${i},'days',this.value)"></div> <div class="f" style="margin:0"><label>Expires</label><input readonly value="${exp||'—'}" style="font-weight:700;color:${expired?'#ef4444':expiring?'#f59e0b':'#22c55e'}"></div> <button class="rmbtn" onclick="rmAb(${i})" style="margin-bottom:2px">✕</button> </div> <div class="f mt8"><label>Indication</label><input type="text" value="${esc(ab.indication||'')}" onchange="updAb(${i},'indication',this.value)"></div> ${expired?'<div style="color:var(--red);font-size:12px;font-weight:700;margin-top:6px">🔴 EXPIRED — Renewal required</div>':''} </div>`;
}).join(’’);

// Concurrent Medications
const medH=(p.concurrentMeds||[]).map((m,i)=>{
const st=MED_STATUS[m.status||‘active’];
const stopped=m.status===‘stopped’;
return`<div class="medcard ${m.status==='stopped'?'stopped':m.status==='onhold'?'onhold':''}"> <div style="display:grid;grid-template-columns:1.5fr 1fr 130px 130px 110px auto;gap:8px;align-items:end"> <div class="f" style="margin:0"><label>Medication Name</label> <input type="text" value="${esc(m.name)}" onchange="updMed(${i},'name',this.value)" style="${stopped?'text-decoration:line-through;opacity:0.6':''}"> </div> <div class="f" style="margin:0"><label>Dose / Frequency</label> <input type="text" value="${esc(m.dose||'')}" onchange="updMed(${i},'dose',this.value)"> </div> <div class="f" style="margin:0"><label>Start Date</label> <input type="date" value="${esc(m.startDate||'')}" onchange="updMed(${i},'startDate',this.value)"> </div> <div class="f" style="margin:0"><label>Stop Date</label> <input type="date" value="${esc(m.stopDate||'')}" onchange="updMedR(${i},'stopDate',this.value)"> </div> <div class="f" style="margin:0"><label>Status</label> <select onchange="updMedR(${i},'status',this.value)" style="color:${st.color};background:${st.bg};font-weight:600"> <option value="active" ${m.status==='active'||!m.status?'selected':''}>Active</option> <option value="onhold" ${m.status==='onhold'?'selected':''}>On Hold</option> <option value="stopped" ${m.status==='stopped'?'selected':''}>Stopped</option> </select> </div> <button class="rmbtn" onclick="rmMed(${i})" style="margin-bottom:2px">✕</button> </div> </div>`;
}).join(’’);

return`${txH} <div style="border-top:1px solid var(--border);padding-top:14px;margin-top:4px"> <div style="font-size:10px;font-weight:700;color:var(--text2);text-transform:uppercase;letter-spacing:1px;margin-bottom:10px">Antibiotics</div> ${abH||'<div style="color:var(--text3);font-size:12px;margin-bottom:8px">None.</div>'} </div> <div style="border-top:1px solid var(--border);padding-top:14px;margin-top:4px"> <div style="font-size:10px;font-weight:700;color:var(--text2);text-transform:uppercase;letter-spacing:1px;margin-bottom:10px">Concurrent Medications</div> ${medH||'<div style="color:var(--text3);font-size:12px;margin-bottom:8px">None added.</div>'} </div>`;}

// ── DEVICES ───────────────────────────────────────────────────────────────────
function rDevices(p){
const h=(p.devices||[]).map((d,i)=>{
const days=dSince(d.insertionDate),old=days!==null&&days>7;
return`<div class="devrow"> <div class="f" style="margin:0"><label>Device</label><select onchange="updDev(${i},'type',this.value)">${DEV_T.map(t=>`<option ${d.type===t?‘selected’:’’}>${t}</option>`).join('')}</select></div> <div class="f" style="margin:0"><label>Insertion Date</label><input type="date" value="${esc(d.insertionDate)}" onchange="updDevR(${i},'insertionDate',this.value)"></div> <div class="f" style="margin:0"><label>Site / Side</label><input type="text" value="${esc(d.site||'')}" onchange="updDev(${i},'site',this.value)" placeholder="e.g. Right femoral"></div> <div class="f" style="margin:0"><label>Condition</label><input type="text" value="${esc(d.condition||'')}" onchange="updDev(${i},'condition',this.value)"></div> <div class="f" style="margin:0"><label>Removal Date</label><input type="date" value="${esc(d.removalDate||'')}" onchange="updDevR(${i},'removalDate',this.value)"></div> <div style="display:flex;flex-direction:column;align-items:flex-end;gap:4px;padding-bottom:2px"> ${days!==null?`<span style="font-size:11px;font-weight:700;color:${old?'var(--red)':'var(--green)'}">${old?‘⚠’:‘✓’} Day ${days+1}</span>`:''} <button class="rmbtn" onclick="rmDev(${i})">✕</button> </div> </div>`;
}).join(’’);
return h||’<div style="color:var(--text3);font-size:12px">No devices recorded.</div>’;}

// ── HOSPITAL COURSE ───────────────────────────────────────────────────────────
function rCourse(p){
const ev=[…(p.hospitalCourse||[])].sort((a,b)=>b.date.localeCompare(a.date));
const h=ev.map(e=>{
const oi=(p.hospitalCourse||[]).findIndex(x=>x===e||(x.date===e.date&&x.text===e.text));
return`<div class="evcard"> <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:6px"> <div style="font-family:var(--mono);font-size:11px;color:var(--accent2)">${fmtD(e.date)} — ${e.date}</div> <button class="rmbtn" onclick="rmEv(${oi})">✕</button> </div> <textarea rows="3" onchange="updEv(${oi},this.value)" style="background:transparent;border:1px solid var(--border);border-radius:6px;padding:6px 10px;width:100%;font-size:13px;color:var(--text);resize:vertical;line-height:1.6">${esc(e.text)}</textarea> </div>`;
}).join(’’);
return h||’<div style="color:var(--text3);font-size:12px">No events recorded yet.</div>’;}

// ── UPDATE FUNCTIONS ──────────────────────────────────────────────────────────
function updF(k,v){upd(p=>p[k]=v);saveD();renderList()}
function updExam(k,v){upd(p=>p.exam[k]=v)}
function updOp(k,v){upd(p=>p.ophthalmology[k]=v)}
function updFu(i,k,v){upd(p=>p.ophthalmology.fundusEntries[i][k]=v)}
function addFundus(){upd(p=>{p.ophthalmology.fundusEntries=p.ophthalmology.fundusEntries||[];p.ophthalmology.fundusEntries.push({date:td(),findings:’’})});renderPt()}
function rmFu(i){upd(p=>p.ophthalmology.fundusEntries.splice(i,1));renderPt()}

function updInv(lt,id,key,val){
upd(p=>{
if(lt===‘ophth_oct’){p.ophthalmology.oct[key]=val;return}
if(lt===‘ophth_perimetry’){p.ophthalmology.perimetry[key]=val;return}
const arr=p[lt]||[];const item=arr.find(x=>x.id===id);if(item)item[key]=val;
});
if(key===‘status’||key===‘result’)renderPt();else{saveD();renderList()}
}
function cycSt(lt,id){
upd(p=>{
let arr;
if(lt===‘ophth_oct’)arr=[p.ophthalmology.oct];
else if(lt===‘ophth_perimetry’)arr=[p.ophthalmology.perimetry];
else arr=p[lt]||[];
const item=arr.find(x=>x.id===id);if(!item)return;
const idx=ST_CYC.indexOf(item.status);item.status=ST_CYC[(idx+1)%ST_CYC.length];
});renderPt();renderList();
}
function movInv(lt,id,dir){upd(p=>{const arr=p[lt];if(!arr)return;const i=arr.findIndex(x=>x.id===id),ni=i+dir;if(ni<0||ni>=arr.length)return;[arr[i],arr[ni]]=[arr[ni],arr[i]]});renderPt()}
function rmInv(lt,id){upd(p=>{const arr=p[lt];if(arr){const i=arr.findIndex(x=>x.id===id);if(i>=0)arr.splice(i,1)}});renderPt()}
function addSL(){upd(p=>p.specificLabs.push(mkInv(‘New Lab’)));renderPt()}
function addOL(){upd(p=>{p.otherLabs=p.otherLabs||[];p.otherLabs.push(mkInv(‘New Lab’))});renderPt()}
function addImg(){upd(p=>p.imaging.push(mkInv(‘New Study’)));renderPt()}
function addNP(){upd(p=>p.npStudies.push(mkInv(‘New NP Study’)));renderPt()}

function updCons(i,k,v){upd(p=>p.consultations[i][k]=v);if(k===‘responded’)renderPt();else{saveD();renderList()}}
function rmCons(i){upd(p=>p.consultations.splice(i,1));renderPt()}
function addConsult(){upd(p=>{p.consultations=p.consultations||[];p.consultations.push({specialty:’’,dateSent:td(),responded:false,doctor:’’,recommendations:’’})});renderPt()}

function addTx(type){
upd(p=>{
p.treatments=p.treatments||[];
if(type===‘IVMP’)p.treatments.push({type:‘IVMP’,startDate:’’,daysTotal:5});
else if(type===‘PLEX’)p.treatments.push({type:‘PLEX’,startDate:’’,sessionsTotal:5,frequency:‘every2days’,labsDone:false,mahurkarDone:false,mahurkarDate:’’,mahurkarSide:‘Right’,mahurkarSite:’’});
else if(type===‘IVIG’)p.treatments.push({type:‘IVIG’,startDate:’’,daysTotal:5,dose:‘0.4g/kg/day’});
});renderPt();
}
function rmTx(i){upd(p=>p.treatments.splice(i,1));renderPt()}
function updTx(i,k,v){upd(p=>p.treatments[i][k]=v)}
function updTxR(i,k,v){upd(p=>p.treatments[i][k]=v);renderPt()}

function addAb(){upd(p=>{p.antibiotics=p.antibiotics||[];p.antibiotics.push({drug:’’,startDate:td(),days:’’,indication:’’})});renderPt()}
function rmAb(i){upd(p=>p.antibiotics.splice(i,1));renderPt()}
function updAb(i,k,v){upd(p=>p.antibiotics[i][k]=v)}
function updAbR(i,k,v){upd(p=>p.antibiotics[i][k]=v);renderPt()}

function addMed(){upd(p=>{p.concurrentMeds=p.concurrentMeds||[];p.concurrentMeds.push({name:’’,dose:’’,startDate:td(),stopDate:’’,status:‘active’})});renderPt()}
function rmMed(i){upd(p=>p.concurrentMeds.splice(i,1));renderPt()}
function updMed(i,k,v){upd(p=>p.concurrentMeds[i][k]=v)}
function updMedR(i,k,v){upd(p=>p.concurrentMeds[i][k]=v);renderPt()}

function addDev(){upd(p=>{p.devices=p.devices||[];p.devices.push({type:‘Mahurkar Catheter’,insertionDate:td(),site:’’,condition:’’,removalDate:’’})});renderPt()}
function rmDev(i){upd(p=>p.devices.splice(i,1));renderPt()}
function updDev(i,k,v){upd(p=>p.devices[i][k]=v)}
function updDevR(i,k,v){upd(p=>p.devices[i][k]=v);renderPt()}

function rmEv(i){upd(p=>p.hospitalCourse.splice(i,1));renderPt()}
function updEv(i,v){upd(p=>p.hospitalCourse[i].text=v)}
function rmLastLab(){upd(p=>p.routineLabs.pop());renderPt()}
function rmLastVit(){upd(p=>p.vitals.pop());renderPt()}

// ── MODALS ────────────────────────────────────────────────────────────────────
function closeM(id){document.getElementById(id).style.display=‘none’}
function openM(id){document.getElementById(id).style.display=‘flex’}

function openAdd(tplKey){
document.getElementById(‘addtit’).textContent=tplKey?‘➕ Admit: ‘+TEMPLATES[tplKey].name:‘➕ Admit New Patient’;
document.getElementById(‘addfot’).innerHTML=`<button class="btn btng" onclick="closeM('addmod')">Cancel</button><button class="btn btnp" onclick="admitPt(${tplKey?`’${tplKey}’`:''})">Admit</button>`;
[‘n-name’,‘n-nar’,‘n-occ’,‘n-res’,‘n-ref’,‘n-ddx’].forEach(id=>{const e=document.getElementById(id);if(e)e.value=’’});
document.getElementById(‘n-diag’).value=tplKey&&TEMPLATES[tplKey]?TEMPLATES[tplKey].diagnosis:’’;
document.getElementById(‘n-doa’).value=td();
document.getElementById(‘n-dos’).value=’’;
document.getElementById(‘n-age’).value=’’;
document.getElementById(‘n-sex’).value=‘M’;
document.getElementById(‘n-hand’).value=‘Right’;
document.getElementById(‘n-mar’).value=‘Married’;
closeM(‘tplmod’);openM(‘addmod’);
}

function openTplModal(){
document.getElementById(‘tpl-grid’).innerHTML=Object.entries(TEMPLATES).map(([k,T])=>` <button class="tpl-btn" onclick="openAdd('${k}')"> <div class="tpl-name">${T.icon} ${T.name}</div> <div class="tpl-sub">${(T.specOrdered||[]).slice(0,2).join(' · ')}${T.imgOrdered?.length?' · '+T.imgOrdered[0]:''}…</div> </button>`).join(’’);
openM(‘tplmod’);
}

function admitPt(tplKey){
const name=document.getElementById(‘n-name’).value.trim();
if(!name){alert(‘Patient name required’);return}
const p=newPt({
name,nameAr:document.getElementById(‘n-nar’).value,age:document.getElementById(‘n-age’).value,
sex:document.getElementById(‘n-sex’).value,doa:document.getElementById(‘n-doa’).value,
dos:document.getElementById(‘n-dos’).value,diagnosis:document.getElementById(‘n-diag’).value,
ddx:document.getElementById(‘n-ddx’).value,ref:document.getElementById(‘n-ref’).value,
handedness:document.getElementById(‘n-hand’).value,residence:document.getElementById(‘n-res’).value,
occupation:document.getElementById(‘n-occ’).value,marital:document.getElementById(‘n-mar’).value
},tplKey);
patients.unshift(p);saveD();closeM(‘addmod’);selPt(p.id);
}

function editInfo(){
const p=getP();if(!p)return;
document.getElementById(‘n-name’).value=p.name;document.getElementById(‘n-nar’).value=p.nameAr||’’;
document.getElementById(‘n-age’).value=p.age;document.getElementById(‘n-sex’).value=p.sex;
document.getElementById(‘n-doa’).value=p.doa;document.getElementById(‘n-dos’).value=p.dos;
document.getElementById(‘n-diag’).value=p.diagnosis;document.getElementById(‘n-ddx’).value=p.ddx;
document.getElementById(‘n-ref’).value=p.referringStaff;document.getElementById(‘n-hand’).value=p.handedness;
document.getElementById(‘n-res’).value=p.residence;document.getElementById(‘n-occ’).value=p.occupation;
document.getElementById(‘n-mar’).value=p.marital;
document.getElementById(‘addtit’).textContent=‘✏️ Edit Patient Info’;
document.getElementById(‘addfot’).innerHTML=`<button class="btn btng" onclick="closeM('addmod')">Cancel</button><button class="btn btnp" onclick="saveEdit()">Save</button>`;
openM(‘addmod’);
}
function saveEdit(){
upd(p=>{
p.name=document.getElementById(‘n-name’).value;p.nameAr=document.getElementById(‘n-nar’).value;
p.age=document.getElementById(‘n-age’).value;p.sex=document.getElementById(‘n-sex’).value;
p.doa=document.getElementById(‘n-doa’).value;p.dos=document.getElementById(‘n-dos’).value;
p.diagnosis=document.getElementById(‘n-diag’).value;p.ddx=document.getElementById(‘n-ddx’).value;
p.referringStaff=document.getElementById(‘n-ref’).value;p.handedness=document.getElementById(‘n-hand’).value;
p.residence=document.getElementById(‘n-res’).value;p.occupation=document.getElementById(‘n-occ’).value;
p.marital=document.getElementById(‘n-mar’).value;
});closeM(‘addmod’);renderPt();renderList();
}
function delPt(){
if(!confirm(‘Remove this patient?’))return;
patients=patients.filter(p=>p.id!==currentId);saveD();currentId=null;
document.getElementById(‘pview’).style.display=‘none’;
document.getElementById(‘empty’).style.display=‘flex’;
renderList();
}

function openLabM(){document.getElementById(‘lab-date’).value=td();LAB_KEYS.forEach(k=>{const e=document.getElementById(‘lab-’+k);if(e)e.value=’’});openM(‘labmod’)}
function saveLabs(){
const date=document.getElementById(‘lab-date’).value||td(),values={};
LAB_KEYS.forEach(k=>{const e=document.getElementById(‘lab-’+k);if(e&&e.value)values[k]=e.value});
if(!Object.keys(values).length){alert(‘Enter at least one value’);return}
upd(p=>{p.routineLabs=p.routineLabs||[];p.routineLabs.push({date,values});p.routineLabs.sort((a,b)=>a.date.localeCompare(b.date))});
closeM(‘labmod’);renderPt();
}
function openVitM(){document.getElementById(‘vit-date’).value=td();VIT_KEYS.forEach(k=>{const e=document.getElementById(‘vit-’+k);if(e)e.value=’’});openM(‘vitmod’)}
function saveVitals(){
const date=document.getElementById(‘vit-date’).value||td();
const entry={date};let any=false;
VIT_KEYS.forEach(k=>{const e=document.getElementById(‘vit-’+k);if(e&&e.value){entry[k]=e.value;any=true}});
if(!any){alert(‘Enter at least one value’);return}
upd(p=>{p.vitals=p.vitals||[];p.vitals.push(entry);p.vitals.sort((a,b)=>a.date.localeCompare(b.date))});
closeM(‘vitmod’);renderPt();
}
function openEvM(){document.getElementById(‘ev-date’).value=td();document.getElementById(‘ev-text’).value=’’;openM(‘evmod’)}
function saveEv(){
const date=document.getElementById(‘ev-date’).value||td(),text=document.getElementById(‘ev-text’).value.trim();
if(!text){alert(‘Enter event description’);return}
upd(p=>{p.hospitalCourse=p.hospitalCourse||[];p.hospitalCourse.push({date,text})});
closeM(‘evmod’);renderPt();
}

// ── SCAN ──────────────────────────────────────────────────────────────────────
function openScan(){
resetScan();
document.querySelectorAll(’.scan-mbtn’).forEach((b,i)=>{b.style.borderColor=i===0?‘var(–accent2)’:‘var(–border2)’});
activeScanMode=‘admission’;
document.getElementById(‘net-warn-scan’).style.display=isFileProto()?‘block’:‘none’;
openM(‘scanmod’);
}
function resetScan(){
scanImgData=null;scannedResult=null;
document.getElementById(‘scan-preview’).style.display=‘none’;
document.getElementById(‘scan-result’).style.display=‘none’;
document.getElementById(‘scan-err’).style.display=‘none’;
document.getElementById(‘scan-drop’).style.display=‘block’;
document.getElementById(‘scan-go-btn’).textContent=‘🔍 Extract Data’;
document.getElementById(‘scan-go-btn’).disabled=false;
document.getElementById(‘scan-file’).value=’’;
}
document.querySelectorAll(’.scan-mbtn’).forEach(b=>b.addEventListener(‘click’,()=>{
activeScanMode=b.dataset.mode;
document.querySelectorAll(’.scan-mbtn’).forEach(x=>x.style.borderColor=‘var(–border2)’);
b.style.borderColor=‘var(–accent2)’;
}));
function handleScanFile(input){
const f=input.files[0];if(!f)return;
const reader=new FileReader();
reader.onload=ev=>{
scanImgData=ev.target.result.split(’,’)[1];
document.getElementById(‘scan-img’).src=ev.target.result;
document.getElementById(‘scan-drop’).style.display=‘none’;
document.getElementById(‘scan-preview’).style.display=‘block’;
document.getElementById(‘scan-result’).style.display=‘none’;
document.getElementById(‘scan-err’).style.display=‘none’;
};reader.readAsDataURL(f);
}
const SCAN_PROMPTS={
admission:`Extract ALL info from this neurology admission sheet (Arabic/English/handwritten). Return ONLY valid JSON: {"name":"","nameAr":"","age":"","sex":"","occupation":"","marital":"","residence":"","handedness":"","dos":"YYYY-MM-DD or empty","doa":"YYYY-MM-DD or empty","diagnosis":"","ddx":"","referringStaff":"","complaint":"","hpi":"","pastHistory":"","personalHistory":"","familyHistory":"","exam_general":"","exam_cn":"","exam_tone":"","exam_RUL":"","exam_LUL":"","exam_RLL":"","exam_LLL":"","exam_reflexesSup":"","exam_reflexesDeep":"","exam_plantar":"","exam_sensSup":"","exam_sensDeep":"","exam_coordination":"","exam_gait":"","exam_cognition":"","uncertain_fields":[]}`,
labs:`Extract ALL lab values from this result printout. Return ONLY valid JSON: {"date":"YYYY-MM-DD","values":{"Hb":"","WBC":"","PLT":"","INR":"","PTT":"","Fibrinogen":"","Na":"","K":"","Creatinine":"","Urea":"","ALT":"","AST":"","Albumin":"","CRP":"","Ca":"","Mg":"","Bilirubin":"","ALP":""},"specific":{"AntAQP4":"","Anti-MOG":"","Virology":"","Quantiferon":"","CSF_OP":"","CSF_Cells":"","CSF_Protein":"","CSF_Glucose":"","CSF_OCB":"","CSF_IgG":"","TumorMarkers":"","ThyroidProfile":"","VasculiticProfile":"","ThrombophiliaProfile":""},"uncertain_fields":[],"raw_text":""}`,
radiology:`Extract this radiology report. Return ONLY valid JSON: {"study_type":"","date":"YYYY-MM-DD or empty","result":"full report text","impression":"impression section","uncertain_fields":[]}`,
consult:`Extract this consultant reply. Return ONLY valid JSON: {"specialty":"","date":"YYYY-MM-DD or empty","doctor":"","recommendations":"full recommendations text","uncertain_fields":[]}`
};
async function runScan(){
if(!scanImgData)return;
if(isFileProto()){document.getElementById(‘scan-err’).style.display=‘block’;document.getElementById(‘scan-err’).textContent=‘Cannot connect via file://. See the warning above for how to fix this.’;return;}
const btn=document.getElementById(‘scan-go-btn’);btn.textContent=‘⏳ Reading…’;btn.disabled=true;
document.getElementById(‘scan-err’).style.display=‘none’;
try{
const res=await fetch(‘https://api.anthropic.com/v1/messages’,{method:‘POST’,headers:{‘Content-Type’:‘application/json’},body:JSON.stringify({model:‘claude-sonnet-4-20250514’,max_tokens:2000,messages:[{role:‘user’,content:[{type:‘image’,source:{type:‘base64’,media_type:‘image/jpeg’,data:scanImgData}},{type:‘text’,text:SCAN_PROMPTS[activeScanMode]}]}]})});
const data=await res.json();
const text=data.content?.find(b=>b.type===‘text’)?.text||’’;
scannedResult=JSON.parse(text.replace(/`json|`/g,’’).trim());
showScanResult(scannedResult);
}catch(e){
document.getElementById(‘scan-err’).style.display=‘block’;
document.getElementById(‘scan-err’).textContent=‘Could not read document. Try a clearer photo, or check your connection.’;
btn.textContent=‘🔍 Extract Data’;btn.disabled=false;
}
}
function showScanResult(data){
const unc=document.getElementById(‘scan-unc’);
if(data.uncertain_fields?.length){unc.style.display=‘block’;unc.textContent=‘⚠ Verify: ‘+data.uncertain_fields.join(’, ‘);}
else unc.style.display=‘none’;
const prev=document.getElementById(‘scan-prev-data’);
if(activeScanMode===‘admission’){
prev.innerHTML=’<table style="width:100%;border-collapse:collapse;font-size:12px">’+
Object.entries(data).filter(([k,v])=>k!==‘uncertain_fields’&&v).map(([k,v])=>
`<tr style="border-bottom:1px solid var(--border)"><td style="padding:3px 8px;color:var(--green);white-space:nowrap">${k.replace(/_/g,' ')}</td><td style="padding:3px 8px;color:var(--text)">${esc(String(v))}</td></tr>`
).join(’’)+’</table>’;
}else if(activeScanMode===‘labs’){
const all={…data.values,…data.specific};
prev.innerHTML=’<div style="color:var(--text2);font-size:11px;margin-bottom:6px">Date: ‘+data.date+’</div><div style="display:flex;flex-wrap:wrap;gap:6px">’+
Object.entries(all).filter(([,v])=>v).map(([k,v])=>`<span style="background:var(--s-res);color:var(--sc-res);border-radius:4px;padding:2px 8px;font-size:11px"><b>${k}:</b> ${esc(v)}</span>`).join(’’)+’</div>’;
if(data.raw_text)prev.innerHTML+=`<div style="color:var(--text3);font-size:11px;margin-top:8px">Other: ${esc(data.raw_text)}</div>`;
}else{
prev.innerHTML=Object.entries(data).filter(([k,v])=>k!==‘uncertain_fields’&&v).map(([k,v])=>
`<div style="margin-bottom:6px"><span style="color:var(--green);font-size:11px">${k}: </span><span style="color:var(--text);font-size:12px">${esc(String(v))}</span></div>`).join(’’);
}
document.getElementById(‘scan-result’).style.display=‘block’;
}
function applyScan(){
const p=getP();if(!p||!scannedResult)return;
const d=scannedResult;
if(activeScanMode===‘admission’){
upd(pt=>{
const fields={name:‘name’,nameAr:‘nameAr’,age:‘age’,sex:‘sex’,occupation:‘occupation’,marital:‘marital’,residence:‘residence’,handedness:‘handedness’,dos:‘dos’,doa:‘doa’,diagnosis:‘diagnosis’,ddx:‘ddx’,referringStaff:‘referringStaff’,complaint:‘complaint’,hpi:‘hpi’,pastHistory:‘pastHistory’,personalHistory:‘personalHistory’,familyHistory:‘familyHistory’};
Object.entries(fields).forEach(([df,pf])=>{if(d[df])pt[pf]=d[df]});
pt.exam=pt.exam||{};
const ef={exam_general:‘general’,exam_cn:‘cn’,exam_tone:‘tone’,exam_RUL:‘RUL’,exam_LUL:‘LUL’,exam_RLL:‘RLL’,exam_LLL:‘LLL’,exam_reflexesSup:‘reflexesSup’,exam_reflexesDeep:‘reflexesDeep’,exam_plantar:‘plantar’,exam_sensSup:‘sensSup’,exam_sensDeep:‘sensDeep’,exam_coordination:‘coordination’,exam_gait:‘gait’,exam_cognition:‘cognition’};
Object.entries(ef).forEach(([df,ef2])=>{if(d[df])pt.exam[ef2]=d[df]});
});
}else if(activeScanMode===‘labs’){
upd(pt=>{
const entry={date:d.date||td(),values:{}};
Object.entries(d.values||{}).forEach(([k,v])=>{if(v)entry.values[k]=v});
pt.routineLabs=pt.routineLabs||[];pt.routineLabs.push(entry);pt.routineLabs.sort((a,b)=>a.date.localeCompare(b.date));
Object.entries(d.specific||{}).forEach(([k,v])=>{
if(!v)return;
const idx=(pt.specificLabs||[]).findIndex(s=>s.name.toLowerCase().includes(k.toLowerCase()));
if(idx>-1){pt.specificLabs[idx].result=v;pt.specificLabs[idx].status=‘resulted’;pt.specificLabs[idx].doneDate=d.date||td();}
});
});
}else if(activeScanMode===‘radiology’){
upd(pt=>{
const idx=(pt.imaging||[]).findIndex(i=>i.name.toLowerCase().includes((d.study_type||’’).toLowerCase()));
const entry={name:d.study_type||‘Imaging Study’,status:‘resulted’,doneDate:d.date||td(),result:(d.result||’’)+(d.impression?’\nImpression: ‘+d.impression:’’)};
if(idx>-1)Object.assign(pt.imaging[idx],entry);else pt.imaging.push({…mkInv(entry.name),…entry});
});
}else if(activeScanMode===‘consult’){
upd(pt=>{pt.consultations=pt.consultations||[];pt.consultations.push({specialty:d.specialty||’’,doctor:d.doctor||’’,dateSent:d.date||td(),responded:true,recommendations:d.recommendations||’’})});
}
closeM(‘scanmod’);renderPt();renderList();
}

// ── AI SUMMARY ────────────────────────────────────────────────────────────────
async function genAISummary(){
const p=getP();if(!p)return;
document.getElementById(‘net-warn-ai’).style.display=isFileProto()?‘block’:‘none’;
document.getElementById(‘ai-loading’).style.display=isFileProto()?‘none’:‘block’;
document.getElementById(‘ai-result’).style.display=‘none’;
openM(‘aimod’);
if(isFileProto())return;
const prompt=`You are a senior neurology consultant. Write a structured clinical summary for handover/discharge based on this data. Be concise and clinical.

Patient: ${p.name}, ${p.age}y ${p.sex}, DOA: ${p.doa} (${admDay(p.doa)})
Diagnosis: ${p.diagnosis||‘Not established’} | DDx: ${p.ddx||’—’}
Referring: ${p.referringStaff||’—’}

Complaint: ${p.complaint||’—’}
HPI: ${p.hpi||’—’}
Past Hx: ${p.pastHistory||’—’} | Personal: ${p.personalHistory||’—’} | Family: ${p.familyHistory||’—’}

Exam:
General: ${p.exam?.general||’—’} | CN: ${p.exam?.cn||’—’}
Tone: ${p.exam?.tone||’—’}
Power: RUL ${p.exam?.RUL||’—’} LUL ${p.exam?.LUL||’—’} RLL ${p.exam?.RLL||’—’} LLL ${p.exam?.LLL||’—’}
Reflexes: Sup ${p.exam?.reflexesSup||’—’} Deep ${p.exam?.reflexesDeep||’—’} Plantar ${p.exam?.plantar||’—’}
Sensory: Sup ${p.exam?.sensSup||’—’} Deep ${p.exam?.sensDeep||’—’}
Coordination: ${p.exam?.coordination||’—’} | Gait: ${p.exam?.gait||’—’}
Cognition: ${p.exam?.cognition||’—’}

Key Investigations:
${[…(p.specificLabs||[]),…(p.otherLabs||[])].filter(l=>l.result).map(l=>l.name+’: ‘+l.result).join(’\n’)||‘Pending’}
Imaging: ${(p.imaging||[]).filter(i=>i.result).map(i=>i.name+’: ‘+i.result).join(’ | ‘)||‘Pending’}
NP: ${(p.npStudies||[]).filter(n=>n.result).map(n=>n.name+’: ‘+n.result).join(’ | ’)||‘Pending’}
Latest labs: ${(p.routineLabs||[]).length?JSON.stringify((p.routineLabs||[]).slice(-1)[0]?.values||{}):‘None’}

Treatment: ${(p.treatments||[]).map(t=>t.type+(t.startDate?’ from ‘+t.startDate:’’)).join(’, ‘)||‘None’}
Concurrent meds: ${(p.concurrentMeds||[]).map(m=>m.name+’ ‘+m.dose+’ (’+m.status+’)’).join(’, ‘)||‘None’}
Hospital course: ${(p.hospitalCourse||[]).map(e=>e.date+’: ‘+e.text).join(’\n’)||‘None recorded’}

Write sections: Presenting Complaint, History, Examination, Investigations, Hospital Course, Treatment, Plan.`;

try{
const res=await fetch(‘https://api.anthropic.com/v1/messages’,{method:‘POST’,headers:{‘Content-Type’:‘application/json’},body:JSON.stringify({model:‘claude-sonnet-4-20250514’,max_tokens:1500,messages:[{role:‘user’,content:prompt}]})});
const data=await res.json();
const text=data.content?.find(b=>b.type===‘text’)?.text||‘Failed to generate.’;
document.getElementById(‘ai-text’).textContent=text;
document.getElementById(‘ai-loading’).style.display=‘none’;
document.getElementById(‘ai-result’).style.display=‘block’;
}catch(e){
document.getElementById(‘ai-text’).textContent=‘Error. Check your connection.’;
document.getElementById(‘ai-loading’).style.display=‘none’;
document.getElementById(‘ai-result’).style.display=‘block’;
}
}
function copyAI(){navigator.clipboard.writeText(document.getElementById(‘ai-text’).textContent).then(()=>alert(‘Copied!’))}

// ── PRINT CURRENT PATIENT ─────────────────────────────────────────────────────
function printPt(){
const p=getP();if(!p){alert(‘Select a patient first’);return}
const w=window.open(’’,’_blank’);
const now=new Date().toLocaleDateString(‘en-GB’,{weekday:‘long’,day:‘numeric’,month:‘long’,year:‘numeric’});
const alerts=getAlerts(p);
const latestLabs=(p.routineLabs||[]).slice(-1)[0];
const latestVits=(p.vitals||[]).slice(-1)[0];

const secPrint=(title,content)=>`<div class="psec"><div class="psec-title">${title}</div><div class="psec-content">${content}</div></div>`;

const invTable=(list,title)=>{
const active=list.filter(l=>l.status!==‘not-ordered’);
if(!active.length)return’’;
return`<div style="margin-bottom:8px"><b>${title}</b><table style="width:100%;border-collapse:collapse;font-size:11px;margin-top:4px"><tr><th style="text-align:left;padding:3px 6px;background:#f0f4ff;border:1px solid #ddd">Test</th><th style="text-align:left;padding:3px 6px;background:#f0f4ff;border:1px solid #ddd">Status</th><th style="text-align:left;padding:3px 6px;background:#f0f4ff;border:1px solid #ddd">Ordered</th><th style="text-align:left;padding:3px 6px;background:#f0f4ff;border:1px solid #ddd">Result</th></tr>`+
active.map(l=>`<tr><td style="padding:3px 6px;border:1px solid #eee">${l.name}</td><td style="padding:3px 6px;border:1px solid #eee;color:${l.status==='resulted'?'#166534':l.status==='done-pending-result'?'#0f766e':l.status==='pending'?'#5b21b6':'#854d0e'};font-weight:600">${ST_LBL[l.status]||l.status}</td><td style="padding:3px 6px;border:1px solid #eee">${fmtD(l.orderedDate)}</td><td style="padding:3px 6px;border:1px solid #eee">${l.result||'—'}</td></tr>`).join(’’)+
‘</table></div>’;
};

const labsHtml=latestLabs?Object.entries(LAB_G).map(([grp,keys])=>{
const relevant=keys.filter(k=>latestLabs.values?.[k]);
if(!relevant.length)return’’;
return`<b>${grp}:</b> `+relevant.map(k=>{const v=latestLabs.values[k];const a=isAbn(v);return`<span ${a?'style="color:#c00;font-weight:700"':''}>${k}: ${stripB(v)}${a?'⚠':''}</span>`}).join(’ · ‘);
}).filter(Boolean).join(’<br>’):‘No lab results entered’;

const vitsHtml=latestVits?VIT_KEYS.filter(k=>latestVits[k]).map(k=>VIT_LBL[k]+’: ‘+latestVits[k]).join(’ · ’):‘No vitals entered’;

const txHtml=(p.treatments||[]).map(tx=>{
if(tx.type===‘IVMP’){const d=tx.startDate?dSince(tx.startDate)+1:null;return’IVMP’+(tx.startDate?’ from ‘+fmtD(tx.startDate):’’)+(d?’ (Day ‘+d+’ of ‘+(tx.daysTotal||5)+’)’+’’:’’)}
if(tx.type===‘IVIG’){const d=tx.startDate?dSince(tx.startDate)+1:null;return’IVIG’+(tx.dose?’ ‘+tx.dose:’’)+(tx.startDate?’ from ‘+fmtD(tx.startDate):’’)+(d?’ (Day ‘+d+’ of ‘+(tx.daysTotal||5)+’)’:’’)}
if(tx.type===‘PLEX’)return’PLEX’+(tx.startDate?’ from ‘+fmtD(tx.startDate)+’ — ‘+plexInfo(tx):’’);
return tx.type;
}).join(’<br>’)||’—’;

const abHtml=(p.antibiotics||[]).map(ab=>{
const exp=ab.startDate&&ab.days?(()=>{const d=new Date(ab.startDate);d.setDate(d.getDate()+ +ab.days);return d.toISOString().split(‘T’)[0]})():null;
return ab.drug+(ab.startDate?’ from ‘+fmtD(ab.startDate):’’)+(exp?’ · expires ‘+fmtD(exp):’’)+(ab.indication?’ (’+ab.indication+’)’:’’);
}).join(’<br>’)||’—’;

const medHtml=(p.concurrentMeds||[]).map(m=>`<span ${m.status==='stopped'?'style="text-decoration:line-through;color:#c00"':m.status==='onhold'?'style="color:#b45309"':''}>${m.name||'—'}${m.dose?' '+m.dose:''} [${m.status}]${m.startDate?' from '+fmtD(m.startDate):''}${m.stopDate?' → '+fmtD(m.stopDate):''}</span>`).join(’<br>’)||’—’;

const evHtml=[…(p.hospitalCourse||[])].sort((a,b)=>b.date.localeCompare(a.date)).map(e=>`<b>${fmtD(e.date)}:</b> ${e.text}`).join(’<br><br>’)||’—’;

const consHtml=(p.consultations||[]).map(c=>`${c.specialty}${c.doctor?' ('+c.doctor+')':''} · Sent ${fmtD(c.dateSent)} · ${c.responded?'✓ Responded':'⚠ Awaiting'}${c.recommendations?'<br><i>'+c.recommendations+'</i>':''}`).join(’<br>’)||’—’;

const devHtml=(p.devices||[]).map(d=>`${d.type}${d.site?' — '+d.site:''} · Inserted ${fmtD(d.insertionDate)} (Day ${(dSince(d.insertionDate)||0)+1})${d.removalDate?' · Removed '+fmtD(d.removalDate):''}`).join(’<br>’)||’—’;

w.document.write(`<!DOCTYPE html><html><head><meta charset="UTF-8"><title>NeuroWard — ${p.name}</title>

  <style>
    body{font-family:Arial,sans-serif;font-size:12px;color:#000;padding:20px;max-width:900px;margin:0 auto}
    h1{font-size:18px;margin-bottom:2px;color:#1e3a5f}
    .meta{font-size:12px;color:#555;margin-bottom:4px}
    .chips{display:flex;flex-wrap:wrap;gap:6px;margin-bottom:12px}
    .chip{border:1px solid #ccc;border-radius:4px;padding:2px 8px;font-size:11px;background:#f5f7ff}
    .chip.diag{border-color:#6d28d9;color:#5b21b6;background:#f3f0ff}
    .alerts{background:#fff7ed;border:1px solid #f59e0b;border-radius:6px;padding:8px;margin-bottom:12px;font-size:11px}
    .psec{border:1px solid #ddd;border-radius:6px;margin-bottom:10px;overflow:hidden;break-inside:avoid}
    .psec-title{background:#f0f4ff;color:#1e3a5f;font-weight:700;font-size:11px;text-transform:uppercase;letter-spacing:1px;padding:6px 12px;border-bottom:1px solid #ddd}
    .psec-content{padding:10px 12px;font-size:12px;line-height:1.7}
    .g2{display:grid;grid-template-columns:1fr 1fr;gap:12px}
    .g3{display:grid;grid-template-columns:1fr 1fr 1fr;gap:12px}
    b{color:#1e3a5f}
    @media print{body{padding:6px}.psec{break-inside:avoid}}
  </style></head><body>

  <h1>⚕ ${p.name||'Unnamed'}${p.nameAr?' · '+p.nameAr:''}</h1>
  <div class="meta">${p.age}y ${p.sex}${p.handedness?' · '+p.handedness+'-handed':''}${p.residence?' · '+p.residence:''}</div>
  <div class="meta">DOA: ${fmtD(p.doa)} · ${admDay(p.doa)}${p.dos?' · DOS: '+fmtD(p.dos):''}${p.referringStaff?' · Ref: '+p.referringStaff:''}</div>
  <div class="meta">Printed: ${now}</div>
  <div class="chips">
    ${p.diagnosis?`<span class="chip diag">${p.diagnosis}</span>`:''}
    ${p.ddx?`<span class="chip">DDx: ${p.ddx}</span>`:''}
  </div>
  ${alerts.length?`<div class="alerts"><b>⚠ Active Alerts:</b> ${alerts.map(a=>a.msg).join(' · ')}</div>`:''}
  ${secPrint('History',`<div class="g2"><div><b>Complaint:</b> ${p.complaint||'—'}<br><br><b>HPI:</b><br>${p.hpi||'—'}</div><div><b>Past Medical Hx:</b><br>${p.pastHistory||'—'}<br><br><b>Personal Hx:</b><br>${p.personalHistory||'—'}<br><br><b>Family Hx:</b><br>${p.familyHistory||'—'}</div></div>`)}
  ${secPrint('Neurological Examination',`
    <div class="g2" style="margin-bottom:8px"><div><b>General:</b> ${p.exam?.general||'—'}</div><div><b>Cranial Nerves:</b> ${p.exam?.cn||'—'}</div></div>
    <div style="margin-bottom:8px"><b>Motor Tone:</b> ${p.exam?.tone||'—'}</div>
    <div class="g2" style="margin-bottom:8px"><div><b>Power:</b> RUL ${p.exam?.RUL||'—'} · LUL ${p.exam?.LUL||'—'} · RLL ${p.exam?.RLL||'—'} · LLL ${p.exam?.LLL||'—'}</div><div><b>Reflexes:</b> Sup ${p.exam?.reflexesSup||'—'} · Deep ${p.exam?.reflexesDeep||'—'} · Plantar ${p.exam?.plantar||'—'}</div></div>
    <div class="g2" style="margin-bottom:8px"><div><b>Sensory:</b> Sup ${p.exam?.sensSup||'—'} · Deep ${p.exam?.sensDeep||'—'}</div><div><b>Coordination:</b> ${p.exam?.coordination||'—'} · <b>Gait:</b> ${p.exam?.gait||'—'}</div></div>
    <div><b>Cognition:</b> ${p.exam?.cognition||'—'}</div>`)}
  ${secPrint('Investigations',`
    ${invTable([...(p.specificLabs||[]),...(p.otherLabs||[])],'Labs')}
    ${invTable(p.imaging||[],'Imaging')}
    ${invTable(p.npStudies||[],'Neurophysiology')}
    <div style="margin-top:6px"><b>Latest Routine Labs (${fmtD(latestLabs?.date)}):</b><br>${labsHtml}</div>`)}
  ${secPrint('Vital Signs',vitsHtml)}
  ${secPrint('Ophthalmology',`VA Right: ${p.ophthalmology?.va_r||'—'} · Left: ${p.ophthalmology?.va_l||'—'}${p.ophthalmology?.notes?'<br>Notes: '+p.ophthalmology.notes:''}`)}
  ${secPrint('Consultations',consHtml)}
  ${secPrint('Treatment',`<b>Active Treatments:</b><br>${txHtml}<br><br><b>Antibiotics:</b><br>${abHtml}<br><br><b>Concurrent Medications:</b><br>${medHtml}`)}
  ${secPrint('Devices & Lines',devHtml)}
  ${secPrint('Hospital Course',evHtml)}
  <div style="text-align:right;font-size:10px;color:#999;margin-top:12px">NeuroWard · ${now}</div>
  <script>window.onload=()=>window.print()<\/script>
  </body></html>`);
  w.document.close();
}

// ── PRINT ALL PATIENTS ────────────────────────────────────────────────────────
function openPrintAll(){
const w=window.open(’’,’_blank’);
const now=new Date().toLocaleDateString(‘en-GB’,{weekday:‘long’,day:‘numeric’,month:‘long’,year:‘numeric’});
let html=`<!DOCTYPE html><html><head><meta charset="UTF-8"><title>NeuroWard Round Sheet</title>

  <style>
    body{font-family:Arial,sans-serif;font-size:12px;color:#000;padding:20px}
    h1{font-size:16px;margin-bottom:4px}
    .ptblock{border:1px solid #ccc;border-radius:6px;padding:14px;margin-bottom:14px;break-inside:avoid;page-break-inside:avoid}
    .ptname{font-size:14px;font-weight:700;margin-bottom:3px}
    .ptmeta{font-size:11px;color:#555;margin-bottom:10px}
    .cols{display:grid;grid-template-columns:1fr 1fr 1fr;gap:14px}
    .col-title{font-size:10px;font-weight:700;text-transform:uppercase;letter-spacing:1px;color:#1e40af;margin-bottom:5px}
    .ar{color:#c00}.ay{color:#b45309}.ab2{color:#1d4ed8}.at{color:#0f766e}
    .ok{color:#166534}
    @media print{body{padding:6px}}
  </style></head><body>

  <h1>⚕ NeuroWard — Daily Round Sheet</h1>
  <div style="font-size:11px;color:#555;margin-bottom:16px">${now} · ${patients.length} patient(s)</div>`;

patients.forEach(p=>{
const alerts=getAlerts(p);
const latestLabs=(p.routineLabs||[]).slice(-1)[0];
const abnLabs=latestLabs?Object.entries(latestLabs.values||{}).filter(([,v])=>isAbn(v)).map(([k,v])=>k+’: ‘+stripB(v)+’ ⚠’):[];
const activeTx=(p.treatments||[]).map(tx=>{
if(tx.type===‘IVMP’&&tx.startDate){const d=dSince(tx.startDate)+1;return d<=(tx.daysTotal||5)?‘IVMP Day ‘+d+’/’+tx.daysTotal:null}
if(tx.type===‘IVIG’&&tx.startDate){const d=dSince(tx.startDate)+1;return d<=(tx.daysTotal||5)?‘IVIG Day ‘+d+’/’+tx.daysTotal:null}
if(tx.type===‘PLEX’&&tx.startDate)return’PLEX — ’+plexInfo(tx);
return null;
}).filter(Boolean);
const pending=[…(p.specificLabs||[]),…(p.otherLabs||[]),…(p.imaging||[]),…(p.npStudies||[])].filter(l=>l.status===‘ordered’||l.status===‘pending’||l.status===‘done-pending-result’);
html+=`<div class="ptblock"> <div class="ptname">${p.name||'Unnamed'}${p.nameAr?' · '+p.nameAr:''} <span style="font-weight:400;font-size:11px;color:#555">· ${p.age}y ${p.sex} · ${admDay(p.doa)}</span></div> <div class="ptmeta">${p.diagnosis||'No diagnosis'}${p.referringStaff?' · Ref: '+p.referringStaff:''}</div> <div class="cols"> <div> <div class="col-title">Alerts & Tasks</div> ${alerts.length?alerts.map(a=>`<div class="a${a.lv}" style="font-size:11px;margin-bottom:2px">• ${a.msg}</div>`).join(''):'<div class="ok">✓ Clear</div>'} </div> <div> <div class="col-title">Active Treatment</div> ${activeTx.length?activeTx.map(t=>`<div style="font-size:11px">• ${t}</div>`).join(''):'<div style="color:#555;font-size:11px">—</div>'} ${abnLabs.length?'<div style="margin-top:6px"><div class="col-title" style="color:#c00">Abnormal Labs</div>'+abnLabs.map(l=>`<div class="ar" style="font-size:11px">• ${l}</div>`).join('')+'</div>':''} </div> <div> <div class="col-title">Pending (${pending.length})</div> ${pending.map(l=>`<div style="font-size:11px">• ${l.name} — ${ST_LBL[l.status]}</div>`).join('')||'<div style="color:#555;font-size:11px">—</div>'} </div> </div> </div>`;
});
html+=`</body></html>`;
w.document.write(html);w.document.close();
setTimeout(()=>w.print(),500);
}

// ── MIGRATE OLD DATA ──────────────────────────────────────────────────────────
function migrate(){
patients.forEach(p=>{
p.collapsedSections=p.collapsedSections||{};
p.otherLabs=p.otherLabs||[];
p.concurrentMeds=p.concurrentMeds||[];
p.vitals=p.vitals||[];
p.attachments=p.attachments||[];
p.exam=p.exam||{};p.exam.cognition=p.exam.cognition||’’;
if(!p.ophthalmology)p.ophthalmology={va_r:’’,va_l:’’,oct:mkInv(‘OCT’),perimetry:mkInv(‘Perimetry’),notes:’’,fundusEntries:[]};
if(!p.ophthalmology.oct?.id)p.ophthalmology.oct=mkInv(‘OCT’);
if(!p.ophthalmology.perimetry?.id)p.ophthalmology.perimetry=mkInv(‘Perimetry’);
[‘specificLabs’,‘otherLabs’,‘imaging’,‘npStudies’].forEach(key=>{(p[key]||[]).forEach(item=>{if(!item.id)item.id=uid()})});
(p.treatments||[]).forEach(tx=>{
if(tx.type===‘PLEX’){
if(tx.labsDone===undefined)tx.labsDone=!!(tx.sessions?.[0]?.labs);
if(tx.mahurkarDone===undefined)tx.mahurkarDone=!!(tx.sessions?.[0]?.mahurkar);
if(!tx.frequency)tx.frequency=‘every2days’;
delete tx.sessions;
}
});
// Migrate home meds to concurrent meds
if(p.homeMeds?.length&&!p.concurrentMeds?.length){
p.concurrentMeds=p.homeMeds.map(m=>({name:m.drug||’’,dose:’’,startDate:’’,stopDate:’’,status:m.status===‘hold’?‘onhold’:m.status||‘active’}));
delete p.homeMeds;
}
});
saveD();// Save after migration
}

// ── INIT ──────────────────────────────────────────────────────────────────────
document.getElementById(‘daybadge’).textContent=new Date().toLocaleDateString(‘en-US’,{weekday:‘long’,month:‘short’,day:‘numeric’});
loadD();
migrate();
renderList();

// ── ATTACHMENTS ───────────────────────────────────────────────────────────────
function openAttachments(){
const p=getP();if(!p)return;
p.attachments=p.attachments||[];
renderAttList(p);
openM(‘attmod’);
}

function renderAttList(p){
const list=p.attachments||[];
const el=document.getElementById(‘att-list’);
if(!list.length){el.innerHTML=’<div style="color:var(--text3);font-size:13px">No attachments yet.</div>’;return;}
el.innerHTML=list.map((a,i)=>` <div style="display:flex;align-items:center;gap:10px;background:var(--surface2);border:1px solid var(--border2);border-radius:8px;padding:10px;margin-bottom:8px"> <div style="font-size:28px;flex-shrink:0">${attIcon(a.type)}</div> <div style="flex:1;min-width:0"> <div style="font-weight:600;font-size:13px;color:var(--text);white-space:nowrap;overflow:hidden;text-overflow:ellipsis">${esc(a.name)}</div> <div style="font-size:11px;color:var(--text3)">${fmtD(a.date)} · ${attSize(a.data)}</div> </div> <div style="display:flex;gap:6px;flex-shrink:0"> ${a.type.startsWith('image/')?`<button class="btn btng btnsm" onclick="previewAtt(${i})">👁 View</button>`:''} <button class="btn btng btnsm" onclick="downloadAtt(${i})">⬇ Save</button> <button class="rmbtn" onclick="rmAtt(${i})">✕</button> </div> </div>`).join(’’);
}

function attIcon(type){
if(type.startsWith(‘image/’))return’🖼’;
if(type===‘application/pdf’)return’📄’;
if(type.includes(‘word’))return’📝’;
return’📎’;
}
function attSize(b64){const bytes=Math.round((b64.length*3)/4);return bytes>1048576?(bytes/1048576).toFixed(1)+‘MB’:Math.round(bytes/1024)+‘KB’;}

function handleAttachFiles(input){
const p=getP();if(!p)return;
p.attachments=p.attachments||[];
const files=Array.from(input.files);
let done=0;
files.forEach(f=>{
const reader=new FileReader();
reader.onload=ev=>{
p.attachments.push({id:uid(),name:f.name,type:f.type||‘application/octet-stream’,date:td(),data:ev.target.result.split(’,’)[1]});
done++;
if(done===files.length){saveD();renderAttList(p);}
};
reader.readAsDataURL(f);
});
input.value=’’;
}

function previewAtt(i){
const p=getP();if(!p)return;
const a=p.attachments[i];
const w=window.open(’’,’_blank’);
w.document.write(`<html><body style="margin:0;background:#000;display:flex;align-items:center;justify-content:center;min-height:100vh"><img src="data:${a.type};base64,${a.data}" style="max-width:100%;max-height:100vh;object-fit:contain"></body></html>`);
w.document.close();
}

function downloadAtt(i){
const p=getP();if(!p)return;
const a=p.attachments[i];
const link=document.createElement(‘a’);
link.href=`data:${a.type};base64,${a.data}`;
link.download=a.name;
link.click();
}

function rmAtt(i){
const p=getP();if(!p)return;
if(!confirm(‘Remove this attachment?’))return;
p.attachments.splice(i,1);
saveD();renderAttList(p);
}

// ── DOCX GENERATION via Anthropic API ────────────────────────────────────────
// These generate DOCX on-the-fly using the API to write and run Node.js code

async function exportMedFil48(){
const p=getP();
if(!p){alert(‘Select a patient first’);return;}
if(isFileProto()){alert(‘DOCX export requires internet connection. Open via http:// (not file://).’);return;}

// Gather all lab data
const latestLabs=(p.routineLabs||[]).slice(-1)[0];
const labVals=latestLabs?.values||{};
const admLabs=(p.routineLabs||[]).sort((a,b)=>a.date.localeCompare(b.date))[0]?.values||{};

const activeTx=(p.treatments||[]).map(tx=>{
if(tx.type===‘IVMP’)return`IVMP ${tx.startDate?'from '+tx.startDate:''} (${tx.daysTotal||5} days)`;
if(tx.type===‘IVIG’)return`IVIG ${tx.dose||''} ${tx.startDate?'from '+tx.startDate:''} (${tx.daysTotal||5} days)`;
if(tx.type===‘PLEX’)return`PLEX ${tx.sessionsTotal||5} sessions ${tx.frequency||'every 2 days'}`;
return tx.type;
}).join(’; ’);

const meds=(p.concurrentMeds||[]).filter(m=>m.status===‘active’).map(m=>`${m.name}${m.dose?' '+m.dose:''}`).join(’, ‘);
const abs=(p.antibiotics||[]).map(ab=>ab.drug+(ab.indication?’ (’+ab.indication+’)’:’’)).join(’, ‘);
const allMeds=[activeTx,meds,abs].filter(Boolean).join(’ | ’);

const patientData={
name:p.name||’’,nameAr:p.nameAr||’’,age:p.age||’’,sex:p.sex||’’,
hospitalNumber:’’,telephone:’’,sitsNo:’’,
dateAdmission:p.doa||’’,dateDischarge:p.dos||’’,
diagnosis:p.diagnosis||’’,
clinicalOnAdmission:p.complaint+(p.hpi?’\n’+p.hpi:’’),
clinicalDischarge:`Exam: General ${p.exam?.general||'—'} | Power RUL${p.exam?.RUL||'?'} LUL${p.exam?.LUL||'?'} RLL${p.exam?.RLL||'?'} LLL${p.exam?.LLL||'?'} | Reflexes ${p.exam?.reflexesDeep||'—'} | Coordination ${p.exam?.coordination||'—'} | Gait ${p.exam?.gait||'—'}`,
hospitalCourse:(p.hospitalCourse||[]).sort((a,b)=>a.date.localeCompare(b.date)).map(e=>e.date+’: ‘+e.text).join(’\n’)||’’,
// Lab values: admission, 72hrs (approx 3 days), discharge (latest)
labHb_adm:admLabs.Hb||’’,labHb_dis:labVals.Hb||’’,
labTLC_adm:admLabs.WBC||’’,labTLC_dis:labVals.WBC||’’,
labPLT_adm:admLabs.PLT||’’,labPLT_dis:labVals.PLT||’’,
labCRP_adm:admLabs.CRP||’’,labCRP_dis:labVals.CRP||’’,
labINR_adm:admLabs.INR||’’,labINR_dis:labVals.INR||’’,
labCreat_adm:admLabs.Creatinine||’’,labCreat_dis:labVals.Creatinine||’’,
labUrea_adm:admLabs.Urea||’’,labUrea_dis:labVals.Urea||’’,
labALT_adm:admLabs.ALT||’’,labALT_dis:labVals.ALT||’’,
labAST_adm:admLabs.AST||’’,labAST_dis:labVals.AST||’’,
labAlb_adm:admLabs.Albumin||’’,labAlb_dis:labVals.Albumin||’’,
labBil_adm:admLabs.Bilirubin||’’,labBil_dis:labVals.Bilirubin||’’,
labNa_adm:admLabs.Na||’’,labNa_dis:labVals.Na||’’,
labK_adm:admLabs.K||’’,labK_dis:labVals.K||’’,
labCa_adm:admLabs.Ca||’’,labCa_dis:labVals.Ca||’’,
labMg_adm:admLabs.Mg||’’,labMg_dis:labVals.Mg||’’,
medications:allMeds,
nihssAdm:’’,nihss72:’’,nihssDis:’’,mrsAdm:’’,mrs72:’’,mrsDis:’’
};

await generateDocxViaAPI(‘medfil48’, patientData, `MedFil48_${(p.name||'patient').replace(/\s+/g,'_')}_${td()}.docx`);
}

async function exportFullDischarge(){
const p=getP();
if(!p){alert(‘Select a patient first’);return;}
if(isFileProto()){alert(‘DOCX export requires internet connection. Open via http:// (not file://).’);return;}
await generateDocxViaAPI(‘fullDischarge’, p, `Discharge_${(p.name||'patient').replace(/\s+/g,'_')}_${td()}.docx`);
}

async function generateDocxViaAPI(template, data, filename){
// Show loading
const loadDiv=document.createElement(‘div’);
loadDiv.style.cssText=‘position:fixed;inset:0;background:rgba(0,0,0,0.7);z-index:9999;display:flex;align-items:center;justify-content:center;flex-direction:column;gap:12px;color:#fff;font-size:15px’;
loadDiv.innerHTML=’<div style="font-size:40px">⏳</div><div>Generating DOCX…</div>’;
document.body.appendChild(loadDiv);

try{
const prompt=template===‘medfil48’?buildMedFil48Prompt(data):buildFullDischargePrompt(data);
const res=await fetch(‘https://api.anthropic.com/v1/messages’,{
method:‘POST’,
headers:{‘Content-Type’:‘application/json’},
body:JSON.stringify({model:‘claude-sonnet-4-20250514’,max_tokens:4000,messages:[{role:‘user’,content:prompt}]})
});
const result=await res.json();
const jsCode=result.content?.find(b=>b.type===‘text’)?.text||’’;

```
// Extract JS code block
const match=jsCode.match(/```javascript\n([\s\S]+?)\n```/)||jsCode.match(/```js\n([\s\S]+?)\n```/)||[null,jsCode];
const code=match[1]||jsCode;

// Execute via blob URL approach — inject script that uses CDN docx
await executeDocxCode(code, filename);
```

}catch(e){
alert(’Error generating DOCX: ’+e.message);
console.error(e);
}finally{
document.body.removeChild(loadDiv);
}
}

function buildMedFil48Prompt(d){
return `Generate JavaScript code using the docx npm library (already available as global ‘docx’) to create a Word document replicating the Cairo University / KSCRTU Med-Fil-48 discharge summary form.

The form has these sections:

1. Header: “Discharge Summary” title, hospital name “Cairo University Hospitals / Kasralainy Stroke and Cerebrovascular Diseases Research and Treatment Unit (KSCRTU)”
1. Arabic header on right side (mirror): مستشفيات جامعة القاهرة / قسم امراض المخ والاعصاب / وحدة أبحاث وعلاج السكتة الدماغية
1. Patient info box: Name, Age, Hospital Number, Date of Admission, Date of Discharge, Telephone number, SITS no.
1. Diagnosis box (large)
1. Personal History section (blank box for editing)
1. Clinical Data section with “On Admission:” and “Discharge:” sub-boxes
1. Hospital Course section (large blank box)
1. Second page: Lab results table with rows: HB, TLC, PLT, CRP, ESR, Procal, INR, Creat, Urea, Uric acid, ALT, AST, Albumin, Bilirubin, Na, K, Ca (T/I), Mg, HBA1c, Cholesterol, Triglycerides, LDL, HDL — 3 columns: Admission, 72hrs, Discharge
1. Investigations section (blank)
1. NIHSS and mRS row: Admission, 72hrs, Discharge columns
1. Plan of Care / Medications (التعليمات) with 6 bullet lines

Pre-fill with this patient data:
${JSON.stringify(d,null,2)}

The code should:

- Use A4 page size
- Use proper borders on all boxes
- Include Arabic text where needed (right-to-left)
- Pre-fill all available data
- Leave empty fields as blank lines for manual completion
- Export the document as base64 and call window._docxDone(base64Data)

Write ONLY the JavaScript code, no explanation. Use this pattern:
const { Document, Packer, Paragraph, TextRun, Table, TableRow, TableCell, BorderStyle, WidthType, ShadingType, AlignmentType, VerticalAlign } = docx;
// … build document …
Packer.toBase64String(doc).then(b64 => window._docxDone(b64));`;
}

function buildFullDischargePrompt(p){
const latestLabs=(p.routineLabs||[]).slice(-1)[0];
const labStr=latestLabs?Object.entries(latestLabs.values||{}).map(([k,v])=>`${k}: ${v}`).join(’, ‘):‘None’;
const txStr=(p.treatments||[]).map(t=>t.type+(t.startDate?’ from ‘+t.startDate:’’)).join(’, ‘)||‘None’;
const medStr=(p.concurrentMeds||[]).map(m=>`${m.name} ${m.dose||''} [${m.status}]`).join(’, ‘)||‘None’;
const abStr=(p.antibiotics||[]).map(ab=>ab.drug+(ab.indication?’ - ‘+ab.indication:’’)).join(’, ‘)||‘None’;
const invStr=[…(p.specificLabs||[]),…(p.otherLabs||[]),…(p.imaging||[]),…(p.npStudies||[])].filter(l=>l.result||l.status===‘resulted’).map(l=>l.name+’: ‘+(l.result||‘resulted’)).join(’\n’)||‘None’;
const courseStr=(p.hospitalCourse||[]).sort((a,b)=>a.date.localeCompare(b.date)).map(e=>e.date+’: ‘+e.text).join(’\n’)||‘None’;
const consStr=(p.consultations||[]).map(c=>c.specialty+(c.responded?’ (Responded: ‘+c.recommendations+’)’:’ (Awaiting)’)).join(’\n’)||‘None’;
const devStr=(p.devices||[]).map(d=>d.type+’ at ‘+d.site+’ inserted ‘+d.insertionDate).join(’\n’)||‘None’;

return `Generate JavaScript code using the docx npm library (already available as global ‘docx’) to create a comprehensive hospital discharge summary Word document.

Design a clean, professional medical discharge summary with these sections:

1. Header: Hospital name “Kasralainy Stroke & Cerebrovascular Diseases Research and Treatment Unit (KSCRTU) — Cairo University Hospitals” with a blue header bar
1. Patient Demographics box: Name (EN + AR), Age, Sex, Handedness, Residence, Occupation, Marital Status, DOA, DOS, Admission Day, Diagnosis, DDx, Referring Staff
1. History: Chief Complaint, HPI, Past Medical History, Personal History, Family History
1. Neurological Examination: General, Cranial Nerves, Tone, Motor Power (RUL/LUL/RLL/LLL), Reflexes, Sensory, Coordination, Gait, Cognition
1. Investigations Results (only those with results)
1. Latest Lab Values
1. Vital Signs (most recent)
1. Consultations
1. Treatment Given: IVMP/IVIG/PLEX details, Antibiotics, Concurrent Medications
1. Devices & Lines
1. Hospital Course (chronological)
1. Discharge Plan / Follow-up

Patient Data:
Name: ${p.name||’’}  Arabic: ${p.nameAr||’’}
Age: ${p.age||’’}  Sex: ${p.sex||’’}  Handedness: ${p.handedness||’’}
Residence: ${p.residence||’’}  Occupation: ${p.occupation||’’}  Marital: ${p.marital||’’}
DOA: ${p.doa||’’}  DOS: ${p.dos||’’}  Referring: ${p.referringStaff||’’}
Diagnosis: ${p.diagnosis||’’}  DDx: ${p.ddx||’’}

HISTORY:
Complaint: ${p.complaint||’’}
HPI: ${p.hpi||’’}
Past Medical: ${p.pastHistory||’’}
Personal: ${p.personalHistory||’’}
Family: ${p.familyHistory||’’}

EXAMINATION:
General: ${p.exam?.general||’’}
CN: ${p.exam?.cn||’’}
Tone: ${p.exam?.tone||’’}
Power: RUL=${p.exam?.RUL||’’} LUL=${p.exam?.LUL||’’} RLL=${p.exam?.RLL||’’} LLL=${p.exam?.LLL||’’}
Reflexes Sup: ${p.exam?.reflexesSup||’’} Deep: ${p.exam?.reflexesDeep||’’} Plantar: ${p.exam?.plantar||’’}
Sensory Sup: ${p.exam?.sensSup||’’} Deep: ${p.exam?.sensDeep||’’}
Coordination: ${p.exam?.coordination||’’}
Gait: ${p.exam?.gait||’’}
Cognition: ${p.exam?.cognition||’’}

INVESTIGATIONS: ${invStr}
LATEST LABS (${latestLabs?.date||’’}): ${labStr}
TREATMENTS: ${txStr}
CONCURRENT MEDS: ${medStr}
ANTIBIOTICS: ${abStr}
CONSULTATIONS: ${consStr}
DEVICES: ${devStr}
HOSPITAL COURSE: ${courseStr}

The code should:

- Use A4 page size (11906 x 16838 DXA), 1 inch margins
- Blue accent color #1e3a5f for headers
- Professional table formatting for the exam grid
- Export as base64 and call window._docxDone(base64Data)

Write ONLY the JavaScript code. Use:
const { Document, Packer, Paragraph, TextRun, Table, TableRow, TableCell, BorderStyle, WidthType, ShadingType, AlignmentType, VerticalAlign, HeadingLevel } = docx;
// … build document …
Packer.toBase64String(doc).then(b64 => window._docxDone(b64));`;
}

async function executeDocxCode(code, filename){
return new Promise((resolve,reject)=>{
window._docxDone=(b64)=>{
const binary=atob(b64);
const arr=new Uint8Array(binary.length);
for(let i=0;i<binary.length;i++)arr[i]=binary.charCodeAt(i);
const blob=new Blob([arr],{type:‘application/vnd.openxmlformats-officedocument.wordprocessingml.document’});
const a=document.createElement(‘a’);
a.href=URL.createObjectURL(blob);
a.download=filename;
a.click();
URL.revokeObjectURL(a.href);
delete window._docxDone;
resolve();
};

```
// Load docx from CDN if not available, then run code
if(window.docx){
  try{new Function(code)();} catch(e){reject(e);}
} else {
  const script=document.createElement('script');
  script.src='https://unpkg.com/docx@9.0.3/build/index.js';
  script.onload=()=>{
    try{new Function(code)();} catch(e){reject(e);}
  };
  script.onerror=()=>reject(new Error('Could not load docx library'));
  document.head.appendChild(script);
}
```

});
}
</script>

</body>
</html>
