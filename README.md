# Structural Verification of a 1 Tonne Mobile Floor Jib Crane

<!-- ============================================================= -->
<!-- HEADER IMAGE                                                   -->
<!-- Source: JIB_CRANE\JIB_CRANE_C1_v3\verification_outputs_20260613_145515\fig_01_Free_Body_Geometry.png -->
<!-- Repo path: figures/wheels/fig_01_Free_Body_Geometry.png        -->
<!-- ============================================================= -->

<!-- ![1 Tonne Mobile Floor Jib Crane](figures/wheels/fig_01_Free_Body_Geometry.png) -->

> _Attach header image above — from `JIB_CRANE_C1_v3\verification_outputs_20260613_145515\fig_01_Free_Body_Geometry.png` (or a product render)._

Independent structural verification of a 1 tonne mobile floor jib crane against its rated load chart, built on a single parametric MATLAB script that runs two independent methods in parallel: closed-form hand calculation to Eurocode 3 and EN 13001, and a from-scratch two-dimensional frame finite element solver. The study covers the crane on its wheels, the crane on deployed outriggers, a theoretical elastic-versus-plastic capacity check of the governing column, and a separate re-rating of the chart to what the structure safely carries.

The project combines:

- MATLAB R2026a (parametric script, hand calc plus in-script 2D frame FEA)
- Closed-form member checks to EN 1993-1-1, EN 1993-1-8 and EN 13001-3-1
- Plane-frame Euler-Bernoulli beam solver written from scratch
- Elastic working-stress and plastic limit-state assessment
- Stability (overturning) analysis in both wheels-only and deployed states
- Hydraulic pressure cross-check on the luffing cylinder
- A material reassessment from nominal S275 to client-confirmed Q235

to answer one question for the client: can the structure safely support its rated 1 tonne chart, and with what margin.

---

## Where the Figures Come From

All figures are produced by the MATLAB scripts into a dated `verification_outputs_*` subfolder inside each script-version folder. The **wheels-only** run and the **deployed-outrigger** run write the same filenames (`fig_01` … `fig_10`, `crane_animation`), so they are kept apart in this repository.

| Repo folder | Source folder \ subfolder | Used for |
|---|---|---|
| `figures/wheels/` | `JIB_CRANE_C1_v3\verification_outputs_20260613_145515\` | Wheels-only and combined-verification sections |
| `figures/deployed/` | `JIB_CRANE_C2_v3\verification_outputs_20260613_150046\` | Deployed-outrigger sections |

Copy the figures from those two source subfolders into `figures/wheels/` and `figures/deployed/` respectively, then uncomment the `![ ]( )` line under each image slot. The full file-by-file map is in the [Figure Index](#figure-index) at the end.

> Note on filenames: the latest runs (C1_v3 and C2_v3) name `fig_02 = Boom_BMD_Track_1` and `fig_03 = Track_2_Deformed_Shape`. The earliest wheels run (`JIB_CRANE_C1\verification_outputs_20260527_200459`) had those two swapped (`fig_02 = Track_2_Deformed_Shape`, `fig_03 = Boom_BMD_Track_1`); use the C1_v3 run to match this README.

---

## Project Objectives

- Verify every load path on the rated chart against agreed factors of safety
- Identify which chart point governs, and which members and connections drive the result
- Cross-check the hand calculation against an independent in-script frame model
- Set out the theoretical elastic-versus-plastic capacity of the governing column
- Assess stability in both the wheels-only and deployed-outrigger configurations
- Carry the confirmed Q235 material through every stress-based check
- State plainly whether the crane can be certified to the chart, and set out the routes forward, including a re-rated chart

---

## Software

| Software | Purpose |
|---|---|
| MATLAB R2026a | Parametric script driving every number in the reports |
| Track 1 (hand calc) | Closed-form member, pin, weld, bolt and stability checks |
| Track 2 (frame FEA) | Plane-frame beam solver for nodal displacements and member stress |
| Microsoft Word | Multi-revision verification reports |

---

## Configuration and Scope

| Field | Detail |
|---|---|
| Project reference | 40467361 |
| Rated capacity (as marked) | 1000 kg at 1400 mm reach |
| Configurations assessed | Outriggers stowed (wheels only) and outriggers deployed (# arrangement) |
| Material (current) | Q235 carbon structural steel (fy 235 MPa), client-confirmed |
| Material (prior basis) | S275 (fy 275 MPa), nominal |
| Scope of physics | Static lifting only |
| Dynamic factor | 1.25 (HC1 hoist duty, EN 13001) |
| Wheelbase | 2360 mm |
| Frame track (rail to rail) | 876 mm |
| Status | Independent verification, issued for review; not a design certificate |

This is a desk check of an existing unit against its rated chart, read directly off the manufacturer drawing set (Assembly, Base, Arm Section 1, Arm Section 2). Wind, seismic action, side pull and snatch loading are outside scope. It is a theoretical calculation exercise; physical load testing and the competent-person examination are carried out separately with the approval officer before the crane is put to work.

---

## Materials, Sections and Allowable Stresses

The crane material was originally analysed on nominal grade S275 (fy 275 MPa). The client subsequently confirmed the material as Q235 (Chinese carbon structural steel, GB/T 700), nominal yield 235 MPa, ultimate 370 to 500 MPa (the minimum 370 MPa is used for weld checks). Q235 is equivalent to ASTM A36 and EN S235JR. All sections are 6 mm wall, well under 16 mm, so the full nominal yield applies with no thickness de-rating.

The roughly 15 percent drop in yield from S275 to Q235 scales down every stress-based factor of safety, because the applied stresses do not change.

### Allowable stresses, S275 (prior basis)

```
Member bending / direct stress (target FoS 1.5):
    sigma_allow = fy / 1.50 = 275 / 1.50 = 183.3 MPa
Pin / bolt shear (target FoS 3.0):
    tau_allow = 0.6 x fy / 3.0 = 55.0 MPa
Fillet weld (on throat):
    tau_allow,weld = 0.7 x fu / 1.5   (fu = 430 MPa)
