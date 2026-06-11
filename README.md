# Tap the Right Color

A browser-based cognitive game inspired by the Stroop Effect, built as an individual assignment using pure HTML, CSS, and vanilla JavaScript, no frameworks, no dependencies. The player must identify the ink color of a displayed word, not what the word actually says, which creates a deliberate mental conflict that gets harder as the game progresses.

---

## How to Play

- A color word appears on screen rendered in a color that may differ from what the word says
- Click the button that matches the ink color of the text, not the word itself
- Every correct answer adds one point
- Every 5 points the game levels up, introducing more colors and a shorter time limit
- A wrong answer or running out of time ends the game immediately

---

## Game Mechanics

**Difficulty Scaling**
The game has three color pools that unlock as the player levels up.

| Level Range | Color Pool Size | Time Limit |
|-------------|----------------|------------|
| Level 1 - 2 | 4 colors | 3000ms |
| Level 3 - 4 | 8 colors | decreasing |
| Level 5+ | 12 colors | minimum 700ms |

Time limit decreases by 250ms per level, with a hard floor of 700ms.

**Anti-Cheat Logic**
The game guarantees the word text never matches the ink color name. A loop runs up to 12 attempts to find a mismatched pair before falling back, ensuring the Stroop conflict is always present.

**Click Guard**
An isReady flag prevents double-clicks or premature inputs. Buttons are disabled until the round is fully set up, and disabled again immediately after the first valid click in each round.

**Highscore Persistence**
The highest score is saved to localStorage under the key tapcolor_highscore and persists across sessions.

---

## Controls

| Input | Action |
|-------|--------|
| Mouse click | Select a color button |
| Keys 1 to 6 | Select button by position |
| P | Pause / Resume |
| Escape | End game and go to result screen |

---

## Tech Stack

- Language: HTML, CSS, JavaScript (vanilla)
- Timer: requestAnimationFrame for smooth visual progress bar
- Storage: localStorage for highscore persistence
- No external libraries or frameworks

---

## Project Structure

```
tap-the-color/
└── index.html    # Complete game in a single file (HTML + CSS + JS)
```

---

## Key Implementation Details

**Timer**
The timer uses requestAnimationFrame for a smooth animated progress bar. A fallback setTimeout runs in parallel as a safety net in case the animation frame is delayed. The pause system skips elapsed time accumulation when isPaused is true without cancelling the animation loop entirely.

**Color Button Generation**
Each round, the full color pool for the current level is shuffled using a Fisher-Yates algorithm and the first 6 colors are taken as button choices. This ensures variety while keeping the number of buttons manageable.

**Screen Management**
Three screens (start, game, result) are managed by toggling a CSS active class and updating aria-hidden attributes for accessibility.

---

## What I Learned

Building this in a single HTML file without any framework forced me to think carefully about state management. Variables like isReady, isPaused, and currentDisplayedColor all need to stay in sync, and bugs from race conditions between the timer and click handlers were only visible during fast gameplay. Using requestAnimationFrame for the timer instead of setInterval also taught me how browser rendering works more concretely than any tutorial had.
