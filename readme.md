# Credit Card Strategy Optimizer (1–5 Cards)

This project is a **browser-based credit card optimizer** that finds the **best 1-, 2-, 3-, 4-, or 5-card combinations** based on your real monthly spending, point values, and net annual fees.

It answers questions like:

* *“What’s the best 2-card setup for my spending?”*
* *“Is this premium card actually worth the annual fee?”*
* *"Which card should I use for each category if I only carry 5?"*

Everything runs **locally in your browser** — no build tools, no dependencies, no data leaving your computer.

---

## What the optimizer does

For every possible combination of 1–5 cards:

1. Looks at **each spending category**
2. Routes that category to the **best card in the combo**
3. Converts points → dollar value
4. Subtracts **net monthly fees**
5. Ranks strategies by **net monthly value**

This avoids common spreadsheet problems like:

* double-counting the same physical card
* fragile formulas
* difficulty modeling real-world rules

---

## How it’s structured

### Categories

Each category has a **monthly spend**:

```json
{ "name": "Dining", "monthlySpend": 500 }
```

### Cards

Each card defines:

* `name`
* `pointValue` (e.g. 0.01 = 1¢ per point)
* `monthlyNetFee` = (annual fee − credits you value) / 12
* `multipliers` per category
* optional `exclusiveGroup` for mutually exclusive variants

Example:

```json
{
  "name": "Amex Gold",
  "pointValue": 0.01,
  "monthlyNetFee": 14.58,
  "multipliers": {
    "Dining": 4,
    "Whole Foods": 4,
    "Other Grocery": 4
  }
}
```

---

## Mutual exclusivity (important)

Some cards have **variants or configurations** that can’t be held together.

Example:

* “Bilt Obsidian (Dining focus)”
* “Bilt Obsidian (Grocery focus)”

These are modeled with:

```json
"exclusiveGroup": "Bilt Obsidian"
```

The optimizer will **never** choose more than one card from the same group in a strategy.

---

## How to run it

1. Create a file called:

   ```
   cards.html
   ```
2. Paste the entire HTML file into it
3. Open it in any modern browser
4. Edit the JSON at the top of the page
5. Click **Run**

That’s it.

---

## Adding a new card (easy)

Click **“Add new card”** in the UI:

* Enter the card name
* A template card is appended to the JSON
* Fill in:

  * multipliers
  * net monthly fee
  * optional `exclusiveGroup`
* Click **Run**

No code changes required.

---

## Assumptions (by design)

This optimizer intentionally keeps things **clean and deterministic**:

✅ Supported:

* flat multipliers
* per-category optimization
* net annual fees
* rent as a category
* mutually exclusive cards

🚫 Not modeled (yet):

* spend caps (e.g. “first $6k at 5x”)
* rotating quarterly categories
* transfer partner bonuses
* redemption heuristics
* sign-up bonuses

Those can be added later if needed, but the current model is ideal for **steady-state, long-term card strategy**.

---

## Why this exists (vs Google Sheets)

Sheets are great for raw math, but struggle with:

* combinatorics
* exclusivity rules
* readability
* future expansion

This tool is:

* deterministic
* inspectable
* fast (≈600 combos total with 10 cards)
* easy to extend

Think of it as a **personal card strategy engine**, not a one-off calculator.

---

## Future ideas (optional)

* Spend caps / tiered earn
* Toggle rent on/off
* Different point values per redemption strategy
* Export results to CSV
* Visual per-category routing chart

---

If you ever want to evolve this into a React app, CLI tool, or hosted version, this structure already supports it cleanly.
