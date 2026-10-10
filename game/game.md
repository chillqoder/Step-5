# Low Poly Mario — Complete Game Creation Prompt

## Step 1 — Understand the game you are building

Build a complete, playable browser platformer called **Low Poly Mario**. It is a Mario game with a carefully designed Minecraft-inspired voxel appearance: red cap, blue overalls, mushroom enemies, turtle shells, question blocks, coins, green pipes, power-ups, pits, and a flagpole at the end of World 1–1.

Preserve Mario's recognizable identity, platforming rules, and left-to-right level progression. The Minecraft influence applies to the rendering, materials, terrain, vegetation, and construction of objects. The game is a side-scrolling platformer rendered in 3D, with gameplay restricted to a single horizontal plane.

Deliver the entire working game in `index.html`, including HTML, CSS, and JavaScript. Load Three.js from a pinned CDN URL. Generate models and pixel textures procedurally. The project must not require a build system, asset downloads, a server-side application, an account, or external image-generation services.

**Every player-facing word must be in English.** This includes the browser title, start screen, HUD, controls, notifications, power-up names, game-over message, victory screen, and buttons.

The result must feel like a small finished game. Prioritize readable art, responsive controls, reliable collisions, and a level that can actually be completed.

## Step 2 — Establish the visual direction

### Overall appearance

Use a cohesive voxel art direction rather than merely substituting cubes for every primitive:

- Build silhouettes from aligned cubes, rectangular blocks, and deliberate stepped forms.
- Use consistent proportions and a shared block scale across the environment.
- Keep the scene bright, warm, clean, and readable.
- Make Mario, enemies, collectibles, and hazards immediately distinguishable.
- Use crisp nearest-neighbor pixel textures with restrained variation.
- Use soft daylight and contact shadows to reveal the three-dimensional block forms.
- Keep surface shading subtle enough that the world does not become muddy.
- Avoid noisy random geometry, disconnected body parts, excessively bulky trees, glossy plastic materials, and large objects obscuring the player.

### Color palette

Use the following as the starting palette, adjusting values only to improve consistency and readability:

| Element | Color direction |
| --- | --- |
| Sky | Soft blue, approximately `#91C9E5` |
| Distant haze | Pale blue-gray |
| Grass | Fresh medium green |
| Foliage | Muted oak green, with lighter top and darker lower layers |
| Dirt | Warm medium brown |
| Bark | Darker warm brown with vertical pixel grain |
| Bricks | Terracotta or warm orange-brown |
| Question blocks | Golden orange |
| Used blocks | Desaturated brown |
| Pipes | Saturated green with darker rims |
| Coins | Bright gold |
| Mario's cap and shirt | Warm red, approximately `#E44735` |
| Mario's overalls | Rich medium blue |
| Skin | Warm peach |
| Gloves and emblem | Warm white |
| HUD background | Deep green-gray, approximately `#263B36` |
| Main UI text | Warm off-white, approximately `#FFF6D9` |

### Materials and textures

Generate small textures, such as 16 × 16 pixels, using canvas. Use `NearestFilter` to preserve their pixel structure. Make texture generation deterministic so restarting the course does not change the artwork.

Create separate textures for grass, dirt, bricks, bark, leaves, and stone. Dirt should have restrained mineral-like flecks. Bricks need readable mortar lines. Bark needs vertical grain. Leaves need grouped pixels that read as foliage, not white noise. Keep character faces, eyes, the cap emblem, question marks, and coin highlights clean and high contrast.

Do not stretch one tiny dirt texture across the entire visible length of the level. Use individual voxel surfaces, repeated UVs, or instancing with a consistent texture scale.

## Step 3 — Build recognizable voxel Mario

Mario must be recognizable at normal gameplay scale without relying on a text label.

Construct him from deliberate block shapes:

1. A compact peach-colored head with squared cheeks.
2. A red cap with a clearly visible projecting brim.
3. A white patch on the cap containing a red pixel-built `M`.
4. Visible white eyes with dark pupils and short dark eyebrows.
5. A projecting block nose.
6. A dark mustache with a stepped lower edge.
7. Dark sideburns and hair at the back of the head.
8. A red shirt.
9. Blue overalls with two shoulder straps and small gold buttons.
10. White gloves.
11. Two separate blue trouser legs and brown shoes.

