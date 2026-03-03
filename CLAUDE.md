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

## Established Design Language

**Color palette** (use these in every new game for visual consistency):

| Role | Hex | Usage |
|---|---|---|
| Background | `#0a0a0f` | Canvas fill |
| Player | `#00ff88` | Player character |
| Bullet | `#ffff00` | Player projectiles |
| Enemy A | `#ff2266` | Grunt (magenta-red) |
| Enemy B | `#ff6600` | Runner (orange) |
| Enemy C | `#aa00ff` | Tank (purple) |
| HUD text | `#00ffcc` | All UI readouts |
| Title / danger | `#ff2266` | Menu title, game over |

Sprites are drawn with canvas primitives (`fillRect`, `arc`) only — no images or SVG.

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

This repo uses Git with GitHub (`origin` = `https://github.com/bolun88/ClaudeCodeTest`). `gh auth setup-git` has been run so `git push` works without any credential dialogs.

**Commit and push after every meaningful unit of work** — a completed feature, a bug fix, a refactor. Never leave the session with uncommitted changes. The goal is that GitHub always reflects the current working state so nothing is ever lost.

- Stage files by name: `git add shooter.html` — never `git add .` blindly
- Push immediately after every commit: `git push`
- Commit messages: short imperative subject line, then a blank line and bullet details if needed (explain *what* changed and *why*, not just *that* it changed)

Example commit message format:
```
Add shield power-up to shooter

- Player gains 2s of invincibility on pickup
- Shield flashes cyan; pickup spawns every 3 waves
```
