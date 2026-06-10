# Aegis: Autonomous Laser Defense System

> A turret that watches a room, spots a ball thrown from any direction, predicts where it's going, and tracks it with a steered laser beam, without any human input. 

**Hack Club Highway project** · Deadline: July 31, 2026

---

## What it does

A camera watches the room. When a ball is thrown — from any angle, at any speed — the system:

1. **Detects** the ball in each camera frame (OpenCV)
2. **Predicts** its flight path (Kalman filter + LSTM)
3. **Steers** a laser beam to track it using a 2-axis galvanometer mirror

No motors rotate the turret — the laser stays fixed and the beam is steered by near-massless mirrors, so there's no rotational inertia to slow it down.

## How it works

```
Camera → OpenCV detection → Kalman/LSTM prediction → Galvo-steered laser
                                                   ↑
                                    ESP32 (I/O bridge: DAC + laser control)
```

- **Perception & prediction** run on a laptop (Python, OpenCV, PyTorch).
- **Actuation** runs through an ESP32, which drives the galvo mirrors (via a DAC) and the laser.
- A real-time polar radar UI shows tracked balls live.

## Build phases

| Phase | What | Status |
|---|---|---|
| 1 | Camera + OpenCV detection + live radar UI | Not started |
| 2 | Kalman filter + LSTM trajectory prediction | Not started |
| 3 | Galvo + laser + coordinate calibration | Not started |
| 4 | Full integration + demo video | Not started |

## Parts

| Subsystem | Part | Status |
|---|---|---|
| Perception | Global-shutter USB camera | To buy |
| Compute | Laptop | Have |
| Control | ESP32 DevKit v1 | Have |
| Actuation | 2-axis galvanometer + driver | To buy |
| Actuation | MCP4922 dual DAC | To buy |
| Actuation | Galvo power supply (±12–24V) | TBD |
| Intercept | 5mW green laser | To buy |
| Safety | Laser-safety glasses | To buy |
| PCBs | Galvo-driver board + laser-control board (OnBoard) | To design |

## Safety

The laser is Class 3R (5mW). It is an eye hazard on direct or reflected beam. Wavelength-matched safety glasses are worn whenever it is powered, and it is never aimed at people.

## Hack Club programs

- **Highway** — $350 hardware grant (deadline July 31, 2026)
- **OnBoard** — $100 PCB grant (rolling) — two custom KiCad boards
- **Stardance** — hours-logged prizes (deadline Sept 30, 2026)

## License

MIT — see [LICENSE](LICENSE).
