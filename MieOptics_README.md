# Mie Scattering & Exit-Cone Optics — Mathematica Notebooks

This repository contains a coherent set of Wolfram *Mathematica* notebooks for computing and visualizing (i) single-particle Mie scattering observables and (ii) exit-cone/critical-angle relationships relevant to optical outcoupling. The materials are suitable both for instructional use (e.g., ChE 495) and for research-grade parameter sweeps with exportable figures.

> **Scope.** The workflows cover extinction/scattering/absorption efficiencies, single-scattering albedo, asymmetry factor, and index/size parameter sweeps, alongside geometric-optics conversions among numerical aperture (NA), f-number, and exit-cone angles.

---

## Repository Contents

- `ChE495_CriticleAnglePlots.nb`  
  Critical-angle exploration and plotting. Computes and visualizes \(\theta_c = \arcsin(n_2/n_1)\) and related outcoupling angles. *(File name has a likely typo: “Criticle” → “Critical”.)*

- `ChE495_ExitConeConversions.nb`  
  Conversions among **NA**, **f-number (f/#)**, and **cone half-angle**. Includes forward and inverse relations (e.g., NA = \(n\sin\theta\), paraxial \(\text{f/#}\approx 1/(2\,\mathrm{NA})\)).

- `Mie_SingleParticleSize.nb`  
  Single-radius (or diameter) Mie calculations and plots. Good for validating lineshapes and peak positions versus wavelength or refractive index.

- `MieCode_ver10_Doversion_withexport.nb`  
  Production-oriented Mie engine with export cells enabled (e.g., PNG/PDF/SVG). Supports scripted sweeps over size parameter and refractive index.

- `MieCode_ExtALL_ver8.7_2.1to2.4index.nb`  
  Extended sweeps for real indices in the 2.1–2.4 range (imaginary part optional/parameterized). Aggregates multiple observables (Q_ext, Q_sca, Q_abs, ω₀, g).

- `MieCode_ExtALL_ver4_forIndividualQPlots.nb`  
  Targeted plots for **efficiencies** (Q terms) with per-observable styling and legends.

- `MieCode_ExtALL_ver4_forIndividualAlbedoPlots.nb`  
  Targeted plots for **single-scattering albedo** (ω₀ = Q_sca / Q_ext) across sweeps.

- `MieCode_ExtALL_ver4_forIndividualAsymFactorPlots (3).nb`  
  Targeted plots for **asymmetry factor** (g = ⟨cos θ⟩). *(Filename contains spaces and parentheses; quote paths if scripting.)*

- `MieCode_ExtALL_ver2.nb`  
  Earlier extended version retained for provenance/regression comparison.

> If you prefer a tidy layout, consider placing `.nb` files under `notebooks/` and using `src/`, `data/`, and `output/` as outlined below.

---

## Theory & Conventions (Brief)

- **Size parameter:** \(x = 2\pi a/\lambda\), where *a* is radius and \(\lambda\) is wavelength in the **embedding medium**.
- **Complex refractive index:** \(m = n + i\kappa\) for the particle; medium index \(n_{\text{med}}\). Many routines use the **relative index** \(m_r = m / n_{\text{med}}\).
- **Efficiencies:** \(Q_{\text{ext}}, Q_{\text{sca}}, Q_{\text{abs}}=Q_{\text{ext}}-Q_{\text{sca}}\). Cross-sections follow \(C = Q\,\pi a^2\).
- **Single-scattering albedo:** \(\omega_0 = Q_{\text{sca}}/Q_{\text{ext}}\).
- **Asymmetry factor:** \(g = \langle \cos\theta \rangle\), via phase function integration.
- **Critical angle:** for transmission from \(n_1\) to \(n_2<n_1\), \(\theta_c = \arcsin(n_2/n_1)\).
- **Exit-cone / NA / f/# relations:** \(\mathrm{NA} = n\sin\theta\), and in the paraxial limit \(\text{f/#} \approx 1/(2\,\mathrm{NA})\). Use exact relations for larger angles.

All notebooks assume consistent units; common choices are **nm** for wavelength and **µm** for radius/diameter, with conversions applied explicitly in code.

---

## Requirements

- Wolfram *Mathematica* **14.x** recommended (13.x typically works).  
- Optional paclets: plotting and units helpers (install on demand via `PacletInstall`).

**Performance tips**
- Disable history: `$HistoryLength = 0;`
- Use `SetDirectory@NotebookDirectory[];` to make paths reproducible.
- For large size-parameter sweeps, prefer vectorized table generation and preallocation of arrays.

---

## Quick Start

1. **Clone**
   ```bash
   git clone <repo-url>.git
   cd <repo-name>
   ```

2. **Open any notebook** in *Mathematica* and evaluate the **Setup/Environment** cell first:
   ```wolfram
   SetDirectory@NotebookDirectory[];
   $HistoryLength = 0;
   ```

3. **Run the computation** section(s), then the **Plot/Export** cells. Where present, “Export” cells will write to `./output/figures` automatically.

---

## Recommended Folder Structure (Optional)

```
.
├── notebooks/
│   ├── ChE495_CriticalAnglePlots.nb
│   ├── ChE495_ExitConeConversions.nb
│   ├── Mie_SingleParticleSize.nb
│   ├── MieCode_*.nb
├── data/
│   └── (optical constants, CSV/JSON tables, etc.)
├── src/
│   └── (shared .wl/.m utilities for Mie, plotting, unit conversions)
└── output/
    ├── figures/
    └── tables/
```

To ensure output paths exist:
```wolfram
SetDirectory@NotebookDirectory[];
dirs = FileNameJoin /@ {{".","output"}, {".","output","figures"}, {".","output","tables"}};
Scan[If[!DirectoryQ[#], CreateDirectory[#, CreateIntermediateDirectories -> True]] &, dirs];
```

---

## Parameterization & Data

- **Indices.** Supply \(n(\lambda), \kappa(\lambda)\) for both particle and medium. If using tabular data, load via `Import["data/index_table.csv"]` and interpolate.
- **Wavelength grids.** Use dense, uniform or log-spaced grids depending on resonance density; keep step sizes small for high-\(x\) regimes.
- **Radius vs diameter.** The notebooks accept either—ensure the size parameter uses **radius** \(a\).

**Example (template)**
```wolfram
(* Degrees-safe helpers and setup *)
SetDirectory@NotebookDirectory[]; $HistoryLength = 0;

(* Input parameters *)
nmed = 1.0; (* medium index *)
a    = 200*10^-9; (* radius in meters *)
λnm  = Range[400, 800, 2]; λ = λnm*10^-9; (* wavelength grid in meters *)

(* Example constant particle index (replace with table/interp) *)
m    = 2.2 + 0.02 I; mr = m/nmed;

x    = 2 Pi a/λ; (* size parameter array *)

(* Call your Mie core here, e.g., MieQ[x, mr] -> {Qext, Qsca, g, ...} *)
(* ... produce plots and Export[...] to output/figures *)
```

---

## Exit-Cone / Critical-Angle Conversions

Key relations used in the `ChE495_*` notebooks:
- **Critical angle:** \(\theta_c = \arcsin(n_2/n_1)\) for \(n_1>n_2\).
- **External collection cone:** for a medium of index \(n\), half-angle \(\theta\) has \(\mathrm{NA}=n\sin\theta\).
- **f-number (paraxial):** \(\text{f/#}\approx 1/(2\,\mathrm{NA})\).  
For high-NA systems use exact aperture geometry rather than paraxial approximations.

---

## Batch / Headless Runs (Optional)

For reproducible exports in CI or server environments, migrate compute kernels to `.wl` scripts (e.g., `src/build_figures.wl`) and run:

```bash
wolframscript -file src/build_figures.wl
```

Parameterize via `$ScriptCommandLine` to sweep sizes, indices, and wavelength bands.

---

## Known Limitations & Tips

- **Very large \(x\):** increase summation order and precision to avoid truncation artifacts in series terms.
- **Highly absorbing media:** ensure robust handling of complex arguments; verify conventions for the relative index.
- **Mesh resolution for plots:** use adequate sampling near resonances to capture narrow features.
- **Filename safety:** avoid spaces/parentheses in output filenames for easier scripting (one file currently includes them and will work, but quoting is required).

---

## Version Control

Add large binary notebooks to Git LFS and keep render artifacts out of the repo:

```
# .gitattributes
*.nb filter=lfs diff=lfs merge=lfs -text
```

```
# .gitignore
/output/
/data/raw/
*.DS_Store
```

---

## License

Choose and include a license file (e.g., MIT, BSD-3-Clause). If omitted, all rights reserved by default.

---

## Citation

Consider adding a `CITATION.cff`:

```yaml
cff-version: 1.2.0
title: "Mie Scattering & Exit-Cone Optics — Mathematica Notebooks"
authors:
  - family-names: Mongelli
    given-names: Guy Francis
version: 0.1.0
date-released: 2025-08-14
```

---

## Contact

Please open an Issue for questions, bugs, or enhancement requests.