```

### Allowable stresses, Q235 (current basis)

```
Member bending / direct stress (target FoS 1.5):
    sigma_allow = fy / 1.50 = 235 / 1.50 = 156.7 MPa
Pin / bolt shear (target FoS 3.0):
    tau_allow = 0.6 x fy / 3.0 = 47.0 MPa
Weld (a = 6 mm, target FoS 1.5):
    fw_allow = 0.7 x fu / 1.50 = 0.7 x 370 / 1.50 = 172.7 MPa
```

### Section Properties

| Member | Section (RHS) | A (cm²) | Ix (cm⁴) | Sx (cm³) | Zx (cm³) |
|---|---|---|---|---|---|
| Boom outer (root) | 100 x 140 x 6 | 27.4 | 748.8 | 107.0 | - |
| Boom middle | 80 x 120 x 6 | 22.6 | 438.2 | 73.0 | - |
| Boom inner (tip) | 60 x 100 x 6 | 17.8 | 227.4 | 45.5 | - |
| Column | 80 x 120 x 6 | 22.6 | 438.2 | 73.0 | 89.7 |
| Base frame / outrigger / leg | 60 x 100 x 6 | 17.8 | 227.4 | 45.5 | - |

Worked example, outer boom 100 x 140 x 6:

```
A  = B*H - (B-2t)*(H-2t) = 140*100 - 128*88 = 2736 mm^2 ~ 27.4 cm^2
Ix = [B*H^3 - (B-2t)*(H-2t)^3] / 12 ~ 748.8 cm^4
Sx = Ix / (H/2) = 748.8 / 7.0 = 107.0 cm^3
```

The plastic section modulus Zx is added for the column to support the elastic-versus-plastic capacity check.

---

## Methodology

Two independent methods are run against the same geometry so the results can be cross-checked. Both read their inputs from one parametric block, so any change to a section size, material grade or weld throat flows through every check at once.

### Track 1: closed-form hand calculation

Each member is reduced to its governing internal action and checked by hand. Bending uses the elastic section modulus, axial load uses the gross area, and the two combine linearly for the column. Pins are checked in double or single shear and in bearing. Welds are checked on the throat area. Stability is a moment balance about the tipping line.

### Track 2: in-script frame finite element check

The crane is modelled as a plane frame of Euler-Bernoulli beam elements. Each element carries a 6 by 6 stiffness matrix in local coordinates, rotated into the global frame and assembled into the system stiffness. The front wheel is pinned and the rear wheel is a roller. Solving the free degrees of freedom gives the nodal displacements, from which element moments and stresses follow. The frame is re-solved for all three chart points because the boom extends to a different length at each.

A side-view 2D frame sees only one base beam, so the base and outrigger elements use twice the area and twice the second moment of area to lump both side rails into one equivalent beam; without this the model would overstate base-frame stress by a factor of two. Crane self-weight is applied at the column-base node.

---

## Load Model and Governing Case

The manufacturer chart gives three rated points, with capacity falling as reach increases. A dynamic amplification factor of 1.25 is applied to the static load, consistent with HC1 hoist duty under EN 13001.

| Chart point | Reach (mm) | Rated load (kg) | Static load (kN) | Factored load W (kN) |
|---|---|---|---|---|
| Close | 1400 | 1000 | 9.81 | 12.26 |
| Mid | 2320 | 700 | 6.87 | 8.58 |
| Far | 3300 | 300 | 2.94 | 3.68 |

The design moment at the column is the factored load times its reach:

```
Close 1000 kg: M = 12.26 kN x 1.400 m = 17.17 kN.m
Mid    700 kg: M =  8.58 kN x 2.320 m = 19.91 kN.m   <-- governs
Far    300 kg: M =  3.68 kN x 3.300 m = 12.14 kN.m
```

The governing case is not the heaviest lift. It is the 700 kg load held out at 2320 mm. The column, the base weld, the boom outer section and the stability check are all worst at this point. An operator who treats the lighter mid-reach lift as the safer one is mistaken.

<!-- IMAGE SLOT — Rated load chart -->
<!-- Source: JIB_CRANE_C1_v3\verification_outputs_20260613_145515\fig_05_Load_Chart.png -->
<!-- ![Rated load chart](figures/wheels/fig_05_Load_Chart.png) -->

> _Attach `figures/wheels/fig_05_Load_Chart.png` — from `JIB_CRANE_C1_v3\verification_outputs_20260613_145515\fig_05_Load_Chart.png` (rated load chart with the safe operating envelope shaded)._

---

## Free Body and Design Forces

At the governing case the factored tip load reacts through the boom into the luffing cylinder and the pivot, and from there down the column to the base and the wheels. The counterweight and crane self-weight act at the rear.

```
Boom moment balance about the pivot
   Cylinder force  F_cyl  = M_worst / a_cyl = 19.91 / 0.346 = 57.49 kN
   Pivot reaction  F_pivot = vector sum(F_cyl, W)           = 63.85 kN

Vertical and moment balance on the base (worst case)
   R_front (pair) = 16.72 kN  -> 8.36 kN per wheel
   R_rear  (pair) = -1.27 kN  -> -0.63 kN per wheel
