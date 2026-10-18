<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="utf-8">
<meta name="viewport" content="width=device-width, initial-scale=1, viewport-fit=cover">
<title>Ledger & Profit Tracker</title>
<style>
:root {
  --bg: #f8fafc;
  --surface: #ffffff;
  --text: #0f172a;
  --muted: #64748b;
  --border: #e2e8f0;
  --primary: #0284c7;
  --primary-sub: #e0f2fe;
  --green: #16a34a;
  --green-sub: #dcfce7;
  --red: #dc2626;
  --red-sub: #fee2e2;
  --amber: #d97706;
  --amber-sub: #fef3c7;
  --purple: #7c3aed;
  --purple-sub: #ede9fe;
  --radius: 12px;
}
:root[data-theme="dark"] {
  --bg: #090d16;
  --surface: #131b2e;
  --text: #f8fafc;
  --muted: #94a3b8;
  --border: #243048;
  --primary: #38bdf8;
  --primary-sub: rgba(56,189,248,0.12);
  --green: #4ade80;
  --green-sub: rgba(74,222,128,0.12);
  --red: #f87171;
  --red-sub: rgba(248,113,113,0.12);
  --amber: #fbbf24;
  --amber-sub: rgba(251,191,36,0.12);
  --purple: #a78bfa;
  --purple-sub: rgba(167,139,250,0.12);
}
* { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }
body {
  margin: 0;
  height: 100vh;
  display: flex;
  flex-direction: column;
  background: var(--bg);
  color: var(--text);
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, sans-serif;
  font-size: 14px;
}
header {
  padding: 12px 16px;
  background: var(--surface);
  border-bottom: 1px solid var(--border);
  display: flex;
  align-items: center;
  justify-content: space-between;
}
header h1 { margin: 0; font-size: 16px; font-weight: 700; }
header small { display: block; font-size: 11px; color: var(--muted); }

#main {
  flex: 1;
  overflow-y: auto;
  padding: 14px 14px 100px;
}

.card { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 12px 14px; margin-bottom: 10px; }
.grid-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-bottom: 8px; }
.kpi { background: var(--surface); border: 1px solid var(--border); border-radius: var(--radius); padding: 10px 12px; }
.kpi .lbl { font-size: 10px; font-weight: 700; text-transform: uppercase; color: var(--muted); }
.kpi .val { font-size: 16px; font-weight: 800; margin-top: 2px; }

.entry-card {
  background: var(--surface);
  border: 1px solid var(--border);
  border-radius: var(--radius);
  padding: 12px;
  margin-bottom: 8px;
  cursor: pointer;
}
.entry-card.crit { border-color: var(--red); background: var(--red-sub); }
.entry-top { display: flex; justify-content: space-between; align-items: flex-start; }
.entry-title { font-size: 14px; font-weight: 700; }
.entry-meta { display: flex; justify-content: space-between; margin-top: 8px; padding-top: 6px; border-top: 1px solid var(--border); font-size: 12px; }

.pill { display: inline-block; font-size: 10px; font-weight: 700; border-radius: 4px; padding: 2px 6px; text-transform: uppercase; }
.pill-cash { background: var(--green-sub); color: var(--green); }
.pill-credit { background: var(--amber-sub); color: var(--amber); }
.pill-crit { background: var(--red-sub); color: var(--red); }
.pill-settled { background: var(--border); color: var(--muted); }
.pill-sp { background: var(--purple-sub); color: var(--purple); }

input, select {
  width: 100%;
  padding: 8px 10px;
  border: 1px solid var(--border);
  border-radius: 6px;
  background: var(--bg);
  color: var(--text);
  font: inherit;
  font-size: 13px;
  outline: none;
}
input:focus, select:focus { border-color: var(--primary); }
label { display: block; font-size: 10px; font-weight: 700; text-transform: uppercase; color: var(--muted); margin: 8px 0 3px; }

