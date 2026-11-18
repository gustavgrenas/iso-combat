# Iso Combat (prototype v9)

Brutal and silly real-time isometric arena combat. It is one self-contained file, `index.html` (about 2.3 MB, most of it embedded sound samples), and works offline.
It is built from `src/` with `npm run build` (esbuild bundles three.js r170 together with the embedded Bungee font).

## Play
https://gustavgrenas.github.io/iso-combat/ — works offline too: `index.html` is the whole game.

## What's new in v9: real sound, health packs, auto-aim, distinct maps

### Sound
- The procedural synth sounds are replaced by **recorded and designed samples** (91 sounds, ~220 variations, all **CC0 / public domain**, see *Credits* below):
  weapons, flesh/bone/wood/stone impacts, gore, near and far explosions, footsteps on grass/stone/wood, jump/land, dodge, kick,
  fall scream, splash, lava, every enemy type (grunts, hurt and death yells, sheep, bees, chainsaw, wheelchair motor…), pickups and UI.
- Samples are mono Ogg Opus (40 kbps) embedded in `index.html` as base64 and decoded asynchronously after the page loads (~1–2 s, the title screen works meanwhile).
  If a sound fails to decode (e.g. Safari older than 18.4 / macOS 15.4 cannot decode Ogg Opus), that sound falls back to the old procedural synth.
- Mixer: random variation + pitch/volume per shot, distance attenuation, stereo pan and low-pass for far sounds, separate far-explosion samples,
  voice limiting (max 30 voices, per-sound caps and priority stealing), small sounds duck under explosions, a reverb send,
  bus compressor and a master limiter with a soft safety clipper (stress test: peak −3 dBFS, 0 clipped samples).
- Every map has its own **ambient sound bed** (birds, night insects, rain, winter wind, autumn wind, sea, lava, sawmill).
- **Volume sliders** (Master / Effekter) in the pause menu, saved in the browser. Gamepad in pause: D-pad ←→ master, LB/RB effects.

### Gameplay
- **Progress HUD**: a small bar under the wave counter shows enemies left in the current wave and how many waves until the next boss.
- **Health packs (Förbandslåda)**: appear periodically on safe open ground (never at the edge, in water or on hazards), with a green glow and a sound;
  they heal 35 % of max HP and vanish after 32 s (blinking for the last 4). About every 14 s on Lätt, 24 s on Normal, 34 s on Svår and 50 s on Brutal; at most 2 at a time.
- **Auto-aim toggle** ("Sikthjälp: På/Av") in the pause menu (gamepad: Y), saved in the browser. When on, aim softly bends toward the nearest *visible*
  enemy in a cone: strongest on gamepad, normal on keyboard and light on mouse (mouse only snaps when the cursor is already near the target).

### Maps
Each map now has its own time of day / weather, palette, lighting, sky colour, weather particles, ambient sound and enemy mix, and the generated islands have different shapes:

| Map | Mood | Weather | Shape | Enemy mix |
|---|---|---|---|---|
| Gläntön | summer day | pollen | round | standard |
| Tallholmen | dusk | fireflies | crescent | more archers |
| Ruinskäret | rain | rain | oval | more brutes |
| Klyftan | winter | snow | long spine | more runners and bombers |
| Krutkällan | autumn evening | falling leaves | six-armed star | many bombers |
| Ölandsgården | high summer | – | (hand-built) | standard |
| Vulkanön | eruption | ash | (hand-built) | standard |
| Sågverket | morning haze | sawdust | (hand-built) | standard |

Note: Normal is no longer identical to v7/v8 in waves on the four maps with a custom enemy mix, and health packs exist on all difficulties.

## What's new in v8: difficulty levels

Pick **Lätt, Normal, Svår or Brutal** on the start screen, under the map select:
↑ ↓ / W S on the keyboard, D-pad or left stick up/down on a gamepad (← → still changes the map), or click a card.
Your choice is remembered (localStorage) and can be forced with `?diff=latt|normal|svar|brutal` (or `0`–`3`).
The difficulty is shown in the HUD under the wave counter and on the game-over screen, and the
high score is kept **per map and difficulty**.

