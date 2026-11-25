# Eikonal Equation & Ray Tracing in Python

Two independent ways to model how light (or sound) bends through a medium whose
refractive index changes from place to place — tracing individual rays, and solving
for whole wavefronts on a grid — checked against each other and against problems
with known exact answers.

<!-- FIGURE: hero image — rays (lines) overlaid on eikonal wavefront contours, e.g. the Luneburg lens -->

**Built with:** Python, NumPy, SciPy, Matplotlib, Jupyter
**Background:** started in PHYS 440 Computational Physics (SFSU, Spring 2026); methods
follow Mark Newman, *Computational Physics*, Chapters 5, 6, 8 and 9.

---

## Contents
1. [The physics](#the-physics)
2. [Methods](#methods)
3. [Validation](#validation)
4. [Benchmarks](#benchmarks)
5. [Results](#results)
6. [How to run](#how-to-run)
7. [Repository structure](#repository-structure)
8. [What I learned / next steps](#what-i-learned--next-steps)
9. [References](#references)

---

## The physics
<!-- 3–5 sentences in your own words:
- Fermat's principle: light takes the path of stationary optical path length, ∫ n ds
- Ray picture (Lagrangian): follow individual rays with the ray equation
- Wavefront picture (Eulerian): solve |∇T| = n/c for arrival time T everywhere at once
- Why both: rays are intuitive but cross and leave gaps; the eikonal gives one clean
  first-arrival time everywhere -->

Key equations:

- Ray equation, reparametrized with dτ = ds/n: `d²r/dτ² = ∇(n²/2)`
- Eikonal equation: `|∇T| = n / c`

## Methods

### Ray tracing (ODE approach)
<!-- State vector [x, y, vx, vy]; RK4 (Newman 8.3); ∇n by central differences
(Newman 5.10) or analytically; launch condition |v| = n at the source.
Shooting method: bisection or secant on the launch angle (Newman 8.6, 6.3). -->

### Eikonal solver (grid PDE approach)
<!-- Godunov upwind differences (why central differences fail at kinks);
closed-form local update; fast sweeping = Gauss–Seidel in 4 alternating
orders (Newman 9.1–9.2); convergence tolerance. -->

## Validation
<!-- The section that shows the code is trustworthy. Fill in with YOUR numbers. -->

| Test | What it checks | Expected | My result |
|---|---|---|---|
| Ray invariant \|v\|² − n² | Integrator accuracy | Error drops ~16× when Δτ is halved (4th order) | _TBD_ |
| Uniform-medium eikonal | Solver vs. exact distance | Error ∝ h (first order) | _TBD_ |
| Rays vs. −∇T | Two methods agree | Ray paths overlap | _TBD_ |
| Shooting method *(optional)* | Hitting a target | Miss distance → 0 | _TBD_ |

<!-- FIGURES: convergence plot (log-log), eikonal error vs h -->

## Benchmarks

### Luneburg lens — optics, exact answer ✅
Index profile `n(r) = √(2 − (r/R)²)` inside the lens, `n = 1` outside.
Parallel rays entering the lens should all focus at **(R, 0)**, and all should have the
**same optical path length, R(1 + π/2)**, from the plane x = −R to the focus.

| Check | Expected | My result |
|---|---|---|
| Max focal miss \|y\| at x = R | → 0 as Δτ shrinks | _TBD_ |
| Spread of optical path lengths across rays | → 0 | _TBD_ |

<!-- Note: ∇n jumps at r = R, so convergence degrades if a big step crosses the
edge — mention how you handled it (small Δτ, or a smooth tanh edge). -->

### SOFAR ocean channel — *planned*
Munk sound-speed profile; ray fan, caustics, and eigenrays to a receiver 50 km away
using the shooting method. Connects to my oceanography background (A.S., CCSF).

### Salt dome — *planned*
High-contrast seismic velocity model; ray shadow zone vs. eikonal first arrivals
filling the domain. Connects to my geology background.

## Results
<!-- 2–4 figures with one-sentence captions. Lead with the most striking one. Keep Figure descriptions short and concise. -->

## How to run
```bash
git clone https://github.com/lesliebogen/eikonal-ray-tracing.git
cd eikonal-ray-tracing
conda env create -f environment.yml    # or: pip install -r requirements.txt
conda activate eikonal
jupyter lab
```
Then run the notebooks in order (01 → 04).

## Repository structure
```
eikonal-ray-tracing/
├── README.md
├── environment.yml
├── src/
│   ├── media.py       # refractive index / velocity models
│   ├── rays.py        # RK4 ray tracer + shooting method
│   └── eikonal.py     # fast-sweeping eikonal solver
├── notebooks/
│   ├── 01_ray_tracing_grin.ipynb
│   ├── 02_shooting_method.ipynb
│   ├── 03_eikonal_validation.ipynb
│   ├── 04_rays_from_eikonal.ipynb
│   └── 05_luneburg_lens.ipynb
└── figures/
```

## What I learned / next steps
<!-- bullets, e.g. why upwinding matters, step size near index
discontinuities, speed (pure-Python sweeps are slow → Numba next),
SOFAR and salt-dome benchmarks, extending to 3D. -->

## References
- M. Newman, *Computational Physics* (Ch. 5, 6, 8, 9)
- R. K. Luneburg, *Mathematical Theory of Optics* (1964)
- W. H. Munk, "Sound channel in an exponentially stratified ocean," *J. Acoust. Soc. Am.* 55, 220 (1974)
- H. Zhao, "A fast sweeping method for eikonal equations," *Math. Comp.* 74, 603 (2005)

