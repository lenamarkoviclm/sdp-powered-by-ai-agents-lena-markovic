# 1. Introduction and Goals

## 1.1 Requirements Overview

PacMan is a terminal-based implementation of the classic arcade game, built in Python 3.12 with no external game frameworks.

Core gameplay requirements:

- **Maze navigation**: Player controls PacMan through a grid-based maze containing walls, pellets, and power pellets
- **Ghost AI**: 4 ghosts (Blinky, Pinky, Inky, Clyde) each with distinct targeting strategies and shared state machine behavior (chase, scatter, frightened)
- **Scoring**: Points for eating pellets (10), power pellets (50), and ghosts (200/400/800/1600 escalating per power pellet)
- **Lives**: Player starts with 3 lives; losing all lives ends the game
- **Level progression**: Clearing all pellets advances to the next level with increased difficulty (faster ghosts, shorter frightened duration)
- **Power pellets**: Temporarily reverse ghost behavior — ghosts become frightened and PacMan can eat them

## 1.2 Quality Goals

| Priority | Quality Goal   | Description                                                                 |
|----------|----------------|-----------------------------------------------------------------------------|
| 1        | Testability    | All game logic testable without terminal/UI — engine produces state, renderer consumes it |
| 2        | Modularity     | Bounded contexts (Maze, Player, Ghost, Game) are independently modifiable   |
| 3        | Correctness    | Ghost AI faithfully reproduces classic PacMan targeting behaviors            |
| 4        | Responsiveness | Game loop maintains smooth input handling and rendering at playable frame rate |
| 5        | Simplicity     | No external dependencies beyond Python 3.12 standard library               |

## 1.3 Stakeholders

| Stakeholder       | Role                  | Expectations                                                    |
|--------------------|-----------------------|-----------------------------------------------------------------|
| Course instructors | Evaluators            | Clean architecture, testable code, documented design decisions  |
| Developer (you)    | Designer & implementer| Clear bounded contexts, maintainable codebase                   |
| Player             | End user              | Responsive controls, recognizable PacMan gameplay               |

## 1.4 Bounded Contexts

The system is decomposed into four bounded contexts following Domain-Driven Design:

- **MAZE** — Grid structure, walls, pellet placement and consumption
- **PLAYER** — PacMan movement, lives, score tracking
- **GHOST** — AI behavior per ghost type, state machine (chase/scatter/frightened), targeting strategies
- **GAME** — Game loop, state management (menu/playing/paused/game-over), level progression, event bus
