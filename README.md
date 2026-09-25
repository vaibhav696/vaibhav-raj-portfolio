# Vaibhav Raj — Product Portfolio

Single-file, no-build static site. `index.html` is the whole thing (inline CSS + ~30 lines of JS
for the theme toggle). Fonts load from Google Fonts. Deploys to GitHub Pages as-is.

```
index.html            the site
Vaibhav_Raj_PM.pdf    linked from the hero and the contact block
README.md             this file
```

---

## ⚠️ Read this before you publish

The **facts** on this site come from your resume and the RTC strategy deck. The
**reasoning** — the "how I framed it", "what I chose not to build", "how that was measured"
passages — is a *drafted reconstruction* written to show the structure of good product
thinking. It is plausible and consistent with your work, but it is not a transcript of your
actual decisions.

**Go through this list and either confirm each passage or replace it with what really happened.**
An interviewer will ask follow-up questions about every one of these, and you need to be able to
answer from memory, not from the page.

### Trade Compliance Engine (section 02)

| Passage | What to confirm |
|---|---|
| "Filing isn't where the loss is" reframe | Was this actually your framing? If the real one was different, use the real one — it'll be more specific. |
| Four reasons for 21 agents over one model | Which of these did you actually argue? Add any real ones I missed. |
| Discipline → agent-count table | Deck says 19 agents across 5 disciplines (8/5/2/3/1); resume says 21. Update the counts so they add to 21, or change the headline to 19. **These currently don't reconcile.** |
| Precision/recall trade-off + verdict ladder | The three-state ladder is from your deck. The recall-over-precision reasoning is mine — confirm it. |
| "Review load per 100 shipments" + "escaped-miss rate" | These are metrics I proposed. If you defined different ones, swap them in. |
| Market figures | All from public sources cited inline. Kale's revenue model, pricing and competitor gap analysis are deliberately excluded. |
| "Where it honestly stands" | Verify this still matches reality. Update as the pilot progresses. |

### Maritime Single Window (section 03)

| Passage | What to confirm |
|---|---|
| Canonical-model reframe | Is this how MSW was actually architected? |
| Three "chose not to build" items (integrations-first, mobile app, dashboards) | **Most important to verify.** These are the strongest signal on the page and the most invented. Replace with real ones if these aren't accurate. |
| "Why Port Klang first" | Was Klang chosen as the hardest corridor, or for a commercial reason? Say whichever is true. |
| "Derivation coverage" metric | This is my invented name for a real idea. If you tracked something else, use its real name. |
| "What I got wrong" — change management | Replace with a real mistake if this one isn't yours. Keep the section either way; it's high-signal. |

### Section 04

- The doc-gen **measurement method** ("weighted by the actual daily mix", "excludes review time")
  is drafted. Replace with how you actually got to 93%.
- The chatbot and forecasting **framings** are mine. Confirm or rewrite.

### Section 06 — "What I'm still working out"

These three open questions are written in your voice but are my invention. **Replace them with
three things you actually don't have answers to.** A fabricated weakness is worse than no
weakness section, and this is the part interviewers will latch onto.

### Also check

- [ ] LinkedIn URL — I guessed `linkedin.com/in/vaibhav-raj-iitb`. Fix it if wrong (it appears twice).
- [ ] Contact details now use the **resume** versions (`vaibhavraj738@gmail.com` / +91 85215 88880).
      Your old site had different ones — make sure the resume PDF, LinkedIn and this site all agree.
- [ ] Agent count: 19 vs 21 (see table above).

---

## Deploy to GitHub Pages

### Option A — replace your existing site (keeps the current URL)

Your existing repo is `vaibhav696/vaibhav_portfilo`, serving at
`https://vaibhav696.github.io/vaibhav_portfilo/`.

```bash
git clone https://github.com/vaibhav696/vaibhav_portfilo.git
cd vaibhav_portfilo
# remove the old site files, then copy the new ones in
cp ~/Desktop/vaibhav-portfolio/index.html .
cp ~/Desktop/vaibhav-portfolio/Vaibhav_Raj_PM.pdf .
git add -A
git commit -m "Rebuild portfolio around product decisions rather than resume bullets"
git push
```

Note: the repo name is misspelled (`portfilo`). Renaming it to `portfolio` in
**Settings → General → Repository name** fixes the public URL. GitHub redirects the old one.

### Option B — a clean `username.github.io` site (best URL)

A repo named exactly `vaibhav696.github.io` serves at `https://vaibhav696.github.io/` —
no subpath. Better for a resume link.

```bash
cd ~/Desktop/vaibhav-portfolio
git init -b main
git add -A
git commit -m "Portfolio"
gh repo create vaibhav696.github.io --public --source=. --push
```

Then **Settings → Pages → Source: Deploy from a branch → main / (root)**. Live in ~1 minute.

### Custom domain (optional)

Add a file named `CNAME` containing just your domain (e.g. `vaibhavraj.com`), point a DNS
`CNAME` record at `vaibhav696.github.io`, and set the domain under Settings → Pages.

---

## Editing

Everything is in `index.html`. Sections are marked with HTML comments (`<!-- ===== CASE 1 ===== -->`).

- **Colours** — the `:root` block at the top. Change it once and both themes follow; the dark
  palette is redefined in the two blocks below it.
- **Fonts** — Newsreader (headings), IBM Plex Sans (body), IBM Plex Mono (labels). Swap in the
  `<link>` tag and the `--f-*` variables.
- **Adding a case study** — copy a `<section>`, bump the `sec-num`, and reuse the
  `block-label` + `block` pattern. The labels (`THE SITUATION`, `HOW I FRAMED IT`,
  `WHAT I CHOSE NOT TO BUILD`, `THE METRIC THAT ACTUALLY MATTERED`, `WHAT I GOT WRONG`)
  are the spine of the page — keep them consistent across cases.
