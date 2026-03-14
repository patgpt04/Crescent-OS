---
layout: page
title: Task Console
permalink: /systems/task-console/
---

<style>
  /* Crescent OS — Task Console UI */
  .tc-wrap { margin-top: 10px; }
  .tc-bar {
    display: flex; flex-wrap: wrap; gap: 10px; align-items: center;
    padding: 12px 12px; border: 1px solid rgba(0,0,0,0.12);
    border-radius: 12px; background: rgba(0,0,0,0.03);
  }
  .tc-bar input[type="text"], .tc-bar select {
    padding: 8px 10px; border-radius: 10px;
    border: 1px solid rgba(0,0,0,0.18); background: white;
    min-width: 170px;
  }
  .tc-btn {
    appearance: none; border: 1px solid rgba(0,0,0,0.18);
    background: white; border-radius: 10px;
    padding: 8px 10px; cursor: pointer; font-weight: 700;
  }
  .tc-btn:hover { background: rgba(0,0,0,0.04); }
  .tc-meta { margin-left: auto; opacity: 0.75; font-size: 0.95em; }

  .tc-grid { display: grid; grid-template-columns: 1fr; gap: 14px; margin-top: 14px; }
  .tc-section {
    border: 1px solid rgba(0,0,0,0.10);
    border-radius: 14px;
    background: rgba(0,0,0,0.02);
    padding: 10px 12px 6px;
  }
  .tc-title { display:flex; gap: 10px; align-items:center; margin: 4px 0 10px; }
  .tc-title h3 { margin: 0; font-size: 1.05rem; }
  .tc-count { margin-left:auto; opacity:0.7; font-size:0.95em; }

  .tc-tasklist { list-style: none; padding: 0; margin: 0; }
  .tc-task {
    display: grid; grid-template-columns: 22px 1fr; gap: 10px;
    padding: 8px 0; border-top: 1px dashed rgba(0,0,0,0.14);
    align-items: start;
  }
  .tc-task:first-child { border-top: none; }
  .tc-task input { transform: translateY(2px); }
  .tc-line { white-space: pre-wrap; line-height: 1.25; }
  .tc-done .tc-line { opacity: 0.55; text-decoration: line-through; }

  .tc-chiprow { margin-top: 4px; display:flex; flex-wrap:wrap; gap: 6px; }
  .tc-chip {
    font-size: 0.78em; padding: 2px 8px; border-radius: 999px;
    border: 1px solid rgba(0,0,0,0.14); opacity: 0.85;
    background: rgba(255,255,255,0.8);
  }

  .tc-export {
    margin-top: 14px;
    border: 1px solid rgba(0,0,0,0.12);
    border-radius: 14px;
    padding: 12px;
    background: rgba(0,0,0,0.02);
  }
  .tc-exporthead { display:flex; gap: 10px; align-items:center; margin-bottom: 10px; }
  .tc-export textarea {
    width: 100%; min-height: 260px;
    border-radius: 12px; padding: 10px;
    border: 1px solid rgba(0,0,0,0.18);
    font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono","Courier New", monospace;
    font-size: 12.5px; line-height: 1.35;
  }

  /* Source block: visible fallback if JS fails */
  #tc-source { margin-top: 16px; }
  .tc-hidden { display: none !important; }
</style>

