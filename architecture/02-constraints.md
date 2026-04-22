# 2. Architecture Constraints

## 2.1 Technical Constraints

| Constraint                  | Description                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| Python 3.12                 | Language and runtime are fixed by course requirements                       |
| Standard library only       | No external game frameworks or third-party packages (e.g., pygame, arcade)  |
| Terminal rendering           | Game output via terminal/console using curses or similar stdlib module       |
| Keyboard input only         | Player input limited to terminal keyboard events                            |
| Single process              | Game runs as a single Python process — no networking, no multiprocessing    |

## 2.2 Organizational Constraints

| Constraint                  | Description                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| Course project              | Must follow course guidelines for structure, documentation, and delivery    |
| arc42 documentation         | Architecture documented using the arc42 template                            |
| Solo development            | Single developer — architecture must stay simple and maintainable           |

## 2.3 Conventions

| Convention                  | Description                                                                 |
|-----------------------------|-----------------------------------------------------------------------------|
| Domain-Driven Design        | System decomposed into bounded contexts: MAZE, PLAYER, GHOST, GAME         |
| Event-driven communication  | Components communicate via events for loose coupling (see ADR-003)          |
| Separation of concerns      | Game logic decoupled from rendering and input handling (see ADR-002)        |
| Testability first           | All game logic testable without terminal — no UI dependencies in core logic |
