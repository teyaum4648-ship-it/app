# EXFIL — a turn-based tactical extraction roguelike

Open `exfil.html` in any browser. One file, no dependencies, no build step.
Progress saves to `localStorage`.

An original game, mechanically inspired by the extraction-shooter genre.

## The loop

Build a weapon → assemble a loadout → deploy → fight or evade → reach an extract.

**Anything you carry into a raid can be lost.** Die and your weapon, armour, rig,
magazines, medical kit and everything you looted are gone. Anything left in the
stash is safe. Extract and you keep all of it.

## Weapon configuration

The centrepiece. Ten platforms, ~85 attachments, twelve slots, and compatibility
that actually bites:

- An optic needs a **rail**, which comes from a railed dust cover, a rail
  handguard, or a side mount. A stock AKM has none, so the optic slot is empty
  until you buy one.
- A suppressor needs a **threaded barrel**. The cheap unthreaded barrel cannot
  take one.
- A foregrip needs a **lower rail**.
- Remove the part that provided a rail and anything mounted to it comes off too.

Every part feeds the combat resolver:

| Stat | What it does in a raid |
|---|---|
| Ergonomics | Action points per shot and per reload, and baseline accuracy |
| Vertical / horizontal recoil | How fast accuracy decays across a burst |
| Dispersion (MOA) | Spread at range |
| Sight range / magnification | How far you can shoot before accuracy collapses |
| Muzzle velocity | Penetration retained at distance |
| Weight | Action points per turn |
| Noise | How far a shot pulls hostiles toward you |

## Ballistics and armour

31 cartridges across six calibres, each with damage, penetration power, armour
damage, fragmentation chance and muzzle velocity.

Armour has a class and a durability value, and **worn plates stop less** — a
battered class 6 plate performs closer to a fresh class 2. Penetration is rolled
against the plate's class scaled by its remaining durability. A stopped round
still delivers blunt damage through the plate.

Seven body parts with independent health. Head or thorax to zero is death. A
blacked arm wrecks your accuracy and slows reloads; a blacked leg costs movement.
Wounds bleed, legs fracture, and only the right medical item fixes each one.

The workbench shows your loaded round's penetration chance against every armour
class at 25 m, 75 m and 150 m, so the choice is informed rather than guessed.

## Controls

Click to move, click a hostile to fire, click a container to loot.

| Key | Action |
|---|---|
| `1` `2` `3` | Stand / crouch / prone — lower stances are steadier *and* harder to spot |
| `Q` | Spend an action point aiming (up to +3) |
| `F` | Cycle fire mode |
| `R` | Reload |
| `E` | Extract, when standing on an exit |
| `Space` | End turn |
| `WASD` / arrows | Step one tile |

Hovering a hostile shows the real hit chance, their armour class and remaining
durability, and whether your current round will get through it.

## Maps

| Map | Level | Character |
|---|---|---|
| Factory | 1 | Tight industrial interior, contact is close and immediate |
| Customs | 1 | Warehouses and lanes, plenty to loot |
| Woods | 3 | Long sightlines that punish iron sights |
| The Lab | 8 | Raiders in body armour, the best loot, a key-locked exit |

Maps are generated fresh every raid.

## Notes

Hostiles get one turn of reaction time when they first spot you — that window is
your chance to break contact. Break line of sight and they will search for a few
turns, then lose you. Unsuppressed gunfire pulls everything within earshot.

Testing hooks live on `window.__sim` — penetration rate sampling, platform
audits, automatic build optimisation, and headless auto-played raids.
