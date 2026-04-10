[README.md](https://github.com/user-attachments/files/26618842/README.md)
# Data-Driven Mathematical Modelling Explains Altered Timing of EARLY FLOWERING 3 in the Wheat Circadian Oscillator

[![DOI](https://img.shields.io/badge/DOI-10.1098%2Frsif.2025.0619-blue)](https://doi.org/10.1098/rsif.2025.0619)
[![Journal](https://img.shields.io/badge/Journal-J._R._Soc._Interface-orange)](https://royalsocietypublishing.org/rsif/article/23/234/20250619/479317/Data-driven-mathematical-modelling-explains)
[![MATLAB](https://img.shields.io/badge/Language-MATLAB-red)](https://www.mathworks.com/products/matlab.html)
[![License](https://img.shields.io/badge/License-see%20journal-lightgrey)](#license)

This repository contains the MATLAB code accompanying the paper:

> **Upadhyay A**, Rowland-Chandler J, Stewart-Wood J, Pingarron-Cardenas G, Tokuda IT, Webb A, Locke JCW. *Data-driven mathematical modelling explains altered timing of EARLY FLOWERING 3 in the wheat circadian oscillator.* J. R. Soc. Interface **23**: 20250619 (2026). https://doi.org/10.1098/rsif.2025.0619

---

## Overview

Circadian rhythms are endogenous ~24 h cycles that allow organisms to anticipate daily environmental changes. In plants, circadian timing is maintained by a network of transcriptional regulators. While the architecture of this oscillator is broadly conserved, EARLY FLOWERING 3 (ELF3) — a key evening-complex component — is expressed at **dawn** in wheat rather than at **dusk** as in the model plant *Arabidopsis thaliana*.

This study uses data-driven mathematical modelling to explain how this shift arose. We developed two ODE-based models of the wheat circadian oscillator (a **compact model** and a **large model**) by incorporating a novel repression of ELF3 transcription by TOC1 into existing *Arabidopsis* clock frameworks. Promoter motif analysis revealed an abundance of TOC1 binding sites in wheat *ELF3* promoters, supporting this hypothesis. Parameter optimisation against experimental transcript-abundance time series shows that this single regulatory change is sufficient to recapitulate the dawn-phased ELF3 expression observed in wheat, and simulations predict that the oscillator is robust to this altered timing.

---

## Repository Structure

```
├── Fig1/                   # Experimental data panels (Fig 1A–D)
│   ├── Fig 1A/
│   ├── Fig 1B/
│   ├── Fig 1C/
│   └── Fig 1D/
│
├── Fig2/                   # Model schematics (Fig 2)
│
├── Fig 3/                  # Parameter optimisation & model fitting
│   ├── wheat_wt_compact_large_simulations.m   # ★ Main script
│   ├── wheat_wt_compact_abhi.m                # Compact model ODEs (9 eqs, 34 params)
│   ├── wheat_large_abhi.m                     # Large model ODEs (Fogelmark-based)
│   ├── CompactModel_Parameters_InitialCond.m  # Compact model parameters & ICs
│   ├── LargeModel_Parameters_InitialCond.m    # Large model parameters & ICs
│   ├── parms_test_ga_abhicompact.m            # GA optimisation wrapper (compact)
│   ├── parms_test_ga_abhilarge.m              # GA optimisation wrapper (large)
│   ├── cost_function_mga_abhicompact.m        # Cost function (compact)
│   ├── cost_function_mga_abhilarge.m          # Cost function (large)
│   ├── WebDataCompact.m                       # Peak/trough extraction (compact)
│   ├── WebDataLarge.m                         # Peak/trough extraction (large)
│   ├── smooth_avgdata_abhi.m                  # Oscillatory data smoothing
│   ├── webb_data_wt.csv                       # Experimental transcript data
│   └── Fig-3.pdf                              # Output figure
│
├── Fig 4/                  # TOC1→ELF3 repression simulations
│   ├── fig4plot_simulations.m                 # ★ Main script
│   ├── wheat_wt_compact_abhi.m                # Compact model ODEs
│   ├── wheat_large_abhi.m                     # Large model ODEs
│   ├── CompactModel_Parameters_InitialCond.m  # Parameters & ICs
│   ├── LargeModel_Parameters_InitialCond.m    # Parameters & ICs
│   ├── WebDataCompact.m / WebDataLarge.m      # Peak/trough data
│   ├── smooth_avgdata_abhi.m                  # Data smoothing
│   ├── webb_data_wt.csv                       # Experimental data
│   └── Fig-4A.pdf, Fig-4B.pdf                # Output figures
│
├── Fig 5/                  # Extended model analysis (Fogelmark framework)
│   ├── FogelmarkModel_Parameters_InitialCond.m  # Fogelmark model parameters & ICs
│   └── Fig-5A.pdf, Fig-5B.pdf, Fig-5C.pdf      # Output figures
│
└── Fig 6/                  # Additional analyses
```

---

## Models

### Compact Model
A minimal representation of the wheat circadian oscillator with **9 ODEs and 34 parameters**. Oscillator components (LHY/CCA1, PRRs/TOC1, Evening Complex) are merged into functional groups. Light input modulates transcription and protein degradation rates. The key addition is a **TOC1-mediated repression of ELF3** not present in *Arabidopsis* models.

### Large Model
A detailed biochemical model based on the Fogelmark *et al.* (2014) *Arabidopsis* framework, extended with the TOC1→ELF3 repression. It includes explicit representations of individual clock genes with transcriptional and protein–protein interactions. Parameters are optimised against a large experimental dataset.

---

## Getting Started

### Requirements

- **MATLAB** (tested with R2022b or later)
- **Global Optimization Toolbox** (for `gamultiobj` — multi-objective genetic algorithm)

### Running the Code

**Figure 3** — Optimised model fitting to experimental data:
```matlab
cd 'Fig 3'
run wheat_wt_compact_large_simulations.m
```

**Figure 4** — Effect of TOC1 repression on ELF3 timing:
```matlab
cd 'Fig 4'
run fig4plot_simulations.m
```

### Key Input Data

The file `webb_data_wt.csv` contains experimental transcript abundance time series for key wheat clock genes (LHY, ELF3, GI, LUX, PRR73, PRR5/TOC1) under light–dark cycles followed by constant light, used for model fitting and validation.

---

## Parameter Optimisation

Model parameters were optimised using the MATLAB **multi-objective genetic algorithm** (`gamultiobj`). The cost function simultaneously minimises two objectives:

1. The mean squared difference between experimental and simulated peak/trough timings of clock gene transcripts.
2. A barrier function ensuring sustained oscillations (penalising damped or arrhythmic solutions).

Optimisation is performed independently for the compact model (`parms_test_ga_abhicompact.m`) and the large model (`parms_test_ga_abhilarge.m`).

---

## Figures Reproduced

| Figure | Description | Main Script |
|--------|-------------|-------------|
| **Fig 3** | Optimised compact and large wheat models capture transcript dynamics of key clock genes | `wheat_wt_compact_large_simulations.m` |
| **Fig 4A** | TOC1 repression of ELF3 is required for correct ELF3 timing (wild type) | `fig4plot_simulations.m` |
| **Fig 4B** | Removing TOC1→ELF3 repression shifts ELF3 to evening expression | `fig4plot_simulations.m` |
| **Fig 5** | Extended analysis using the Fogelmark model framework | See `Fig 5/` |

---

## Citation

If you use this code, please cite:

```bibtex
@article{Upadhyay2026,
  author  = {Upadhyay, Abhishek and Rowland-Chandler, Jamila and Stewart-Wood, Julia and Pingarron-Cardenas, Gabriela and Tokuda, Isao T. and Webb, Alex and Locke, James C. W.},
  title   = {Data-driven mathematical modelling explains altered timing of {EARLY FLOWERING 3} in the wheat circadian oscillator},
  journal = {Journal of The Royal Society Interface},
  volume  = {23},
  number  = {234},
  pages   = {20250619},
  year    = {2026},
  doi     = {10.1098/rsif.2025.0619}
}
```

---

## Related Publications

- Rowland-Chandler J *et al.* (2023). Wheat EARLY FLOWERING 3 affects heading date without disrupting circadian oscillations. *Plant Physiology* **191**(2): 1383–1403. [DOI: 10.1093/plphys/kiac538](https://doi.org/10.1093/plphys/kiac538)
- Fogelmark K & Bhatt H (2014). Modelling the *Arabidopsis* circadian clock. *BMC Systems Biology* **8**: 1–13.
- De Caluwé J *et al.* (2016). A compact model for the complex plant circadian clock. *Frontiers in Plant Science* **7**: 74.

---

## License

This code accompanies a publication in the *Journal of The Royal Society Interface*. Please refer to the [journal's policies](https://royalsocietypublishing.org/rsif/for-authors) and the supplementary material terms for reuse conditions. If you use or adapt this code, please cite the paper above.

---

## Contact

For questions about the code or models, please contact the corresponding authors via the details provided in the [published paper](https://doi.org/10.1098/rsif.2025.0619).
