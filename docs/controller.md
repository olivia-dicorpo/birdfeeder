# Hatch controller

Canonical source: **[olivia-dicorpo/birdfeeder-ctlr](https://github.com/olivia-dicorpo/birdfeeder-ctlr)**.

This workspace keeps a copy under `controller/` (Tiny Tapeout layout: `src/`, `test/`, `info.yaml`, GitHub Actions). Do not treat the old Downloads FPGA dump as the design; that copy is gone.

Datasheet notes: [`controller/docs/info.md`](../controller/docs/info.md).

## Behavior

`birdfeeder_top` drives an SG90 continuous-rotation servo from one **detection-arm** switch (`ui_in[1]` OR `uio[1]`, active high).

1. **OPENING** (digit `1`) — open 3 s
2. **OPEN** (digit `2`) — stop/hold while the switch is closed; after release, wait 10 s (re-close restarts the wait)
3. **CLOSING** (digit `3`) — close 3 s (arm trip aborts and reopens)
4. **IDLE** (digit `0`) — stop until the next trip

Jog: `diag_up` `ui_in[2]`, `diag_down` `ui_in[3]`; both held cancels drive.

| cmd | Action | Pulse |
|-----|--------|-------|
| 00 | Stop | 1.5 ms |
| 01 | Open | 1.0 ms |
| 10 | Close | 2.0 ms |

Clock 10 MHz. PWM on `uio[0]` (OE only while active). 8-segment state on `uo_out`; DP while PWM is on.

## Wiring

- Onboard piano switch `ui_in[1]` already has a pull-down.
- External switch on `uio[1]`: ~10 kΩ pulldown to GND, switch to 3.3 V. `uio[1]` OE is off.

## Files

| Path | Role |
|------|------|
| `controller/src/project.v` | `tt_um_olivia_dicorpo_birdfeeder` wrapper |
| `controller/src/birdfeeder_top.v` | Door FSM |
| `controller/src/sg90_continuous_pwm.v` | 50 Hz PWM |
| `controller/test/test.py` | Cocotb tests |
| `controller/.github/workflows/` | Tiny Tapeout GDS, sim, FPGA, docs CI |

```sh
cd controller/test && make -B
```
