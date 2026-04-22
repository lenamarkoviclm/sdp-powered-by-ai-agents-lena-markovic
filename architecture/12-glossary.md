# 12. Glossary

| Term                | Definition                                                                                      |
|---------------------|-------------------------------------------------------------------------------------------------|
| Pellet              | Small dot in the maze that PacMan eats for 10 points                                            |
| Power Pellet        | Large dot that makes ghosts frightened for a limited time; worth 50 points                      |
| Ghost House         | Central area where ghosts spawn and return after being eaten                                    |
| Chase Mode          | Ghost AI mode where each ghost targets PacMan using its unique strategy                         |
| Scatter Mode        | Ghost AI mode where each ghost targets its assigned corner of the maze                          |
| Frightened Mode     | Ghost state after a power pellet is eaten; ghosts move randomly and can be eaten by PacMan      |
| Eaten Mode          | Ghost state after being eaten by PacMan; ghost navigates back to the ghost house to respawn     |
| Targeting Strategy  | Algorithm a ghost uses to select its target tile in chase mode (varies per ghost type)           |
| Tick                | One iteration of the game loop: input → update → render                                         |
| Bounded Context     | A DDD concept; a self-contained domain area with its own models and logic (Maze, Player, Ghost, Game) |
| EventBus            | Publish/subscribe dispatcher that enables decoupled communication between bounded contexts       |
| Game State          | Top-level mode the game is in: menu, playing, paused, game-over, level transition               |
| Blinky              | Red ghost; targets PacMan's current tile directly                                               |
| Pinky               | Pink ghost; targets 4 tiles ahead of PacMan's facing direction                                  |
| Inky                | Cyan ghost; targets using a vector from Blinky's position through a point 2 tiles ahead of PacMan |
| Clyde               | Orange ghost; targets PacMan when far (>8 tiles), retreats to scatter corner when close         |
| curses              | Python standard library module for terminal-based UI rendering and keyboard input               |
| ADR                 | Architecture Decision Record; documents a significant design decision with context and rationale |
| C4 Model            | Software architecture model with four levels: Context, Container, Component, Code               |
| arc42               | Template for documenting software architecture in 12 chapters                                   |