```

| Quantity | Value |
|---|---|
| Cylinder force, F_cyl | 57.49 kN |
| Pivot reaction, F_pivot | 63.85 kN |
| Front wheel reaction (pair) | 16.72 kN |
| Rear wheel reaction (pair) | -1.27 kN |

The near-zero, slightly negative rear reaction confirms the crane is close to tipping at this point.

### Hydraulic Cross-Check (luffing cylinder)

The luffing cylinder is the Type 80 unit, bore 50 mm, piston area 1963 mm².

```
Pressure required = 57.49 kN / 1963 mm^2 = 29.3 MPa (293 bar)
(a 30 mm bore would need 813 bar -> confirms 50 mm bore is the luffer)
```

<!-- IMAGE SLOTS — Free body + wheel reactions -->
<!-- Source: JIB_CRANE_C1_v3\verification_outputs_20260613_145515\fig_01_Free_Body_Geometry.png -->
<!-- Source: JIB_CRANE_C1_v3\verification_outputs_20260613_145515\fig_06_Wheel_Reactions.png -->
<!-- ![Free body at the governing case](figures/wheels/fig_01_Free_Body_Geometry.png) -->
<!-- ![Wheel reactions and outrigger-arm stress](figures/wheels/fig_06_Wheel_Reactions.png) -->

> _Attach `figures/wheels/fig_01_Free_Body_Geometry.png` and `figures/wheels/fig_06_Wheel_Reactions.png` — both from `JIB_CRANE_C1_v3\verification_outputs_20260613_145515\` (free body at 700 kg @ 2320 mm; wheel reactions left, outrigger-arm stress profile right)._

---

## Detailed Results — Wheels-Only / Combined Verification (S275 basis, Rev 3/4)

The combined verification report is issued on nominal S275. Each member is reported with its stress, factor of safety, target and verdict at the governing case (700 kg at 2320 mm). Stability values are the worst across the chart.

### Boom bending

```
Boom outer (RHS 100 x 140 x 6) at root
   M = 19.91 kN.m   Sx = 107.0 cm^3
   sigma = 19.91e6 / 107.0e3 = 186.2 MPa   FoS = 275 / 186.2 = 1.48 (marginal)
Boom middle (RHS 80 x 120 x 6) at its root
   M = 7.90 kN.m    Sx = 73.0 cm^3
   sigma = 108.1 MPa   FoS = 2.54 (pass)
Boom inner (RHS 60 x 100 x 6) at its root
   M = 4.05 kN.m    Sx = 45.5 cm^3
   sigma = 89.0 MPa    FoS = 3.09 (pass)
```

Tip deflection at the worst case reaches about 23 mm by hand against an L/200 serviceability guide of roughly 12 mm, so the boom is outside the deflection guide at full reach.

### Column under combined load

```
Column (RHS 80 x 120 x 6), combined axial + bending
   N = 10.64 kN   A = 22.6 cm^2   M = 19.91 kN.m   Sx = 73.0 cm^3
   sigma_axial = 10.64e3 / 2260 =   4.7 MPa
   sigma_bend  = 19.91e6 / 73.0e3 = 272.7 MPa
   sigma_total =                    277.4 MPa
   FoS = 275 / 277.4 = 0.99 -> Fail (at yield)

Column buckling (Euler, weak axis, K = 2 cantilever)
   lambda = 80.6   Pcr = 720 kN   N = 10.64 kN   FoS = 67.6 (pass)
```

The column is a bending problem, not a buckling problem. To recover a 1.5 margin the section modulus would need to rise by about half.

### Base frame and outrigger arm

```
Outrigger arm (RHS 60 x 100 x 6)
   M_arm = 8.19 kN.m   Sx = 45.5 cm^3
   sigma = 8.19e6 / 45.5e3 = 180.1 MPa   FoS = 275 / 180.1 = 1.53 -> Pass
```

### Pins and connections

```
Boom pivot pin, dia 30 mm, double shear
   F_pivot = 63.85 kN   tau = 45.2 MPa   bearing = 141.9 MPa   FoS = 2.44 -> Fail*
Cylinder rod-end pin (as modelled, dia 20 mm), double shear
   F_cyl = 57.49 kN   tau = 91.5 MPa   bearing = 191.6 MPa   FoS = 1.20 -> Fail
Cross-check at Base Detail-4 pin (dia 40 mm), double shear
   tau = 22.9 MPa   bearing = 95.8 MPa   FoS = 4.81 -> Pass
Hook pin, dia 20 mm, single shear
   tau = 27.3 MPa   bearing = 28.6 MPa   FoS = 4.03 -> Pass
```

\* On the conservative series-factor basis coded, the pivot pin reads 2.44; on the conventional lifting-pin basis it moves to about 3.65 and passes. The basis-independent finding is that a 20 mm rod-end pin is undersized for the 57 kN it would carry; the 40 mm Detail-4 pin clears the check on either basis. The as-built diameter should be confirmed on the machine.

### Welds

```
Column-to-base weld (6 mm fillet)   sigma = 234.9 MPa   FoS = 0.85 -> Fail
Boom-bracket weld   (6 mm fillet)   tau   = 35.5 MPa    FoS = 3.39 -> Pass
Outrigger weld      (6 mm fillet)   sigma = 155.0 MPa   FoS = 1.29 -> Fail
```

### Slewing-flange bolts

```
5 x M16 grade 8.8 on 170 mm PCD, polar-moment method
   Bolt capacity : tension 113.0 kN, shear 75.4 kN
   Peak demand   : tension  93.7 kN, shear  2.1 kN per bolt
   Interaction utilisation = 0.35 -> FoS (combined) = 1.69 -> Pass
