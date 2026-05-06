## How It Was Built

Single-file vanilla HTML/CSS/JS — no frameworks, no bundler, no dependencies beyond two Google Fonts.

### Structure

Everything lives in `index.html` in three sections:
- **CSS** — all styling via custom properties (CSS vars) at the top
- **HTML** — the card shell, tabs, and game UI in the middle  
- **JS** — all game logic in a single script block at the bottom

### Game Logic

The deck is a standard 6-deck shoe (312 cards) shuffled with a Fisher-Yates algorithm. Cards are objects `{ suit, value, hidden }`. Hand value uses standard blackjack rules — aces count as 11 then drop to 1 to avoid busting.

Dealer follows hard H17 rules (hits soft 17). Player actions — hit, stand, double, split detection — are all handled in plain JS with no state library.

### Card Counting

Uses the **Hi-Lo system**. Each card gets a tag when dealt:
- `+1` for 2–6
- `0` for 7–9  
- `−1` for 10, J, Q, K, A

Two counts are tracked:
- **Running Count** — cumulative across the whole shoe, never resets mid-shoe
- **This Hand** — Hi-Lo value of only the cards currently on screen, resets each deal

The Edge badge (Favorable / Neutral / Unfavorable) is driven by the True Count (running ÷ decks remaining) under the hood.

### Ask Joker Hint System

`getOptMove()` implements a full basic strategy lookup table covering hard totals, soft totals, and pairs. It also applies the **Illustrious 18** Hi-Lo deviations — for example standing on 16 vs 10 when the true count is ≥ 0 instead of hitting. `explainMove()` then generates a plain-English explanation of why that move is correct given the current hand and count.

### UI

Dark luxury aesthetic — deep black background, neon cyan/purple/pink palette. The game is mounted inside a joker playing card shape (rounded `border-radius`, glowing border, corner 🃏 marks). Fonts are Bebas Neue (display), DM Mono (data/labels), and Outfit (body). All animations are pure CSS keyframes.
