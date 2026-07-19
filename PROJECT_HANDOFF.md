# sats-portfolio — Project Handoff

> Portfolio website for **Sri Madhav** (GitHub: [srimadhavsats](https://github.com/srimadhavsats)).
> Goal: one place where recruiters, interviewers and potential collaborators can see — in under two minutes —
> that Sri has been in crypto since **2012**, has depth across the entire space, and now ships real Web3 products.
>
> Last updated: 2026-07-19

---

## 1. Current state

| Item | Status |
|---|---|
| Prototype site (`index.html`) | ✅ Built — single-file, zero build step, responsive, dark theme |
| Deployed to GitHub Pages | ⬜ Not yet — see §4 |
| CV (master, general-purpose) | ✅ `handoff/Sri_Madhav_CV_Master.html` + PDF (kept out of git — see §6) |
| Opus CV-generator handoff | ✅ `handoff/CV_MASTER_HANDOFF.md` (kept out of git) |
| README polish, OG image, custom domain, analytics | ⬜ Backlog — see §5 |

## 2. Design system

- **Aesthetic**: dark cypherpunk-professional. Bitcoin orange (`#f7931a`) as the only strong accent on a
  near-black blue (`#0b0e14`). Subtle grid background + blurred glow orbs. Terminal card in the hero
  (nod to Arch Linux / CLI identity) with a blinking cursor.
- **Fonts**: Inter (UI) + JetBrains Mono (accents, stats, chips) via Google Fonts, with system fallbacks.
- **Motion**: scroll-reveal via a plain scroll listener (progressive enhancement — with JS disabled the page
  is fully visible). Hover lift on cards/buttons.
- **Live data**: BTC price ticker in the nav from the CoinGecko public API; fails silently if blocked.
- **No build step by design**: one HTML file, works on GitHub Pages as-is, trivial to edit.

## 3. Content architecture (in page order)

1. **Hero** — "In crypto since 2012. Building in it now." + stat chips (years in crypto is computed
   dynamically from 2012, so it never goes stale) + terminal card.
2. **Ethos** — 3 cards: Bitcoin-first-but-fluent-everywhere, open source > closed source, self-custody.
3. **Journey** — 8-stop timeline 2012 → 2026. This is the differentiator; most candidates can't show this.
4. **Projects** — 4 cards with live/code links: Sats Swap (Solidity/Foundry AMM DEX), Eagle Eye
   (FastAPI+React macro analytics), SATS Trading Monitor (async order-flow pipeline), C2 Intel-Net
   (React 19 news command center).
5. **Skills** — 4 groups: Blockchain & Web3 · Infrastructure & Linux · Development · Markets/Analysis/QA.
6. **Experience** — Web3 Infrastructure & Community Ops (2012–present) + GlobalLogic/Google (2019–2024).
7. **Contact** — email + GitHub. **Deliberately no phone number** on the public site (spam/scraper hygiene);
   phone lives only on the CV, which is sent directly to people.

## 4. Deployment (already live — single-branch setup)

**Live at https://srimadhavsats.github.io/sats-portfolio/ — served from `main` branch, root folder.**
GitHub Pages source was switched from the initial `gh-pages` branch to `main`/root on 2026-07-19, and
`gh-pages` was deleted. `main` is now the only branch.

To update the site, just edit and push — Pages rebuilds automatically:

```bash
git add index.html
git commit -m "…"
git push origin main   # site refreshes in ~1 minute
```

Remaining repo polish: add the URL + a description and topics (`portfolio`, `web3`, `blockchain`,
`bitcoin`) in the repo's About sidebar so the repo itself looks finished.

## 5. Backlog (in priority order)

1. ~~Deploy~~ ✅ Done 2026-07-19 — live, single-branch (`main`/root). Test on a phone.
2. ~~README + repo About~~ ✅ Done — description, homepage and topics set via API 2026-07-20.
3. ~~OG image~~ ✅ Done 2026-07-20 — `og-image.png` (1200×630) + og:image/twitter:card meta tags.
   Regenerate: edit the card HTML (scratchpad `og.html` pattern), screenshot via headless Edge
   `--window-size=1200,630 --screenshot`.
4. ~~Add socials~~ ✅ Done — X @srimadhav_btc + @the2012crypto, LinkedIn, plus on-chain identity section
   (6 crypto domains + OpenSea link). **Do not add**: Telegram handle or phone (Sri's explicit request).
   NFT live-fetch from OpenSea was considered and deliberately skipped: the API requires a key and a
   client-side fetch would fail unpredictably — a static OpenSea profile link is robust.
5. ~~Resume link on site~~ ✅ Done 2026-07-20 — `cv/Sri_Madhav_CV.pdf` (public, **phone-free**; email +
   LinkedIn + GitHub only) linked from the hero "Download CV" button. Regenerate from
   `handoff/Sri_Madhav_CV_Master.html` by stripping the phone span, then headless-Edge print-to-pdf.
6. **Custom domain** — e.g. `srimadhav.dev` or a `.eth`-linked domain (fits the brand). Optional.
7. **Blog/writing section** — Sri wants to build a crypto-education brand eventually; the site's `sec-head`
   pattern extends naturally to a "Writing" section. Keep for v2.
8. **Analytics** — GoatCounter or Plausible (privacy-respecting, fits the ethos) if visit data is wanted.

## 6. Decisions & constraints (for whoever picks this up)

- **Truthfulness**: every claim on the site traces to Sri's real history (see `handoff/CV_MASTER_HANDOFF.md`
  fact sheet). Do not add achievements that aren't there.
- **Privacy**: `handoff/` is gitignored — it contains phone number and the personal fact sheet. Never commit
  it to this public repo. The site shows email + GitHub only.
- **No frameworks**: keep the site a single static file unless there's a strong reason. Recruiters don't
  care about the portfolio's own stack; they care about the four project repos.
- **Tone**: confident, specific, zero buzzword-slop. "14 years" claims are backed by the timeline detail
  (faucets, Electrum, ShapeShift, Luna airdrop) that only a genuine OG would know.
