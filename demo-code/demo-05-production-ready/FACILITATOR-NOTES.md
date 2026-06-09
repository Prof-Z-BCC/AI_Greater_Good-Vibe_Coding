# Demo 05 — Review Retention Credits Wizard
## Facilitator Notes

**Session:** 8 — Deploying AI-Built Apps  
**Time:** ~35 minutes  
**Purpose:** Show a complete, real-world AI-assisted application that was built, tested, and deployed to production — including the messy parts (DNS, environment variables, email routing, hydration bugs) that tutorials skip.

This is not a toy. It was built for SUNY Broome's advising office, is deployed on Vercel, and sends real emails to real advisors.

---

## What the App Does

A multi-step intake wizard for students requesting a review of retained credits. Students complete a 6-step form covering:

1. Student identification (name, email, last name for advisor routing)
2. Credit load and course history
3. Work hours and outside obligations
4. Caregiving and life circumstances
5. Academic support usage
6. Review and submit

On submission, the app's decision engine evaluates the responses and assigns a risk band (low / moderate / high / critical). Two emails go out automatically:

- **Student:** Confirmation with their submitted summary and recommended next steps
- **Advisor:** Full intake report with risk band, rationale bullets, and the student's contact info — routed by last name (A–K → Advisor A, L–Z → Advisor B)
- **BCC:** Sent silently to the department for audit/monitoring

The UI uses SUNY Broome's official brand colors (Soft Black `#232323`, Gold `#EEB310`, Realistic Gold `#d3bc8d`, White Cap `#fbfaf5`) and web-safe approximations of the official typography.

**Live URL:** `https://advisor-intake-platform-professor-z-s-projects.vercel.app/intake`  
**GitHub:** `Prof-Z-BCC/advisor-intake-platform`

---

## The Build Prompts (Streamlined)

These are the key prompts used to build this application from scratch. They are condensed for presentation — the actual build involved iteration and debugging, which is part of the story.

---

### Prompt 1 — Project scaffolding

```
Create a Next.js application for a student intake wizard for retained credit review.

Requirements:
- Multi-step form wizard with 6 steps: student info, course load, work obligations, 
  caregiving/life circumstances, support usage, and a review/submit step
- Each step is a separate component; a WizardShell manages state and navigation
- TypeScript throughout; define a shared IntakeFormData type in lib/schema.ts
- All form fields must have labels and be keyboard-navigable (WCAG 2.1 AA)
- Use Tailwind CSS for styling; no component libraries

Do not implement email sending yet. Just the form structure, state management, and 
a final review step that displays all collected answers before submission.
```

**What this produced:** The complete wizard UI — all six step components, the WizardShell state machine, TypeScript types, and a working multi-step form with back/next navigation.

---

### Prompt 2 — Decision engine

```
Add a decision engine to lib/decisionEngine.ts.

It should take a completed IntakeFormData object and return:
- A riskLevel: "low" | "moderate" | "high" | "critical"
- A creditBand: the recommended maximum credits for the next term
- An array of rationale strings explaining the recommendation

Rules:
- Students working 30+ hours/week: increase risk
- Students with caregiving responsibilities: increase risk  
- Students who have not used tutoring or advising: increase risk
- Students with prior withdrawal or academic holds: increase risk
- Risk levels compound — multiple factors push toward higher risk bands

Keep the logic transparent and readable. Each rule should be a named function 
or clearly labeled conditional so a non-developer can read and modify the rules.
```

**What this produced:** A clean, readable decision engine that a non-developer (advisor, administrator) could open and modify.

---

### Prompt 3 — Email routing

```
Add a POST API route at app/api/intake/route.ts that:

1. Accepts the completed IntakeFormData and decision engine output
2. Sends two emails using the Resend library:
   - A student confirmation email to the address they provided
   - An advisor summary email routed by last name:
     - Last names A–K → process.env.ADVISOR_A_EMAIL
     - Last names L–Z → process.env.ADVISOR_B_EMAIL
3. Sends a BCC of the advisor email to process.env.INTAKE_BCC_EMAIL

Email templates should be in lib/emailTemplates.ts as separate functions 
that return HTML strings. Use inline styles only (no CSS classes) — 
most email clients strip external CSS.

Required environment variables:
- RESEND_API_KEY
- FROM_EMAIL
- ADVISOR_A_EMAIL
- ADVISOR_B_EMAIL
- INTAKE_BCC_EMAIL
```

