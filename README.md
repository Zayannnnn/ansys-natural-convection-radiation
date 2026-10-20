<div align="center">

# ANSYS Fluent | Natural Convection & Radiation

### 3D CFD Simulation of Coupled Heat Transfer in an Enclosure

**ANSYS Fluent 2026 R1 · Laminar Flow · Energy Equation · S2S Radiation**

</div>

---

## Project Overview

This project investigates heat transfer and fluid flow inside a three-dimensional enclosure using **ANSYS Fluent**. A heated wall drives temperature gradients in the air, while the model accounts for fluid motion, thermal transport, and surface-to-surface radiative exchange.

The project brings together the three conservation laws used in computational fluid dynamics (CFD)—**mass, momentum, and energy**—with the **Surface-to-Surface (S2S) radiation model**.

> **Current status:** The provided Fluent report records 600 iterations. Continuity and energy residuals had not reached their specified convergence criteria at the end of that run. Results are therefore documented as preliminary, not as a fully converged or validated solution.

## Objectives

- Formulate the mass, momentum, and energy conservation equations for a thermal-flow problem.
- Examine buoyancy-driven natural convection in air.
- Include radiative exchange between enclosure surfaces using the S2S model.
- Inspect the mesh and its reported quality metrics.
- Review temperature contours, velocity vectors, and residual histories.
- Document solver settings and convergence limitations transparently.

## Simulation at a Glance

| Item | Setting reported by Fluent |
|---|---|
| Software | ANSYS Fluent |
| Version | 2026 R1 (`26.1.0-10262`) |
| Domain | 3D |
| Solver formulation | Pressure-based |
| Time formulation | Steady |
| Viscous model | Laminar |
| Energy equation | Enabled |
| Radiation | Surface-to-Surface (S2S) |
| Pressure–velocity coupling | Coupled |
| Pressure discretization | Body Force Weighted |
| Momentum discretization | Second Order Upwind |
| Energy discretization | Second Order Upwind |
| Gradient reconstruction | Least Squares Cell Based |
| Pseudo-time method | Enabled |

## Physical Model

The simulation represents air in an enclosure with one prescribed-temperature wall and other walls configured with mixed thermal conditions, external convection, and external radiation.

### Main thermal settings

| Parameter | Value |
|---|---:|
| Heated wall temperature (`w-low-x`) | 473.15 K |
| External/free-stream temperature | 293.15 K |
| External emissivity on mixed-condition walls | 0.75 |
| Internal emissivity of heated wall | 0.95 |
| Heated wall | Stationary, no-slip |
| Radiation model | S2S |

The supplied report does **not** explicitly state the overall enclosure dimensions or gravity-vector components. These should be verified from the final geometry and operating conditions before being recorded as confirmed model inputs.

## Governing Equations

The equations below describe the main conservation principles. The momentum and energy forms shown are common simplified forms for steady, incompressible Newtonian flow with constant properties; they are explanatory forms and are not a claim that every assumption is exactly satisfied by the selected Fluent material model.

### 1. Conservation of Mass — Continuity

For steady flow:

\[
\nabla\cdot(\rho\mathbf{V})=0
\]

For constant-density incompressible flow:

\[
\frac{\partial u}{\partial x}
+\frac{\partial v}{\partial y}
+\frac{\partial w}{\partial z}=0
\]

Here, \(\mathbf{V}=(u,v,w)\) is the velocity vector. Continuity enforces conservation of mass.

### 2. Conservation of Momentum — Navier–Stokes

For steady, incompressible Newtonian flow with constant viscosity:

\[
\rho(\mathbf{V}\cdot\nabla)\mathbf{V}
=-\nabla p+\mu\nabla^2\mathbf{V}+\rho\mathbf{g}
\]

The terms represent inertia, pressure forces, viscous forces, and gravity. In a natural-convection problem, temperature-related density differences in a gravitational field can drive buoyancy-induced flow.

The supplied report specifies an **incompressible ideal-gas density model**. Accordingly, the constant-density equation above is a simplified explanatory form rather than the complete variable-density formulation.

### 3. Conservation of Energy

For steady flow with constant thermal conductivity and no volumetric heat source in the fluid:

\[
\rho c_p(\mathbf{V}\cdot\nabla T)=k\nabla^2T
\]

Fourier’s law for conductive heat flux is:

\[
\mathbf{q}_{cond}=-k\nabla T
\]

The energy equation accounts for thermal energy transported by moving air and heat conducted through the fluid.

### 4. Surface-to-Surface Radiation

A simplified net radiative heat-flux relation for a surface exchanging with large isothermal surroundings is:

\[
q''_{rad}=\varepsilon\sigma(T_w^4-T_{sur}^4)
\]

where \(\varepsilon\) is emissivity and \(\sigma\) is the Stefan–Boltzmann constant.

The Fluent S2S model calculates radiative exchange between participating enclosure surfaces using emissivities and geometric view factors. The equation above is a simplified illustration, not the full S2S formulation.

## Material Properties

### Fluid: Air

| Property | Value in the report |
|---|---:|
| Density model | Incompressible ideal gas |
| Specific heat, \(c_p\) | 1021 J/(kg·K) |
| Thermal conductivity, \(k\) | 0.0371 W/(m·K) |
| Dynamic viscosity, \(\mu\) | \(2.485\times10^{-5}\) kg/(m·s) |
| Molecular weight | 28.966 kg/kmol |

### Solid: Insulation

| Property | Value in the report |
|---|---:|
| Density | 50 kg/m³ |
| Specific heat, \(c_p\) | 800 J/(kg·K) |
| Thermal conductivity, \(k\) | 0.09 W/(m·K) |

## Boundary Conditions

The values in this section are transcribed from the supplied Fluent report. Wall names are the Fluent boundary-zone names.

