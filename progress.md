# SBI MaxGain Savings Calculator — Progress Tracker

_Last updated: 9 Nov 2026_

---

## 💡 Why this project exists

I have an SBI home loan (originally ₹68.48 L, **₹64.62 L outstanding**, **277 months left**, **7.55% p.a.**) and I'm planning to convert it into an **SBI MaxGain** account.

MaxGain turns a home loan into an overdraft account: any surplus money I keep in it is subtracted from the outstanding principal before interest is calculated. The EMI stays the same, but more of it goes to principal, so the loan closes years earlier — and unlike a prepayment, I can withdraw that money anytime.

The catch: MaxGain usually carries a **higher interest rate** (≈ +0.5% over the regular loan) plus a conversion fee. So the real question is:

> **"If I park X today and Y every month, how much interest do I actually save — and is it worth the higher rate?"**

No bank calculator answers that properly, so I built my own.

## 🎯 What the calculator answers

- Net interest saved with MaxGain (after conversion charges)
- When I become debt-free and how many years of tenure get cut
- Side-by-side: **current loan** vs **prepaying the regular loan** vs **MaxGain**
- Break-even surplus needed to justify MaxGain's higher rate
- MaxGain's tax-free "return" vs a post-tax FD
- Year-by-year table and a chart of outstanding principal vs parked surplus

---

## 🧭 Goals

| # | Goal | Status |
|---|------|--------|
| 1 | Understand how SBI MaxGain works and whether it suits my loan | ✅ Done |
| 2 | Build a static calculator page (HTML + Tailwind CSS) with every field configurable | ✅ Done |
| 3 | Persist user inputs in `localStorage` so values survive a refresh | ✅ Done |
| 4 | Push code to `github.com/prasad1101/maxgain-calculator` | ✅ Done (9 Nov 2026) |
| 5 | Publish on **GitHub Pages** | ⬜ To do |
| 6 | Confirm real MaxGain rate + conversion charge with SBI branch and plug them in | ⬜ To do |
| 7 | Decide: convert to MaxGain or prepay the regular loan | ⬜ To do |

---

## ✅ Progress log

### 9 Nov 2026
- Researched current SBI rates: regular home loan 7.50–8.80%, MaxGain 8.00–9.30% (revision effective 15 Oct 2026). Rate-switch fee per SBI T&C: ₹5,000 + GST.
- Built `index.html` — single static page, Tailwind CDN + Chart.js (cdnjs).
- Inputs: outstanding principal, current rate, remaining months, EMI (auto/manual), MaxGain rate, conversion fee, initial deposit, monthly deposit, yearly step-up %, yearly bonus + month, one-time withdrawal + month, FD rate, tax slab.
- Outputs: hero savings figure, loan-free date, tenure cut, 3-scenario comparison, break-even card, FD comparison, chart, year-by-year table.
- All inputs auto-saved to `localStorage` (key: `maxgain-calc`); "Reset to defaults" clears them.
- Sanity check with defaults (₹5 L initial + ₹25k/month):
  - Current loan: ~₹72.0 L interest, 23y 1m
  - Prepay regular @ 7.55%: ~₹23.4 L, 9y 4m
  - MaxGain @ 8.05%: ~₹25.9 L, 9y 7m

