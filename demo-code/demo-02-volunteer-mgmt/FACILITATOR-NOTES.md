# Demo 02 — Volunteer Management App
## Facilitator Notes

**Session:** 3 — From Idea to Architecture  
**Time:** ~25 minutes  
**Purpose:** Show how prompt specificity directly changes output quality — and what that means for building real software.

---

## The Three Versions

### poor-prompt/index.html
**Prompt used:**
```
Make a volunteer app.
```

**What the AI produced:**
- Bare-bones form with name and email only
- No data persistence — refresh = lost data
- No input validation (empty submissions accepted)
- No semantic HTML, no labels, not keyboard-navigable
- Inline `var` and no separation of concerns
- Works in the most minimal sense — but barely

**What to point out to students:**
- The AI is not wrong — it made *something*
- "It does what I asked" is not the same as "it does what I needed"
- Notice what's missing: you have to know what to ask for

---

### better-prompt/index.html
**Prompt used:**
```
Create a volunteer management application with HTML, CSS, and JavaScript.
Include a form to add volunteers with name and email, and a list showing all volunteers.
```

**What improved:**
- Styled and readable
- Basic input validation (empty field check)
- XSS protection via `escapeHtml()`
- Labels connected to inputs
- Empty state handled gracefully
- Better visual hierarchy

**What's still missing:**
- No data persistence (still loses data on refresh)
- No phone or availability fields
- No ability to mark volunteers active/inactive
- Accessibility is better but not complete (no focus management, no ARIA live regions)

**What to point out:**
- More detail = more complete output
- But there's still a gap between "better" and "production-ready"
- The developer still has to *know what to ask for*

---

### excellent-prompt/index.html
**Prompt used:**
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

**What improved:**
- All required fields (name, email, phone, availability)
- Active/inactive toggle with visual badge
- localStorage persistence — data survives refresh
- Full ARIA labels, live regions, focus management
- Responsive grid layout (desktop + mobile)
- Email validation with regex
- Summary bar (total / active / inactive count)
- Error messaging with `role="alert"` for screen readers

**What to point out:**
- This is the closest to something you'd actually hand to a user
- The prompt specified *behavior, not just features* — "keyboard-navigable", "persists on refresh"
- Even this version would need more work for real deployment: no server, no auth, no real database

---

## Discussion Questions

1. **What's the cost of a bad prompt?**  
   Not just "less features" — it's the hidden assumptions. The poor-prompt version *works*. A student might ship it.

2. **What do you have to know to write the excellent prompt?**  
   You have to know about accessibility, persistence, validation, responsive design — *before* you ask. The AI doesn't volunteer those requirements.

3. **Where does the excellent version still fall short?**  
   No authentication, no server, no real database. Anyone with the URL can see all volunteers. What does that mean for real community use?

4. **What's the role of the human here?**  
   The AI wrote all three versions. The human decides which one is acceptable to ship.

---

## ADA / Accessibility Notes (Title II)

The excellent version includes:
- `<label>` elements associated with every form field via `for`/`id` pairs
- `<fieldset>` and `<legend>` for the checkbox group
- `aria-required="true"` on required fields
- `role="alert"` and `aria-live="polite"` for dynamic error and status messages
- `scope="col"` on table headers
- `aria-label` on action buttons that include the volunteer's name
- Focus management after form submission

The poor and better versions deliberately omit many of these — that's the point.

---

## Timing Suggestion

| Activity | Time |
|----------|------|
| Show poor-prompt live (open in browser) | 3 min |
| Ask "what's wrong?" — let students observe | 4 min |
| Show better-prompt, compare | 4 min |
| Show excellent-prompt, walk through features | 6 min |
| Discussion questions | 8 min |
