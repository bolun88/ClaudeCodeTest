# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

A collection of self-contained browser games. Each game is a **single HTML file** with no external dependencies — all CSS and JavaScript are embedded inline.

## Running / Testing

Open any HTML file directly in a browser (double-click in Explorer, or run via CLI):

```bash
start shooter.html      # Windows default browser
start tictactoe.html
```

There is no build step, no package manager, no bundler, and no test runner.

## Architecture Pattern

Every game follows the same constraints:
- **One file** — HTML + `<style>` + `<script>` all in a single `.html` file
- **No external dependencies** — no CDN links, no imports, no frameworks
- **Canvas-based rendering** for games that need animation; DOM manipulation for simpler games
- **Dark retro color palette** (near-black background, neon accent colors)

### shooter.html structure

The file is organized into labeled sections (marked with `// ─── Section ───` comments):

| Section | Responsibility |
|---|---|
| Canvas & scaling | Responsive `transform: scale()` to fit any viewport |
| Input | `keys{}` map for keyboard; `mouseX/Y/Down` for mouse |
| State machine | `STATE` enum: MENU → PLAYING → LEVEL_COMPLETE → GAME_OVER |
| Score | `scoreSystem` — tracks score + high score via `localStorage` |
| Screen shake | `screenShake.trigger(mag, dur)` / `screenShake.apply(ctx)` |
| Particles | Array of short-lived square particles; used for explosions and sparks |
| Bullets | Shared array for player and enemy projectiles |
| Player | `class Player` — movement, aiming, firing, animation, damage |
| Enemy | `class Enemy` — 3 types (grunt/runner/tank), walk animation, health bar |
| Wave system | `wave` object — builds a shuffled spawn queue per level |
| Game loop | Fixed-timestep accumulator at 60fps; `tick(dt)` + `render()` |
| Collision | Circle-overlap checks; `filterAlive()` mutates arrays in-place |

**Level scaling:** grunts always spawn; runners from level 2; tanks from level 3. Spawn interval = `max(0.4, 1.5 - level * 0.08)`.

## Git Workflow

This repo uses Git with GitHub (`origin` = `https://github.com/bolun88/ClaudeCodeTest`).

- Commit only changed game files by name — never `git add .` blindly
- Push every commit: `git push`
- Write descriptive commit messages explaining *what changed and why*