| | Lätt | Normal | Svår | Brutal |
|---|---|---|---|---|
| Hero HP | 150 | 100 | 100 | 100 |
| Damage taken | ×0.6 | ×1 | ×1.2 (×1.25 from enemies) | ×1.5 (×1.6 from enemies) |
| Enemy HP | ×0.75 | ×1 | ×1.25 | ×1.5 |
| Enemies per wave / ramp | ×0.7 / ×0.65 | ×1 / ×1 | ×1.25 / ×1.25 | ×1.5 / ×1.5 |
| Spawn interval | ×1.5 | ×1 | ×0.8 | ×0.6 |
| Enemy speed | ×0.88 | ×1 | ×1.08 | ×1.18 |
| Attack cooldown (aggression) | ×1.35 | ×1 | ×0.85 | ×0.7 |
| Telegraph / wind-up time | ×1.45 | ×1 | ×0.85 | ×0.7 |
| Ranged aim error (bows, MG, Kanonkocken) | ×2.4 | ×1 | ×0.6 | ×0.35 |
| Korv (health) drops | ×2.2 | ×1 | ×0.6 | ×0.3 |
| Ammo drops | ×1.7 | ×1 | ×0.75 | ×0.5 |
| Heal on wave clear | +40 | +20 | +10 | 0 |
| First boss (Rullstolskrigaren) | wave 7 | wave 5 | wave 4 | wave 3 |
| Extra elite brutes per wave | – | – | +1 | +2 |
| Aim assist cone | ×1.5 | ×1 | ×1 | ×0.85 |
| Falling off / lava | rescued, −25 HP (lava −40) | death | death | death |

Normal is exactly the v7 tuning. The difficulty is one data table (`src/difficulty.js`); the simulation only
stores its id, so it is part of the serialized state and stays deterministic.

## What's new in v7: gamepad support

Plug in a gamepad (or connect one over Bluetooth) and press any button. A small toast says **"Handkontroll ansluten"**. The keyboard and mouse keep working, and the on-screen prompts follow whichever device you used last.
Gamepads are read through the standard Gamepad API ("standard" mapping). Xbox, PlayStation (DualSense/DualShock) and Switch Pro controllers each get their own button labels.

| Action | Xbox | PlayStation | Switch Pro |
|---|---|---|---|
| Move | Left stick | Left stick | Left stick |
| Aim 360° (aim assist); push the stick all the way = fire | Right stick | Right stick | Right stick |
| Fire | RT | R2 | ZR |
| Jump (hold for a higher jump) | A | ✕ | B |
| Dodge roll | B | ○ | A |
| Kick | X | □ | Y |
| Previous / next weapon | LB / RB, D-pad ← → | L1 / R1, D-pad ← → | L / R, D-pad ← → |
| Pause | Menu | Options | + |
| Map select | Stick/D-pad to browse, A to start | Stick/D-pad, ✕ | Stick/D-pad, B |
| Back (pause → resume, game over → menu) | B | ○ | A |

The controller rumbles on your shots, on explosions nearby and when you are hit, where the browser supports it.

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

## Credits (sound)

All sound samples are released under **CC0 1.0 (public domain dedication)** — free to use and redistribute, no attribution required; credited here anyway with thanks.
Clips were cut, levelled, trimmed and re-encoded to Opus. Freesound clips come from the public high-quality previews of CC0 sounds; each licence was checked on the sound's page.

**Kenney** (www.kenney.nl, CC0 1.0):
- Digital Audio — https://kenney.nl/assets/digital-audio — CC0 1.0
- Impact Sounds — https://kenney.nl/assets/impact-sounds — CC0 1.0
- Interface Sounds — https://kenney.nl/assets/interface-sounds — CC0 1.0
- RPG Audio — https://kenney.nl/assets/rpg-audio — CC0 1.0
- Sci-fi Sounds — https://kenney.nl/assets/sci-fi-sounds — CC0 1.0

