---
layout: page
title: Task Console
permalink: /systems/task-console/
---

<style>
  /* Crescent OS — Task Console UI */
  .tc-wrap { margin-top: 10px; }
  .tc-bar {
    display:flex; flex-wrap:wrap; gap:10px; align-items:center;
    padding:12px 12px; border:1px solid rgba(0,0,0,0.12);
    border-radius:12px; background:rgba(0,0,0,0.03);
  }
  .tc-btn {
    appearance:none; border:1px solid rgba(0,0,0,0.18);
    background:#fff; border-radius:10px;
    padding:7px 10px; cursor:pointer; font-weight:800;
    letter-spacing:0.2px;
  }
  .tc-btn:hover { background: rgba(0,0,0,0.04); }
  .tc-bar input[type="text"], .tc-bar select{
    padding:7px 10px; border-radius:10px;
    border:1px solid rgba(0,0,0,0.18); background:#fff;
    min-width:160px;
  }
  .tc-meta { margin-left:auto; opacity:0.75; font-size:0.95em; }

  .tc-hud{
    display:flex; flex-wrap:wrap; gap:10px; align-items:center;
    margin-top:10px; padding:12px;
    border:1px solid rgba(0,0,0,0.10);
    border-radius:12px; background:rgba(0,0,0,0.02);
  }
  .tc-pill{
    padding:6px 10px; border-radius:999px;
    border:1px solid rgba(0,0,0,0.14);
    background:rgba(255,255,255,0.85);
    font-weight:900; font-size:0.92em;
  }
  .tc-progress{
    flex:1; min-width:220px; height:10px;
    border-radius:999px; overflow:hidden;
    background:rgba(0,0,0,0.10);
    border:1px solid rgba(0,0,0,0.10);
  }
  .tc-progress > div{
    width:0%; height:100%;
    background: linear-gradient(90deg, #22c55e, #16a34a);
  }

  .tc-grid { display:grid; grid-template-columns:1fr; gap:14px; margin-top:14px; }
  .tc-section{
    border:1px solid rgba(0,0,0,0.10);
    border-radius:14px; background:rgba(0,0,0,0.02);
    padding:10px 12px 6px;
  }
  .tc-title{ display:flex; gap:10px; align-items:center; margin:4px 0 10px; }
  .tc-title h3{ margin:0; font-size:1.05rem; }
  .tc-count{ margin-left:auto; opacity:0.7; font-weight:900; font-size:0.95em; }

  .tc-tasklist { list-style:none; padding:0; margin:0; }
  .tc-task{
    display:grid; grid-template-columns:22px 1fr; gap:10px;
    padding:8px 0; border-top:1px dashed rgba(0,0,0,0.14);
    align-items:start;
  }
  .tc-task:first-child { border-top:none; }
  .tc-task input { transform: translateY(2px); }
  .tc-line{ white-space:pre-wrap; line-height:1.25; }
  .tc-done .tc-line { opacity:0.55; text-decoration:line-through; }

  .tc-chiprow{ margin-top:4px; display:flex; flex-wrap:wrap; gap:6px; }
  .tc-chip{
    font-size:0.78em; padding:2px 8px; border-radius:999px;
    border:1px solid rgba(0,0,0,0.14); opacity:0.85;
    background:rgba(255,255,255,0.8);
  }

  .tc-export{
    margin-top:14px;
    border:1px solid rgba(0,0,0,0.12);
    border-radius:14px;
    padding:12px;
    background:rgba(0,0,0,0.02);
  }
  .tc-exporthead{ display:flex; gap:10px; align-items:center; margin-bottom:10px; }
  .tc-export textarea{
    width:100%; min-height:260px;
    border-radius:12px; padding:10px;
    border:1px solid rgba(0,0,0,0.18);
    font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono","Courier New", monospace;
    font-size: 12.5px; line-height: 1.35;
  }
  .tc-hidden{ display:none !important; }
</style>

<div class="tc-wrap">
  <div class="tc-bar">
    <button class="tc-btn" type="button" id="tc-newday-btn">NEW DAY</button>
    <button class="tc-btn" type="button" id="tc-export-btn" title="Optional backup / paste into GitHub if you want canonical history">BACKUP (EXPORT)</button>
    <button class="tc-btn" type="button" id="tc-copy-btn">COPY BACKUP</button>
    <button class="tc-btn" type="button" id="tc-clear-btn" title="Clears local ticks (does not affect GitHub)">CLEAR LOCAL</button>

    <label style="display:flex; align-items:center; gap:8px;">
      <span style="font-weight:900;">Sort</span>
      <select id="tc-sort">
        <option value="system" selected>System (In Progress → Key Order → A–Z)</option>
        <option value="az">A–Z</option>
        <option value="section">Section Order (as written)</option>
      </select>
    </label>

    <label style="display:flex; align-items:center; gap:8px;">
      <input type="checkbox" id="tc-hide-done" />
      <span style="font-weight:900;">Hide ✅</span>
    </label>

    <input type="text" id="tc-search" placeholder="Search…" />
    <span class="tc-meta" id="tc-meta">Auto‑save: ON</span>
  </div>

  <div class="tc-hud">
    <span class="tc-pill" id="tc-date">SATURDAY — 14 Mar 2026</span>
    <span class="tc-pill" id="tc-pill-total">Tasks: 0</span>
    <span class="tc-pill" id="tc-pill-done">Done: 0</span>
    <span class="tc-pill" id="tc-pill-xp">XP: 0</span>
    <div class="tc-progress" aria-label="progress"><div id="tc-progress-bar"></div></div>
    <span class="tc-pill" id="tc-pill-level">Level: 1</span>
  </div>

  <div class="tc-bar" style="margin-top:10px;">
    <strong>Quick Add</strong>
    <select id="tc-add-section"></select>
    <input type="text" id="tc-add-text" placeholder="Type full task line (e.g. 🔴🟠 — Add Gym Schedule (**Saturday**))" style="flex:1; min-width:260px;" />
    <button class="tc-btn" type="button" id="tc-add-btn">ADD</button>
  </div>

  <div class="tc-grid" id="tc-app"></div>

  <div class="tc-export tc-hidden" id="tc-export">
    <div class="tc-exporthead">
      <strong>Backup Export (optional)</strong>
      <span style="margin-left:auto; opacity:0.7;" id="tc-export-meta"></span>
    </div>
    <textarea id="tc-export-text" spellcheck="false"></textarea>
  </div>
</div>

<!-- RAW STARTING POINT (imported once into local storage). After first run, local storage is the “live” console. -->
<script type="text/plain" id="tc-raw">
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
  // ===== Crescent Task OS (Local-First) =====
  // Daily use: auto-saves to browser. No export required.
  // Backup export: optional (GitHub canonical / cross-device copy).

  const KEY_ORDER = ["👁️‍🗨️","⚠️","🔴","⚪️","🟢","🟠","🟣","🟡","⚫️","🔵","◻️"];
  const STORE_DATA = "crescent.tc.data.v1";   // tasks + sections
  const STORE_META = "crescent.tc.meta.v1";   // date label, xp, etc.
  const STORE_HISTORY = "crescent.tc.history.v1"; // archive of completed tasks by date (local)

  const elApp = document.getElementById("tc-app");
  const elMeta = document.getElementById("tc-meta");
  const elDate = document.getElementById("tc-date");

  const elSort = document.getElementById("tc-sort");
  const elHide = document.getElementById("tc-hide-done");
  const elSearch = document.getElementById("tc-search");

  const elAddSection = document.getElementById("tc-add-section");
  const elAddText = document.getElementById("tc-add-text");

  const btnAdd = document.getElementById("tc-add-btn");
  const btnNewDay = document.getElementById("tc-newday-btn");
  const btnExport = document.getElementById("tc-export-btn");
  const btnCopy = document.getElementById("tc-copy-btn");
  const btnClear = document.getElementById("tc-clear-btn");

  const exportWrap = document.getElementById("tc-export");
  const exportText = document.getElementById("tc-export-text");
  const exportMeta = document.getElementById("tc-export-meta");

  const pillTotal = document.getElementById("tc-pill-total");
  const pillDone = document.getElementById("tc-pill-done");
  const pillXP = document.getElementById("tc-pill-xp");
  const pillLevel = document.getElementById("tc-pill-level");
  const bar = document.getElementById("tc-progress-bar");

  const raw = (document.getElementById("tc-raw")?.textContent || "").replace(/\r\n/g,"\n");

  const norm = (s) => (s || "").replace(/\uFE0F/g, "");
  const now = () => new Date();
  const dayName = (d) => ["Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday"][d.getDay()];
  const monthName = (d) => ["Jan","Feb","Mar","Apr","May","Jun","Jul","Aug","Sep","Oct","Nov","Dec"][d.getMonth()];
  const dateLabel = (d) => `${dayName(d).toUpperCase()} — ${String(d.getDate()).padStart(2,"0")} ${monthName(d)} ${d.getFullYear()}`;

  const load = (k, fallback) => { try { return JSON.parse(localStorage.getItem(k) || JSON.stringify(fallback)); } catch { return fallback; } };
  const save = (k, v) => localStorage.setItem(k, JSON.stringify(v));

  const isTaskLine = (line) => {
    const s = (line || "").trim();
    if (!s) return false;
    if (!s.includes(" — ")) return false;
    if (!(s.includes("(**") || s.includes("( )"))) return false;
    return true;
  };

  const stableKey = (line) => norm((line || "").trim().replace(/^✅\s*—\s*/,""));
  const keyRank = (line) => {
    const n = norm(line);
    if (n.includes("(**In Progress**)")) return -1;
    for (let i=0;i<KEY_ORDER.length;i++){
      if (n.includes(norm(KEY_ORDER[i]))) return i;
    }
    return 999;
  };

  // --- Import raw console into structured data (only if no stored data exists)
  const importFromRaw = () => {
    const lines = raw.split("\n");
    const sections = [];
    let current = { title: "TOP", tasks: [], nonTasks: [] };

    lines.forEach(line => {
      const t = line.trim();
      if (t.startsWith("## ")) {
        sections.push(current);
        current = { title: t.replace(/^##\s*/,""), tasks: [], nonTasks: [] };
        return;
      }
      if (isTaskLine(line)) {
        const done = t.startsWith("✅");
        current.tasks.push({
          id: stableKey(line),
          line: t,
          done,
          createdAt: Date.now(),
          order: current.tasks.length
        });
      } else {
        current.nonTasks.push(line);
      }
    });
    sections.push(current);

    // Section order excludes TOP
    const order = sections.map(s => s.title).filter(t => t !== "TOP");

    // Build add dropdown list
    return {
      sections: sections.map(s => ({ title: s.title, nonTasks: s.nonTasks, tasks: s.tasks })),
      sectionOrder: order
    };
  };

  const ensureData = () => {
    const existing = localStorage.getItem(STORE_DATA);
    if (existing) return load(STORE_DATA, null);
    const imported = importFromRaw();
    save(STORE_DATA, imported);
    // meta date comes from the raw header, but we also set a live local date label
    const meta = { date: dateLabel(now()), xp: 0, level: 1 };
    save(STORE_META, meta);
    save(STORE_HISTORY, []);
    return imported;
  };

  let data = ensureData();
  let meta = load(STORE_META, { date: dateLabel(now()), xp: 0, level: 1 });

  const getAllTasks = () => {
    const secs = data.sections.filter(s => s.title !== "TOP");
    return secs.flatMap(s => s.tasks.map(t => ({...t, section: s.title})));
  };

  const computeHUD = () => {
    const tasks = getAllTasks();
    const total = tasks.length;
    const done = tasks.filter(t=>t.done).length;
    const xp = done * 10;
    const level = Math.max(1, Math.floor(xp/100)+1);
    const pct = total ? Math.round((done/total)*100) : 0;

    pillTotal.textContent = `Tasks: ${total}`;
    pillDone.textContent = `Done: ${done}`;
    pillXP.textContent = `XP: ${xp}`;
    pillLevel.textContent = `Level: ${level}`;
    bar.style.width = pct + "%";
    elDate.textContent = meta.date || dateLabel(now());
  };

  const applySort = (tasks, mode) => {
    const copy = [...tasks];
    if (mode === "section") {
      // stable "as written" order (order field)
      copy.sort((a,b) => (a.order ?? 0) - (b.order ?? 0));
      return copy;
    }
    if (mode === "az") {
      copy.sort((a,b)=> a.line.localeCompare(b.line));
      return copy;
    }
    // system sort
    copy.sort((a,b)=>{
      const ra = keyRank(a.line), rb = keyRank(b.line);
      if (ra !== rb) return ra - rb;
      return a.line.localeCompare(b.line);
    });
    return copy;
  };

  const rebuildSectionDropdown = () => {
    elAddSection.innerHTML = "";
    (data.sectionOrder || []).forEach(title => {
      const opt = document.createElement("option");
      opt.value = title;
      opt.textContent = title;
      elAddSection.appendChild(opt);
    });
  };

  const render = () => {
    const q = (elSearch.value || "").trim().toLowerCase();
    const hide = elHide.checked;
    const sortMode = elSort.value;

    elApp.innerHTML = "";

    data.sectionOrder.forEach(title => {
      const sec = data.sections.find(s => s.title === title);
      if (!sec) return;

      let tasks = sec.tasks.map(t => ({...t, section: title}));
      if (q) tasks = tasks.filter(t => t.line.toLowerCase().includes(q));
      if (hide) tasks = tasks.filter(t => !t.done);
      tasks = applySort(tasks, sortMode);

      const wrap = document.createElement("div");
      wrap.className = "tc-section";

      const head = document.createElement("div");
      head.className = "tc-title";

      const h = document.createElement("h3");
      h.textContent = title;

      const c = document.createElement("div");
      c.className = "tc-count";
      const doneCount = sec.tasks.filter(t=>t.done).length;
      c.textContent = `${doneCount}/${sec.tasks.length} ✅`;

      head.appendChild(h);
      head.appendChild(c);

      const ul = document.createElement("ul");
      ul.className = "tc-tasklist";

      tasks.forEach(t => {
        const li = document.createElement("li");
        li.className = "tc-task" + (t.done ? " tc-done" : "");

        const cb = document.createElement("input");
        cb.type = "checkbox";
        cb.checked = !!t.done;

        cb.addEventListener("change", () => {
          // update in data
          const secRef = data.sections.find(s => s.title === title);
          const taskRef = secRef?.tasks.find(x => x.id === t.id);
          if (taskRef) taskRef.done = cb.checked;

          save(STORE_DATA, data);
          elMeta.textContent = "Auto‑save: ON ✅";
          render();
        });

        const box = document.createElement("div");
        const line = document.createElement("div");
        line.className = "tc-line";
        line.textContent = t.line;

        const chips = document.createElement("div");
        chips.className = "tc-chiprow";

        if (t.line.includes("(**In Progress**)")) {
          const chip = document.createElement("span");
          chip.className = "tc-chip";
          chip.textContent = "In Progress";
          chips.appendChild(chip);
        }

        KEY_ORDER.forEach(k => {
          if (norm(t.line).includes(norm(k))) {
            const chip = document.createElement("span");
            chip.className = "tc-chip";
            chip.textContent = k;
            chips.appendChild(chip);
          }
        });

        box.appendChild(line);
        if (chips.childNodes.length) box.appendChild(chips);

        li.appendChild(cb);
        li.appendChild(box);
        ul.appendChild(li);
      });

      wrap.appendChild(head);
      wrap.appendChild(ul);
      elApp.appendChild(wrap);
    });

    computeHUD();
  };

  const backupExport = () => {
    // Builds a paste-ready markdown BODY (not front matter).
    // This is OPTIONAL. Your daily use does NOT require it.
    const out = [];

    // We export a clean task console body.
    out.push("🌙 CRESCENT — TASK CONSOLE  ");
    out.push((meta.date || dateLabel(now())).replace("—","—") );
    out.push("");
    out.push("## 🔑 Task Key (Authoritative Order)");
    out.push("👁️‍🗨️ → ⚠️ → 🔴 → ⚪️ → 🟢 → 🟠 → 🟣 → 🟡 → ⚫️ → 🔵 → ◻️ → none");
    out.push("");
    out.push("---");
    out.push("");

    data.sectionOrder.forEach(title => {
      const sec = data.sections.find(s=>s.title===title);
      if (!sec) return;
      out.push(`## ${title}`);
      // export in system order
      const tasks = applySort(sec.tasks.map(t=>({...t})), "system");
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
    exportWrap.classList.remove("tc-hidden");
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
      elMeta.textContent = "Select + copy (Ctrl/Cmd+C) ✅";
    }
  };

  const clearLocal = () => {
    // Clears local storage and re-imports from raw. GitHub unchanged.
    localStorage.removeItem(STORE_DATA);
    localStorage.removeItem(STORE_META);
    localStorage.removeItem(STORE_HISTORY);
    data = ensureData();
    meta = load(STORE_META, { date: dateLabel(now()), xp: 0, level: 1 });
    rebuildSectionDropdown();
    exportWrap.classList.add("tc-hidden");
    elMeta.textContent = "Reset local state ✅";
    render();
  };

  const newDay = () => {
    // New Day = remove completed tasks (archive them locally) + update date label.
    const today = dateLabel(now());
    const history = load(STORE_HISTORY, []);

    const completed = [];
    data.sections.forEach(sec => {
      if (sec.title === "TOP") return;
      const remaining = [];
      sec.tasks.forEach(t => {
        if (t.done) completed.push({ date: meta.date, section: sec.title, line: t.line, id: t.id });
        else remaining.push(t);
      });
      sec.tasks = remaining;
    });

    history.push({ rolledFrom: meta.date, rolledTo: today, completed });
    save(STORE_HISTORY, history);

    meta.date = today;
    save(STORE_META, meta);
    save(STORE_DATA, data);

    exportWrap.classList.add("tc-hidden");
    elMeta.textContent = `NEW DAY ✅ (completed archived locally)`;
    render();
  };

  // Quick Add
  const quickAdd = () => {
    const title = elAddSection.value;
    const text = (elAddText.value || "").trim();
    if (!title || !text) { elMeta.textContent = "Add requires section + text"; return; }
    if (!isTaskLine(text)) { elMeta.textContent = "Format must include ' — ' and (**Day**) or ( )"; return; }

    const sec = data.sections.find(s=>s.title===title);
    if (!sec) { elMeta.textContent = "Section not found"; return; }

    const id = stableKey(text);
    if (sec.tasks.some(t=>t.id===id)) { elMeta.textContent = "Duplicate task (same text)"; return; }

    sec.tasks.push({ id, line: text, done:false, createdAt: Date.now(), order: sec.tasks.length });
    save(STORE_DATA, data);

    elAddText.value = "";
    elMeta.textContent = "Added ✅ (auto‑saved)";
    render();
  };

  // Wire events
  rebuildSectionDropdown();
  render();

  elSort.addEventListener("change", render);
  elHide.addEventListener("change", render);
  elSearch.addEventListener("input", render);

  btnAdd.addEventListener("click", quickAdd);
  btnNewDay.addEventListener("click", newDay);
  btnExport.addEventListener("click", backupExport);
  btnCopy.addEventListener("click", copyBackup);
  btnClear.addEventListener("click", clearLocal);
})();
</script>
