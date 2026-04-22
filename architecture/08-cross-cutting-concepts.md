# 8. Cross-cutting Concepts

## 8.1 Event System

The EventBus is the central communication mechanism across bounded contexts.

**Pattern**: Publish/subscribe with typed events.

| Event              | Publisher          | Subscribers                        |
|--------------------|--------------------|------------------------------------|
| PelletEaten        | CollisionDetector  | PlayerModule (score), MazeModule (remove pellet) |
| PowerPelletEaten   | CollisionDetector  | PlayerModule (score), GhostStateMachine (frightened mode) |
| GhostCollision     | CollisionDetector  | PlayerModule (score or lose life), GhostStateMachine (eaten) |
| LifeLost           | PlayerModule       | GameStateManager (reset positions) |
| GameOver           | PlayerModule       | GameStateManager (transition to GAME_OVER) |
| LevelCleared       | MazeModule         | LevelManager (advance level), GameStateManager (reset) |
| LevelStarted       | LevelManager       | GhostStateMachine (reset timers), MazeModule (reload grid) |

Events carry minimal data (e.g., `GhostCollision` includes ghost mode and ghost type). Subscribers react independently — no ordering guarantees needed.

## 8.2 Error Handling

Since this is a single-process terminal game, error handling is straightforward:

| Category            | Strategy                                                      |
|---------------------|---------------------------------------------------------------|
| Invalid input       | Ignored — unrecognized keys produce no action                 |
| Wall collision      | Movement rejected — PacMan stays in current position          |
| Terminal resize     | Detect via curses; pause game if terminal is too small        |
| Unexpected crash    | Curses cleanup via `wrapper()` or try/finally to restore terminal state |

The curses `wrapper()` function ensures the terminal is always restored to a sane state, even on unhandled exceptions.

## 8.3 Game State Management

The game uses a finite state machine for top-level state:

```
MENU → PLAYING → PAUSED → PLAYING
                → GAME_OVER → MENU
                → LEVEL_TRANSITION → PLAYING
```

Each state defines which inputs are accepted and what gets updated/rendered. The GameLoop delegates to the current state handler.

## 8.4 Logging

| Aspect     | Approach                                                            |
|------------|---------------------------------------------------------------------|
| Module     | Python `logging` standard library                                   |
| Output     | File-based (`pacman.log`) — cannot log to terminal during curses    |
| Level      | `DEBUG` for development, `WARNING` for release                      |
| What to log| State transitions, event publications, ghost mode changes, errors   |

Logging to a file avoids interfering with curses terminal output.

## 8.5 Testability

| Concept                | How it's achieved                                              |
|------------------------|----------------------------------------------------------------|
| No UI in logic         | Game engine and domain modules have zero curses imports         |
| State-in, state-out    | Each `update()` call takes current state + input, returns new state |
| Event bus in tests     | Tests can subscribe to events to assert correct behavior       |
| Ghost AI               | Targeting strategies are pure functions: position in → target tile out |
| Maze                   | Tests load maze from simple string/list representations        |
