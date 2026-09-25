# Vaibhav Raj — Product Portfolio

Single-file static site. `index.html` is the whole thing — inline CSS, no JavaScript, no build step.
Fonts load from Google Fonts. Drop it on GitHub Pages and it works.

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
| Hero | Name, role, one-line positioning, "currently" strip |
| About | **Who I am · Where I work · Background** — bio, employer card, timeline |
| Work | 5 entries. Three have full interface snapshots |
| How I work | Four operating principles + toolkit |
| Contact | Dark block with email, phone, LinkedIn, résumé |

Each project follows the same shape: **What I did** → **What it's achieved so far** → **product
snapshot** → (for the two big ones) **the call I'd defend**.

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

### 4. Quick checks

- [ ] **LinkedIn URL** — I guessed `linkedin.com/in/vaibhav-raj-iitb`. Appears twice.
- [ ] **Photo** — the avatar is a "VR" monogram. To use a real photo, drop `me.jpg` in the folder
      and replace the monogram in the `.avatar` div with the commented-out `<img>` tag right above it.
- [ ] **Contact details** use the résumé versions (`vaibhavraj738@gmail.com` / +91 85215 88880).
      Your old site had different ones — make résumé, LinkedIn and site agree.
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
- **Mockups scroll horizontally on phones** — they have a `min-width: 880px` inside
  `.shot-scroll`. That's deliberate; a squashed product shot reads worse than a scrollable one.