### Heated wall — `w-low-x`

| Setting | Reported value |
|---|---:|
| Thermal boundary type | Temperature |
| Temperature | 473.15 K |
| Wall thickness | 0 m |
| Internal emissivity | 0.95 |
| Motion | Stationary |
| Shear condition | No Slip |
| Included in S2S view-factor calculation | Yes |

### Other wall zones

| Wall zone | Thickness (m) | External \(h\) (W/m²·K) | Free-stream temperature (K) | External emissivity | Radiation temperature (K) |
|---|---:|---:|---:|---:|---:|
| `w-high-x` | 0.05 | 5 | 293.15 | 0.75 | 293.15 |
| `w-high-y` | 0.95 | 3 | 293.15 | 0.75 | 293.15 |
| `w-low-y` | 0.95 | 3 | 293.15 | 0.75 | 293.15 |
| `w-high-z` | 0.05 | 5 | 293.15 | 0.75 | 293.15 |
| `w-low-z` | 0.05 | 5 | 293.15 | 0.75 | 293.15 |

The report identifies these zones as stationary walls with no-slip shear conditions, insulation material, and mixed thermal boundary conditions. The external convection relation is:

\[
q''_{conv}=h(T_w-T_\infty)
\]

**Model verification required:** The report lists **0.95 m wall thicknesses** and a **0.05 W/m³ heat-generation rate** for `w-high-y` and `w-low-y`. These values appear unusual for this enclosure model and should be checked in Fluent before being treated as intentional. The report does not provide enough information to infer or replace them safely.

## Mesh Information

| Metric | Value reported |
|---|---:|
| Cells | 8,960 |
| Faces | 28,304 |
| Nodes | 10,455 |
| Cell type | Hexahedral |
| Minimum orthogonal quality | 0.99162966 |
| Maximum aspect ratio | 4.9696031 |

The reported minimum orthogonal quality is high. However, mesh quality alone does not establish solution accuracy; a mesh-independence study is not documented in the supplied report.

## Solver and Numerical Settings

| Setting | Value |
|---|---|
| Pressure–velocity scheme | Coupled |
| Pressure interpolation | Body Force Weighted |
| Momentum scheme | Second Order Upwind |
| Energy scheme | Second Order Upwind |
| Gradient method | Least Squares Cell Based |
| Pseudo-time method | Global time step enabled |
| Explicit pressure relaxation factor | 0.5 |
| Explicit momentum relaxation factor | 0.5 |
| Density relaxation factor | 1.0 |
| Body-force relaxation factor | 1.0 |
| Energy relaxation factor | 0.75 |

## Solution Status and Convergence

The supplied report records **600 iterations**.

| Equation | Final residual | Criterion | Status in report |
|---|---:|---:|---|
| Continuity | 0.03913837 | 0.001 | Not converged |
| x-velocity | 0.0007750211 | 0.001 | Converged |
| y-velocity | 0.0007616651 | 0.001 | Converged |
| z-velocity | 0.0005327863 | 0.001 | Converged |
| Energy | \(3.132311\times10^{-6}\) | \(1\times10^{-6}\) | Not converged |

**Interpretation:** The velocity residuals satisfy their listed criteria, but continuity and energy do not. Iteration count alone is not proof of convergence. Before reporting final quantitative conclusions, review residual trends, monitor heat-transfer quantities, and resolve the outstanding convergence criteria.

### Report-definition values

| Name | Reported value |
|---|---:|
| `parameter-1` | −2.445026 W |
| `parameter-2` | −0.8811617 W |

The report does not identify the precise definitions or surface/sign conventions associated with these parameters. Their physical interpretation should be confirmed in Fluent before labelling them as specific heat-transfer rates.

## Post-Processing Included in the Report

The exported report contains:

- Residual history plot.
- Mesh visualization.
- Temperature contour visualization.
- Velocity-vector visualization.

### Recommended next steps

1. Verify enclosure dimensions, coordinate system, gravity direction, and wall-zone assignments.
2. Check the unusual thickness and heat-generation values noted above.
3. Address the continuity and energy convergence criteria.
4. Save the final case and data files together.
5. Create and label the intended center plane and line/rake surfaces using coordinates that match the actual geometry.
6. Export temperature contours, velocity vectors, wall heat flux, and S2S radiation results.
7. Perform a mesh-independence study if quantitative accuracy is required.

## Numerical Method

ANSYS Fluent uses the **Finite Volume Method (FVM)** to discretize conservation equations over computational cells. It iteratively solves the resulting algebraic equations for pressure, velocity, and temperature. The S2S model calculates radiative exchange between participating surfaces through view factors.

The principal solution fields are:

\[
u(x,y,z),\quad v(x,y,z),\quad w(x,y,z),\quad p(x,y,z),\quad T(x,y,z)
\]

These fields are used to interpret the airflow, temperature distribution, and thermal behaviour of the enclosure.

## Suggested Repository Layout

```text
ansys-natural-convection-radiation/
├── README.md
├── report/
│   └── ansys-fluent-simulation-report.pdf
├── images/
│   ├── mesh.png
│   ├── temperature-contours.png
│   ├── velocity-vectors.png
│   └── residuals.png
└── fluent-files/
    └── README.md
```

Add only files that you intend to publish. ANSYS case/data files may be large; check repository limits and avoid uploading unrelated personal information or private local file paths.

## References

- ANSYS Fluent 2026 R1 simulation report provided with this project.
- ANSYS Fluent documentation for conservation equations, wall boundary conditions, and the Surface-to-Surface radiation model.

## Author

**Zayan**  
Mechanical Engineering · Academic CFD Project

---

*This README documents the settings and convergence status visible in the supplied report. It distinguishes reported data from items that still require verification and does not claim that the solution is fully converged or independently validated.*
