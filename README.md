# Vaibhav Raj — Product Portfolio

Single-file static site. `index.html` is the whole thing — inline CSS, inline JavaScript, no build step,
no dependencies. Fonts load from Google Fonts. Drop it on GitHub Pages and it works.

The product snapshots are not pictures. Each one runs: you can replay the compliance engine,
open the evidence behind any single check, drag a confidence threshold, shorten a forecast
horizon until the warning disappears. There is also an **Ask about my work** panel that
answers questions from a written answer bank — no model, no server. The interaction is the
argument — see [the interactive layer](#the-interactive-layer).

Design takes its cues from zapier.com: warm off-white ground, orange accent, geometric sans,
large rounded product shots, generous spacing. Light theme only, by choice.

```
index.html            the site
Vaibhav_Raj_PM.pdf    linked from the hero and the contact block
README.md             this file
```

## Page structure

| Section | What it does |
|---|---|
| Nav | About · **Work** · **How I work** · Contact. Work and How I work open dropdown menus. Below 760px it collapses to a **Menu** button with everything nested inside |
| Hero | Name, role, one-line positioning, "currently" strip |
| About | **Who I am · Where I work · Background** — bio, employer card, timeline |
| Work | 5 entries. Three have full interface snapshots |
| How I work | Three tabs — **Principles** (four operating beliefs), **UX thinking** (a six-step walkthrough of one decision), **Toolkit** |
| Contact | Dark block with email, phone, LinkedIn, résumé. Email and phone have copy-to-clipboard buttons |
| Everywhere | Scroll progress bar and scroll-spy in the nav, a ⌘K / Ctrl-K quick-jump palette, count-up stats, back-to-top, and an **Ask about my work** panel |

Each project follows the same shape: **What I did** → **What it's achieved so far** → **product
snapshot** → (for the two big ones) **the call I'd defend**.

---

## The interactive layer

Five product snapshots, each one playable. Every control is built from the tokens already in
`:root` — no new colours, no new fonts — so the page looks the same standing still.

| Snapshot | What moves | What it is arguing |
|---|---|---|
| **01 Trade Compliance Engine** | **Replay the run** streams all 21 agents in, with a live clock and progress meter. Filter by verdict (All 21 / Clear 19 / Review 2). Click any agent to open the evidence it retained — authority, list version, matched entity, score, and the rule applied | Decomposition and defensibility. The evidence drawer *is* principle 04: a broker can show an auditor which list, which version, which match |
| **02 Maritime Single Window** | Click an agency to light up which canonical fields its form derives (green) and which it has to capture on its own (amber), with the derivation coverage computed per agency. The **Canonical model: Off** toggle shows the counterfactual: 47 field entries become 282 | Why the model came before the integrations. Coverage is the number that makes port 21 cheaper than port 1 |
| **03 AI Document Generation** | Eight document types, each with its own field set and confidence scores. Click a field to see which source documents it came from, highlighted in the sources row. Drag the **review threshold** and watch fields flip to human review | Principle 02, made draggable. One number decides how much work the model hands back, and that number comes from what "wrong" costs |
| **04 Cargo forecasting** | Switch the horizon between 3, 7 and 14 days. The confidence band tightens to ±4%, the bars beyond the horizon ghost out — and the 23 Oct breach disappears from view entirely | Horizon beats precision. The short horizon has the better number and is useless |
| **05 UPI reconciliation** | Step through all eight NPCI cycles (C6–C8 have not opened yet and say so). Switch the latency histogram between **Average** and **The tail** | The tail is the product. On the average scale the 0.013% that decides the cycle is invisible |

**Honesty note.** These are interface *recreations*, and making them move makes them feel more real
than they are. Each caption now says "interactive recreation" and says what is simulated — the 18.4s
compliance clock is explicitly played back faster than real time. The data is still invented. Keep
that labelling.

### The ask panel

A launcher in the bottom-right corner opens a question panel. Someone types a question, it is
matched against answers written in advance, and the matching answer comes back with a link into
the relevant part of the page and two related follow-ups.

**It is not a chatbot, and it says so.** There is no model, no API key, no server and no network
call — the whole thing is in this file. That is a deliberate choice rather than a shortcut, and
the panel has an answer explaining it:

> A plausible-sounding wrong answer about my own work, given to someone deciding whether to hire
> me, is an expensive failure — so I didn't build the thing that can produce one.

Which is principle 02 applied to the portfolio itself. **Keep that framing.** A portfolio that
claims AI product experience and ships a fake "AI chatbot" fails badly the moment an interviewer
opens the source; one that ships a deterministic answer bank and explains why reads as judgement.

What it costs to run: nothing. What can be abused: nothing. What it can invent: nothing.

#### Editing the answers

Everything lives in the `A` array in the **ASK PANEL** `<script>` at the end of `<body>`. One
object per answer:

```js
{ id: 'coverage',                       // referenced by other answers' rel
  t: 'Derivation coverage',             // shown on the follow-up chip
  k: ['derivation coverage', ...],      // what a reader might type
  go: ['#p2', 'See coverage per agency'],   // optional link into the page
  rel: ['metrics', 'msw'],              // up to two follow-ups offered after it
  a: '<p>...</p>' }                     // the answer, as HTML
```

`STARTERS` lists the five question chips shown before anyone types.

**How matching works:** the question is lowercased and stripped of punctuation, then scored
against every entry's keywords — a multi-word phrase scores `3 × its word count`, a single
word scores 3, a word from the entry's own title scores 1, and a question starting with "why"
gives the "Why…" answers 3. Highest score wins; anything under 3 is treated as no match. The
reader's text is written with `textContent`, never `innerHTML`.

**When you add an answer,** give it keywords in the words a stranger would use, not yours —
"what did you get wrong" rather than "retrospective". Then test the phrasings you expect: a
generic entry can outscore a specific one if they share a keyword, which is why `agents` belongs
to the "why 21 agents" answer rather than the compliance overview.

**If it has no answer** it says so and offers the direct route, rather than guessing. That
fallback is the most important behaviour in the panel — don't replace it with a generic
"I'm not sure" that invites a second try.

### Global behaviour

- **⌘K / Ctrl-K** (or `/`) opens a quick-jump palette: every section, project, principle and
  walkthrough step, type-to-filter, arrow keys, Enter. It routes through the page's own deep-link
  handler, so jumping to `#ux-4` still opens the right tab *and* the right step.
- **Scroll progress** fills the hairline under the nav; the current section underlines itself.
- **Stat tiles count up** once when they scroll into view. Only the leading number animates — the
  `~`, `#`, `+` and the `<small>` suffix are left exactly as written.
- **Copy buttons** on the email and phone, with a toast. Falls back to a hidden textarea where the
  Clipboard API is refused.

### How it degrades

- `prefers-reduced-motion: reduce` — no reveals, no count-up, and the compliance replay resolves
  to its finished state in one step rather than streaming.
- **No JavaScript** — the `.js` class is set by a one-line inline script in `<head>`, and every
  reveal rule is gated on it, so the page renders fully visible and fully readable. The snapshots
  show their default state, which is the same static content the page had before.
- **A backgrounded tab** — `requestAnimationFrame` stops when the page is hidden, so the replay
  carries a wall-clock watchdog that settles every agent and tidies up regardless.
- Each module is its own IIFE that returns early if its nodes are missing, so deleting a snapshot
  from the HTML cannot break the rest of the page.

### Where to edit it

Everything new lives in two places, both inside `index.html`:

- the **`INTERACTIVE LAYER`** block at the end of the `<style>` element — controls, drawers,
  palette, toast, back-to-top;
- the **`INTERACTIVE LAYER`** `<script>` at the end of `<body>`, numbered 1–10 by concern.

Data that drives a snapshot sits next to it: the 21 agents and their evidence are rows in the
`#agentRows` table body, the agency field maps are `data-d` / `data-o` attributes on each
`.ag-card`, the forecast modes are the `MODES` object, the settlement cycles are the `C` array,
and the eight document types are a JSON island in `<script id="docData">`.

---

## ⚠️ Before you publish

### 1. The interface snapshots are recreations, not screenshots

I don't have access to the real product, so the three app UIs are built in HTML/CSS from the agent
names, verdict model, form types and report structure in your RTC deck. They're labelled as
recreations on the page, in a caption under each one, and again in the footer note. **Keep that
labelling** — it's the difference between showing your work and misrepresenting it.

The data in them is plausible but invented (`CR-4821`, `MV KOTA SEGAR`, `MSKU4471208`, the
Lianyungang → Hamburg shipment). If any of it is accidentally close to a real customer record,
change it. If you *can* get cleared real screenshots, swap them in — replace the
`<div class="shot">…</div>` block with `<img src="shot-1.png" alt="…">` and keep the caption.

### 2. Numbers that don't reconcile

**Your resume says 21 agents; the RTC deck says 19** (8 party screening / 5 cargo & controls /
2 classification / 3 filing-cost-duty / 1 sustainability). The site says 21 throughout, including
"2 of 21 checks" and the 21-bar status strip in the compliance mockup. Pick one and make the
resume, the deck and this site agree.

### 3. Things I wrote that you need to own

The **What I did** bullets are expanded from your resume and are safe. These are mine and need
your confirmation — an interviewer will ask follow-ups on every one:

- **"The call I'd defend hardest: 21 agents, not one model"** — the four reasons for decomposition.
- **The precision/recall argument** for tuning screening toward recall. The three-state verdict
  ladder is from your deck; the reasoning behind it is my reconstruction.
- **"Build the model before the integrations"** and **"What I got wrong"** (change management,
  side-by-side verification view) on the Maritime Single Window.
- **"Derivation coverage"** — my name for a real idea. If you tracked something else, use its name.
- **The measurement note on the 93%** ("weighted by the actual daily mix", "excludes review time").
- **The framings** in *Also shipped* — support-as-product-clarity, horizon-beats-precision.
- **All four principles** in *How I work*.

Replace anything that isn't how it actually went. Your real reasoning will be more specific than
mine, and specific is the whole point.

### 4. The résumé PDF and the site now agree

Resolved. The site says **IIT Bombay** (the bio, the timeline, the footer) and so does
`Vaibhav_Raj_PM.pdf`. If you edit one, edit the other — anyone who clicks "Download résumé"
sees both.

### 5. Quick checks

- [ ] **LinkedIn URL** — I guessed `linkedin.com/in/vaibhav-raj-iitb`. Appears twice.
- [ ] **Dropdown menus** — `Work` lists all five projects, `How I work` lists the four principles.
      Open on hover (desktop), on tap (touch) and on keyboard focus; close on Escape, on selecting an
      item, or on clicking away. Selecting an item scrolls to that section and leaves it 15px clear of
      the sticky header. Edit the `<div class="menu">` blocks in the nav to change the entries.
- [ ] **Mobile nav** — below 760px the links collapse into a **Menu** button; the dropdowns render
      inline and expanded inside that panel, so every project is reachable on a phone.
- [ ] **Photo** — the avatar is a "VR" monogram. To use a real photo, drop `me.jpg` in the folder
      and replace the monogram in the `.avatar` div with the commented-out `<img>` tag right above it.
- [ ] **Contact details** use the résumé versions (`vaibhavraj738@gmail.com` / +91 85215 88880).
      Your old site had different ones — make résumé, LinkedIn and site agree.
- [ ] **The ask panel's answers** — every one is written in your voice and drawn from what was
      already on the page, but they are now *your* words to a stranger. Read all 25 and change
      anything you would not say out loud in an interview. The ones on what you got wrong, what
      you are looking for, and whether the screenshots are real matter most.
- [ ] **The snapshot data** — the interactive layer added more of it: 13 further agents with
      their evidence, seven more document types, eight settlement cycles. All invented but
      plausible. Check nothing accidentally resembles a real customer record, and that the
      agent names and document types match what you would actually claim in an interview.
- [ ] **21 agents** — the snapshot now lists all 21 by name, split 8 party screening / 3
      classification / 5 cargo & controls / 4 filing-cost-duty / 1 sustainability. Your deck said
      19. Make the deck, the résumé and this page agree.
- [ ] **Kale description** — I described the Platform team as owning "shared AI and data services
      the rest of the products build on." Correct it if that's not the remit.

---

## Deploy to GitHub Pages

### Option A — clean URL (recommended)

A repo named exactly `vaibhav696.github.io` serves at `https://vaibhav696.github.io/` with no subpath.

```bash
cd ~/Desktop/vaibhav-portfolio && gh repo create vaibhav696.github.io --public --source=. --push
```

Then **Settings → Pages → Source: Deploy from a branch → main / (root)**. Live in about a minute.

### Option B — replace your existing site

```bash
git clone https://github.com/vaibhav696/vaibhav_portfilo.git
cd vaibhav_portfilo
cp ~/Desktop/vaibhav-portfolio/index.html ~/Desktop/vaibhav-portfolio/Vaibhav_Raj_PM.pdf .
git add -A && git commit -m "Rebuild portfolio" && git push
```

The repo name is misspelled (`portfilo`). Renaming it to `portfolio` under
**Settings → General → Repository name** fixes the public URL; GitHub redirects the old one.

### Custom domain

Add a `CNAME` file containing just your domain, point a DNS `CNAME` record at
`vaibhav696.github.io`, then set the domain under Settings → Pages.

---

## Editing

Sections are marked with HTML comments (`<!-- ===== PROJECT 02 ===== -->`).

- **Colours** — the `:root` block at the top. `--accent` is the orange; `--clear` / `--review` /
  `--match` are the verdict colours used in the mockups.
- **Fonts** — Figtree (everything) + IBM Plex Mono (labels, data, URLs). Swap in the `<link>` tag
  and the `--f` / `--f-mono` variables.
- **Adding a project** — copy an `<article class="proj">`, bump the `.num`, and reuse
  `proj-head` → `proj-cols` (What I did / tiles) → `shot-wrap` → `callout`.
- **Building another mockup** — the `.shot` component is a browser frame wrapping `.app`
  (sidebar + main). Inside, `.tbl` gives you the agent-results table, `.ag-grid` the agency cards,
  `.split` the document-generation two-pane, and `.v v-clear|v-review|v-match|v-pend` the
  status pills.
- **The UX thinking tab** — `<div class="ux">` is a rail of `.ux-step` buttons driving one `.ux-stage`
  panel each. To add a step, add a button to `.ux-rail` and a matching `.ux-stage` with the same
  `aria-controls`/`id` pair; the script picks up the count on its own. `.ux-blk.is-reject` is the
  dashed "what I rejected" block, `.ux-art` the dark code-style artefact.
- **Deep links open the right tab** — a link to `#ux-4` or `#w3` switches tab and step before
  scrolling, so every nav entry works from a cold page load.
- **Mockups scroll horizontally on phones** — they have a `min-width: 880px` inside
  `.shot-scroll`. That's deliberate; a squashed product shot reads worse than a scrollable one.
