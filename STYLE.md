# Visual Style Guide

## Style Goal

The game should look like a bright, readable isometric classroom-lab puzzle. It should feel educational without looking like a worksheet. The world is playful, clean, and geometric, with characters and numbers designed for instant readability at small sizes.

The intended output is a set of image-generation prompts or asset briefs for room tiles, characters, props, exit signage, and UI elements that can be used in a single-page HTML game.

## Overall Art Direction

Use a stylized 2.5D isometric look with simple shapes, soft edges, and crisp silhouettes.

Keywords:

- Isometric educational puzzle game
- Bright classroom-lab environment
- Clean geometric shapes
- Soft toy-like materials
- Clear readable numbers
- Friendly but not childish
- High contrast gameplay objects
- No heavy texture noise
- No realistic grime
- No dark fantasy or sci-fi mood

The game should resemble a polished digital board game or educational app, not a realistic 3D scene.

## Camera And Asset Angle

All room and character assets should be designed for an isometric camera.

Recommended angle:

- Camera rotated 45 degrees around the vertical axis.
- Camera looking down at roughly 30 to 35 degrees.
- Orthographic projection.

Assets should be generated or drawn as if they sit on an isometric diamond floor.

Characters should face generally toward the camera or slightly toward the lower-right side of the screen, so their bodies and carried numbers remain visible.

## Color Palette

The palette should be cheerful and varied, with good contrast between gameplay roles.

Suggested palette:

- Floor base: warm light gray, pale mint, or soft blue-gray.
- Tile accents: muted teal, pale yellow, soft coral, and white.
- Player: saturated green or cyan.
- NPCs: varied warm colors such as orange, yellow, pink, and violet.
- Exit: bright blue with white and yellow details.
- Correct objective highlights: green.
- Warning or wrong exit attempt: coral red.
- Attraction state: clear blue.
- Repulsion state: clear red-orange.

Avoid a single dominant hue across the whole game. The scene should not become mostly purple, mostly beige, mostly dark blue, or mostly brown.

## Lighting

Lighting should be soft and consistent.

Use:

- Soft overhead light.
- Gentle ambient shadows under characters.
- Light contact shadows under props.
- No dramatic shadows.
- No high-contrast noir lighting.

The scene must remain readable on small screens. Shadows should support depth but never obscure numbers or gameplay objects.

## Room Style

The room is an isometric learning arena: part classroom, part math activity floor, part playful lab.

Core room features:

- Diamond-shaped isometric floor tiles.
- Low boundary walls or raised edges.
- Bottom-left entrance zone.
- Top-right exit doorway.
- Exit sign mounted near the exit.
- Minimal props that do not block gameplay.

Suggested visual motifs:

- Subtle multiplication symbols on some tiles.
- Small chalk marks or math glyphs as floor decoration.
- Rounded educational blocks along the walls.
- Soft plastic or painted wood materials.
- Thin colored edge lines to clarify tile boundaries.

The room should not be cluttered. The player needs to track moving characters, so the floor must stay visually calm.

## Floor Tiles

Floor tiles should be modular and easy to repeat.

Tile style:

- Isometric diamond shape.
- Light surface.
- Thin outline.
- Slight bevel or soft edge.
- Occasional accent tiles with simple math symbols.
- No detailed texture that competes with numbers.

Prompt fragment:

```text
single isometric diamond floor tile, bright educational puzzle game style, light blue-gray surface, subtle bevel, thin teal outline, soft shadow, clean vector-like 3D render, orthographic view, transparent background
```

## Walls And Boundaries

The room boundary should be low and readable, not tall enough to hide characters.

Wall style:

- Low isometric wall blocks.
- Rounded edges.
- Pale neutral surfaces.
- Colored top trim.
- Soft contact shadows.

The walls should communicate the room boundary clearly while leaving the play area open.

## Exit

The exit is at the top-right corner and should be instantly recognizable.

Exit style:

- Small isometric doorway or portal.
- Bright blue frame.
- White interior glow or light panel.
- Yellow trim or arrow marker.
- Signboard placed above or beside it.

The exit should feel inviting but should not look magical or dangerous.

Prompt fragment:

```text
isometric educational game exit doorway, bright blue rounded frame, white glowing interior panel, yellow arrow trim, clean toy-like 3D style, orthographic isometric view, transparent background
```

## Equation Sign

The equation sign is one of the most important assets.

It must be:

- Readable.
- Simple.
- High contrast.
- Large enough for multiplication equations.
- Visually attached to the exit area.

Sign style:

- Small classroom chalkboard or digital whiteboard.
- Rounded rectangle.
- Dark green chalkboard or white display surface.
- Thick readable equation text.
- Space for forms like `3 x ? = 18` and `4 x 7 = ?`.

If image generation cannot produce reliable text, generate the board blank and render the equation dynamically in HTML canvas. This is the recommended implementation approach.

Prompt fragment for blank sign:

```text
blank isometric classroom equation signboard, rounded dark green chalkboard surface, light wood frame, mounted on small stand, clean educational puzzle game style, no text, orthographic isometric view, transparent background
```

## Character Style

Characters should be simple number-carrying figures with clear silhouettes.

They are not realistic humans. They should look like friendly classroom game pieces or small toy mascots.

