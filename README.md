# Stage Three HackBio : SARS-CoV-2-scRNA-seq Trajectory Analysis
Reference:
Ravindra et al., “Single-cell longitudinal analysis of SARS-CoV-2 infection in human bronchial epithelial cells,” PLOS Biology (2021).
https://journals.plos.org/plosbiology/article?id=10.1371/journal.pbio.3001143


## Overview

This analysis reproduces the trajectory and cell-type dynamics described in Ravindra et al. (2021), using single-cell RNA-seq data from human bronchial epithelial cells infected with SARS-CoV-2 at multiple time points (mock, 1dpi, 2dpi, 3dpi).

The goal is to reconstruct cellular states, identify key genes (e.g., ACE2, ENO2), and analyze infection-driven transcriptional changes.


## Objectives

1. Identify major cell types (e.g., basal, ciliated, goblet, ionocyte, immune-like).

2. Examine transcriptional dynamics across infection stages (mock → 3dpi).

3. Perform pseudotime analysis to trace infection-induced differentiation trajectories.

4. Compare ACE2 vs. ENO2 expression as biomarkers of infection and metabolic activity.

5. Visualize transcriptional shifts using UMAP, violin plots, and pseudotime gradients.


## Dataset

Source: GSE166766 (NCBI GEO)

Files: .mtx, .tsv format (10X Genomics output).

--------Sample Description Approx................Cells

GSM5082289_mock	Uninfected control.......~17k

GSM5082290_1dpi	1 day post-infection.....~18k

GSM5082291_2dpi	2 days post-infection....~20k

GSM5082292_3dpi	3 days post-infection....~22k


## Pipeline Summary

The notebook runs a full Scanpy-based scRNA-seq analysis pipeline on TPU (Google Colab compatible).

Step Description
1. Setup & Imports	Installs scanpy, scrublet, decoupler, etc.
2. Load Data	Reads .mtx + .tsv for mock, 1dpi, 2dpi, 3dpi separately
3. QC Filtering	Filters cells with <200 genes, >10% mitochondrial reads
4. Normalization & HVG Selection	Library-size normalization + log transform + top 2000 HVGs
5. PCA, Neighbors, UMAP	Dimensionality reduction for visualization
6. Clustering (Leiden)	Detects transcriptomic communities
7. Cell Annotation	Uses PanglaoDB + CellMarker references (airway + immune + neuronal)
8. Pseudotime (DPT)	Orders cells along infection trajectory per condition
9. DE Analysis	Ranks top genes (mock → 3dpi progression)
10. Dropout Bias Visualizes ACE2 and ENO2 zero-inflation
11. Visualization & Summary	UMAPs, pseudotime trends, and summary table of results


## Biological Highlights

- ACE2 marks cells permissive to SARS-CoV-2 entry.

- ENO2 increases during late infection, reflecting metabolic reprogramming.

- Pseudotime trajectories capture progression from uninfected epithelial → interferon-responsive → infected states.

- The analysis supports that airway epithelium undergoes transcriptional remodeling as infection advances.


## Key Visualizations

- UMAP — shows clustering by condition and infection state.

- Pseudotime Gradient — traces infection trajectory (mock → 3dpi).

- ACE2 & ENO2 Trendlines — show gene expression vs. pseudotime.

- Dropout Histograms — assess bias in low-expression genes.


## Reproducibility

Dependencies are logged using:

        import session_info
        session_info.show()

Recommended environment:

- Python 3.9+

- Scanpy ≥ 1.9.3

- Decoupler ≥ 1.6.0

- Seaborn ≥ 0.13.0

- Matplotlib ≥ 3.8.0


## Credits

- Original dataset: Ravindra et al., PLOS Biology, 2021

- Analysis pipeline: Adapted and optimized by HackBio

- Tools: Scanpy, Decoupler, Matplotlib, Seaborn