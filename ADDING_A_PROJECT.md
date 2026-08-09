# Handoff: Adding a New Project to the Portfolio Site

> **Purpose:** step-by-step instructions for an AI coding agent (Gemini Antigravity, Claude, Cursor, etc.)
> to add a newly built project to Sri Madhav's portfolio site, or to add a live link to an existing one.
>
> **Repo:** `sats-portfolio` · **Live:** https://srimadhavsats.github.io/sats-portfolio/
> **Last updated:** 2026-08-10

---

## 0. Read this first — how the site works

- The **entire site is one file: `index.html`**. There is no build step, no framework, no bundler,
  no `npm install`. You edit HTML directly and push.
- CSS lives in a single `<style>` block at the top of `index.html`. **Do not add a CSS file.**
- Deployment: GitHub Pages serves the `main` branch, root folder. **Pushing to `main` deploys it**
  (live in ~1 minute). There is no `gh-pages` branch and no Actions workflow — don't create one.
- Everything must stay **self-contained**: no new external scripts, no CDN libraries, no image files
  unless committed to the repo. The only external calls the page makes are Google Fonts, the
  CoinGecko price ticker, and GoatCounter analytics.

**Never add to this site:** Sri's phone number, his Telegram handle, or any personal/family details.
Contact is email + GitHub + X + LinkedIn only.
**Never commit the `handoff/` folder** — it is gitignored because it contains his phone number.

---

## 1. The task, in short

To add a project you edit **four things**:

| # | What | Where |
|---|---|---|
| 1 | Project card | `index.html` → `<section id="projects">` → `<div class="proj-grid">` |
| 2 | "Shipped projects" counter | `index.html` → hero stat row (search `Shipped projects`) |
| 3 | Journey blurb (optional) | `index.html` → search `Builder mode` |
| 4 | README table row | `README.md` → "Featured projects" table |

---

## 2. Add the project card

Find this block in `index.html`:

```html
<section id="projects" class="wrap">
  ...
  <div class="proj-grid">
```

Insert a new `<div class="proj-card reveal">…</div>` **inside `.proj-grid`**. Cards render in source
order — put the most impressive/newest project first.

### Template (copy, then fill the ALL-CAPS placeholders)

```html
      <div class="proj-card reveal">
        <div class="proj-top">
          <h3>PROJECT NAME</h3>
          <div class="proj-links">
            <a href="LIVE_URL" target="_blank" rel="noopener">live ↗</a>
            <a href="REPO_URL" target="_blank" rel="noopener">code ↗</a>
          </div>
        </div>
        <p>ONE OR TWO SENTENCES. What it does and the single most impressive technical detail. Use
        &amp; for ampersands. Wrap a standout phrase in &lt;strong&gt;…&lt;/strong&gt; if one earns it.</p>
        <div class="chip-row">
          <span class="chip hot">PRIMARY TECH</span><span class="chip">TECH 2</span><span class="chip">TECH 3</span><span class="chip">TECH 4</span><span class="chip">TECH 5</span>
        </div>
      </div>
```

### Rules for the card

- **`class="proj-card reveal"`** — the `reveal` class is required; it drives the scroll-in animation.
  Omitting it leaves the card invisible until a resize.
- **Links:** `live ↗` first, then `code ↗`. **Omit the `live` link entirely if the project isn't
  deployed** — do not link a dead URL. Always keep `target="_blank" rel="noopener"`.
  Use the trailing `↗` character exactly as shown.
- **Chips:** first chip gets `class="chip hot"` (orange, for the headline technology); the rest get
  `class="chip"`. Aim for **4–6 chips**. Keep them on one line with no whitespace between the
  `</span><span…>` tags (prevents stray gaps).
- **Description:** 1–2 sentences, max ~40 words. Concrete over adjectives. Escape `&` as `&amp;`.

### Worked example (the real Sats Hack Flow card)