Core character features:

- Rounded body.
- Small feet or base.
- Simple face.
- Short arms holding or wearing a number badge.
- Large visible carried number above the head or on a front placard.
- Soft shadow underneath.

The player and NPCs should share the same construction style but differ in color and visual priority.

## Player Character

The player should be the most recognizable character.

Player style:

- Main color: saturated green or cyan.
- Slightly brighter outline or rim.
- Confident forward-facing pose.
- Larger or clearer number badge.
- Optional small backpack or sash to imply carrying a number.

The player should look active and controllable.

Prompt fragment:

```text
isometric toy-like player character for educational puzzle game, rounded cyan body, simple friendly face, small feet, holding a blank number placard, clean 2.5D render, orthographic isometric view, soft shadow, transparent background, no text
```

## NPC Characters

NPCs should be visually distinct from the player but clearly belong to the same world.

NPC style:

- Rounded toy-like bodies.
- Different body colors.
- Simpler than the player.
- Blank number placard or space for dynamic number text.
- Neutral friendly expressions.

Each NPC can have a small visual variation:

- Different color.
- Different head shape.
- Small cap, antenna-like knob, or scarf.
- Different placard shape.

Avoid making NPCs too detailed, because several will be on screen at once.

Prompt fragment:

```text
isometric toy-like NPC character for educational puzzle game, rounded orange body, simple friendly face, holding a blank number placard, clean 2.5D render, orthographic isometric view, soft shadow, transparent background, no text
```

## Number Display

Numbers are gameplay-critical. They should usually be rendered dynamically in the game rather than baked into image assets.

Recommended approach:

- Generate blank placards or badges.
- Draw numbers with canvas text.
- Use a bold rounded sans-serif font.
- Use dark text on a light badge.
- Add a small outline or shadow for contrast.

Badge style:

- White or pale yellow placard.
- Rounded corners.
- Dark outline.
- Large centered number.

Numbers should remain readable even when characters overlap visually.

## Awareness Radius

NPC awareness can be shown as a subtle isometric ring or translucent area around the NPC.

Attraction state:

- Blue ring or blue pulsing outline.
- Calm and smooth.

Repulsion state:

- Red-orange ring or red pulsing outline.
- Slightly sharper or more energetic.

These indicators should be optional and lightweight. They should not hide the floor or make the screen noisy.

Prompt fragment for ring:

```text
transparent isometric awareness ring effect, thin glowing blue ellipse, soft edge, clean educational puzzle game style, transparent background
```

## Interaction And Swap Effect

When two characters swap numbers, use a small readable effect.

Suggested effect:

- Two curved arrows between the characters.
- Small sparkle or pop near the number badges.
- Brief yellow highlight on both badges.

The effect should be clear but short-lived.

Prompt fragment:

```text
small isometric number swap effect, two curved yellow arrows forming an exchange symbol, clean educational game style, soft glow, transparent background
```

## UI Style

UI should match the classroom-lab theme.

Use:

- Rounded panels with restrained corners.
- White or very light panels.
- Dark readable text.
- Accent colors from the room palette.
- Simple icon-like symbols.

Avoid large decorative menus. The main screen should be the playable room.

## Asset Generation Rules

For image generation:

- Ask for transparent backgrounds for character, prop, sign, and effect assets.
- Ask for orthographic isometric view.
- Ask for no text when the game needs dynamic numbers or equations.
- Ask for clean silhouettes.
- Ask for simple material and low texture detail.
- Generate assets at higher resolution than needed, then scale down in the game.

Suggested export sizes:

- Character sprites: 512 x 512.
- Floor tiles: 512 x 512.
- Exit prop: 512 x 512.
- Signboard: 512 x 512.
- Effects: 512 x 512.

## Negative Prompt Guidance

Avoid:

- Photorealism.
- Dark cinematic lighting.
- Busy backgrounds.
- Tiny unreadable text.
- Baked-in numbers unless explicitly requested.
- Complex facial details.
- Harsh outlines.
- Pixel art, unless the whole game changes to pixel art.
- Anime style.
- Medieval fantasy.
- Futuristic cyberpunk.
- Horror or danger themes.
- Heavy gradients that obscure gameplay.

## Master Prompt Template

Use this template for individual assets:

```text
[asset description], isometric educational puzzle game asset, bright classroom-lab style, clean toy-like 2.5D render, rounded geometric shapes, soft overhead lighting, gentle contact shadow, crisp readable silhouette, orthographic isometric camera, high contrast gameplay readability, transparent background, no text
```

Example:

```text
small rounded NPC character holding a blank number placard, isometric educational puzzle game asset, bright classroom-lab style, clean toy-like 2.5D render, orange body with pale yellow badge, soft overhead lighting, gentle contact shadow, crisp readable silhouette, orthographic isometric camera, transparent background, no text
```

## Consistency Checklist

Before accepting generated assets, check:

- The angle matches the isometric room.
- The asset has no unwanted background.
- The silhouette is readable at small size.
- The number or equation area is blank if text will be rendered dynamically.
- The colors fit the shared palette.
- The player is more visually prominent than NPCs.
- The asset does not introduce a different genre or mood.
- The room remains visually calm enough for moving gameplay.
