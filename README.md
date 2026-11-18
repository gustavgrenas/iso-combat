# Iso Combat (prototype v1)

Brutal and silly real-time isometric arena combat. It is one self-contained file, `index.html` (about 0.57 MB), and works offline.
It is built from `src/` with `npm run build` (esbuild bundles three.js r170 together with the embedded Bungee font).

## Controls
| Key | Action |
|---|---|
| WASD / arrows | Run (screen-relative) |
| Mouse | Aim |
| Left click (hold) | Attack with the current weapon |
| Right click / F | Kick (knockback that sends enemies off the edge) |
| Space | Dodge roll (brief invulnerability) |
| 1–4, Q/E, scroll wheel | Yxa · Armborst · Bomb · Fårkanon |
| Esc / P | Pause · M: sound · F3: perf overlay |

URL flags: `?perf` (perf overlay), `?play` (skip the menu), `?god`, `?bot` (autopilot).

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

## Tests (headless Chrome via Playwright)
- `tests/input.py` exercises real keyboard and mouse input: movement direction, all weapons, dodge, kick ring-out, pause, game over and restart.
- `tests/shots.py` takes 1080p screenshots into `shots/`.
- `tests/survive.py` covers the autopilot without god mode, plus a sim stress test.
- `tests/stuck.py` checks that waves never stall.
- `tests/perf.py` measures CPU per frame at 1440×900 @2x.
