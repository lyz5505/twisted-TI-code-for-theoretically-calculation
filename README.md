# Theoretical Calculations for Twisted Topological-Insulator Films

Mathematica calculations and angle-resolved spectral data for twisted Bi₂Se₃ thin films, using a coupled-surface-state (CSS) model and its moiré continuum extension.

The notebook collects the original calculations for Extended Data Figures **E2–E3** and Supplementary Figures **S9–S13**: finite-thickness band structures, moiré bands, unfolded spectral functions, layer projections, interface-potential tuning, Wilson loops, and averaging over a twist-angle distribution.

## Repository contents

```text
.
├── README.md
├── TwistedBS_E2_E3_S9-S13_clean.nb
└── spectrum different angle/
    ├── <angle>deg.csv
    ├── <angle> zoom in.csv
    └── test.csv
```

- **[TwistedBS_E2_E3_S9-S13_clean.nb](TwistedBS_E2_E3_S9-S13_clean.nb)** — the main Mathematica notebook. Model definitions and unfolding functions are embedded; separate source notebooks are not required.
- **[spectrum different angle/](spectrum%20different%20angle/)** — 301 CSV files: 200 standard per-angle spectra, 100 higher-resolution spectra, and an additional `test.csv` calculation file outside the named angle grid.

## Requirements

- Wolfram Mathematica. The notebook was saved with Wolfram 14.2, and numerical checks were performed with version 14.2.1. Compatibility with earlier versions has not been verified.
- Sufficient memory for the moiré Hamiltonians. With the default shell cutoff of 5, the 3+1 and 3+5 Hamiltonians have dimensions 1456 × 1456 and 2912 × 2912, respectively.
- The original calculations use `ParallelTable`; Mathematica may launch parallel kernels when these cells are evaluated.

**Additional input for S12:** `peakfit - Sheet1.csv`, containing the measured angular-intensity distribution, must be placed alongside the notebook. This file is **not currently included in this repository**. The supplied spectral CSV files alone are insufficient to reproduce the experimental-distribution-weighted average.

## Getting started

1. Download or clone this repository, retaining the folder structure above.
2. Open `TwistedBS_E2_E3_S9-S13_clean.nb` in Mathematica.
3. Start with a fresh kernel and evaluate Section 0, followed by the required sections in order. The first cell clears variables in the `Global` context.
4. To select the 3+1 twist angle, edit the settings in the first input cell:

   ```wl
   theta31 = 1.;  (* Twist angle in degrees *)
   moireShell = 5;
   ```

5. Run Sections 1–4 for untwisted reference bands and the selected single-angle 3+1 calculation. Continue with the later sections for interface-potential tuning, Wilson loops, the 3+5 comparison, or angle averaging.

The 3+1 calculation runs **one twist angle at a time**. Changing `theta31` requires reevaluating the model construction and subsequent spectral calculations. E3 and S13 rebuild their models at a fixed angle of **1°**, independently of `theta31`.

## Notebook sections and manuscript figures

| Section | Calculation | Figure |
|---|---|---|
| 0 | Parameters and original CSS model definitions | Shared setup |
| 1 | Untwisted 4QL and 8QL band dispersions | S9 |
| 2 | Interface-displacement data and Fourier fits | Shared moiré setup |
| 3 | Single-angle 3+1 moiré Hamiltonian and bands | E2, S10, S11 |
| 4 | Original unfolding functions and single-angle 3+1 spectra | E2, S10, S11 |
| 5 | Interface onsite potential, band inversion, and Γ-point gaps at 1° | E3(a–d) |
| 6 | Original Wilson-loop and Wannier-center-flow calculations | E3(e–f) |
| 7 | 3+5 bands and unfolded spectra at 1°, with layer projections | S13 |
| 8 | Weighted averaging of existing per-angle spectra | S12 |

The figure mapping follows the actual figure captions in the manuscript, rather than its earlier table-of-contents numbering.

For the multi-angle panels, select the corresponding angle in the single-angle 3+1 calculation:

- **E2:** 2°, 3°, and 5°.
- **S10:** 0.6°, 0.8°, and 1.0°.
- **S11:** 0.2°, 0.4°, and 0.5°.

The notebook does not automatically loop over these angles or assemble every multi-angle panel in one run. S13 includes spectra for all components, the top 3QL, and the topmost 1QL.

## Spectral data

CSV files have the following columns:

| Column | Meaning |
|---|---|
| `kpath` | Cumulative distance along the external momentum path, in Å⁻¹ |
| `E_meV` | Energy in meV |
| `Ak_all` | Unfolded spectral intensity summed over all components |
| `Ak_top` | Unfolded spectral intensity projected onto the top 3QL |

**`<angle>deg.csv`** files use angles in degrees. The standard averaging grid covers −5° to 15° in 0.1° steps, excluding zero. These spectra use an energy grid from −40 to 40 meV in 0.5 meV steps and the external path 0.1K → Γ → 0.1M.

**`<angle> zoom in.csv`** files are a separate higher-resolution calculation, with momentum broadening, an energy grid from −15 to 15 meV in 0.1 meV steps, and the shorter path 0.025K → Γ → 0.025M. Some integer-angle filenames retain Mathematica formatting, such as `1. zoom in.csv`. These files are not mixed into the standard angle average in Section 8.

Section 8 reads existing standard spectra and applies the original interpolation and angular-bin weights. It calculates averages over **−5° to 15°** and **−5° to 5°**, followed by a central enlargement of the latter. It does not recompute spectra at additional angles.

## Numerical conventions

- Hamiltonian energies and spectral CSV energies are in **meV**; momenta are in **Å⁻¹**.
- The single-angle 3+1 band plot retains the original `/1000` conversion, so that plot uses **eV**.
- The E3 potential is applied to both QLs adjacent to the 3QL/1QL interface through `Hallcp[k, 1, mu, mu]`. Representative values are 0, 25.164, and 30 meV.
- Section 6 retains the original `INTDOT`, `Tran2`, and `WTPBUT` Wilson-loop implementation. Its default grid is `num = 100`, `num2 = 200`.
- The calculations retain the original parameters, variable names, Fourier fits, and eigensystem-selection conventions. Changing the plane-wave cutoff, momentum grid, or retained eigenstates requires checking convergence.

## Reproduction checks

Small-scale checks of the assembled original calculations confirmed Hermitian 3+1 and 3+5 Hamiltonians, numerical unfolded spectra, interface-gap calculations, and a coarse-grid Wilson loop. The original parameters give untwisted Γ-point gaps of approximately **16.67 meV for 4QL** and **2.04 meV for 8QL**.

These checks do not constitute a full rerun of every manuscript panel. Full-resolution calculations, convergence checks, and visual comparison with the manuscript remain necessary for complete figure reproduction. The complete S12 weighted average additionally requires the angular-distribution input noted above.
