title: "Aegis Autonomous Defense System"
author: "rajolisathvik"
description: "Autonomous laser turret that tracks thrown balls using a camera, Kalman filter, LSTM prediction, and a galvo-steered beam."
created_at: "2026-06-9

# Build Journal — Aegis

Newest entries at the top. Each entry: what I did, what broke, what I decided and why.
This log is the evidence trail for Stardance hours and the story for Highway — write it as you work, not after.

---

## 2026-06-09 — Project pivot + planning

**Decided:**
- Dropped the 360°-rotating gearbox turret. Problem: rotational inertia — a heavy turret on a
  gearbox can't slew fast enough to track a ball thrown from an arbitrary direction. By the time it
  rotates, the ball has arrived.
- Replaced it with a **fixed laser + 2-axis galvanometer mirror**. The beam is steered by
  near-massless mirrors, so repositioning is effectively instant. This removes the biggest
  mechanical risk in the project.
- Demo goal is **visible tracking** (beam lands on and follows the moving ball), not physical
  interception. Achievable at 5mW; physical interception is not.

**Detection rethink:**
- 3× TF-Luna LiDAR can't work for this. They're single-point rangefinders with a 2° beam — three
  thin "tripwires" in space. A ball thrown from any direction almost always passes through the gaps,
  and a single distance reading can't give a 3D trajectory anyway.
- Switching to a **single camera + OpenCV blob detection**. One camera sees the whole room volume
  and gives a position every frame — exactly what the Kalman/LSTM stack needs.

**Architecture principle locked in:**
- Tight tracking loop stays local (camera → detection → prediction → galvo). Keep the slow stuff
  (UI, telemetry, any heavy ML) off the hot path so it never blocks the beam.
- Considering running the core Kalman tracker on the ESP32 to avoid a network round-trip during
  tracking, with the laptop/LSTM as an enhancement layer.

**Next:**
- [ ] Set up GitHub repo with README + this journal + MIT license
- [ ] Confirm whether TF-Lunas were already purchased (decides budget vs. repurpose)
- [ ] Buy the camera (decide: global-shutter now vs. basic webcam first)
- [ ] Phase 1: camera capture + OpenCV blob detection + position stream

**Open questions:**
- Frame rate / shutter type actually needed for my ball speeds and room size?
- Single-camera 3D position from apparent ball size — accurate enough, or go stereo?
- Galvo driver voltage/current — what supply to buy?

---

<!-- Template for new entries — copy this:

## YYYY-MM-DD — short title

**Did:**
-

**Broke / learned:**
-

**Decided:**
-

**Next:**
- [ ]

-->
