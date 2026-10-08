# Mie scattering notebook archive

Mathematica notebooks for spherical-particle scattering, including efficiencies, cross sections, albedo, asymmetry-related plots, and refractive-index/particle-size explorations.

## Functions and engineering applications

| Source | Observed function | Inferred application |
|---|---|---|
| `Mie_SingleParticleSize.nb` | Relative refractive indices, size parameters, Mie coefficients `an`/`bn`, and `QScat[a]`, `QAbs[a]`, `QExt[a]`, `albedo[a]` | Comparing spherical-particle optical response and checking numerical scattering models [1] |
| `MieCode_ExtALL_ver4_forIndividualQPlots.nb`, `...CPlots.nb`, `...AlbedoPlots.nb`, `...AsymFactorPlots.nb` | Specialized observable/plot notebook variants | Exploring efficiency, cross-section, and angular-scattering trends |
| `MieCode_ExtALL_ver8.7_2.1to2.4index.nb`, `ForAppendix_ext*.nb` | Historical sweep/appendix variants | Candidate parameter comparisons; verify actual active inputs in each revision |

Potential engineering applications include scattering-layer design, particle sizing, optical diffuser screening, and benchmarks for FDTD calculations [1,2]. Single-particle Mie calculations alone do not predict multiple-scattering layers or complete OLED device efficiency.

## Example function calls

Requires Mathematica. With the current directory set to the repository root:

```wolfram
nb = NotebookOpen[FileNameJoin[{Directory[], "Mie_SingleParticleSize.nb"}]];
```

Evaluate the model-parameter, derived-quantity, coefficient, and observable-definition cells before these calls:

```wolfram
N[{QScat[r], QAbs[r], QExt[r]}]
N[albedo[r]]
N[QExt[r] - QScat[r] - QAbs[r]]
```

`r` is the notebook’s configured sphere radius. Its parameter annotations use **micrometers** for radius and vacuum wavelength; preserve a consistent length convention. The outputs are dimensionless efficiencies, the scattering/extinction ratio, and an extinction-balance check expected to be near zero within numerical precision. Check the medium absorption assumptions before applying familiar lossless-host identities. These calls match source definitions but were not evaluated in a Wolfram runtime here.

## Version selection and limitations

Start with the substantive `Mie_SingleParticleSize.nb`, not the numerically latest filename. **`MieCode_ver10_Doversion_withexport.nb` contains only an incomplete `Do` expression**, not a production Mie/export engine. The earlier `MieOptics_README.md` also names critical-angle/exit-cone files that are not present in this repository; it is retained as historical documentation, and those capabilities are not claimed here.

Inspect complex-index sign conventions, series truncation (Wiscombe is explicitly cited in the notebook), host-medium assumptions, and small/large size-parameter convergence. Compare against an independent Mie implementation and limiting cases. The available `CITATION.cff` can be consulted for project metadata, but bibliographic identifiers should be checked before reuse. Notebook names do not establish an externally published or validated release.

## Review scope and software citation

Documentation reviewed on 2026-10-08 against source commit [`62d5829d4b62`](https://github.com/gmongell/Mathematica_MS_Dissertation_UR_ChemE/tree/62d5829d4b6219102760055f49e09a170991237b). “Observed” means supported by source inspection; engineering applications are reasoned possibilities unless explicitly demonstrated. Scholarly references provide methodological context and do not certify these implementations. Runtime validation is stated separately above.

For software attribution, cite Guy Francis Mongelli, *Mathematica_MS_Dissertation_UR_ChemE*, the [repository](https://github.com/gmongell/Mathematica_MS_Dissertation_UR_ChemE), the exact commit used, and your access date. Also cite the relevant method publications and any original third-party contributors. No unverified software DOI or release version is assigned by this documentation.

## Scholarly references

1. W. J. Wiscombe (1980). “Improved Mie scattering algorithms.” *Applied Optics* 19, 1505–1509. [DOI: 10.1364/AO.19.001505](https://doi.org/10.1364/AO.19.001505). Reference for numerical Mie series evaluation and truncation.

2. A. F. Oskooi et al. (2010). “Meep: A flexible free-software package for electromagnetic simulations by the FDTD method.” *Computer Physics Communications* 181, 687–702. [DOI: 10.1016/j.cpc.2009.11.008](https://doi.org/10.1016/j.cpc.2009.11.008). Method/software reference for FDTD, dispersive media, sources, and boundary conditions.

## Ownership and existing license notices

Copyright (c) 2025 Guy Francis Mongelli

The existing project notice declares Apache License 2.0 for project code. Documentation, prose, and figures are declared CC BY 4.0; notebook code cells are Apache-2.0 and narrative/figures CC BY 4.0. Preserve all file-level and third-party notices. This README update does not change ownership or licensing terms.
