---
layout: page
title: Task Console
permalink: /systems/task-console/
---

<!--
INTERACTIVE TASK CONSOLE (Layer 2)

How it works:
- Tasks are detected from the rendered markdown lines that contain " — " and "(**" or "( )"
- Checking a task stores state in localStorage (browser-only)
- EXPORT creates paste-ready markdown where completed tasks become: "✅ — Task (...)" per your rules
- To persist canonically: export -> paste into this file -> commit

No automation / no GitHub write-back.
-->

<style>
  /* Crescent Task Console — minimal, command-centre */
  #tc-controls {
    display: flex;
    flex-wrap: wrap;
    gap: 8px;
    align-items: center;
    margin: 14px 0 18px;
    padding: 10px 12px;
    border: 1px solid rgba(255,255,255,0.12);
    border-radius: 10px;
    background: rgba(255,255,255,0.03);
  }
  #tc-controls .tc-btn {
    appearance: none;
    border: 1px solid rgba(255,255,255,0.18);
    background: rgba(255,255,255,0.06);
    color: inherit;
    padding: 8px 10px;
    border-radius: 8px;
    cursor: pointer;
    font-weight: 600;
  }
  #tc-controls .tc-btn:hover { background: rgba(255,255,255,0.10); }
  #tc-controls .tc-meta {
    margin-left: auto;
    opacity: 0.85;
    font-size: 0.95em;
  }
  .tc-tasklist { margin: 6px 0 0; padding: 0; }
  .tc-task {
    list-style: none;
    display: grid;
    grid-template-columns: 22px 1fr;
    gap: 10px;
    align-items: start;
    padding: 6px 0;
    border-bottom: 1px dashed rgba(255,255,255,0.10);
  }
  .tc-task:last-child { border-bottom: none; }
  .tc-task input { margin-top: 2px; transform: translateY(1px); }
  .tc-task .tc-label { white-space: pre-wrap; }
  .tc-task.tc-done .tc-label { opacity: 0.6; text-decoration: line-through; }
  .tc-hidden { display: none !important; }

  /* Export modal */
  #tc-export {
    margin-top: 10px;
    padding: 10px 12px;
    border: 1px solid rgba(255,255,255,0.12);
    border-radius: 10px;
    background: rgba(255,255,255,0.03);
  }
  #tc-export textarea {
    width: 100%;
    min-height: 260px;
    padding: 10px;
    border-radius: 8px;
    border: 1px solid rgba(255,255,255,0.18);
    background: rgba(0,0,0,0.25);
    color: inherit;
    font-family: ui-monospace, SFMono-Regular, Menlo, Monaco, Consolas, "Liberation Mono", "Courier New", monospace;
    font-size: 12.5px;
    line-height: 1.35;
  }
  #tc-export .tc-export-head {
    display: flex;
    gap: 8px;
    align-items: center;
    margin-bottom: 8px;
  }
</style>

<div id="tc-controls" class="tc-hidden">
  <button class="tc-btn" id="tc-export-btn" type="button">EXPORT MARKDOWN</button>
  <button class="tc-btn" id="tc-copy-btn" type="button">COPY EXPORT</button>
  <button class="tc-btn" id="tc-clear-btn" type="button">CLEAR CHECKS</button>
  <span class="tc-meta" id="tc-meta">Ready</span>
</div>

<div id="tc-export" class="tc-hidden">
  <div class="tc-export-head">
    <strong>Export (paste back into <code>systems/task-console/index.md</code> body)</strong>
  </div>
  <textarea id="tc-export-text" spellcheck="false"></textarea>
</div>

<div id="tc-body" markdown="1">

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

