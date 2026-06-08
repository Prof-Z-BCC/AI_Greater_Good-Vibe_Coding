# ARCHITECTURE.md — Community Task Manager
# Created before Session 8 to anchor all future AI prompts

## Purpose
This document defines the authoritative data model, file structure, function names,
and naming conventions for the Community Task Manager application.
Include this document's contents in every AI prompt that modifies this codebase.

---

## Data Model

```
Task {
  id:         number    — Date.now(), unique identifier
  title:      string    — required, non-empty
  assignee: {
    name:     string    — required
    email:    string    — optional, validated format if provided
  }
  status:     string    — 'pending' | 'complete'
  priority:   string    — 'low' | 'medium' | 'high' | 'urgent'
  category:   string    — 'general' | 'outreach' | 'admin' | 'events'
  dueDate:    string    — YYYY-MM-DD format, nullable
  createdAt:  string    — ISO 8601 (new Date().toISOString())
}
```

## Storage
- Key: `'tasks'` in localStorage
- Value: JSON.stringify(Task[])
- Read:  JSON.parse(localStorage.getItem('tasks') || '[]')

---

## File Structure

```
v8-recovered/
  index.html    — HTML structure only, no inline scripts, no inline styles
  styles.css    — All styles
  app.js        — All JavaScript
  ARCHITECTURE.md — This file
```

---

## Key Functions (do not duplicate — update in place)

| Function | Location | Purpose |
|----------|----------|---------|
| `renderTasks()` | app.js | Renders the full task list based on current state.filter and state.searchQuery |
| `saveTasks()` | app.js | Writes state.tasks to localStorage |
| `loadTasks()` | app.js | Reads tasks from localStorage into state.tasks |
| `addTask(data)` | app.js | Pushes a new task, saves, and re-renders |
| `deleteTask(id)` | app.js | Removes task by id, saves, and re-renders |
| `setFilter(value)` | app.js | Sets state.filter, re-renders |
| `setSearch(query)` | app.js | Sets state.searchQuery, re-renders |
| `sortTasks(field)` | app.js | Sorts a COPY of state.tasks, re-renders (does NOT mutate state.tasks) |
| `escapeHtml(str)` | app.js | XSS prevention — use for all user-provided content |

---

## State Object

```javascript
const state = {
  tasks: [],          // loaded from localStorage
  filter: 'all',      // 'all' | 'pending' | 'complete'
  searchQuery: '',    // current search string
};
```

---

## Naming Conventions

- All references to the data entity: **task** (not 'item', 'todo', 'record')
- Variable names: camelCase
- CSS classes: kebab-case
- Form field IDs: task-title, assignee-name, assignee-email, task-status, task-priority, task-category, task-due
- Function parameters: descriptive names matching the Task model fields

---

## CSS Classes for Task Badges

```
.status-pending    — yellow background
.status-complete   — green background
.priority-low      — blue background
.priority-medium   — amber background
.priority-high     — red background
.priority-urgent   — dark red background
```

---

## Constraints

- No external libraries or frameworks
- All form fields must have visible <label> elements
- innerHTML: only used with escapeHtml() applied to all user content
- sortTasks(): must sort a copy — never mutate state.tasks directly
- filter and search must compose: both can be active simultaneously
