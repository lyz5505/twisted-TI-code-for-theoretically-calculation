# Twisted TI: Theoretical Calculations

Mathematica code and spectral data for twisted Bi2Se3 films, based on a coupled-surface-state model. The notebook contains calculations for **E2, E3, and S9-S13**.

## Files

- `TwistedBS_E2_E3_S9-S13_1deg_k_broadening_shared.nb`: model definitions, band structures, unfolded spectra, layer projections, Wilson loops, and angle averaging.
- `spectrum different angle/`: precomputed spectral data in CSV format.

## Usage

Open the notebook in Mathematica and evaluate the sections in order, starting with a fresh kernel. Edit the first cell to choose the single 3+1 twist angle:

```wl
theta31 = 1.;  (* Angle in degrees *)
moireShell = 5;
```

E2, S10, and S11 share the single-angle calculation. E3 and S13 use a fixed angle of 1 degree. S9 compares untwisted 4QL and 8QL films; S12 averages existing spectra.

The 3+1 and 3+5 spectra include energy and momentum broadening. All layer projections share the same eigensystems to avoid repeated diagonalization.

CSV columns are `kpath`, `E_meV`, `Ak_all`, and `Ak_top`. To run S12, also place `peakfit - Sheet1.csv` beside the notebook; this input is not yet included in the repository.

The notebook was saved with Wolfram 14.2.
