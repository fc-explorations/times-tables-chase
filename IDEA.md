# Times Table Chase

## Goal

Build a small educational game that runs as a single static HTML page, suitable for GitHub Pages. The game teaches and reinforces multiplication facts through movement, prediction, and number swapping.

The final implementation should be contained in one `index.html` file, with embedded CSS and JavaScript. It should not require a build step, server-side code, npm dependencies, or external assets.

## Core Concept

The player controls a character that carries a visible number. The room contains NPC characters, and each NPC also carries a visible number. The player must reach the exit while carrying the correct number required by a multiplication equation shown at the exit.

The exit sign shows a partial equation in one of these forms:

```text
A x ? = C
A x B = ?
```

The missing value is the target number. The player must be carrying that number when they step onto the exit tile.

Examples:

```text
3 x ? = 18
Target number: 6

4 x 7 = ?
Target number: 28
```

The challenge is that the player can only change the number they carry by interacting with NPCs.

## Platform Requirements

- Single HTML file.
- Must run by opening the file directly in a browser.
- Must also run when hosted on GitHub Pages.
- No backend.
- No bundler.
- No external dependencies required.
- Use plain HTML, CSS, and JavaScript.
- Canvas rendering is recommended.

## Screens And Navigation

The game should have three main screens:

- Splash screen.
- Settings screen.
- Game screen.

The splash screen is the first screen shown when the page loads.

The splash screen should include:

- Game title.
- Start button.
- Settings button.

The Start button begins a new run using the current settings as the starting difficulty. The Settings button opens the settings screen.

The settings screen should allow the player, parent, or teacher to configure the starting level generation and starting NPC behavior before beginning a run.

Required settings:

1. Starting number of NPCs.
2. Allowed values for `B` in the multiplication equation.
3. Starting NPC awareness amount from `0` to `1`.
4. Room size in number of tiles per side.
5. Speed multiplier.
6. Player-vs-NPC speed ratio.
7. Minimum and maximum square state duration in seconds.
8. Toggle for showing or hiding NPC awareness radius overlays.

The settings screen should include:

- A way to return to the splash screen.
- A way to start the game directly using the selected settings.
- Sensible defaults, so the game is playable without changing anything.

Suggested default settings:

```js
const settings = {
  npcCount: 5,
  allowedBValues: [2, 3, 4, 5],
  awarenessAmount: 0.5,
  roomSize: 10,
  speedMultiplier: 1,
  playerNpcSpeedRatio: 1,
  squareStateMinSeconds: 5,
  squareStateMaxSeconds: 12,
  showAwareness: true
};
```

`settings.roomSize = 10` means the room is `10 x 10` tiles. The room should stay square for the first version.

`settings.speedMultiplier` controls the movement speed for the player and NPCs. Higher speed should make the game harder and should increase score rewards.

`settings.playerNpcSpeedRatio` controls the player's speed relative to the NPCs:

- `1`: the player and NPCs use the same base movement speed, so a chasing NPC cannot close distance from behind by speed alone.
- Greater than `1`: the player moves faster than NPCs.
- Less than `1`: the player moves slower than NPCs, so a chasing NPC can catch the player.

Lower player-vs-NPC speed ratios should increase score rewards because the room is harder.

`settings.squareStateMinSeconds` and `settings.squareStateMaxSeconds` control how long each NPC remains in one square-wave state before switching between attraction and repulsion. Both endpoints should be configurable from `5` to `30` seconds. Each NPC should pick a state duration from this range when a level is generated, and the full square-wave cycle should be twice that duration.

The settings define the start of a run only. As the player solves rooms and advances to later levels, the game should automatically increase difficulty from those starting values.

The settings should be stored in memory at minimum. Local storage is optional if the implementation wants settings to persist after page refresh.

## Camera And Perspective

The game should use an isometric perspective.

Internally, game logic can use a simple 2D grid or 2D world coordinate system. Rendering should project world coordinates into isometric screen coordinates.