```

### Summary table (S275 basis)

| Check | Stress / value | FoS | Target | Verdict |
|---|---|---|---|---|
| Boom outer (100x140x6) | 186.2 MPa | 1.48 | 1.50 | Marginal |
| Boom middle (80x120x6) | 108.1 MPa | 2.54 | 1.50 | Pass |
| Boom inner (60x100x6) | 89.0 MPa | 3.09 | 1.50 | Pass |
| Column, combined N+M | 277.4 MPa | 0.99 | 1.50 | Fail (at yield) |
| Column buckling, Pcr | 720 kN | 67.6 | 1.50 | Pass |
| Outrigger arm | 180.1 MPa | 1.53 | 1.50 | Pass |
| Pivot pin (Ø30) | 45.2 MPa | 2.44 | 3.00 | Fail |
| Cylinder rod-end pin (Ø20 as modelled) | 91.5 MPa | 1.20 | 3.00 | Fail |
| Hook pin (Ø20) | 27.3 MPa | 4.03 | 3.00 | Pass |
| Slewing bolts (5 x M16, 8.8) | util 0.35 | 1.69 | 1.50 | Pass |
| Weld, column base | 234.9 MPa | 0.85 | 1.50 | Fail |
| Weld, boom bracket | 35.5 MPa | 3.39 | 1.50 | Pass |
| Weld, outrigger | 155.0 MPa | 1.29 | 1.50 | Fail |
| Stability, forward (worst point) | 9.20 kN.m | 0.96 | 1.50 | Fail (tips) |
| Stability, sideways slew (2376 track) | 7.77 kN.m | 1.05 | 1.50 | Fail |
| Deployed leg member (RHS 60x100x6) | 143.7 MPa | 1.91 | 1.50 | Pass |

Eight of the fourteen structural and mechanical checks fall short of target on a working-stress basis, and both stability checks fall short as well.

<!-- IMAGE SLOTS — Boom bending / stress, FoS summary -->
<!-- Source: JIB_CRANE_C1_v3\verification_outputs_20260613_145515\fig_02_Boom_BMD_Track_1.png -->
<!-- Source: JIB_CRANE_C1_v3\verification_outputs_20260613_145515\fig_08_FoS_Summary.png -->
<!-- ![Boom bending moment](figures/wheels/fig_02_Boom_BMD_Track_1.png) -->
<!-- ![Factor of safety summary](figures/wheels/fig_08_FoS_Summary.png) -->

> _Attach `figures/wheels/fig_02_Boom_BMD_Track_1.png` and `figures/wheels/fig_08_FoS_Summary.png` — both from `JIB_CRANE_C1_v3\verification_outputs_20260613_145515\`._

---

## Stability Against Overturning (wheels-only)

Stability is a moment balance about the front wheel line, 980 mm ahead of the column. The restoring moment comes from the crane self-weight and counterweight acting behind that line; the overturning moment from the load acting ahead of it.

```
Overturning balance about the front wheel line
   Restoring  M_R  = m_crane x g x (L_fwd + e_cg) = 700 x 9.81 x 1.280 = 8.79 kN.m
   Overturning M_OT = m_load x g x (reach - L_fwd)   [static load]
```

| Load case | M_OT (kN.m) | FoS = M_R / M_OT | Verdict |
|---|---|---|---|
| 1000 kg @ 1400 mm | 4.12 | 2.13 | Pass |
| 700 kg @ 2320 mm | 9.20 | 0.96 | Fail |
| 300 kg @ 3300 mm | 6.83 | 1.29 | Fail |

Worst stability factor 0.96 at 700 kg / 2320 mm against a target of 1.50, a fail in the wheels-only state. The near-zero rear wheel reaction is the same result seen from the support side.

<!-- IMAGE SLOT — Stability chart and map (wheels) -->
<!-- Source: JIB_CRANE_C1_v3\verification_outputs_20260613_145515\fig_07_Stability.png -->
<!-- ![Stability factor at each chart point and the factor map](figures/wheels/fig_07_Stability.png) -->

> _Attach `figures/wheels/fig_07_Stability.png` — from `JIB_CRANE_C1_v3\verification_outputs_20260613_145515\` (stability FoS at each chart point, left; factor map across reach and load with the chart overlaid, right)._

---

## Frame Model Results (Track 2)

The plane-frame solver was run for all three chart points. The model confirms the column as the most-stressed member, in agreement with the hand calculation.

| Chart point | Tip deflection (mm) | Peak stress (MPa) | FoS |
|---|---|---|---|
| 1000 kg @ 1400 mm | 51.5 | 235.1 | 1.17 |
| 700 kg @ 2320 mm | 102.7 | 272.7 | 1.01 |
| 300 kg @ 3300 mm | 101.1 | 166.2 | 1.65 |

The frame FoS values are quoted against yield directly (275 / peak stress), which is why they read slightly higher than the 1.5-target factors in the hand calc. Peak frame stress at the governing case is 272.7 MPa at the column, against yield 275 MPa and the 183 MPa allowable.

<!-- IMAGE SLOTS — Frame deformed shape and stress distribution -->
<!-- Source: JIB_CRANE_C1_v3\verification_outputs_20260613_145515\fig_03_Track_2_Deformed_Shape.png -->
<!-- Source: JIB_CRANE_C1_v3\verification_outputs_20260613_145515\fig_04_Track_2_Stress_Distribution.png -->
<!-- ![Frame deformed shape, scaled x20](figures/wheels/fig_03_Track_2_Deformed_Shape.png) -->
<!-- ![Frame stress distribution](figures/wheels/fig_04_Track_2_Stress_Distribution.png) -->

> _Attach `figures/wheels/fig_03_Track_2_Deformed_Shape.png` and `figures/wheels/fig_04_Track_2_Stress_Distribution.png` — both from `JIB_CRANE_C1_v3\verification_outputs_20260613_145515\` (deformed shape, scale x20, tip 102.68 mm; frame stress, peak 272.7 MPa at the column)._

### Operating envelope sweep

The model also sweeps the boom through its luffing range and tracks the boom-root stress and factor of safety against the chart capacity at each angle. The capacity drops at the reaches where stress approaches the allowable line.

<!-- IMAGE SLOTS — Operating envelope sweep + animation -->
<!-- Source: JIB_CRANE_C1_v3\verification_outputs_20260613_145515\fig_10_Crane_Operation_Animation.png -->
<!-- Source: JIB_CRANE_C1_v3\verification_outputs_20260613_145515\crane_animation (gif) -->
<!-- ![Operating-envelope sweep](figures/wheels/fig_10_Crane_Operation_Animation.png) -->
<!-- ![Crane operation animation](figures/wheels/crane_animation.gif) -->

> _Attach `figures/wheels/fig_10_Crane_Operation_Animation.png` and `figures/wheels/crane_animation.gif` — both from `JIB_CRANE_C1_v3\verification_outputs_20260613_145515\`._

---

## Cross-Validation of the Two Methods

| Quantity | Track 1 (hand) | Track 2 (frame) | Agreement |
|---|---|---|---|
| Column-base moment (kN.m) | 19.91 | 19.91 | Exact |
| Peak member stress (MPa) | 277.4 | 272.7 | Within 2% |
| Boom tip deflection (mm) | 22.7 | 102.7 | Different basis |

The deflection gap is expected and not an error. The hand figure is boom bending alone, treating the column top as rigid. The frame figure adds the column bending and the base flexibility, so the tip moves much further. For a stress verdict the moment and stress agreement is what matters, and that is tight.

<!-- IMAGE SLOT — Method cross-check -->
<!-- Source: JIB_CRANE_C1_v3\verification_outputs_20260613_145515\fig_09_T1_vs_T2_cross_check.png -->
<!-- ![Method cross-check, hand calc vs in-MATLAB FEA](figures/wheels/fig_09_T1_vs_T2_cross_check.png) -->

> _Attach `figures/wheels/fig_09_T1_vs_T2_cross_check.png` — from `JIB_CRANE_C1_v3\verification_outputs_20260613_145515\` (side-by-side comparison of the two methods)._

---

## Elastic and Plastic Capacity of the Column

The column reaches first-fibre yield at a working-stress factor of safety of 0.99 (S275 basis), which means the applied moment at 1.25 times the rated load is essentially at the first-yield moment of the section. First yield, however, is not collapse. The collapse limit for a compact steel section is the plastic moment, larger than the first-yield moment by the shape factor, 1.23 for this hollow section. That difference is reserve the elastic working-stress check does not credit.

```
Column RHS 80 x 120 x 6, elastic-to-plastic capacity (nominal S275)
   Sx = 73.0 cm^3   Zx = 89.7 cm^3   shape factor Zx/Sx = 1.23
   applied moment @ 1.25 x SWL = 19.91 kN.m   (elastic FoS 0.99)
   first-yield moment My        = 20.08 kN.m
   plastic moment Mp (nom 275)  = 24.67 kN.m   (reserve over My +23%)
   plastic moment Mp (mill 320) = 28.71 kN.m   (reserve over My +43%)
   plastic margin on applied moment: Mp / 19.91 = 1.24 (nominal S275)
