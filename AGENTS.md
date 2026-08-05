# AGENTS.md

## What this is

Asteroids clone: HTML5 Canvas game in a single browser script. **No build system, no bundler, no package.json, no tests, no CI.**

## Run / verify

- Open `index.html` in a browser, or `npx serve .` → `http://localhost:3000`.
- There is **no test, lint, or build command**. "Verification" is manual: load the page and play. Canvas consoles (`console.log`) are the debug tool.

## Critical gotcha: plain global script

`index.html` loads `game.js` via a plain `<script src="game.js">` tag — **NOT** `type="module"`.

- Everything in `game.js` (classes `Ship`, `Asteroid`, `Bullet`, `Particle`, and the `W`/`H`/`loop()`/`initGame()` functions) are **global-scope, no imports/exports**.
- Do **not** add `import`/`export` or switch the script to a module — it will break loading.

## Structure

- `game.js` — all logic + rendering in one file. Order matters: entities (Bullet/Asteroid/Ship/Particle) are defined before the game-state functions (`initGame`, `nextLevel`, `killShip`, `update`, `draw`, `loop`) that use them.
- Game loop is time-based with `dt` (clamped to 0.05s), orchestrated from a `state` machine (`'playing'` | `'dead'` | `'gameover'`).
- Canvas dimensions are **defined twice and must stay in sync**: `const W = 800; const H = 600;` in `game.js` and the `<canvas width="800" height="600">` (plus body/CSS) in `index.html`.

## Conventions

- UI strings are **Spanish** (e.g. `NIVEL`, `GAME OVER`, `PUNTAJE`). Keep new on-screen text in Spanish.
- Section banners use `// ── Name ──…` comment separators. Follow that style when adding sections.
- Attribute `dead` on entities + filter-out after each tick is the standard cleanup pattern — keep it.
