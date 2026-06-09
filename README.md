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
│   ├── poor-prompt/           Bare-bones output from a vague prompt
│   ├── better-prompt/         Improved output with more specificity
│   └── excellent-prompt/      Full-featured output from a detailed prompt
│
├── demo-03-community-finder/  Session 9 — Community Impact (Broome County)
│   ├── prototype/             AI-generated first draft
│   └── polished/              Human-refined, grant-ready version
│
├── demo-04-broken-project/    Session 4 — Context Drift
│   ├── v1-clean/ … v7-full-degradation/   Progressive degradation
│   └── v8-recovered/          Recovered with architecture document
│
└── demo-05-production-ready/  Session 8 — Deployment
    └── FACILITATOR-NOTES.md   Build prompts + release setup guide
                               (live app: GitHub Prof-Z-BCC/advisor-intake-platform)
```

---

## How to Use These Examples

### For Facilitators
Demos 01–04 contain `index.html` files you can open directly in a browser — no server required. Demo 05 is a deployed Next.js application; see its `FACILITATOR-NOTES.md` for the live URL and release setup steps.

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

## Demo 02 — Volunteer Management App (Session 3)

Three versions of the same application, each produced by a prompt of different quality. Open each `index.html` side by side to compare.

| Folder | Prompt quality | What's missing |
|--------|----------------|----------------|
| `poor-prompt/` | "Make a volunteer app." | No persistence, no validation, no accessibility |
| `better-prompt/` | Named the features explicitly | No persistence, incomplete accessibility |
| `excellent-prompt/` | Specified behavior, accessibility, and constraints | Nothing critical — this is production-adjacent |

See `FACILITATOR-NOTES.md` for the full prompts and discussion questions.

---

## Demo 03 — Community Resource Finder (Session 9)

Two versions of a Broome County community resource finder, showing the difference between an AI-generated first draft and a human-refined version.

| Folder | Description |
|--------|-------------|
| `prototype/` | Functional first draft — plain layout, basic search |
| `polished/` | Grant-ready version — branded, accessible, SUNY Broome data |

The polished version is appropriate to show to grant reviewers and community stakeholders. See `FACILITATOR-NOTES.md` for talking points.

---

## Demo 05 — Review Retention Credits Wizard (Session 8)

A real, deployed application — not a teaching toy. Built to handle student intake for retained credit review at SUNY Broome Community College.

**What it does:** Multi-step intake form → decision engine → routed emails to advisors + student confirmation + BCC audit trail.

**Tech stack:** Next.js, TypeScript, Tailwind CSS, Resend (email), Vercel (deployment)

**Live app:** `https://advisor-intake-platform-professor-z-s-projects.vercel.app/intake`  
**Source:** `github.com/Prof-Z-BCC/advisor-intake-platform`

The `FACILITATOR-NOTES.md` contains the 4 streamlined build prompts, an honest account of what the build actually looked like (including debugging and deployment friction), and complete step-by-step release setup instructions.

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
