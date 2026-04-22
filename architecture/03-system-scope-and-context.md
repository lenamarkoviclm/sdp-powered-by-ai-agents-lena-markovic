# 3. System Scope and Context

## 3.1 Business Context

PacMan is a self-contained terminal game. The system boundary is simple: a single player interacts with the game through a terminal.

| Actor/System | Description                                      |
|--------------|--------------------------------------------------|
| Player       | Human user who controls PacMan via keyboard      |
| Terminal     | Console that renders the game and captures input  |

- The player sends movement commands (arrow keys / WASD) to the game
- The game renders its state (maze, score, lives) to the terminal
- The terminal displays the rendered output back to the player

## 3.2 Technical Context

The game runs as a single Python process. The only external interface is the terminal via Python's `curses` module (standard library).

| Interface        | Technology       | Purpose                          |
|------------------|------------------|----------------------------------|
| Keyboard input   | `curses.getch()` | Capture player movement commands |
| Screen output    | `curses` window  | Render maze, entities, HUD       |

There are no network interfaces, databases, or file system dependencies at runtime.

## 3.3 C4 System Context Diagram

![C4 System Context](diagrams/c4-context.puml)

```plantuml
@startuml c4-context
!include https://raw.githubusercontent.com/plantuml-stdlib/C4-PlantUML/master/C4_Context.puml

title System Context Diagram — PacMan Game

Person(player, "Player", "Controls PacMan using keyboard input")

System(pacman_system, "PacMan Game", "Classic arcade game: navigate a maze, eat pellets, avoid ghosts, score points")

System_Ext(terminal, "Terminal / Console", "Renders the game grid and accepts keyboard input")

Rel(player, pacman_system, "Sends movement commands")
Rel(pacman_system, terminal, "Renders game state")
Rel(terminal, player, "Displays maze, score, lives")

@enduml
```

The diagram source is at `architecture/diagrams/c4-context.puml`.