```

Two further effects act in the same direction and are not credited in the elastic check: the calculations carry a 1.25 dynamic factor that a static check would not, and the luffing cylinder acting as a diagonal strut would relieve some of the column bending the cantilever model does not capture. The other members stay well within the elastic range, which is why the column is the lone item near its limit.

The factor of safety of 0.99 is therefore a conservative first-yield working-stress result, not a prediction of failure. What the structure does not have, on the as-built numbers, is the conventional 1.5 working-stress margin at mid reach for repeated service. This is a theoretical calculation; physical load testing is carried out separately with the approval officer.

---

## Deployed-Outrigger Configuration (Q235 basis, Rev C)

The deployed legs form a # arrangement: two fore-aft rails crossed by a front and a rear transverse beam, each reaching out to a foot. The legs stand at 90 degrees to the rails, so they widen the lateral track but add no forward reach. The front tipping line stays at the front frame line, 980 mm ahead of the column, exactly where it sits on the wheels.

### Deployed geometry

| Quantity | Value | Source |
|---|---|---|
| Leg arm (lateral reach) | 750 mm | DETAIL-11 |
| Foot drop | 230 mm | DETAIL-11 |
| Foot pad | 80 mm | DETAIL-11 |
| Levelling jack screw | M16 (Ø16) | DETAIL-11 |
| Leg hinge pin | Ø32 | DETAIL-11 |
| Alternative leg arm | 798 mm | DETAIL B-B |
| Foot eye | Ø20 | DETAIL-12 |
| Inner frame width | 1100 mm | GA plan view |
| Frame track, rail to rail | 876 mm | Base drawing |
| Deployed lateral track | 2500 mm (foot-to-foot) | GA plan view |
| Front foot ahead of column | 980 mm (front frame line) | Base drawing + warning plate |
| Leg member section | RHS 60 x 100 x 6 | drawing |
| Material | Q235 (fy 235 MPa) | client-confirmed |

The 980 mm front line follows from two agreeing readings: the base top-view chain places the slew centre about 1076 mm behind the front edge, and the warning plate marks 420 mm at the front, so the line sits at 1400 minus 420, or 980 mm. The deployed track is read off the updated general-arrangement plan view at 2500 mm foot-to-foot, replacing the 2376 mm earlier derived from the bare leg-arm projection.

<!-- IMAGE SLOT — Deployed footprint plan -->
<!-- Source: JIB_CRANE_C2_v3\verification_outputs_20260613_150046\fig_01_Free_Body_Geometry.png -->
<!-- ![Deployed footprint in plan](figures/deployed/fig_01_Free_Body_Geometry.png) -->

> _Attach `figures/deployed/fig_01_Free_Body_Geometry.png` — from `JIB_CRANE_C2_v3\verification_outputs_20260613_150046\` (two rails plus four transverse legs, # arrangement; 2500 mm track, 980 mm front line)._

### Forward stability (deployed)

```
Overturning balance about the front foot line
   Restoring  M_R  = m_crane x g x (L_fwd + e_cg) = 700 x 9.81 x (0.980 + 0.300) = 8.79 kN.m
   Overturning M_OT = m_load x g x (reach - L_fwd)   [static load]
```

| Load case | M_OT (kN.m) | FoS = M_R / M_OT | Verdict |
|---|---|---|---|
| 1000 kg @ 1400 mm | 4.12 | 2.13 | Pass |
| 700 kg @ 2320 mm | 9.20 | 0.96 | Fail (tips) |
| 300 kg @ 3300 mm | 6.83 | 1.29 | Fail |

Forward stability is set by the masses and the geometry, not the steel, so it is identical to the wheels-only case. Transverse legs do not move the front line, so they cannot improve forward stability.

### Sideways stability during slew

```
Overturning balance about a side foot line
   Restoring  M_R  = m_crane x g x t_half = 700 x 9.81 x 1.250 = 8.58 kN.m
   Overturning M_OT = m_load x g x (reach - t_half)   [static load]
   Half-track from the drawing: t_half = 2500 / 2 = 1250 mm (track 2500 mm)