Recommended projection:

```js
screenX = (worldX - worldY) * tileWidth / 2;
screenY = (worldX + worldY) * tileHeight / 2;
```

The room should visually read as an isometric floor. Characters can be rendered as simple shapes or sprites standing on the floor. The player enters from the bottom-left corner. The exit is placed at the top-right corner.

## Room Layout

The room is a bounded rectangular or square isometric area.

Required layout:

- Player start: bottom-left corner of the room.
- Exit: top-right corner of the room.
- NPCs: distributed inside the room.
- Exit sign: next to or above the exit, clearly showing the partial equation.

The player wins only when they reach the exit while carrying the correct target number.

If the player reaches the exit with the wrong number, the game should clearly reject the attempt and continue the level.

## Characters

There are two kinds of characters:

- Player
- NPC

All characters have:

- Position in world coordinates.
- Velocity or movement direction.
- A carried number.
- A visible label showing the carried number.
- Collision or interaction radius.

The player is controlled by keyboard, preferably arrow keys or WASD.

NPCs are autonomous.

## Number Exchange Mechanic

When two characters interact, they exchange the numbers they are carrying.

This includes:

- Player with NPC.
- NPC with NPC.

The simplest implementation is:

1. Detect when two characters overlap or come within interaction distance.
2. Swap their `carriedNumber` values.
3. Apply a short cooldown to both characters so they do not instantly swap back on the next frame.

Example:

```js
function swapNumbers(a, b) {
  const temp = a.carriedNumber;
  a.carriedNumber = b.carriedNumber;
  b.carriedNumber = temp;
}
```

The swap should be visible immediately because the number label above each character changes.

## Educational Rule

Every level should generate a multiplication puzzle.

The equation has three values:

```text
A x B = C
```

One of `B` or `C` is hidden.

The missing value is the required carried number.

Suggested level generation:

- Choose `A` from 2 to 12.
- Choose `B` from the configured `settings.allowedBValues`.
- Compute `C = A * B`.
- Randomly choose whether to hide `B` or `C`.
- Set `targetNumber` to the hidden value.
- Ensure one character in the room starts with `targetNumber`.
- Add distractor numbers to other NPCs and possibly to the player.

The `allowedBValues` setting controls which multiplication tables appear.

For example:

```js
settings.allowedBValues = [2, 3];
```

This means generated levels only use equations where `B` is `2` or `3`, such as:

```text
7 x 2 = 14
5 x 3 = 15
```

If the equation hides `B`, then the missing answer is one of the configured values.

Example:

```text
7 x ? = 14
Target number: 2
```

If the equation hides `C`, then the missing answer is a product from the configured multiplication tables.

Example:

```text
5 x 3 = ?
Target number: 15
```

This lets a parent or teacher focus the game on specific tables.

The player should need to reason about the multiplication equation to know which number to obtain.

## NPC Movement

NPCs usually move randomly.

Default NPC behavior:

- Wander around the room.
- Occasionally choose a new random direction.
- Avoid leaving the room bounds.
- Move slower than or similar to the player.
- Use inertia so movement eases toward target velocity instead of snapping instantly.

Each NPC has an awareness radius derived from the configured awareness amount. When another character enters this radius, the NPC stops pure random wandering and reacts to that character.

The reaction is either:

- Attraction: move toward the detected character.
- Repulsion: move away from the detected character.

This creates moving number targets that the player must chase, avoid, predict, or manipulate.

## NPC Awareness

Each NPC should scan for nearby characters within its awareness radius.

The settings screen exposes starting awareness as a normalized value between `0` and `1`:

- `0`: NPCs are never aware of other characters and always move randomly.
- `1`: NPCs are always aware of every other character in the room.
- Values between `0` and `1`: NPCs are aware within a proportional radius.

During a run, the actual level awareness should be derived from the starting setting and the current level.

Recommended implementation:

