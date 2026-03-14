---
layout: page
title: Task Console
permalink: /systems/task-console/
---

<style>
  :root{
    --bg: rgba(0,0,0,0.03);
    --card: rgba(0,0,0,0.02);
    --border: rgba(0,0,0,0.12);
    --dash: rgba(0,0,0,0.14);
    --text: rgba(0,0,0,0.86);
    --muted: rgba(0,0,0,0.62);

    --crescent:#4f46e5;
    --mpw:#0ea5e9;
    --personal:#f59e0b;

    --green1:#22c55e;
    --green2:#16a34a;
  }

  .cc-wrap{ margin-top: 6px; }
  .cc-title{ margin: 0 0 6px; font-weight: 1100; letter-spacing: 0.2px; }
  .cc-subtitle{ margin: 0 0 12px; color: var(--muted); font-weight: 850; font-size: 0.95em; }

  /* Tabs */
  .cc-tabs{
    display:flex; gap:8px; flex-wrap:wrap; align-items:center;
    margin: 0 0 12px;
  }
  .cc-tab{
    appearance:none;
    border:1px solid rgba(0,0,0,0.14);
    background:#fff;
    border-radius: 999px;
    padding: 7px 10px;
    cursor:pointer;
    font-weight: 1000;
    opacity: 0.85;
  }
  .cc-tab[aria-selected="true"]{
    opacity: 1;
    border-color: rgba(0,0,0,0.22);
    box-shadow: 0 1px 0 rgba(0,0,0,0.06);
  }

  /* Status line */
  .cc-status{
    display:flex; flex-wrap:wrap; gap:10px; align-items:center;
    margin: 0 0 12px;
  }
  .cc-badge{
    display:inline-flex; gap:8px; align-items:center;
    padding: 6px 10px;
    border: 1px solid rgba(0,0,0,0.14);
    border-radius: 999px;
    background: rgba(255,255,255,0.85);
    font-weight: 950;
    font-size: 0.92em;
  }
  .cc-progress{
    flex:1; min-width: 240px; height: 10px;
    border-radius: 999px; overflow:hidden;
    border: 1px solid rgba(0,0,0,0.10);
    background: rgba(0,0,0,0.10);
  }
  .cc-progress > div{
    width:0%; height:100%;
    background: linear-gradient(90deg, var(--green1), var(--green2));
  }
  .cc-meta{
    margin-left:auto;
    color: var(--muted);
    font-weight: 950;
    font-size: 0.92em;
  }

  /* Views */
  .cc-view{ display:none; }
  .cc-view.active{ display:block; }

  /* Lanes (focal) */
  .cc-lanes{
    display:grid;
    grid-template-columns: 1fr;
    gap: 14px;
  }
  @media(min-width: 980px){
    .cc-lanes{ grid-template-columns: 1fr 1fr 1fr; }
  }
  .cc-lane{
    border: 1px solid rgba(0,0,0,0.10);
    border-radius: 16px;
    background: var(--card);
    padding: 12px 12px 10px;
  }
  .cc-lanehead{
    display:flex; gap:10px; align-items:center; margin-bottom: 10px;
  }
  .cc-dot{
    width: 10px; height: 10px; border-radius: 99px;
    background: var(--crescent);
    box-shadow: 0 0 0 3px rgba(79,70,229,0.15);
  }
  .cc-lane[data-lane="MPW"] .cc-dot{
    background: var(--mpw);
    box-shadow: 0 0 0 3px rgba(14,165,233,0.18);
  }
  .cc-lane[data-lane="PERSONAL"] .cc-dot{
    background: var(--personal);
    box-shadow: 0 0 0 3px rgba(245,158,11,0.18);
  }
  .cc-lanetitle{ margin:0; font-weight: 1150; letter-spacing: 0.2px; }
  .cc-lanemeta{ margin-left:auto; font-weight: 950; color: var(--muted); font-size: 0.92em; }

  .cc-block{
    border:1px solid rgba(0,0,0,0.10);
    border-radius: 14px;
    background: rgba(255,255,255,0.45);
    padding: 10px;
    margin-top: 10px;
  }
  .cc-blockhead{
    display:flex; gap:8px; align-items:center;
    margin-bottom: 8px;
  }
  .cc-blockhead h4{ margin:0; font-weight: 1150; font-size: 0.95rem; }
  .cc-blockmeta{ margin-left:auto; font-weight: 950; color: var(--muted); font-size: 0.9em; }

  .cc-tasklist{ list-style:none; padding:0; margin:0; }
  .cc-task{
    display:grid;
    grid-template-columns: 22px 1fr;
    gap: 10px;
    padding: 8px 0;
    border-top: 1px dashed var(--dash);
    align-items:start;
  }
  .cc-task:first-child{ border-top:none; }
  .cc-task input{ transform: translateY(2px); }
  .cc-line{ white-space: pre-wrap; line-height: 1.25; color: var(--text); }
  .cc-done .cc-line{ opacity: 0.55; text-decoration: line-through; }

  .cc-rowactions{
    display:flex; flex-wrap:wrap; gap:8px;
    margin-top: 6px;
  }
  .cc-mini{
    appearance:none;
    border:1px solid rgba(0,0,0,0.14);
    background:#fff;
    border-radius: 10px;
    padding: 4px 8px;
    cursor:pointer;
    font-weight: 1000;
    font-size: 0.82em;
    opacity: 0.92;
  }
  .cc-mini:hover{ background: rgba(0,0,0,0.04); }

  .cc-add{
    display:flex; gap:8px; align-items:center; flex-wrap:wrap;
    margin-top: 10px;
  }
  .cc-add input[type="text"]{
    flex:1; min-width: 210px;
    padding: 8px 10px;
    border:1px solid rgba(0,0,0,0.18);
    background:#fff;
    border-radius: 12px;
  }
  .cc-btn{
    appearance:none;
    border:1px solid rgba(0,0,0,0.18);
    background:#fff;
    border-radius: 12px;
    padding: 8px 10px;
    cursor:pointer;
    font-weight: 1050;
  }
  .cc-btn:hover{ background: rgba(0,0,0,0.04); }

  /* Events view panels */
  .cc-panels{
    display:grid;
    grid-template-columns: 1fr;
    gap: 14px;
    margin-top: 4px;
  }
  @media(min-width: 980px){
    .cc-panels{ grid-template-columns: 1.25fr 0.75fr; }
  }
  .cc-panel{
    border: 1px solid rgba(0,0,0,0.10);
    border-radius: 16px;
    background: var(--card);
    padding: 12px;
  }
  .cc-panel h3{ margin: 0 0 10px; font-weight: 1150; }
  .cc-list{ margin:0; padding-left: 18px; color: var(--text); }
  .cc-muted{ color: var(--muted); font-weight: 850; }

  /* Actions dropdown */
  details.cc-actions{
    border: 1px solid var(--border);
    border-radius: 14px;
    background: var(--bg);
    padding: 10px 12px;
    margin: 14px 0 0;
  }
  details.cc-actions > summary{
    cursor:pointer;
    font-weight: 1100;
    list-style:none;
  }
  details.cc-actions > summary::-webkit-details-marker{ display:none; }
  .cc-actions-row{
    display:flex; flex-wrap:wrap; gap:10px; align-items:center;
    margin-top: 10px;
  }
  .cc-actions-row input[type="text"], .cc-actions-row select{
    padding: 8px 10px;
    border:1px solid rgba(0,0,0,0.18);
    background:#fff;
    border-radius: 12px;
    min-width: 160px;
  }
  .cc-actions-row label{
    display:flex; gap:8px; align-items:center;
    font-weight: 950;
    color: var(--text);
  }

  /* Backup export */
  .cc-export{
    margin-top: 14px;
    border: 1px solid var(--border);
    border-radius: 16px;
    padding: 12px;
    background: var(--card);
  }
  .cc-exporthead{
    display:flex; gap:10px; align-items:center; margin-bottom: 10px;
  }
  .cc-export textarea{
    width:100%;
    min-height: 260px;
    border-radius: 14px;
    padding: 10px;
    border: 1px solid rgba(0,0,0,0.18);
    font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono","Courier New", monospace;
    font-size: 12.5px;
    line-height: 1.35;
  }
  .cc-hidden{ display:none !important; }