Use shoulder and hip pivots for limbs. Animate the limbs around those pivots instead of rotating detached meshes around their centers. Keep the head, cap, and torso stable enough that the character remains readable while running.

Keep Mario's face visible in the side-scrolling camera. A slight turn toward the direction of movement is sufficient; do not hide his entire face whenever he walks left.

Provide three power states:

- **Small:** approximately 1.6 world units tall, with a compact collision body.
- **Big:** approximately 2.5 world units tall, with a matching taller collision body and persistent larger visual scale.
- **Fire:** the larger body plus a clearly different red-and-white costume and the ability to fire projectiles.

Running, jumping, stomping, damage, and death animations must preserve the underlying power-state scale. An animation must never silently turn Big Mario back into Small Mario.

## Step 4 — Build a clean voxel environment

### Terrain

Use grass-topped dirt islands with straight, readable collision edges. Individual top tiles should reveal a block grid. The front face should show dirt layers. Exposed ends of islands must make pits visually obvious.

Keep the playable ground at `y = 0`. Extend visible dirt several units downward. Do not bridge pits with hidden collision boxes or background geometry.

### Trees — treat these as designed assets

Trees are a visual priority. Build compact stylized oak trees with:

- A straight, narrow, square trunk.
- A distinct bark texture with vertical grain.
- Optional short branches that support the crown.
- A broad lower foliage layer with stepped or missing corners.
- A slightly smaller or more compact middle layer.
- A small upper crown.
- Darker leaves underneath and lighter leaves on top.
- Foliage cubes aligned to a shared grid.

Give each tree one coherent canopy silhouette. Do not assemble trees from arbitrary overlapping lumps, giant scaled cubes, spheres, or cones. Use a small family of controlled variants, with modest changes in height and crown width.

Place trees behind the gameplay lane, roughly at `z = -7` to `z = -14`. Keep trunks and crowns away from Mario's silhouette, question blocks, jump landing areas, and coin trails. Trees are scenery, not gameplay collision objects.

### Bushes, hills, and clouds

- Bushes: small stepped clusters, positioned behind the player lane.
- Hills: broad, layered voxel terraces with a recognizable stepped profile.
- Distant ridges: lower contrast and increasingly softened by atmospheric haze.
- Clouds: clean white stepped slabs, assembled from a few deliberate rectangular forms.

Use a seeded arrangement of scenery. Favor composition and open space over filling every available location.

### Pipes

Make pipes square in cross-section while preserving the familiar Mario pipe silhouette. Each pipe needs a green body, a wider dark-green rim, a recessed dark opening, and a restrained highlight on one side.

The body and rim must both have world-space collision bounds. Mario must be able to land on the top. Pipes are obstacles in this first level; pipe entry and underground travel are outside its required scope.

## Step 5 — Use a readable side-scrolling camera

Use an orthographic 3D camera with a small downward angle. Show the top and front surfaces of blocks while keeping jump distances easy to judge.

- Gameplay movement uses `x` and `y`; keep the player at `z = 0`.
- Follow Mario horizontally with a small forward offset, approximately 3.5 units.
- Smooth camera movement using a delta-time-aware exponential interpolation.
- Use a vertical view span of approximately 17 units on a landscape display.
- Follow high jumps gently without shaking during normal running or every enemy bounce.
- Initialize the camera at the player before rendering the first frame.
- Update orthographic bounds correctly when the viewport changes.
- Keep the HUD and overlays readable at narrow widths.

The level should remain navigable in a narrow viewport, although keyboard input remains the required control method.

## Step 6 — Implement input and movement

### Controls

| Input | Action |
| --- | --- |
| `A` or `Left Arrow` | Move left |
| `D` or `Right Arrow` | Move right |
| `W` or `Up Arrow` | Jump in every power state |
| `Space` while Small or Big | Jump |
| `Space` while Fire | Throw a fireball |
| `X` while Fire | Throw a fireball |
| `R` | Restart the complete run |
| `Enter` on start, game-over, or victory screen | Start a new run |

