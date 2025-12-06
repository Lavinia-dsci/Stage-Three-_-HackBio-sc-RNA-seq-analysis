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
1dpi - We identified ionocytes, myofibroblasts, 
2dpi - We identified ionocyytes, myofibroblasts, vascular smooth cells, neurons, ciliated cells, alveolar macrophages, and airway goblet cells
3 dpi - Alveolar macrophages, airway goblets, ciliated cells, Clara cells

### 2. Cell type correlation with SARS-CoV-2 Infection

1 dpi — Early Infection and Viral Entry. 
At one day post-infection (1 dpi), the predominant cell types identified were ionocytes and myofibroblasts, reflecting the earliest molecular events of SARS-CoV-2 invasion. Ionocytes, although rare, are known to express exceptionally high levels of ACE2 and TMPRSS2, the viral entry receptors, making them prime targets for infection. Their activation at this stage suggests that SARS-CoV-2 initiates infection within this subpopulation of airway epithelial cells, consistent with their role in regulating ion transport and airway surface homeostasis. Concurrently, the appearance of myofibroblasts indicates an early tissue response to epithelial disruption. These cells, typically involved in extracellular matrix remodelling, likely respond to local cytokine release and begin early repair processes. Together, these findings suggest that 1 dpi represents the onset of infection, characterised by viral entry into ACE2-rich cells and the first wave of epithelial stress signalling.

2 dpi — Active Replication and Cellular Reprogramming. 
At two days post-infection (2 dpi), the cellular landscape became more heterogeneous, with the identification of ionocytes, myofibroblasts, vascular smooth muscle cells, neurons, ciliated cells, alveolar macrophages, and airway goblet cells. This stage corresponds to widespread viral replication and systemic transcriptional reprogramming. Ionocytes remained transcriptionally active, indicating persistent viral replication within ACE2⁺ cells. The presence of ciliated epithelial cells, another known viral target, marks loss of mucociliary defence as infection spreads across the bronchial surface. The emergence of alveolar macrophages reflects innate immune activation and cytokine release, while goblet cell expansion suggests early mucus hypersecretion—a protective but pathogenic response. Interestingly, neuronal-like cells appeared transcriptionally altered, which may relate to neuroepithelial signalling and early sensory dysfunction observed in COVID-19 (e.g., anosmia). Vascular smooth muscle and myofibroblast activation indicate evolving tissue remodelling and vascular inflammation, consistent with early hypoxic and fibrotic signalling. Altogether, 2 dpi represents the peak of infection activity, where viral replication, immune activation, and epithelial remodelling co-occur.

3 dpi — Inflammation, Remodelling, and Repair. 
By three days post-infection (3 dpi), the cellular composition shifted toward alveolar macrophages, airway goblet cells, ciliated cells, and Clara (club) cells, indicating a transition from viral replication to inflammation and tissue remodelling. Alveolar macrophages dominated, reflecting robust innate immune activity and potential contribution to cytokine-driven inflammation. The persistence and overrepresentation of goblet cells signify mucus hypersecretion and epithelial metaplasia, characteristic of severe airway inflammation. Ciliated cells, while still present, exhibited stressed and partially degraded transcriptional profiles, suggesting cytopathic effects and impaired regeneration. The emergence of Clara cells, which serve as progenitors capable of repairing epithelial damage, suggests that recovery and regeneration are beginning to occur even as inflammation persists. Thus, 3 dpi represents the transition to the host inflammatory and reparative phase, defined by immune dominance, secretory cell hyperplasia, and the first signs of epithelial regeneration.

In summary, across the infection timeline, we observe a temporal cascade of cellular responses: viral entry and sensing (1 dpi), widespread infection and metabolic stress (2 dpi), and inflammation with repair initiation (3 dpi). Ionocytes and ciliated cells mark susceptibility, macrophages and goblet cells reflect host response, and Clara cells symbolise tissue renewal. This continuum captures the pseudotemporal choreography of SARS-CoV-2 infection, bridging molecular dynamics with cellular pathology.

### 3. ACE2 vs ENO2

ACE2 was mainly found in ciliated and ionocyte clusters at 2–3 dpi, but its presence does not reflect infection rates. Although ACE2 serves as the viral receptor, it appears in only some infected cells due to dropout bias in single-cell RNA-seq and possible downregulation after infection. Dropout histograms showed mostly zero counts for ACE2 in these clusters. Thus, ACE2 is more a marker of susceptibility than of infection level. ENO2 expression increased along pseudotime, peaking at 3 dpi, indicating metabolic reprogramming, as it is a glycolytic enzyme found in neuroendocrine and metabolically active cells. Its rise alongside viral and interferon-stimulated genes (like MX1, IFITM1, ISG15) points to a shift toward aerobic glycolysis, characteristic of viral replication and oxidative stress. ACE2 marks susceptibility to viral entry, while ENO2 indicates infection progression and metabolic adaptation, together providing a dual-marker model of cellular response.

