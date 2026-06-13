---
title: "Aegis Autonomous Defense System"
author: "your-slack-username"
description: "Autonomous laser turret that tracks thrown balls using a camera, Kalman filter, LSTM prediction, and a galvo-steered beam."
created_at: "2026-06-09"
total_time_spent: "3h"
---

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

*(add your sketch photo here — drag image into GitHub or use ![sketch](images/sketch.jpg))*

**Total time spent: 2h**

---

## 2026-06-11 — CAD model + mechanical design

**Built:**
- Wedge base + box enclosure model in Onshape
- 30° tilt angle locked in — covers full room volume from desk height without oversteering toward the floor
- Open front face for galvo mirrors and laser beam exit — no aperture cutout needed
- Lip on back edge of wedge to prevent box sliding, two screw holes to lock it down
- Camera mount on top front edge, angled forward with the box tilt

**Decided:**
- No tripod — wedge base sits directly on desk. Lower center of gravity, more stable for a demo, simpler to build.
- 30° is the right angle for desk height — covers low throws, high throws, and arcing balls across the room
- Fixed tilt for now, adjustable later if needed (locking hinge upgrade path exists)
- Camera FOV constraint accepted for v1 — balls thrown from in front of the unit only. 360° coverage is a future improvement.

*(add your Onshape screenshot here — ![CAD model](<img width="1194" height="709" alt="image" src="https://github.com/user-attachments/assets/55a5f2f5-d663-48c2-b3f9-f3f7fa831b80" />
))*

**Next:**
- [ ] Export .STEP from Onshape and add to repo root
- [ ] Add screenshot to images/ folder and link in this entry
- [ ] Start KiCad schematic for galvo-driver/DAC + op-amp board
- [ ] Write firmware skeleton for ESP32

**Total time spent: 1h**

---