### 9 Nov 2026 — pre-deploy polish (local)
- **Input validation**: negatives, principal/rates ≤ 0, tenure < 1 or non-whole, bonus month outside 1–12, withdrawal month < 1, tax slab > 100 → listed in a red warning, bad fields outlined, results greyed out (previously tenure 0 gave an Infinity EMI).
- **Loan-never-closes guard**: if the MaxGain rate makes interest exceed the EMI (or the loan doesn't close in 50 years), results are blocked with an explanation instead of showing a fake "loan-free in 1 month" saving.
- **FD card** now says when an FD beats MaxGain after tax (it always claimed MaxGain wins).
- **Main result** now also says how MaxGain compares with prepaying (defaults: prepaying saves ₹2.63 L more — the cost of liquidity).
- **Mobile**: fixed horizontal scroll at 375 px (year table was stretching the grid); scenario badges no longer wrap; chart date labels stay horizontal.
- Pinned Tailwind CDN to `3.4.16`.
- Verified: defaults unchanged (EMI ₹49,340; current ₹72.05 L / 277 mo; prepay ₹23.37 L / 112 mo; MaxGain ₹25.95 L / 115 mo); zero deposits → −₹18.99 L; no JS errors; desktop + 375 px screenshots checked in headless Chrome.

### 9 Nov 2026 — one-screen redesign (UI only, calculation untouched)
- **No page scroll**: app is a full-height shell (`100dvh`). Desktop (≥1024 px): inputs on the left, results on the right — plain-language verdict, "3 choices" cards, then a tabbed panel (Chart / Year by year / Worth it?) that takes the remaining height.
- **Simpler inputs**: 7 key fields visible (loan, rate, months, MaxGain rate, switch fee, park now, park monthly); EMI, step-up, bonus, withdrawal, FD and tax under **More options** (auto-opens on tall desktop screens or when one of those fields has an error). Plain labels ("Park every month", "Switch fee", "Money locked" / "Withdrawable").
- **Verdict in words**: "Parking ₹5 L now + ₹25k/month cuts your total interest from ₹72 L to ₹26 L… Prepaying instead saves ₹2.63 L more."
- **Phones & tablets (<1024 px)**: bottom bar switches between ✏️ Inputs and 📊 Results and always shows the saving; "3 choices" becomes a tab. Respects the iPhone home-indicator safe area.
- Short screens (≤720 px tall) get tighter spacing and hide the helper text under input groups.
- Last view/tab remembered in `localStorage` (`maxgain-ui`).
- Verified 0 px page and panel overflow at 1920×1080, 1440×900, 1366×657, 1280×720, 1024×768, 820×1180, 768×1024, 414×896, 390×844, 375×667, 360×640, 320×568. On 320×568 the chart is small (~85 px).
- ⚠️ Gotcha: adding a custom `screens` entry to the Tailwind config disables `max-lg:` classes — use `[@media(max-height:720px)]:` style variants instead.

---

## 🚀 Publishing checklist (GitHub Pages)

- [x] Clone repo: `git clone https://github.com/prasad1101/maxgain-calculator.git`
- [x] Add `index.html` and `progress.md` at the repo root
- [x] `git add . && git commit -m "Add MaxGain calculator" && git push`
- [ ] Repo → **Settings → Pages → Deploy from a branch → `main` / root**
- [ ] Verify live at `https://prasad1101.github.io/maxgain-calculator/`
- [ ] Add a `README.md` with screenshot + live link

---

## 🛠️ Enhancement backlog

### High value
- [ ] **Rate-change scenarios** — model repo rate moving ±0.25/0.5% over time
- [ ] **Multiple withdrawals** — list of planned withdrawals (amount + month) instead of one
- [ ] **Daily-balance mode** — salary credited on day X, spends through the month (closer to SBI's daily calculation)
- [ ] **Tax view** — old vs new regime; show when interest drops below the ₹2 L Section 24(b) cap
- [ ] **EMI-reduction option** — keep tenure, reduce EMI instead of shortening tenure

### UX
- [ ] Dark mode toggle
- [ ] Shareable link — encode inputs in URL so a scenario can be sent to someone
- [ ] Save multiple named scenarios in `localStorage` and compare them
- [ ] Download yearly table as CSV / print-friendly view
- [ ] Hindi / Marathi labels

### Tech
- [ ] Replace Tailwind CDN with a built CSS file (smaller, no runtime compile)
- [ ] Move calculation logic to `calc.js` + add unit tests (Node/Jest)
- [ ] Lighthouse pass: accessibility labels, performance
- [ ] GitHub Action to deploy Pages on push

---

## 📏 Best practices & rules

### Project rules
1. **Static only** — no backend, no build step required to run. `index.html` must work by double-clicking it and on GitHub Pages.
2. **No user data leaves the browser** — inputs live only in `localStorage`. No analytics, trackers or external API calls with user values.
3. **Every input configurable** — no hard-coded loan figures in logic; defaults live in one `DEFAULTS` object.
4. **Accuracy first** — any change to the calculation must be checked against a known case (see *Verification* below) before pushing.
5. **Update this file with every change** — add a dated entry to the Progress log and tick/move items in Goals and Backlog.

### Code standards
- Keep logic (calculation) separate from UI (rendering). Pure functions: same inputs → same outputs, no DOM access inside `simulate()` / `calcEmi()`.
- Use descriptive names (`outstandingPrincipal`, not `p`) in new code; comment any finance formula with what it represents.
- Format money with Indian grouping (`toLocaleString('en-IN')`) and show lakh/crore short forms.
- Validate inputs: no negatives, months ≥ 1, rate > 0; show a clear warning instead of breaking.
- Wrap every `localStorage` read/write in `try/catch` — it can be disabled (private mode) and the page must still work.
- When adding a new input, add it to `DEFAULTS` too, so older saved values merge cleanly (`{ ...DEFAULTS, ...saved }`).
- Mobile first: check layout at 375 px width before pushing.
- Accessibility: every input has a visible `<label>`; colour is never the only signal (use text/icons too).
- Load third-party scripts only from trusted CDNs (cdnjs / Tailwind CDN), pin versions (e.g. `Chart.js/4.4.1`, `cdn.tailwindcss.com/3.4.16`).

### Git workflow
- `main` = what's live on GitHub Pages. Never push untested changes straight to `main`.
- Work on feature branches: `feature/<name>` (e.g. `feature/rate-change-scenarios`), `fix/<name>` for bugs.
- Small, focused commits with clear messages: `feat: add multiple withdrawals`, `fix: EMI auto-calc when rate is 0`, `docs: update progress.md`.
- Open a PR into `main`, self-review the diff, test the Pages preview / local file, then merge.
- Tag releases when a milestone ships: `v1.0.0` (first public version), `v1.1.0` (new feature), `v1.0.1` (fix).
- Never commit secrets, personal statements, or real account numbers.

### Verification before every push
- [ ] Defaults give: current loan ≈ ₹72.0 L interest / 277 months; MaxGain (₹5 L + ₹25k/mo @ 8.05%) ≈ ₹25.9 L / ~115 months
- [ ] EMI auto-calc for ₹64.62 L @ 7.55% / 277 months ≈ ₹49,340
- [ ] Zero deposits + MaxGain rate higher than current → savings shown as **negative** (sanity check)
- [ ] Refresh the page → values persist; "Reset to defaults" restores defaults
- [ ] No console errors; chart renders; table fills
- [ ] Works on mobile width and in Chrome + Safari

---

## ⚠️ Important instructions

### For using the calculator (the finance side)
- **Get real numbers from SBI first**: exact MaxGain rate for your profile, conversion fee (in writing), and the drawing-power reduction schedule.
- **Use your actual EMI** from the loan statement instead of the auto-calculated one.
- Parked money only saves interest **while it stays in the account** — treat it as untouchable unless it's an emergency.
- Keep a separate emergency fund mindset: MaxGain is liquid, but withdrawals immediately increase interest.
- Review the numbers every time SBI revises the rate (repo-linked) and log it here.
- Results are estimates (monthly model vs SBI's daily balance). **Not financial advice.**

### For working on this repo (with Claude or anyone else)
- Read this file first — it's the single source of truth for why, what's done and what's next.
- Pick the next item from **Enhancement backlog**, move it to *In progress*, and log it when done.
- Don't change calculation logic and UI in the same commit — easier to review and revert.
- If a change affects numbers, update the **Verification** expected values above in the same commit.
- Keep the page a single self-contained `index.html` until the "split into `calc.js`" backlog item is done.

---

## 📌 Notes & assumptions

- Interest modelled monthly on (principal − parked surplus); SBI uses the daily balance, so real numbers differ slightly.
- When parked surplus ≥ outstanding principal, the loan is treated as closable; any excess is returned.
- Rates are floating (EBLR / repo-linked) and change over time.
- Not financial advice — confirm everything with the SBI branch.