### 4. Cluster with the highest abundance of ACE2 after 3dpi

The violin/dot plot and accompanying statistical tests reveal that ACE2 expression varies significantly across cell clusters at 3 days post-infection (3 dpi), indicating that SARS-CoV-2 receptor expression is not uniform among airway cell types.

#### The Kruskal–Wallis test (H = 243.63, p = 9.41×10⁻⁵⁰) confirms a highly significant difference in ACE2 expression across clusters. This nonparametric result establishes that at least one cluster exhibits a distinct expression profile, prompting post hoc pairwise testing.

Pairwise Mann–Whitney U-tests (FDR-corrected) show that:

- Airway goblet cells (Cluster 0) express significantly higher ACE2 levels than most other clusters (smooth muscle, epithelial, and vascular smooth cells; all adjusted p < 1×10⁻⁶).

- Moderate ACE2 expression persists in epithelial cells (Cluster 2 and 6), consistent with their known role as part of the airway barrier.

- Smooth muscle and vascular clusters show minimal ACE2 activity, indicating that these deeper structural cells are less involved in viral entry.

#### The Chi-squared test for ACE2⁺ enrichment (χ² = 255.84, p = 2.31×10⁻⁵²) further supports that ACE2-positive cells are disproportionately enriched in goblet cell clusters, confirming a cell-type–specific susceptibility pattern.

#### Significance
These findings suggest that airway goblet cells—which produce mucus and form part of the mucosal defense—represent the primary site of viral entry and replication at 3 dpi.
This aligns with experimental evidence showing that SARS-CoV-2 preferentially infects ACE2⁺ secretory and ciliated cells, leading to mucus hypersecretion and epithelial dysfunction.

The elevated ACE2 in goblet cells implies:

- Enhanced viral tropism in mucus-producing regions of the airway.

- Disruption of mucociliary clearance, which worsens infection spread.

- Potential feedback upregulation or persistence of ACE2 expression despite immune activation, reflecting continued viral-host interaction.

Meanwhile, lower ACE2 levels in smooth muscle and fibroblast clusters suggest limited direct infection, but these cells may respond indirectly to cytokine-mediated tissue remodelling.

### 5. Pseudotime Interpretation of SARS-CoV-2 Infection Progression

The root cell for pseudotime analysis was selected from the basal epithelial cluster, as these represent progenitor-like cells in the bronchial epithelium, aligning with Ravindra et al., 2021.

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
This indicates localised viral entry and immune activation within the epithelial layer.
The late-pseudotime branch likely contains infected or interferon-stimulated cells beginning to alter transcription.

Interpretation:
1 dpi marks the onset of infection. A subset of ACE2-positive epithelial cells begins to sense and respond to viral RNA, initiating mild stress and antiviral transcription.

#### 2 dpi – Systemic Transcriptional Reprogramming

At 2 dpi, the pseudotime plot (your second image) shows a long, continuous gradient with an expanded late-pseudotime region.
Distinct epithelial subtypes; airway epithelial cells (1 & 4) and ionocytes (5) dominate this progression, while mesothelial and vascular smooth-muscle cells remain peripheral and transcriptionally inert.
A small, high-pseudotime tail (yellow in your figure) indicates a cluster of cells undergoing strong transcriptional divergence, probably representing heavily infected or apoptotic cells.

Interpretation:
2 dpi is the turning point: widespread antiviral signaling and metabolic activation reshape epithelial transcriptional identity while non-target structural cells remain relatively stable.

#### 3 dpi – Inflammation, Remodelling, and Cellular Breakdown

At 3 dpi, the pseudotime manifold becomes fragmented, losing the clear gradient seen earlier.
Distinct clusters appear for goblet cells, macrophages, airway epithelial, and smooth-muscle cells, indicating immune infiltration and epithelial remodelling.
Late-pseudotime regions are dominated by inflammatory epithelial and macrophage cells, while normal ciliated and basal populations shrink.

Interpretation:
By 3 dpi, infection has transitioned from replication to inflammation.
Transcriptomic order collapses, and airway structure gives way to an inflammatory microenvironment with strong immune and secretory remodelling.