</style>

<div class="cc-wrap">
  <h2 class="cc-title">🌙 Crescent Command Centre</h2>
  <div class="cc-subtitle">Three lanes. One day. No drift.</div>

  <div class="cc-tabs" role="tablist" aria-label="Console Tabs">
    <button class="cc-tab" type="button" role="tab" aria-selected="true" data-view="lanes">Command Centre</button>
    <button class="cc-tab" type="button" role="tab" aria-selected="false" data-view="events">Upcoming Events</button>
  </div>

  <div class="cc-status">
    <span class="cc-badge" id="cc-date">—</span>
    <span class="cc-badge" id="cc-summary">Progress: 0/0</span>
    <div class="cc-progress" aria-label="progress"><div id="cc-progress-bar"></div></div>
    <span class="cc-meta" id="cc-meta">Auto‑save: ON</span>
  </div>

  <!-- VIEW: LANES -->
  <div class="cc-view active" id="cc-view-lanes">
    <div class="cc-lanes" id="cc-lanes"></div>
  </div>

  <!-- VIEW: EVENTS -->
  <div class="cc-view" id="cc-view-events">
    <div class="cc-panels">
      <div class="cc-panel">
        <h3>📅 Upcoming Events</h3>
        <ul class="cc-list" id="cc-events"></ul>
        <div class="cc-muted" id="cc-events-empty">No events captured yet.</div>
      </div>
      <div class="cc-panel">
        <h3>🧭 Reserved Space</h3>
        <div class="cc-muted">
          Reminders • Inbox/Capture • Calendar sync • Projects view<br/>
          (We keep this empty on purpose for expansion.)
        </div>
      </div>
    </div>
  </div>

  <!-- ACTIONS (kept out of the way) -->
  <details class="cc-actions">
    <summary>⚙️ Actions & Filters</summary>
    <div class="cc-actions-row">
      <button class="cc-btn" type="button" id="cc-newday-btn">NEW DAY</button>
      <button class="cc-btn" type="button" id="cc-export-btn">BACKUP (EXPORT)</button>
      <button class="cc-btn" type="button" id="cc-copy-btn">COPY BACKUP</button>
      <button class="cc-btn" type="button" id="cc-reset-btn" title="Reset local state back to the seed in this file.">RESET LOCAL</button>

      <label>
        Sort
        <select id="cc-sort">
          <option value="system" selected>System</option>
          <option value="az">A–Z</option>
          <option value="written">As Written</option>
        </select>
      </label>

      <label>
        <input type="checkbox" id="cc-hide-done" />
        Hide ✅
      </label>

      <input type="text" id="cc-search" placeholder="Search…" />
    </div>
  </details>

  <!-- Export (optional) -->
  <div class="cc-export cc-hidden" id="cc-export">
    <div class="cc-exporthead">
      <strong>Backup Export (optional)</strong>
      <span style="margin-left:auto; opacity:0.7; font-weight:900;" id="cc-export-meta"></span>
    </div>
    <textarea id="cc-export-text" spellcheck="false"></textarea>
  </div>
