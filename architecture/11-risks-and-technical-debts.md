# 11. Risks and Technical Debts

## 11.1 Risks

| ID   | Risk                                      | Probability | Impact | Mitigation                                                        |
|------|-------------------------------------------|-------------|--------|-------------------------------------------------------------------|
| R-1  | curses not available on Windows            | High        | Medium | Document WSL requirement; consider `windows-curses` as fallback   |
| R-2  | Terminal too small for maze rendering      | Medium      | Medium | Detect terminal size at startup; show error and pause if too small|
| R-3  | Game loop timing inconsistency            | Medium      | High   | Use `time.monotonic()` for tick timing; decouple logic from render speed |
| R-4  | Ghost AI doesn't match classic behavior   | Medium      | Medium | Reference original PacMan Dossier for targeting algorithms; validate with tests |
| R-5  | Event bus makes debugging harder          | Low         | Medium | File-based logging of all published events (ADR-006); keep event catalog documented |

## 11.2 Technical Debts

| ID   | Debt                                      | Description                                                       | Priority |
|------|-------------------------------------------|-------------------------------------------------------------------|----------|
| TD-1 | No persistent high scores                 | Scores are lost when the game exits; no file-based persistence yet| Low      |
| TD-2 | Single maze layout                        | Only one hardcoded maze; no level editor or maze loading from file | Low      |
| TD-3 | No sound                                  | Terminal rendering has no audio support; classic PacMan has iconic sounds | Low   |
| TD-4 | Fruit bonus not implemented               | Classic PacMan has bonus fruit items per level; not in initial scope | Low     |
| TD-5 | No integration tests for curses rendering | Renderer is manually tested; automated terminal testing is complex | Medium   |
