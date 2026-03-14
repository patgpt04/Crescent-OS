---
layout: page
title: Task Console
permalink: /systems/task-console/
---

<style>
  /* Crescent OS — Task Console UI (game-menu vibes, minimal) */
  .tc-wrap { margin-top: 10px; }
  .tc-bar{
    display:flex; flex-wrap:wrap; gap:10px; align-items:center;
    padding:12px; border:1px solid rgba(0,0,0,0.12);
    border-radius:14px; background:rgba(0,0,0,0.03);
  }
  .tc-btn{
    appearance:none; border:1px solid rgba(0,0,0,0.18);
    background:#fff; border-radius:12px;
    padding:8px 10px; cursor:pointer; font-weight:800;
    letter-spacing:0.2px;
  }
  .tc-btn:hover{ background:rgba(0,0,0,0.04); }
  .tc-bar input[type="text"], .tc-bar select{
    padding:8px 10px; border-radius:12px;
    border:1px solid rgba(0,0,0,0.18); background:#fff;
    min-width:170px;
  }
  .tc-meta{ margin-left:auto; opacity:0.75; font-size:0.95em; }

  .tc-hud{
    display:flex; gap:12px; align-items:center; flex-wrap:wrap;
    margin-top:10px; padding:12px;
    border:1px solid rgba(0,0,0,0.10);
    border-radius:14px; background:rgba(0,0,0,0.02);
  }
  .tc-hud .tc-pill{
    padding:6px 10px; border-radius:999px;
    border:1px solid rgba(0,0,0,0.14);
    background:rgba(255,255,255,0.8);
    font-weight:800; font-size:0.92em;
  }
  .tc-progress{
    flex:1; min-width:240px;
    height:10px; border-radius:999px;
    background:rgba(0,0,0,0.10); overflow:hidden;
    border:1px solid rgba(0,0,0,0.10);
  }
  .tc-progress > div{
    height:100%; width:0%;
    background:linear-gradient(90deg, #21c55d, #16a34a);
  }

  .tc-grid{ display:grid; grid-template-columns:1fr; gap:14px; margin-top:14px; }
  .tc-section{
    border:1px solid rgba(0,0,0,0.10);
    border-radius:16px; background:rgba(0,0,0,0.02);
    padding:10px 12px 6px;
  }
  .tc-title{ display:flex; gap:10px; align-items:center; margin:4px 0 10px; }
  .tc-title h3{ margin:0; font-size:1.05rem; }
  .tc-count{ margin-left:auto; opacity:0.7; font-size:0.95em; font-weight:800; }

  .tc-tasklist{ list-style:none; padding:0; margin:0; }
  .tc-task{
    display:grid; grid-template-columns:22px 1fr; gap:10px;
    padding:8px 0; border-top:1px dashed rgba(0,0,0,0.14);
    align-items:start;
  }
  .tc-task:first-child{ border-top:none; }
  .tc-task input{ transform:translateY(2px); }
  .tc-line{ white-space:pre-wrap; line-height:1.25; }
  .tc-done .tc-line{ opacity:0.55; text-decoration:line-through; }

  .tc-chiprow{ margin-top:4px; display:flex; flex-wrap:wrap; gap:6px; }
  .tc-chip{
    font-size:0.78em; padding:2px 8px; border-radius:999px;
    border:1px solid rgba(0,0,0,0.14); opacity:0.85;
    background:rgba(255,255,255,0.8);
  }

  .tc-export{
    margin-top:14px; border:1px solid rgba(0,0,0,0.12);
    border-radius:16px; padding:12px; background:rgba(0,0,0,0.02);
  }
  .tc-exporthead{ display:flex; gap:10px; align-items:center; margin-bottom:10px; }
  .tc-export textarea{
    width:100%; min-height:260px; border-radius:14px; padding:10px;
    border:1px solid rgba(0,0,0,0.18);
    font-family:ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono","Courier New", monospace;
    font-size:12.5px; line-height:1.35;
  }

  .tc-hidden{ display:none !important; }
</style>

<div class="tc-wrap">
  <div class="tc-bar">
    <button class="tc-btn" type="button" id="tc-export-btn">EXPORT MARKDOWN</button>
    <button class="tc-btn" type="button" id="tc-copy-btn">COPY EXPORT</button>
    <button class="tc-btn" type="button" id="tc-clear-btn">CLEAR CHECKS</button>

    <label style="display:flex; align-items:center; gap:8px;">
      <span style="font-weight:900;">Sort</span>
      <select id="tc-sort">
        <option value="system" selected>System (In Progress → Key Order → A-Z)</option>
        <option value="az">A-Z</option>
      </select>
    </label>

    <label style="display:flex; align-items:center; gap:8px;">
      <input type="checkbox" id="tc-hide-done" />
      <span style="font-weight:900;">Hide ✅</span>
    </label>

    <input type="text" id="tc-search" placeholder="Search…" />
    <span class="tc-meta" id="tc-meta">Loading…</span>
  </div>

  <div class="tc-hud">
    <span class="tc-pill" id="tc-pill-total">Tasks: 0</span>
    <span class="tc-pill" id="tc-pill-done">Done: 0</span>
    <span class="tc-pill" id="tc-pill-xp">XP: 0</span>
    <div class="tc-progress" aria-label="progress"><div id="tc-progress-bar"></div></div>
    <span class="tc-pill" id="tc-pill-level">Level: 1</span>
  </div>

  <div class="tc-grid" id="tc-app"></div>

  <div class="tc-export tc-hidden" id="tc-export">
    <div class="tc-exporthead">
      <strong>Export (paste back into <code>systems/task-console/index.md</code>)</strong>
      <span style="margin-left:auto; opacity:0.7;" id="tc-export-meta"></span>
    </div>
    <textarea id="tc-export-text" spellcheck="false"></textarea>
  </div>
</div>

<!-- RAW CANONICAL SOURCE (this is what the script reads; GitHub rendering can’t break it) -->
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
  const KEY_ORDER = ["👁️‍🗨️","⚠️","🔴","⚪️","🟢","🟠","🟣","🟡","⚫️","🔵","◻️"];
  const STORAGE_KEY = "crescent.tc.state.v3";

  const app = document.getElementById("tc-app");
  const meta = document.getElementById("tc-meta");
  const sortSel = document.getElementById("tc-sort");
  const hideDone = document.getElementById("tc-hide-done");
  const search = document.getElementById("tc-search");

  const exportWrap = document.getElementById("tc-export");
  const exportText = document.getElementById("tc-export-text");
  const exportMeta = document.getElementById("tc-export-meta");

  const btnExport = document.getElementById("tc-export-btn");
  const btnCopy = document.getElementById("tc-copy-btn");
  const btnClear = document.getElementById("tc-clear-btn");

  const pillTotal = document.getElementById("tc-pill-total");
  const pillDone = document.getElementById("tc-pill-done");
  const pillXP = document.getElementById("tc-pill-xp");
  const pillLevel = document.getElementById("tc-pill-level");
  const bar = document.getElementById("tc-progress-bar");

  const rawEl = document.getElementById("tc-raw");
  if (!rawEl || !app) return;

  const norm = (s) => (s || "").replace(/\uFE0F/g, "");
  const load = () => { try { return JSON.parse(localStorage.getItem(STORAGE_KEY) || "{}"); } catch { return {}; } };
  const save = (s) => localStorage.setItem(STORAGE_KEY, JSON.stringify(s));
  const state = load();

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
    for (let i = 0; i < KEY_ORDER.length; i++) {
      if (n.includes(norm(KEY_ORDER[i]))) return i;
    }
    return 999;
  };

  const parseSections = (raw) => {
    const lines = raw.replace(/\r\n/g,"\n").split("\n");
    const sections = [];
    let current = { title: "TOP", lines: [] };
    for (const line of lines) {
      if (line.trim().startsWith("## ")) {
        sections.push(current);
        current = { title: line.trim().replace(/^##\s*/,""), lines: [] };
      } else {
        current.lines.push(line);
      }
    }
    sections.push(current);
    return sections;
  };

  const sections = parseSections(rawEl.textContent || "");

  const model = sections.map(sec => {
    const tasks = [];
    const nonTasks = [];
    sec.lines.forEach(l => {
      if (isTaskLine(l)) {
        const k = stableKey(l);
        const baseDone = l.trim().startsWith("✅");
        const done = (typeof state[k] === "boolean") ? state[k] : baseDone;
        tasks.push({ line: l.trim(), key: k, done, baseDone, section: sec.title });
      } else {
        nonTasks.push(l);
      }
    });
    return { title: sec.title, nonTasks, tasks };
  });

  const applySort = (tasks, mode) => {
    const copy = [...tasks];
    if (mode === "az") return copy.sort((a,b) => a.line.localeCompare(b.line));
    return copy.sort((a,b) => {
      const ra = keyRank(a.line), rb = keyRank(b.line);
      if (ra !== rb) return ra - rb;
      return a.line.localeCompare(b.line);
    });
  };

  const countAll = () => model.reduce((acc,s) => acc + (s.title==="TOP" ? 0 : s.tasks.length), 0);
  const countDoneAll = () => model.reduce((acc,s) => acc + (s.title==="TOP" ? 0 : s.tasks.filter(t=>t.done).length), 0);

  const calcXP = () => {
    // simple: 10 XP per completed task
    return countDoneAll() * 10;
  };
  const calcLevel = (xp) => {
    // simple: 100 xp per level
    return Math.max(1, Math.floor(xp / 100) + 1);
  };

  const renderHUD = () => {
    const total = countAll();
    const done = countDoneAll();
    const xp = calcXP();
    const lvl = calcLevel(xp);
    pillTotal.textContent = `Tasks: ${total}`;
    pillDone.textContent = `Done: ${done}`;
    pillXP.textContent = `XP: ${xp}`;
    pillLevel.textContent = `Level: ${lvl}`;
    const pct = total ? Math.round((done/total)*100) : 0;
    bar.style.width = pct + "%";
  };

  const render = () => {
    const q = (search.value || "").trim().toLowerCase();
    const mode = sortSel.value;
    const hide = hideDone.checked;

    app.innerHTML = "";

    model.forEach(sec => {
      if (sec.title === "TOP") return;

      let tasks = applySort(sec.tasks, mode);
      if (q) tasks = tasks.filter(t => t.line.toLowerCase().includes(q));
      if (hide) tasks = tasks.filter(t => !t.done);

      const wrap = document.createElement("div");
      wrap.className = "tc-section";

      const head = document.createElement("div");
      head.className = "tc-title";

      const h = document.createElement("h3");
      h.textContent = sec.title;

      const c = document.createElement("div");
      c.className = "tc-count";
      c.textContent = `${sec.tasks.filter(t=>t.done).length}/${sec.tasks.length} ✅`;

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
          t.done = cb.checked;
          state[t.key] = cb.checked;
          save(state);
          render();
          meta.textContent = `Saved ✅ (${countDoneAll()}/${countAll()})`;
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
      app.appendChild(wrap);
    });

    renderHUD();
    meta.textContent = `Ready ✅ (${countDoneAll()}/${countAll()})`;
  };

  const buildExport = () => {
    // Rebuild the raw markdown, same section order, tasks optionally sorted visually but export keeps your canonical order:
    // We'll export in "System sort" for cleanliness (matches your system).
    const out = [];

    // TOP content (header + key order + separators etc) is in raw script itself; we reconstruct by reusing raw sections:
    const top = model.find(m => m.title === "TOP");
    if (top) out.push(...top.nonTasks);

    model.forEach(sec => {
      if (sec.title === "TOP") return;

      // section heading
      out.push("");
      out.push(`## ${sec.title}`);

      const tasks = applySort(sec.tasks, "system"); // export in system order
      tasks.forEach(t => {
        const s = t.line.trim();
        if (t.done) {
          if (s.startsWith("✅")) out.push(s + "  ");
          else {
            const idx = s.indexOf(" — ");
            const rest = idx >= 0 ? s.slice(idx + 3) : s;
            out.push(`✅ — ${rest}  `);
          }
        } else {
          // ensure not ✅
          if (s.startsWith("✅")) {
            const idx = s.indexOf(" — ");
            const rest = idx >= 0 ? s.slice(idx + 3) : s.replace(/^✅\s*/,"");
            out.push(`— ${rest}  `);
          } else out.push(s + "  ");
        }
      });

      out.push("");
      out.push("---");
    });

    // trim trailing empties
    while (out.length && out[out.length-1].trim()==="") out.pop();
    return out.join("\n");
  };

  const showExport = () => {
    const txt = buildExport();
    exportText.value = txt;
    exportWrap.classList.remove("tc-hidden");
    exportMeta.textContent = `Tasks: ${countAll()} | Done: ${countDoneAll()}`;
  };

  const copyExport = async () => {
    showExport();
    exportText.focus();
    exportText.select();
    try {
      await navigator.clipboard.writeText(exportText.value);
      meta.textContent = "Copied export ✅";
    } catch {
      meta.textContent = "Select + Copy (Ctrl/Cmd+C) ✅";
    }
  };

  const clearChecks = () => {
    localStorage.removeItem(STORAGE_KEY);
    // reset state in-memory
    model.forEach(sec => sec.tasks.forEach(t => t.done = false));
    meta.textContent = "Cleared ✅";
    render();
  };

  btnExport.addEventListener("click", showExport);
  btnCopy.addEventListener("click", copyExport);
  btnClear.addEventListener("click", clearChecks);

  sortSel.addEventListener("change", render);
  hideDone.addEventListener("change", render);
  search.addEventListener("input", render);

  render();
})();
</script>
