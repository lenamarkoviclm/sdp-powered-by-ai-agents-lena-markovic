# 6. Runtime View

## 6.1 Game Loop — Single Tick

The core game loop runs a fixed-timestep cycle every tick:

![Sequence — Game Loop](diagrams/sequence-game-loop.puml)

1. **Input**: Poll keyboard for player command (non-blocking)
2. **Update**: Move PacMan based on command, then update all ghost positions via AI
3. **Collisions**: Detect PacMan-pellet and PacMan-ghost collisions, publish events
4. **Render**: Pass current game state to renderer for terminal output

## 6.2 Power Pellet Eaten

When PacMan eats a power pellet, all ghosts enter frightened mode temporarily.

![Sequence — Power Pellet](diagrams/sequence-power-pellet.puml)

1. CollisionDetector publishes `PowerPelletEaten` on the EventBus
2. PlayerModule subscribes — adds 50 points to score
3. GhostStateMachine subscribes — queries LevelManager for frightened duration (decreases with level), transitions all ghosts to FRIGHTENED mode
4. Ghosts reverse direction and move randomly until the timer expires
5. On timer expiry, ghosts return to their previous mode (chase or scatter)

## 6.3 Ghost Collision

Two outcomes depending on ghost mode at the moment of collision.

![Sequence — Ghost Collision](diagrams/sequence-ghost-collision.puml)

**Chase/Scatter mode** (ghost kills PacMan):
1. `GhostCollision{mode=CHASE}` published
2. PlayerModule loses a life
3. If lives remain → `LifeLost` event, positions reset
4. If no lives → `GameOver` event, game state transitions to GAME_OVER

**Frightened mode** (PacMan eats ghost):
1. `GhostCollision{mode=FRIGHTENED}` published
2. PlayerModule gains score (200 × multiplier, escalating per ghost eaten)
3. GhostStateMachine transitions that ghost to EATEN — it navigates back to the ghost house and respawns
