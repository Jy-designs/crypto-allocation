# Venga · Bundle Allocation prototype

Clickable prototype of the Bundle Allocation screen, built from the Figma frame `bundles::allocation` (node 169533:13403) in the "👨🏻‍💻 Crypto" file, page "27-10-2026 Bundles v1.1". The locked state and the error messages come from the "Token Choose List" frames on the same page.

Plain HTML, CSS and JavaScript in one file: no build step or dependencies.

## How it works
- **− / +** change one coin by 5 or 1 percentage points, depending on the tab at the top (see below). They snap to multiples of the step (5%: 12 → 15, 34 → 30). Press and hold to repeat. The other **unlocked** coins rebalance so the total stays at 100%, in proportion to what each holds above €12 (none goes under €12). Values are kept exact behind the scenes, so a step up then down lands back exactly where it started. The 5% tab shows whole percentages; the 1% tab shows up to two decimals, so a step visibly comes from all the other coins (+1% on one of 5 coins → 19.75% each for the other 4). Euro values are rounded to cents and always add up to the bundle amount.
- Under the heading, one line reads "Select your allocation below." in grey. When there's an error, the same line shows the red error message instead, so nothing moves.
- Each row shows the percentage and its euro value out of the bundle amount, e.g. "20% (€20)" at €100 or "20% (€23)" at €115. Euros show cents only when needed (€12.50).
- **Minimum €12 per coin.** Percentages are whole numbers, so the minimum is the smallest whole % worth at least €12 (12% at €100, 5% = €12.50 at €250, 20% at €60). A coin stops there (e.g. 15% → 12% at €100) and its − greys out (40% opacity), so it can't go lower and no error message is needed. (The "A minimum of €12 per coin." message is still in the code but can't be reached.) A coin can't go above what leaves the other coins their €12 (52% with 5 coins and nothing locked).
- **The total never goes above (or below) 100%.** + stops once the other unlocked coins are all at €12, and when every other coin is locked the last coin's − / + are disabled. The "Total allocation must be 100% in order to save." message and the disabled **Apply changes** (40% opacity, as in Figma) are still wired up as a safeguard, but normal use can't reach them.
- **Lock** freezes a coin: grey circle, open padlock icon, − / + at 40% opacity (Figma node 168063:15968).
- The **donut** and the heading stay pinned at the top; only the coin list scrolls.
- The donut redraws live with the Figma geometry (gaps, rounded corners, logo near the end of each segment). **Every coin segment is at least big enough for its logo**, however small its share; the rest of the ring is shared out proportionally.
- **Apply changes** (at 100%) shows a short "Changes applied" message. Nothing is saved.

## Prototype controls (not in Figma)
- **"5% steps" / "1% steps" tabs** above the phone (at the top of the screen on a phone) set how far − / + move a coin. Switching keeps the current split. Add `#1` to the link to open on the 1% tab, e.g. https://jy-designs.github.io/crypto-allocation/#1.
- **▼ / ▲ next to the heading** change the bundle amount by €5 (press and hold to repeat), from the minimum up to €10,000. The minimum is €20 per coin: €40 for 2 coins, €60 for 3, €80 for 4, €100 for 5. Lowering the amount can raise the minimum %, so coins under it are lifted to it (taken from the unlocked coins first). Switching to more coins raises the amount to that minimum if needed.
- The **"5 coins" dropdown** in the top right (not in Figma) switches the bundle between 2, 3, 4 and 5 coins (Bitcoin, Ethereum, Binance, Solana, XRP, in that order), starting from an even split.

## Settings (top of the script in `index.html`)
- `INVEST` (100): starting bundle amount in euros.
- `AMOUNT_STEP` (5), `AMOUNT_MAX` (10000), `MIN_AMOUNT_PER_COIN` (20): the ▼ / ▲ amount control.
- `MIN_EUR` (12): minimum per coin.
- `STEP` (5): starting − / + step in percentage points (the tabs switch it between 5 and 1).

## Files
- `index.html`: the whole prototype
- `assets/`: SVGs exported from Figma (coin icons, donut icons, lock / unlock, − / +, status bar). `donut-bnb.svg` is recoloured to white so the Binance logo matches the other donut logos (it's dark in Figma).
- `fonts/`: Labil Grotesk Regular, Medium and Bold (licensed, excluded from git by `.gitignore`)

## Open it
- Live: https://jy-designs.github.io/crypto-allocation/ (GitHub Pages, `main` / root). The Labil Grotesk fonts aren't in the repo, so the live page falls back to the system font.
- Locally: double-click `index.html`, or run the "Venga Allocation" preview (port 3620). With the `fonts/` folder present it uses Labil Grotesk.
- On a computer the tabs sit above the phone frame on a white page, and the frame shrinks to fit shorter windows so nothing needs page scrolling.
- On a phone (480px or narrower) it goes full screen and drops the fake status bar, with the tabs across the top.

## Not in this version
- The bundle name sheet and the preset chips from the earlier Basket Allocation prototype.
- The back button does nothing.

Figures are sample data, not real client data.
