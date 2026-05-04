# VIFD-Thermal-Control

**Variational Information Flow Dynamics: A Physics-Inspired AI Framework for Control of Complex Energy and Thermal Systems**

>  H. Hamam  

---

## What is VIFD?

VIFD couples a thermal PDE with a decision-field PDE so that the controller and the temperature field evolve together on the same spatial domain. Instead of computing a scalar control signal from a scalar error, VIFD maintains a full 2-D decision field π(x, t) that diffuses, advects, and relaxes toward a temperature-dependent target. The result is a spatially coherent, energy-aware actuation map with provable exponential convergence.

---

## Key results

| Metric | VIFD | Threshold | PID | MPC-lite |
|---|---|---|---|---|
| Final T_max [°C] | 53.6 | **45.0** | 50.5 | 51.4 |
| Mean control effort | **0.00199** | 0.00600 | 0.00361 | 0.00319 |
| Efficiency ratio | **5 082** | 3 106 | 3 640 | 3 841 |
| Spatial TV | **0.00067** | 0.01704 | 0.00121 | 0.00106 |

VIFD achieves 64 % higher efficiency than the threshold controller while using one third of its control energy and producing actuation that is 25× spatially smoother.

---

## Repository contents

```
VIFD-Thermal-Control/
│
├── VIFD_Thermal_Control_Full_Research_Notebook.ipynb   # Full 15-section research notebook
│
├── figures/                # All manuscript figures (PDF + PNG)
│   ├── fig_01_domain_setup.*
│   ├── fig_02_coupled_evolution.*
│   ├── fig_03_convergence.*
│   ├── fig_04_information_coupling.*
│   ├── fig_05_uncertainty_validation.*
│   ├── fig_06_scenarios.*
│   ├── fig_07_sensitivity_pareto.*
│   ├── fig_08_baseline_trajectories.*
│   ├── fig_09_final_temperature_fields.*
│   ├── fig_10_quantitative_comparison.*
│   ├── fig_11_phase_space_attractor.*
│   └── fig_COMPOSITE_manuscript_figure.*
│
├── tables/                 # All manuscript tables (CSV + LaTeX)
│   ├── table1_parameters.csv
│   ├── table2_comparison.csv
│   ├── table3_pareto.csv
│   ├── table4_scenarios.csv
│   └── manuscript_tables.tex
│
├── outputs/
│   └── outputs_summary.txt  # Reproducibility record of all key numbers
│
└── README.md
```

---

## Physics model

**Temperature PDE**

$$\rho c_p \frac{\partial T}{\partial t} = k \nabla^2 T + Q(\mathbf{x},t) - h \pi (T - T_\infty)$$

**Decision-field PDE**

$$\frac{\partial \pi}{\partial t} + \mathbf{v}(T) \cdot \nabla \pi = D \nabla^2 \pi - \alpha(\pi - \pi_{\text{tgt}}(T)) - \beta \pi$$

The coupling is bidirectional: π actively cools T through the last term of the temperature equation, and T drives π through the relaxation target π_tgt(T) and the temperature-dependent velocity field v(T).

**Lyapunov bound:** The energy functional E(t) = ½ ‖π − π_tgt‖² decays at rate 2(α + β) = 7.40, guaranteeing exponential convergence.

---

## Novelties

1. Bidirectional T ↔ π coupling — π is a physical field, not a scalar signal
2. Temperature-dependent solenoidal advection v(T) — directional information propagation
3. Energy-penalty parameter β — a Pareto-optimal accuracy–energy trade-off with a smooth, monotone frontier
4. Mutual information I(T; π) trajectory — model-agnostic quantification of spatial coherence
5. Analytical uncertainty validation — numerical variance tracks σ² = 4Dt
6. Phase-space attractor analysis — VIFD as a dynamical system with a bounded attractor
7. Lyapunov rate 2(α + β) proved and numerically verified
8. Four-scenario robustness (steady, step surge, oscillating, fault+heal) — 1.5 °C spread with no re-tuning
9. Fair five-way comparison (Threshold, Proportional, PID, MPC-lite, VIFD) in the same physical model

---

## How to run

The notebook is designed for **Google Colab + Google Drive**.

1. Upload `VIFD_Thermal_Control_Full_Research_Notebook.ipynb` to Google Drive.
2. Open it in Google Colab.
3. Mount your Drive when prompted — outputs are saved to:
   ```
   /content/drive/MyDrive/Outputs/VIFD_EnergyAI_Submission/
   ```
4. Run all cells in order. The full 15-section notebook completes in approximately 15–20 minutes on a standard Colab CPU runtime.

**Dependencies** (all pre-installed on Colab):

```
numpy  scipy  matplotlib  pandas
```

No additional packages are required.

---

## Numerical configuration

| Parameter | Value | Stability condition | Status |
|---|---|---|---|
| Grid | 100 × 100 | — | — |
| Δx | 0.0100 m | — | — |
| Δt | 0.0006 s | — | — |
| k (thermal) | 0.01 W m⁻¹K⁻¹ | r_T = 0.060 ≤ 0.25 | ✓ |
| D (diffusion) | 0.00035 | r_π = 0.0021 ≤ 0.25 | ✓ |
| α (relaxation) | 3.5 | α·Δt < 1 | ✓ |
| β (energy penalty) | 0.2 | β·Δt < 1 | ✓ |
| v_max (advection) | 0.1414 | CFL = 0.0076 ≤ 1 | ✓ |
| Lyapunov rate | 2(α+β) = 7.40 | > 0 | ✓ |

---

## Citation

If you use this code or results in your work, please cite:

```bibtex
@article{guizani2026vifd,
  title   = {Variational Information Flow Dynamics: A Physics-Inspired {AI}
             Framework for Control of Complex Energy and Thermal Systems},
  author  = {Guizani, S. and Alshammari, T. and Hamam, H.},
  journal = {Energy and AI},
  year    = {2026},
  publisher = {Elsevier}
}
```

---

## Affiliations

- Alfaisal University, Riyadh, Saudi Arabia
- Université de Moncton, Moncton, Canada
- University of Ha'il, Ha'il, Saudi Arabia
- University of Johannesburg, Johannesburg, South Africa

---

## License

This repository is made available for academic reproducibility. Please contact the corresponding author for any use beyond replication of the published results.
