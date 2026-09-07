# Quick & dirty watermaker test rig

Interim bench rig for the [watermaker](../watermaker) project. The real HP pump
(AR RC 13.17 C + F6 flange, Mister Worker) has a ~1 month lead time; this rig
lets us pressurise the system and exercise valves, pressure sensors, flow
meters, the PRV and the pressure switch in the meantime.

**Pressure source: a Kärcher K5 pressure washer, used intact.** No VFD, no
motor swap, no coupling. Pressure is set hydraulically with a bypass.

Decided 2026-08-20. Scope: water-only hydraulic testing. Not for membranes,
not for permeate, not for process validation.

## Why the K5

| | K5 | AR RC 13.17 (real pump) |
|---|---|---|
| Max pressure | ~145 bar | 170 bar |
| Flow | ~500 L/h | ~780 L/h (13 L/min) |
| Motor | 2.1 kW, water-cooled induction | Busck 1.1 kW 4-pole via VFD |
| Duty | continuous-ish (induction, water-cooled) | continuous |

- We need 15–20 bar operating, 30 bar for the HP pressure-test gate, 25 bar
  for PRV/PS1 checks. The K5 does that at ~20 % of its rating.
- 500 L/h is about 1/3 of design feed flow — enough for valve, sensor,
  flow-meter and pressure tests; **not** enough to run a membrane at design
  recovery. Membranes stay out of the rig.
- K2/K3 (universal motor, intermittent duty) were rejected — they overheat on
  long runs. The "WD3" is a wet/dry vacuum, not a pressure washer.
- Pulling the K5 pump head off its motor was rejected: the wobble plate sits
  directly on the motor shaft with no independent bearings — there is nothing
  to couple to.
- Stand-in 24 mm hollow/solid-shaft triplex pumps for the Busck motor were
  rejected: nothing credible on Amazon, AliExpress clones have unverified
  shaft/flange and a China lead time, Interpump WS102 (~£320) / W97 (€765) are
  too expensive for a stand-in.

## Rig layout

```
feed tank ──► [pre-pump ~3 bar, see below] ──► V2 ──► sand filter ──► 5 µm ──► 1 µm ──► K5 inlet
                                                                             │
                                           K5 outlet (M22×1.5 → 3/8" BSP) ───┘
                                                     │
                              ┌──────────────────────┴──────────────────────┐
                              │  HP manifold / DUT section                  │
                              │  PT3 (0–25 bar) · PS1 (25 bar) · PRV (25)   │
                              │  V6 motorised ball valve · V3 check         │
                              │  vessel (EMPTY or blank spool)              │
                              │  flow meter(s)                              │
                              └───────────────┬──────────────┬──────────────┘
                                              │              │
                                   V5 needle (bypass)   test outlet / drain
                                              │              │
                                              └──► feed tank ◄┘
```

**System pressure is set by throttling the bypass (V5 needle), never by
closing the outlet.** A dead-headed K5 trips its total-stop switch and cycles
the motor on/off. Always leave a flow path back to the tank.

## Parts list

### Already in hand / ordered in the main project (reuse, do not buy again)

| Item | Where from | Notes |
|---|---|---|
| WIKA A-10 PT1/PT1b/PT2 (0–6 bar), PT3 (0–25 bar) | in hand | 4–20 mA |
| Shako PU220D solenoids V2/V7a/V7b/V9 | ordered 2026-08-18 | LP side only, 0–10 bar coils |
| Tameson BL2SA3-012 V6 motorised ball valve, 63 bar | ordered 2026-08-18 | the only HP-zone actuated valve |
| Tameson CLYS-012 / 900402 check valves V3/V8/V10 | ordered 2026-08-18 | |
| Tameson NLS-012 V5 needle valve | ordered 2026-08-18 | this is the bypass throttle |
| END-Armaturen SV320023 PRV, 25 bar | inquiry sent | test lift point on the rig |
| PS1 25 bar mechanical switch | open | |
| Flow meter(s) | per main project | |
| Atlas EZO-EC / pH | in hand | not needed for hydraulic tests |

### To buy for the rig

