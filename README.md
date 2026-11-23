readme_text = """# Untargeted LC-MS Metabolomics Analysis of Inflammatory Bowel Disease (IBD)

## Overview
End-to-end Python bioinformatics pipeline for untargeted negative-mode (`[M-H]-`) LC-MS metabolomics profiling comparing **Control (`CD`)** vs. **Disease (`HC`)** cohorts alongside **Pooled Quality Control (`QC`)** samples (`n = 10` `.mzML` runs).

## Pipeline Workflow
1. **Spectral Inspection & Feature Detection (`asari 2.0.0`):**
   - Verified negative ionization mode (`[M-H]-`) and `0.3 ppm` mass accuracy via `asari peek`.
   - Performed retention time alignment (calibrated against `QC_PREFA02`) and peak detection across all 10 samples, yielding **6,832 raw metabolite features**.
2. **Quality Control & Data Preprocessing (`pandas`, `numpy`):**
   - Filtered features present in $\ge 50\%$ of biological samples (`4/8`) and retained reproducible features with **Pooled QC Relative Standard Deviation ($\text{RSD} < 30\%$)**, resulting in **2,263 high-confidence features**.
   - Applied half-minimum missing-value imputation, sample-wise median normalization, and $\log_2$ transformation.
3. **Multivariate & Differential Abundance Analysis (`scikit-learn`, `scipy`, `statsmodels`):**
   - **PCA:** Confirmed tight technical clustering of `QC_PREFA02` and `QC_PREFB02` and clear biological separation between Control and Disease along **PC1 (`29.0%` variance)**.
   - **Differential Testing:** Identified **386 significantly altered features** (`197 Up in Disease`, `189 Down in Disease`; $\vert{}\log_2\text{FC}\vert{} \ge 1.0$, Welch's $t$-test $p < 0.05$, Benjamini–Hochberg FDR correction).
4. **Empirical Compound & Adduct Deconvolution (`khipu` / `asari deconvolute`):**
   - Grouped **4,212 features into 1,442 empirical compounds (`khipus`)**, resolving $^{13}\text{C}/^{12}\text{C}$ isotopologues, formate (`[M+HCOO]-`), and sodium formate adducts.

## Repository Structure
```text
LCMS_Untargeted_Metabolomics_IBD/
├── 01_IBD_Untargeted_Metabolomics_Pipeline.ipynb
├── README.md
└── results/
    ├── figures/
    │   ├── ibd_pca_plot.png
    │   └── ibd_volcano_plot.png
    └── tables/
        ├── asari_raw_feature_table.tsv
        ├── asari_khipu_empirical_compounds.tsv
        ├── normalized_log2_feature_matrix.csv
        └── ibd_annotated_metabolites.csv