<div class="tc-wrap">
  <div class="tc-bar" id="tc-bar">
    <button class="tc-btn" type="button" id="tc-export-btn">EXPORT</button>
    <button class="tc-btn" type="button" id="tc-copy-btn">COPY</button>

    <label style="display:flex; align-items:center; gap:8px;">
      <span style="font-weight:700;">Sort</span>
      <select id="tc-sort">
        <option value="system" selected>System (In Progress → Key Order → A-Z)</option>
        <option value="az">A-Z</option>
      </select>
    </label>

    <label style="display:flex; align-items:center; gap:8px;">
      <input type="checkbox" id="tc-hide-done" />
      <span style="font-weight:700;">Hide ✅</span>
    </label>

    <input type="text" id="tc-search" placeholder="Search tasks…" />

    <span class="tc-meta" id="tc-meta">Loading…</span>
  </div>

  <div class="tc-bar" style="margin-top:10px;">
    <strong>Quick Add</strong>
    <select id="tc-add-section"></select>
    <input type="text" id="tc-add-text" placeholder="Type task text (e.g. 🔴🟠 — Gym schedule (**Saturday**))" style="flex:1; min-width:260px;" />
    <button class="tc-btn" type="button" id="tc-add-btn">ADD</button>
    <button class="tc-btn" type="button" id="tc-clear-local">CLEAR LOCAL</button>
  </div>

  <div class="tc-grid" id="tc-app"></div>

  <div class="tc-export tc-hidden" id="tc-export">
    <div class="tc-exporthead">
      <strong>Export (paste back into <code>systems/task-console/index.md</code> body)</strong>
      <span style="margin-left:auto; opacity:0.7;" id="tc-export-meta"></span>
    </div>
    <textarea id="tc-export-text" spellcheck="false"></textarea>
  </div>

  <!-- CANONICAL SOURCE (fallback) -->
  <div id="tc-source" markdown="1">

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

  </div>
</div>