The Space behavior changes when Mario gains a fire flower. Explain this clearly in the short control text. `W` and `Up Arrow` must always remain available for jumping.

Prevent arrow keys and Space from scrolling the browser. Trigger jumps and fireballs on fresh key presses rather than unrestricted browser key repeats. Clear held inputs when the window loses focus and when restarting.

### Movement targets

Use these values as a coherent starting configuration:

| Parameter | Value |
| --- | --- |
| Maximum horizontal speed | 8.5 units per second |
| Ground acceleration | 60 units per second squared |
| Air acceleration | 55 units per second squared |
| Ground deceleration | 65 units per second squared |
| Air deceleration | 30 units per second squared |
| Gravity | −40 units per second squared |
| Jump launch speed | 17 units per second |
| Maximum falling speed | Approximately 34 units per second |
| Coyote time | Approximately 100 milliseconds |
| Jump buffer | Approximately 120 milliseconds |

Accelerate toward the target velocity without overshooting it. Releasing a movement key should decelerate smoothly. Allow useful air steering. Keep left/right behavior symmetrical.

Implement variable-height jumping: releasing the active jump key early should reduce upward velocity and produce a shorter jump. A held jump should reach roughly 3.5 units above its launch surface with these parameters.

Coyote time permits a jump shortly after leaving a ledge. Jump buffering stores a press just before landing and triggers it once a legal jump becomes available. Neither feature may permit repeated mid-air jumps.

## Step 7 — Make physics and collisions dependable

Use a fixed simulation step, ideally `1 / 120` second, with an accumulator. Render separately using `requestAnimationFrame`. Cap accumulated elapsed time after long frame stalls.

Use explicit axis-aligned gameplay bounds independently of decorative meshes. Maintain world-space collision bounds for every solid. Keep the player's position defined consistently, preferably at the center of the feet.

Resolve horizontal and vertical motion consistently:

1. Record the previous feet position for swept landing and stomp checks.
2. Apply input, jump logic, acceleration, and gravity.
3. Move horizontally and resolve side collisions.
4. Move vertically and resolve floors, platforms, and ceilings.
5. Determine whether Mario is grounded.
6. Resolve enemy interactions based on the previous and current positions.
7. Update power-ups, coins, projectiles, and level progression.
8. Update visual transforms from the authoritative simulation state.

Required behavior:

- Mario rests exactly on solid surfaces.
- He cannot pass through pipes, blocks, or terrain edges.
- He can jump onto pipes and floating platforms.
- A head impact cancels upward velocity.
- Standing on a platform must not count as hitting its side.
- A broken brick immediately stops participating in collisions.
- Collision checks use actual world transforms, including objects inside groups.
- Walking off an island causes a real fall.
- Falling below approximately `y = -6` starts the death sequence.
- Enlarging Mario under a low ceiling must not embed him in geometry; defer growth until sufficient space is available.
- Cosmetic block bumps must not unpredictably move their collision boxes.

## Step 8 — Implement coins and interactive blocks

### Coins

Create bright gold voxel coins with a readable flat face and slight thickness. Rotate and gently bob world coins. Collect them when Mario's body overlaps their pickup volume.

A collected coin must disappear immediately and award its value exactly once. Award one extra life for every 100 coins, with a distinct sound and visible HUD update.

### Question blocks

Question blocks are golden cubes with a clean white pixel-built `?`. Make the glyph legible from the gameplay camera.

When hit from below:

1. Cancel Mario's upward movement.
2. Bump the block briefly.
3. Mark it as used immediately.
4. Remove the question glyph.
5. Change the block to the used-block material.
6. Release exactly one coin, mushroom, or flower according to its contents.

Used blocks remain solid but never award another item. A coin emerging from a block should visibly pop upward and then disappear. Dispose of the correct mesh or group resources when removing the effect.

### Bricks

Small Mario bumps bricks without destroying them. Big Mario and Fire Mario break bricks from below. A destroyed brick disappears, awards its score, and stops blocking movement, mushrooms, or fireballs.

