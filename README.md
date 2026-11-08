# FanumTaxxed — counter sales, VAT solved

A browser calculator for counter sales at New A to Z Mobile Center (Nepal). One customer, one bill, one total, with several items on it. Works offline on a laptop or an iPhone.

FanumTaxxed is the Fanum-tax name for it: the app that takes its bite of the total *exactly* — and never takes a bite out of what you typed.

Live page: https://fanumtaxxed.vercel.app/

---

## 0. Design restored (2026-10-30)

The neobrutalist × retro look of the original design is back — dotted paper page,
yellow FANUM TAXXED title card, ink borders with hard offset shadows, chunky
mode tiles, receipt bars with the barcode, the scrolling marquee, the black
retro footer, and the dark FANUM-991ES PLUS calculator chassis with its green
LCD and green / red / amber / teal VAT keys. VT323, Archivo Black, Geist and
Geist Mono are **self-hosted in `fonts/`** — the app still works fully offline.

This was a re-skin only: `solver.js`, `app.js` and `calc.js` are byte-identical
to the audited fixed build (121/121 battery checks pass).

---

## 1. What this build fixes (2026-10-29)

An independent audit of the previous build found the auto-adjust engine had been deleted
outright, along with three other defects. This build restores and repairs all of it:

| # | Defect in the audited build | Fix here |
|---|---|---|
| 1 | **Auto rate/qty adjustment deleted** — "Off by NPR 176.73" and nothing else happened | Engine restored, **ON by default** (audit: every generation of it was mathematically correct — 43 doz @ 874.11 for the reference scenario) |
| 2 | **The old "fixed box" ping-pong** — typing one box wiped the other, so typed counts/rates reverted | The box you type in is **never overwritten**; a typed count and a typed rate are both remembered. The last-edited one anchors, only the other box reconciles |
| 3 | **VAT-space trap via the calculator** — 37,586.70 → −VAT → "Use as bill total" divided by 1.13 again → 29,435.90 | The calculator remembers which VAT space the shown number is in (BASE / INCL badge on the LCD). "Use" converts back to the original figure first, so a number is never taxed twice |
| 4 | **Mode switch reinterpreted the number** — 42,473 incl-total silently became the excl-base | GROSS and BASE are two **linked boxes**; switching mode relabels the workflow but never reinterprets a typed number |

Also in this build: rates go to deeper decimals **only when that is what makes the line
exact to the paisa** (PRD: "fine decimal precision"), and every line shows count suggestions
with the rate each count needs, plus the exact counts for the total.

## 2. The bill

- **REV mode** (from total): type what the customer paid — VAT inclusive.
  Taxable base = paid ÷ (1 + V%). VAT = paid − base.
- **NON-REV mode** (from base, the default): type the taxable amount, or leave it blank and
  build the bill from the items — each line is priced as entered, VAT goes on top.
- The GROSS and BASE boxes are linked both ways: type either one, the other follows.
- All line math runs on the **before-tax subtotal X**, never on the gross.

## 3. Items and the auto-adjust engine

Each bill line is a count × rate in PCS, DOZEN or KG, auto-labelled ITEM n (no item-name
picker). Type the count or the rate; the other box reconciles.

**Auto-adjust (on by default):** one line takes the rest of the total. Which line is
tickable per line card ("takes the rest"); the master switch is in the Items header.

- The box you last typed is the **anchor** and is never overwritten:
  - typed count → kept exactly; the rate absorbs (remainder ÷ count, 2 dp, deeper only to be exact)
  - typed rate → pcs/doz: the count snaps to the nearest valid step (whole pcs / half dozens) and the rate reconciles; kg: the weight absorbs at 3–4 dp and the typed rate is kept
- Steppers: pcs ±1/+10, dozens ±½/+1.
- Suggestions under each absorber line: counts near the current one with the rate each
  needs (tap to apply), the exact counts for this total, and for kg the nearby weights.
- The status line says **Exact** or **Off by NPR x**.

## 4. Side calculator (FNT-991ES PLUS)

- **Instant keys** — pointerdown-driven, no click delay, no debounce; fast sequences like
  `465893` register every digit.
- **Live preview** — the result shows before `=`; `=` commits to the history tape.
- **Four separate replay buttons** ↑ ↓ ← → — cursor editing and history recall (no circular pad).
- **VAT keys**: +VAT, −VAT, VAT AMT (the tax inside the shown number) and VAT SPLIT (base + VAT + total). All use the bill's VAT %.
- **Use as bill total / taxable amount** — converts the number into the right VAT space
  first (see fix #3 above).
- **Sounds**: distinct tone per operation, ON by default.
- **Voice**: spoken keys and results, OFF by default, queued so it never slows a keystroke.
- **Voice input (MIC)**: speak numbers and operators — "three fifty times two fifty" types
  `350×250` in real time (browser speech recognition; Chrome, Edge and Safari).
- **History**: per-device (localStorage), per-record delete, tap or ↑/↓ to recall.

## 5. Files

| File | Role |
|---|---|
| `index.html` | Page layout: Items → SALES CALC → Result, calculator shell, credit footer. |
| `style.css` | Styling. Mobile first; Casio-style calculator body. |
| `app.js` | Bill UI: linked amounts, line cards, anchor editing, suggestions, events. |
| `calc.js` | Calculator: instant keys, cursor editing, sounds, voice, history, VAT keys. |
| `solver.js` | Pure rules (linked amounts, absorber engine, USE conversion, calculator). No DOM. Runs in Node for tests. |
| `voice.js` | Calculator voice input: speech → word/number parser → keys (the MIC button). |

Tests live outside this repository (Node battery against `solver.js`, 121 checks).
Deploying: see `DEPLOY.md` (GitHub Pages, Vercel, Cloudflare Workers — no env vars, no
secrets, no database).

## 6. Limits

- Not every total can be hit to the paisa when nobody absorbs or the counts are pinned;
  the app shows the variance rather than faking a match.

## 7. Run it

No build step and no dependencies. Open `index.html` in any browser, or use the live page above.

---

Developed & Designed by [Urbish](https://github.com/UrbishCodes) · [LinkedIn](https://www.linkedin.com/in/urbish)
