# Iso Combat (prototype v2)

Brutal and silly real-time isometric arena combat. It is one self-contained file, `index.html` (about 0.57 MB), and works offline.
It is built from `src/` with `npm run build` (esbuild bundles three.js r170 together with the embedded Bungee font).

## Play
https://gustavgrenas.github.io/iso-combat/ — works offline too: `index.html` is the whole game.

## Controls (default: keyboard twin-stick)
| Key | Action |
|---|---|
| WASD | Run (screen-relative) |
| Arrow keys | Aim **and** attack in that direction while held (8 directions; two arrows = diagonal). Soft aim assist bends toward the nearest enemy within ~25°. |
| Shift | Kick (knock enemies off the edge) |
| Space | Dodge roll |
| Q/E or 1–4 | Switch weapon: Yxa · Armborst · Bomb · Fårkanon |
| Esc / P · M | Pause · sound |

Mouse aiming is available as an option: **"Styrning: Tangentbord / Mus"** in the title or pause menu (saved in the browser). In mouse mode: mouse = aim, left click = attack, right click/Shift = kick, mouse wheel = switch weapon.

## Architecture (prepared for online play)
- `src/sim.js` is a pure fixed-timestep simulation at 60 Hz. It has no DOM and no three.js, and runs in Node.
  - All state is plain objects plus typed arrays.
  - Input commands are plain objects `{mx,mz,ax,az,fire,alt,dodge,weap}`, one per player: `step(S, inputs[])`.
  - The game is deterministic, using a seeded RNG stored in the state.
  - `snapshot()` and `restore()` exist.
  - `tests/sim_determinism.mjs` verifies identical results from a seed and from a snapshot.
- Players are kept as a list in `S.players`, and the local player is index 0. `addPlayer()` is ready for more players.
- The sim sends events (`expl`, `gib`, `vox`, `impact`, `callout`, `sfx`…) to the renderer. All cosmetic effects live on the client: debris, blood, smoke and sound.
- `src/render.js` renders everything with a few `InstancedMesh` batches: world, voxels, characters/projectiles, debris, blood, fx. That comes to about 6 draw calls plus the shadow pass. Debris is capped at 1300 and fx at 900. Pixel ratio is capped at 2.

## Source
This public repo contains only the built game (`index.html`) and this README. The source (`src/`: sim, renderer, aim assist, audio) and the headless test scripts live in the private working folder and are not published here.
