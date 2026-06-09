# Demo 03 — Broome County Community Resource Finder
## Facilitator Notes

**Session:** 9 — AI for Community Impact  
**Time:** ~30 minutes  
**Purpose:** Show what AI-assisted development can produce for real community needs — and demonstrate the gap between "AI-built first draft" and "human-refined, production-ready tool."

This demo also serves as the primary **grant showcase example**. When presenting to community stakeholders or grant reviewers, open the polished version.

---

## The Two Versions

### prototype/index.html
**The AI-generated first draft.**

Built from a moderately detailed prompt in a single pass. Functional but rough:
- Plain table-style card layout
- Basic search and category dropdown
- No persistence (refreshing resets the view — acceptable for a finder tool, but no favorites or history)
- Accessible in structure (labels, semantic HTML) but not polished for screen readers
- Mobile rendering is passable but not optimized

**What to say to students:**
> "This is what you get in about 5 minutes. It works. A real community member could use this. But would you hand it to your organization's website?"

---

### polished/index.html
**The human-refined version.**

The same data and functionality, rebuilt with intentional design decisions:
- Skip-to-content link (critical for screen reader and keyboard users)
- Header with gradient and visible search bar
- Category filter chips with `aria-pressed` state (proper for toggle buttons)
- Color-coded cards by category with accessible color contrast
- `aria-live="polite"` on the results count — screen readers announce changes
- Phone numbers are tappable `tel:` links (mobile-first)
- Website links open in new tab with `aria-label` warning ("opens in new tab")
- Responsive grid: 1 column on mobile, multi-column on desktop
- Empty state with clear guidance
- Footer credits SUNY Broome and notes the demo disclaimer

**What to point out:**
- Every one of these improvements was a human decision
- The AI can make any of them *if told to* — but it won't volunteer them
- This is the difference between "vibe coded" and "thoughtfully built"

---

## Grant Reviewer Talking Points

If showing this as part of an "AI for the Greater Good" grant pitch:

1. **Real community data.** This uses actual Broome County organizations — Food Bank, Rescue Mission, Southern Tier Independence Center, SUNY Broome, etc. It's not hypothetical.

2. **Built by learners.** A student with 10 weeks of training could build the prototype version. The polished version models what they could aspire to.

3. **Directly serves residents.** A tool like this could be deployed today. The barrier isn't technology — it's knowing *what to ask for* and *what to check*.

4. **Demonstrates the human-in-the-loop principle.** The AI generated both versions. A human decided which one to ship. That judgment is what this program teaches.

---

## Discussion Questions

1. **Which version would you trust more as a community member?** Why? What specifically makes the difference?

2. **What's missing from even the polished version?**  
   (Real-time data, search by zip code, multilingual support, submit-a-resource form, screen reader testing with actual users...)

3. **Who maintains this?** The AI built it. A community organization needs to keep the data current. What does that workflow look like?

4. **What could go wrong if this was deployed as-is?**  
   (Outdated phone numbers, organizations that closed, no one monitoring it...)

5. **What would it take to make this genuinely production-ready?**  
   This is a good lead-in to Session 8 (Deployment).

---

## ADA / Accessibility Notes (Title II)

The polished version includes:
- Skip-to-content link (first focusable element on the page)
- `role="search"` and `aria-label` on the search region
- `role="group"` and `aria-labelledby` on the filter chip group
- `aria-pressed` on toggle buttons (not `aria-selected`, which is for listbox)
- `aria-live="polite"` and `aria-atomic="true"` on the results count
- `aria-label` on cards' action buttons naming the specific resource
- `tel:` links for phone numbers (tappable on mobile, readable by screen readers)
- All SVG icons marked `aria-hidden="true"` (decorative)
- `<article>` elements for each resource card (screen reader landmark)
- `<h2>` for card titles (within `<main>` under `<h1>`) — correct heading hierarchy

The prototype version deliberately omits most of these — that contrast is part of the lesson.

---

## Timing Suggestion

| Activity | Time |
|----------|------|
| Open prototype — let students react | 3 min |
| Walk through the prototype code — what works | 4 min |
| Open polished — side-by-side if possible | 4 min |
| Walk through key accessibility improvements | 6 min |
| Discussion questions (pick 2–3) | 8 min |
| Closing: "What would it take to deploy this for real?" | 5 min |