.btn {
  display: block;
  width: 100%;
  border: 0;
  border-radius: 6px;
  padding: 10px;
  font: inherit;
  font-size: 13px;
  font-weight: 700;
  text-align: center;
  cursor: pointer;
  margin-top: 10px;
  color: #fff;
  background: var(--primary);
}
.btn-sub { background: var(--border); color: var(--text); }
.btn-del { background: var(--red-sub); color: var(--red); }
.btn-purple { background: var(--purple); color: #fff; }
.btn-sm { padding: 4px 8px; border-radius: 4px; border: 1px solid var(--border); background: transparent; font-size: 11px; font-weight: 600; cursor: pointer; color: var(--text); }

.dock { position: fixed; right: 14px; bottom: 64px; display: flex; gap: 6px; z-index: 5; }
.fab { height: 40px; padding: 0 14px; border-radius: 20px; border: 0; color: #fff; font-size: 12px; font-weight: 700; cursor: pointer; display: flex; align-items: center; box-shadow: 0 4px 10px rgba(0,0,0,0.15); }
.fab.sale { background: var(--primary); }
.fab.buy { background: #334155; }
.fab.sp { background: var(--purple); }

nav {
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  background: var(--surface);
  border-top: 1px solid var(--border);
  display: flex;
  padding: 4px 0;
  z-index: 4;
}
nav button {
  flex: 1;
  background: none;
  border: 0;
  color: var(--muted);
  padding: 6px 0;
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2px;
  font-size: 10px;
  font-weight: 600;
  cursor: pointer;
}
nav button.active { color: var(--primary); }
nav button[data-tab="special"].active { color: var(--purple); }
nav button b { font-size: 16px; font-weight: 400; }

.modal-layer { position: fixed; inset: 0; background: rgba(15,23,42,0.6); display: none; align-items: flex-end; z-index: 10; }
.modal-layer.open { display: flex; }
.modal-box { background: var(--surface); width: 100%; max-height: 90vh; overflow-y: auto; border-radius: 16px 16px 0 0; padding: 16px; }
.modal-head { display: flex; justify-content: space-between; align-items: center; margin-bottom: 12px; }
.modal-head h3 { margin: 0; font-size: 16px; font-weight: 700; }
.modal-close { background: none; border: 0; font-size: 18px; color: var(--muted); cursor: pointer; }

.item-draft { border: 1px dashed var(--border); border-radius: 6px; padding: 8px; margin-bottom: 6px; background: var(--bg); }
.seg-bar { display: flex; background: var(--border); padding: 3px; border-radius: 6px; gap: 3px; margin-bottom: 10px; }
.seg-bar button { flex: 1; border: 0; background: none; padding: 6px; border-radius: 4px; font-size: 11px; font-weight: 700; color: var(--muted); cursor: pointer; }
.seg-bar button.active { background: var(--surface); color: var(--text); }
.sec-title { font-size: 11px; font-weight: 700; text-transform: uppercase; color: var(--muted); margin: 12px 0 6px; }
</style>
<script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-app-compat.js"></script>
<script src="https://www.gstatic.com/firebasejs/9.23.0/firebase-database-compat.js"></script>
</head>
<body>

<header>
  <div>
    <h1>Ledger & Profit Tracker</h1>
    <small>Independent Books</small>
  </div>
  <button class="btn-sm" id="btn-theme">Mode</button>
</header>

<div id="main"></div>

<div class="dock">
  <button class="fab sp" id="dock-sp">+ Special</button>
  <button class="fab buy" id="dock-buy">+ Purchase</button>
  <button class="fab sale" id="dock-sale">+ Sale</button>
</div>

<nav>
  <button data-tab="home" class="active"><b>▦</b>Dashboard</button>
  <button data-tab="sales"><b>☰</b>Sales</button>
  <button data-tab="purchases"><b>☷</b>Purchases</button>
  <button data-tab="special"><b>★</b>Special</button>
  <button data-tab="settings"><b>⚙</b>Settings</button>
</nav>

<div class="modal-layer" id="modal">
  <div class="modal-box" id="modal-content"></div>
</div>

<script>
let DB = {
  cur: 'Rs',
  theme: 'system',
  sales: [],
  purchases: [],
  specialSales: [],
  specialPurchases: [],
  firebaseUrl: ''
};

let currentTab = 'home';
let spSubTab = 'sales';
let searchFilter = '';
let draftItems = [];
let fbRef = null;

try {
  const local = localStorage.getItem('pt_fresh_db_v3');
  if (local) DB = Object.assign(DB, JSON.parse(local));
} catch(e) {}

const $ = sel => document.querySelector(sel);
const today = () => new Date().toISOString().slice(0, 10);
const num = v => Number(v) || 0;
const money = n => DB.cur + ' ' + (Math.round(num(n) * 100) / 100).toLocaleString(undefined, { minimumFractionDigits: 0, maximumFractionDigits: 2 });
const esc = s => String(s == null ? '' : s).replace(/[&<>"']/g, c => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c]));
const fmtDate = d => {
  if (!d) return '-';
  const p = d.split('-');
  return p.length === 3 ? `${p[2]}/${p[1]}/${p[0]}` : d;
};

function saveDB() {
  try { localStorage.setItem('pt_fresh_db_v3', JSON.stringify(DB)); } catch(e){}
  if (fbRef) {
    try {
      fbRef.set({
        cur: DB.cur,
        sales: DB.sales,
        purchases: DB.purchases,
        specialSales: DB.specialSales,
        specialPurchases: DB.specialPurchases
      });
    } catch(e){}
  }
}

function calcSale(s) {
  const items = (s.items && s.items.length) ? s.items : [{ qty: 1, cost: 0, price: 0 }];
  let rev = 0, cost = 0;
  items.forEach(i => {
    rev += num(i.qty) * num(i.price);
    cost += num(i.qty) * num(i.cost);
  });
  const paid = (s.pays || []).reduce((a, b) => a + num(b.a), 0);
  const rem = Math.max(0, Math.round((rev - paid) * 100) / 100);
  return { rev, cost, profit: rev - cost, paid, rem };
}

function calcPurchase(p) {
  const items = (p.items && p.items.length) ? p.items : [{ qty: 1, price: 0 }];
  let total = 0;
  items.forEach(i => { total += num(i.qty) * num(i.price); });
  const paid = (p.pays || []).reduce((a, b) => a + num(b.a), 0);
  const rem = Math.max(0, Math.round((total - paid) * 100) / 100);
  return { total, paid, rem };
}

function evalOverdue(dateStr, dueStr, rem) {
  if (rem <= 0.009) return { isCrit: false, text: 'Settled' };
  const target = dueStr || dateStr;
  if (!target) return { isCrit: false, text: 'Unpaid' };
  const diff = Math.round((new Date(today()) - new Date(target)) / 864e5);
  if (diff >= 60) return { isCrit: true, text: `${diff}d Overdue (60d+)` };
  if (diff > 0) return { isCrit: false, text: `${diff}d Overdue` };
  return { isCrit: false, text: dueStr ? `Due in ${Math.abs(diff)}d` : 'Pending' };
}

function getMetrics(isSpecial = false) {
  const sl = isSpecial ? DB.specialSales : DB.sales;
  const pl = isSpecial ? DB.specialPurchases : DB.purchases;
  let res = { rev: 0, profit: 0, rec: 0, buyTotal: 0, pay: 0, critCount: 0 };
  
  (sl || []).forEach(s => {
    const c = calcSale(s);
    res.rev += c.rev;
    res.profit += c.profit;
    res.rec += c.rem;
    if (evalOverdue(s.date, s.due, c.rem).isCrit) res.critCount++;
  });
  (pl || []).forEach(p => {
    const c = calcPurchase(p);
    res.buyTotal += c.total;
    res.pay += c.rem;
    if (evalOverdue(p.date, p.due, c.rem).isCrit) res.critCount++;
  });
  return res;
}

function renderCard(item, kind, isSpecial = false) {
  const isSale = kind === 'sale';
  const c = isSale ? calcSale(item) : calcPurchase(item);
  const ov = evalOverdue(item.date, item.due, c.rem);
  const title = isSale ? (item.cust || 'Direct Customer') : (item.supplier || 'Direct Supplier');
  const details = (item.items || []).map(i => `${i.name} ×${i.qty}`).join(' · ') || 'No item detail';
  const total = isSale ? c.rev : c.total;

  return `
  <div class="entry-card ${ov.isCrit ? 'crit' : ''}" data-act="open-detail" data-kind="${kind}" data-id="${item.id}" data-sp="${isSpecial}">
    <div class="entry-top">
      <div>
        <span class="entry-title">${esc(title)}</span>
        ${item.inv ? `<span style="font-family:monospace;font-size:11px;color:var(--muted);background:var(--border);padding:1px 4px;border-radius:4px;margin-left:4px">#${esc(item.inv)}</span>` : ''}
      </div>
      <div>
        <span class="pill ${item.type==='Cash'?'pill-cash':'pill-credit'}">${item.type||'Credit'}</span>
        ${ov.isCrit ? `<span class="pill pill-crit">${ov.text}</span>` : c.rem <= 0.009 ? `<span class="pill pill-settled">Settled</span>` : `<span class="pill pill-credit">${ov.text}</span>`}
      </div>
    </div>
    <div style="font-size:12px;color:var(--muted);margin-top:4px">${esc(details)}</div>
    <div class="entry-meta">
      <span style="color:var(--muted)">${fmtDate(item.date)}${item.due ? ` · Due ${fmtDate(item.due)}` : ''}</span>
      <div>
        <span>${money(total)}</span>
        ${c.rem > 0.009 ? `<span style="font-weight:700;margin-left:6px;color:var(--red)">Bal: ${money(c.rem)}</span>` : `<span style="font-weight:700;margin-left:6px;color:var(--green)">Paid</span>`}
      </div>
    </div>
  </div>`;
}

function renderGroup(list, kind, isSpecial = false) {
  if (!list || !list.length) return `<div class="card" style="text-align:center;color:var(--muted);padding:20px">No entries found.</div>`;
  const map = {};
  list.forEach(i => {
    const d = i.date || 'Undated';
    if (!map[d]) map[d] = [];
    map[d].push(i);
  });
  const dates = Object.keys(map).sort((a,b) => new Date(b) - new Date(a));
  let out = '';
  dates.forEach(d => {
    out += `<div class="sec-title">${d === today() ? 'Today · ' : ''}${fmtDate(d)}</div>`;
    out += map[d].map(item => renderCard(item, kind, isSpecial)).join('');
  });
  return out;
}

function viewDashboard() {
  const m = getMetrics(false);
  const pendingSales = (DB.sales || []).filter(s => calcSale(s).rem > 0.009).slice(0, 5);
  const pendingBuys = (DB.purchases || []).filter(p => calcPurchase(p).rem > 0.009).slice(0, 5);

  return `
  ${m.critCount > 0 ? `
  <div class="card" style="border-color:var(--red);background:var(--red-sub)">
    <b style="color:var(--red);font-size:13px">🚨 OVERDUE ALERT: ${m.critCount} entries are 60+ days overdue!</b>
  </div>` : ''}

  <div class="grid-2">
    <div class="kpi">
      <div class="lbl">To Collect (Sales)</div>
      <div class="val" style="color:var(--amber)">${money(m.rec)}</div>
    </div>
    <div class="kpi">
      <div class="lbl">To Pay (Purchases)</div>
      <div class="val" style="color:var(--red)">${money(m.pay)}</div>
    </div>
  </div>

  <div class="grid-2">
    <div class="kpi"><div class="lbl">Total Sales</div><div class="val">${money(m.rev)}</div></div>
    <div class="kpi"><div class="lbl">Total Purchases</div><div class="val">${money(m.buyTotal)}</div></div>
  </div>

  <div class="card" style="display:flex;justify-content:space-between;align-items:center">
    <div>
      <div class="lbl">Estimated Profit</div>
      <div class="val" style="font-size:20px;color:${m.profit>=0?'var(--green)':'var(--red)'}">${money(m.profit)}</div>
    </div>
    <button class="btn-sm" id="btn-export-quick">Export Excel</button>
  </div>

  <div class="sec-title">Pending Collections</div>
  ${pendingSales.length ? pendingSales.map(s => renderCard(s, 'sale', false)).join('') : '<div class="card" style="color:var(--muted);font-size:12px">No outstanding sales.</div>'}

  <div class="sec-title">Pending Supplier Dues</div>
  ${pendingBuys.length ? pendingBuys.map(p => renderCard(p, 'purchase', false)).join('') : '<div class="card" style="color:var(--muted);font-size:12px">No outstanding payables.</div>'}
  `;
}

function viewSales() {
  const m = getMetrics(false);
  const q = searchFilter.trim().toLowerCase();
  const filtered = (DB.sales || []).filter(s => {
    return (s.inv || '').toLowerCase().includes(q) || (s.cust || '').toLowerCase().includes(q) || (s.items || []).some(i => (i.name || '').toLowerCase().includes(q));
  }).sort((a,b) => new Date(b.date) - new Date(a.date));

  return `
  <div class="grid-2">
    <div class="kpi"><div class="lbl">Sales Turnover</div><div class="val">${money(m.rev)}</div></div>
    <div class="kpi"><div class="lbl">Receivable</div><div class="val" style="color:var(--amber)">${money(m.rec)}</div></div>
  </div>
  <input type="text" id="inp-search" placeholder="Search customer, invoice, product..." value="${esc(searchFilter)}" style="margin-bottom:8px">
  <div>${renderGroup(filtered, 'sale', false)}</div>
  `;
}

function viewPurchases() {
  const m = getMetrics(false);
  const q = searchFilter.trim().toLowerCase();
  const filtered = (DB.purchases || []).filter(p => {
    return (p.inv || '').toLowerCase().includes(q) || (p.supplier || '').toLowerCase().includes(q) || (p.items || []).some(i => (i.name || '').toLowerCase().includes(q));
  }).sort((a,b) => new Date(b.date) - new Date(a.date));

  return `
  <div class="grid-2">
    <div class="kpi"><div class="lbl">Purchases Total</div><div class="val">${money(m.buyTotal)}</div></div>
    <div class="kpi"><div class="lbl">Payable</div><div class="val" style="color:var(--red)">${money(m.pay)}</div></div>
  </div>
  <input type="text" id="inp-search" placeholder="Search supplier, bill, item..." value="${esc(searchFilter)}" style="margin-bottom:8px">
  <div>${renderGroup(filtered, 'purchase', false)}</div>
  `;
}

function viewSpecial() {
  const m = getMetrics(true);
  const q = searchFilter.trim().toLowerCase();
  const list = spSubTab === 'sales' ? DB.specialSales : DB.specialPurchases;
  const filtered = (list || []).filter(item => {
    const party = (spSubTab === 'sales' ? item.cust : item.supplier) || '';
    return (item.inv || '').toLowerCase().includes(q) || party.toLowerCase().includes(q) || (item.items || []).some(i => (i.name || '').toLowerCase().includes(q));
  }).sort((a,b) => new Date(b.date) - new Date(a.date));

  return `
  <div class="card" style="border-left:3px solid var(--purple)">
    <b style="color:var(--purple);font-size:13px">★ SPECIAL LEDGER</b>
    <div style="font-size:12px;color:var(--muted)">Separate records completely isolated from general business.</div>
  </div>

  <div class="grid-2">
    <div class="kpi">
      <div class="lbl">Special Sales</div>
      <div class="val">${money(m.rev)}</div>
      <small style="color:var(--muted)">Rec: ${money(m.rec)}</small>
    </div>
    <div class="kpi">
      <div class="lbl">Special Purchases</div>
      <div class="val">${money(m.buyTotal)}</div>
      <small style="color:var(--muted)">Pay: ${money(m.pay)}</small>
    </div>
  </div>

  <div class="card" style="display:flex;justify-content:space-between;align-items:center;margin-bottom:8px">
    <div class="lbl">Special Net Profit</div>
    <div class="val" style="font-size:18px;color:${m.profit>=0?'var(--green)':'var(--red)'}">${money(m.profit)}</div>
  </div>

  <div class="seg-bar">
    <button class="${spSubTab==='sales'?'active':''}" id="sp-tab-sales">Special Sales (${(DB.specialSales||[]).length})</button>
    <button class="${spSubTab==='purchases'?'active':''}" id="sp-tab-purchases">Special Purchases (${(DB.specialPurchases||[]).length})</button>
  </div>

  <input type="text" id="inp-search" placeholder="Search special operations..." value="${esc(searchFilter)}" style="margin-bottom:8px">
  <div>${renderGroup(filtered, spSubTab === 'sales' ? 'sale' : 'purchase', true)}</div>
  `;
}

function viewSettings() {
  return `
  <div class="card">
    <div style="font-weight:700;margin-bottom:8px">Preferences</div>
    <div class="grid-2">
      <div>
        <label>Theme</label>
        <select id="cfg-theme">
          <option value="system" ${DB.theme==='system'?'selected':''}>System Default</option>
          <option value="light" ${DB.theme==='light'?'selected':''}>Light</option>
          <option value="dark" ${DB.theme==='dark'?'selected':''}>Dark</option>
        </select>
      </div>
      <div>
        <label>Currency</label>
        <input id="cfg-cur" value="${esc(DB.cur)}">
      </div>
    </div>
  </div>

  <div class="card">
    <div style="font-weight:700">Firebase Realtime Sync</div>
    <div style="font-size:12px;color:var(--muted);margin-bottom:6px">Connect database URL to sync data instantly with your dad's app.</div>
    <input id="cfg-firebase" placeholder="https://database.firebaseio.com" value="${esc(DB.firebaseUrl||'')}">
    <button class="btn btn-sub" id="btn-save-firebase">Connect Firebase</button>
  </div>

  <div class="card">
    <div style="font-weight:700;margin-bottom:8px">Backup & Export</div>
    <button class="btn btn-sub" id="btn-export-full">Export Complete Excel Workbook</button>
    <button class="btn btn-sub" id="btn-copy-json">Copy JSON Backup String</button>
    <button class="btn btn-sub" id="btn-paste-json">Restore From JSON String</button>
  </div>
  `;
}

function render() {
  const main = $('#main');
  if (currentTab === 'home') main.innerHTML = viewDashboard();
  else if (currentTab === 'sales') main.innerHTML = viewSales();
  else if (currentTab === 'purchases') main.innerHTML = viewPurchases();
  else if (currentTab === 'special') main.innerHTML = viewSpecial();
  else if (currentTab === 'settings') main.innerHTML = viewSettings();

  document.querySelectorAll('nav button').forEach(b => {
    b.classList.toggle('active', b.getAttribute('data-tab') === currentTab);
  });
}

function showModal(html) {
  $('#modal-content').innerHTML = html;
  $('#modal').classList.add('open');
}
function hideModal() {
  $('#modal').classList.remove('open');
}

function renderDraftRows(isSale) {
  const cont = $('#draft-wrap');
  if (!cont) return;
  cont.innerHTML = draftItems.map((item, idx) => `
    <div class="item-draft">
      <div style="display:flex;justify-content:space-between;align-items:center;margin-bottom:4px">
        <span style="font-size:11px;font-weight:700">Item #${idx+1}</span>
        ${draftItems.length > 1 ? `<button class="btn-sm" style="color:var(--red)" data-act="del-draft-row" data-idx="${idx}">✕</button>` : ''}
      </div>
      <input placeholder="Product / Item name" data-act="inp-draft-name" data-idx="${idx}" value="${esc(item.name)}">
      ${isSale ? `
      <div style="display:grid;grid-template-columns:1fr 1fr 1fr;gap:4px;margin-top:4px">
        <input type="number" inputmode="decimal" placeholder="Qty" data-act="inp-draft-qty" data-idx="${idx}" value="${item.qty}">
        <input type="number" inputmode="decimal" placeholder="Cost" data-act="inp-draft-cost" data-idx="${idx}" value="${item.cost}">
        <input type="number" inputmode="decimal" placeholder="Price" data-act="inp-draft-price" data-idx="${idx}" value="${item.price}">
      </div>` : `
      <div class="grid-2" style="margin-top:4px">
        <input type="number" inputmode="decimal" placeholder="Quantity" data-act="inp-draft-qty" data-idx="${idx}" value="${item.qty}">
        <input type="number" inputmode="decimal" placeholder="Price" data-act="inp-draft-price" data-idx="${idx}" value="${item.price}">
      </div>`}
    </div>
  `).join('');
}

function openSaleForm(id = null, isSpecial = false) {
  const list = isSpecial ? DB.specialSales : DB.sales;
  const s = id ? list.find(x => x.id === id) : null;
  draftItems = (s && s.items && s.items.length) ? JSON.parse(JSON.stringify(s.items)) : [{ name: '', qty: 1, cost: '', price: '' }];

  showModal(`
  <div class="modal-head">
    <h3>${id ? 'Edit Sale' : (isSpecial ? '★ New Special Sale' : 'New Sale')}</h3>
    <button class="modal-close" data-act="close-modal">✕</button>
  </div>
  <div class="grid-2">
    <div><label>Invoice Number</label><input id="form-inv" placeholder="e.g. 1042" value="${esc(s ? s.inv : '')}"></div>
    <div><label>Sale Type</label>
      <select id="form-type">
        <option value="Credit" ${s && s.type==='Credit'?'selected':''}>Credit</option>
        <option value="Cash" ${s && s.type==='Cash'?'selected':''}>Cash</option>
      </select>
    </div>
  </div>
  <div class="grid-2">
    <div><label>Customer Name</label><input id="form-party" placeholder="Name" value="${esc(s ? s.cust : '')}"></div>
    <div><label>Date</label><input id="form-date" type="date" value="${s ? s.date : today()}"></div>
  </div>
  <div><label>Due Date (Optional)</label><input id="form-due" type="date" value="${s ? (s.due||'') : ''}"></div>

  <div style="display:flex;justify-content:space-between;align-items:center;margin:10px 0 4px">
    <span style="font-size:11px;font-weight:700;color:var(--muted);text-transform:uppercase">Line Items</span>
    <button class="btn-sm" id="btn-add-line">+ Add Item</button>
  </div>
  <div id="draft-wrap"></div>

  ${!id ? `<label>Initial Received Payment</label><input id="form-init-pay" type="number" inputmode="decimal" placeholder="0">` : ''}
  <button class="btn ${isSpecial?'btn-purple':''}" id="btn-save-sale" data-id="${id||''}" data-sp="${isSpecial}">Save Entry</button>
  `);

  renderDraftRows(true);
}

function openPurchaseForm(id = null, isSpecial = false) {
  const list = isSpecial ? DB.specialPurchases : DB.purchases;
  const p = id ? list.find(x => x.id === id) : null;
  draftItems = (p && p.items && p.items.length) ? JSON.parse(JSON.stringify(p.items)) : [{ name: '', qty: 1, price: '' }];

  showModal(`
  <div class="modal-head">
    <h3>${id ? 'Edit Purchase' : (isSpecial ? '★ New Special Purchase' : 'New Purchase')}</h3>
    <button class="modal-close" data-act="close-modal">✕</button>
  </div>
  <div class="grid-2">
    <div><label>Bill / Ref #</label><input id="form-inv" placeholder="e.g. B-99" value="${esc(p ? p.inv : '')}"></div>
    <div><label>Type</label>
      <select id="form-type">
        <option value="Credit" ${p && p.type==='Credit'?'selected':''}>Credit</option>
        <option value="Cash" ${p && p.type==='Cash'?'selected':''}>Cash</option>
      </select>
    </div>
  </div>
  <div class="grid-2">
    <div><label>Supplier Name</label><input id="form-party" placeholder="Name" value="${esc(p ? p.supplier : '')}"></div>
    <div><label>Date</label><input id="form-date" type="date" value="${p ? p.date : today()}"></div>
  </div>
  <div><label>Due Date (Optional)</label><input id="form-due" type="date" value="${p ? (p.due||'') : ''}"></div>

  <div style="display:flex;justify-content:space-between;align-items:center;margin:10px 0 4px">
    <span style="font-size:11px;font-weight:700;color:var(--muted);text-transform:uppercase">Line Items</span>
    <button class="btn-sm" id="btn-add-line">+ Add Item</button>
  </div>
  <div id="draft-wrap"></div>

  ${!id ? `<label>Initial Payment Made</label><input id="form-init-pay" type="number" inputmode="decimal" placeholder="0">` : ''}
  <button class="btn ${isSpecial?'btn-purple':''}" id="btn-save-purchase" data-id="${id||''}" data-sp="${isSpecial}">Save Entry</button>
  `);

  renderDraftRows(false);
}

function openDetail(kind, id, isSpecial = false) {
  const isSale = kind === 'sale';
  const list = isSale ? (isSpecial ? DB.specialSales : DB.sales) : (isSpecial ? DB.specialPurchases : DB.purchases);
  const entry = list.find(x => x.id === id);
  if (!entry) return;
  if (!Array.isArray(entry.pays)) entry.pays = [];

  const c = isSale ? calcSale(entry) : calcPurchase(entry);
  const title = isSale ? (entry.cust || 'Direct Customer') : (entry.supplier || 'Direct Supplier');
  const total = isSale ? c.rev : c.total;

  showModal(`
  <div class="modal-head">
    <div>
      <span class="pill ${isSpecial?'pill-sp':''}">${isSpecial?'★ Special ':''}${isSale?'Sale':'Purchase'}</span>
      <h3 style="margin-top:2px">${esc(title)}</h3>
    </div>
    <button class="modal-close" data-act="close-modal">✕</button>
  </div>

  <div style="font-size:12px;color:var(--muted);margin-bottom:8px">
    ${entry.inv ? `Invoice/Ref: <b>${esc(entry.inv)}</b> · ` : ''}Date: ${fmtDate(entry.date)}${entry.due ? ` · Due: ${fmtDate(entry.due)}` : ''}
  </div>

  <div class="grid-2">
    <div class="kpi"><div class="lbl">Total</div><div class="val">${money(total)}</div></div>
    <div class="kpi"><div class="lbl">Balance</div><div class="val" style="color:${c.rem>0.009?'var(--red)':'var(--green)'}">${c.rem>0.009?money(c.rem):'Settled'}</div></div>
  </div>

  <div class="sec-title">Itemized Breakdown</div>
  <div class="card" style="padding:6px 10px;margin-bottom:10px">
    ${(entry.items || []).map(it => `
      <div style="display:flex;justify-content:space-between;padding:4px 0;font-size:12px;border-bottom:1px solid var(--border)">
        <span>${esc(it.name)} ×${it.qty}</span>
        <b>${money(it.qty * it.price)}</b>
      </div>
    `).join('')}
  </div>

  <div class="sec-title">Logged Payments (${entry.pays.length})</div>
  ${entry.pays.length ? entry.pays.map((p, pIdx) => `
    <div style="display:flex;justify-content:space-between;align-items:center;padding:6px 0;border-bottom:1px solid var(--border);font-size:12px">
      <div>
        <span>${fmtDate(p.d)}</span> <span style="font-family:monospace;font-size:11px;background:var(--border);padding:1px 4px;border-radius:4px">${esc(p.method\vert{}\vert{}'Cash')}</span>${p.remark ? `<div style="font-size:11px;color:var(--muted)">${esc(p.remark)}</div>` : ''}
      </div>
      <div style="display:flex;align-items:center;gap:6px">
        <b>${money(p.a)}</b>
        <button class="btn-sm" style="color:var(--red)" data-act="del-pay" data-kind="${kind}" data-id="${id}" data-pidx="${pIdx}" data-sp="${isSpecial}">✕</button>
      </div>
    </div>
  `).join('') : '<div style="font-size:12px;color:var(--muted);padding:4px 0">No payment logs yet.</div>'}

  ${c.rem > 0.009 ? `
  <div class="card" style="margin-top:10px;background:var(--border)">
    <div style="font-weight:700;font-size:12px;margin-bottom:6px">Record Payment</div>
    <div class="grid-2">
      <div><label>Amount</label><input id="reg-pay-amount" type="number" inputmode="decimal" step="any" value="${Math.round(c.rem*100)/100}"></div>
      <div><label>Mode</label>
        <select id="reg-pay-mode">
          <option value="Cash">Cash</option>
          <option value="Cheque">Cheque</option>
          <option value="Bank Transfer">Bank Transfer</option>
        </select>
      </div>
    </div>
    <div class="grid-2">
      <div><label>Date</label><input id="reg-pay-date" type="date" value="${today()}"></div>
      <div><label>Notes / Cheque #</label><input id="reg-pay-remark" placeholder="Optional notes"></div>
    </div>
    <button class="btn" id="btn-submit-pay" data-kind="${kind}" data-id="${id}" data-sp="${isSpecial}">Record Payment</button>
  </div>` : ''}

  <div class="grid-2" style="margin-top:10px">
    <button class="btn btn-sub" data-act="edit-entry" data-kind="${kind}" data-id="${id}" data-sp="${isSpecial}">Edit Record</button>
    <button class="btn btn-del" data-act="del-entry" data-kind="${kind}" data-id="${id}" data-sp="${isSpecial}">Delete Record</button>
  </div>
  `);
}

document.addEventListener('click', function(e) {
  const navBtn = e.target.closest('nav button');
  if (navBtn) {
    currentTab = navBtn.getAttribute('data-tab');
    searchFilter = '';
    render();
    $('#main').scrollTop = 0;
    return;
  }

  if (e.target.closest('#dock-sale')) { openSaleForm(null, false); return; }
  if (e.target.closest('#dock-buy')) { openPurchaseForm(null, false); return; }
  if (e.target.closest('#dock-sp')) {
    showModal(`
      <div class="modal-head">
        <h3>New Special Operation</h3>
        <button class="modal-close" data-act="close-modal">✕</button>
      </div>
      <p style="font-size:12px;color:var(--muted)">Select record type for the isolated special ledger:</p>
      <button class="btn btn-purple" onclick="hideModal(); openSaleForm(null, true)">★ Add Special Sale</button>
      <button class="btn btn-sub" onclick="hideModal(); openPurchaseForm(null, true)">★ Add Special Purchase</button>
    `);
    return;
  }

  if (e.target.closest('#btn-theme')) {
    const cur = document.documentElement.getAttribute('data-theme');
    const target = cur === 'dark' ? 'light' : 'dark';
    DB.theme = target;
    applyTheme();
    saveDB();
    return;
  }

  if (e.target.closest('[data-act="close-modal"]')) { hideModal(); return; }
  if (e.target.id === 'modal') { hideModal(); return; }

  if (e.target.id === 'sp-tab-sales') { spSubTab = 'sales'; render(); return; }
  if (e.target.id === 'sp-tab-purchases') { spSubTab = 'purchases'; render(); return; }

  const entryCard = e.target.closest('[data-act="open-detail"]');
  if (entryCard) {
    const kind = entryCard.getAttribute('data-kind');
    const id = entryCard.getAttribute('data-id');
    const isSp = entryCard.getAttribute('data-sp') === 'true';
    openDetail(kind, id, isSp);
    return;
  }

  if (e.target.closest('#btn-add-line')) {
    const isSale = $('#btn-save-sale') !== null;
    draftItems.push(isSale ? { name: '', qty: 1, cost: '', price: '' } : { name: '', qty: 1, price: '' });
    renderDraftRows(isSale);
    return;
  }

  const delRow = e.target.closest('[data-act="del-draft-row"]');
  if (delRow) {
    const idx = Number(delRow.getAttribute('data-idx'));
    if (draftItems.length > 1) {
      draftItems.splice(idx, 1);
      const isSale = $('#btn-save-sale') !== null;
      renderDraftRows(isSale);
    }
    return;
  }

  const saveSaleBtn = e.target.closest('#btn-save-sale');
  if (saveSaleBtn) {
    const id = saveSaleBtn.getAttribute('data-id');
    const isSp = saveSaleBtn.getAttribute('data-sp') === 'true';
    const inv = $('#form-inv').value.trim();
    const cust = $('#form-party').value.trim();
    const type = $('#form-type').value;
    const date = $('#form-date').value || today();
    const due = $('#form-due').value || '';

    const clean = [];
    for (let it of draftItems) {
      const name = (it.name || '').trim();
      const qty = num(it.qty);
      const cost = num(it.cost);
      const price = num(it.price);
      if (!name || qty <= 0) {
        alert('Please fill in valid product names and quantities.');
        return;
      }
      clean.push({ name, qty, cost, price });
    }

    const payload = { inv, cust, type, date, due, items: clean };
    const target = isSp ? DB.specialSales : DB.sales;

    if (id) {
      const it = target.find(x => x.id === id);
      if (it) Object.assign(it, payload);
    } else {
      const initPay = num($('#form-init-pay') ? $('#form-init-pay').value : 0);
      const revTotal = clean.reduce((acc, x) => acc + (x.qty * x.price), 0);
      const amt = type === 'Cash' ? (initPay || revTotal) : initPay;
      target.push({
        ...payload,
        id: (isSp ? 'ss_' : 's_') + Date.now(),
        pays: amt > 0 ? [{ d: date, a: amt, method: 'Cash', remark: '' }] : []
      });
    }

    saveDB();
    hideModal();
    render();
    return;
  }

  const saveBuyBtn = e.target.closest('#btn-save-purchase');
  if (saveBuyBtn) {
    const id = saveBuyBtn.getAttribute('data-id');
    const isSp = saveBuyBtn.getAttribute('data-sp') === 'true';
    const inv = $('#form-inv').value.trim();
    const supplier = $('#form-party').value.trim();
    const type = $('#form-type').value;
    const date = $('#form-date').value || today();
    const due = $('#form-due').value || '';

    const clean = [];
    for (let it of draftItems) {
      const name = (it.name || '').trim();
      const qty = num(it.qty);
      const price = num(it.price);
      if (!name || qty <= 0) {
        alert('Please fill in valid item names and quantities.');
        return;
      }
      clean.push({ name, qty, price });
    }

    const payload = { inv, supplier, type, date, due, items: clean };
    const target = isSp ? DB.specialPurchases : DB.purchases;

    if (id) {
      const it = target.find(x => x.id === id);
      if (it) Object.assign(it, payload);
    } else {
      const initPay = num($('#form-init-pay') ? $('#form-init-pay').value : 0);
      const costTotal = clean.reduce((acc, x) => acc + (x.qty * x.price), 0);
      const amt = type === 'Cash' ? (initPay || costTotal) : initPay;
      target.push({
        ...payload,
        id: (isSp ? 'sp_' : 'p_') + Date.now(),
        pays: amt > 0 ? [{ d: date, a: amt, method: 'Cash', remark: '' }] : []
      });
    }

    saveDB();
    hideModal();
    render();
    return;
  }

  const editBtn = e.target.closest('[data-act="edit-entry"]');
  if (editBtn) {
    const kind = editBtn.getAttribute('data-kind');
    const id = editBtn.getAttribute('data-id');
    const isSp = editBtn.getAttribute('data-sp') === 'true';
    hideModal();
    if (kind === 'sale') openSaleForm(id, isSp);
    else openPurchaseForm(id, isSp);
    return;
  }

  const delBtn = e.target.closest('[data-act="del-entry"]');
  if (delBtn) {
    if (!confirm('Permanently delete this entry?')) return;
    const kind = delBtn.getAttribute('data-kind');
    const id = delBtn.getAttribute('data-id');
    const isSp = delBtn.getAttribute('data-sp') === 'true';
    if (kind === 'sale') {
      if (isSp) DB.specialSales = DB.specialSales.filter(x => x.id !== id);
      else DB.sales = DB.sales.filter(x => x.id !== id);
    } else {
      if (isSp) DB.specialPurchases = DB.specialPurchases.filter(x => x.id !== id);
      else DB.purchases = DB.purchases.filter(x => x.id !== id);
    }
    saveDB();
    hideModal();
    render();
    return;
  }

  const payBtn = e.target.closest('#btn-submit-pay');
  if (payBtn) {
    const kind = payBtn.getAttribute('data-kind');
    const id = payBtn.getAttribute('data-id');
    const isSp = payBtn.getAttribute('data-sp') === 'true';
    const list = kind === 'sale' ? (isSp ? DB.specialSales : DB.sales) : (isSp ? DB.specialPurchases : DB.purchases);
    const item = list.find(x => x.id === id);
    if (!item) return;

    const amt = num($('#reg-pay-amount').value);
    const c = kind === 'sale' ? calcSale(item) : calcPurchase(item);

    if (amt <= 0) { alert('Enter an amount greater than zero.'); return; }
    if (amt > c.rem + 0.1) { alert(`Amount cannot exceed balance of ${money(c.rem)}.`); return; }

    if (!Array.isArray(item.pays)) item.pays = [];
    item.pays.push({
      d: $('#reg-pay-date').value || today(),
      a: Math.round(amt * 100) / 100,
      method: $('#reg-pay-mode').value || 'Cash',
      remark: ($('#reg-pay-remark').value || '').trim()
    });

    saveDB();
    openDetail(kind, id, isSp);
    render();
    return;
  }

  const delSinglePay = e.target.closest('[data-act="del-pay"]');
  if (delSinglePay) {
    if (!confirm('Delete this payment log?')) return;
    const kind = delSinglePay.getAttribute('data-kind');
    const id = delSinglePay.getAttribute('data-id');
    const pIdx = Number(delSinglePay.getAttribute('data-pidx'));
    const isSp = delSinglePay.getAttribute('data-sp') === 'true';
    const list = kind === 'sale' ? (isSp ? DB.specialSales : DB.sales) : (isSp ? DB.specialPurchases : DB.purchases);
    const item = list.find(x => x.id === id);
    if (item && item.pays) {
      item.pays.splice(pIdx, 1);
      saveDB();
      openDetail(kind, id, isSp);
      render();
    }
    return;
  }

  if (e.target.closest('#btn-export-quick') || e.target.closest('#btn-export-full')) {
    exportExcel();
    return;
  }

  if (e.target.closest('#btn-save-firebase')) {
    const url = $('#cfg-firebase').value.trim();
    DB.firebaseUrl = url;
    saveDB();
    alert(url ? 'Firebase URL connected.' : 'Sync disconnected.');
    initFirebase();
    return;
  }

  if (e.target.closest('#btn-copy-json')) {
    navigator.clipboard.writeText(JSON.stringify(DB));
    alert('JSON backup copied to clipboard.');
    return;
  }
  if (e.target.closest('#btn-paste-json')) {
    const s = prompt('Paste your raw JSON string:');
    if (!s) return;
    try {
      const p = JSON.parse(s);
      if (Array.isArray(p.sales)) {
        DB = Object.assign(DB, p);
        saveDB();
        render();
        alert('Data restored successfully.');
      } else alert('Invalid JSON structure.');
    } catch(err) { alert('Failed to parse JSON.'); }
    return;
  }
});

document.addEventListener('input', function(e) {
  if (e.target.id === 'inp-search') {
    searchFilter = e.target.value;
    render();
    const el = $('#inp-search');
    if (el) {
      el.focus();
      el.setSelectionRange(el.value.length, el.value.length);
    }
    return;
  }

  const act = e.target.getAttribute('data-act');
  if (!act) return;
  const idx = Number(e.target.getAttribute('data-idx'));
  if (isNaN(idx) || !draftItems[idx]) return;

  if (act === 'inp-draft-name') draftItems[idx].name = e.target.value;
  else if (act === 'inp-draft-qty') draftItems[idx].qty = e.target.value;
  else if (act === 'inp-draft-cost') draftItems[idx].cost = e.target.value;
  else if (act === 'inp-draft-price') draftItems[idx].price = e.target.value;
});

document.addEventListener('change', function(e) {
  if (e.target.id === 'cfg-theme') {
    DB.theme = e.target.value;
    applyTheme();
    saveDB();
  } else if (e.target.id === 'cfg-cur') {
    DB.cur = e.target.value.trim() || 'Rs';
    saveDB();
    render();
  }
});

function applyTheme() {
  const root = document.documentElement;
  if (DB.theme === 'dark') root.setAttribute('data-theme', 'dark');
  else if (DB.theme === 'light') root.setAttribute('data-theme', 'light');
  else {
    const isDark = window.matchMedia && window.matchMedia('(prefers-color-scheme: dark)').matches;
    root.setAttribute('data-theme', isDark ? 'dark' : 'light');
  }
}

function initFirebase() {
  if (!DB.firebaseUrl || typeof firebase === 'undefined') return;
  try {
    const clean = DB.firebaseUrl.trim().replace(/\/+$/, '');
    if (!firebase.apps.length) firebase.initializeApp({ databaseURL: clean });
    fbRef = firebase.database().ref('pt_fresh_db_v3');
    fbRef.on('value', snap => {
      const val = snap.val();
      if (val) {
        if (Array.isArray(val.sales)) DB.sales = val.sales;
        if (Array.isArray(val.purchases)) DB.purchases = val.purchases;
        if (Array.isArray(val.specialSales)) DB.specialSales = val.specialSales;
        if (Array.isArray(val.specialPurchases)) DB.specialPurchases = val.specialPurchases;
        if (val.cur) DB.cur = val.cur;
        try { localStorage.setItem('pt_fresh_db_v3', JSON.stringify(DB)); } catch(e){}
        render();
      }
    });
  } catch(e) {}
}

function exportExcel() {
  if (typeof XLSX === 'undefined') { alert('Excel export engine not loaded.'); return; }
  const wb = XLSX.utils.book_new();
  const regM = getMetrics(false);
  const spM = getMetrics(true);

  const overview = [
    ['Ledger Overview Statement', ''],
    ['Generated On', today()],
    ['', ''],
    ['=== GENERAL OPERATIONS ===', ''],
    ['Total Sales', regM.rev],
    ['Total Receivables', regM.rec],
    ['Total Purchases', regM.buyTotal],
    ['Total Payables', regM.pay],
    ['Estimated Net Profit', regM.profit],
    ['', ''],
    ['=== SPECIAL OPERATIONS ===', ''],
    ['Special Sales', spM.rev],
    ['Special Receivables', spM.rec],
    ['Special Purchases', spM.buyTotal],
    ['Special Payables', spM.pay],
    ['Special Margin', spM.profit]
  ];
  XLSX.utils.book_append_sheet(wb, XLSX.utils.aoa_to_sheet(overview), 'Summary');

  function makeSheet(list, isSale) {
    const rows = [['Date', 'Ref / Inv', 'Type', 'Party', 'Items', 'Total', 'Paid', 'Balance', 'Due Date', 'Status']];
    (list || []).forEach(x => {
      const c = isSale ? calcSale(x) : calcPurchase(x);
      const str = (x.items || []).map(i => `${i.name} (x${i.qty})`).join(', ');
      rows.push([x.date, x.inv||'', x.type||'Credit', (isSale?x.cust:x.supplier)||'', str, isSale?c.rev:c.total, c.paid, c.rem, x.due||'-', c.rem<=0.009?'Settled':'Pending']);
    });
    return XLSX.utils.aoa_to_sheet(rows);
  }

  XLSX.utils.book_append_sheet(wb, makeSheet(DB.sales, true), 'Sales');
  XLSX.utils.book_append_sheet(wb, makeSheet(DB.purchases, false), 'Purchases');
  XLSX.utils.book_append_sheet(wb, makeSheet(DB.specialSales, true), 'Special Sales');
  XLSX.utils.book_append_sheet(wb, makeSheet(DB.specialPurchases, false), 'Special Purchases');

  XLSX.writeFile(wb, `Business_Ledger_${today()}.xlsx`);
}

applyTheme();
initFirebase();
render();
</script>
</body>
</html>
