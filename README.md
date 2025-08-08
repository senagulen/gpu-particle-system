# GPU Particle System — WebGL2 (Transform Feedback)

**Live demo:** https://senagulen.github.io/gpu-particle-system/webgl2/  
**Video:** [assets/demo.mp4](assets/demo.mp4) (or YouTube link)

A real-time particle simulation where **position/velocity are updated on the GPU** via WebGL2 **Transform Feedback** (ping-pong buffers). No per-frame CPU uploads. Visual polish includes **velocity-based color**, **trails** (decay buffer), and **bloom** (bright-pass + blur). Runs smoothly on Intel Arc integrated/discrete GPUs.

## Highlights
- **GPU-resident sim:** TF ping-pong of `vec2 pos`, `vec2 vel` (no per-frame `bufferSubData`).
- **Effects:** trails buffer with decay, bloom post-process, additive blending.
- **Scales in real time:** adjust particle count live; FPS overlay for perf.

## What changed vs. a basic CPU particle demo
- Moved update step **from CPU → GPU** using Transform Feedback.
- Switched from per-frame CPU uploads to **persistent GPU buffers** (preallocated).
- Added **post-processing** (bloom) and **temporal trails** for clarity + style.
- Tuned **DPR scaling** and avoided buffer reallocations for smooth FPS.

## Architecture (short)
- `pos/vel` live in two GPU buffers (A and B).  
- **Pass 1 (update):** Vertex shader reads A, writes B via TF (forces, mouse, modes).  
- **Pass 2 (render):** Draw points from B with additive blending & color by speed.  
- **Ping-pong:** swap A↔B each frame.  
- **Trails:** offscreen buffer with slight decay each frame; composite to main.  
- **Bloom:** threshold → blur → composite.

## Performance (Intel Arc A370M @ 1920×1080, Chrome)
> Replace the numbers with your measurements below.

| Particles | FPS (Trails Off, Bloom Off) | FPS (Trails On, Bloom On) |
|---------:|------------------------------:|---------------------------:|
| 10,000   | **[e.g., 120]**              | **[e.g., 100]**            |
| 20,000   | **[   ]**                    | **[   ]**                  |
| 30,000   | **[   ]**                    | **[   ]**                  |

**How I measured:** window at 100% zoom, fullscreen; waited ~5 seconds per setting; recorded the overlay FPS.  
**System:** Intel Arc A370M, 12th Gen Intel(R) Core(TM) i7-12700H (2.30 GHz), Windows 11, Chrome 139.0.7258.66/67.

## Controls
- **Modes:** Default, Rain, Snow, Fireworks, Galaxy  
- **Sliders:** Particle Count, Speed, Mouse Effect, Point Size  
- **Mouse:** move = attract, click = fireworks impulse

## Run locally
Open `webgl2/index.html` in Chrome/Edge/Firefox.

## Keywords
WebGL2, Transform Feedback, Ping-Pong Buffers, GPU-Resident Simulation, Additive Blending, Trails, Bloom, Intel Arc A370M