**What this produced:** The full email routing system including HTML email templates with inline styles.

---

### Prompt 4 — Brand styling

```
Apply SUNY Broome Community College's official brand colors and typography 
to the entire application.

Colors:
- Soft Black: #232323 (primary text, backgrounds)
- Gold: #EEB310 (primary accent, active states, buttons)
- Realistic Gold: #d3bc8d (secondary accents, borders)
- White Cap: #fbfaf5 (page background, card surfaces)

Typography:
- Albertus Titling is the official headline font but is a commercial print font — 
  use Playfair Display (Google Fonts) as the closest free web equivalent
- Chaparral Pro is the official body font — use EB Garamond (Google Fonts) as 
  the approved alternative
- Buttons: #232323 text on #EEB310 background (required for WCAG contrast)

Apply consistently to: wizard UI (all 6 steps), WizardShell, email templates.
Create a shared styleConstants.ts in lib/ so components don't hardcode values.
```

**What this produced:** A branded application and matching email templates in SUNY Broome's palette.

---

## What the Build Process Actually Looked Like

This is the honest version — worth sharing with students:

- **4 main prompts** built the core application
- **~12 follow-up exchanges** fixed real issues: a React hydration mismatch on back-navigation, a git index.lock blocking commits, DNS records that weren't propagating, email deliverability from `onboarding@resend.dev` vs. a verified domain
- **DNS debugging took longer than coding** — Network Solutions auto-appending domains to DNS host fields is a common, non-obvious failure mode
- **The AI caught its own security issue** — flagged that the Resend API key had appeared in the chat and recommended rotating it
- **One bug the AI introduced:** `WizardShell` needed a `key` prop to prevent hydration mismatches on browser back-navigation — this was a subtle React SSR issue, not a visible crash

**What to tell students:** The 4 prompts above took maybe 45 minutes. The debugging and deployment took 3–4 hours spread across two sessions. That ratio is realistic for a first deployment.

---

## Release Setup — Step by Step

These are the steps to take this from a fresh GitHub clone to a live, sending application. Written for a developer new to deployment.

### Prerequisites

