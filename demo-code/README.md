# AI for the Greater Good — Demo Repository

**Program:** AI-Assisted Software Development for the Greater Good  
**Series:** 10-Session Public Curriculum  
**License:** Public Domain / CC0 — Free to use, adapt, and share

---

## Repository Structure

This repository contains reference code for all live demonstrations across the 10-session series. Each demo folder contains the same project at different maturity levels, showing both what AI does well and where human judgment is essential.

```
demo-repo/
├── demo-01-todo-app/          Session 1 — What is Vibe Coding?
│   ├── good/                  Working, clean implementation
│   ├── obvious-flaws/         Errors a beginner can spot
│   └── subtle-flaws/          Errors that look fine but aren't
│
├── demo-02-volunteer-mgmt/    Session 3 — Architecture
├── demo-03-community-finder/  Session 9 — Community Impact
├── demo-04-broken-project/    Session 4 — Context Drift
└── demo-05-production-ready/  Session 8 — Deployment
```

---

## How to Use These Examples

### For Facilitators
Each folder contains an `index.html` you can open directly in a browser — no server required for basic demos. More advanced examples note when a local server is needed.

### For Participants
Try running each version yourself. Ask yourself:
- Does it work?
- Does it work *correctly*?
- What's missing?
- Would you trust this with real user data?

### The Core Lesson
AI generates **working code** quickly. That is genuinely useful.  
Working code is not the same as **correct, secure, or complete** code.  
Your job is to be the human in the loop.

---

## Demo 01 — To-Do App (Session 1)

**The prompt used:**
```
Create a simple to-do application with HTML, CSS, and JavaScript.
```

Three versions of the same app:

| Folder | Description | What to look for |
|--------|-------------|-----------------|
| `good/` | Clean, functional implementation | Data persists on refresh, accessible markup, readable code |
| `obvious-flaws/` | Broken in ways beginners can spot | App crashes, buttons don't work, visual errors |
| `subtle-flaws/` | Broken in ways that look fine | Runs without errors but loses data, has XSS vulnerability, inaccessible to screen readers |

---

## Prompt Quality Examples (Session 3)

These prompts were used to demonstrate how specificity changes output quality.

**Poor prompt:**
```
Make a volunteer app.
```

**Better prompt:**
```
Create a volunteer management application with HTML, CSS, and JavaScript.
Include a form to add volunteers with name and email, and a list showing all volunteers.
```

**Excellent prompt:**
```
Create a volunteer management application using HTML, CSS, and JavaScript.

Requirements:
- A form to register new volunteers with fields for: full name, email address, phone number, 
  and availability (checkboxes for: Monday, Wednesday, Friday, Weekend)
- A volunteer list that displays all registered volunteers in a table
- The ability to mark a volunteer as active or inactive
- Data should persist when the page is refreshed (use localStorage)
- The interface should be accessible: all form fields must have labels, 
  the table must have proper headers, and all interactive elements must be keyboard-navigable
- Use semantic HTML5 elements
- The design should be clean and work on both desktop and mobile screens

Do not use any external libraries or frameworks.
```

**What changes:** Specificity in prompts = specificity in output. The more you understand what you need, the better the result. This is why understanding software — even at a basic level — makes you dramatically more effective with AI tools.

---

## Sessions Reference

| Session | Topic | Demo Used |
|---------|-------|-----------|
| 1 | What is Vibe Coding? | demo-01 (all three versions) |
| 2 | Free Tools | demo-01/good (live build) |
| 3 | From Idea to Architecture | demo-02 (prompt comparison) |
| 4 | File Alignment & Context Drift | demo-04 (intentional degradation) |
| 5 | Why AI Code Can't Be Trusted | demo-01/subtle-flaws (security review) |
| 6 | Validation & Testing | demo-01/subtle-flaws (test writing) |
| 7 | Security & Privacy | demo-02 (vulnerability demo) |
| 8 | Deploying AI-Built Apps | demo-05 (full deployment) |
| 9 | AI for Community Impact | demo-03 (community use case) |
| 10 | The Future Workforce | all demos (retrospective) |