```

| Load case | M_OT (kN.m) | FoS = M_R / M_OT | Verdict |
|---|---|---|---|
| 1000 kg @ 1400 mm | 1.47 | 5.83 | Pass |
| 700 kg @ 2320 mm | 7.35 | 1.17 | Fail |
| 300 kg @ 3300 mm | 6.03 | 1.42 | Fail |

The close-in lift is steady, but the mid and far points fall below target. Solving the balance for 1.50 at the governing mid point gives a half-track of 1392 mm, a full track of 2784 mm. The 2500 mm drawn track is about 284 mm short, so the deployed width must be confirmed on the machine and, if the feet adjust, set to at least 2784 mm before slewing at mid or long reach. The manufacturer figure marks the deployed width as adjustable up to roughly 2870 mm.

<!-- IMAGE SLOT — Sideways stability vs track (deployed) -->
<!-- Source: JIB_CRANE_C2_v3\verification_outputs_20260613_150046\fig_07_Stability.png -->
<!-- ![Sideways stability against deployed track](figures/deployed/fig_07_Stability.png) -->

> _Attach `figures/deployed/fig_07_Stability.png` — from `JIB_CRANE_C2_v3\verification_outputs_20260613_150046\` (2500 mm sits below the 1.50 target; about 2784 mm is needed)._

### Deployed leg member

```
Deployed leg, RHS 60 x 100 x 6, cantilever (Q235)
   front reaction (1000 kg @ 1400 mm) = 17.43 kN total -> 8.72 kN per leg
   lever (leg arm) = 750 mm = 0.750 m
   M_leg = 8.72 x 0.750 = 6.54 kN.m
   Sx = 45.5 cm^3 = 45.5e3 mm^3
   sigma = M_leg / Sx = 6.54e6 / 45.5e3 = 143.7 MPa
   FoS = fy / sigma = 235 / 143.7 = 1.64 -> Pass