```js
function getAwarenessRadius(currentAwarenessAmount, room) {
  const maxDistance = Math.hypot(room.width, room.height);
  return currentAwarenessAmount * maxDistance;
}
```

With this approach, `currentAwarenessAmount = 0` gives an awareness radius of `0`, and `currentAwarenessAmount = 1` gives an awareness radius large enough to cover the full room.

Nearby characters may include:

- The player.
- Other NPCs.

If the player is inside an NPC's awareness radius, the NPC must always focus on the player, even if another NPC is closer. This prevents NPCs from getting locked into stable NPC-vs-NPC attraction pairs while ignoring the player.

If the player is not inside the awareness radius and multiple NPCs are inside it, the NPC should focus on the closest NPC. The chosen focus determines whether the NPC moves toward or away from that target, depending on the NPC's current square-wave mode.

Pseudo-code:

```js
function findAwarenessTarget(npc, characters) {
  if (distanceBetween(npc, player) < npc.awarenessRadius) {
    return player;
  }

  let closest = null;
  let closestDistance = Infinity;

  for (const other of characters) {
    if (other === npc) continue;
    if (other.type === "player") continue;

    const distance = distanceBetween(npc, other);
    if (distance < npc.awarenessRadius && distance < closestDistance) {
      closest = other;
      closestDistance = distance;
    }
  }

  return closest;
}
```

If no target is inside the radius, the NPC wanders randomly.

## Square Wave Personality

Each NPC has a unique square wave. The square wave determines whether the NPC is currently attracted to or repelled by nearby characters.

Each NPC is characterized by:

- `phase`: starting offset of the wave.
- `wavelength`: duration of a full attraction/repulsion cycle.
- `stateDuration`: duration of each attraction or repulsion state.

Use game time to evaluate the square wave.

Recommended interpretation:

- First half of the cycle: attraction.
- Second half of the cycle: repulsion.
- The cycle wavelength is `stateDuration * 2`.

Pseudo-code:

```js
function getNpcMode(npc, timeSeconds) {
  const t = (timeSeconds + npc.phase) % npc.wavelength;
  const halfWave = npc.wavelength / 2;
  return t < halfWave ? "attract" : "repel";
}
```

Each NPC should have a different phase and wavelength so their behaviors do not synchronize.

Example NPC configuration:

```js
{
  carriedNumber: 6,
  awarenessRadius: 3.5,
  phase: 0.8,
  wavelength: 4.0
}
```

The attraction/repulsion mode can be shown visually with subtle effects, such as:

- A colored ring around the NPC.
- Blue for attraction.
- Red for repulsion.
- A pulsing awareness circle.

This is optional but useful for readability.

## NPC Steering

When an NPC has a target inside its awareness radius:

```js
const direction = normalize(target.position - npc.position);

if (mode === "attract") {
  npc.velocity = direction * npc.speed;
}

if (mode === "repel") {
  npc.velocity = -direction * npc.speed;
}
```

When the NPC has no target:

```js
npc.velocity = randomWanderVelocity;
```

NPCs should still respect room bounds.

## Player Controls

Recommended controls:

- Arrow keys or WASD to move.
- Movement should be continuous rather than tile-by-tile.
- Movement should include light inertia, so stopping and direction changes feel smooth rather than jittery.

Because the view is isometric, controls should remain intuitive:

- Up moves toward the top of the screen.
- Down moves toward the bottom of the screen.
- Left moves toward the left of the screen.
- Right moves toward the right of the screen.

The implementation can convert screen-direction input into world-coordinate movement.

## Win Condition

The player wins when:

1. The player reaches the exit area.
2. The player's carried number equals `targetNumber`.

On win:

- Show a success message.
- Show the solved equation.
- Award points for the solved room.
- Automatically advance to the next level after a short pause.

If the player reaches the exit with the wrong number:

- Show a short error or warning message.
- Do not reset the level.
- Let the player continue trying.

## Level Progression

