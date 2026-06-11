# Aegis: Autonomous Laser Defense System

> A turret that watches a room, spots a ball thrown from any direction, predicts where it's going, and tracks it with a steered laser beam, without any human input. 

**Hack Club Highway project** · Deadline: July 31, 2026

---

## What it does



1. **Detects** the ball in each camera frame (OpenCV)
2. **Predicts** its flight path (Kalman filter + LSTM)
3. **Steers** a laser beam to track it using a 2-axis galvanometer mirror

The laser stays fixed and the beam is steered by near-massless mirrors, to prevent being slowed down to rotational inertia. 

## Why I built this

I watched some footage of the Iron Dome, and tried to replicate a scaled down version. I was really interested in its ability to detect a fast moving object from any axis, and wanted to understand how it worked

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

The laser is 5mW, which is quite the hazard on the unprotected eye. Safety glasses to the matched wavelenght are worn while the laser in use. 


## License

MIT — see [LICENSE](LICENSE).
