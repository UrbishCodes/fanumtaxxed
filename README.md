# 🧾 FanumTaxxed

**Make the bill match the money. Every time. To the paisa.**

A fast counter-sales and VAT app for shops that sell by the piece, the dozen and the kilo. Type what the customer pays, and FanumTaxxed splits it into price and VAT, then builds item lines that add up *exactly*. Includes a retro shop calculator with VAT keys, history, sounds and voice input. 🎙️

🌐 **Live:** https://fanumtaxxed.vercel.app

| | |
|---|---|
| 🛠️ Built with | Plain HTML, CSS and JavaScript. No framework, no backend, no account. |
| 📴 Offline | Works without internet after the first load. Fonts are bundled. |
| 📱💻 Screens | Made for a phone at the counter, and works on a laptop too. |
| 🇳🇵 Default VAT | 13%. Change it any time, or tap 5, 10, 13, 18 or 20. |

---

## 📚 Contents

1. [The problem this solves](#1--the-problem-this-solves)
2. [How FanumTaxxed helps](#2--how-fanumtaxxed-helps)
3. [Quick start](#3--quick-start)
4. [Sales calculator (the two modes)](#4--sales-calculator-the-two-modes)
5. [Items and the auto-adjust engine](#5--items-and-the-auto-adjust-engine)
6. [The bill you get](#6--the-bill-you-get)
7. [FANUM-991ES PLUS shop calculator](#7--fanum-991es-plus-shop-calculator)
8. [Voice input](#8--voice-input)
9. [Your data and privacy](#9--your-data-and-privacy)
10. [Files in this project](#10--files-in-this-project)
11. [Run, test and deploy](#11--run-test-and-deploy)
12. [Rules the code follows](#12--rules-the-code-follows)
13. [Honest limits](#13--honest-limits)
14. [Author](#14--author)

---

## 1. 🚨 The problem this solves

### Under-billing in Nepal

> *In simple words: a shop sells something for 10,000 but writes a bill for less, so less VAT gets paid. This is called **under-billing**. It is a known problem in Nepal, and the author's view is that no practical fix exists for it today.*

Under-billing is not always cheating on purpose. A lot of it comes from how hard it is to bill *correctly* at a busy counter:

| What happens at the counter | Why it pushes shops toward a wrong bill |
|---|---|
| The customer says "I'll pay 7,500, all in." | The shop must work backwards: what is the price and what is the VAT? Doing it by hand is slow. |
| You cannot sell 245.82 mobile covers. | The count must be a whole number, so the rate has to change to make the total fit. Finding that rate by hand is tedious. |
| Two apps are needed: one for VAT, one for invoicing. | Switching between them, with a queue waiting, invites shortcuts. |
| Totals are rounded "close enough". | A bill that is a few rupees off does not match the money collected. |
| Several items on one bill. | Making all the lines add up to one agreed total is a puzzle. |

When correct billing is slow, the easy path is a rough or smaller bill. 😕

### The aim

**Make the correct bill the easiest bill.** One tool, a few taps, and the bill total equals what the customer actually paid, with the right VAT shown.

> ⚠️ **Fair note:** FanumTaxxed is a billing helper. It cannot force anyone to issue a bill, and it does not talk to any tax office. What it does is remove the *"it's too much work"* excuse, so an exact bill takes seconds.

---

## 2. ✅ How FanumTaxxed helps

| Counter problem | What the app does |
|---|---|
| "I only know the total the customer pays." | **Reverse VAT.** Type the total (gross). The app strips VAT and shows the price (base) and the VAT amount. |
| "I know my price, add VAT." | **Non-reverse VAT.** Type the price (base). The app adds VAT and shows the total. |
| "Pieces can't be fractions." | Pcs stay whole numbers. Dozens go in halves. The **rate** adjusts instead of the count. |
| "Lines must add up to the total." | **Auto-adjust.** One line takes the rest, so lines add up to the base to the paisa. |
| "I need to compare options fast." | **Suggestion chips** show nearby counts and the rate each one needs. |
| "I need to paste this into my invoice." | **Copy billing lines** gives ready-to-paste text. |
| "I need to do a quick sum or VAT check." | Built-in calculator with `+VAT`, `−VAT`, `VAT AMT`, `VAT SPLIT`. |
| "My hands are full." | **Voice input.** Say "three fifty times two fifty". |
| "Next customer!" | One-tap **Reset**. |

Result: the **status bar says "Exact"** when lines + VAT equal the total, or **"Off by NPR x"** when they do not. It never hides a gap. 🔍

---

## 3. 🚀 Quick start

1. Open the live page, or open `index.html` from a copy of this folder.
2. Type the bill amount in one box:
   - **GROSS** = what the customer pays, VAT included.
   - **BASE** = the price before VAT.
   The other box fills itself in.
3. In **ITEMS**, pick a unit (Pcs, Kg or Dozen) and type a count *or* a rate. The other fills in so the line fits the total.
4. Check the status says **Exact** ✅.
5. Tap **COPY BILLING LINES** and paste into your invoice.
6. Tap **RESET** for the next bill.

---

## 4. 🔀 Sales calculator (the two modes)

The **SALES CALC** card holds the amounts.

| Mode | In simple words | Which box you type in | Default |
|---|---|---|---|
| **NON-REVERSE** | Start from the price, add VAT on top. | BASE | ✅ Yes |
| **REVERSE** | Start from the total paid, take VAT out. | GROSS | No |

Switching mode never changes a number you typed. It only changes the hint text and which box the calculator's **Use as…** button fills.

### 🔗 Linked boxes

**GROSS** and **BASE** are linked: type in one and the other fills in. The box you typed in is the one the app trusts.

- Typing GROSS: `BASE = GROSS ÷ (1 + VAT%)`, and `VAT = GROSS − BASE`.
- Typing BASE: `VAT = BASE × VAT%`, and `GROSS = BASE + VAT`.
- Everything is rounded to 2 decimals (paisa).
- Leave both empty and the bill is **built from the item lines**: base = sum of lines, VAT added on top.

📝 *Example (13% VAT):* customer pays **7,500.00** → base **6,637.17**, VAT **862.83**.

The ledger below shows **VAT AMOUNT** and **TOTAL — INCL. VAT**, plus a one-line formula that explains how it got there.

---

## 5. ⚙️ Items and the auto-adjust engine

### 🧱 Lines

Each line is labelled ITEM 1, ITEM 2, and so on.

- **Unit:** Pcs, Kg or Dozen.
- **Count** (or **Weight** for kg) and **Rate** (NPR per piece, dozen or kg).
- Step buttons: Pcs `−1 / +1 / +10`, Dozen `−½ / +½ / +1`. Kg has none.
- **TAKES THE REST** tick box (more on this below).
- **REMOVE** deletes the line. Removing the last one leaves a fresh empty line.

Changing a line's unit clears its count and rate. *(Why: a price per kg and a price per piece are completely different numbers, so converting would be wrong.)*

### 📏 Unit rules

| Unit | Count rule | What adjusts to fit the total |
|---|---|---|
| 🧢 Pcs | Whole numbers only. | The rate. |
| 📦 Dozen | Whole dozens or halves, like 22 or 22.5. | The rate. |
| ⚖️ Kg | Decimals, up to 4 places. | The weight, or the rate, depending on what you typed. |

Messages you may see: "Enter the count.", "Pcs are whole numbers.", "Dozens go in halves.", "Weight goes to 4 decimals.", "Enter the rate."

### 🤖 Auto-adjust

> *In simple words: when the total is fixed, one line is the "flexible" line that soaks up the difference so everything adds up exactly. The other lines stay exactly as you typed.*

The master switch **Auto-adjust — the last line takes the rest** is **on** by default.

- Once there is a target (a typed BASE or GROSS), the **last line** is the flexible one. If there is only one line, that line does it.
- All other lines are priced exactly as typed: `count × rate`.
- Tick **TAKES THE REST** on a different line to choose yourself. That turns the master switch off.
- Untick it and nobody adjusts, so the gap (variance) simply shows.
- Switch auto-adjust back on to return to "last line takes the rest".
- With no target typed, nothing adjusts and the bill is built from the lines as entered.

### 🔒 The anchor rule (your typing is never overwritten)

> *In simple words: on the flexible line, the box you typed in last stays exactly as you typed it. The other box shows the number needed to make the total work.*

| You typed | Unit | What happens |
|---|---|---|
| **Count** | Pcs, Dozen or Kg | Your count stays. The rate is worked out: `rate = what's left ÷ count`. It uses 2 decimals, and more (up to 5) only when that is needed to be exact to the paisa. |
| **Rate** | Pcs or Dozen | The app picks the nearest allowed count (whole piece, or half dozen). Then the rate is recalculated from that count. The count rule wins. |
| **Rate** | Kg | Your rate stays. The weight is solved. It uses 3 decimals (what a scale shows), or 4 when that makes the amount exact. |

Both typed values are remembered, and only the last one leads. Nothing gets wiped when you touch the other box. When a line stops being the flexible one, its computed box becomes a normal typed value, so numbers never jump around.

### 🪙 Closing the last paisa

If the leftover does not divide evenly, a gap of a few paisa can remain. If another line has **count 1**, its rate moves by exactly that gap. The line says *"Rate moved 1 paisa to close the bill."* If no line can take it, the gap stays visible.

### 💡 Suggestion chips

On the flexible line, when there is a target:

- **Pcs and Dozen:** five counts around the current one, each with the rate it would need. Counts that hit the total exactly are marked **exact**. A note lists more exact counts (up to about double the suggestion).
- **Kg:** up to four weights near the exact one, each with its gap in paisa (3 and 4 decimals).
- Tap a chip to use that count or weight.

### 🧪 Worked examples

From the deployment notes, checked against the solver:

| Case | Result |
|---|---|
| Dozen line, rate typed **870**, GROSS **42,473** | Count **43**, rate **874.11**, base 37,586.73, VAT 4,886.27, **Exact** ✅ |
| Same bill, count typed **44** | Count stays 44, rate becomes **854.2439** (extra decimals keep it exact) |
| Kg line, base **6,637.17**, rate typed **150** | Weight **44.2478 kg**, **Exact** ✅ |

---

## 6. 🧾 The bill you get

The **RESULT — BILL RECEIPT** card shows:

- **Status:**
  - ✅ **Exact. Lines + VAT = NPR …** lines match the total.
  - ⚠️ **Off by NPR …** they do not.
  - 🧮 **Built from items…** no total was typed.
  - ❌ The error to fix.
- **Line rows:** count, unit, rate and amount, with notes like "(takes the rest)" and "(1 paisa)".
- **Totals:** taxable base, lines total, variance, VAT, lines + VAT, bill total.
- **Billing lines**, ready to paste:

```
ITEM 1: 43 doz × NPR 874.11 = NPR 37,586.73
Taxable total NPR 37,586.73
VAT 13% NPR 4,886.27
Bill total NPR 42,473.00
```

- 📋 **COPY BILLING LINES** copies that text (with a backup method for older browsers).
- 🔄 **RESET** clears everything and returns to NON-REVERSE with 13% VAT.

---

## 7. 🧮 FANUM-991ES PLUS shop calculator

Tap **CALCULATOR →** at the top. It slides up from the bottom on phones and floats at the right on wide screens (900 px and up). Close it with **Close** or `Esc`.

### ⌨️ Keys

| Group | Keys |
|---|---|
| Numbers and operators | `0–9`, `.`, `+`, `−`, `×`, `÷`, `(`, `)` |
| Editing | `C` clear, `⌫` delete at the cursor, `←` `→` move the cursor |
| History | `↑` earlier entry, `↓` later entry |
| Answer | `=` |
| VAT keys | `+VAT`, `−VAT`, `VAT AMT`, `VAT SPLIT` |
| Send to the bill | **Use as taxable amount** or **Use as bill total** |

- ⚡ Keys fire the instant you touch them (no click delay), so fast typing never drops a digit.
- 👀 A live preview of the answer shows before you press `=`.
- Brackets, decimals and negative numbers work. Divide by zero or a broken sum shows **Error**.
- Sums are read by a small safe parser. No `eval`.

### 🏷️ VAT keys

The VAT rate is the one in the SALES CALC card.

| Key | Simple meaning | Maths |
|---|---|---|
| `+VAT` | Add VAT to this number. | `n × (1 + V/100)`. Marks it **INCL**. |
| `−VAT` | Take VAT out of this number. | `n ÷ (1 + V/100)`. Marks it **BASE**. |
| `VAT AMT` | How much of this total is VAT? | `n × V ÷ (100 + V)` |
| `VAT SPLIT` | Show base and VAT for this total. | Shows the base, and a note: base + VAT = total. |

### 🏁 The BASE / INCL badge and "Use as…"

After `+VAT`, `−VAT` or `VAT SPLIT`, the screen shows a badge: **BASE** (a before-VAT number) or **INCL** (an after-VAT number). The calculator also remembers the number it came from.

When you press **Use as…**, it first converts the number into the form the bill needs, so **VAT is never charged twice**:

- In **REVERSE** mode the bill wants the total with VAT. A BASE number is turned back into its original total.
- In **NON-REVERSE** mode the bill wants the price before VAT. An INCL number is turned back into its original base.
- A notice says **"Converted first…"** when that happened.

### 🕘 History

- Every `=`, `VAT AMT` and `VAT SPLIT` is saved. Up to 100 entries.
- **HIST** opens the list. Tap a row to bring it back, 📋 to copy the result, ✕ to delete it.
- `↑` / `↓` step through history. A **HIST** light shows while an old entry is on screen.

### 🔊 Sound and spoken keys

- **SOUND** (on by default): a different tone for every kind of key, made with the Web Audio API.
- **VOICE** (off by default): reads digits, operators and answers aloud. Numbers are said as words.
- **MIC**: talk to the calculator (next section).

### 🖥️ Physical keyboard

With the panel open and no text box selected: digits, `.`, `+ - * /`, `( )`, `Enter` or `=`, `Backspace`, `Delete` (clear), arrows, `Esc` (close).

---

## 8. 🎙️ Voice input

Tap **MIC** and speak. Your words become key presses, exactly like tapping the buttons. A live line shows what the app heard.

- 🌐 Needs a browser with the Speech Recognition API (Chrome, Edge, Safari). Others show a notice.
- 🗣️ Set to English (India), runs continuously, shows interim results. If the engine changes a word, the entry is cleared and replayed.
- 🎤 Needs microphone permission. If blocked, a notice tells you how to allow it.
- ☁️ Speech recognition is done by the browser and may use the browser maker's online service. FanumTaxxed does not store or send your audio anywhere.

| You say | You get |
|---|---|
| `three fifty times two fifty` | `350×250` |
| `one hundred fifty` or `one fifty` | `150` |
| `two lakh fifty thousand` | `250000` |
| `one crore` | `10000000` |
| `three point five` | `3.5` |
| `seventeen eight zero two` | `17802` |
| `two into two` | `2×2` (Indian English "into") |
| `plus`, `minus`, `times`, `divided by`, `equals` | operators and `=` |
| `open bracket`, `close bracket` | `(` and `)` |
| `clear`, `reset` | `C` |
| `delete`, `backspace` | `⌫` |

Filler words like "um", "please" and "the" are ignored. The word-to-number parser has 39 automated tests.

---

## 9. 🔐 Your data and privacy

- No account. No server. No database. All maths runs in your browser.
- The bill is **not saved**. Reload the page and it is gone.
- Saved on your device only (`localStorage`):

| Key | What it keeps |
|---|---|
| `ftx_calc_hist` | Calculator history (up to 100 entries) |
| `ftx_snd` | Sound on or off |
| `ftx_voice` | Spoken keys on or off |

- Each device has its own history. Nothing syncs.
- 📊 The page loads one outside script, **Vercel Web Analytics** (`cdn.vercel-insights.com`). The app works without it. To remove it, delete the two `<script>` lines near the top of `index.html` that mention it.
- Voice input uses the browser's own speech service, as noted above.

---

## 10. 🗂️ Files in this project

| File | What it does |
|---|---|
| `index.html` | Page structure, the line template and the calculator panel. |
| `style.css` | The look: neobrutalist and retro. Dotted paper, thick ink borders, hard shadows, receipt-style ledgers, scrolling marquee, calculator body. Mobile first, with breakpoints at 430, 640 and 900 px and a reduced-motion setting. |
| `app.js` | Screen logic: state, linked amounts, lines, the anchor rule, results, copy and reset. |
| `solver.js` | The brain, with no screen code: VAT maths, auto-adjust engine, kg weight solver, suggestions, safe expression parser. Runs in the browser and in Node. |
| `calc.js` | The calculator: keys, cursor, history, sounds, spoken keys, keyboard. |
| `voice.js` | Microphone input and the words-to-keys parser. Also runs in Node for tests. |
| `fonts/` | Bundled fonts: VT323, Archivo Black, Geist, Geist Mono. |
| `test/voice-parser.test.js` | 39 tests for the voice parser. |
| `DEPLOY.md` | Deploy steps and a 60-second smoke test. |
| `package.json`, `package-lock.json` | Project info and the test command. |
| `vercel.json`, `wrangler.toml`, `.nojekyll` | Settings for Vercel, Cloudflare and GitHub Pages. |

Scripts load in this order: `solver.js`, `app.js`, `calc.js`, `voice.js`. `app.js` shares a small `window.FanumTax` (mode, VAT rate, "use target") that the calculator reads. `calc.js` shares `window.FanumCalc` (`key`, `open`, `isOpen`) that voice input uses.

---

## 11. 🏃 Run, test and deploy

### ▶️ Run

No build step.

```bash
git clone https://github.com/UrbishCodes/fanumtaxxed.git
cd fanumtaxxed
# open index.html in a browser, or serve the folder:
npx serve .
```

Microphone, clipboard and audio work best on `https` or `localhost`.

### 🧪 Test

```bash
npm test
```

Runs the 39 voice-parser tests (number words, lakh and crore, decimals, operators, fillers, glued symbols). The VAT and bill engine does not have automated tests in this repository yet.

### 🚢 Deploy

It is a static site, so any static host works. See `DEPLOY.md` for the full steps.

| Host | How |
|---|---|
| Vercel (current) | Import the repo, framework "Other", no build command, output `./`. |
| GitHub Pages | Serve the repo root (`.nojekyll` is included). |
| Cloudflare | `wrangler deploy` using the included `wrangler.toml`. |

✔️ **60-second check after deploying** (from `DEPLOY.md`): the header says **FanumTaxxed**; Auto-adjust is on; a dozen line at rate 870 with GROSS 42,473 gives count 43, rate 874.11, **Exact**; typing a different count keeps it as typed; in the calculator, `37586.70` → `−VAT` → **Use as bill total** (REVERSE mode) sets **37,586.70**.

---

## 12. 📐 Rules the code follows

- 💰 Money is counted in whole paisa inside the code, so tiny decimal errors cannot sneak in. It is shown with Indian digit grouping (1,23,456.00).
- Reverse VAT: `BASE = GROSS ÷ (1 + V/100)`, `VAT = GROSS − BASE`. Non-reverse: `VAT = BASE × V/100`, `GROSS = BASE + VAT`.
- Pcs are whole. Dozens are whole or half. Kg takes up to 4 decimals.
- Pcs and dozen counts are never rounded to fit a total. The rate changes instead.
- The box you are typing in is never overwritten. User input always wins.
- One VAT rate is used everywhere: the sales card and the calculator.
- Sending a calculator number to the bill never taxes it twice.

---

## 13. 🧭 Honest limits

- 🎯 **Not every total can be exact.** With whole pieces and 2-decimal rates, some totals cannot hit the paisa. The app adds more decimals to the rate when that makes the line exact, and otherwise shows the gap.
- 🗃️ **No bill history.** Bills are not saved and stock is not tracked. Only the calculator keeps history.
- 🏷️ **No item list.** Lines are named ITEM 1, ITEM 2. There are no item names, buy cost or max rate boxes on the page. The profit-band check (loss below 5%, over-tax above 8.5%) and the "above max rate" flag exist in `solver.js` but have no inputs to work from, so they do not appear. The item-search code is also not used by the page.
- 🏛️ **No link to any tax office.** It helps make correct bills. It does not file or send anything.
- 🎙️ **Voice input** depends on the browser and works best in a quiet place.
- 🧪 **Tests** cover only the voice parser.
- 📦 `package.json` lists `@vercel/analytics`, but the page loads the analytics script directly instead.

---

## 14. 👤 Author

Designed and developed by **Urbish** 💛

- GitHub: https://github.com/UrbishCodes
- LinkedIn: https://www.linkedin.com/in/urbish