- Node.js 18+ installed locally
- A GitHub account
- A [Vercel](https://vercel.com) account (free tier is fine)
- A [Resend](https://resend.com) account (free tier sends 3,000 emails/month)
- A domain you control, or willingness to use `onboarding@resend.dev` for testing

---

### Step 1 — Clone and install locally

```bash
git clone https://github.com/Prof-Z-BCC/advisor-intake-platform.git
cd advisor-intake-platform
npm install
```

---

### Step 2 — Create your environment file

Copy the example:

```bash
cp .env.example .env.local
```

Open `.env.local` and fill in all five values:

```
RESEND_API_KEY=re_xxxxxxxxxxxxxxxxxxxx
FROM_EMAIL=intake@yourdomain.com
ADVISOR_A_EMAIL=advisor.a@institution.edu
ADVISOR_B_EMAIL=advisor.b@institution.edu
INTAKE_BCC_EMAIL=you@institution.edu
```

For local testing you can use `FROM_EMAIL=onboarding@resend.dev` — Resend allows this without domain verification.

---

### Step 3 — Test locally

```bash
npm run dev
```

Open `http://localhost:3000` — it will redirect to `/intake`. Go through the full wizard and submit. Check that emails arrive. If they don't, check the terminal for API errors.

---

### Step 4 — Verify your sending domain in Resend

Skip this step if using `onboarding@resend.dev` for testing. To send from your own domain:

1. Go to [resend.com](https://resend.com) → **Domains** → **Add Domain**
2. Enter your domain (e.g., `yourdomain.com`)
3. Resend gives you three DNS records to add — a DKIM TXT, an SPF TXT, and an MX record
4. Add them in your DNS provider's control panel

**Common DNS pitfall:** Many providers (Network Solutions, iPage, GoDaddy) auto-append your domain to the Host field. If Resend gives you host `resend._domainkey`, enter exactly that — not `resend._domainkey.yourdomain.com`. The provider adds the domain automatically.

After adding records, click **Verify** in Resend. Propagation takes 5–60 minutes depending on your provider. Use `nslookup -type=TXT resend._domainkey.yourdomain.com` to confirm records are live before clicking Verify.

---

### Step 5 — Deploy to Vercel

1. Go to [vercel.com/new](https://vercel.com/new) → Import from GitHub
2. Select `advisor-intake-platform`
3. Before clicking Deploy, expand **Environment Variables** and add all five:

| Name | Value |
|------|-------|
| `RESEND_API_KEY` | Your Resend API key |
| `FROM_EMAIL` | `intake@yourdomain.com` (or `onboarding@resend.dev` for testing) |
| `ADVISOR_A_EMAIL` | First advisor's email |
| `ADVISOR_B_EMAIL` | Second advisor's email |
| `INTAKE_BCC_EMAIL` | Your monitoring/audit email |

4. Click **Deploy**. Build takes ~90 seconds.

---

### Step 6 — Find your stable production URL

After deploying, Vercel gives you two kinds of URLs:

- **Preview URL** (changes every deploy): `your-project-abc123-username.vercel.app` — do not share this
- **Production URL** (stable): `your-project-username.vercel.app` — this is what you share

Find it: Vercel dashboard → your project → **Domains** section, or click the **Visit** button on the project overview page.

The intake form is at `[production-url]/intake`. Share that URL — the root `/` has nothing on it.

---

### Step 7 — End-to-end test before going live

Submit the form using your own email address. Verify:

- Confirmation screen appears after submitting
- Student confirmation email arrives
- Advisor email arrives (check routing: A–K goes to Advisor A, L–Z goes to Advisor B)
- BCC copy arrives at `INTAKE_BCC_EMAIL`

If emails don't arrive: Vercel → **Deployments** → current deployment → **Functions** → find the `/api/intake` POST → check the response for Resend error messages.

---

### Step 8 — Rotate your API key

Before sharing the app publicly: go to [resend.com](https://resend.com) → **API Keys** → delete the key you used for development → create a new one → update `RESEND_API_KEY` in Vercel Environment Variables → redeploy.

This is especially important if the key appeared in any chat history, terminal output, or screenshot.

---

## Customizing the App

### Change advisor routing

Open `app/api/intake/route.ts` and find the routing logic. The current split is A–K / L–Z — change the condition to any rule that fits your department.

### Change credit band recommendations

Open `lib/decisionEngine.ts`. Each risk factor is a clearly labeled conditional. The rules are written to be readable by non-developers — no CS background needed to adjust the thresholds.

### Add more advisors

Add environment variables (`ADVISOR_C_EMAIL`, etc.), update the routing logic in `route.ts`, and adjust the last-name ranges.

### Change the FROM address

Update `FROM_EMAIL` in Vercel Environment Variables and redeploy. No code changes needed.

---

## ADA / Accessibility Notes (Title II)

The application was built to WCAG 2.1 AA:

- All form fields have associated `<label>` elements
- Required fields use `aria-required="true"`
- Error messages use `role="alert"` for screen reader announcement
- The wizard step indicator uses `aria-current="step"` 
- All interactive elements are keyboard-navigable (Tab, Enter, Space)
- Color contrast: `#232323` text on `#EEB310` = 5.6:1 (passes AA for normal text)
- Color contrast: `#232323` text on `#fbfaf5` = 16.4:1 (passes AAA)
- No information is conveyed by color alone

The email templates use inline styles only (required for email client compatibility) and are structured with readable heading hierarchy for email screen readers.

---

## Discussion Questions

1. **What made this deployable vs. just "running on your computer"?**  
   Environment variables, a verified sending domain, a production host. The code was the same — the infrastructure is what made it real.

2. **What would break this if a department handed it off to someone else?**  
   The API keys are in Vercel, not the code. The DNS is on a personal domain. The routing is hardcoded to two specific advisors. A real handoff requires documentation and institutional ownership of the domain.

3. **Who maintains this after it ships?**  
   The AI built it. It won't update itself when advisor emails change or when students report bugs. What does a maintenance plan look like for an AI-built tool?

4. **What would IT say about this?**  
   No SUNY infrastructure was used. Emails come from a personal domain. Student data is passing through Resend (a third-party service). Is that appropriate? What would institutional deployment look like instead?

5. **This took 4–5 hours total. What would the same project cost to commission from a developer?**  
   A reasonable estimate for a contracted developer: $1,500–$3,000. What does that mean for community organizations with limited budgets?

---

## Timing Suggestion

| Activity | Time |
|----------|------|
| Open live app, walk through wizard | 5 min |
| Show the 4 build prompts — what each produced | 8 min |
| "What the build actually looked like" — the honest version | 5 min |
| Walk through release setup steps (overview, not full demo) | 7 min |
| Discussion questions (pick 3) | 10 min |
