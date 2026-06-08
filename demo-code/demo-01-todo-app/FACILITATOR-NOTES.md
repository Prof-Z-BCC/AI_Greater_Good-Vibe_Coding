# Demo 01 — Facilitator Notes
## To-Do App: Three Versions

---

## The Prompt Used in Session 1

```
Create a simple to-do application with HTML, CSS, and JavaScript.
```

This is an **intentionally vague prompt**. Use it to show participants that AI generates *something* — but what it generates depends heavily on what you asked for.

---

## Running the Demo (Session 1, ~10–15 minutes)

1. Open `good/index.html` in your browser. Show it works — add tasks, check them off, refresh. Data persists.
2. Open `obvious-flaws/index.html`. Click "Add." Nothing happens. Ask participants: "What's wrong?"
3. Open `subtle-flaws/index.html`. It looks identical to the good version. Add a task — it works. Mark one done — it works. **Then enter this as a task:**

   ```
   <img src=x onerror="alert('XSS — your data could be stolen')">
   ```

   An alert box pops up. Ask: "Would you have caught that before shipping this app?"

---

## Discussion Points

### Obvious Flaws → "AI code needs to be tested, not just run"
- The app *looks* fine in the code editor
- It only fails when you *use* it
- Lesson: run the code, click every button, try edge cases

### Subtle Flaws → "Working code is not safe code"
- The app *works* — tasks are added, saved, displayed
- The XSS vulnerability requires someone to know what to look for
- Lesson: security review is a separate skill from writing code — Sessions 5 and 7

### Good Version → "This is what the full prompt should have specified"
- The good version required a **much more detailed prompt** to produce
- It also required the developer to know what *accessibility* means (aria labels, keyboard nav)
- Lesson: prompt quality = output quality — Session 3

---

## The Prompt Quality Lesson (Session 3)

Show what happens when you improve the prompt progressively:

**Level 1 (vague):**
```
Make a to-do app.
```

**Level 2 (functional):**
```
Create a to-do app with HTML, CSS, and JavaScript. Users should be able to add tasks, 
mark them as complete, and delete them.
```

**Level 3 (specific):**
```
Create a to-do application with HTML, CSS, and JavaScript.

Requirements:
- Input field with a submit button to add tasks
- Tasks display in a list below the form
- Each task has a checkbox to mark complete and an X button to delete
- Completed tasks should have a strikethrough style
- Data must persist when the page is refreshed (use localStorage)
- All interactive elements must be keyboard accessible
- All form inputs must have visible labels
- No external libraries
```

**What participants observe:**
- Level 1 might produce the "obvious flaws" version
- Level 2 improves it but misses accessibility and persistence
- Level 3 produces something close to the "good" version

**Key insight:** The better your prompt, the more you understand the problem. 
Understanding the problem *is* the skill. AI helps you build — but you have to know what to build.