Add a brief, restrained visual response for a bump or break. Keep feedback secondary to collision clarity.

## Step 9 — Implement mushrooms and fire flowers

### Super mushroom

Use a red block-built cap with white patches and a cream stem. Give it a compact recognizable mushroom silhouette.

The mushroom emerges above its question block, then moves horizontally at approximately 2.6 units per second. It obeys gravity, lands on terrain and platforms, and reverses direction when it reaches a wall or pipe. Remove it if it falls out of the level.

Collecting it changes Small Mario to Big Mario. Big and Fire Mario must not be downgraded by collecting another mushroom. Award points on collection.

### Fire flower

Build the flower from a warm center, contrasting block petals, a green stem, and a small leaf. It emerges from its block and remains in place, with restrained bobbing or rotation.

Collecting a flower grants the Fire state, including the larger body, a visible costume change, and projectile attacks. Show a brief English hint explaining that Space now shoots and W or Up Arrow still jumps.

### Damage while powered up

When Big or Fire Mario touches a dangerous enemy from the side, shrink him to Small Mario instead of immediately killing him. Grant approximately 2.2 seconds of temporary invulnerability. Use controlled blinking to communicate the protection period.

Pits and the timer remain lethal in every power state.

## Step 10 — Implement Goombas

Create recognizable brown mushroom enemies with a broad stepped head, a narrower lower body, cream or brown feet, white eyes, dark pupils, and angry eyebrows.

Goombas:

- Patrol horizontally at approximately 3 units per second.
- Have an initial direction determined consistently for a restart.
- Reverse direction at obstacles and patrol boundaries.
- Obey gravity and land on valid platforms.
- Can fall into pits and disappear below the death boundary.
- Damage Mario through side contact.
- Are defeated when Mario descends onto their top.

For a valid stomp, compare Mario's previous feet position with the enemy's top. Do not treat any descending side overlap as a stomp. On a stomp, flatten the Goomba briefly, award points, play a short sound, and bounce Mario upward with approximately 62% of normal jump launch speed.

Dead or squashed enemies must immediately stop damaging the player and must not award points multiple times.

## Step 11 — Implement Koopas and shell interactions

Create block-built turtle enemies with a green shell, pale shell rim, yellow head and body, visible eyes, feet, and a small tail.

Use explicit states:

1. **Walking:** patrols at approximately 2.4 units per second and damages Mario through side contact.
2. **Stationary shell:** entered when stomped; head and feet retract; horizontal speed becomes zero.
3. **Sliding shell:** entered when a stationary shell is kicked from the side or stomped again; travels at approximately 11 units per second.
4. **Defeated:** no further gameplay collisions or score awards.

A stationary shell should wake after approximately six seconds if it has not been kicked. A sliding shell reverses at walls and pipes, defeats enemies it strikes, and can damage Mario.

Stomping a sliding shell stops it and bounces Mario. Kicking a shell grants approximately 0.2 seconds of protection from that same shell, preventing an immediate unavoidable hit. This protection must not make unrelated enemies harmless.

Keep transitions and visual poses synchronized. A waking Koopa must show its head and feet again. Shell kills must award points and can participate in a score combo.

## Step 12 — Implement fireballs

Fire Mario can shoot small warm-orange block projectiles in the direction he faces.

- Start a projectile just outside Mario's body.
- Use a horizontal speed of approximately 13 units per second.
- Limit firing with a cooldown of approximately 0.22 seconds.
- Allow at most three active fireballs at once.
- Apply gravity at approximately 80% of Mario's gravity.
- Bounce from floors with an upward speed of approximately 9.5 units per second.
- Remove a projectile when it hits a wall, falls out of bounds, or defeats an enemy.
- Permit hits on Goombas and Koopas, including their shell states.
- Award points once per defeated enemy.

Projectiles must not pass through solid blocks or continue defeating several enemies after being removed. Use a small emissive center without overwhelming bloom or large particles.

## Step 13 — Build World 1–1 from an explicit layout

### Coordinate conventions

