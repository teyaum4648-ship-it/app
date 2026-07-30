# EXFIL

Two self-contained games in this repo. One file each, no dependencies, no build
step — open in a browser. Progress saves to `localStorage`.

| File | What it is |
|---|---|
| **`exfil-fps.html`** | **First-person extraction shooter.** Real-time, mouse-look, raw WebGL2. The main game. |
| `exfil.html` | The earlier turn-based tactical version, same systems on a grid. |

Original games, mechanically inspired by the extraction-shooter genre.

---

## The loop

Build a weapon → assemble a loadout → deploy → fight or evade → reach an extract.

**Anything you carry in can be lost.** Die and your weapon, armour, rig,
magazines, medical kit and everything you looted are gone. Anything left in the
stash is safe.

## Controls

Click the view to capture the mouse.

| Input | Action |
|---|---|
| `W A S D` | Move |
| Mouse | Look · left fires · **right aims down sights** |
| `Shift` | Sprint |
| `C` / `Z` | Crouch · prone — steadier *and* harder to be seen |
| `Space` | Jump |
| `R` | Reload |
| `F` | Fire mode |
| `E` | Search a container |
| `H` | Use whichever medical item actually treats what's wrong |
| `Esc` | Pause, and mouse sensitivity |

## Ballistics

Rounds are **simulated projectiles**, not hitscan. Each one leaves the muzzle at
the weapon's real muzzle velocity, travels, and falls under gravity — so at
distance you lead and hold over. A 9x19 subsonic drops roughly six times as far
as 5.56 over the same 100 m.

Hits are resolved against **seven per-limb hitboxes**, not a probability table.
Where the round passes through the body is where it lands: a high shot over a
crouching target misses entirely, and a head shot is a head shot because the
geometry says so.

Armour has a class and a durability value, and **worn plates stop less** — a
battered class 6 plate performs closer to a fresh class 2. Penetration is rolled
against class scaled by remaining durability, and a stopped round still delivers
blunt damage. Head or thorax to zero is death. A destroyed arm wrecks your hold;
a destroyed leg slows you. Wounds bleed on the clock, legs fracture, and only the
right item fixes each one.

## Weapon configuration

Ten platforms, ~85 attachments, twelve slots, with compatibility that bites:

- An optic needs a **rail** — from a railed dust cover, a rail handguard, or a
  side mount. A stock AKM has none, so the optic slot stays empty until you buy one.
- A suppressor needs a **threaded barrel**. The cheap unthreaded barrel won't take one.
- A foregrip needs a **lower rail**.
- Remove the part that provided a rail and anything mounted to it comes off too.

Every part drives how the gun actually behaves:

| Stat | What it does |
|---|---|
| Ergonomics | How fast the sights come up, and how much the weapon sways |
| Vertical / horizontal recoil | Degrees of climb per round, which you fight during a burst |
| Dispersion (MOA) | The cone your rounds leave in |
| Sight range / magnification | Field of view when aimed, and how far you can identify |
| Muzzle velocity | Travel time, drop, and penetration retained at range |
| Weight | Movement speed |
| Noise | How far a shot pulls hostiles toward you |

Recoil is applied to your view and settles back at a rate set by ergonomics — a
long burst walks off target unless you build the gun to hold it or pull down.

The workbench shows time-to-aim, recoil per shot, drop at 100 m, and your loaded
round's penetration chance against every armour class at 25/75/150 m.

## Hostiles

Scavs, scav snipers, raiders, and a boss with guards. Each has a detection range,
a **reaction time** before the first shot, and an aiming error in degrees — that
error is the whole of the difficulty. They fire the same projectile system you do,
so their rounds travel, drop, and can miss.

The turn a hostile first acquires you is spent getting into position, not
shooting. That window is your chance to break contact — and if you do break line
of sight, they search for a few seconds and then lose you. Unsuppressed gunfire
pulls in everything within earshot.

## Maps

| Map | Level | Character |
|---|---|---|
| Factory | 1 | Enclosed plant, contact inside 30 m |
| Customs | 1 | Warehouses and lanes, the most to loot |
| Woods | 3 | Open treeline, engagements past 100 m |
| The Lab | 8 | Armoured raiders, best loot, key-locked exit |

Levels are generated fresh every raid and extruded into 3D from a grid.

## Requirements

WebGL2, and a keyboard and mouse — it needs a pointer it can capture, so it will
not play on a touchscreen. The menus, workbench and traders work anywhere.

## Testing hooks

`window.__sim` exposes the combat maths for headless testing: penetration-rate
sampling, per-limb hitbox tracing, platform audits, automatic build optimisation,
and `autoRaid()`, which runs full raids with no renderer at a fixed timestep.