| # | Item | Spec | Qty | Approx. cost | Notes |
|---|---|---|---|---|---|
| 1 | Kärcher K5 pressure washer | K5 Classic / Premium, whichever is discounted | 1 | 2 500–3 500 SEK | Must be K5 (or K7), not K2/K3 |
| 2 | M22×1.5 → 3/8" BSP adapter | Kärcher Quick Connect (14 mm nipple) female → 3/8" BSP male, brass, ≥250 bar | 1–2 | 100–200 SEK | Standard pressure-washer fitting |
| 3 | HP hose, short | Pro DN8 R1 steel-braid, 3/8" BSP both ends, ≥200 bar, 1–2 m | 1 | 200–400 SEK | Rig only. The Kärcher DN6 hose is also fine; neither goes in the final system |
| 4 | 3/8" → 1/2" BSP SS adapters | SS316 | 2–4 | 100–300 SEK | To enter the SS316 valve fleet (G1/2) |
| 5 | Extra needle valve (optional) | Brass or SS, 1/2" BSP, 0–100 bar | 1 | 200–400 SEK | If V5 is needed elsewhere in the DUT chain |
| 6 | Test gauge | Glycerine-filled, 0–40 bar, 1/4" BSP + adapter | 1 | 150–300 SEK | Independent reference for PT3 / PS1 / PRV |
| 7 | Feed tank | 60–100 L tub/bucket, outlet near bottom | 1 | 150–300 SEK | Also the bypass/drain return |
| 8 | Feed hose + fittings | 12 mm PE / LF 3000 from the pre-pump section, 3/4" garden-hose adapter at the K5 inlet | — | 100–200 SEK | K5 wants ≥ ~600 L/h at 0.5–1 bar inlet; it does not self-prime well — hence the pre-pump |
| 9 | Inlet strainer | Kärcher inlet filter or inline mesh | 1 | 50–150 SEK | Protect the K5 from debris |
| 10 | PTFE tape / thread sealant, BSP washers | | — | 100 SEK | |
| 11 | RCD-protected outlet | 16 A, 230 V | 1 | — | K5 is 2.1 kW on a 10 A plug; nothing else on the circuit |

Budget: roughly 4 000–6 000 SEK for the HP side plus 1 500–3 000 SEK for the
pre-pump section below, of which the K5 and the garden pump stay useful
afterwards.

## Pre-pump (emulates the seawater feed pump)

On site the feed comes from an existing seawater pump at **~3 bar** through a
12 mm PE / LF 3000 outlet into V2 → sand filter → 5 µm → 1 µm → HP-pump
suction, with PT1 / PT1b / PT2 (0–6 bar) reading the ΔPs. The rig needs the
same thing from the test tank, for two reasons:

1. The K5 wants ≥ ~600 L/h at 0.5–1 bar on its inlet and does not self-prime
   from a low tank.
2. The LP fleet only behaves realistically with ~3 bar upstream: V2 is
   servo-assisted and needs ≥0.5 bar ΔP to open at all, the cartridge/sand
   filter ΔP readings need real flow, and PT1/PT1b/PT2 need something to read.

### Requirements

| | Value | Why |
|---|---|---|
| Pressure | 2.5–4 bar at the working point | matches the ~3 bar site pump; K5 inlet accepts up to 12 bar |
| Flow | 1 000–1 500 L/h at ~3 bar | design feed flow; the K5 takes ~500 L/h, the rest returns via bypass |
| Type | self-priming jet/garden pump or flooded-suction centrifugal, **no pressure switch** | continuous running, no hydrophor cycling |
| Power | 230 V, ≤ ~1 kW | separate socket from the K5 |
| Ports | 1" BSP in/out typical → adapt to 12 mm PE / LF 3000 | same interface as the site pump outlet |
| Wetted | plastic/brass/SS, tap water | rig only |

A cheap 600–900 W self-priming garden pump (Biltema / Jula / Gardena class:
~3 000–3 500 L/h open flow, ~4 bar shut-off) hits this band once it is
running into the rig's resistance; expect roughly 1 000–2 000 SEK. Do **not**
buy a "hydrofor"/booster variant with an integral pressure switch — it will
cycle against the K5's total-stop.

### Parts

| # | Item | Spec | Qty | Approx. cost | Notes |
|---|---|---|---|---|---|
| 12 | Garden / jet pump | 230 V, 600–900 W, self-priming, ~3 bar @ 1 000–1 500 L/h, 1" BSP ports | 1 | 1 000–2 000 SEK | No pressure switch |
| 13 | Suction hose + foot valve/strainer | 1" reinforced suction hose, 1–2 m, foot valve with strainer | 1 | 200–400 SEK | Keeps prime, protects the pump |
| 14 | Outlet adapters | 1" BSP → 1/2" BSP → LF 3000 12 mm push-fit stud | 1 set | 150–300 SEK | Gives the same 12 mm PE interface as the site pump |
| 15 | Throttle / regulating valve on the pre-pump outlet | 1/2" ball or needle, brass | 1 | 100–200 SEK | Sets the ~3 bar working point |
| 16 | Pressure gauge 0–6 bar | 1/4" BSP, glycerine | 1 | 100–150 SEK | Reference for PT1 |
| 17 | Return line to tank | 12 mm PE or 1/2" hose + 1/2" ball valve | 1 | 100–200 SEK | Pre-pump bypass so the pump never dead-heads when V2 closes |

