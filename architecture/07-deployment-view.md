# 7. Deployment View

## 7.1 Infrastructure

PacMan runs as a single Python process inside a terminal emulator. There are no servers, containers, or network infrastructure.

![Deployment View](diagrams/deployment-view.puml)

## 7.2 Deployment Nodes

| Node               | Description                                                        |
|--------------------|--------------------------------------------------------------------|
| Player's Machine   | Any machine with Python 3.12 and a terminal (Linux, macOS, Windows)|
| Terminal Emulator  | bash, zsh, cmd, or any terminal that supports curses rendering     |
| Python 3.12 Runtime| CPython interpreter — runs the game as a single process            |

## 7.3 How to Run

```bash
python3 -m pacman
```

No installation steps beyond having Python 3.12. No external dependencies, no build step, no configuration files.

## 7.4 Platform Notes

| Platform | curses support                                                  |
|----------|-----------------------------------------------------------------|
| Linux    | `curses` included in standard library                           |
| macOS    | `curses` included in standard library                           |
| Windows  | Requires `windows-curses` package, or use WSL                   |
