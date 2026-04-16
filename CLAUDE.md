# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

A collection of self-contained browser games. Each game is a single `.html` file with all CSS and JavaScript inlined — no build tools, no dependencies, no server required. Open any file directly in a browser to play.

## Architecture pattern

Every game follows the same structure:
- **HTML** — minimal markup (a `<canvas>` or a grid of `<div>`s)
- **`<style>`** block — all styling inline in `<head>`
- **`<script>`** block — all game logic inline at end of `<body>`

Games use vanilla JS only. No frameworks, no imports, no bundler.

### Tic-Tac-Toe (`tic-tac-toe.html`)
- State: `board[9]`, `currentPlayer`, `gameOver`, `mode`, `scores`
- AI uses full minimax (unbeatable) — `minimax()` recursively scores every board state
- Two modes: `'pvp'` (two players) and `'ai'` (player vs minimax)
- Win detection via `WINS` — array of 8 winning index triples

### Brick Breaker (`brick-breaker.html`)
- Wrapped in an IIFE to avoid polluting global scope
- State machine: `'idle' | 'playing' | 'dead' | 'gameover' | 'levelclear'`
- Game loop: `requestAnimationFrame` → `update()` → `draw()`
- Speed scales with score (`currentSpeed()`) and increases each level (`levelSpeedBonus`)
- High score persisted via `localStorage` key `'bbHighScore'`
- Brick collision uses overlap-based side detection (smallest overlap axis determines reflection)
- Paddle angle control: ball reflects at an angle proportional to offset from paddle centre (max ±0.38π)

## Running games

Open the HTML file directly in a browser — no server needed:
```
# From project root
start brick-breaker.html
start tic-tac-toe.html
```

Or drag the file into a browser tab.

## Git workflow

Remote: `origin` → `https://github.com/saahilnaik/ClaudeCodeTest`

**After every code change, commit and push immediately.** This keeps the GitHub repo in sync and ensures every state is recoverable.

Stage only the files that were changed (never `git add .`):
```
git add <changed-file>
git commit -m "<type>: <short description>"
git push
```

### Commit message format

Use a short prefix that describes the nature of the change:

| Prefix | When to use |
|--------|-------------|
| `feat:` | new feature or game |
| `fix:` | bug fix |
| `style:` | visual/CSS-only change |
| `refactor:` | code restructure, no behaviour change |
| `docs:` | CLAUDE.md or comment updates |

Keep the subject line under 72 characters. If more context is needed, add a blank line followed by a body.

**Examples:**
```
feat: add sound effects to brick-breaker
fix: prevent AI from playing after game over in tic-tac-toe
style: update colour palette to match dark theme
docs: document brick-breaker state machine in CLAUDE.md
```