### Layout

```
test tank ──► foot valve/strainer ──► [pre-pump ~3 bar] ──► throttle ──► gauge/PT1 ──► V2 ──► sand filter ──► PT1b ──► 5 µm ──► 1 µm ──► PT2 ──► K5 inlet
                                             │
                                        pre-pump bypass (ball valve) ──► test tank
```

Same rule as the HP side: the pre-pump always has a path back to the tank.
When V2 closes (e.g. during an alarm test) the pre-pump must not dead-head —
leave the pre-pump bypass cracked or let a small relief line run
continuously. A jet pump will tolerate a short dead-head but will heat up.

### Extra tests this enables

- [ ] V2 opens/closes at ~3 bar upstream, and **fails to open** at <0.5 bar ΔP
      (confirm the servo-assisted limitation, then never design around zero ΔP).
- [ ] Sand-filter ΔP (PT1 − PT1b) and cartridge ΔP (PT1b − PT2) at 1 000 L/h,
      clean, and with a deliberately blinded cartridge.
- [ ] Low-suction-pressure alarm at the HP pump inlet (PT2 threshold) by
      throttling the pre-pump.
- [ ] PT1/PT1b/PT2 vs reference gauge at 0 / 1 / 2 / 3 / 4 bar.
- [ ] LP pressure-test gate at 5–6 bar on the PE + LF 3000 fleet (close V2
      against the pre-pump shut-off head; a ~4 bar garden pump will need the
      K5 or a hand test pump to reach 6 bar).

## What the rig is for

Maps to the commissioning gates in
`../watermaker/design/BUILD-AND-COMMISSIONING.md` §1/§3:

- [ ] **LP pressure test at 5–6 bar** (LP fleet, PE + LF 3000): can be done
      from the LP booster; the K5 is overkill here.
- [ ] **HP pressure test at 30 bar, water only, signed log** (SS316 12 mm +
      DIN 2353, V3, V5, V6, PT3, vessel ports): K5 at ~30 bar via bypass.
- [ ] PT3 calibration check vs test gauge at 0 / 10 / 20 / 25 / 30 bar.
- [ ] PS1 trip point (25 bar NC) and reset behaviour.
- [ ] PRV lift / reseat point (factory 25 bar).
- [ ] V6 open/close under 20 bar, limit switches, relay drive from one DO.
- [ ] Check valves V3/V8/V10 hold/crack direction.
- [ ] Flow-meter pulse rate vs bucket-and-stopwatch at 200 / 350 / 500 L/h.
- [ ] LP solenoids V2/V7a/V7b/V9 open/close at ≥0.5 bar differential
      (servo-assisted — they will not open at zero ΔP).
- [ ] Leak check of every DIN 2353 joint at 30 bar.

**Not for:** membrane performance, recovery setting, permeate quality, anything
drinking-water. Vessel runs empty or with a blank spool.

## Operating notes

- Set pressure with the bypass needle; outlet side open to tank. Never
  dead-head.
- Tap water only. Flush the K5 and the SS fleet after each session; don't
  leave brackish or chlorinated water standing in the aluminium K5 head.
- First long run: feel the K5 head/motor housing after 15, 30, 60 min. At
  20–30 bar load it should stay lukewarm.
- Brass/aluminium wetted parts on the K5 — fine for tests, not for anything
  downstream of V7b in the final system.
- Kärcher/pro hoses are rig-only. Final HP zone is SS316 12 mm OD + DIN 2353
  per `../watermaker/design/decisions/019-per-zone-tubing.md`.
- Keep the E-stop / PS1 chain wiring realistic even on the rig so the PS1 test
  is meaningful.

## In parallel: drive-train commissioning (no pump)

While the K5 does the hydraulics, commission the real drive on the bench:

- Busck T3A90S-4 (1.1 kW, 4-pole, B34) + the system VFD, motor unloaded or
  braked by a DC lab motor through a jaw coupling (24 mm H7 + 8 mm key on the
  motor side) and a 3D-printed plate located on the motor's Ø95 B14 spigot.
- Tune: V/f curve, ramp times, current limit at motor FLA (2.55 A Y / 4.43 A Δ),
  direction, klixon (NC thermocontacts) in the trip chain, STO wiring.
- When the AR RC 13.17 C + F6 arrives it bolts straight onto the B34 face —
  no coupling, no plate. Anti-seize on the shaft before sliding the pump on.

## Later

After the Mister Worker pump is delivered, buy a separate plunger pump for the
lab DC motors (solid shaft, jaw coupling, printed spigot-located plate). Not
before — nothing on Amazon/AliExpress was worth it as a stand-in.
