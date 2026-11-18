# Iso Combat (prototype v6)

Brutal and silly real-time isometric arena combat. It is one self-contained file, `index.html` (about 0.8 MB), and works offline.
It is built from `src/` with `npm run build` (esbuild bundles three.js r170 together with the embedded Bungee font).

## Play
https://gustavgrenas.github.io/iso-combat/ — works offline too: `index.html` is the whole game.

## What's new in v6

- **Map select** at the start: a card with a live thumbnail for every map (arrow keys / A-D to browse, Enter/Space or a click to start). The last map you picked is remembered.
- **Roomier maps**: every island has about 1.5x the area, with open arenas to fight in.
- **Four new enemies**:
  - **Rullstolskrigaren** (boss): an armoured wheelchair with a machine gun on the armrest. He drifts, rams you and fires MG bursts.
  - **Bikungen**: a beekeeper who releases a bee swarm. The flamethrower burns the bees.
  - **Kanonkocken**: lobs soup pots. A marker shows where each one lands, and it leaves a slippery, scalding soup puddle.
  - **Gravgrävaren**: digs down and moves as a mound. He bursts up under you and leaves an open grave behind.
- **Interactive elements** (one reusable system):
  - lifts and moving platforms
  - levers
  - conveyors and a log flume
  - bounce and launch pads
  - the big saw, a lava sluice, a log trap, eruptions, and collapsing timber stacks
- **Interactives on the older maps**: Gläntön, Tallholmen, Ruinskäret, Klyftan and Krutkällan each get a lift up onto a plateau, plus a ring-out conveyor with a lever that reverses it.
- **New map: Vulkanön**
  - Lava kills instantly, enemies included. Lava rivers pour off the island's edge.
  - Eruptions throw rocks, with a landing marker for each.
  - A lift up to the crater rim.
  - A lever that opens a lava sluice right under the enemies.
  - Pads on basalt pillars, floating platforms out to the islets, and plank bridges that break.
- **New map: Sågverket**
  - The big saw is deadly; a lever turns it on and off.
  - A feed belt runs into the saw, and the yard belt has a lever that reverses it.
  - A log flume carries logs to the river.
  - Up in the loft, a lever sends rolling logs down.
  - Timber stacks collapse into rolling logs when you blow them up.
  - A log hoist, a water wheel, the river and its bridges, and a hay pad that launches you up the watchtower.
- New kill tags for the hazards (lava, saw, logs, eruption).

## What's new in v5
- **Jump:** Space now jumps, and the dodge roll moved to **C** (or Ctrl). The jump is quick: a fast rise, a short hang and a quick fall, with a little air control. Holding Space gives a full jump of about 1.3 m, which clears a 1 m ledge or a 1.5 m gap. A tap gives a short hop. A shadow under the hero shows where you will land. Falling off the edge still kills. The jump state is part of the sim state and is saved in snapshots.
- **More varied maps:** six maps now rotate between runs (pick one with `?map=0…5`). Each has its own hazard, shown when the run starts:
  - **Gläntön:** powder kegs and a rope bridge.
  - **Tallholmen:** dense forest and jump pads.
  - **Ruinskäret:** ruin floors that crack and fall away.
  - **Klyftan (new):** chasms with fragile bridges.
  - **Krutkällan (new):** powder kegs everywhere.
  - **Ölandsgården (new):** an Öland farmyard with a sea all around (see below).
  - New features on the maps:
    - Chasms with plank bridges. Brutes and explosions can break the bridges.
    - Raised mesas that you can only reach by jumping. They hold bonus pickups.
    - Jump pads that throw you to a set spot.
    - Floors that crumble under your weight.
    - Powder kegs that you can shoot, set on fire or hit. They explode in chains.
    - Enemies come from several changing spawn zones, marked with a brief red ring.
    - Runners jump up ledges and across gaps after you. Stuck brutes smash through cover.
- **Ölandsgården:** an arena on Öland:
  - A white main house and a falu-red barn with an all-red garage door and a white loft window.
  - A pool house with a deck, and a pool you can fall into.
  - A stone-walled terrace, an in-ground **trampoline** that bounces you high, and an apple tree.
  - An open lawn, and a gravel drive.
  - Alvar limestone, dry-stone walls, junipers, a windmill with turning sails, and raukar along the shore.
  - The sea all around.
  - Buildings are cover and can be destroyed.
  - Walls between the camera and the hero are dithered away around the hero.
- **Organic low-poly world instead of cubes:**
  - Ruins, pillars, walls, boulders and raukar are built from hewn stones that are chamfered, have jittered corners and overlap their neighbours.
  - Plaster and board walls are seamless.
  - Roofs are real sloped gables.
  - Crates are 1 m plank crates with battens and braces.
  - Coastlines and terrace edges are rounded: the staircase corners are cut or filled.
  - Cliff faces bulge with depth, and the underside of each island is smooth and faceted.
  - Ledges, islets and limestone slabs are irregular shapes.
  - Destruction throws faceted stone shards and long wood splinters. Gore chunks are irregular too.
  - Everything is still instanced or merged, and debris is pooled.
- **Small death texts:** kill and death lines are now small tags that float above the body. A compact kill feed sits in the top-right corner. Wave banners are smaller and shorter.
- **New sound:** everything is still procedural WebAudio:
  - Sounds are layered: a transient, a tonal body, a noise tail and distortion.
  - Pitch and level vary on every shot, and sounds are panned in stereo by their screen position.
  - The chainsaw has an engine sound that idles and revs.
  - The minigun has a barrel whine that follows the spin-up and spin-down.
  - The flamethrower roars.
  - New sounds: footsteps, jump and landing, pad whoosh, trampoline boing, splashes, cracking floors and stone rubble.
  - A voice cap, a glue compressor, a soft clipper and a limiter keep the mix clean.