Use `x` for progression, `y` for height, and `z` for depth. Mario, coins, enemies, blocks, and pipes use the gameplay lane at `z = 0`. Scenery sits behind that lane.

One ordinary block is approximately `1 × 1 × 1` world unit. In the block tables below, the height is the **bottom** of the block, not its center.

Use a level range of approximately `x = -60` to `x = 96`. Start Mario at `x = -56`, just above the ground. Place the flagpole at `x = 86` and a small castle around `x = 90`.

### Ground and pits

Create these solid ground intervals:

| Segment | From x | To x |
| --- | ---: | ---: |
| 1 | −60 | −40 |
| 2 | −36 | −20 |
| 3 | −20 | −4 |
| 4 | 0 | 14 |
| 5 | 18 | 30 |
| 6 | 30 | 44 |
| 7 | 48 | 70 |
| 8 | 74 | 96 |

Create open pits at `[-40, -36]`, `[-4, 0]`, `[14, 18]`, `[44, 48]`, and `[70, 74]`. Adjacent solid segments may share an edge. Do not accidentally create a collision wall at a shared boundary.

### Intended progression

1. **Opening, x −60 to −40:** a safe runway for learning movement and jumping. Introduce the first gap with a coin over it. Keep scenery away from the landing.
2. **First landing, x −36 to −20:** a pipe, Goombas, the first question-block row, a mushroom, and the first walking Koopa. Teach jumping over obstacles and hitting blocks.
3. **Platforms, x −20 to −4:** a low brick platform, more question blocks, enemies, and an optional upper route. Reward controlled jumps.
4. **Fire introduction, x 0 to 14:** a taller pipe, mixed enemies, the first fire flower, a high block route, and a staircase toward the next gap.
5. **Middle section, x 18 to 44:** alternate lower coin blocks, pipes, enemy encounters, and further power-ups. Provide safe recovery space between challenges.
6. **Long final approach, x 48 to 70:** a short staircase, mixed Goombas and Koopas, a flower block, and an optional elevated coin route.
7. **Finish, x 74 to 96:** one final cluster of enemies, a mushroom, a final staircase, a clearly visible flagpole, and the castle beyond it.

Build staircases with filled supporting columns rather than unexplained floating individual steps. Keep the required ground route completable without collecting an optional power-up. Upper routes must have a reachable launch surface. Verify all mandatory jumps using the actual physics parameters.

### Floating block blueprint

Use a standard lower row at `y = 4.4` and an upper row at `y = 7.4`.

| x positions | Bottom y | Type and contents |
| --- | ---: | --- |
| −27 | 4.4 | Brick |
| −26 | 4.4 | Question block: mushroom |
| −25 | 4.4 | Brick |
| −24 | 4.4 | Question block: coin |
| −23, −22, −21 | 4.4 | Bricks |
| −13, −12, −11 | 3.2 | Low brick platform |
| −8 | 4.4 | Brick |
| −7, −6 | 4.4 | Question blocks: coins |
| −5 | 4.4 | Brick |
| −2, −1 | 7.4 | Bricks on the optional upper route |
| 2 | 7.4 | Brick |
| 3 | 7.4 | Question block: coin |
| 4 | 7.4 | Brick |
| 9 | 4.4 | Question block: flower |
| 10 | 4.4 | Brick |
| 11 | 4.4 | Question block: coin |
| 21 | 4.4 | Question block: coin |
| 22 | 4.4 | Brick |
| 23 | 4.4 | Question block: coin |
| 36 | 4.4 | Question block: mushroom |
| 38, 39 | 4.4 | Bricks |
| 40 | 4.4 | Question block: flower |
| 41 | 7.4 | Brick |
| 42 | 7.4 | Question block: coin |
| 60 | 4.4 | Brick |
| 61 | 4.4 | Question block: flower |
| 62 | 4.4 | Brick |
| 63 | 4.4 | Question block: coin |
| 64 | 7.4 | Brick |
| 80 | 4.4 | Question block: mushroom |
| 81 | 4.4 | Brick |
| 82 | 4.4 | Question block: coin |

