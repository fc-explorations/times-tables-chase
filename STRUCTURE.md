# Project Structure

## Goal

This project should remain simple to run and publish. The game must work as a static GitHub Pages project and should be playable from a single `index.html` page.

The preferred implementation is:

- One main `index.html` file.
- Embedded CSS.
- Embedded JavaScript.
- Optional local image/audio assets in small folders.
- No backend.
- No build step.
- No npm dependency required.

## Recommended Repository Layout

```text
TimesTableChase/
  index.html
  IDEA.md
  STYLE.md
  STRUCTURE.md
  assets/
    characters/
    room/
    ui/
    effects/
    audio/
```

Only `index.html` is required for the first playable version. The `assets/` folder is optional and should be added only when generated or hand-made assets are actually used.

## Root Files

### `index.html`

The playable game.

It should contain:

- HTML shell.
- Canvas element.
- Splash screen UI.
- Settings screen UI.
- Leaderboard UI.
- Embedded CSS.
- Embedded JavaScript.

The game should run when opened directly in a browser and when hosted on GitHub Pages.

### `IDEA.md`

The game design specification.

Use this file to understand:

- Core mechanics.
- Educational rules.
- NPC behavior.
- Scoring.
- Level progression.
- Win conditions.

### `STYLE.md`

The visual art direction.

Use this file to guide:

- Image generation.
- Character design.
- Room design.
- UI visual style.
- Color and lighting choices.

### `STRUCTURE.md`

The implementation and asset organization guide.

Use this file to decide:

- Where files should go.
- How assets should be named.
- How the single-page implementation should be organized.

## Single-Page Implementation Structure

Even though the game should live in one HTML file, the code inside `index.html` should still be organized into clear sections.

Recommended order:

```html
<!doctype html>
<html>
  <head>
    <!-- Metadata -->
    <!-- Embedded CSS -->
  </head>
  <body>
    <!-- Splash screen -->
    <!-- Settings screen -->
    <!-- Game screen / canvas -->
    <!-- Leaderboard UI -->
    <!-- Embedded JavaScript -->
  </body>
</html>
```

Recommended JavaScript sections:

```text
Constants
Settings defaults
Game state
DOM references
Utility functions
Projection and rendering helpers
Equation generation
Level generation
Scoring and leaderboard
Input handling
Player update logic
NPC update logic
Collision and number swapping
Win and level progression
Main update loop
Main render loop
Screen navigation
Initialization
```

Keep these sections separated with short comments so an LLM copilot can modify the game without hunting through unrelated code.

## Screen Organization

The game has three main screens:

- Splash screen.
- Settings screen.
- Game screen.

The splash screen should contain:

- Game title.
- Start button.
- Settings button.
- Leaderboard display or leaderboard button.

The settings screen should contain:

- Starting NPC count control.
- Allowed `B` values control.
- Starting awareness control.
- Back button.
- Start button.

The game screen should contain:

- Canvas.
- Current score.
- Current level.
- Current equation.
- Optional pause or return button.

## Asset Folder

Use `assets/` only if the implementation uses image or audio files. If the game is fully drawn with canvas shapes, `assets/` can be omitted.

Recommended structure:

```text
assets/
  characters/
  room/
  ui/
  effects/
  audio/
```

### `assets/characters/`

Character sprites and variants.

Examples:

```text
player_cyan_idle.png
npc_orange_idle.png
npc_pink_idle.png
npc_yellow_idle.png
```

Generated character assets should not include baked-in numbers. Numbers should usually be drawn dynamically in canvas.

### `assets/room/`

Room, floor, wall, and exit assets.

Examples:

```text
tile_floor_default.png
tile_floor_math_symbol.png
wall_low_straight.png
wall_x_2.png
wall_x_4.png
wall_y_2.png
wall_y_4.png
exit_blue_doorway.png
sign_chalkboard_blank.png
```