Progression to the next level should be automatic. The player should not need to manually choose the next level after solving a room.

Each new level should generate:

- A new equation.
- A new target number.
- New NPC numbers.
- New NPC phases and wavelengths.
- A larger or equal number of NPCs than the previous level.
- A larger or equal awareness radius than the previous level.

The settings screen defines the starting level only:

- `settings.npcCount` is the number of NPCs on level 1.
- `settings.awarenessAmount` is the awareness amount on level 1.
- `settings.allowedBValues` defines the multiplication table range or set used throughout the run.
- `settings.roomSize` defines the square room size used for the run.

Recommended automatic progression:

```js
function getLevelNpcCount(settings, level) {
  return settings.npcCount + Math.floor((level - 1) / 2);
}

function getLevelAwarenessAmount(settings, level) {
  return Math.min(1, settings.awarenessAmount + (level - 1) * 0.05);
}
```

This means:

- NPC count increases by 1 every 2 levels.
- Awareness increases by `0.05` each level.
- Awareness is capped at `1`.

Suggested difficulty progression:

- Early levels: hide `C`, so the player calculates the product.
- Later levels: sometimes hide `B`, so the player solves the missing factor.
- Add more distractor NPCs.
- Increase the number of NPCs automatically.
- Increase NPC awareness radius automatically.
- Give NPCs more varied square wave timing.

Difficulty progression should be predictable and derived from the starting settings. Do not treat settings as fixed values for every level.

## Scoring

The player should earn points every time they solve a room.

Points should increase with level and difficulty. Harder settings and harder generated levels should be worth more.

Difficulty factors that increase score:

- Higher level number.
- Larger allowed `B` value set.
- Larger values in the allowed `B` value set.
- Larger generated numbers, especially larger `A`, `B`, and `C`.
- More NPCs in the level.
- Larger NPC awareness radius.
- Higher speed multiplier.
- Lower player-vs-NPC speed ratio.

Suggested scoring formula:

```js
function calculateRoomScore(game) {
  const { level, settings, equation, currentNpcCount, currentAwarenessAmount } = game;

  const base = 100;
  const levelBonus = level * 25;
  const rangeBonus = settings.allowedBValues.length * 15;
  const maxB = Math.max(...settings.allowedBValues);
  const numberBonus = equation.a * 2 + equation.b * 3 + Math.floor(equation.c / 2) + maxB * 5;
  const npcBonus = currentNpcCount * 20;
  const awarenessBonus = Math.round(currentAwarenessAmount * 100);
  const speedBonus = Math.round(settings.speedMultiplier * 80);
  const playerRatioBonus = Math.round((1.6 - settings.playerNpcSpeedRatio) * 100);

  return base + levelBonus + rangeBonus + numberBonus + npcBonus + awarenessBonus + speedBonus + playerRatioBonus;
}
```

The exact values can be tuned, but the formula should preserve these principles:

- Solving later levels gives more points.
- Practicing more tables gives more points than practicing fewer tables.
- Larger multiplication values give more points.
- More NPCs gives more points.
- More awareness gives more points.
- Higher speed gives more points.
- Slower player movement relative to NPC movement gives more points.

The game should show the current score during play.

## Leaderboard

Add a score leaderboard.

The leaderboard should track high scores for completed runs or current attempts. Because the game is a single static HTML page, the leaderboard should be local to the browser.

Recommended implementation:

- Store leaderboard entries in `localStorage`.
- Keep the top 10 scores.
- Each entry should include score, highest level reached, and date.
- A player name field is optional. If no name is entered, use a default such as `"Player"`.

Suggested leaderboard entry:

```js
{
  name: "Player",
  score: 2450,
  highestLevel: 8,
  date: "2026-06-06"
}
```

The splash screen should include a way to view the leaderboard, or the leaderboard can be displayed on the splash screen below the Start and Settings buttons.

When a run ends, or when the player chooses to return to the splash screen, save the score if it qualifies for the leaderboard.

