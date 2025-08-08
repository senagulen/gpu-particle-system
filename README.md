# GPU Particle System — WebGL2 (Transform Feedback)

**Live Demo:** [Open Here](https://senagulen.github.io/gpu-particle-system/webgl2/)  
**Video:** [assets/demo.mp4](assets/demo.mp4)

A real-time particle simulation where **position and velocity are updated fully on the GPU** using WebGL2 **Transform Feedback** (ping-pong buffers).  
No per-frame CPU uploads — all physics and rendering remain GPU-resident.  
Visual polish includes **velocity-based color**, **temporal trails**, and **bloom post-processing**.

---

## **Development Journey**

**Phase 1 — CPU → GPU Migration**
- Started from a basic CPU particle loop with per-frame buffer uploads.
- Migrated all update logic to the GPU using Transform Feedback (two `vec2` attributes: position, velocity).
- Implemented a **ping-pong buffer** swap each frame to avoid overwriting data mid-update.

**Phase 2 — Visual & Interaction Enhancements**
- Added **velocity-based coloring** to give motion a visible gradient.
- Implemented **temporal trails**: particles leave a fading path via an offscreen framebuffer with decay.
- Integrated **bloom**: threshold bright particles → blur → composite.

**Phase 3 — Modes & Controls**
- **Default** – particles drift with gentle motion.
- **Rain** – constant downward velocity, randomized wind.
- **Snow** – slow descent, horizontal sway.
- **Fireworks** – mouse click spawns radial burst.
- **Galaxy** – orbiting spiral formation with rotational velocity.
- Added live controls (particle count, speed, mouse influence, point size).

**Phase 4 — Performance Tuning**
- Preallocated buffers to avoid GC spikes.
- Enabled **devicePixelRatio scaling** for sharper rendering on high-DPI screens.
- Reduced state changes by batching VAO binds and minimizing WebGL calls.
- Confirmed no per-frame `bufferSubData` calls — only GPU reads/writes.

---

## **Performance (Intel Arc A370M @ 1920×1080, Chrome)**

| Particles | FPS (Trails Off, Bloom Off) | FPS (Trails On, Bloom On) |
|---------:|------------------------------:|---------------------------:|
| 10,000   | **120**                       | **115–119**                |
| 20,000   | **120**                       | **115–119**                |
| 30,000   | **120**                       | **115–119**                |

**How measured:** Fullscreen, 100% zoom, ~5 seconds stable average.  
**System:** Intel Arc A370M, 12th Gen Intel(R) Core(TM) i7-12700H (2.30 GHz), Windows 11, Chrome 139.

---

## **Architecture Overview**
1. **Update Pass (Transform Feedback)**  
   - Read positions/velocities from Buffer A.  
   - Apply forces based on mode, mouse position, and speed settings.  
   - Write results to Buffer B.

2. **Render Pass**  
   - Render points from Buffer B using instanced drawing.  
   - Apply velocity-based color in fragment shader.

3. **Ping-Pong**  
   - Swap A ↔ B each frame.

4. **Trails + Bloom**  
   - Trails: Offscreen framebuffer with decay factor each frame.  
   - Bloom: Bright-pass → Gaussian blur → composite.

---

## **Controls**
- **Modes:** Default, Rain, Snow, Fireworks, Galaxy  
- **Sliders:** Particle Count, Speed, Mouse Effect, Point Size  
- **Mouse:** Move to attract, click for fireworks burst

---

## **Run Locally**
Clone the repo and open `webgl2/index.html` in Chrome/Edge/Firefox.

---

## **Keywords**
WebGL2, Transform Feedback, Ping-Pong Buffers, GPU-Resident Simulation, Additive Blending, Trails, Bloom, Intel Arc A370M