<script>
(() => {
  const KEY_ORDER = ["👁️‍🗨️","⚠️","🔴","⚪️","🟢","🟠","🟣","🟡","⚫️","🔵","◻️"];
  const STORAGE_KEY = "crescent.tc.state.v2";
  const ADD_KEY = "crescent.tc.added.v1";

  const norm = (s) => (s || "").replace(/\uFE0F/g, ""); // remove variation selectors
  const keyRank = (line) => {
    const n = norm(line);
    // In Progress first (special)
    if (n.includes("(**In Progress**)")) return -1;
    // Find earliest key in order that appears in the line
    for (let i = 0; i < KEY_ORDER.length; i++) {
      if (n.includes(norm(KEY_ORDER[i]))) return i;
    }
    return 999; // none
  };

  const source = document.getElementById("tc-source");
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

  const addSection = document.getElementById("tc-add-section");
  const addText = document.getElementById("tc-add-text");
  const btnAdd = document.getElementById("tc-add-btn");
  const btnClearLocal = document.getElementById("tc-clear-local");

  if (!source || !app) return;

  const isTaskLine = (line) => {
    const s = (line || "").trim();
    if (!s) return false;
    // Must contain your delimiter
    if (!s.includes(" — ")) return false;
    // Must contain "(**" or "( )" markers (your dates/slots)
    if (!(s.includes("(**") || s.includes("( )"))) return false;
    // Exclude headings
    if (s.startsWith("#")) return false;
    return true;
  };

  const parseLine = (line) => {
    const s = (line || "").trim();
    const done = s.startsWith("✅");
    const idx = s.indexOf(" — ");
    const prefix = idx >= 0 ? s.slice(0, idx) : "";
    const rest = idx >= 0 ? s.slice(idx + 3) : s;
    return { raw: line, done, prefix, rest };
  };

  const stableKey = (line) => {
    // Stable identity: strip leading ✅ and whitespace
    return norm((line || "").trim().replace(/^✅\s*—\s*/,""));
  };

  const loadJSON = (k, fallback) => {
    try { return JSON.parse(localStorage.getItem(k) || JSON.stringify(fallback)); }
    catch { return fallback; }
  };
  const saveJSON = (k, v) => localStorage.setItem(k, JSON.stringify(v));

  // Build model from source text
  const rawText = source.innerText.replace(/\r\n/g, "\n");
  const lines = rawText.split("\n");

  // Sections: keyed by heading text
  const sections = [];
  let current = { title: "TOP", lines: [] };

  for (const line of lines) {
    if (line.trim().startsWith("## ")) {
      // push previous
      sections.push(current);
      current = { title: line.trim().replace(/^##\s*/,""), lines: [line] };
    } else {
      current.lines.push(line);
    }
  }
  sections.push(current);

  // Extract tasks per section, keep non-task content
  const state = loadJSON(STORAGE_KEY, {});
  const added = loadJSON(ADD_KEY, {}); // { "Section Title": ["task line", ...] }

  const model = sections.map(sec => {
    const tasks = [];
    const nonTasks = [];
    sec.lines.forEach(l => {
      if (isTaskLine(l)) {
        const p = parseLine(l);
        const k = stableKey(l);
        // local override only affects UI + export marking complete
        const localDone = (typeof state[k] === "boolean") ? state[k] : p.done;
        tasks.push({ line: l.trim(), key: k, done: localDone, baseDone: p.done, section: sec.title });
      } else {
        nonTasks.push(l);
      }
    });

    const extra = (added[sec.title] || []).map(l => {
      const k = stableKey(l);
      const p = parseLine(l);
      const localDone = (typeof state[k] === "boolean") ? state[k] : p.done;
      return { line: l.trim(), key: k, done: localDone, baseDone: p.done, section: sec.title, added: true };
    });

    return { title: sec.title, nonTasks, tasks: tasks.concat(extra) };
  });

  // Populate section dropdown for Quick Add (skip TOP)
  const sectionTitles = model.map(m => m.title).filter(t => t !== "TOP");
  addSection.innerHTML = "";
  sectionTitles.forEach(t => {
    const opt = document.createElement("option");
    opt.value = t; opt.textContent = t;
    addSection.appendChild(opt);
  });

  const applySort = (tasks, mode) => {
    const copy = [...tasks];
    if (mode === "az") {
      copy.sort((a,b) => a.line.localeCompare(b.line));
      return copy;
    }
    // system: In Progress → Key order → A-Z
    copy.sort((a,b) => {
      const ra = keyRank(a.line), rb = keyRank(b.line);
      if (ra !== rb) return ra - rb;
      return a.line.localeCompare(b.line);
    });
    return copy;
  };

  const render = () => {
    const q = (search.value || "").trim().toLowerCase();
    const mode = sortSel.value;
    const hide = hideDone.checked;

    app.innerHTML = "";
    let total = 0, doneCount = 0;

    model.forEach(sec => {
      // We only render real sections (skip TOP)
      if (sec.title === "TOP") return;

      let tasks = applySort(sec.tasks, mode);

      if (q) tasks = tasks.filter(t => t.line.toLowerCase().includes(q));
      if (hide) tasks = tasks.filter(t => !t.done);

      total += sec.tasks.length;
      doneCount += sec.tasks.filter(t => t.done).length;

      const wrap = document.createElement("div");
      wrap.className = "tc-section";

      const title = document.createElement("div");
      title.className = "tc-title";

      const h = document.createElement("h3");
      h.textContent = sec.title;

      const c = document.createElement("div");
      c.className = "tc-count";
      c.textContent = `${sec.tasks.filter(t => t.done).length}/${sec.tasks.length} ✅`;

      title.appendChild(h);
      title.appendChild(c);

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
          saveJSON(STORAGE_KEY, state);
          render();
          meta.textContent = `Saved ✅ (${countDoneAll()}/${countAll()})`;
        });

        const box = document.createElement("div");
        const line = document.createElement("div");
        line.className = "tc-line";
        line.textContent = t.line;

        // chips
        const chips = document.createElement("div");
        chips.className = "tc-chiprow";

        if (t.line.includes("(**In Progress**)")) {
          const chip = document.createElement("span");
          chip.className = "tc-chip";
          chip.textContent = "In Progress";
          chips.appendChild(chip);
        }
        // key chips in order
        KEY_ORDER.forEach(k => {
          if (norm(t.line).includes(norm(k))) {
            const chip = document.createElement("span");
            chip.className = "tc-chip";
            chip.textContent = k;
            chips.appendChild(chip);
          }
        });
        if (t.added) {
          const chip = document.createElement("span");
          chip.className = "tc-chip";
          chip.textContent = "Local";
          chips.appendChild(chip);
        }

        box.appendChild(line);
        if (chips.childNodes.length) box.appendChild(chips);

        li.appendChild(cb);
        li.appendChild(box);
        ul.appendChild(li);
      });

      wrap.appendChild(title);
      wrap.appendChild(ul);
      app.appendChild(wrap);
    });

    meta.textContent = `Ready ✅ (${countDoneAll()}/${countAll()})`;
  };

  const countAll = () => model.reduce((acc, s) => acc + (s.title === "TOP" ? 0 : s.tasks.length), 0);
  const countDoneAll = () => model.reduce((acc, s) => acc + (s.title === "TOP" ? 0 : s.tasks.filter(t=>t.done).length), 0);

  const buildExport = () => {
    // Rebuild a clean markdown body with the same headings in the same order.
    // Completed tasks get ✅ prefix (we do NOT remove existing ✅).
    const out = [];

    // Pull the TOP section non-task text verbatim (header block above first ##)
    const top = model.find(m => m.title === "TOP");
    if (top) {
      top.nonTasks.forEach(l => out.push(l));
    }

    // For each section, output heading + tasks in current sort mode
    const mode = sortSel.value;
    model.forEach(sec => {
      if (sec.title === "TOP") return;

      // Ensure there's a blank line before section heading
      if (out.length && out[out.length - 1].trim() !== "") out.push("");
      // Print heading as "## ..."
      out.push(`## ${sec.title}`);

      // If the original section had separators like --- right after heading, preserve by keeping original nonTasks lines that were in-section
      // But we rebuild simple: tasks only. (Still paste-ready, still your structure.)
      // Add tasks:
      const tasks = applySort(sec.tasks, mode);
      tasks.forEach(t => {
        const s = t.line.trim();
        if (t.done) {
          // ensure ✅ format: ✅ — rest after first " — "
          if (s.startsWith("✅")) {
            out.push(s + "  ");
          } else {
            const idx = s.indexOf(" — ");
            const rest = idx >= 0 ? s.slice(idx + 3) : s;
            out.push(`✅ — ${rest}  `);
          }
        } else {
          out.push(s + "  ");
        }
      });

      // Section separator if your original uses --- widely (keep consistent feel)
      out.push("");
      out.push("---");
      out.push("");
    });

    // Remove the last separator clutter (optional cleanup)
    while (out.length && out[out.length - 1].trim() === "") out.pop();
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
      meta.textContent = "Copied ✅";
    } catch {
      meta.textContent = "Select + Copy (Ctrl/Cmd+C) ✅";
    }
  };

  btnExport.addEventListener("click", showExport);
  btnCopy.addEventListener("click", copyExport);

  sortSel.addEventListener("change", render);
  hideDone.addEventListener("change", render);
  search.addEventListener("input", render);

  btnAdd.addEventListener("click", () => {
    const sec = addSection.value;
    const text = (addText.value || "").trim();
    if (!sec || !text) {
      meta.textContent = "Type task + choose section";
      return;
    }
    // Basic safety: must look like a task line
    if (!isTaskLine(text)) {
      meta.textContent = "Add format: must contain ' — ' + (**) or ( )";
      return;
    }
    added[sec] = added[sec] || [];
    added[sec].push(text);
    saveJSON(ADD_KEY, added);

    // Add into model live too
    const target = model.find(m => m.title === sec);
    if (target) {
      const k = stableKey(text);
      const p = parseLine(text);
      const localDone = (typeof state[k] === "boolean") ? state[k] : p.done;
      target.tasks.push({ line: text, key: k, done: localDone, baseDone: p.done, section: sec, added: true });
    }

    addText.value = "";
    render();
    meta.textContent = "Added (local) ✅";
  });

  btnClearLocal.addEventListener("click", () => {
    localStorage.removeItem(STORAGE_KEY);
    localStorage.removeItem(ADD_KEY);
    meta.textContent = "Cleared local state ✅ (refresh page)";
  });

  // If JS is working, hide the raw source so you only see the interactive UI
  source.classList.add("tc-hidden");
  render();
})();
</script>
