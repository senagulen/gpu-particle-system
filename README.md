# GPU Particle System — WebGL2 (Transform Feedback)

**Live Demo:** [Open Here](https://senagulen.github.io/gpu-particle-system/webgl2/)  
**Video demo:** [Watch on YouTube](https://www.youtube.com/watch?v=Thkd88IJLIE)

A real-time particle simulation where **position and velocity are updated fully on the GPU** using WebGL2 **Transform Feedback** (ping-pong buffers).  
No per-frame CPU uploads — all physics and rendering remain GPU-resident.  
Visual polish includes **velocity-based color**, **temporal trails**, and **bloom post-processing**.


**Tech Stack:** JavaScript (ES6), WebGL2, GLSL, HTML/CSS

---

## Development Journey

**Phase 1 — CPU → GPU Migration**
- Started from a basic CPU particle loop with per-frame buffer uploads.
- Migrated all update logic to the GPU using Transform Feedback (two `vec2` attributes: position, velocity).
- Implemented a **ping-pong buffer** swap each frame to avoid overwriting data mid-update.

**Phase 2 — Visual & Interaction Enhancements**
- **Velocity-based color** to visualize motion magnitude.
- **Temporal trails** via an offscreen framebuffer with decay.
- **Bloom** post-process: bright-pass → separable blur → composite.

**Phase 3 — Modes & Controls**
- **Default** – gentle swirl with mouse attraction.
- **Rain** – downward drift with subtle horizontal sway.
- **Snow** – slow descent with more pronounced horizontal oscillation.
- **Fireworks** – click to spawn a radial burst (impulse velocities).
- **Galaxy** – central attraction + tangential velocity for orbital motion.
- Live controls: particle count, simulation speed, mouse influence, point size.

**Phase 4 — Performance Tuning**
- **Preallocated GPU buffers**; no per-frame `bufferSubData`.
- **DPR-aware rendering** for crisp visuals without overdraw.
- Minimized state churn (buffer/VAO reuse) and batched work.

---

## Performance (Intel Arc A370M @ 1920×1080, Chrome)

| Particles | FPS (Trails Off, Bloom Off) | FPS (Trails On, Bloom On) |
|---------:|------------------------------:|---------------------------:|
| 10,000   | **120**                       | **115–119**                |
| 20,000   | **120**                       | **115–119**                |
| 30,000   | **120**                       | **115–119**                |

**Frame time (derived):** ~**8.33 ms** at 120 FPS; **~8.7–8.4 ms** with bloom/trails.  
**How measured:** Fullscreen, 100% zoom, ~5s stable average via on-screen FPS counter.  
**System:** Intel Arc A370M, 12th Gen Intel(R) Core(TM) i7-12700H (2.30 GHz), Windows 11, Chrome 139.

---

## Architecture Overview
1. **Update Pass (Transform Feedback)**  
   Read positions/velocities from Buffer A → apply mode + mouse forces → write to Buffer B.
2. **Render Pass**  
   Draw points from Buffer B with additive blending; color by velocity magnitude.
3. **Ping-Pong**  
   Swap A ↔ B every frame (no CPU round-trip).
4. **Trails + Bloom**  
   Trails: offscreen buffer with per-frame decay.  
   Bloom: threshold → separable blur → composite.

---

## Controls
- **Modes:** Default, Rain, Snow, Fireworks, Galaxy  
- **Sliders:** Particle Count, Speed, Mouse Effect, Point Size  
- **Mouse:** Move to attract, click for fireworks burst

---

## Run Locally
Clone the repo and open `webgl2/index.html` in Chrome/Edge/Firefox.

---

## Keywords
WebGL2, Transform Feedback, Ping-Pong Buffers, GPU-Resident Simulation, Additive Blending, Trails, Bloom, Intel Arc A370M



