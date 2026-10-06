# 🎯 AGENTS.md: Open-Pacman Guide

This document summarizes critical repository constraints and architectural non-obvious details to prevent mistakes during development and debugging.

## ⚡️ How to Run / Execution
*   **Execution Context:** This project is a browser-based game. No complex command-line setup or server is necessary. Running the game requires a fully configured HTML/CSS/JS setup in a browser environment.
*   **Game Loop Origin:** The game is driven by `requestAnimationFrame` (found in `main.js`), not synchronous loops. State changes are asynchronous/frame-based.

## 🕹️ Interaction & Input
*   **Primary Control:** Player movement relies entirely on keyboard events (`keydown` listeners in `main.js`). The game does not have a CLI input mechanism.
*   **Game Flow Control:** Game state transitions (e.g., "Restart") are triggered specifically by clicking the interactive control button rendered in the HTML overlay.

## ⚙️ Architectural Quirks & Game Logic
*   **Movement Constraint (Crucial):** Pacman movement is grid-aligned. Full grid moves (like eating a dot) only occur when the player's position is reported as `aligned()` (i.e., rounded to cell coordinates, checks in `game.js`). The fractional component of position is used for smooth animation mid-movement.
*   **Pathfinding & Intent:** The system supports explicit path queuing for Pacman via `p.nextDir`. If this queued direction is possible (not blocked by a wall or door), it is prioritized immediately, overriding the current direction (`p.dir`).
*   **Tunneling:** The maze supports toroidal wrapping (tunnels). If Pacman or a Ghost moves off-screen in the dedicated `TUNNEL_ROW`, it wraps instantly to the opposite edge.
*   **Ghost AI (Hunter):** The 'hunter' ghost uses rudimentary chase logic. Its next move decision prioritizes the direction that minimizes the Manhattan distance to Pacman's current rounded grid position.
*   **Failure Handling:** Upon collision, the game does not immediately end; it decrements lives and immediately calls `resetPositions()`, returning to the last safe starting location.