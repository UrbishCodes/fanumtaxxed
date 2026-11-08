# FanumTaxxed — sales, VAT and a shop calculator

A lightweight, offline-first counter-sales web app. Type an amount (or build a bill from
item lines), get the VAT and the total exactly right, and hand over a clean receipt.
No account, no cloud, no build step — it runs straight from `index.html` in any browser.

Live page: https://fanumtaxxed.vercel.app/

## What it does

- **Sales & VAT** — NON-REVERSE (net → gross, the default) and REVERSE (gross → net).
  GROSS and BASE are two linked boxes: type either one and the other fills in. VAT rate is
  editable, with quick chips for 5 / 10 / 13 / 18 / 20%.
- **Bill from item lines** — add lines, each a count × rate in PCS, DOZEN or KG.
- **Auto-adjust** — the last line takes the rest of the total, so the bill closes to the
  paisa. The box you typed last is the anchor and is never overwritten; the other box
  reconciles.
- **Receipt** — billing lines ready to paste into an invoice, plus a one-tap copy.
- **Shop calculator** — a FANUM-991ES PLUS style calculator with instant keys, live preview,
  per-device history, VAT keys (+VAT / −VAT / VAT AMT / VAT SPLIT), key sounds, spoken
  readback, and **voice input**: say "three fifty times two fifty" and it types `350×250`.

## Problems it solves

- **The VAT-space trap** — a number that already includes VAT must not be taxed again. The
  calculator remembers which space a number is in, and "Use as taxable amount" converts it
  first, so a figure is never taxed twice.
- **The ping-pong bug** — typing one box used to wipe the other. Now the box you type in is
  never overwritten; only the other box reconciles.
- **Totals that won't close** — the auto-adjust engine lets one line absorb the remainder,
  so the lines add up to the taxable base exactly instead of showing "off by NPR x".

## How to use

1. Leave the VAT mode on **NON-REV** and set the rate (default 13%).
2. Either type the **BASE** (taxable amount), or add **item lines** (count × rate) and let
   the last line take the rest.
3. Read the **RESULT** — totals, VAT, and billing lines. Copy them into the invoice.
4. Open **CALCULATOR →** for quick arithmetic, VAT keys, and voice input.

## Files

| File | Role |
|---|---|
| `index.html` | Page layout: Items → Sales Calc → Result, calculator shell, credit footer. |
| `style.css` | Styling. Mobile-first; Casio-style calculator body. |
| `app.js` | Bill UI: linked amounts, line cards, anchor editing, suggestions, events. |
| `calc.js` | Calculator: instant keys, cursor editing, sounds, voice, history, VAT keys. |
| `solver.js` | Pure rules (linked amounts, absorber engine, USE conversion, calculator). No DOM. |
| `voice.js` | Calculator voice input: speech → word/number parser → keys (the MIC button). |

Voice parser tests: `node test/voice-parser.test.js`.

## Limits

- Not every total can be hit to the paisa when nobody absorbs or the counts are pinned; the
  app shows the variance rather than faking a match.
- Voice input uses the browser's speech recognition, so it works in Chrome, Edge and Safari
  (not Firefox).

## Run it

No build step and no dependencies. Open `index.html` in any browser, or use the live page above.

---

Designed & developed by [Urbish](https://github.com/UrbishCodes) · [LinkedIn](https://www.linkedin.com/in/urbish)