</div>

<!-- RAW SEED CONSOLE -->
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
  const LANES = ["CRESCENT","MPW","PERSONAL"];
  const KEY_ORDER = ["👁️‍🗨️","⚠️","🔴","⚪️","🟢","🟠","🟣","🟡","⚫️","🔵","◻️"];

  // v4 forces a clean rebuild and avoids stale state bugs
  const STORE_DATA = "crescent.cc.simple.data.v4";
  const STORE_META = "crescent.cc.simple.meta.v4";
  const STORE_ARCH = "crescent.cc.simple.archive.v4";

  const elMeta = document.getElementById("cc-meta");
  const elDate = document.getElementById("cc-date");
  const elSummary = document.getElementById("cc-summary");
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

  const bar = document.getElementById("cc-progress-bar");

  const elEvents = document.getElementById("cc-events");
  const elEventsEmpty = document.getElementById("cc-events-empty");

  const raw = (document.getElementById("cc-raw")?.textContent || "").replace(/\r\n/g,"\n");

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

  // Lane inference (LOCKED):
  // - WORK/MPW -> MPW
  // - SOCIAL -> PERSONAL (unless manually moved)
  // - Old "PERSONAL" section = life ops -> CRESCENT (your umbrella)
  // - Everything else -> CRESCENT
  const inferLaneFromSection = (sectionTitle) => {
    const t = (sectionTitle || "").toLowerCase().trim();
    if (t.includes("work") || t.includes("mpw")) return "MPW";
    if (t.includes("social")) return "PERSONAL";
    if (t === "personal") return "CRESCENT";
    return "CRESCENT";
  };

  const sectionIsTodaysTasks = (sectionTitle) => (sectionTitle || "").toLowerCase().includes("today") && (sectionTitle || "").toLowerCase().includes("tasks");
  const lineHasTodayMarker = (line) => (line || "").includes(`(**${todayWeekday()}**)`) || (line || "").includes(`(**${todayWeekday().toUpperCase()}**)`);
  const isTodayTask = (t) => sectionIsTodaysTasks(t.sectionTitle) || lineHasTodayMarker(t.line);

  const importFromRaw = () => {
    const lines = raw.split("\n");
    const sections = [];
    let current = { title: "TOP", tasks: [], nonTasks: [] };
    const events = [];

    lines.forEach((line) => {
      const t = line.trim();

      if (t.startsWith("## ")) {
        sections.push(current);
        current = { title: t.replace(/^##\s*/,""), tasks: [], nonTasks: [] };
        return;
      }

      // EVENTS section: store as events, not tasks
      if ((current.title || "").toLowerCase().includes("events")) {
        if (t) events.push(t);
        return;
      }

      if (isTaskLine(line)) {
        current.tasks.push({
          id: stableId(line),
          line: t,
          done: t.startsWith("✅"),
          order: current.tasks.length,
          createdAt: Date.now(),
          sectionTitle: current.title,
          lane: inferLaneFromSection(current.title)
        });
      } else {
        current.nonTasks.push(line);
      }
    });

    sections.push(current);
    const sectionOrder = sections.map(s => s.title).filter(x => x !== "TOP");
    return { sections, sectionOrder, events };
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

  const allTasks = () => data.sections
    .filter(s => s.title !== "TOP")
    .flatMap(s => (s.tasks || []).map(t => ({...t})));

  const applySort = (tasks, mode) => {
    const copy = [...tasks];
    if (mode === "written") { copy.sort((a,b)=>(a.order??0)-(b.order??0)); return copy; }
    if (mode === "az") { copy.sort((a,b)=>(a.line||"").localeCompare(b.line||"")); return copy; }
    copy.sort((a,b)=>{
      const ra = keyRank(a.line), rb = keyRank(b.line);
      if (ra !== rb) return ra - rb;
      return (a.line||"").localeCompare(b.line||"");
    });
    return copy;
  };

  const toggleDone = (taskId, value) => {
    for (const sec of data.sections){
      if (sec.title === "TOP") continue;
      const found = (sec.tasks || []).find(x => x.id === taskId);
      if (found){ found.done = value; save(STORE_DATA, data); return true; }
    }
    return false;
  };

  const moveLane = (taskId, toLane) => {
    for (const sec of data.sections){
      if (sec.title === "TOP") continue;
      const found = (sec.tasks || []).find(x => x.id === taskId);
      if (found){ found.lane = toLane; save(STORE_DATA, data); return true; }
    }
    return false;
  };

  const pinTask = (lane, taskId) => {
    meta.pinned = meta.pinned || {CRESCENT:null,MPW:null,PERSONAL:null};
    meta.pinned[lane] = taskId;
    save(STORE_META, meta);
  };

  const updateStatus = () => {
    const tasks = allTasks();
    const total = tasks.length;
    const done = tasks.filter(t=>t.done).length;
    const pct = total ? Math.round((done/total)*100) : 0;

    elDate.textContent = meta.dateLabel || dateLabel(now());
    elSummary.textContent = `Progress: ${done}/${total}`;
    bar.style.width = pct + "%";
    elMeta.textContent = "Auto‑save: ON";
  };

  const renderEvents = () => {
    elEvents.innerHTML = "";
    const events = data.events || [];
    if (!events.length){
      elEventsEmpty.style.display = "block";
      return;
    }
    elEventsEmpty.style.display = "none";
    events.forEach(e => {
      const li = document.createElement("li");
      li.textContent = e;
      elEvents.appendChild(li);
    });
  };

  const laneCard = (lane) => {
    const card = document.createElement("div");
    card.className = "cc-lane";
    card.dataset.lane = lane;

    const head = document.createElement("div");
    head.className = "cc-lanehead";

    const dot = document.createElement("div");
    dot.className = "cc-dot";

    const title = document.createElement("h3");
    title.className = "cc-lanetitle";
    title.textContent = lane;

    const metaEl = document.createElement("div");
    metaEl.className = "cc-lanemeta";
    metaEl.id = `cc-lanemeta-${lane}`;

    head.appendChild(dot);
    head.appendChild(title);
    head.appendChild(metaEl);

    const next = document.createElement("div");
    next.className = "cc-block";
    const nextHead = document.createElement("div");
    nextHead.className = "cc-blockhead";
    nextHead.innerHTML = `<h4>NEXT</h4><div class="cc-blockmeta" id="cc-nextmeta-${lane}">—</div>`;
    const nextList = document.createElement("ul");
    nextList.className = "cc-tasklist";
    nextList.id = `cc-next-${lane}`;
    next.appendChild(nextHead);
    next.appendChild(nextList);

    const today = document.createElement("div");
    today.className = "cc-block";
    const todayHead = document.createElement("div");
    todayHead.className = "cc-blockhead";
    todayHead.innerHTML = `<h4>TODAY</h4><div class="cc-blockmeta" id="cc-todaymeta-${lane}">0/0</div>`;
    const todayList = document.createElement("ul");
    todayList.className = "cc-tasklist";
    todayList.id = `cc-today-${lane}`;
    today.appendChild(todayHead);
    today.appendChild(todayList);

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
    cb.addEventListener("change", () => { toggleDone(t.id, cb.checked); render(); });

    const box = document.createElement("div");

    const line = document.createElement("div");
    line.className = "cc-line";
    line.textContent = t.line;

    const actions = document.createElement("div");
    actions.className = "cc-rowactions";

    const pinBtn = document.createElement("button");
    pinBtn.className = "cc-mini";
    pinBtn.type = "button";
    pinBtn.textContent = "SET NEXT";
    pinBtn.addEventListener("click", () => { pinTask(laneContext, t.id); render(); });

    const moveBtn = document.createElement("button");
    moveBtn.className = "cc-mini";
    moveBtn.type = "button";
    moveBtn.textContent = "MOVE";
    moveBtn.addEventListener("click", () => {
      const idx = LANES.indexOf(t.lane);
      const nextLane = LANES[(idx + 1) % LANES.length];
      moveLane(t.id, nextLane);
      if (meta.pinned && meta.pinned[t.lane] === t.id){ meta.pinned[t.lane] = null; save(STORE_META, meta); }
      render();
    });

    actions.appendChild(pinBtn);
    actions.appendChild(moveBtn);

    box.appendChild(line);
    box.appendChild(actions);

    li.appendChild(cb);
    li.appendChild(box);
    return li;
  };

  const quickAdd = (lane, inputEl) => {
    const text = (inputEl.value || "").trim();
    if (!text) return;

    let line = text;
    if (!isTaskLine(line)) line = `— ${line} (**${todayWeekday()}**)`;

    const id = stableId(line);
    if (allTasks().some(t => t.id === id)) { elMeta.textContent = "Duplicate"; return; }

    const targetTitle = data.sectionOrder.find(s => (s||"").toLowerCase().includes("today") && (s||"").toLowerCase().includes("tasks")) || data.sectionOrder[0];
    const sec = data.sections.find(s => s.title === targetTitle);
    if (!sec) return;

    sec.tasks = sec.tasks || [];
    sec.tasks.push({
      id,
      line: line.trim(),
      done: false,
      order: sec.tasks.length,
      createdAt: Date.now(),
      sectionTitle: sec.title,
      lane
    });

    save(STORE_DATA, data);
    inputEl.value = "";
    render();
  };

  const renderLanes = () => {
    if (!elLanes.dataset.ready){
      elLanes.innerHTML = "";
      elLanes.appendChild(laneCard("CRESCENT"));
      elLanes.appendChild(laneCard("MPW"));
      elLanes.appendChild(laneCard("PERSONAL"));
      elLanes.dataset.ready = "1";
    }

    const q = (elSearch.value || "").trim().toLowerCase();
    const hide = elHide.checked;
    const sortMode = elSort.value;

    const tasks = allTasks().map(t => ({...t, lane: t.lane || inferLaneFromSection(t.sectionTitle)}));

    LANES.forEach(lane => {
      const laneTasks = tasks.filter(t => t.lane === lane);
      const laneTodayAll = laneTasks.filter(isTodayTask);
      const laneTodayDone = laneTodayAll.filter(t => t.done).length;

      const laneMeta = document.getElementById(`cc-lanemeta-${lane}`);
      if (laneMeta) laneMeta.textContent = `Today: ${laneTodayDone}/${laneTodayAll.length}`;

      const nextUl = document.getElementById(`cc-next-${lane}`);
      const todayUl = document.getElementById(`cc-today-${lane}`);
      const nextMeta = document.getElementById(`cc-nextmeta-${lane}`);
      const todayMeta = document.getElementById(`cc-todaymeta-${lane}`);

      nextUl.innerHTML = "";
      todayUl.innerHTML = "";

      let todayTasks = laneTodayAll;
      if (q) todayTasks = todayTasks.filter(t => (t.line||"").toLowerCase().includes(q));
      if (hide) todayTasks = todayTasks.filter(t => !t.done);
      todayTasks = applySort(todayTasks, sortMode);

      const pinnedId = meta?.pinned?.[lane] || null;
      let nextTask = pinnedId ? laneTasks.find(t => t.id === pinnedId) : null;

      if (nextTask && hide && nextTask.done) nextTask = null;
      if (nextTask && q && !(nextTask.line||"").toLowerCase().includes(q)) nextTask = null;
      if (!nextTask) nextTask = todayTasks.find(t => !t.done) || todayTasks[0] || null;

      if (nextTask){
        nextUl.appendChild(taskRow(nextTask, lane));
        nextMeta.textContent = nextTask.done ? "Complete ✅" : "Locked";
      } else {
        const li = document.createElement("li");
        li.className = "cc-task";
        li.innerHTML = `<div></div><div class="cc-line" style="opacity:0.7;">No NEXT task set.</div>`;
        nextUl.appendChild(li);
        nextMeta.textContent = "—";
      }

      todayTasks.forEach(t => todayUl.appendChild(taskRow(t, lane)));
      todayMeta.textContent = `${laneTodayDone}/${laneTodayAll.length}`;
    });
  };

  const render = () => {
    renderLanes();
    updateStatus();
    renderEvents();
  };

  const newDay = () => {
    const arch = load(STORE_ARCH, []);
    const from = meta.dateLabel || dateLabel(now());
    const to = dateLabel(now());

    const completed = [];
    data.sections.forEach(sec => {
      if (sec.title === "TOP") return;
      const keep = [];
      (sec.tasks || []).forEach(t => {
        if (t.done) completed.push({ id:t.id, line:t.line, lane:t.lane, section: sec.title });
        else keep.push(t);
      });
      sec.tasks = keep;
    });

    const remaining = new Set(allTasks().map(t=>t.id));
    LANES.forEach(l => { if (meta.pinned && meta.pinned[l] && !remaining.has(meta.pinned[l])) meta.pinned[l] = null; });

    meta.dateLabel = to;
    save(STORE_META, meta);

    arch.push({ rolledFrom: from, rolledTo: to, completed });
    save(STORE_ARCH, arch);
    save(STORE_DATA, data);

    exportWrap.classList.add("cc-hidden");
    render();
  };

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

    data.sectionOrder.forEach(title => {
      const sec = data.sections.find(s => s.title === title);
      if (!sec) return;
      if ((sec.title || "").toLowerCase().includes("events")) return;

      out.push(`## ${sec.title}`);
      const tasks = applySort((sec.tasks || []).map(t=>({...t})), "system");
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

    out.push("## 📅 EVENTS");
    (data.events || []).forEach(e => out.push(`${e}  `));

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
    try { await navigator.clipboard.writeText(txt); } catch {}
  };

  const resetLocal = () => {
    localStorage.removeItem(STORE_DATA);
    localStorage.removeItem(STORE_META);
    localStorage.removeItem(STORE_ARCH);
    data = ensureData();
    meta = load(STORE_META, { dateLabel: dateLabel(now()), pinned:{CRESCENT:null,MPW:null,PERSONAL:null} });
    exportWrap.classList.add("cc-hidden");
    elLanes.dataset.ready = "";
    render();
  };

  // Tabs
  const tabs = Array.from(document.querySelectorAll(".cc-tab"));
  const showView = (name) => {
    tabs.forEach(t => t.setAttribute("aria-selected", t.dataset.view === name ? "true" : "false"));
    document.getElementById("cc-view-lanes").classList.toggle("active", name === "lanes");
    document.getElementById("cc-view-events").classList.toggle("active", name === "events");
  };
  tabs.forEach(t => t.addEventListener("click", () => showView(t.dataset.view)));

  // Wire controls
  btnNewDay.addEventListener("click", newDay);
  btnExport.addEventListener("click", backupExport);
  btnCopy.addEventListener("click", copyBackup);
  btnReset.addEventListener("click", resetLocal);

  elSort.addEventListener("change", () => { render(); });
  elHide.addEventListener("change", () => { render(); });
  elSearch.addEventListener("input", () => { render(); });

  // Ensure meta exists
  meta.dateLabel = meta.dateLabel || dateLabel(now());
  save(STORE_META, meta);

  // Initial render
  render();
})();
</script>
``
