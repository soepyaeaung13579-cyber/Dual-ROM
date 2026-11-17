# Dual-Field Reduced-Order Modeling for Load-Agnostic Static Simulation and Boundary Stress Reconstruction

This repository contains the source code, interactive 3D simulation studio, and numerical validation suite accompanying the manuscript **"Dual-Field Reduced-Order Modeling for Load-Agnostic Static Simulation and Boundary Stress Reconstruction"** (*Advanced Modeling and Simulation in Engineering Sciences*).

The repository includes two interactive Python applications:

1. **Dual-ROM Framework (Method II & Method III)**:
   - File: `Dual_ROM_Method_II_III.py`
   - Pre-computes a dual nodal stress basis ($\mathbf{\Psi} = \mathcal{L}(\mathbf{\Phi})$) offline via Gauss-point stress extrapolation with inverse-distance nodal averaging, reducing online 3D Cauchy stress reconstruction to a single matrix-vector multiplication.
   - Implements both **Uniform Snapshot Sampling (USS, Method II)** and **Boundary-Clustered Snapshot Sampling (BCSS, Method III)** to resolve localized boundary-layer stress gradients near structural supports.

2. **Traditional Displacement ROM (Method I)**:
   - File: `Traditional_Displacement_ROM_Method_I.py`
   - Conventional baseline POD-ROM approach that projects only the displacement field and reconstructs stresses online via the full element-level strain-displacement operator and Gauss-point extrapolation.

Both applications are built with **PyQt6** for the ANSYS-Workbench-style workflow GUI and **PyVista/VTK** for real-time 3D rendering of finite element meshes, boundary conditions, consistent nodal loads, and full 3D stress/error contours.

---

## What's New (v1.1.0 — Manuscript Revision Update)

- **Consistent Shape-Function Quadrature Load Distribution (`_compute_transverse_line_weights` & `define_loads_at_pos`)**:
  - Automatically detects element order and distributes transverse line loads across the top surface using consistent finite element edge quadrature:
    - **Quadratic Elements (`Hexa20`, `Tet10`)**: Consistent `1:4:1` Simpson quadrature weighting ($[1, 4, 2, 4, \dots, 4, 1]$) on primary element planes.
    - **Linear Elements (`Hexa8`, `Tet4`)**: Consistent `1:1` trapezoidal quadrature weighting ($[1, 2, \dots, 2, 1]$).
- **Singular-Zone-Excluded Stress NMAE ($\text{NMAE}_{\sigma, R > 2h}$)**:
  - Evaluates stress reconstruction accuracy both across the **Full 3D Domain** and with the **2-element-layer contact singularity neighborhood ($R \le 2h$) excluded** around the applied line load while preserving fixed/pinned support boundaries, confirming mesh-independent stress recovery outside localized load singularities.
- **Step 7 — Out-of-Span Predictive Generalization & Linear Independence Suite**:
  - Records offline training snapshot coordinates ($\mathbf{x}_{\text{train}}$) and computes the orthonormal basis and numerical rank of the training load matrix $\mathbf{F}_{\text{train}}$.
  - Automatically generates and evaluates $N = 50$ strictly out-of-span unseen test load positions satisfying $\text{rank}([\mathbf{F}_{\text{train}}, \mathbf{f}_{\text{test}}]) = \text{rank}(\mathbf{F}_{\text{train}}) + 1$ ($\mathbf{f}_{\text{test}} \notin \text{span}(\mathbf{F}_{\text{train}})$).
  - Pre-factorizes the reduced stiffness matrix for rapid batch evaluation, plotting spatial error profiles and statistical distributions, with one-click CSV export for manuscript tables.
- **Interactive Mode Reconstruction**:
  - Added user-selectable POD mode count (`Disp Modes`) and fast `⚡ Reconstruct ROM` button to perform parametric sensitivity studies on SVD energy thresholds ($\eta$) without re-solving full-order snapshots.

---

## Features

- **3D Solid Continuum Elements**: `Hexa8`, `Hexa20` (Full 27-pt, Reduced 8-pt, Irons 14-pt), `Tet4`, `Tet10`
- **Boundary Conditions**: Cantilever, Fixed-Fixed, Simply Supported, Elastic Foundation (Winkler surface quadrature)
- **Snapshot Sampling Strategies**: Uniform Snapshot Sampling (`USS`) and Boundary-Clustered Snapshot Sampling (`BCSS`)
- **Stress Recovery**: Gauss-point stress extrapolation with weighted nodal averaging across all 6 Cauchy stress components ($\sigma_{xx}, \sigma_{yy}, \sigma_{zz}, \tau_{xy}, \tau_{yz}, \tau_{zx}$), von Mises, Max Principal, and Tresca failure criteria
- **7-Step Interactive Workflow**:
  1. Geometry & Meshing
  2. Loads & Boundary Conditions
  3. Full-Order Model (FOM) Solver & Equilibrium Verification
  4. 3D Post-Processing & Failure Safety Factor Contours
  5. Offline ROM Training & SVD Energy Spectrum Analysis
  6. Single-Load ROM vs. FEM Validation (Full-Domain & Singular-Zone-Excluded NMAE)
  7. Multi-Load Out-of-Span Generalization & Rank Independence Study (50 Unseen Cases + CSV Export)

---

## Quick Start

```bash
# Clone the repository
git clone https://github.com/soepyaeaung13579-cyber/Dual-ROM.git
cd Dual-ROM

# Install dependencies (virtual environment recommended)
pip install -r requirements.txt

# Run the Dual-ROM studio (Method II & Method III)
python "Dual_ROM_Method_II_III.py"

# Or run the Traditional Displacement ROM baseline (Method I)
python "Traditional_Displacement_ROM_Method_I.py"
```

---

## Repository Structure

```text
.
├── Dual_ROM_Method_II_III.py                # Dual-ROM Studio (Methods II & III)
├── Traditional_Displacement_ROM_Method_I.py # Traditional Displacement ROM Studio (Method I)
├── requirements.txt                         # Python package dependencies
├── CITATION.cff                             # GitHub Citation metadata file
├── LICENSE                                  # MIT License
├── .gitignore
└── README.md                                # Project documentation
```

---

## Citation

If you use this software or methodology in your research, please cite both the research article and software repository below (or click **"Cite this repository"** on the GitHub sidebar):

### Journal Article (BibTeX)
```bibtex
@article{dual_rom_amses2026,
  title   = {Dual-Field Reduced-Order Modeling for Load-Agnostic Static Simulation and Boundary Stress Reconstruction},
  author  = {Aung, Soe Pyae and {CSE Lab}},
  journal = {Advanced Modeling and Simulation in Engineering Sciences},
  year    = {2026},
  note    = {Manuscript ID: ee109cb6-ccc0-4afc-89aa-6dd49290c89a}
}
```

### Software Repository (BibTeX)
```bibtex
@software{dual_rom_software2026,
  title   = {Dual-ROM: Dual-Field Reduced-Order Modeling Framework for 3D Structural Simulation and Boundary Stress Reconstruction},
  author  = {Aung, Soe Pyae and {CSE Lab}},
  year    = {2026},
  version = {1.1.0},
  url     = {https://github.com/soepyaeaung13579-cyber/Dual-ROM}
}
```

---

## License

This project is licensed under the MIT License — see the [LICENSE](LICENSE) file for details.
