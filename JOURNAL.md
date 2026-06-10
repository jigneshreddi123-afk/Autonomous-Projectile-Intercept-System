# Build Journal — Aegis



---

## 2026-06-09 — Project pivot + planning

**Decided:**
- Dropped the 360°-rotating gearbox turret. A heavy turret on a gearbox has too much rotational intertia,
  which would slow it down and make it unable to track incoming balls. 
- Replaced it with a **fixed laser + 2-axis galvanometer mirror**. The beam is steered by
  near-massless mirrors, so repositioning is almost instant. 
- Demo goal is **visible tracking** (beam lands on and follows the moving ball), not physical
  interception. 

**Detection rethink:**
- 3× TF-Luna LiDAR can't work for this. They're single-point rangefinders with a 2° beam, which act
  as thin tripwires in space. A ball thrown from any direction almost always is going to pass through
  the gaps.
- Switching to a **single camera + OpenCV blob detection**. One camera sees the whole room volume
  and gives a position every frame , which aligns with the Kalman/LSTM stack.

**Architecture principle locked in:**
- Tight tracking loop stays local (camera → detection → prediction → galvo). Keep the slow stuff
  (UI, telemetry, any heavy ML) off the hot path so it never blocks the beam.
- Considering running the core Kalman tracker on the ESP32 

**Still thinking:**
- Frame rate / shutter type actually needed for my ball speeds and room size?
- Single-camera 3D position from apparent ball size — accurate enough, or go stereo?
- Galvo driver voltage/current — what supply to buy?

---