<script>
(() => {
  const STORAGE_KEY = "crescent.taskconsole.v1";
  const body = document.getElementById("tc-body");
  const controls = document.getElementById("tc-controls");
  const exportWrap = document.getElementById("tc-export");
  const exportText = document.getElementById("tc-export-text");
  const meta = document.getElementById("tc-meta");

  const btnExport = document.getElementById("tc-export-btn");
  const btnCopy = document.getElementById("tc-copy-btn");
  const btnClear = document.getElementById("tc-clear-btn");

  if (!body) return;

  // Load saved state
  const loadState = () => {
    try { return JSON.parse(localStorage.getItem(STORAGE_KEY) || "{}"); }
    catch { return {}; }
  };
  const saveState = (state) => {
    localStorage.setItem(STORAGE_KEY, JSON.stringify(state));
  };

  // A stable key for a task line (text-only)
  const keyOf = (line) => {
    // Remove leading checkbox markers + trim
    return line.replace(/^✅\s*—\s*/,"").trim();
  };

  // Detect whether a line is a task
  const isTaskLine = (line) => {
    const s = line.trim();
    if (!s) return false;
    // Must contain " — " (your canonical task delimiter)
    if (!s.includes(" — ")) return false;
    // Must include a date/slot marker "(**" or "( )"
    if (!(s.includes("(**") || s.includes("( )"))) return false;
    return true;
  };

  // Parse prefix + task label portion
  const parseLine = (line) => {
    const s = line.trim();
    const done = s.startsWith("✅");
    // Split on first " — "
    const idx = s.indexOf(" — ");
    const prefix = idx >= 0 ? s.slice(0, idx) : "";
    const rest = idx >= 0 ? s.slice(idx + 3) : s;
    return { raw: line, done, prefix, rest };
  };

  const state = loadState();

  // Convert rendered markdown paragraphs that contain task lines into interactive lists.
  const paras = Array.from(body.querySelectorAll("p"));
  let taskCount = 0;

  paras.forEach(p => {
    const lines = (p.innerText || "").split("\n").map(x => x.trimEnd());
    const taskLines = lines.filter(isTaskLine);
    if (taskLines.length === 0) return;

    // Build UI list
    const ul = document.createElement("ul");
    ul.className = "tc-tasklist";

    taskLines.forEach(line => {
      const { done, rest } = parseLine(line);
      const k = keyOf(line);

      const li = document.createElement("li");
      li.className = "tc-task";

      const cb = document.createElement("input");
      cb.type = "checkbox";

      // Effective done: file-marked OR saved override
      const saved = state[k];
      const effectiveDone = (typeof saved === "boolean") ? saved : done;
      cb.checked = effectiveDone;
      if (effectiveDone) li.classList.add("tc-done");

      cb.addEventListener("change", () => {
        state[k] = cb.checked;
        saveState(state);
        li.classList.toggle("tc-done", cb.checked);
        meta.textContent = `Saved ✅ (${countDone()}/${taskCount})`;
      });

      const label = document.createElement("div");
      label.className = "tc-label";
      label.textContent = line; // keep full line visible

      li.appendChild(cb);
      li.appendChild(label);
      ul.appendChild(li);

      taskCount++;
    });

    // Hide original paragraph and insert list after it
    p.classList.add("tc-hidden");
    p.insertAdjacentElement("afterend", ul);
  });

  const countDone = () => {
    let done = 0;
    // done if local state says true OR label line starts with ✅
    const all = Array.from(body.querySelectorAll(".tc-task"));
    all.forEach(li => {
      const cb = li.querySelector("input[type=checkbox]");
      if (cb && cb.checked) done++;
    });
    return done;
  };

  // Build export markdown body (not front matter)
  const buildExportBody = () => {
    // Export based on the ORIGINAL markdown text inside #tc-body,
    // but swap any task line prefix to ✅ if checked.
    const text = body.innerText.replace(/\r\n/g, "\n");
    const lines = text.split("\n");

    const doneKeys = new Set();
    // Collect checked tasks by reading visible labels from UI
    const labels = Array.from(body.querySelectorAll(".tc-task .tc-label"));
    labels.forEach(lab => {
      const line = (lab.textContent || "").trim();
      if (!isTaskLine(line)) return;
      const k = keyOf(line);
      const cb = lab.parentElement.querySelector("input[type=checkbox]");
      if (cb && cb.checked) doneKeys.add(k);
    });

    const out = lines.map(line => {
      if (!isTaskLine(line)) return line;
      const k = keyOf(line);
      if (!doneKeys.has(k)) {
        // If it was marked ✅ in file but user unchecked, restore original by removing ✅
        // (keeps original prefix if present)
        const s = line.trim();
        if (s.startsWith("✅")) {
          // try to reconstruct as "— rest" if no key remains; fallback to rest only
          const idx = s.indexOf(" — ");
          const rest = idx >= 0 ? s.slice(idx + 3) : s.replace(/^✅\s*/,"");
          return "— " + rest;
        }
        return line;
      }

      // Mark complete per your rule: "✅ — Task (...)"
      const s = line.trim();
      const idx = s.indexOf(" — ");
      const rest = idx >= 0 ? s.slice(idx + 3) : s;
      return "✅ — " + rest;
    }).join("\n");

    return out.trimEnd();
  };

  const showExport = () => {
    const out = buildExportBody();
    exportText.value = out;
    exportWrap.classList.remove("tc-hidden");
    meta.textContent = `Export ready ✅ (${countDone()}/${taskCount})`;
  };

  const copyExport = async () => {
    if (exportWrap.classList.contains("tc-hidden")) showExport();
    exportText.focus();
    exportText.select();
    try {
      await navigator.clipboard.writeText(exportText.value);
      meta.textContent = "Copied export to clipboard ✅";
    } catch {
      // Fallback: selection already done
      meta.textContent = "Select + copy (Ctrl/Cmd+C) ✅";
    }
  };

  const clearChecks = () => {
    // Clear saved state and uncheck all (does NOT rewrite file until export+commit)
    localStorage.removeItem(STORAGE_KEY);
    const cbs = Array.from(body.querySelectorAll(".tc-task input[type=checkbox]"));
    cbs.forEach(cb => { cb.checked = false; cb.dispatchEvent(new Event("change")); });
    meta.textContent = "Cleared ✅";
  };

  // Wire buttons
  btnExport.addEventListener("click", showExport);
  btnCopy.addEventListener("click", copyExport);
  btnClear.addEventListener("click", clearChecks);

  // If we found tasks, show controls
  if (taskCount > 0) {
    controls.classList.remove("tc-hidden");
    meta.textContent = `Loaded ✅ (${countDone()}/${taskCount})`;
  }
})();
</script>
