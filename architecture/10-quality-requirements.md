# 10. Quality Requirements

## 10.1 Quality Tree

```
Quality
├── Testability
│   ├── Game logic testable without UI
│   └── Ghost AI strategies are pure functions
├── Modularity
│   ├── Bounded contexts independently modifiable
│   └── Event-driven coupling only
├── Correctness
│   ├── Ghost AI matches classic PacMan behavior
│   └── Scoring rules are accurate
├── Responsiveness
│   ├── Input feels immediate
│   └── Consistent frame rate
└── Simplicity
    ├── No external dependencies
    └── Minimal abstraction layers
```

## 10.2 Quality Scenarios

| ID  | Quality       | Scenario                                                                                  | Expected Result                                              |
|-----|---------------|-------------------------------------------------------------------------------------------|--------------------------------------------------------------|
| QS-1| Testability   | Developer runs unit tests for ghost targeting logic                                       | All tests pass without launching a terminal or curses         |
| QS-2| Testability   | Developer runs unit tests for collision detection                                         | Tests use in-memory maze representation, no rendering needed  |
| QS-3| Modularity    | Developer adds a new ghost type with custom AI                                            | Only Ghost module changes; no modifications to engine or renderer |
| QS-4| Modularity    | Developer swaps curses renderer for a different output                                    | Only Renderer module changes; game logic untouched            |
| QS-5| Correctness   | Blinky targets PacMan's current tile in chase mode                                        | Blinky moves toward PacMan's exact position                   |
| QS-6| Correctness   | Player eats 4 ghosts on one power pellet                                                  | Score increments: 200, 400, 800, 1600                        |
| QS-7| Responsiveness| Player presses arrow key during gameplay                                                  | PacMan changes direction within the next tick (<100ms)        |
| QS-8| Responsiveness| Game runs on a standard machine                                                           | Maintains consistent tick rate without visible stutter        |
| QS-9| Simplicity    | New developer reads the codebase                                                          | Can understand module boundaries and data flow within 30 min  |
