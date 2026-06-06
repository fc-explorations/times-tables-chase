# Times Table Chase

Times Table Chase is an educational isometric chase game for practicing multiplication facts.

The player controls a character carrying a visible number. Each NPC also carries a number. The goal is to reach the exit while carrying the number that solves the multiplication equation shown on the exit sign.

[LINK to the game](https://fc-explorations.github.io/times-tables-chase/)

## Game Idea

Each room has an exit with a partial multiplication equation:

```text
A x ? = C
A x B = ?
```

The missing value is the number the player must carry when leaving the room.

For example:

```text
3 x ? = 18
```

The player needs to exit while carrying `6`.

Or:

```text
4 x 7 = ?
```

The player needs to exit while carrying `28`.

The player cannot type the answer. Instead, they must move through the room and swap numbers with other characters.

## Core Mechanic

When two characters touch, they exchange the numbers they are carrying.

This means the player has to:

- Read the equation.
- Work out the missing number.
- Find the character carrying that number.
- Chase or intercept them.
- Swap numbers.
- Reach the exit with the correct number.

The game turns multiplication practice into a movement puzzle rather than a static quiz.

## NPC Behavior

NPCs usually wander around the room.

Each NPC has an awareness radius. When another character enters that radius, the NPC reacts to the closest character inside it.

NPCs alternate between two modes:

- Attraction: move toward the closest character.
- Repulsion: move away from the closest character.

Each NPC has its own timing pattern, so their behavior changes at different moments. This creates a room where numbers are constantly moving, clustering, escaping, and swapping.

## Learning Goal

The game reinforces multiplication facts by making the answer useful inside a live puzzle.

The player repeatedly practices:

- Calculating products.
- Finding missing factors.
- Recognizing multiplication table values.
- Making quick decisions under light pressure.

Settings can focus the game on specific multiplication tables, such as only the `2` and `3` tables.

## Progression

The game starts from the selected settings and becomes harder automatically.

As levels increase:

- More NPCs are added.
- NPC awareness increases.
- Equations continue to change.
- The room becomes more chaotic and tactical.

The run is endless. Solving a room automatically starts the next one.

## Settings

The game includes settings for:

- Starting number of NPCs.
- Multiplication table values used for `B`.
- Starting awareness radius.
- Room size.
- Number of random wall obstacles.
- Overall speed.
- Player speed relative to NPC speed.
- Player movement mode: room axes or screen directions.
- Whether awareness rings are visible.

If the player is slower than the NPCs, the game becomes harder and awards more points.

## Scoring

The player earns points each time they solve a room.

Higher scores come from harder conditions:

- Higher level.
- More multiplication tables enabled.
- Larger multiplication values.
- More NPCs.
- More walls.
- Larger awareness radius.
- Higher overall speed.
- Slower player speed relative to NPC speed.

Scores are saved locally in the browser leaderboard.

## Controls

Use keyboard movement:

- Arrow keys.
- WASD.

The game is designed as a simple static browser game that can run from GitHub Pages.
