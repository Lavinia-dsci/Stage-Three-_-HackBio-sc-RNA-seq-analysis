# Stage Three HackBio: SARS-CoV-2-scRNA-seq Trajectory Analysis
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

## Findings

### 1. Cell type identification across infection stages
   
Single-cell RNA-seq of SARS-CoV-2–infected human bronchial epithelial cells revealed distinct transcriptional changes at different infection stages. Basal and club cells were predominant early, but by 1 dpi, ciliated cells showed ISG expression, indicating viral detection. At 2 dpi, increased goblet and ciliated cells suggested mucus production and immune activity. By 3 dpi, ionocytes and some ciliated cells displayed high viral and metabolic gene expression, reflecting active infection or metabolic shifts, consistent with the virus's preference for targeting airway epithelial cells.

### 2. Cell type correlation with SARS-CoV-2 Infection

The infection trajectory (mock → 3 dpi) showed a shift from structural to secretory and metabolically active epithelial states. Ciliated and goblet cells, which express ACE2, are key infection targets, while basal cells mostly remain uninfected but may proliferate for tissue repair. Rare ionocytes have high metabolic activity and ACE2 expression, suggesting susceptibility and transcriptional changes during infection. These cell-type transitions in pseudotime indicate infection-driven airway epithelium reorganization, aligning with Ravindra et al. (2021).

### 3. ACE2 vs ENO2

ACE2 was mainly found in ciliated and ionocyte clusters at 2–3 dpi, but its presence does not reflect infection rates. Although ACE2 serves as the viral receptor, it appears in only some infected cells due to dropout bias in single-cell RNA-seq and possible downregulation after infection. Dropout histograms showed mostly zero counts for ACE2 in these clusters. Thus, ACE2 is more a marker of susceptibility than of infection level. ENO2 expression increased along pseudotime, peaking at 3 dpi, indicating metabolic reprogramming, as it is a glycolytic enzyme found in neuroendocrine and metabolically active cells. Its rise alongside viral and interferon-stimulated genes (like MX1, IFITM1, ISG15) points to a shift toward aerobic glycolysis, characteristic of viral replication and oxidative stress. ACE2 marks susceptibility to viral entry, while ENO2 indicates infection progression and metabolic adaptation, together providing a dual-marker model of cellular response.

### 4. Cluster with the highest abundance of ACE2 after 3dpi
   
Scanpy's DPT algorithm mapped cells along an infection trajectory from uninfected (mock) to late infection (3 dpi). Early pseudotime cells showed basal/club-like expression, while later cells (2–3 dpi) had increased ACE2, ENO2, and interferon-response genes, indicating a shift to a stressed epithelial phenotype. UMAP plots revealed a gradient of rising ACE2/ENO2 matching infection progression, with ACE2-high 3 dpi clusters aligning with ionocytes and ciliated cells, reflecting airway viral tropism.

This means that at 3 dpi, a unique UMAP cluster showed high ACE2 and ENO2 levels, indicating cells with major transcriptional changes. These cells display markers of active viral entry, metabolic shift, oxidative stress, and interferon response. This cluster likely consists of infected, stressed epithelial cells, possibly undergoing apoptosis. The presence of both ACE2 and ENO2 suggests infection drives metabolic activation to support viral replication.

### 5. Pseudotime Interpretation of SARS-CoV-2 Infection Progression

#### Mock (0 dpi) – Homeostatic Airway Organization

In the mock condition, the pseudotime trajectory is smooth and linear, with a clear progression from early to late states.
Distinct cell types are well separated: airway epithelial cells, ciliated cells, smooth muscle, and mesothelial cells occupy ordered positions along pseudotime.
This structure reflects normal differentiation and turnover of bronchial epithelium.
There is no evidence of transcriptional stress or branching, and metabolic and immune pathways remain quiescent.

Interpretation:
Mock represents a healthy, homeostatic airway epithelium with preserved lineage hierarchy and stable cell identity.

#### 1 dpi – Early Viral Perturbation and Activation

At one day post-infection, pseudotime becomes branched and slightly distorted, suggesting that subsets of cells begin to diverge transcriptionally.
Clusters annotated as pulmonary alveolar type II cells, airway epithelial cells, and ionocytes shift toward mid-to-late pseudotime, while basal and smooth-muscle populations remain stable.
This indicates localized viral entry and immune activation within the epithelial layer.
The late-pseudotime branch likely contains infected or interferon-stimulated cells beginning to alter transcription.

Interpretation:
1 dpi marks the onset of infection. A subset of ACE2-positive epithelial cells begins to sense and respond to viral RNA, initiating mild stress and antiviral transcription.

#### 2 dpi – Systemic Transcriptional Reprogramming

At 2 dpi, the pseudotime plot (your second image) shows a long, continuous gradient with an expanded late-pseudotime region.
Distinct epithelial subtypes; airway epithelial cells (1 & 4) and ionocytes (5) dominate this progression, while mesothelial and vascular smooth-muscle cells remain peripheral and transcriptionally inert.
A small, high-pseudotime tail (yellow in your figure) indicates a cluster of cells undergoing strong transcriptional divergence, probably representing heavily infected or apoptotic cells.

Interpretation:
2 dpi is the turning point: widespread antiviral signaling and metabolic activation reshape epithelial transcriptional identity while non-target structural cells remain relatively stable.

#### 3 dpi – Inflammation, Remodeling, and Cellular Breakdown

At 3 dpi, the pseudotime manifold becomes fragmented, losing the clear gradient seen earlier.
Distinct clusters appear for goblet cells, macrophages, airway epithelial, and smooth-muscle cells, indicating immune infiltration and epithelial remodelling.
Late-pseudotime regions are dominated by inflammatory epithelial and macrophage cells, while normal ciliated and basal populations shrink.

Interpretation:
By 3 dpi, infection has transitioned from replication to inflammation.
Transcriptomic order collapses, and airway structure gives way to an inflammatory microenvironment with strong immune and secretory remodelling.

#### Concise Conclusion

Pseudotime analysis across infection stages reveals the temporal unfolding of SARS-CoV-2 pathogenesis in airway epithelium, from ordered homeostasis to chaotic inflammation.
Infection initiates in ACE2-positive epithelial and ionocyte cells, triggers a metabolic surge (ENO2 ↑) and antiviral activation (2 dpi), and culminates in immune infiltration and epithelial collapse (3 dpi).
