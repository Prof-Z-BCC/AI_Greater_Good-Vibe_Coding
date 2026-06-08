# Demo 04 — Facilitator Notes
## Context Drift: A Project That Degrades Over Time

---

## The Scenario

We're building a **community task manager** — a simple app for a small nonprofit to track tasks assigned to volunteers. We start with a clean, well-structured codebase and simulate what happens when we continue development across multiple AI sessions without anchoring context.

Each version represents one or more AI sessions later. Open them in order and walk participants through the degradation.

---

## Versions at a Glance

| Version | State | Key Problem Introduced |
|---------|-------|----------------------|
| v1-clean | ✅ Clean | Starting point. Well-structured, consistent naming. |
| v2-first-feature | ✅ Good | Tags added correctly — context was still fresh. |
| v3-naming-drift | ⚠️ Drift begins | Same concept now called 'task', 'item', and 'todo' in different places. |
| v4-duplicate-logic | ⚠️ Worsening | Two render functions. Both called. One overrides the other. |
| v5-growing-file | ⚠️ Structural | Single file is now 400+ lines. Functions poorly named. |
| v6-conflicting-models | ❌ Breaking | Data model changed mid-project. Old and new structures coexist. |
| v7-full-degradation | ❌ Broken | App partially works. Bugs are invisible until specific actions. |
| v8-recovered | ✅ Recovered | ARCHITECTURE.md added. Code refactored. Naming consistent. |

---

## How to Run the Demo (Session 4, ~20 minutes)

1. **Open v1-clean/index.html** — show participants the clean state. "This is day one."
2. **Open v2-first-feature/index.html** — "Session 2, still clean. AI had full context."
3. **Open v3-naming-drift/index.html** — *search the code for 'task', 'item', and 'todo'.* All three appear for the same concept. "This is drift beginning."
4. **Open v4-duplicate-logic/index.html** — show both `renderTasks()` and `displayItems()` in the code. "Which one runs? Both. One overwrites the other."
5. **Open v5-growing-file/index.html** — scroll through the code. "How long before you can't find anything?"
6. **Open v6-conflicting-models/index.html** — show the console errors. Open DevTools and demonstrate the data model mismatch.
7. **Open v7-full-degradation/index.html** — add a task, mark it complete, then try to filter. "It looks like it works. But it doesn't — not reliably."
8. **Open v8-recovered/index.html** AND **open ARCHITECTURE.md** — show them side by side. "This document is what prevents everything we just saw."

---

## Key Discussion Questions

- "At which version would you have shipped this to real users?"
- "At which version did you *notice* the drift?"
- "What would you need to catch v3-naming-drift before it became v7-full-degradation?"

---

## The Prompt That Caused Each Version's Problem

**v3 (naming drift) — the prompt that started it:**
```
Add a way to mark tasks as high priority.
```
*(No context about existing naming — AI introduced 'item' and 'todo' as synonyms)*

**v4 (duplicate logic) — the prompt:**
```
The list isn't updating when I add a task. Fix it.
```
*(AI added a second render function instead of debugging the first)*

**v5 (growing file) — the prompt:**
```
Add categories, due dates, and a search bar.
```
*(Three features in one prompt, no file structure guidance)*

**v6 (conflicting models) — the prompt:**
```
Change tasks to have an assignee field — a volunteer's name.
```
*(AI updated the data model but didn't migrate existing data or update all rendering logic)*

---

## The Fix: What v8 Did Differently

1. Created ARCHITECTURE.md *before* any new feature work
2. Every subsequent prompt started with: "Refer to ARCHITECTURE.md for the data model, file structure, and function names"
3. Explicitly told AI: "Do not add new render functions — update renderTask() in app.js"
4. Reviewed code after each AI session before starting the next
