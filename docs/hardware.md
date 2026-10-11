# Hardware

Designed in Autodesk Fusion 360. Native archives are under `cad/fusion/`. Neutral STEP/STL exports are under `cad/step/` and `cad/stl/`.

Envelope sizes below are axis-aligned bounding boxes of the exported solids (millimetres). They describe the part file, not necessarily the assembled overall size.

## Fusion assembly

Root design: **actual bird feeder**.

`cad/fusion/birdfeeder-corrected.f3z` (2026-07-21) is the later Fusion archive. It references:

| Component | Role |
|-----------|------|
| body correct | Main tray / body |
| lid correct | Moving hatch / lid |
| clear seed storage | Transparent hopper |
| servo | SG90 micro servo |
| gear | Servo output gear |
| rack | Linear rack on the lid/hatch |
| bracket | Servo / mechanism mount |

`cad/fusion/actual-bird-feeder.f3z` (2026-07-03) is an earlier export of the same assembly (root version 20).

`cad/fusion/body-correct.f3d` (2026-09-27) is a later native body part.

## Parts (latest STEP)

| File | Approx. envelope (mm) | Notes |
|------|----------------------|--------|
| `cad/step/body-correct.step` | 136 × 150 × 76 | Body, 2026-09-27 |
| `cad/step/lid-correct.step` | 110 × 161 × 69 | Lid, 2026-09-25 |
| `cad/step/clear-seed-storage.step` | 115 × 167 × 300 | Hopper, ~300 mm tall |
| `cad/step/roof.step` | 150 × 224 × 307 | Roof over the hopper |
| `cad/step/perch-left.step` | 100 × 100 × 87 | Left perch |
| `cad/step/perch-right.step` | 96 × 94 × 87 | Right perch |
| `cad/step/spring-arm-right.step` | 27 × 68 × 66 | Detection arm |
| `cad/step/spring-arm-left.step` | 27 × 68 × 66 | Mirrored detection arm |
| `cad/step/base.step` | 200 × 200 × 62 | Base |
| `cad/step/actual-bird-feeder.step` | 141 × 172 × 87 | Partial assembly export (body + perch-right + triangle), 2026-08-27 |

## Bought parts

| Item | File / number | Use |
|------|----------------|-----|
| SG90 continuous-rotation micro servo | `cad/vendor/sg90-micro-servo.stl` | Hatch drive |
| McMaster-Carr torsion spring 9271K26 | `cad/vendor/mcmaster-9271K26-torsion-spring.step` | Detection-arm return |
| McMaster-Carr torsion spring 9271K891 | `cad/vendor/mcmaster-9271K891-torsion-spring.step` | Detection-arm return |

The FPGA design treats the arm as a **single level-sensitive switch** (pest / detection arm). Wire that switch to `ui_in[1]` or `uio[1]` with a pulldown if the pin would otherwise float.

## Meshes

| File | Source |
|------|--------|
| `cad/stl/actual-bird-feeder.stl` | Fusion export of the assembly (2026-07-20) |
| `cad/stl/lid-correct.stl` | Lid |
| `cad/stl/perch.stl` | Perch |
| `cad/stl/perch-new.stl` | Later perch mesh |

## Suggested print / CAD workflow

1. Open `cad/fusion/birdfeeder-corrected.f3z` in Fusion for mates and the rack-and-pinion layout.
2. Prefer the dated STEP files in `cad/step/` when a part was updated after that archive (body, lid, perches, spring arms, roof, base).
3. Use STLs for slicing only after checking they match the STEP you intend to build.
