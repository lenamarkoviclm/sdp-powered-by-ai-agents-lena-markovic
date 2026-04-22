# Architecture Decisions

## ADR-001: Use Python with Terminal Rendering

**Status:** Accepted

**Context:**
PacMan is a real-time game requiring grid rendering, keyboard input, and game loop management. We need a language and rendering approach that supports these requirements within the course tech stack (Python 3.12).

**Decision:**
Use Python 3.12 with terminal-based rendering (curses or similar). No external game frameworks.

**Rationale:**
- Matches course tech stack requirement
- Terminal rendering keeps focus on game logic, not graphics
- No external dependencies beyond standard library
- Testable — game logic is decoupled from rendering

**Consequences:**
- No pixel-level graphics — grid-based ASCII rendering
- Game loop must handle terminal refresh rate
- Input handling limited to keyboard via terminal

## ADR-002: Separate Game Logic from Rendering

**Status:** Accepted

**Context:**
Game logic (movement, collision, scoring) and rendering (display) are different concerns. Coupling them makes testing difficult.

**Decision:**
Use a clean separation: game engine produces state, renderer consumes state. The engine knows nothing about how the game is displayed.

**Rationale:**
- Game logic is independently testable (no terminal needed)
- Renderer can be swapped (terminal, GUI, web) without changing logic
- Follows single responsibility principle

**Consequences:**
- Need a well-defined game state interface between engine and renderer
- Slightly more code upfront for the abstraction

## ADR-003: Use Event-Driven Ghost AI

**Status:** Accepted

**Context:**
Each ghost in PacMan has different behavior (chase, scatter, frightened). Ghost state changes are triggered by game events (power pellet eaten, timer expired).

**Decision:**
Model ghost behavior as a state machine driven by game events. Each ghost type has its own chase strategy.

**Rationale:**
- State machine maps naturally to ghost modes (chase/scatter/frightened)
- Different chase strategies per ghost type (Blinky targets PacMan directly, Pinky targets ahead of PacMan, etc.)
- Event-driven transitions are testable

**Consequences:**
- Need to implement strategy pattern for ghost targeting
- Timer management for scatter/chase mode switching

## ADR-004: Publish/Subscribe EventBus for Cross-Context Communication

**Status:** Accepted

**Context:**
The four bounded contexts (Maze, Player, Ghost, Game) need to react to each other's state changes (e.g., pellet eaten → update score → check level cleared). Direct method calls would create tight coupling between contexts.

**Decision:**
Implement a central EventBus using the publish/subscribe pattern. Components publish typed events; interested components subscribe to specific event types.

**Rationale:**
- Bounded contexts stay decoupled — publishers don't know about subscribers
- Easy to add new reactions to existing events without modifying publishers
- Events are testable — tests can subscribe and assert

**Consequences:**
- Harder to trace control flow (no direct call stack)
- No ordering guarantees between subscribers
- Need a clear event catalog to avoid event sprawl

## ADR-005: Fixed-Timestep Game Loop

**Status:** Accepted

**Context:**
The game loop must handle input, update game state, and render at a consistent rate. Variable timesteps cause inconsistent game speed across machines.

**Decision:**
Use a fixed-timestep game loop where each tick advances game state by a constant time delta, independent of rendering speed.

**Rationale:**
- Consistent game speed regardless of machine performance
- Deterministic updates — easier to test and debug
- Simpler collision detection (no need to interpolate between frames)

**Consequences:**
- If a tick takes too long, the game slows down rather than skipping frames
- Frame rate is tied to tick rate in this simple implementation

## ADR-006: File-Based Logging During Gameplay

**Status:** Accepted

**Context:**
During gameplay, curses owns the terminal — any print/log output to stdout/stderr corrupts the display.

**Decision:**
Use Python's `logging` module with a file handler (`pacman.log`). No console logging during curses sessions.

**Rationale:**
- Avoids corrupting curses terminal output
- Log file persists after game exits for debugging
- Standard library — no extra dependencies

**Consequences:**
- Developers must check the log file rather than terminal output for debugging
- Log file needs to be excluded from version control
