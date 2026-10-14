# Hop damping test (Wii, Riivolution)

This is the first on-console test. It changes one float in the game's memory. It needs no code and no Wiinject build.

## What it changes

- Address `0x80891A4C` is a float in StaticR.rel data. Stock value is `0.998`.
- `Kart::Movement::UpdateHopPhysics` (`0x80579968`) multiplies the vertical velocity by this value every frame.
- The test writes `0.5` (`3F000000`). Vertical velocity then halves each frame, so hops should be much shorter and lower.
- This is a shared global. Other systems may read it too, so expect side effects. That is fine for a test.

## Install

1. Copy `HopTest.xml` to the SD card at `riivolution/HopTest.xml`.
2. Open Riivolution from the Homebrew Channel.
3. Select Mario Kart Wii (disc ID `RMCP01`).
4. Under "Hop damping", pick **Halved**.
5. Boot the game and do a hop in any race or time trial.

## What to expect

- **Hop is much shorter and lower.** The patch works and the address is right.
- **No change.** Riivolution's memory patch may run before StaticR.rel loads, so the game overwrites the value. Tell me this and we switch to a Wiinject hook that writes the value each frame.
- **Something else changes.** Note what changed. That tells us the constant has wider use than the hop.

## Revert

Pick **Stock** in the Riivolution menu, or delete `riivolution/HopTest.xml`.

## Notes

- The disc must be `RMCP01` (PAL). The repo's pinned `main.dol` and `StaticR.rel` hashes match that release. If Riivolution doesn't list the game, check the disc ID.
- Wiinject is not needed for this test. It's needed later for code hooks. Its README doesn't fully say whether a hook can call the original function back, so we check that before writing hooks.
