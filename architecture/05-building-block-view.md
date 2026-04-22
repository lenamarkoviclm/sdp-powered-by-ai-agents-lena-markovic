# 5. Building Block View

## 5.1 Level 1 — Container View

The system is composed of six containers within a single Python process.

![C4 Container Diagram](diagrams/c4-container.puml)

| Container       | Responsibility                                              |
|-----------------|-------------------------------------------------------------|
| Input Handler   | Captures keyboard events via curses, translates to commands |
| Game Engine     | Game loop, state management, collision detection, event bus |
| Maze Module     | Grid structure, wall layout, pellet state                   |
| Player Module   | PacMan position, movement, lives, score                     |
| Ghost Module    | Ghost AI, state machine, per-ghost targeting strategies      |
| Renderer        | Reads game state, draws to terminal via curses              |

Data flow: `Input Handler → Game Engine → [Maze, Player, Ghost] → Game Engine → Renderer`

The Game Engine orchestrates each tick. Domain modules (Maze, Player, Ghost) own their data and logic. The Event Bus inside the engine enables cross-module communication without direct coupling.

## 5.2 Level 2 — Component View: Game Engine

![Component Diagram — Game Engine](diagrams/c4-component-engine.puml)

| Component          | Responsibility                                           |
|--------------------|----------------------------------------------------------|
| GameLoop           | Fixed-timestep cycle: input → update → render            |
| GameStateManager   | Tracks game state (menu, playing, paused, game-over)     |
| LevelManager       | Current level, difficulty scaling (ghost speed, timers)   |
| EventBus           | Publish/subscribe dispatcher for decoupled communication |
| CollisionDetector  | Checks PacMan-pellet and PacMan-ghost collisions per tick|

## 5.3 Level 2 — Component View: Ghost Module

![Component Diagram — Ghost Module](diagrams/c4-component-ghost.puml)

| Component         | Responsibility                                                  |
|-------------------|-----------------------------------------------------------------|
| Ghost             | Entity with position, type, current mode                        |
| GhostStateMachine | Mode transitions: chase → scatter → frightened → eaten          |
| BlinkyStrategy    | Targets PacMan's current tile                                   |
| PinkyStrategy     | Targets 4 tiles ahead of PacMan's facing direction              |
| InkyStrategy      | Targets based on Blinky's position and PacMan's facing          |
| ClydeStrategy     | Targets PacMan when far (>8 tiles), scatter corner when close   |

Ghost targeting follows the strategy pattern: the state machine selects the active strategy based on ghost type and current mode. In frightened mode, all ghosts use random movement. In eaten mode, ghosts navigate back to the ghost house.