Add supported steps near `x = 10–13`, `x = 56–57`, and `x = 82–84`. Target increasing top heights of approximately one, two, and three units. Ensure the staircase does not conceal an enemy or trap Big Mario beneath the overhead row.

### Pipes

| Center x | Height |
| ---: | ---: |
| −34 | 2.0 |
| 4 | 2.4 |
| 27 | 2.0 |
| 42 | 2.4 |

Use a body width of approximately 1.6 units and a rim width of approximately 2 units.

### Coin placement

Place collectible world coins at these positions. Each pair is `(x, y)`:

- First gap: `(-38, 2.6)`.
- First block row: x `−27` through `−21`, at y `6.0`.
- Low platform: x `−13`, `−12`, `−11`, at y `4.7`.
- Next question row: x `−7`, `−6`, `−5`, at y `6.0`.
- Second gap: `(-2, 2.6)`.
- First flower row: `(9, 6.0)`, `(10, 6.0)`, `(11, 6.0)`.
- Third gap: `(15, 3.0)`, `(16, 3.4)`, `(17, 3.0)`.
- Middle row: `(22, 6.0)`, `(23, 6.0)`.
- Later power-up rows: `(36, 6.0)`, `(40, 6.0)`.
- Fourth gap: `(46, 3.0)`, `(47.5, 3.2)`.
- Final long-segment row: `(61, 6.0)`, `(62, 6.0)`, `(63, 6.0)`.
- Fifth gap: `(71, 3.2)`, `(72.5, 3.4)`.
- Final approach: `(78, 2.8)`, `(81, 6.0)`.

Coin trails should suggest useful jump arcs. They must not falsely promise a safe platform where there is none.

### Goomba population

Create 14 Goombas with the following initial positions and patrol ranges:

| Initial x | Patrol minimum x | Patrol maximum x | Feet y |
| ---: | ---: | ---: | ---: |
| −28 | −33 | −25 | 0 |
| −34 | −36.5 | −31 | 2.0 |
| −16 | −19 | −12 | 0 |
| −10 | −13 | −6 | 0 |
| 2 | 1 | 8 | 0 |
| 6 | 4 | 13 | 0 |
| 20 | 18 | 25 | 0 |
| 25 | 22 | 29 | 0 |
| 34 | 31 | 40 | 0 |
| 40 | 36 | 43 | 0 |
| 52 | 48 | 55 | 0 |
| 58 | 55 | 62 | 0 |
| 64 | 60 | 69 | 0 |
| 78 | 74 | 80 | 0 |

The Goomba at `x = -34` begins on the first pipe. Its height must match the actual pipe top. If it walks off the top, apply gravity normally.

### Koopa population

Create eight Koopas:

| Initial x | Patrol minimum x | Patrol maximum x | Feet y |
| ---: | ---: | ---: | ---: |
| −24 | −27 | −21 | 0 |
| 10 | 6 | 13 | 0 |
| 4 | 3.2 | 4.8 | 2.4 |
| 26.5 | 23 | 29 | 0 |
| 43 | 37 | 43 | 0 |
| 50 | 48 | 55.5 | 0 |
| 65 | 62 | 69 | 0 |
| 81 | 74 | 80.5 | 0 |

The Koopa at `x = 4` begins on the taller pipe. Resolve any initial overlap between an enemy and a solid rather than leaving the enemy trapped inside the pipe or a stair block. Clamp initial positions to valid patrol bounds where necessary.

## Step 14 — Implement score, time, lives, and checkpoints

### HUD

Use a restrained compact HUD with these English labels:

- `Score`: at least six digits, initially `000000`.
- `Coins`: initially `× 00`.
- `World`: `1-1`.
- `Time`: initially `300`, shown as whole seconds.
- `Lives`: initially `× 3`.
- `Power`: `Small`, `Big`, or `Fire`; a dash is acceptable for Small if the interface makes the meaning clear.

Keep the HUD legible over the sky and terrain. Use a dark translucent strip or restrained dark backing, with warm white and gold text. Avoid oversized cards, excessive borders, and visual clutter.

### Scoring rules

Use a clear consistent scoring table:

| Event | Points |
| --- | ---: |
| Collect a world coin | 100 |
| Receive a coin from a question block | 200 |
| Collect a power-up | 1,000 |
| Break a brick | 50 |
| First enemy defeat in a combo | 100 |
| Second combo defeat | 200 |
| Third combo defeat | 400 |
| Fourth combo defeat | 800 |
| Fifth and subsequent combo defeats | 1,000 |

A combo expires after approximately 1.6 seconds without another eligible enemy defeat. Shell and fireball defeats should award points. Do not accidentally double-score the same event through multiple collision handlers. If a block-release bonus is included, make it an explicit separate rule and keep the HUD consistent with it.

### Timer and lives

The timer decreases only during active play. It must not run on the start screen or continue after the course has been cleared. Reaching zero starts a death exactly once.

Start with exactly three lives. Deduct one life once per death. The third death ends a three-life run unless additional lives have been earned. Do not require a fourth death to show Game Over.

### Checkpoints

Use checkpoint boundaries near `x = -56`, `-20`, `0`, `30`, and `48`. Advance checkpoints only when the player reaches them. Choose a safe respawn point on the solid ground just beyond the checkpoint.

After a non-final death:

- Keep the score, coin total, and current level progression.
- Respawn Mario at the most recently reached checkpoint.
- Restore the Small state.
- Reset velocity, jump buffering, held inputs, and transient animation state.
- Grant a short invulnerability period.
- Preserve the remaining timer, with at least 60 seconds available after a timeout-related respawn.

A full restart rebuilds the course and resets score, coins, lives, time, checkpoints, enemies, blocks, collectibles, projectiles, power state, input state, and camera position.

## Step 15 — Implement death and course completion

### Death

Small Mario dies after harmful enemy contact. Every power state dies after falling into a pit or timing out.

Show a brief upward death hop followed by a fall. Update the actual rendered position during this animation. Disable normal gameplay input and enemy damage processing until respawn or Game Over. Deduct the life once at the end of the death sequence.

### Finish

Place a tall narrow block-built flagpole at `x = 86`, with a gold top marker and red flag. Put a compact brick castle beyond it.

Trigger course completion when Mario reaches the pole's valid interaction region, not merely because an arbitrary world x threshold is crossed while falling below the level. Stop the active timer and disable further damage. If Mario reaches the pole in the air, settle him toward the ground in a short controlled finish animation.

After the finish animation, show `Course Clear!`, the final score and coin total, and a `Play Again` button. Restart must create a fresh complete run.

## Step 16 — Keep the start screen simple

The final start screen must be minimal, similar to a classic small browser game overlay. It must not use the earlier large portrait-based welcome composition.

Use:

- Title: `Low Poly Mario`.
- One short objective line: `Collect coins. Stomp enemies. Reach the flag.`
- One main button: `Play`.
- A small control summary.
- Optional secondary hint: `Press Enter to start`.

Suggested control text:

`A / D or ← / → to move · W / ↑ to jump`

`Space to jump or shoot with a fire flower · X to shoot · R to restart`

Use the rendered world as the backdrop, with a restrained dark translucent overlay. Keep text compact and centered. Avoid a giant Mario face, illustration panel, separate How to Play button, multi-column instruction cards, long welcome prose, marketing copy, and joke-filled descriptions.

The game-over screen should use `Game Over`, a short message such as `Out of lives. One more run?`, and `Try Again`. The victory screen should use `Course Clear!` and `Play Again`.

## Step 17 — Add concise sound and visual feedback

Generate simple sounds with the Web Audio API. Initialize or resume audio after a user interaction. The game must still work if audio is unavailable.

Include distinct short sounds for jumping, collecting a coin, bumping a block, stomping, collecting a power-up, throwing a fireball, losing a life, earning an extra life, and clearing the course.

Use short English notifications for important state changes, such as `Super Size!` and `Flower Power! W / ↑ to jump · Space / X to shoot`. Keep notifications out of Mario's immediate action area and remove them after a short delay.

Keep animation feedback controlled. Limit particles, screen shake, flashing, and visual effects that obscure the playfield.