```html
      <div class="proj-card reveal">
        <div class="proj-top">
          <h3>Sats Hack Flow</h3>
          <div class="proj-links">
            <a href="https://srimadhavsats.github.io/sats-hack-flow/" target="_blank" rel="noopener">live ↗</a>
            <a href="https://github.com/srimadhavsats/sats-hack-flow" target="_blank" rel="noopener">code ↗</a>
          </div>
        </div>
        <p>A smart-contract hack &amp; blockchain forensic visualizer: fund-flow bubble maps, call-trace inspection, and an OSINT toolkit across 20 major incidents (2016–2025, incl. the $1.46B Bybit heist). Plus a <strong>live address tracer</strong> pulling real Ethereum data via raw JSON-RPC + Blockscout.</p>
        <div class="chip-row">
          <span class="chip hot">React · Vite</span><span class="chip">JSON-RPC</span><span class="chip">vis-network</span><span class="chip">On-chain forensics</span><span class="chip">Blockscout API</span>
        </div>
      </div>
```

---

## 3. Bump the "Shipped projects" counter

In the hero stat row, find and increment the number:

```html
<div class="stat"><div class="num">5</div><div class="lbl">Shipped projects</div></div>
```

It must equal the number of `.proj-card` blocks in `.proj-grid`. (The neighbouring "Years in crypto"
stat is computed in JavaScript from 2012 — **leave that one alone**.)

---

## 4. Journey blurb (optional but nice)

Search for `Builder mode` in the timeline section and append the new project to the sentence listing
what he's shipped. Keep it a plain prose list, not a new bullet.

---

## 5. README table

Add a row to the "Featured projects" table in `README.md`. Link the **live URL if it exists**,
otherwise the repo URL:

```markdown
| [Project Name](LIVE_OR_REPO_URL) | One-line description | Tech, Tech, Tech |
```

---

## 6. Verify before pushing

Run these checks — all must pass:

1. **Card count matches the counter:**
   ```bash
   grep -c 'class="proj-card reveal"' index.html   # must equal the "Shipped projects" number
   ```
2. **No broken links** — every `href` in the new card returns HTTP 200:
   ```bash
   curl -s -o /dev/null -w "%{http_code}\n" -L "LIVE_URL"
   curl -s -o /dev/null -w "%{http_code}\n" -L "REPO_URL"
   ```
   (LinkedIn returns `999` — that's their anti-bot response, not a broken link.)
3. **Privacy:** these must return nothing —
   ```bash
   grep -n "8840333984\|t\.me/" index.html
   ```
4. **Visual check:** open `index.html` in a browser, scroll to "Things I've shipped". The new card
   should fade in on scroll, sit flush in the grid, and its links should open in new tabs.

---

## 7. Deploy

```bash
git add index.html README.md
git commit -m "Add <Project Name> to portfolio projects"
git push origin main
```

Pages rebuilds automatically. Confirm it went live (~1 min):

```bash
curl -s "https://srimadhavsats.github.io/sats-portfolio/?v=$RANDOM" | grep -c "PROJECT NAME"
```

**Commit message style:** short imperative subject, no trailing period, no AI attribution or
`Co-Authored-By` trailers of any kind — Sri wants his repos showing only his own authorship.

---

## 8. Also worth doing on the project's own repo

So the new project doesn't look unfinished when a recruiter clicks through:

- Set the repo **description**, **topics**, and **homepage** (the live URL) in the About sidebar.
- Make sure the project repo's own `README.md` has a live link and a short feature list.

---

## 9. Design reference (if you need to match the look)

CSS variables are defined in `:root` at the top of `index.html`:

| Token | Value | Use |
|---|---|---|
| `--bg` | `#0b0e14` | page background |
| `--card` | `#131824` | card background |
| `--card-border` | `#1e2636` | borders |
| `--text` / `--text-dim` / `--text-faint` | `#e6e9f0` / `#9aa3b5` / `#6b7488` | text hierarchy |
| `--orange` | `#f7931a` | Bitcoin orange — the only strong accent |
| `--mono` | JetBrains Mono | chips, stats, terminal |
| `--sans` | Inter | everything else |

Aesthetic: dark cypherpunk-professional. Bitcoin orange is the *only* accent colour — don't introduce
new hues. Reuse existing classes rather than writing new CSS.