- **Tests:** `tests/v5_features.mjs` checks:
  - The jump: apex, short hop, air time, landing on a 1 m ledge, crossing a 1.5 m gap, and snapshot/restore mid-air.
  - Pads, the trampoline, keg chains and crumbling floors.
  - Bot runs on all six maps.
  - The v4 tests (muzzle, weapons, input, now with Space for jump and C/Ctrl for dodge) and sim determinism also pass.

## What's new in v4
- **Organic floating island:** a noise-generated outline with peninsulas, bays, narrow bridges and detached ledges instead of a square grid. It has raised terraces with ramps, blended grass, dirt and stone, sandy layered cliffs with small ledges, a stepped underside, and small floating islets. The palette is brighter, with a cyan sky.
- **Three maps** rotate between runs: Gläntön, Tallholmen and Ruinskäret. You can pick one with `?map=0|1|2`.
- **Pine forests:**
  - Pines are cover: they block bolts, bullets and movement.
  - They burn: the Eldkastare sets them alight, fire spreads and they burn down to charred stumps.
  - They can be felled: the chainsaw, axe or a brute's club fells them, and a falling pine crushes enemies.
  - Trees between the camera and the hero turn see-through.
- **Ruins and pillars:** sandstone ruins with vines.
- Pathfinding and edge falls follow the new terrain heights.
- **Muzzle-accurate shots:** every projectile, flame and tracer now leaves the real muzzle of the weapon the hero is holding, in every aim direction and while running. The sim and renderer share one weapon rig, and the arms are IK'd onto the weapon grips.
- **New characters:**
  - **Hero:** face, beard, helmet, armour and a cape on a spring. Every weapon is held properly in both hands.
  - **Springkåpa:** hooded runner with a dagger.
  - **Bjässe:** brute with a club and a belly.
  - **Bågkåpa:** archer with a bow and a quiver.
  - **Bombkåpa (new):** throws fused bombs.
- Characters have expressive yelling faces (when hurt, burning, falling or firing) and better animation. A gold ring marks the hero and red rings mark enemies, so teams are easy to tell apart.
- **Tests:**
  - A muzzle test checks 5 rigged weapons × 8 directions × standing/running, plus the archer. The worst error is 0.0005 units.
  - The weapon/tree tests pass on all three maps.
  - The input/UI tests, sim determinism and smoke tests also pass.

## What's new in v3
- **Look:** smooth stylised low-poly characters (rounded/bevelled shapes, rim light) instead of pure voxels. Characters have procedural animation: run cycle with lean, attack anticipation and follow-through, dodge roll, hit reactions and squash-and-stretch.
- **Environment:** textured ground with ambient occlusion, rounded blocks, and a floating island with dirt cliffs and an underside.
- **Game feel:** snappier movement, hitstop on heavy hits, capped trauma screen shake, sprite sparks/fire/smoke/dust, swing trails, and blood and scorch decals that fade out.
- **New weapons:**
  - **Eldkastare:** sets enemies on fire. Burning enemies run around screaming, spread the fire and burn to charred chunks. It also ignites crates and wooden beams. Uses fuel.
  - **Minigun:** spins up, then fires very fast, slows you down, pushes enemies off the edge and overheats. Spent casings fly. Uses bullets.
  - **Motorsåg:** hold to rev. It saws through enemies (they get cut in half) and through crates. Uses fuel.
- Fuel, bullets and gas drop from enemies and are topped up between waves.

## Controls (default: keyboard twin-stick)
| Key | Action |
|---|---|
| WASD | Run (screen-relative) |
| Arrow keys | Aim **and** attack in that direction while held (8 directions; two arrows = diagonal). Soft aim assist bends toward the nearest enemy within ~25°. Hold for the flamethrower, minigun and chainsaw. |
| Shift | Kick (knock enemies off the edge) |
| Space | Jump (hold for full height: up ledges and across gaps) |
| C / Ctrl | Dodge roll (also puts out fire on you) |
| Q/E or 1–7 | Switch weapon: Yxa · Armborst · Bomb · Fårkanon · Eldkastare · Minigun · Motorsåg |
| Esc / P · M | Pause · sound |

Mouse aiming is available as an option: **"Styrning: Tangentbord / Mus"** in the title or pause menu (saved in the browser). In mouse mode: mouse = aim, left click (hold) = attack, right click/Shift = kick, mouse wheel = switch weapon.

## Architecture (prepared for online play)
- `src/sim.js` is a pure fixed-timestep simulation at 60 Hz. It has no DOM and no three.js, and runs in Node.
  - Input commands are plain objects, one per player: `step(S, inputs[])`.
  - The game is deterministic, using a seeded RNG stored in the state.
  - `snapshot()` and `restore()` are available.
- The sim sends events to the client renderer. All cosmetic effects stay on the client: particles, decals, trails, hitstop, shake and sound.
- **Renderer:**
  - Per-bone instanced meshes, about 30–40 draw calls including the shadow pass, and about 200k triangles.
  - Merged static island.
  - One draw call each for sprite particles, decals and trails.
  - Flame particles are capped at 420.
  - Pixel ratio is capped at 2, with a 2048 shadow map.
  - Shaders are pre-compiled at start.

## Source
This public repo contains only the built game (`index.html`) and this README. The source (`src/`) and the headless test scripts live in the private working folder and are not published here.