The sign asset should usually be blank so the equation can be rendered dynamically.

### `assets/ui/`

Menu panels, buttons, badges, and leaderboard decorations if image assets are needed.

Examples:

```text
panel_light.png
badge_number_blank.png
button_blue.png
```

Prefer CSS for UI when possible. Use image assets only when they add meaningful visual quality.

### `assets/effects/`

Short visual effects.

Examples:

```text
ring_awareness_blue.png
ring_awareness_red.png
swap_arrows_yellow.png
success_spark.png
```

Effects should have transparent backgrounds.

### `assets/audio/`

Optional sound effects.

Examples:

```text
swap.wav
success.wav
wrong_exit.wav
button_click.wav
```

Audio should be optional. The game should still work if audio fails to load.

## Naming Conventions

Use lowercase file names with underscores.

Good:

```text
npc_orange_idle.png
tile_floor_default.png
ring_awareness_blue.png
```

Avoid:

```text
NPC Orange Idle.png
floorTileFinal2.png
new_asset_REAL_FINAL.png
```

Use names that describe:

- Asset type.
- Variant.
- State.
- Color, if relevant.

Recommended pattern:

```text
type_variant_state.png
```

Examples:

```text
player_cyan_idle.png
npc_orange_idle.png
ring_awareness_red.png
button_primary_idle.png
button_primary_hover.png
```

## Asset Generation Rules

Generated assets should follow `STYLE.md`.

Important rules:

- Use transparent backgrounds for sprites, props, and effects.
- Use orthographic isometric view for room and character assets.
- Do not bake dynamic text into image assets.
- Do not bake carried numbers into character assets.
- Do not bake equations into sign assets.
- Keep silhouettes readable at small sizes.
- Export larger than needed and scale down in game.

Recommended source size:

```text
512 x 512 px
```

The game can render these at smaller sizes depending on screen resolution.

## Dynamic Text

The following should be rendered by the game, not embedded in image assets:

- Character carried numbers.
- Exit equation.
- Current score.
- Current level.
- Leaderboard entries.
- Settings values.

This keeps the game flexible and avoids unreadable generated text.

## GitHub Pages Notes

GitHub Pages should be able to serve the game directly from the repository.

Recommended deployment target:

```text
index.html
assets/
```

Avoid absolute local file paths. Asset URLs should be relative.

Good:

```js
const playerImage = new Image();
playerImage.src = "assets/characters/player_cyan_idle.png";
```

Avoid:

```js
playerImage.src = "/Users/name/project/assets/player.png";
```

## Local Storage

The leaderboard and optional saved settings can use browser `localStorage`.

Suggested keys:

```js
const STORAGE_KEYS = {
  settings: "timesTableChase.settings",
  leaderboard: "timesTableChase.leaderboard"
};
```

Stored data should be treated as optional. If parsing fails, the game should fall back to defaults.

## Implementation Constraints

Keep the first version mechanically complete before adding asset complexity.

Recommended first version:

- Draw everything with canvas shapes.
- Use dynamic canvas text for numbers and equations.
- Implement splash, settings, game, scoring, and leaderboard.
- Implement player movement, NPC movement, swapping, and automatic progression.

Recommended later version:

- Add generated character sprites.
- Add generated room tiles.
- Add visual effects.
- Add optional audio.

## LLM Copilot Guidance

When implementing the game, prefer this order:

1. Create `index.html`.
2. Implement screen navigation.
3. Implement settings.
4. Implement game state and level generation.
5. Implement canvas rendering with simple shapes.
6. Implement player movement.
7. Implement NPC movement.
8. Implement number swapping.
9. Implement win checks and automatic level progression.
10. Implement scoring.
11. Implement local leaderboard.
12. Add assets only after the canvas-only version works.

Do not split the first implementation into many source files unless the project requirements change. The single-file target is important because the game should be easy to run from GitHub Pages and easy to inspect.
