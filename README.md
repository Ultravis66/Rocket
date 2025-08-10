## Transient Rocket Nozzle vs. Steady-State

**The transient run plateaus around **0.70 MN** while the steady-state solution predicts **~1.10 MN**.  
That gap is expected during startup because the nozzle flow hasn’t fully “settled” to the design exit conditions.

**Thrust definition**

## Geometry

**Inspiration:** Based on the “Design and analysis of morphing rocket nozzle for thrust vector control” concept on GrabCAD, but **morphing/TVC was intentionally removed** for this exercise. The model here is a **fixed, 2D axisymmetric bell nozzle** sized for clarity and repeatability.  
Reference: https://grabcad.com/library/design-and-analysis-of-morphing-rocket-nozzle-for-thrust-vector-control-1

**Coordinate system:** Axisymmetric (x along centerline, r radial). Throat is x = 0.

### Simplifications vs. the morphing concept
- **2D axisymmetric**: captures startup physics (shock/expansion pattern, BL growth, plume development) at low cost.

### Mesh note
- Throat and immediate downstream wall (BL resolution),
- Lip and external shear layer (startup entrainment),
- Early plume shock cell region (gradient capturing).


### Species breakdown (what the “Species (Air)” field shows)
- **Inside the nozzle:** Air mass fraction ≈ 0 (combustion products dominate in the model).  
- **Plume mixing region:** Air fraction rises from 0 → 1 with radius; a thicker mixed layer early in time = more entrainment = lower thrust.  
- **Cold core note:** The centerline can look “cold” (e.g., ~270 K) right after the exit due to rapid expansion.

### Exhaust Species Breakdown (mass fraction) RP-1/LOX fuel

| Species           | Formula | Fraction | Percentage |
|-------------------|:------:|--------:|-----------:|
| Water             | H₂O    | 0.291   | 29.1%      |
| Carbon Dioxide    | CO₂    | 0.227   | 22.7%      |
| Carbon Monoxide   | CO     | 0.056   | 5.6%       |
| Hydrogen          | H₂     | 0.048   | 4.8%       |
| Oxygen            | O₂     | 0.007   | 0.7%       |
| Nitrogen          | N₂     | 0.371   | 37.1%      |
| **Total**         | —      | **1.000** | **100.0%** |

---

### Mesh & pressure ramp
**Pressure ramp used in the transient:**

![Pressure ramp](PressureRamp.png)

**2D axisymmetric mesh:**

![Mesh](Mesh.png)

Key settings: Δt = 1e−6 s, total simulated time = 0.025 s, implicit transient, ~25 inner iterations/step.

---

1. **Run past the ramp:** Continue until \(\dot m(t)\) and \(p_e(t)\) flatten for several ms; then time-average thrust.
2. **Compute thrust at the exit plane:** Use area-weighted \(\dot m\), \(V_e\), and \(p_e\); subtract \(p_\infty A_e\). Avoid “net wall force only” during transients.
3. **Smooth the signal:** Moving average over 1–5× the acoustic transit time of the nozzle to remove shock flapping.
4. **Domain & outlet:** Ensure far-field is large and non-reflecting; \(p_\infty\) in transient matches the steady case.

**Expectation:** Once the ramp completes and the exit plane stabilizes, the transient time-average should rise toward the steady ≈1.1 MN (within modeling error). If it doesn’t, the exit is likely still over/under-expanded or the averaging window is too short.


### Mach
<video controls muted playsinline loop style="max-width:100%;height:auto;">
  <source src="Media/Mach.mp4" type="video/mp4">
</video>

### Species (Air)
<video controls muted playsinline loop style="max-width:100%;height:auto;">
  <source src="Media/SpeciesAir.mp4" type="video/mp4">
</video>

### Temperature
<video controls muted playsinline loop style="max-width:100%;height:auto;">
  <source src="Media/Temp.mp4" type="video/mp4">
</video>

### Velocity
<video controls muted playsinline loop style="max-width:100%;height:auto;">
  <source src="Media/Velocity.mp4" type="video/mp4">
</video>

> **Having trouble playing the videos on GitHub?**  
> Watch the demo on Reddit:  
> Videos available on request.
[Watch the video on Reddit »](https://www.reddit.com/r/CFD/comments/1mm4p5g/transient_rocket_simulation/?new_reddit=true)

