# 4. Solution Strategy

## 4.1 Technology Decisions

| Decision                    | Rationale                                                        |
|-----------------------------|------------------------------------------------------------------|
| Python 3.12                 | Course requirement; rich standard library                        |
| `curses` for rendering      | Stdlib terminal UI — no external dependencies (ADR-001)          |
| No game framework           | Keeps focus on architecture and game logic                       |

## 4.2 Architecture Approach

| Strategy                         | How it's applied                                                              |
|----------------------------------|-------------------------------------------------------------------------------|
| Separation of concerns (ADR-002) | Game engine produces state → renderer consumes it. Input, logic, and display are independent layers |
| Domain-Driven Design             | Four bounded contexts: MAZE, PLAYER, GHOST, GAME — each with clear responsibilities |
| Event-driven communication (ADR-003) | Event bus decouples components. Events like `PelletEaten`, `PowerPelletEaten`, `GhostCollision` trigger cross-context reactions |
| State machine for ghosts         | Ghost modes (chase/scatter/frightened) modeled as state machine with event-driven transitions |
| Strategy pattern for ghost AI    | Each ghost type (Blinky, Pinky, Inky, Clyde) has its own targeting strategy   |

## 4.3 Game Loop Design

The game follows a fixed-timestep loop:

1. **Input** — Read keyboard input (non-blocking via curses)
2. **Update** — Advance game state: move PacMan, move ghosts, check collisions, process events
3. **Render** — Draw current game state to terminal

This ensures game logic runs at a consistent rate regardless of rendering speed.

## 4.4 Key Design Decisions Summary

| Concern              | Solution                                                  |
|----------------------|-----------------------------------------------------------|
| Ghost AI behavior    | Strategy pattern — one targeting algorithm per ghost type  |
| Ghost mode switching | State machine driven by timer events and game events      |
| Cross-context comms  | Event bus — publishers don't know about subscribers        |
| Testability          | Engine has no curses dependency; all logic unit-testable   |
| Level progression    | Game context manages difficulty parameters per level       |