**Freesound** (freesound.org, CC0 1.0):
- "Flamethrower" by 1bob — https://freesound.org/people/1bob/sounds/831929/ — CC0 1.0
- "Basic Metal Clang Free" by AardsReal — https://freesound.org/people/AardsReal/sounds/842171/ — CC0 1.0
- "Rocks.wav" by adamgryu — https://freesound.org/people/adamgryu/sounds/336023/ — CC0 1.0
- "war horn.wav" by adharca — https://freesound.org/people/adharca/sounds/539956/ — CC0 1.0
- "Young Male Scream" by annatabernero — https://freesound.org/people/annatabernero/sounds/219654/ — CC0 1.0
- "Grunt 3" by aulix24 — https://freesound.org/people/aulix24/sounds/795468/ — CC0 1.0
- "Water Poured onto a Burning Fireplace – Hissing Steam and Sizzling Embers" by bassimat — https://freesound.org/people/bassimat/sounds/870180/ — CC0 1.0
- "MaleDeathSound12.wav" by Blankened — https://freesound.org/people/Blankened/sounds/577033/ — CC0 1.0
- "Man Screams His Death Into The Void" by BlockFrog — https://freesound.org/people/BlockFrog/sounds/752055/ — CC0 1.0
- "Denim_And_Cloth_Step_Foley_16" by BlondPanda — https://freesound.org/people/BlondPanda/sounds/778518/ — CC0 1.0
- "Spark.wav" by BMacZero — https://freesound.org/people/BMacZero/sounds/94132/ — CC0 1.0
- "Bone Crunch Fast.wav" by BrassKnucklesFilms — https://freesound.org/people/BrassKnucklesFilms/sounds/192146/ — CC0 1.0
- "Blast + Flamethrower cooldown" by Breviceps — https://freesound.org/people/Breviceps/sounds/447941/ — CC0 1.0
- "ugh (male)" by brookmorgan — https://freesound.org/people/brookmorgan/sounds/189565/ — CC0 1.0
- "Birds In Light Rain Ambience (Scotland)" by BurghRecords — https://freesound.org/people/BurghRecords/sounds/530898/ — CC0 1.0
- "Break off wood stick 4" by CAT-FOX_ALEX — https://freesound.org/people/CAT-FOX_ALEX/sounds/875588/ — CC0 1.0
- "Little Whoosh 2" by ch_ase — https://freesound.org/people/ch_ase/sounds/423799/ — CC0 1.0
- "R02-24-Airplane Tire Skid.wav" by craigsmith — https://freesound.org/people/craigsmith/sounds/479498/ — CC0 1.0
- "Clean fast Swoosh.aiff" by Danjocross — https://freesound.org/people/Danjocross/sounds/507466/ — CC0 1.0
- "Distant Explosion" by danlucaz — https://freesound.org/people/danlucaz/sounds/517747/ — CC0 1.0
- "Crickets At Night - Raw sound" by Defelozedd94 — https://freesound.org/people/Defelozedd94/sounds/522299/ — CC0 1.0
- "Ocean Waves" by derjuli — https://freesound.org/people/derjuli/sounds/693388/ — CC0 1.0
- "Agressive Bees" by dlp_coasters — https://freesound.org/people/dlp_coasters/sounds/462867/ — CC0 1.0
- "Crossbow Fire XI" by DUDE_X-SoundLab — https://freesound.org/people/DUDE_X-SoundLab/sounds/752211/ — CC0 1.0
- "Squish.ogg" by egomassive — https://freesound.org/people/egomassive/sounds/536765/ — CC0 1.0
- "Bow Release Hit" by EminYILDIRIM — https://freesound.org/people/EminYILDIRIM/sounds/536068/ — CC0 1.0
- "sheep 1" by esperar — https://freesound.org/people/esperar/sounds/171152/ — CC0 1.0
- "sheep 2.mp3" by esperar — https://freesound.org/people/esperar/sounds/171151/ — CC0 1.0
- "sheep 3.mp3" by esperar — https://freesound.org/people/esperar/sounds/171149/ — CC0 1.0
- "Digging in wet course sand (1)" by f3bbbo — https://freesound.org/people/f3bbbo/sounds/651292/ — CC0 1.0
- "Death Scream" by Feed_ — https://freesound.org/people/Feed_/sounds/523216/ — CC0 1.0
- "Lava Loop 2" by Fission9 — https://freesound.org/people/Fission9/sounds/474850/ — CC0 1.0
- "Crossbow Firing" by GameWithBepis — https://freesound.org/people/GameWithBepis/sounds/561475/ — CC0 1.0
- "Cutting down a tree, falling to the ground.wav" by hadescolossus — https://freesound.org/people/hadescolossus/sounds/704670/ — CC0 1.0
- "explosion2.wav" by IdkMrGarcia — https://freesound.org/people/IdkMrGarcia/sounds/446624/ — CC0 1.0
- "tree fall" by igroglaz — https://freesound.org/people/igroglaz/sounds/593875/ — CC0 1.0
- "Fire_Crackle_01.wav" by IPaddeh — https://freesound.org/people/IPaddeh/sounds/422851/ — CC0 1.0
- "Munch 9.wav" by JarredGibb — https://freesound.org/people/JarredGibb/sounds/217342/ — CC0 1.0
- "shoveling-dirt" by JeffPearson — https://freesound.org/people/JeffPearson/sounds/849840/ — CC0 1.0
- "Swing Woosh" by Jofae — https://freesound.org/people/Jofae/sounds/389590/ — CC0 1.0
- "human male scream 5" by JohnsonBrandEditing — https://freesound.org/people/JohnsonBrandEditing/sounds/243382/ — CC0 1.0
- "Electric Train Motor Idle - Loop (Queensland Rail InterCity Express).wav" by JotrainG — https://freesound.org/people/JotrainG/sounds/700034/ — CC0 1.0
- "Enemy hurt 1" by K27K_Mike — https://freesound.org/people/K27K_Mike/sounds/853659/ — CC0 1.0
- "Male Disgusted" by kanyonwyvern — https://freesound.org/people/kanyonwyvern/sounds/740303/ — CC0 1.0
- "Stone Crumble 2" by Kastenfrosch — https://freesound.org/people/Kastenfrosch/sounds/521956/ — CC0 1.0
- "Barrel Break 4.wav" by kevinkace — https://freesound.org/people/kevinkace/sounds/66772/ — CC0 1.0
- "Crate Break 4.wav" by kevinkace — https://freesound.org/people/kevinkace/sounds/66780/ — CC0 1.0
- "spark2.ogg" by Kneeling — https://freesound.org/people/Kneeling/sounds/643861/ — CC0 1.0
- "factory ambience industrial pump room hum with PA announcement heavy echo and pump turn on start.flac" by kyles — https://freesound.org/people/kyles/sounds/637521/ — CC0 1.0
- "Wood_Log_Scrape_Roll_Concrete_02" by LamaMakesMusic — https://freesound.org/people/LamaMakesMusic/sounds/828614/ — CC0 1.0
- "tire_skid_02" by loganzsound — https://freesound.org/people/loganzsound/sounds/774265/ — CC0 1.0
- "splash 1b" by Logicogonist — https://freesound.org/people/Logicogonist/sounds/807962/ — CC0 1.0
- "Chainsaw Cutting.wav" by lonemonk — https://freesound.org/people/lonemonk/sounds/185579/ — CC0 1.0
- "Basic Fire whoosh" by LookIMadeAThing — https://freesound.org/people/LookIMadeAThing/sounds/260554/ — CC0 1.0
- "Basic Fire Whoosh 2" by LookIMadeAThing — https://freesound.org/people/LookIMadeAThing/sounds/260555/ — CC0 1.0
- "squelch watermelon" by MaddieCooper — https://freesound.org/people/MaddieCooper/sounds/739184/ — CC0 1.0
- "Boing 2" by magnuswaker — https://freesound.org/people/magnuswaker/sounds/540790/ — CC0 1.0
- "minigun_Overheat.wav" by Marregheriti — https://freesound.org/people/Marregheriti/sounds/266102/ — CC0 1.0
- "classic double swwosh .wav" by martian — https://freesound.org/people/martian/sounds/19312/ — CC0 1.0
- "Shovel Gong 170504_1500.wav" by megashroom — https://freesound.org/people/megashroom/sounds/391492/ — CC0 1.0
- "Distant Explosion" by Mr_KeybOred — https://freesound.org/people/Mr_KeybOred/sounds/372086/ — CC0 1.0
- "Voice_AdultMale_PainGrunts_09.wav" by MrFossy — https://freesound.org/people/MrFossy/sounds/547209/ — CC0 1.0
- "circula_mono_4824.wav" by Mystikuum — https://freesound.org/people/Mystikuum/sounds/400867/ — CC0 1.0
- "Wet Splat 1.mp3" by nebulasnails — https://freesound.org/people/nebulasnails/sounds/495118/ — CC0 1.0
- "Cartoon Wow Scream Voice" by NicknameLarry — https://freesound.org/people/NicknameLarry/sounds/508898/ — CC0 1.0
- "Swinging staff whoosh (strong) 04.wav" by Nightflame — https://freesound.org/people/Nightflame/sounds/422513/ — CC0 1.0
- "Grunt" by nomiqbomi — https://freesound.org/people/nomiqbomi/sounds/578788/ — CC0 1.0
- "verydistantexplosion.wav" by Opticreep — https://freesound.org/people/Opticreep/sounds/255527/ — CC0 1.0
- "Headshot.wav" by Pablobd — https://freesound.org/people/Pablobd/sounds/511194/ — CC0 1.0
- "Bow Drawn" by Paveroux — https://freesound.org/people/Paveroux/sounds/490556/ — CC0 1.0
- "Pain 2" by Peter_Scream — https://freesound.org/people/Peter_Scream/sounds/720758/ — CC0 1.0
- "Machine Gun 001 - single shot.ogg" by pgi — https://freesound.org/people/pgi/sounds/212601/ — CC0 1.0
- "Machine Gun 002 - single shot.ogg" by pgi — https://freesound.org/people/pgi/sounds/212607/ — CC0 1.0
- "swoosh.wav" by PorkMuncher — https://freesound.org/people/PorkMuncher/sounds/263595/ — CC0 1.0
- "squelch_3" by QuantumFellow — https://freesound.org/people/QuantumFellow/sounds/734836/ — CC0 1.0
- "axe hits - 1.wav" by quatricise — https://freesound.org/people/quatricise/sounds/582372/ — CC0 1.0
- "Cannon Shot" by qubodup — https://freesound.org/people/qubodup/sounds/187767/ — CC0 1.0
- "Collision" by qubodup — https://freesound.org/people/qubodup/sounds/332058/ — CC0 1.0
- "Waboba Moon Ball Water Splash 5" by qubodup — https://freesound.org/people/qubodup/sounds/867460/ — CC0 1.0
- "Boom with Rumble" by Qvantor — https://freesound.org/people/Qvantor/sounds/879263/ — CC0 1.0
- "earth rumble.wav" by Reitanna — https://freesound.org/people/Reitanna/sounds/217657/ — CC0 1.0
- "Short Scream" by riippumattog — https://freesound.org/people/riippumattog/sounds/788867/ — CC0 1.0
- "Splash-eau-goudron1.wav" by rombart — https://freesound.org/people/rombart/sounds/186748/ — CC0 1.0
- "Sharp Explosion 4 (of 5)" by Rudmer_Rotteveel — https://freesound.org/people/Rudmer_Rotteveel/sounds/336011/ — CC0 1.0
- "Displeasure - Urgh why" by Sadiquecat — https://freesound.org/people/Sadiquecat/sounds/801096/ — CC0 1.0
- "Longbow Release 2.wav" by saturdaysoundguy — https://freesound.org/people/saturdaysoundguy/sounds/394179/ — CC0 1.0
- "cartoon-sound-single-boing" by sdroliasnick — https://freesound.org/people/sdroliasnick/sounds/731262/ — CC0 1.0
- "Tom-esque Scream 1.mp3" by se2001 — https://freesound.org/people/se2001/sounds/464361/ — CC0 1.0
- "FUSE.wav" by Seidolon — https://freesound.org/people/Seidolon/sounds/372186/ — CC0 1.0
- "An automatic rifle being shot once // punchy sounding" by serøutōnin--deprivəd — https://freesound.org/people/ser%C3%B8ut%C5%8Dnin--depriv%C9%99d/sounds/855654/ — CC0 1.0
- "Crossbow Shot.wav" by spookymodem — https://freesound.org/people/spookymodem/sounds/249810/ — CC0 1.0
- "sqeeeek_sheep.wav" by sqeeeek — https://freesound.org/people/sqeeeek/sounds/237103/ — CC0 1.0
- "Match Sizzle 02" by squareal — https://freesound.org/people/squareal/sounds/237407/ — CC0 1.0
- "Chainsaw - Pull to Start and Run.wav" by Stefan021 — https://freesound.org/people/Stefan021/sounds/431742/ — CC0 1.0
- "Autumn wind and dry leaves.wav" by Stek59 — https://freesound.org/people/Stek59/sounds/457318/ — CC0 1.0
- "ATM INT Wind Howl Cold Winter 09 (stereo).wav" by Takkyi — https://freesound.org/people/Takkyi/sounds/546871/ — CC0 1.0
- "BeesShortBurst.wav" by taylordonj — https://freesound.org/people/taylordonj/sounds/521364/ — CC0 1.0
- "Quake Short.wav" by TheMinkman — https://freesound.org/people/TheMinkman/sounds/199803/ — CC0 1.0
- "Simple Sizzle .m4a" by thomashill1 — https://freesound.org/people/thomashill1/sounds/478800/ — CC0 1.0
- "Grunt1 - Death Pain.wav" by tonsil5 — https://freesound.org/people/tonsil5/sounds/416839/ — CC0 1.0
- "Grunt2 - Death Pain.wav" by tonsil5 — https://freesound.org/people/tonsil5/sounds/416838/ — CC0 1.0
- "Arrow Impact" by Twisted_Euphoria — https://freesound.org/people/Twisted_Euphoria/sounds/205938/ — CC0 1.0
- "Forest Ambience with Birds" by upkey — https://freesound.org/people/upkey/sounds/826326/ — CC0 1.0
- "large explosion 1" by V-ktor — https://freesound.org/people/V-ktor/sounds/482993/ — CC0 1.0
- "water_splash_10shots.wav" by vibe_crc — https://freesound.org/people/vibe_crc/sounds/50623/ — CC0 1.0
- "bone break_5.wav" by Vinni_R — https://freesound.org/people/Vinni_R/sounds/630594/ — CC0 1.0
- "Bone Breaking" by wackoME — https://freesound.org/people/wackoME/sounds/413430/ — CC0 1.0
- "Grunt1.wav" by whisperbandnumber1 — https://freesound.org/people/whisperbandnumber1/sounds/397276/ — CC0 1.0
- "Steam/hiss" by wubitog — https://freesound.org/people/wubitog/sounds/234782/ — CC0 1.0
- "Fast whoosh, bamboo swoosh through air, version 2" by zapsplat.com — https://freesound.org/people/zapsplat.com/sounds/719637/ — CC0 1.0
- "Jumppad" by zivs — https://freesound.org/people/zivs/sounds/725734/ — CC0 1.0

Game code, models and procedural graphics: © the author. Engine: three.js (MIT). Font: Bungee (SIL Open Font License 1.1).

## Source
This public repo contains only the built game (`index.html`) and this README. The source (`src/`) and the headless test scripts live in the private working folder and are not published here.