```

The leg passes at 1.64 on Q235 (1.91 on S275). The largest foot reaction comes from the heaviest lift taken in close, not from the tipping case, because member force follows the vertical load while tipping follows the lever arm.

### Items carried over from the wheels-only check (recomputed on Q235)

The legs attach to the base frame, below the slewing bearing. The axial load and the bending moment that pass up into the column, the boom and its pins are fixed by the boom geometry and the lifted load, and do not change when the base stands on legs instead of wheels. Every stress-based factor below is recomputed on Q235.

| Check | FoS | Target | Verdict |
|---|---|---|---|
| Boom outer (100x140x6) | 1.26 | 1.50 | Fail |
| Boom middle (80x120x6) | 2.17 | 1.50 | Pass |
| Boom inner (60x100x6) | 2.64 | 1.50 | Pass |
| Column, combined N+M | 0.85 | 1.50 | Fail |
| Column buckling | 67.6 | 1.50 | Pass |
| Base outrigger arm (Q235) | 1.30 | 1.50 | Fail |
| Pivot pin (Ø30) | 2.08 | 3.00 | Fail |
| Cylinder rod-end pin (Ø20 as modelled) | 1.03 | 3.00 | Fail |
| Hook pin (Ø20) | 3.44 | 3.00 | Pass |
| Slewing bolts (5 x M16, 8.8) | 1.69 | 1.50 | Pass |
| Weld, column base | 0.73 | 1.50 | Fail |
| Weld, boom bracket | 2.92 | 1.50 | Pass |

The Ø40 Base DETAIL-4 cylinder pin clears its check at 4.11 on Q235. The slewing bolts and the hook pin are grade 8.8 fasteners, so they do not move with the base-steel grade.

### Stowed versus deployed (Q235)

| Item | Wheels stowed | Legs deployed (# arrangement) |
|---|---|---|
| Front tipping line ahead of column | 980 mm | 980 mm (no forward reach) |
| Lateral foot track | 876 mm | 2500 mm |
| Forward tipping, worst point | FoS 0.96, Fail | FoS 0.96, Fail (unchanged) |
| Sideways tipping during slew | narrow track, tips | FoS 1.17, Fail (track short) |
| Front ground-support member | outrigger arm 180 MPa, FoS 1.30, Fail | leg arm 144 MPa, FoS 1.64, Pass |
| Column, combined N+M | 277 MPa, FoS 0.85, Fail | unchanged |
| Column-to-base weld | 235 MPa, FoS 0.73, Fail | unchanged |
| Boom outer section | 186 MPa, FoS 1.26, Fail | unchanged |
| Pins and slewing bolts | Q235 base, Gr 8.8 bolts | unchanged |

The legs widen the track and ease the leg member, so sideways tipping becomes possible to manage and the leg passes. Forward tipping, the column, the weld, the boom and the outrigger arm are the same in both states, because the legs do not act on them and the material is common to both.

<!-- IMAGE SLOTS — Deployed-state stability bars + Q235 FoS summary -->
<!-- Source: JIB_CRANE_C2_v3\verification_outputs_20260613_150046\fig_07_Stability.png -->
<!-- Source: JIB_CRANE_C2_v3\verification_outputs_20260613_150046\fig_08_FoS_Summary.png -->
<!-- ![Stability in the deployed state](figures/deployed/fig_07_Stability.png) -->
<!-- ![Factor-of-safety summary on Q235](figures/deployed/fig_08_FoS_Summary.png) -->

> _Attach `figures/deployed/fig_07_Stability.png` and `figures/deployed/fig_08_FoS_Summary.png` — both from `JIB_CRANE_C2_v3\verification_outputs_20260613_150046\` (deployed-state stability bars; Q235 factor-of-safety summary, column buckling bar 67.6 capped for readability)._

---

## Material Sensitivity: S275 versus Q235

| Check | S275 (prior) | Q235 (current) |
|---|---|---|
| Boom outer (100x140x6) | 1.48 Marginal | 1.26 Fail |
| Outrigger arm | 1.53 Pass | 1.30 Fail |
| Column, combined N+M | 0.99 Fail | 0.85 Fail |
| Column-to-base weld | 0.85 Fail | 0.73 Fail |
| Outrigger weld | 1.29 Fail | 1.11 Fail |
| Deployed leg member | 1.91 Pass | 1.64 Pass |
| Pivot pin (Ø30) | 2.44 | 2.08 |
| Cylinder rod-end pin (Ø20) | 1.20 | 1.03 |
| Hook pin (Ø20) | 4.03 | 3.44 |
| Cylinder pin (Ø40, Detail-4) | 4.81 | 4.11 |
| Sideways stability, worst | 1.05 @ 2376 mm | 1.17 @ 2500 mm |

What did not move: forward stability stays 0.96 (a moment balance does not depend on steel grade or track); the slewing bolts (1.69) and hook pin are grade 8.8 fasteners; the 980 mm front overhang and 2360 mm wheelbase are unchanged. The failing-check count stays at 8 of 14.

---

## Consolidated Results

| Area | Quantity | Result | Status |
|---|---|---|---|
| Governing case | 700 kg at 2320 mm | M = 19.91 kN.m | Mid-reach governs |
| Column | Combined N+M | 277 MPa, FoS 0.99 (S275) / 0.85 (Q235) | Fail (at yield) |
| Column-base weld | Stress couple | 235 MPa, FoS 0.85 (S275) / 0.73 (Q235) | Fail |
| Boom outer | Bending | 186 MPa, FoS 1.48 (S275) / 1.26 (Q235) | Marginal / Fail |
| Stability (forward) | Worst point | FoS 0.96 | Fail (tips) |
| Stability (deployed, sideways) | Worst point | FoS 1.05 @ 2376 / 1.17 @ 2500 | Fail (track short) |
| Column capacity | Shape factor | 1.23 | Plastic reserve beyond first yield |
| Cross-validation | Hand vs frame stress | 277.4 vs 272.7 MPa | Within 2% |
| Checks below target | Combined | 8 of 14 | Cannot certify to chart |

---

## Engineering Conclusions

- In neither stance does the crane meet the agreed factors of safety across its rated chart. On its wheels it carries the loads, but at the governing 700 kg mid-reach point the column reaches yield (FoS 0.99 on S275, 0.85 on Q235) and its base weld is past target (0.85 / 0.73).
- Forward tipping fails at mid reach (FoS 0.96, below one) on the corrected 980 mm front line. A transverse leg adds no forward reach, so deploying the outriggers cannot help it.
- The confirmed Q235 material thins the structural margin across the board and pulls the boom outer and the base outrigger arm below target, where they were marginal or passing on S275.
- Deploying the outriggers is a precondition for slewing and eases the front-support member, but does nothing for forward tipping or for the column and its base weld.
- The column reaches first yield near the rated load but retains plastic reserve (shape factor 1.23) that the elastic check does not credit; the FoS near 1.0 is a conservative first-yield result, not a forecast of failure. The report is conservative, not wrong.
- The two independent methods agree to within 2% on the quantities that decide the verdict.

---

## Recommendations and Routes Forward

| Route | Outcome |
|---|---|
| **Re-rate the chart (recommended)** | Reduce the rated capacities so every working-stress check clears target. The separate Load Re-Rating Report sets a maximum SWL of 650 kg, about 390 kg at mid reach and 255 kg at far reach. Cheapest route, no fabrication, applies in both stances. |
| **Strengthen the column and base (keeps full rating)** | A larger column section of the order of 100 x 150 x 8 in S275, base weld grown to suit, cylinder pin confirmed or upsized. The only route that preserves the full 1 tonne chart. |
| **Confirm mill yield, cylinder geometry and deployed track** | Parallel actions. Confirming actual mill yield and the cylinder anchor would likely lift the column margin above 1.0; confirming the deployed track to 2784 mm clears sideways stability. |

In all cases the deployed track must be confirmed to at least 2784 mm before the crane is slewed at mid or long reach. The crane should not be worked to the current 1 tonne chart until forward tipping, the column, the base weld, the boom outer, the outrigger arm and the cylinder pin are addressed.

---

## Assumptions and Items to Confirm

- **Material, Q235.** Client-confirmed. Nominal yield 235 MPa used for all verdicts; all sections are 6 mm wall, so no thickness de-rating applies. Mill certificates would confirm the actual yield, which commonly runs a little above nominal and would add margin.
- **Crane self-weight, 700 kg.** Client-supplied with no bill of materials. A swing of plus or minus 100 kg moves both stability results.
- **Centre-of-gravity offset, 0.30 m behind the column.** Estimated from the counterweight and pump positions; affects stability only.
- **Front overhang to tipping line, 980 mm.** Resolved from the base top-view chain and the warning-plate 420 mm dimension, which agree. The 2360 mm wheelbase on the updated drawing is unchanged. Worth a tape check on the machine.
- **Deployed lateral track, 2500 mm.** Read off the updated general-arrangement plan view; make-or-break input for sideways stability. Adjustable up to about 2870 mm per the manufacturer figure. As-built width must be measured.
- **Leg deployment angle, 90 degrees.** Taken from the deployment schematic. A forward splay would advance the front line and improve forward stability, so it should be confirmed.
- **Leg arm, 750 mm, and section, RHS 60 x 100 x 6.** From DETAIL-11; DETAIL B-B shows a 798 mm variant, confirm which is fitted.
- **Cylinder lever arm (0.346 m) and rod-end pin diameter.** Not fully dimensioned on the drawings. The pin verdict is conditional on the as-built size (Ø20 fails; Ø40 Detail-4 passes at 4.81 on S275, 4.11 on Q235).
- **Weld throats, 6 mm.** Taken as the drawing minimum. If site welds are smaller the weld checks worsen.

---

## Standards Referenced

- EN 13001-3-1, crane structural design, load effects and proof of competence
- EN 1993-1-1, design of steel structures, general rules
- EN 1993-1-8, design of joints
- GB/T 700, carbon structural steels (Q235)
- Cross-referenced with AS 1418.1 and SS 559 for the Singapore context

---

## Revision History

| Stage | Revision | Date | Description |
|---|---|---|---|
| Wheels-only | Rev 0 | 27 May 2026 | First issue (wheels only), summary findings with figures |
| Wheels-only | Rev 1 | 08 Jun 2026 | Full hand calculations for every check; methodology expanded |
| Wheels-only | Rev 2 | 09 Jun 2026 | Cylinder-pin cross-check against Base Detail-4; column capacity check added |
| Deployed | Rev A | 10 Jun 2026 | First deployed-outrigger study, front line at the 1200 mm place-holder |
| Deployed | Rev B | 11 Jun 2026 | Front overhang corrected to 980 mm; forward-stability and leg-reaction figures revised |
| Deployed | Rev C | 12 Jun 2026 | Material set to Q235; deployed track taken at 2500 mm; reframed as theoretical elastic-versus-plastic capacity |
| Combined | Rev 3 | 10 Jun 2026 | Wheels-only and deployed verification consolidated into one submission |
| Combined | Rev 4 / Rev 5 | 12 Jun 2026 | Combined submission updated |
| Re-rating | Rev R1 / Rev R2 | 11–12 Jun 2026 | Reduced SWL chart with operating limitations, issued separately |

---

## Project Files

| File | Description |
|---|---|
| `jib_crane_verification_C1_v3.m` | Wheels-only parametric MATLAB R2026a script, latest (`JIB_CRANE_C1_v3\`) |
| `jib_crane_verification_C2_v3.m` | Deployed-outrigger parametric MATLAB R2026a script, latest, Q235 / 2500 mm track (`JIB_CRANE_C2_v3\`) |
| `Jib_Crane_Combined_Structural_Verification_Rev4.pdf` | Combined wheels-only and deployed verification, single submission |
| `Jib_Crane_Combined_Structural_Verification_Rev5.pdf` | Combined verification, latest revision |
| `Jib_Crane_Deployed_Outrigger_Verification_RevC.pdf` | Deployed-outrigger stability and leg-member check on Q235 (`JIB_CRANE_C2_v3\`) |
| `Jib_Crane_Revision_Note_RevB_to_RevC.pdf` | Change note: S275 to Q235, 2376 mm to 2500 mm track (`JIB_CRANE_C2_v3\`) |
| `Jib_Crane_Load_Re-Rating_Report_RevR2.pdf` | Reduced SWL chart with operating limitations |
| `console_log.txt` | Full run log including the factor-of-safety tables (in each `verification_outputs_*` subfolder) |
| `workspace.mat` | Saved variables for re-plotting without re-running (in each `verification_outputs_*` subfolder) |
| `figures/wheels/`, `figures/deployed/` | Result plots, figures and animation (see Figure Index) |

---

## Figure Index

The two source subfolders are produced by the latest runs of each script. The wheels-only and deployed runs share filenames, so they are split into two repo folders.

### Wheels-only / combined — source: `JIB_CRANE_C1_v3\verification_outputs_20260613_145515\` → repo `figures/wheels/`

| Figure | File | Shows |
|---|---|---|
| Fig 1 | `fig_01_Free_Body_Geometry` | Free body at the governing case (header, free-body section) |
| Fig 2 | `fig_02_Boom_BMD_Track_1` | Boom bending moment and stress |
| Fig 3 | `fig_03_Track_2_Deformed_Shape` | Frame deformed shape (scale x20) |
| Fig 4 | `fig_04_Track_2_Stress_Distribution` | Frame stress distribution |
| Fig 5 | `fig_05_Load_Chart` | Rated load chart and safe envelope |
| Fig 6 | `fig_06_Wheel_Reactions` | Wheel reactions and outrigger-arm stress |
| Fig 7 | `fig_07_Stability` | Stability FoS at each chart point and factor map (wheels) |
| Fig 8 | `fig_08_FoS_Summary` | Factor-of-safety summary |
| Fig 9 | `fig_09_T1_vs_T2_cross_check` | Method cross-check, hand calc vs FEA |
| Fig 10 | `fig_10_Crane_Operation_Animation` | Operating-envelope sweep |
| Animation | `crane_animation` | Luffing-cycle animation |
| Log | `console_log.txt` | Full run log |
| Data | `workspace.mat` | Saved variables for re-plotting |

### Deployed-outrigger — source: `JIB_CRANE_C2_v3\verification_outputs_20260613_150046\` → repo `figures/deployed/`

| Figure | File | Shows |
|---|---|---|
| Fig 1 | `fig_01_Free_Body_Geometry` | Deployed footprint in plan (# arrangement, 2500 mm track) |
| Fig 7 | `fig_07_Stability` | Sideways stability vs track / deployed-state stability bars |
| Fig 8 | `fig_08_FoS_Summary` | Factor-of-safety summary on Q235 |

> The deployed run also writes the full `fig_01`–`fig_10` set, `crane_animation`, `console_log.txt` and `workspace.mat`; only the three figures referenced in the deployed sections are listed above. Earlier runs are available under `JIB_CRANE_C1\…_20260527_200459\` (note the `fig_02`/`fig_03` swap), `JIB_CRANE_C1_v2\…_20260609_111022\`, `JIB_CRANE_C2_v1\…_20260610_030747\` and `JIB_CRANE_C2_v2\…_20260611_045424\`.

---

## Future Work

- Confirm the as-built inputs flagged above (material yield, self-weight, CG, cylinder geometry, pin diameter, deployed track) before any certification
- Quantify the cylinder triangulation relief on column bending that the cantilever model does not credit
- Build a 3D finite element model of the column-to-base joint to capture the weld stress in detail
- Extend the stability map to the corner slew positions between the fore-aft and side tipping lines

---

## Author

**Mohammad Haris** — Mechanical Engineer | FEA & CFD Engineer

GitHub: [github.com/MohammadHaris13](https://github.com/MohammadHaris13)

---

*This project is an independent verification exercise. It is not a design certificate and does not replace load testing or a competent-person examination before the crane is put to work.*
