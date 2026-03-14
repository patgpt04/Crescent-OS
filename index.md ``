---
layout: page
title: Task Console
permalink: /systems/task-console/
---

<style>
  /* =========================================================
     CRESCENT COMMAND CENTRE — Local-First Task OS (Layer 2)
     - Auto-saves in browser (no GitHub paste required daily)
     - 3 Lanes for TODAY: CRESCENT / MPW / PERSONAL
     - NEXT + TODAY queue per lane
     - Optional backup export
     ========================================================= */

  :root{
    --bg: rgba(0,0,0,0.03);
    --card: rgba(0,0,0,0.02);
    --border: rgba(0,0,0,0.12);
    --border2: rgba(0,0,0,0.10);
    --dash: rgba(0,0,0,0.14);
    --txt: rgba(0,0,0,0.86);
    --muted: rgba(0,0,0,0.62);

    --green1:#22c55e;
    --green2:#16a34a;

    --crescent:#4f46e5;
    --mpw:#0ea5e9;
    --personal:#f59e0b;
  }

  .cc-wrap{ margin-top: 10px; }

  /* Top command bar */
  .cc-bar{
    display:flex; flex-wrap:wrap; gap:10px; align-items:center;
    padding:12px; border:1px solid var(--border);
    border-radius:14px; background: var(--bg);
  }
  .cc-btn{
    appearance:none; border:1px solid rgba(0,0,0,0.18);
    background:#fff; border-radius:12px;
    padding:8px 10px; cursor:pointer; font-weight:900;
    letter-spacing:0.2px;
  }
  .cc-btn:hover{ background: rgba(0,0,0,0.04); }
  .cc-bar input[type="text"], .cc-bar select{
    padding:8px 10px; border-radius:12px;
    border:1px solid rgba(0,0,0,0.18); background:#fff;
    min-width:170px;
  }
  .cc-meta{ margin-left:auto; opacity:0.8; font-weight:800; }

  /* HUD */
  .cc-hud{
    display:flex; flex-wrap:wrap; gap:10px; align-items:center;
    margin-top:10px; padding:12px;
    border:1px solid var(--border2);
    border-radius:14px; background: var(--card);
  }
  .cc-pill{
    padding:6px 10px; border-radius:999px;
    border:1px solid rgba(0,0,0,0.14);
    background:rgba(255,255,255,0.85);
    font-weight:900; font-size:0.92em;
  }
  .cc-progress{
    flex:1; min-width:220px; height:10px;
    border-radius:999px; overflow:hidden;
    background:rgba(0,0,0,0.10);
    border:1px solid rgba(0,0,0,0.10);
  }
  .cc-progress > div{
    width:0%; height:100%;
    background: linear-gradient(90deg, var(--green1), var(--green2));
  }

  /* Tabs */
  .cc-tabs{
    display:flex; flex-wrap:wrap; gap:8px; align-items:center;
    margin-top:12px;
  }
  .cc-tab{
    appearance:none; border:1px solid rgba(0,0,0,0.14);
    background:#fff; border-radius:999px;
    padding:7px 10px; cursor:pointer; font-weight:900;
    opacity:0.85;
  }
  .cc-tab[aria-selected="true"]{
    opacity:1; border-color: rgba(0,0,0,0.22);
    box-shadow: 0 1px 0 rgba(0,0,0,0.06);
  }

  /* Main grid */
  .cc-grid{ display:grid; grid-template-columns: 1fr; gap:14px; margin-top:14px; }
  @media(min-width: 980px){
    .cc-grid.cc-three{ grid-template-columns: 1fr 1fr 1fr; }
  }

  /* Lane cards */
  .cc-lane{
    border:1px solid var(--border2);
    border-radius:16px; background: var(--card);
    padding:12px 12px 10px;
  }
  .cc-lanehead{
    display:flex; gap:10px; align-items:center; margin:0 0 8px;
  }
  .cc-lanetitle{
    margin:0; font-size:1.05rem; font-weight:1000;
    letter-spacing:0.2px;
  }
  .cc-lanebadge{
    margin-left:auto;
    font-weight:900; opacity:0.75;
  }
  .cc-lanedot{
    width:10px; height:10px; border-radius:99px;
    background: var(--crescent);
    box-shadow: 0 0 0 3px rgba(79,70,229,0.15);
  }
  .cc-lane[data-lane="MPW"] .cc-lanedot{
    background: var(--mpw);
    box-shadow: 0 0 0 3px rgba(14,165,233,0.18);
  }
  .cc-lane[data-lane="PERSONAL"] .cc-lanedot{
    background: var(--personal);
    box-shadow: 0 0 0 3px rgba(245,158,11,0.18);
  }

  /* Subsections inside lane */
  .cc-sub{
    margin-top:10px;
    border:1px solid rgba(0,0,0,0.10);
    border-radius:14px; background: rgba(255,255,255,0.45);
    padding:10px;
  }
  .cc-subhead{
    display:flex; gap:8px; align-items:center; margin-bottom:8px;
  }
  .cc-subhead h4{
    margin:0; font-size:0.95rem; font-weight:1000;
  }
  .cc-submeta{ margin-left:auto; opacity:0.7; font-weight:900; font-size:0.9em; }

  .cc-tasklist{ list-style:none; padding:0; margin:0; }
  .cc-task{
    display:grid; grid-template-columns: 22px 1fr; gap:10px;
    padding:8px 0; border-top:1px dashed var(--dash);
    align-items:start;
  }
  .cc-task:first-child{ border-top:none; }

  .cc-task input{ transform: translateY(2px); }
  .cc-line{ white-space:pre-wrap; line-height:1.25; color: var(--txt); }
  .cc-done .cc-line{ opacity:0.55; text-decoration: line-through; }

  .cc-rowmeta{
    display:flex; gap:8px; align-items:center; flex-wrap:wrap;
    margin-top:4px;
  }
  .cc-chip{
    font-size:0.78em; padding:2px 8px; border-radius:999px;
    border:1px solid rgba(0,0,0,0.14); opacity:0.88;
    background:rgba(255,255,255,0.85);
    font-weight:800;
  }
  .cc-chip.lane{ border-color: rgba(0,0,0,0.18); }
  .cc-chip.lane[data-lane="CRESCENT"]{ color: #2f2acb; }
  .cc-chip.lane[data-lane="MPW"]{ color: #0277bd; }
  .cc-chip.lane[data-lane="PERSONAL"]{ color: #b45309; }

  .cc-mini{
    appearance:none; border:1px solid rgba(0,0,0,0.14);
    background:#fff; border-radius:10px;
    padding:4px 8px; cursor:pointer; font-weight:900;
    font-size: 0.82em; opacity:0.9;
  }
  .cc-mini:hover{ background: rgba(0,0,0,0.04); }

  .cc-add{
    display:flex; gap:8px; align-items:center; flex-wrap:wrap;
    margin-top:10px;
  }
  .cc-add input[type="text"]{
    flex:1; min-width:220px;
    padding:8px 10px; border-radius:12px;
    border:1px solid rgba(0,0,0,0.18); background:#fff;
  }

  /* Secondary views */
  .cc-view{ display:none; }
  .cc-view.active{ display:block; }

  /* Export */
  .cc-export{
    margin-top:14px;
    border:1px solid var(--border);
    border-radius:16px;
    padding:12px;
    background: var(--card);
  }
  .cc-exporthead{ display:flex; gap:10px; align-items:center; margin-bottom:10px; }
  .cc-export textarea{
    width:100%; min-height:260px;
    border-radius:14px; padding:10px;
    border:1px solid rgba(0,0,0,0.18);
    font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono","Courier New", monospace;
    font-size: 12.5px; line-height: 1.35;
  }
  .cc-hidden{ display:none !important; }

  /* Small helper text */
  .cc-note{
    margin-top: 10px;
    padding: 10px 12px;
    border: 1px solid rgba(0,0,0,0.10);
    border-radius: 14px;
    background: rgba(0,0,0,0.02);
    color: var(--muted);
    font-weight: 700;
  }
</style>

<div class="cc-wrap">
  <!-- COMMAND BAR -->
  <div class="cc-bar">
    <button class="cc-btn" type="button" id="cc-newday-btn" title="Roll to a new day (local). Completed tasks are archived locally.">NEW DAY</button>
    <button class="cc-btn" type="button" id="cc-export-btn" title="Optional backup export (paste into GitHub if you want).">BACKUP (EXPORT)</button>
    <button class="cc-btn" type="button" id="cc-copy-btn">COPY BACKUP</button>
    <button class="cc-btn" type="button" id="cc-reset-btn" title="Reset local state back to the seed console in this file.">RESET LOCAL</button>

    <label style="display:flex; align-items:center; gap:8px;">
      <span style="font-weight:1000;">Sort</span>
      <select id="cc-sort">
        <option value="system" selected>System (In Progress → Key Order → A–Z)</option>
        <option value="az">A–Z</option>
        <option value="written">As Written</option>
      </select>
    </label>

    <label style="display:flex; align-items:center; gap:8px;">
      <input type="checkbox" id="cc-hide-done" />
      <span style="font-weight:1000;">Hide ✅</span>
    </label>

    <input type="text" id="cc-search" placeholder="Search…" />
    <span class="cc-meta" id="cc-meta">Auto‑save: ON</span>
  </div>

  <!-- HUD -->
  <div class="cc-hud">
    <span class="cc-pill" id="cc-date">—</span>
    <span class="cc-pill" id="cc-pill-total">Tasks: 0</span>
    <span class="cc-pill" id="cc-pill-done">Done: 0</span>
    <span class="cc-pill" id="cc-pill-xp">XP: 0</span>
    <div class="cc-progress" aria-label="progress"><div id="cc-progress-bar"></div></div>
    <span class="cc-pill" id="cc-pill-level">Level: 1</span>
  </div>

  <!-- TABS -->
  <div class="cc-tabs" role="tablist" aria-label="Task Console Views">
    <button class="cc-tab" type="button" role="tab" aria-selected="true" data-view="today">Crescent Command Centre</button>
    <button class="cc-tab" type="button" role="tab" aria-selected="false" data-view="projects">Projects</button>
    <button class="cc-tab" type="button" role="tab" aria-selected="false" data-view="backlog">Backlog</button>
    <button class="cc-tab" type="button" role="tab" aria-selected="false" data-view="calendar">Calendar</button>
    <button class="cc-tab" type="button" role="tab" aria-selected="false" data-view="admin">Admin</button>
    <button class="cc-tab" type="button" role="tab" aria-selected="false" data-view="archive">Archive</button>
  </div>

  <!-- TODAY VIEW -->
  <div class="cc-view active" id="cc-view-today">
    <div class="cc-grid cc-three" id="cc-lanes"></div>

    <div class="cc-note">
      <strong>How this works:</strong> This page auto-saves as you use it (same device/browser). Backup export is optional.
      Lane assignment is automatic from your original sections, and you can manually change any task’s lane with “MOVE LANE”.
    </div>
  </div>

  <!-- OTHER VIEWS (placeholder scaffolds so we can expand later without rework) -->
  <div class="cc-view" id="cc-view-projects">
    <div class="cc-note"><strong>Projects</strong> is next. We’ll wire 👁️‍🗨️ (In Progress) into quest cards.</div>
  </div>
  <div class="cc-view" id="cc-view-backlog">
    <div class="cc-note"><strong>Backlog</strong> is next. We’ll group by domains inside Crescent (Finance/Trading/Health/Ops) + MPW + Personal.</div>
  </div>
  <div class="cc-view" id="cc-view-calendar">
    <div class="cc-note"><strong>Calendar</strong> will be fed by real calendar later (Layer 3). For now we’ll show your embedded events.</div>
  </div>
  <div class="cc-view" id="cc-view-admin">
    <div class="cc-note"><strong>Admin</strong> will surface bills/subscriptions and Sunday admin cleanly.</div>
  </div>
  <div class="cc-view" id="cc-view-archive">
    <div class="cc-note"><strong>Archive</strong> will show completed, streaks, and “wins” later.</div>
  </div>

  <!-- EXPORT -->
  <div class="cc-export cc-hidden" id="cc-export">
    <div class="cc-exporthead">
      <strong>Backup Export (optional)</strong>
      <span style="margin-left:auto; opacity:0.7; font-weight:900;" id="cc-export-meta"></span>
    </div>
    <textarea id="cc-export-text" spellcheck="false"></textarea>
  </div>
</div>

<!-- =========================================================
     RAW SEED CONSOLE (starting point; used only on first import
     or when you RESET LOCAL)
     ========================================================= -->
<script type="text/plain" id="cc-raw">
🌙 CRESCENT — TASK CONSOLE  
SATURDAY — 14 Mar 2026

## 🔑 Task Key (Authoritative Order)
👁️‍🗨️ → ⚠️ → 🔴 → ⚪️ → 🟢 → 🟠 → 🟣 → 🟡 → ⚫️ → 🔵 → ◻️ → none

---

## 🗓️ TODAY’S TASKS (Saturday)
🔴⚫️ — Create Crescent OS & OpenClaw Migration Plan (**Saturday**)  
🔴🟠 — Add Gym Schedule to Tasks (**Saturday**)  
🔴🟡 — Snapchat Check (**Saturday**)  
⚠️🟢 — Add -$150 to Bills Sheet (**Saturday**)  
🔴🟢 — Update Crunchyroll in Bills (**Saturday**)

---

## 🎯 PRIMARY FOCUS — Active Projects
👁️‍🗨️ — Perfect Crescent OS & Crescent Agent (Github & OpenClaw) (**In Progress**)  
👁️‍🗨️ — Set Up Bedroom (**In Progress**)

---

## 🌙 CRESCENT SYSTEM TASKS
⚠️🟢 — Pay Back Bills Account (**Sunday 15 March 2026**)  
🔴🟢⚫️ — Unsubscribe Basecamp ( )  
⚫️ — Add Events to Task Console ( )  
🔴⚫️ — Build Crescent Research Workflow ( )  
⚫️ — Goal Operating System ( )  
🔴⚫️ — Download OneNote on Phone ( )  
⚫️ — New Crescent Calendar ( )  
⚫️ — Road Map ( )  
⚫️ — Fix Phone Notifications ( )  
⚫️ — Create MPW Agent ( )  
⚫️ — Research Power Automate ( )  
— Learn AI Prompt ( )  
— AI Systems ( )  
— Office Spotify Playlist ( )

---

## 💻 SUNDAY ADMIN
⚪️ — Task review (**Sunday**)  
⚪️ — Iron work clothes (**Sunday**)  
⚪️ — Portfolio review (**Sunday**)  
⚪️ — Schedule review (**Sunday**)

---

## 🏠 PERSONAL
👁️‍🗨️ — Set Up Bedroom (**In Progress**)  
🔴🟣 — Apartment Lease Situation ( )  
🔴🟣 — Take Wooden Drawers Back ( )  
🟠🟣🟡 — Organise Meal Prep w/ Parents ( )  
🟣 — Move Remaining Stuff from Apartment ( )  
🟣 — Move Bedroom Around ( )  
🟣 — Car Clean ( )  
🟠 — Recover ( )  
🟠 — Book Dentist Checkup @ Yarraville Dental Clinic ( )

---

## 💼 WORK
🟡 — Ask Cam about moving my super to BT Panorama ( )  
— Office Spotify Playlist ( )  
⚫️ — Create MPW Agent ( )

---

## 🫂 SOCIAL
🔴🟡 — Text Angas List of Car Repairs ( )  
🔴🟡 — Tell Grandma about Breakup ( )

---

## 📅 EVENTS
— March OKR Catch Up w/ Cam & Thelma (**Wednesday 8 April : 10:45am – 11:15am**)
</script>

<script>
(() => {
  // =========================================================
  // Crescent Command Centre — Local-first store
  // =========================================================
  const LANES = ["CRESCENT","MPW","PERSONAL"];
  const KEY_ORDER = ["👁️‍🗨️","⚠️","🔴","⚪️","🟢","🟠","🟣","🟡","⚫️","🔵","◻️"];

  const STORE_DATA = "crescent.cc.data.v1";     // { sections:[{title, tasks:[...]}], sectionOrder:[...] }
  const STORE_META = "crescent.cc.meta.v1";     // { dateLabel, pinned:{CRESCENT:id,MPW:id,PERSONAL:id} }
  const STORE_ARCH = "crescent.cc.archive.v1";  // [{rolledFrom, rolledTo, completed:[...]}]

  // Elements
  const elMeta = document.getElementById("cc-meta");
  const elDate = document.getElementById("cc-date");
  const elSort = document.getElementById("cc-sort");
  const elHide = document.getElementById("cc-hide-done");
  const elSearch = document.getElementById("cc-search");

  const elLanes = document.getElementById("cc-lanes");

  const btnNewDay = document.getElementById("cc-newday-btn");
  const btnExport = document.getElementById("cc-export-btn");
  const btnCopy = document.getElementById("cc-copy-btn");
  const btnReset = document.getElementById("cc-reset-btn");

  const exportWrap = document.getElementById("cc-export");
  const exportText = document.getElementById("cc-export-text");
  const exportMeta = document.getElementById("cc-export-meta");

  const pillTotal = document.getElementById("cc-pill-total");
  const pillDone = document.getElementById("cc-pill-done");
  const pillXP = document.getElementById("cc-pill-xp");
  const pillLevel = document.getElementById("cc-pill-level");
  const bar = document.getElementById("cc-progress-bar");

  const raw = (document.getElementById("cc-raw")?.textContent || "").replace(/\r\n/g,"\n");

  // Helpers
  const norm = (s) => (s || "").replace(/\uFE0F/g,"");
  const load = (k, fallback) => { try { return JSON.parse(localStorage.getItem(k) || JSON.stringify(fallback)); } catch { return fallback; } };
  const save = (k, v) => localStorage.setItem(k, JSON.stringify(v));

  const now = () => new Date();
  const dayName = (d) => ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"][d.getDay()];
  const monthName = (d) => ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"][d.getMonth()];
  const dateLabel = (d) => `${dayName(d).toUpperCase()} — ${String(d.getDate()).padStart(2,"0")} ${monthName(d)} ${d.getFullYear()}`;
  const todayWeekday = () => dayName(now()); // e.g. "Saturday"

  const isTaskLine = (line) => {
    const s = (line || "").trim();
    if (!s) return false;
    if (!s.includes(" — ")) return false;
    if (!(s.includes("(**") || s.includes("( )"))) return false;
    return true;
  };

  const stableId = (line) => norm((line || "").trim().replace(/^✅\s*—\s*/,""));
  const keyRank = (line) => {
    const n = norm(line);
    if (n.includes("(**In Progress**)")) return -1;
    for (let i=0;i<KEY_ORDER.length;i++){
      if (n.includes(norm(KEY_ORDER[i]))) return i;
    }
    return 999;
  };

  const inferLaneFromSection = (sectionTitle) => {
    const t = (sectionTitle || "").toLowerCase();
    if (t.includes("work") || t.includes("mpw")) return "MPW";
    if (t.includes("social")) return "PERSONAL";
    if (t.includes("events")) return "MPW"; // your events currently are mostly work; we can refine later
    return "CRESCENT";
  };

  const lineHasTodayMarker = (line) => {
    const wd = todayWeekday(); // "Saturday"
    return (line || "").includes(`(**${wd}**)`) || (line || "").includes(`(**${wd.toUpperCase()}**)`);
  };

  const sectionIsTodaysTasks = (sectionTitle) => {
    const t = (sectionTitle || "").toLowerCase();
    return t.includes("today") && t.includes("tasks");
  };

  // Import raw seed into data model
  const importFromRaw = () => {
    const lines = raw.split("\n");
    const sections = [];
    let current = { title: "TOP", tasks: [], nonTasks: [] };

    lines.forEach((line) => {
      const t = line.trim();
      if (t.startsWith("## ")) {
        sections.push(current);
        current = { title: t.replace(/^##\s*/,""), tasks: [], nonTasks: [] };
        return;
      }
      if (isTaskLine(line)) {
        const done = t.startsWith("✅");
        const id = stableId(line);
        current.tasks.push({
          id,
          line: t,
          done,
          order: current.tasks.length,
          createdAt: Date.now(),
          section: current.title,
          lane: inferLaneFromSection(current.title)
        });
      } else {
        current.nonTasks.push(line);
      }
    });
    sections.push(current);

    // preserve written order (skip TOP)
    const sectionOrder = sections.map(s => s.title).filter(x => x !== "TOP");
    return { sections, sectionOrder };
  };

  const ensureData = () => {
    const existing = localStorage.getItem(STORE_DATA);
    if (existing) return load(STORE_DATA, null);
    const d = importFromRaw();
    save(STORE_DATA, d);

    const m = { dateLabel: dateLabel(now()), pinned: { CRESCENT:null, MPW:null, PERSONAL:null } };
    save(STORE_META, m);
    save(STORE_ARCH, []);
    return d;
  };

  let data = ensureData();
  let meta = load(STORE_META, { dateLabel: dateLabel(now()), pinned:{CRESCENT:null,MPW:null,PERSONAL:null} });

  // Flatten helpers
  const allTasks = () => data.sections
    .filter(s => s.title !== "TOP")
    .flatMap(s => s.tasks.map(t => ({...t, sectionTitle: s.title})));

  const updateHud = () => {
    const tasks = allTasks();
    const total = tasks.length;
    const done = tasks.filter(t => t.done).length;
    const xp = done * 10;
    const level = Math.max(1, Math.floor(xp/100)+1);
    const pct = total ? Math.round((done/total)*100) : 0;

    elDate.textContent = meta.dateLabel || dateLabel(now());
    pillTotal.textContent = `Tasks: ${total}`;
    pillDone.textContent = `Done: ${done}`;
    pillXP.textContent = `XP: ${xp}`;
    pillLevel.textContent = `Level: ${level}`;
    bar.style.width = pct + "%";
  };

  // Sort
  const applySort = (tasks, mode) => {
    const copy = [...tasks];
    if (mode === "written") {
      copy.sort((a,b) => (a.order ?? 0) - (b.order ?? 0));
      return copy;
    }
    if (mode === "az") {
      copy.sort((a,b)=> (a.line || "").localeCompare(b.line || ""));
      return copy;
    }
    // system
    copy.sort((a,b)=>{
      const ra = keyRank(a.line), rb = keyRank(b.line);
      if (ra !== rb) return ra - rb;
      return (a.line || "").localeCompare(b.line || "");
    });
    return copy;
  };

  // TODAY selection rules:
  // - anything under the "TODAY’S TASKS" section is today
  // - OR tasks with (**<weekday>**) marker are today
  const isTodayTask = (t) => sectionIsTodaysTasks(t.sectionTitle) || lineHasTodayMarker(t.line);

  // Pinning (NEXT)
  const pinTask = (lane, taskId) => {
    meta.pinned = meta.pinned || {CRESCENT:null,MPW:null,PERSONAL:null};
    meta.pinned[lane] = taskId;
    save(STORE_META, meta);
  };

  const moveLane = (taskId, toLane) => {
    // Find task and update lane
    for (const sec of data.sections){
      if (sec.title === "TOP") continue;
      const found = sec.tasks.find(x => x.id === taskId);
      if (found){
        found.lane = toLane;
        save(STORE_DATA, data);
        elMeta.textContent = "Auto‑save: ON ✅";
        return true;
      }
    }
    return false;
  };

  const toggleDone = (taskId, value) => {
    for (const sec of data.sections){
      if (sec.title === "TOP") continue;
      const found = sec.tasks.find(x => x.id === taskId);
      if (found){
        found.done = value;
        save(STORE_DATA, data);
        elMeta.textContent = "Auto‑save: ON ✅";
        return true;
      }
    }
    return false;
  };

  // UI Builders
  const laneCard = (lane) => {
    const card = document.createElement("div");
    card.className = "cc-lane";
    card.dataset.lane = lane;

    const head = document.createElement("div");
    head.className = "cc-lanehead";

    const dot = document.createElement("div");
    dot.className = "cc-lanedot";

    const title = document.createElement("h3");
    title.className = "cc-lanetitle";
    title.textContent = lane;

    const badge = document.createElement("div");
    badge.className = "cc-lanebadge";
    badge.id = `cc-badge-${lane}`;

    head.appendChild(dot);
    head.appendChild(title);
    head.appendChild(badge);

    // NEXT block
    const next = document.createElement("div");
    next.className = "cc-sub";
    const nextHead = document.createElement("div");
    nextHead.className = "cc-subhead";
    const nextH = document.createElement("h4");
    nextH.textContent = "NEXT";
    const nextMeta = document.createElement("div");
    nextMeta.className = "cc-submeta";
    nextMeta.id = `cc-next-meta-${lane}`;
    nextHead.appendChild(nextH);
    nextHead.appendChild(nextMeta);
    next.appendChild(nextHead);

    const nextList = document.createElement("ul");
    nextList.className = "cc-tasklist";
    nextList.id = `cc-next-${lane}`;
    next.appendChild(nextList);

    // TODAY block
    const today = document.createElement("div");
    today.className = "cc-sub";
    const todayHead = document.createElement("div");
    todayHead.className = "cc-subhead";
    const todayH = document.createElement("h4");
    todayH.textContent = "TODAY";
    const todayMeta = document.createElement("div");
    todayMeta.className = "cc-submeta";
    todayMeta.id = `cc-today-meta-${lane}`;
    todayHead.appendChild(todayH);
    todayHead.appendChild(todayMeta);
    today.appendChild(todayHead);

    const todayList = document.createElement("ul");
    todayList.className = "cc-tasklist";
    todayList.id = `cc-today-${lane}`;
    today.appendChild(todayList);

    // Quick add
    const add = document.createElement("div");
    add.className = "cc-add";

    const input = document.createElement("input");
    input.type = "text";
    input.placeholder = `Quick Add to ${lane}…`;
    input.id = `cc-add-${lane}`;

    const addBtn = document.createElement("button");
    addBtn.className = "cc-btn";
    addBtn.type = "button";
    addBtn.textContent = "ADD";
    addBtn.addEventListener("click", () => quickAdd(lane, input));

    add.appendChild(input);
    add.appendChild(addBtn);

    card.appendChild(head);
    card.appendChild(next);
    card.appendChild(today);
    card.appendChild(add);

    return card;
  };

  const taskRow = (t, laneContext) => {
    const li = document.createElement("li");
    li.className = "cc-task" + (t.done ? " cc-done" : "");

    const cb = document.createElement("input");
    cb.type = "checkbox";
    cb.checked = !!t.done;
    cb.addEventListener("change", () => {
      toggleDone(t.id, cb.checked);
      render();
    });

    const box = document.createElement("div");

    const line = document.createElement("div");
    line.className = "cc-line";
    line.textContent = t.line;

    const metaRow = document.createElement("div");
    metaRow.className = "cc-rowmeta";

    const laneChip = document.createElement("span");
    laneChip.className = "cc-chip lane";
    laneChip.dataset.lane = t.lane || laneContext;
    laneChip.textContent = t.lane || laneContext;
    metaRow.appendChild(laneChip);

    // Pin button
    const pinBtn = document.createElement("button");
    pinBtn.className = "cc-mini";
    pinBtn.type = "button";
    pinBtn.textContent = "SET NEXT";
    pinBtn.addEventListener("click", () => {
      pinTask(laneContext, t.id);
      elMeta.textContent = "Pinned NEXT ✅";
      render();
    });
    metaRow.appendChild(pinBtn);

    // Move lane button (cycles)
    const moveBtn = document.createElement("button");
    moveBtn.className = "cc-mini";
    moveBtn.type = "button";
    moveBtn.textContent = "MOVE LANE";
    moveBtn.addEventListener("click", () => {
      const current = t.lane || laneContext;
      const idx = LANES.indexOf(current);
      const nextLane = LANES[(idx + 1) % LANES.length];
      moveLane(t.id, nextLane);
      // If it was pinned in old lane, unpin
      if (meta.pinned && meta.pinned[current] === t.id) {
        meta.pinned[current] = null;
        save(STORE_META, meta);
      }
      elMeta.textContent = `Moved to ${nextLane} ✅`;
      render();
    });
    metaRow.appendChild(moveBtn);

    // Chips: show key emojis present
    KEY_ORDER.forEach(k => {
      if (norm(t.line).includes(norm(k))) {
        const chip = document.createElement("span");
        chip.className = "cc-chip";
        chip.textContent = k;
        metaRow.appendChild(chip);
      }
    });
    if (t.line.includes("(**In Progress**)")) {
      const chip = document.createElement("span");
      chip.className = "cc-chip";
      chip.textContent = "In Progress";
      metaRow.appendChild(chip);
    }

    box.appendChild(line);
    box.appendChild(metaRow);

    li.appendChild(cb);
    li.appendChild(box);
    return li;
  };

  // Quick add: adds to TODAY’S TASKS section if it exists, else first section.
  // Adds a weekday marker by default for today: (**<weekday>**)
  const quickAdd = (lane, inputEl) => {
    const text = (inputEl.value || "").trim();
    if (!text) { elMeta.textContent = "Type a task first"; return; }

    // We accept either a full task line OR a plain sentence.
    // If user typed plain sentence, we wrap it into a basic task.
    let line = text;
    if (!isTaskLine(line)) {
      // Make it a simple "—" task with today marker
      line = `— ${line} (**${todayWeekday()}**)`;
    } else {
      // Ensure it has a today marker if none present
      if (!(line.includes("(**") || line.includes("( )"))) {
        line = `${line} (**${todayWeekday()}**)`;
      }
      if (!line.includes(`(**${todayWeekday()}**)`) && !sectionIsTodaysTasks("TODAY’S TASKS")) {
        // no-op; tasks can be non-today even if added. user can edit later.
      }
    }

    // Create id and place
    const id = stableId(line);
    // Prevent duplicates by id
    if (allTasks().some(t => t.id === id)) { elMeta.textContent = "Duplicate (same text)"; return; }

    const targetTitle = data.sectionOrder.find(s => sectionIsTodaysTasks(s)) || data.sectionOrder[0];
    const sec = data.sections.find(s => s.title === targetTitle);
    if (!sec) { elMeta.textContent = "No section found"; return; }

    sec.tasks.push({
      id,
      line: line.trim(),
      done: false,
      order: sec.tasks.length,
      createdAt: Date.now(),
      section: sec.title,
      lane: lane
    });

    save(STORE_DATA, data);
    inputEl.value = "";
    elMeta.textContent = `Added to ${lane} ✅`;
    render();
  };

  // Render lanes
  const render = () => {
    // Build lanes container once
    if (!elLanes.dataset.ready) {
      elLanes.innerHTML = "";
      elLanes.appendChild(laneCard("CRESCENT"));
      elLanes.appendChild(laneCard("MPW"));
      elLanes.appendChild(laneCard("PERSONAL"));
      elLanes.dataset.ready = "1";
    }

    const q = (elSearch.value || "").trim().toLowerCase();
    const hideDone = elHide.checked;
    const sortMode = elSort.value;

    const tasks = allTasks();

    // Badge counts per lane for today
    LANES.forEach(lane => {
      const badge = document.getElementById(`cc-badge-${lane}`);
      const laneTasks = tasks.filter(t => (t.lane || inferLaneFromSection(t.sectionTitle)) === lane);
      const todayLane = laneTasks.filter(isTodayTask);
      const doneToday = todayLane.filter(t => t.done).length;
      badge.textContent = `Today: ${doneToday}/${todayLane.length}`;
    });

    // For each lane: compute NEXT + TODAY
    LANES.forEach(lane => {
      const nextUl = document.getElementById(`cc-next-${lane}`);
      const todayUl = document.getElementById(`cc-today-${lane}`);
      const nextMeta = document.getElementById(`cc-next-meta-${lane}`);
      const todayMeta = document.getElementById(`cc-today-meta-${lane}`);

      nextUl.innerHTML = "";
      todayUl.innerHTML = "";

      const laneTasks = tasks
        .map(t => ({...t, lane: t.lane || inferLaneFromSection(t.sectionTitle)}))
        .filter(t => t.lane === lane);

      let todayTasks = laneTasks.filter(isTodayTask);

      // search / hide filters
      if (q) todayTasks = todayTasks.filter(t => (t.line || "").toLowerCase().includes(q));
      if (hideDone) todayTasks = todayTasks.filter(t => !t.done);

      todayTasks = applySort(todayTasks, sortMode);

      // NEXT: pinned if exists and in lane; else first not-done today task; else empty
      const pinnedId = meta?.pinned?.[lane] || null;
      let nextTask = pinnedId ? laneTasks.find(t => t.id === pinnedId) : null;
      if (nextTask && hideDone && nextTask.done) nextTask = null;
      if (nextTask && q && !(nextTask.line || "").toLowerCase().includes(q)) nextTask = null;

      if (!nextTask) {
        nextTask = todayTasks.find(t => !t.done) || todayTasks[0] || null;
      }

      if (nextTask) {
        nextUl.appendChild(taskRow(nextTask, lane));
        nextMeta.textContent = nextTask.done ? "Complete ✅" : "Locked";
      } else {
        const li = document.createElement("li");
        li.className = "cc-task";
        li.innerHTML = `<div></div><div class="cc-line" style="opacity:0.7;">No NEXT task set.</div>`;
        nextUl.appendChild(li);
        nextMeta.textContent = "—";
      }

      // TODAY list
      todayTasks.forEach(t => todayUl.appendChild(taskRow(t, lane)));
      const totalLaneToday = laneTasks.filter(isTodayTask).length;
      const doneLaneToday = laneTasks.filter(isTodayTask).filter(t => t.done).length;
      todayMeta.textContent = `${doneLaneToday}/${totalLaneToday} ✅`;

    });

    updateHud();
  };

  // NEW DAY: archives completed tasks (local), clears pinned if completed, updates date label
  const newDay = () => {
    const arch = load(STORE_ARCH, []);
    const from = meta.dateLabel || dateLabel(now());
    const to = dateLabel(now());

    const completed = [];
    data.sections.forEach(sec => {
      if (sec.title === "TOP") return;
      const keep = [];
      sec.tasks.forEach(t => {
        if (t.done) completed.push({ id: t.id, line: t.line, lane: t.lane, section: sec.title });
        else keep.push(t);
      });
      sec.tasks = keep;
    });

    // Clear pinned if it no longer exists
    const remainingIds = new Set(allTasks().map(t => t.id));
    LANES.forEach(l => {
      if (meta.pinned && meta.pinned[l] && !remainingIds.has(meta.pinned[l])) meta.pinned[l] = null;
    });

    meta.dateLabel = to;
    save(STORE_META, meta);

    arch.push({ rolledFrom: from, rolledTo: to, completed });
    save(STORE_ARCH, arch);

    save(STORE_DATA, data);
    exportWrap.classList.add("cc-hidden");
    elMeta.textContent = "NEW DAY ✅ (archived locally)";
    render();
  };

  // Backup export (optional): builds a paste-ready markdown BODY.
  // NOTE: This is a backup/snapshot. Daily use does not require this.
  const backupExport = () => {
    const out = [];
    out.push("🌙 CRESCENT — TASK CONSOLE  ");
    out.push((meta.dateLabel || dateLabel(now())));
    out.push("");
    out.push("## 🔑 Task Key (Authoritative Order)");
    out.push("👁️‍🗨️ → ⚠️ → 🔴 → ⚪️ → 🟢 → 🟠 → 🟣 → 🟡 → ⚫️ → 🔵 → ◻️ → none");
    out.push("");
    out.push("---");
    out.push("");

    // Export sections in original order; tasks marked ✅ if done
    data.sectionOrder.forEach(title => {
      const sec = data.sections.find(s => s.title === title);
      if (!sec) return;
      out.push(`## ${title}`);

      const tasks = applySort(sec.tasks.map(t => ({...t})), "system");
      tasks.forEach(t => {
        const s = (t.line || "").trim();
        if (t.done) {
          if (s.startsWith("✅")) out.push(s + "  ");
          else {
            const idx = s.indexOf(" — ");
            const rest = idx >= 0 ? s.slice(idx+3) : s;
            out.push(`✅ — ${rest}  `);
          }
        } else {
          // ensure not ✅
          if (s.startsWith("✅")) {
            const idx = s.indexOf(" — ");
            const rest = idx >= 0 ? s.slice(idx+3) : s.replace(/^✅\s*/,"");
            out.push(`— ${rest}  `);
          } else out.push(s + "  ");
        }
      });

      out.push("");
      out.push("---");
      out.push("");
    });

    while (out.length && out[out.length-1].trim()==="") out.pop();
    const txt = out.join("\n");
    exportText.value = txt;
    exportWrap.classList.remove("cc-hidden");
    exportMeta.textContent = "Optional backup. Daily use = auto‑save.";
    return txt;
  };

  const copyBackup = async () => {
    const txt = backupExport();
    exportText.focus(); exportText.select();
    try {
      await navigator.clipboard.writeText(txt);
      elMeta.textContent = "Copied backup ✅";
    } catch {
      elMeta.textContent = "Select + Copy (Ctrl/Cmd+C) ✅";
    }
  };

  const resetLocal = () => {
    localStorage.removeItem(STORE_DATA);
    localStorage.removeItem(STORE_META);
    localStorage.removeItem(STORE_ARCH);
    data = ensureData();
    meta = load(STORE_META, { dateLabel: dateLabel(now()), pinned:{CRESCENT:null,MPW:null,PERSONAL:null} });
    exportWrap.classList.add("cc-hidden");
    elMeta.textContent = "Reset local state ✅";
    // force lane container rebuild
    elLanes.dataset.ready = "";
    render();
  };

  // Tabs wiring
  const tabs = Array.from(document.querySelectorAll(".cc-tab"));
  const showView = (name) => {
    tabs.forEach(t => t.setAttribute("aria-selected", t.dataset.view === name ? "true" : "false"));
    document.querySelectorAll(".cc-view").forEach(v => v.classList.remove("active"));
    const target = document.getElementById(`cc-view-${name}`);
    if (target) target.classList.add("active");
  };
  tabs.forEach(t => t.addEventListener("click", () => showView(t.dataset.view)));

  // Events
  elSort.addEventListener("change", render);
  elHide.addEventListener("change", render);
  elSearch.addEventListener("input", render);

  btnNewDay.addEventListener("click", newDay);
  btnExport.addEventListener("click", backupExport);
  btnCopy.addEventListener("click", copyBackup);
  btnReset.addEventListener("click", resetLocal);

  // Initial
  // Keep date label always current unless you want it fixed: we set it to real-world date on load.
  meta.dateLabel = meta.dateLabel || dateLabel(now());
  // If meta date is stale (different from today), keep it — NEW DAY is the explicit roll.
  save(STORE_META, meta);

  render();
})();
</script>
