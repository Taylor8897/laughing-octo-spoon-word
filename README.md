<!DOCTYPE html>
<html lang="zh-CN">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width,initial-scale=1">
<title>艾宾浩斯记忆计划</title>
<style>
  *{box-sizing:border-box;}
  html,body{margin:0;padding:0;}
  body{
    font-family:-apple-system,BlinkMacSystemFont,"Segoe UI","PingFang SC","Hiragino Sans GB","Microsoft YaHei",sans-serif;
    background:#f5f6f8;color:#1f2328;-webkit-font-smoothing:antialiased;
    line-height:1.5;
  }
  .wrap{max-width:1080px;margin:0 auto;padding:26px 18px 70px;}
  .hidden{display:none !important;}

  /* ---------- 顶栏 ---------- */
  .topbar{display:flex;flex-wrap:wrap;gap:16px;justify-content:space-between;align-items:flex-end;margin-bottom:18px;}
  h1{font-size:21px;margin:0;letter-spacing:.2px;font-weight:700;}
  .sub{margin:6px 0 0;font-size:12.5px;color:#8b939c;}
  .datebox{display:flex;align-items:center;gap:8px;flex-wrap:wrap;}

  .pill{
    display:inline-flex;align-items:center;gap:8px;
    background:#fff;border:1px solid #e6e9ee;border-radius:999px;
    padding:8px 15px;font-size:13px;color:#3d444d;
    box-shadow:0 1px 2px rgba(16,24,40,.04);white-space:nowrap;
  }
  .pill b{color:#111827;font-weight:600;}
  .pill .dot{width:7px;height:7px;border-radius:50%;background:#22c55e;box-shadow:0 0 0 3px #dcfce7;flex:none;}
  .pill.manual .dot{background:#f59e0b;box-shadow:0 0 0 3px #fef3c7;}
  .pill .muted{color:#98a1ab;font-size:12px;}

  .date-input{
    border:1px solid #e0e5ec;background:#fff;border-radius:10px;
    padding:8px 11px;font-size:12.5px;color:#414a55;font-family:inherit;
    outline:none;cursor:pointer;transition:.15s;
  }
  .date-input:hover{border-color:#cbd5e1;}
  .date-input:focus{border-color:#93b4ff;box-shadow:0 0 0 3px rgba(59,130,246,.13);}

  /* ---------- 按钮 ---------- */
  .btn{
    border:none;border-radius:10px;padding:12px 22px;font-size:14px;font-weight:500;
    cursor:pointer;transition:.15s;font-family:inherit;white-space:nowrap;
  }
  .btn.primary{background:#2f6fed;color:#fff;box-shadow:0 1px 3px rgba(47,111,237,.32);}
  .btn.primary:hover{background:#2560d8;}
  .btn.primary:active{transform:translateY(1px);}
  .btn.ghost{background:#fff;border:1px solid #e0e5ec;color:#414a55;}
  .btn.ghost:hover{background:#f5f7fa;border-color:#cfd6e0;}
  .btn.danger{background:#fff;border:1px solid #fecaca;color:#dc2626;}
  .btn.danger:hover{background:#fef2f2;border-color:#fca5a5;}
  .btn.small{padding:7px 14px;font-size:12.5px;border-radius:8px;}

  /* ---------- 卡片 / 输入 ---------- */
  .card{
    background:#fff;border:1px solid #e9ecf1;border-radius:14px;
    box-shadow:0 1px 2px rgba(16,24,40,.04);
  }
  .input-card{display:flex;gap:10px;padding:14px;margin-bottom:18px;}
  .input-card input{
    flex:1;min-width:0;border:1px solid #e4e8ee;background:#f9fafb;border-radius:10px;
    padding:12px 15px;font-size:14.5px;outline:none;transition:.15s;
    font-family:inherit;color:#1f2328;
  }
  .input-card input::placeholder{color:#a9b1bb;}
  .input-card input:focus{background:#fff;border-color:#93b4ff;box-shadow:0 0 0 3px rgba(59,130,246,.13);}

  /* ---------- Tabs + 工具栏 ---------- */
  .tabs-bar{display:flex;justify-content:space-between;align-items:center;gap:12px;flex-wrap:wrap;margin-bottom:16px;}
  .tabs{
    display:flex;gap:4px;background:#eceef2;padding:4px;border-radius:11px;width:fit-content;
  }
  .tabs button{
    border:none;background:transparent;padding:8px 20px;border-radius:8px;
    font-size:13.5px;color:#5b6470;cursor:pointer;font-family:inherit;
    font-weight:500;transition:.15s;display:flex;align-items:center;
  }
  .tabs button.active{background:#fff;color:#1f2328;box-shadow:0 1px 3px rgba(16,24,40,.09);}
  .badge{
    display:inline-block;min-width:18px;padding:0 5px;height:18px;line-height:18px;
    text-align:center;border-radius:9px;background:#2f6fed;color:#fff;
    font-size:11px;margin-left:6px;font-weight:600;
  }
  .badge.zero{background:#c3c9d2;}
  .toolbar{display:flex;gap:8px;}

  /* ---------- 面板 ---------- */
  .panel-head{display:flex;align-items:baseline;gap:10px;margin:0 2px 12px;}
  .panel-head.mt{margin-top:24px;}
  .panel-head h2{font-size:15px;margin:0;font-weight:600;color:#2b3138;}
  .hint{font-size:12.5px;color:#98a1ab;}
  .section-label{font-size:12px;color:#8b939c;font-weight:600;margin:16px 2px 8px;letter-spacing:.4px;}
  .section-label:first-child{margin-top:4px;}

  /* ---------- 通用行 ---------- */
  .row{
    display:flex;align-items:center;gap:12px;flex-wrap:wrap;
    background:#fff;border:1px solid #e9ecf1;border-radius:12px;
    padding:13px 15px;margin-bottom:9px;transition:.18s;
    box-shadow:0 1px 2px rgba(16,24,40,.03);
  }
  .row:hover{border-color:#dbe2ec;}
  .row.is-overdue{border-color:#fde3c4;background:#fffbf5;}
  .row.is-done{opacity:.62;background:#fafbfc;}

  .tag{
    display:inline-flex;align-items:center;
    padding:5px 12px;border-radius:8px;font-size:13.5px;font-weight:600;
    background:var(--bg);color:var(--fg);border:1px solid var(--bd);
    white-space:nowrap;max-width:100%;overflow:hidden;text-overflow:ellipsis;
  }
  .tag.done{background:#f1f3f5;color:#a6adb6;border-color:#e6e9ee;text-decoration:line-through;}

  .meta{font-size:12.5px;color:#8b939c;flex:1;min-width:120px;}
  .meta em{font-style:normal;color:#e08a2b;}
  .row-actions{margin-left:auto;display:flex;align-items:center;gap:8px;}
  .done-mark{font-size:12.5px;color:#9aa1a9;font-weight:500;}

  /* ---------- 项目列表 ---------- */
  .proj-row{padding:11px 14px;}
  .proj-row .meta{min-width:150px;}
  .mini-progress{
    width:56px;height:6px;border-radius:3px;background:#eceef2;overflow:hidden;flex:none;
  }
  .mini-progress i{display:block;height:100%;border-radius:3px;transition:width .3s;}
  .prog-text{
    font-size:12px;color:#8b939c;flex:none;font-variant-numeric:tabular-nums;
    min-width:32px;text-align:right;
  }

  /* ---------- 延期菜单 ---------- */
  .delay-menu{
    width:100%;display:flex;align-items:center;gap:8px;flex-wrap:wrap;
    padding-top:11px;margin-top:2px;border-top:1px dashed #e6e9ee;
    font-size:12.5px;color:#7b838d;
  }
  .delay-opt{
    background:#f5f7fa;border:1px solid #e3e8ef;border-radius:7px;
    padding:5px 11px;font-size:12.5px;cursor:pointer;font-family:inherit;
    color:#3d444d;transition:.15s;
  }
  .delay-opt:hover{background:#e8f0ff;border-color:#b9cdfa;color:#2f6fed;}
  .delay-cancel{
    background:transparent;border:none;color:#98a1ab;font-size:12.5px;
    cursor:pointer;font-family:inherit;padding:5px 6px;
  }
  .delay-cancel:hover{color:#5b6470;}

  /* ---------- 空状态 ---------- */
  .empty{
    text-align:center;padding:52px 20px;color:#98a1ab;font-size:14px;
    background:#fff;border:1px dashed #e6e9ee;border-radius:14px;
  }
  .empty.slim{padding:34px 20px;}
  .empty .em{font-size:32px;display:block;margin-bottom:12px;line-height:1;}

  /* ---------- 日历 ---------- */
  .cal-card{padding:16px;}
  .cal-wrap{overflow-x:auto;padding-bottom:4px;}
  .cal-head{
    display:grid;grid-template-columns:repeat(7,minmax(0,1fr));gap:8px;
    margin-bottom:8px;min-width:720px;
  }
  .cal-head div{
    text-align:center;font-size:12px;color:#98a1ab;font-weight:500;padding:4px 0;
  }
  .cal-grid{
    display:grid;grid-template-columns:repeat(7,minmax(0,1fr));gap:8px;min-width:720px;
  }
  .cell{
    background:#fff;border:1px solid #eaedf2;border-radius:11px;
    min-height:98px;padding:8px;display:flex;flex-direction:column;gap:6px;
    transition:.15s;
  }
  .cell.is-past{background:#fafbfc;border-color:#f0f2f5;}
  .cell.is-today{border-color:#2f6fed;box-shadow:0 0 0 2.5px rgba(47,111,237,.13);background:#fff;}
  .cell-head{display:flex;align-items:center;gap:5px;font-size:12.5px;color:#5b6470;}
  .cell-head .num{font-weight:600;color:#2b3138;font-size:13px;}
  .cell-head .mon{font-size:10.5px;color:#98a1ab;}
  .cell.is-past .cell-head .num{color:#b9c0c9;}
  .cell-head .today-badge{
    font-size:10px;background:#2f6fed;color:#fff;border-radius:5px;padding:1px 5px;font-weight:500;
  }
  .cell-head .cnt{
    margin-left:auto;font-size:10.5px;color:#8b939c;background:#f1f3f5;
    border-radius:6px;padding:1px 6px;
  }
  .cell-body{display:flex;flex-direction:column;gap:4px;overflow-y:auto;max-height:150px;}
  .cell-body::-webkit-scrollbar{width:4px;}
  .cell-body::-webkit-scrollbar-thumb{background:#e2e6ec;border-radius:2px;}
  .mtag{
    font-size:11.5px;line-height:1.4;padding:3px 7px;border-radius:6px;
    background:var(--bg);color:var(--fg);border:1px solid var(--bd);
    white-space:nowrap;overflow:hidden;text-overflow:ellipsis;
  }
  .mtag.done{background:#f1f3f5;color:#b3bac3;border-color:#eaedf2;text-decoration:line-through;}

  .legend{margin-top:14px;font-size:12px;color:#98a1ab;display:flex;gap:16px;flex-wrap:wrap;}

  /* ---------- Toast ---------- */
  .toast{
    position:fixed;left:50%;bottom:38px;transform:translate(-50%,20px);
    background:#1f2328;color:#fff;padding:12px 22px;border-radius:10px;
    font-size:13.5px;opacity:0;pointer-events:none;transition:.28s;z-index:99;
    box-shadow:0 10px 28px rgba(0,0,0,.2);max-width:80vw;text-align:center;
  }
  .toast.show{opacity:1;transform:translate(-50%,0);}

  .store-note{margin-top:20px;text-align:center;font-size:12px;color:#a9b1bb;}

  @media (max-width:640px){
    .wrap{padding:18px 12px 60px;}
    h1{font-size:18px;}
    .sub{font-size:12px;}
    .input-card{flex-direction:column;}
    .input-card .btn{width:100%;}
    .datebox{width:100%;}
    .pill{font-size:12px;padding:7px 12px;}
    .tabs-bar{flex-direction:column;align-items:stretch;}
    .toolbar{justify-content:flex-end;}
    .row{gap:9px;}
    .row-actions{margin-left:0;width:100%;justify-content:flex-end;}
    .meta{flex-basis:100%;}
    .proj-row .meta{flex-basis:100%;}
  }
</style>
</head>
<body>
<div class="wrap">

  <header class="topbar">
    <div>
      <h1>📚 艾宾浩斯记忆计划</h1>
      <p class="sub">输入今天学习的内容，自动安排 第 1 / 2 / 4 / 7 / 15 / 30 天 的复习</p>
    </div>
    <div class="datebox">
      <div class="pill" id="todayPill"><span class="dot"></span><span id="pillText"></span></div>
      <input type="date" id="datePicker" class="date-input">
      <button class="btn ghost small hidden" id="backToday">回到今日</button>
    </div>
  </header>

  <section class="card input-card">
    <input id="taskInput" type="text" autocomplete="off"
           placeholder="输入今天学习的内容，例如：词组34-53、word list3（可用逗号一次输入多个）">
    <button class="btn primary" id="addBtn">安排复习</button>
  </section>

  <div class="tabs-bar">
    <nav class="tabs">
      <button class="active" data-tab="today">今日复习<span class="badge zero" id="todayBadge">0</span></button>
      <button data-tab="overview">总览</button>
    </nav>
    <div class="toolbar">
      <button class="btn ghost small" id="exportBtn">导出数据</button>
      <button class="btn ghost small" id="importBtn">导入数据</button>
      <input type="file" id="importFile" accept=".json,application/json" hidden>
    </div>
  </div>

  <!-- 今日复习 -->
  <main id="panelToday" class="panel">
    <div class="panel-head">
      <h2>今日复习</h2>
      <span class="hint" id="todayHint"></span>
    </div>
    <div id="todayList"></div>
  </main>

  <!-- 总览 -->
  <main id="panelOverview" class="panel hidden">
    <div class="panel-head">
      <h2>总览</h2>
      <span class="hint">自当前日期起未来一个月的复习安排</span>
    </div>
    <div class="card cal-card">
      <div class="cal-wrap">
        <div class="cal-head">
          <div>周一</div><div>周二</div><div>周三</div><div>周四</div><div>周五</div><div>周六</div><div>周日</div>
        </div>
        <div class="cal-grid" id="calGrid"></div>
      </div>
      <div class="legend">
        <span>● 彩色标签 = 需复习的记忆项目</span>
        <span>● 灰色划线 = 已打卡完成</span>
      </div>
    </div>

    <!-- 项目列表 -->
    <div class="panel-head mt">
      <h2>记忆项目</h2>
      <span class="hint" id="projHint"></span>
    </div>
    <div id="projList"></div>
    <p class="store-note">数据保存在本机浏览器中，可随时「导出数据」备份</p>
  </main>

</div>
<div class="toast" id="toast"></div>

<script>
(function(){
  'use strict';

  /* ============ 常量 ============ */
  const STORE_KEY  = 'ebbinghaus_plan_v1';
  const INTERVALS  = [1, 2, 4, 7, 15, 30];
  const STAGE_NAME = ['第1次复习','第2次复习','第3次复习','第4次复习','第5次复习','第6次复习'];
  const WEEK_CN    = '日一二三四五六';

  const PALETTE = [
    {bg:'#eef2ff', fg:'#4338ca', bd:'#c7d2fe'},
    {bg:'#ecfdf5', fg:'#047857', bd:'#a7f3d0'},
    {bg:'#fff7ed', fg:'#c2410c', bd:'#fed7aa'},
    {bg:'#fdf2f8', fg:'#be185d', bd:'#fbcfe8'},
    {bg:'#eff6ff', fg:'#1d4ed8', bd:'#bfdbfe'},
    {bg:'#f5f3ff', fg:'#6d28d9', bd:'#ddd6fe'},
    {bg:'#fefce8', fg:'#a16207', bd:'#fef08a'},
    {bg:'#f0fdfa', fg:'#0f766e', bd:'#99f6e4'},
    {bg:'#fef2f2', fg:'#b91c1c', bd:'#fecaca'},
    {bg:'#f8fafc', fg:'#334155', bd:'#cbd5e1'}
  ];

  /* ============ 日期工具（全部基于 UTC 运算，杜绝时区漂移） ============ */

  // 北京时间的今天（UTC+8），返回 'YYYY-MM-DD'
  function beijingToday(){
    const d = new Date(Date.now() + 8 * 3600 * 1000);
    return d.toISOString().slice(0, 10);
  }
  function parseDate(s){
    const p = s.split('-');
    return new Date(Date.UTC(+p[0], +p[1] - 1, +p[2]));
  }
  function fmtDate(dt){ return dt.toISOString().slice(0, 10); }
  function addDays(s, n){
    const dt = parseDate(s);
    dt.setUTCDate(dt.getUTCDate() + n);
    return fmtDate(dt);
  }
  function weekdayOf(s){ return parseDate(s).getUTCDay(); }   // 0=周日
  function shortDate(s){ const p = s.split('-'); return (+p[1]) + '/' + (+p[2]); }
  function esc(s){
    return String(s).replace(/[&<>"']/g, function(m){
      return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m];
    });
  }

  /* ============ 状态 ============ */
  let state = { items: [], colorSeq: 0 };
  let manual = false;                // 是否手动指定了查看日期
  let currentDate = beijingToday();  // 当前“查看”的日期
  let openDelay = null;              // 展开延期菜单的 key

  /* ---------- 数据规范化（兼容旧数据 / 导入数据） ---------- */
  function normalizeItem(it){
    if(!it || typeof it !== 'object') return null;
    if(typeof it.id !== 'string' || !it.id) return null;
    if(typeof it.text !== 'string' || !it.text) return null;

    const created = (typeof it.created === 'string' && /^\d{4}-\d{2}-\d{2}$/.test(it.created))
      ? it.created : beijingToday();

    let reviews = Array.isArray(it.reviews) ? it.reviews : [];
    reviews = reviews.map(function(r, k){
      const st = (r && typeof r.stage === 'number') ? r.stage : (k + 1);
      const dt = (r && typeof r.date === 'string' && /^\d{4}-\d{2}-\d{2}$/.test(r.date))
        ? r.date : addDays(created, INTERVALS[k] || 1);
      return { stage: st, date: dt, done: !!(r && r.done) };
    });
    if(!reviews.length){
      reviews = INTERVALS.map(function(gap, k){
        return { stage: k + 1, date: addDays(created, gap), done: false };
      });
    }

    const color = (typeof it.color === 'number' && it.color >= 0) ? it.color : 0;

    return { id: it.id, text: it.text, color: color, created: created, reviews: reviews };
  }

  /* ---------- 本地存储 ---------- */
  function load(){
    try{
      const raw = localStorage.getItem(STORE_KEY);
      if(raw){
        const obj = JSON.parse(raw);
        if(obj && Array.isArray(obj.items)){
          const items = obj.items.map(normalizeItem).filter(Boolean);
          state.items = items;
          state.colorSeq = (typeof obj.colorSeq === 'number' && obj.colorSeq >= 0)
            ? obj.colorSeq : items.length;
        }
      }
    }catch(e){ /* 忽略损坏数据 */ }
    if(!Array.isArray(state.items)) state.items = [];
    if(typeof state.colorSeq !== 'number') state.colorSeq = state.items.length;
  }
  function save(){
    try{ localStorage.setItem(STORE_KEY, JSON.stringify(state)); }catch(e){}
  }

  /* ============ DOM ============ */
  const $ = function(id){ return document.getElementById(id); };
  const pillEl      = $('todayPill');
  const pillTextEl  = $('pillText');
  const datePicker  = $('datePicker');
  const backToday   = $('backToday');
  const taskInput   = $('taskInput');
  const addBtn      = $('addBtn');
  const todayBadge  = $('todayBadge');
  const todayHint   = $('todayHint');
  const todayList   = $('todayList');
  const calGrid     = $('calGrid');
  const projList    = $('projList');
  const projHint    = $('projHint');
  const exportBtn   = $('exportBtn');
  const importBtn   = $('importBtn');
  const importFile  = $('importFile');
  const toastEl     = $('toast');

  /* ============ Toast ============ */
  let toastTimer = null;
  function toast(msg){
    toastEl.textContent = msg;
    toastEl.classList.add('show');
    clearTimeout(toastTimer);
    toastTimer = setTimeout(function(){ toastEl.classList.remove('show'); }, 2000);
  }

  /* ============ 渲染：顶栏 ============ */
  function renderHeader(){
    const rt = beijingToday();
    const wk = WEEK_CN[weekdayOf(currentDate)];
    if(manual){
      pillTextEl.innerHTML = '查看 <b>' + currentDate + '</b> 周' + wk +
        ' <span class="muted">· 北京今日 ' + rt + '</span>';
      pillEl.classList.add('manual');
    }else{
      pillTextEl.innerHTML = '北京时间 <b>' + currentDate + '</b> 周' + wk;
      pillEl.classList.remove('manual');
    }
    datePicker.value = currentDate;
    backToday.classList.toggle('hidden', !manual);
  }

  /* ============ 渲染：今日复习 ============ */
  function findReview(key){
    const parts = key.split('|');
    const item = state.items.find(function(i){ return i.id === parts[0]; });
    if(!item) return null;
    const idx = parseInt(parts[1], 10);
    if(isNaN(idx) || !item.reviews[idx]) return null;
    return { item: item, idx: idx, rv: item.reviews[idx] };
  }

  function reviewRowHTML(item, rv, idx, isOverdue){
    const c = PALETTE[item.color % PALETTE.length];
    const key = item.id + '|' + idx;
    const done = !!rv.done;
    const delayOpen = (openDelay === key);

    let meta = (STAGE_NAME[rv.stage - 1] || '复习') + ' · 学习于 ' + shortDate(item.created);
    if(isOverdue) meta += ' · <em>原定 ' + shortDate(rv.date) + '</em>';

    let actions = '';
    if(done){
      actions = '<span class="done-mark">✓ 已完成</span>';
    }else{
      actions =
        '<button class="btn small primary done-btn" data-key="' + key + '">打卡</button>' +
        '<button class="btn small ghost delay-btn" data-key="' + key + '">延期</button>';
    }

    let delayMenu = '';
    if(delayOpen && !done){
      let opts = '';
      for(let d = 1; d <= 3; d++){
        const target = addDays(currentDate, d);
        opts += '<button class="delay-opt" data-key="' + key + '" data-d="' + d + '">' +
                shortDate(target) + '（+' + d + '天）</button>';
      }
      delayMenu = '<div class="delay-menu"><span>延期至：</span>' + opts +
                  '<button class="delay-cancel" data-key="' + key + '">取消</button></div>';
    }

    return '<div class="row ' + (done ? 'is-done' : '') +
      (isOverdue && !done ? ' is-overdue' : '') + '">' +
      '<span class="tag ' + (done ? 'done' : '') + '" style="--bg:' + c.bg + ';--fg:' + c.fg + ';--bd:' + c.bd + '">' +
        esc(item.text) + '</span>' +
      '<span class="meta">' + meta + '</span>' +
      '<div class="row-actions">' + actions + '</div>' +
      delayMenu +
    '</div>';
  }

  function renderToday(){
    const todayItems = [];
    const overdueItems = [];

    state.items.forEach(function(item){
      item.reviews.forEach(function(rv, idx){
        if(rv.date === currentDate){
          todayItems.push({ item: item, rv: rv, idx: idx });
        }else if(rv.date < currentDate && !rv.done){
          overdueItems.push({ item: item, rv: rv, idx: idx });
        }
      });
    });

    todayItems.sort(function(a, b){
      return a.item.created.localeCompare(b.item.created) || (a.idx - b.idx);
    });
    overdueItems.sort(function(a, b){
      return a.rv.date.localeCompare(b.rv.date) || a.item.created.localeCompare(b.item.created);
    });

    const pending = todayItems.filter(function(x){ return !x.rv.done; }).length + overdueItems.length;
    todayBadge.textContent = pending;
    todayBadge.classList.toggle('zero', pending === 0);

    todayHint.textContent = todayItems.length ? ('共 ' + todayItems.length + ' 项任务') : '';

    let html = '';
    if(!todayItems.length && !overdueItems.length){
      html = '<div class="empty"><span class="em">🎉</span>' +
             currentDate + ' 暂无复习任务，放松一下吧～</div>';
    }else{
      if(overdueItems.length){
        html += '<div class="section-label">逾期未完成 · ' + overdueItems.length + '</div>';
        html += overdueItems.map(function(x){
          return reviewRowHTML(x.item, x.rv, x.idx, true);
        }).join('');
      }
      if(todayItems.length){
        if(overdueItems.length){
          html += '<div class="section-label">当日任务 · ' + todayItems.length + '</div>';
        }
        html += todayItems.map(function(x){
          return reviewRowHTML(x.item, x.rv, x.idx, false);
        }).join('');
      }
    }
    todayList.innerHTML = html;
  }

  /* ============ 渲染：总览日历 ============ */
  function renderOverview(){
    // 以 currentDate 所在周的周一为起点，共 35 天（覆盖未来一个月）
    const offset = (weekdayOf(currentDate) + 6) % 7;   // 周一 = 0
    const start = addDays(currentDate, -offset);

    const map = {};
    state.items.forEach(function(item){
      item.reviews.forEach(function(rv, idx){
        if(!map[rv.date]) map[rv.date] = [];
        map[rv.date].push({ item: item, rv: rv, idx: idx });
      });
    });

    let html = '';
    for(let i = 0; i < 35; i++){
      const d = addDays(start, i);
      const isToday = (d === currentDate);
      const isPast  = (d < currentDate);
      const list    = map[d] || [];
      const dayNum  = +d.slice(8);
      const monNum  = +d.slice(5, 7);

      let tags = '';
      list.forEach(function(entry){
        const c = PALETTE[entry.item.color % PALETTE.length];
        const done = !!entry.rv.done;
        tags += '<span class="mtag ' + (done ? 'done' : '') + '"' +
                ' title="' + esc(entry.item.text) + ' · ' +
                (STAGE_NAME[entry.rv.stage - 1] || '复习') + (done ? '（已完成）' : '') + '"' +
                ' style="--bg:' + c.bg + ';--fg:' + c.fg + ';--bd:' + c.bd + '">' +
                esc(entry.item.text) + '</span>';
      });

      html += '<div class="cell' + (isToday ? ' is-today' : '') + (isPast ? ' is-past' : '') +
        '" title="' + d + '">' +
        '<div class="cell-head">' +
          '<span class="num">' + dayNum + '</span>' +
          (dayNum === 1 ? '<span class="mon">' + monNum + '月</span>' : '') +
          (isToday ? '<span class="today-badge">今日</span>' : '') +
          (list.length ? '<span class="cnt">' + list.length + '</span>' : '') +
        '</div>' +
        '<div class="cell-body">' + tags + '</div>' +
      '</div>';
    }
    calGrid.innerHTML = html;
  }

  /* ============ 渲染：记忆项目列表 ============ */
  function isItemFinished(item){
    return item.reviews.every(function(r){ return r.done; });
  }

  function renderProjects(){
    // 只展示尚未完成全部复习的项目
    const active = state.items.filter(function(it){ return !isItemFinished(it); });

    active.sort(function(a, b){
      return a.created.localeCompare(b.created) || a.id.localeCompare(b.id);
    });

    projHint.textContent = active.length ? ('进行中 ' + active.length + ' 项') : '';

    if(!active.length){
      projList.innerHTML = '<div class="empty slim"><span class="em">📭</span>' +
        '暂无进行中的记忆项目，去上方输入框添加吧～</div>';
      return;
    }

    projList.innerHTML = active.map(function(item){
      const c = PALETTE[item.color % PALETTE.length];
      const total = item.reviews.length;
      const doneCount = item.reviews.filter(function(r){ return r.done; }).length;
      const pct = total ? Math.round(doneCount / total * 100) : 0;

      const next = item.reviews.find(function(r){ return !r.done; });
      const nextText = next
        ? ('下次 ' + shortDate(next.date) + ' · ' + (STAGE_NAME[next.stage - 1] || '复习'))
        : '已完成';

      return '<div class="row proj-row">' +
        '<span class="tag" style="--bg:' + c.bg + ';--fg:' + c.fg + ';--bd:' + c.bd + '">' +
          esc(item.text) + '</span>' +
        '<span class="meta">创建于 ' + shortDate(item.created) + ' · ' + nextText + '</span>' +
        '<span class="mini-progress" title="复习进度 ' + doneCount + '/' + total + '">' +
          '<i style="width:' + pct + '%;background:' + c.fg + '"></i>' +
        '</span>' +
        '<span class="prog-text">' + doneCount + '/' + total + '</span>' +
        '<div class="row-actions">' +
          '<button class="btn small danger del-btn" data-id="' + item.id + '">删除</button>' +
        '</div>' +
      '</div>';
    }).join('');
  }

  /* ============ 总渲染 ============ */
  function render(){
    if(!manual) currentDate = beijingToday();
    renderHeader();
    renderToday();
    renderOverview();
    renderProjects();
  }

  /* ============ 交互：添加项目 ============ */
  function doAdd(){
    const raw = taskInput.value.trim();
    if(!raw){ taskInput.focus(); return; }

    const parts = raw.split(/[,，\n;；]+/)
      .map(function(s){ return s.trim(); })
      .filter(function(s){ return s.length > 0; });
    if(!parts.length){ taskInput.focus(); return; }

    const created = currentDate;
    const stamp = Date.now().toString(36);

    parts.forEach(function(text, i){
      state.items.push({
        id: 'it_' + stamp + '_' + i + '_' + Math.random().toString(36).slice(2, 7),
        text: text,
        color: (state.colorSeq++) % PALETTE.length,
        created: created,
        reviews: INTERVALS.map(function(gap, k){
          return { stage: k + 1, date: addDays(created, gap), done: false };
        })
      });
    });

    save();
    taskInput.value = '';
    render();
    toast('已添加 ' + parts.length + ' 个记忆项目，安排 6 次复习');
  }

  addBtn.addEventListener('click', doAdd);
  taskInput.addEventListener('keydown', function(e){
    if(e.key === 'Enter'){ e.preventDefault(); doAdd(); }
  });

  /* ============ 交互：今日复习列表 ============ */
  todayList.addEventListener('click', function(e){
    // 打卡
    const doneBtn = e.target.closest('.done-btn');
    if(doneBtn){
      const f = findReview(doneBtn.dataset.key);
      if(f && !f.rv.done){
        f.rv.done = true;
        openDelay = null;
        const finished = isItemFinished(f.item);
        save(); render();
        toast(finished ? '🎉 该项目全部复习完成，已从列表移出' : '已打卡 ✓');
      }
      return;
    }
    // 展开 / 收起延期菜单
    const delayBtn = e.target.closest('.delay-btn');
    if(delayBtn){
      const key = delayBtn.dataset.key;
      openDelay = (openDelay === key) ? null : key;
      renderToday();
      return;
    }
    // 选择延期天数（当日后 3 天内）
    const opt = e.target.closest('.delay-opt');
    if(opt){
      const f = findReview(opt.dataset.key);
      if(f){
        const days = parseInt(opt.dataset.d, 10);
        f.rv.date = addDays(currentDate, days);
        openDelay = null;
        save(); render();
        toast('已延期至 ' + f.rv.date);
      }
      return;
    }
    // 取消
    const cancel = e.target.closest('.delay-cancel');
    if(cancel){
      openDelay = null;
      renderToday();
      return;
    }
  });

  /* ============ 交互：项目列表（删除） ============ */
  projList.addEventListener('click', function(e){
    const btn = e.target.closest('.del-btn');
    if(!btn) return;

    const id = btn.dataset.id;
    const item = state.items.find(function(i){ return i.id === id; });
    if(!item) return;

    if(!confirm('确定删除「' + item.text + '」吗？\n删除后对应的全部复习日程将一并移除。')) return;

    state.items = state.items.filter(function(i){ return i.id !== id; });
    openDelay = null;
    save(); render();
    toast('已删除「' + item.text + '」');
  });

  /* ============ 交互：导出数据 ============ */
  function exportData(){
    if(!state.items.length && !confirm('当前没有任何数据，仍要导出空文件吗？')) return;

    const payload = JSON.stringify({
      version: 1,
      exportedAt: new Date().toISOString(),
      colorSeq: state.colorSeq,
      items: state.items
    }, null, 2);

    const blob = new Blob([payload], { type: 'application/json;charset=utf-8' });
    const url = URL.createObjectURL(blob);
    const a = document.createElement('a');
    a.href = url;
    a.download = '艾宾浩斯记忆计划_' + beijingToday() + '.json';
    document.body.appendChild(a);
    a.click();
    document.body.removeChild(a);
    setTimeout(function(){ URL.revokeObjectURL(url); }, 1500);
    toast('数据已导出');
  }

  /* ============ 交互：导入数据 ============ */
  function importData(file){
    const reader = new FileReader();
    reader.onload = function(ev){
      let obj;
      try{
        obj = JSON.parse(ev.target.result);
      }catch(err){
        alert('导入失败：文件不是有效的 JSON 格式');
        return;
      }

      const rawItems = (obj && Array.isArray(obj.items)) ? obj.items
                     : (Array.isArray(obj) ? obj : null);
      if(!rawItems){
        alert('导入失败：文件中没有找到 items 数据');
        return;
      }

      const incoming = rawItems.map(normalizeItem).filter(Boolean);
      if(!incoming.length){
        alert('导入失败：文件中没有可用的记忆项目');
        return;
      }

      const overwrite = confirm(
        '导入 ' + incoming.length + ' 个项目：\n\n' +
        '点「确定」= 覆盖现有数据\n' +
        '点「取消」= 合并到现有数据（相同 ID 跳过）'
      );

      if(overwrite){
        state.items = incoming;
        state.colorSeq = (typeof obj.colorSeq === 'number' && obj.colorSeq >= 0)
          ? obj.colorSeq : incoming.length;
      }else{
        const exist = {};
        state.items.forEach(function(i){ exist[i.id] = true; });
        let added = 0;
        incoming.forEach(function(it){
          if(!exist[it.id]){ state.items.push(it); exist[it.id] = true; added++; }
        });
        state.colorSeq = Math.max(state.colorSeq || 0, state.items.length);
        toast('合并完成，新增 ' + added + ' 个项目');
      }

      openDelay = null;
      save(); render();
      if(overwrite) toast('已覆盖导入 ' + incoming.length + ' 个项目');
    };
    reader.onerror = function(){ alert('读取文件失败，请重试'); };
    reader.readAsText(file, 'utf-8');
  }

  exportBtn.addEventListener('click', exportData);

  importBtn.addEventListener('click', function(){ importFile.click(); });
  importFile.addEventListener('change', function(e){
    const f = e.target.files && e.target.files[0];
    if(f) importData(f);
    e.target.value = '';   // 允许重复导入同一个文件
  });

  /* ============ 交互：日期切换 ============ */
  datePicker.addEventListener('change', function(e){
    const v = e.target.value;
    if(!v) return;
    currentDate = v;
    manual = (v !== beijingToday());
    openDelay = null;
    render();
  });

  backToday.addEventListener('click', function(){
    manual = false;
    currentDate = beijingToday();
    openDelay = null;
    render();
    toast('已回到今日');
  });

  /* ============ 交互：Tab 切换 ============ */
  document.querySelectorAll('.tabs button').forEach(function(btn){
    btn.addEventListener('click', function(){
      document.querySelectorAll('.tabs button').forEach(function(b){ b.classList.remove('active'); });
      btn.classList.add('active');
      const tab = btn.dataset.tab;
      $('panelToday').classList.toggle('hidden', tab !== 'today');
      $('panelOverview').classList.toggle('hidden', tab !== 'overview');
    });
  });

  /* ============ 自动校正北京时间（跨天自动刷新） ============ */
  setInterval(function(){
    if(!manual){
      const t = beijingToday();
      if(t !== currentDate){
        currentDate = t;
        render();
      }
    }
  }, 30000);

  document.addEventListener('visibilitychange', function(){
    if(!document.hidden && !manual){
      const t = beijingToday();
      if(t !== currentDate){ currentDate = t; render(); }
    }
  });

  /* ============ 启动 ============ */
  load();
  render();

})();
</script>
</body>
</html>