## Step 18 — Organize the single-file implementation

Within `index.html`, keep the code organized into understandable sections:

1. Configuration and rendering setup.
2. Procedural textures and shared materials.
3. Voxel model factories.
4. Explicit level data and scenery generation.
5. Input state and player movement.
6. Collision helpers.
7. Enemy state machines.
8. Block interactions and power-ups.
9. Coins, fireballs, scoring, and sound.
10. Game states, restart, respawn, and finish logic.
11. Fixed-step simulation and rendering loop.
12. DOM event bindings.

Use explicit game states such as `title`, `play`, `dying`, `gameover`, and `win`. Keep state transitions centralized and prevent gameplay updates from running in the wrong state.

Reuse materials. Instance repeated terrain tiles and similar background geometry where useful. Cap device pixel ratio, for example at 2. Keep shadow quality reasonable. Remove obsolete objects and dispose of their geometry without destroying materials or textures still in use.

Rebuilding the level must not accumulate old scenery references, enemy arrays, event listeners, animation effects, or orphaned projectiles. Maintain deterministic scenery and stable gameplay across repeated restarts.

## Step 19 — Verify gameplay before calling it complete

Check the actual simulation, not only JavaScript syntax. Use focused automated checks where possible and perform a browser playthrough for rendering, input, sound, and visual quality.

Verify all of the following:

1. The title screen uses the final minimal design and English copy.
2. Play and Enter start a fresh run.
3. The player lands on the initial ground without jitter.
4. Running reaches the configured speed without overshoot.
5. Releasing movement stops Mario predictably.
6. Left and right movement behave symmetrically.
7. Full jumps and short jumps have visibly different heights.
8. Coyote time permits one late ledge jump.
9. Buffered jumping activates after landing without allowing double jumps.
10. Mario lands on pipes and floating blocks.
11. Pipe collisions use the correct world positions.
12. Question blocks can be activated only once.
13. Coin-pop effects appear and clean up without errors.
14. Small Mario bumps bricks; powered-up Mario breaks them.
15. Broken bricks stop participating in every relevant collision system.
16. Mushrooms emerge, move, reverse at walls, fall, and can be collected.
17. Growth under a ceiling does not trap or teleport Mario.
18. Big Mario stays visibly big through running, jumping, and stomping.
19. Fire Mario can jump with W or Up Arrow and shoot with Space or X.
20. Fireball cooldown, maximum count, bouncing, and removal work.
21. Fireballs defeat enemies and award points once.
22. Descending top contact stomps a Goomba; side contact causes damage.
23. Koopa walking, shell, sliding, stopping, and waking states work.
24. Kicking a shell cannot immediately damage the kicker.
25. Sliding shells defeat other enemies, bounce off walls, and remain dangerous afterward.
26. Powered-up damage shrinks Mario and starts temporary invulnerability.
27. Pits are real gaps with no hidden support.
28. All mandatory gaps and obstacles are traversable with Small Mario.
29. Every enemy starts in a valid non-penetrating position.
30. Exactly three ordinary deaths end an initial three-life run.
31. Checkpoint respawns are safe and do not cause immediate repeated death.
32. A timeout loses one life and permits a viable respawn.
33. The timer pauses on title, game-over, and completed-course screens.
34. Reaching the flag completes the course exactly once.
35. Full restart clears all run-specific state and does not accumulate objects.
36. Window resizing updates the camera and renderer correctly.
37. Input does not remain stuck after losing focus.
38. Trees have intentional stepped crowns and never obstruct essential gameplay.
39. Mario's face, cap, eyes, mustache, and outfit remain recognizable during play.
40. The game remains stable at different rendering frame rates and produces no uncaught runtime errors.

## Step 20 — Deliver the finished result

Deliver a working `index.html` implementing this specification. Keep this document as the complete English creation prompt and design reference.

The final result should combine Mario's familiar movement and interactions with a consistent, carefully composed voxel world: clean terrain, well-shaped trees, readable characters, crisp materials, compact English UI, and a first level that feels reliable from the opening runway to the flagpole.