## Minimal Data Model

Suggested JavaScript objects:

```js
const game = {
  screen: "splash",
  level: 1,
  score: 0,
  highestLevelReached: 1,
  timeSeconds: 0,
  settings: {
    npcCount: 5,
    allowedBValues: [2, 3, 4, 5],
  awarenessAmount: 0.5,
  roomSize: 10,
  speedMultiplier: 1,
  playerNpcSpeedRatio: 1,
  squareStateMinSeconds: 5,
  squareStateMaxSeconds: 12
  },
  equation: {
    a: 3,
    b: 6,
    c: 18,
    missing: "b",
    targetNumber: 6
  },
  room: {
    width: 10,
    height: 10
  },
  currentNpcCount: 5,
  currentAwarenessAmount: 0.5,
  player: {
    type: "player",
    x: 0.5,
    y: 9.5,
    carriedNumber: 4,
    speed: 3,
    swapCooldown: 0
  },
  npcs: [
    {
      type: "npc",
      x: 4,
      y: 4,
      carriedNumber: 6,
      speed: 1.5,
      awarenessRadius: 7.1,
      phase: 0.5,
      wavelength: 4,
      wanderTimer: 0,
      swapCooldown: 0
    }
  ],
  exit: {
    x: 9.5,
    y: 0.5,
    radius: 0.6
  },
  leaderboard: [
    {
      name: "Player",
      score: 2450,
      highestLevel: 8,
      date: "2026-06-06"
    }
  ]
};
```

In this example, `currentNpcCount` and `currentAwarenessAmount` should be computed from the starting settings and the current level. `awarenessRadius` should be computed from `currentAwarenessAmount` and the room size when the level is created.

## Rendering Priorities

The game should clearly communicate:

- The player's carried number.
- Each NPC's carried number.
- The exit equation.
- The target objective.
- The current level.
- The current score.
- The room boundaries.
- The difference between the player and NPCs.
- NPC attraction or repulsion state when awareness overlays are enabled.

Recommended visual choices:

- Isometric diamond tiles for the floor.
- Player in a distinct color.
- NPCs in neutral colors.
- Number labels above characters.
- Exit marker or doorway at the top-right corner.
- Signboard near the exit with the partial equation.

To draw objects correctly in isometric perspective, sort characters by their world depth before rendering:

```js
characters.sort((a, b) => (a.x + a.y) - (b.x + b.y));
```

## Implementation Notes For An LLM Copilot

Start with a single `index.html` containing:

- `<canvas>` for the game.
- Embedded `<style>` for layout.
- Embedded `<script>` for all game logic.

Recommended implementation order:

1. Create canvas and resize handling.
2. Add basic screen state for splash, settings, and game.
3. Build splash screen buttons and leaderboard display.
4. Build settings controls for starting NPC count, allowed `B` values, starting awareness amount, room size, speed multiplier, player-vs-NPC speed ratio, square state duration range, and awareness overlay visibility.
5. Implement world-to-isometric projection.
6. Draw the isometric room.
7. Add player movement.
8. Add NPCs with visible numbers, using level-derived NPC count.
9. Add number swapping on collision.
10. Add equation generation and exit sign, using `settings.allowedBValues`.
11. Add win and wrong-number exit checks.
12. Add scoring when a room is solved.
13. Add automatic progression to the next level.
14. Add NPC random wandering.
15. Add NPC awareness radius, using level-derived awareness amount.
16. Add closest-character focus inside the awareness radius.
17. Add square-wave attraction and repulsion.
18. Add local-storage leaderboard.
19. Add polish.

Keep the first version mechanically complete before adding decorative effects.

## Design Intent

The game should feel like a small tactical chase puzzle rather than a quiz. The multiplication equation tells the player what number they need, but the room dynamics determine how they obtain it.

The educational value comes from repeatedly solving multiplication facts under light movement pressure, while the square-wave NPC behavior adds timing and prediction.
