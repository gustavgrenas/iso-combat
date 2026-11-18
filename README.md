# Iso Combat (prototype v3)

Brutal and silly real-time isometric arena combat. It is one self-contained file, `index.html` (about 0.63 MB), and works offline.
It is built from `src/` with `npm run build` (esbuild bundles three.js r170 together with the embedded Bungee font).

## Play
https://gustavgrenas.github.io/iso-combat/ — works offline too: `index.html` is the whole game.

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
| Space | Dodge roll (also puts out fire on you) |
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
