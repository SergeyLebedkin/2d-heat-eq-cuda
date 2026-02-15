# heat2d_cuda_sdl

A tiny **2D explicit heat-equation** demo (radius-4 / 8th-order Laplacian) on **CUDA** with live **SDL2** visualization.

It simulates:

\[
u_t = \kappa \nabla^2 u + s(x,y)\,g(t)
\]

with a short source injection at the center (or user-chosen point).

## Features
- CUDA double-buffered time stepping (explicit Euler)
- 8th-order (radius-4) Laplacian per axis: `u_xx + u_yy`
- SDL2 window showing a grayscale heatmap (auto contrast)
- Simple CLI

## Build (Linux)
Prereqs:
- CUDA toolkit (nvcc)
- SDL2 dev package

Ubuntu/Debian:
```bash
sudo apt-get update
sudo apt-get install -y libsdl2-dev
```

Build:
```bash
cmake -S . -B build -DCMAKE_BUILD_TYPE=Release
cmake --build build -j
```

Run:
```bash
./build/heat2d --nx 512 --ny 512 --steps 200000 --fps 60 --dump-every 5
```

Precise per-step kernel timing (CUDA events, synchronizes each step):
```bash
./build/heat2d --timing --timing-print-every 200
```

## CLI
```text
--nx N            grid size X (default 512)
--ny N            grid size Y (default 512)
--dx X            spacing (default 1.0)
--dy Y            spacing (default 1.0)
--dt T            time step (default 0.05)
--kappa K         diffusivity (default 1.0)
--steps S         number of steps (default 200000)
--dump-every D    render every D steps (default 5)
--fps F           cap render FPS (default 60)
--src-x i         source x index (default nx/2)
--src-y j         source y index (default ny/2)
--src-steps P     inject for P steps (default 20)
--src-amp A       per-step additive amplitude (default 5.0)
--no-vis          run without SDL window (still computes)
--ppm-out file    (optional) write a final PPM image
--timing          measure kernel execution time each step (CUDA events)
--timing-print-every N
                 print running timing stats every N steps (default 200)
```

## Notes
- Boundaries use **Dirichlet** `u=0` (we skip updating a 4-cell border).
- Explicit diffusion stability depends on `dt`, `kappa`, and grid spacing. Start conservative.

# 2d-heat-eq-cuda
