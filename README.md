Spatial Transcriptomics Progression Analysis

This repository contains three complementary analysis scripts used to study astrocytoma biology, proliferation, spatial heterogeneity, and survival outcomes using:

- Spatial transcriptomics–derived ROI data (GeoMx DSP)

- Visium 10X Spatial transcriptomics

- Differential expression (DESeq2)

- Gene set enrichment analysis (GSEA)

- Penalized Cox regression survival modeling (TCGA)

Scripts are modular but conceptually linked, progressing from spatial expression analysis → pathway discovery → survival relevance.

The workflow compares Grade 2, Grade 3, and Grade 4 glioma samples from the same patient across disease progression first using GeoMx, then using Visium 10X.

Script 1: Spatial ROI Expression Analysis & Visualization

Performs ROI-level spatial transcriptomic analysis using normalized and raw count data. This script integrates:

- Exploratory visualization

- Gene–proliferation correlations (Ki67%)

- Dimensionality reduction and clustering

- Differential expression (IV vs other)

- Gene set enrichment analysis

Input Data

- Q3_Normalize_progression.xlsx

- SegmentProperties — ROI metadata

- TargetCountMatrix — expression counts

Input Data

Key Steps

1. Load normalized counts and metadata from Excel

2. Convert expression matrix to AnnData

3. Align metadata with observations

4. Attach metadata to observations

Exploratory Analysis

- Dot plots
- PCA, neighbors graph, UMAP
- Leiden clustering

Regression Analysis

Visualization:

- Scatterplots with regression lines

- Error bars for grouped means

Differential Expression (DESeq2)

- Raw counts converted to integer matrix

- Single-factor design

Gene Set Enrichment Analysis

Dependencies

 - Python ≥ 3.9

- scanpy

- pandas

- numpy

- seaborn

- matplotlib

- scikit-learn

- scipy

- pydeseq2

- gseapy

- sanbomics

Script 2: Visium Spatial Transcriptomics Analysis

Analyzes 10x Genomics Visium spatial transcriptomics data to characterize spatial gene expression programs and tissue architecture in glioma samples.

Key Objectives

- Identify spatially variable genes

- Map gene expression to tissue coordinates

- Compare spatial domains with histopathology

Workflow
1. Data Loading

Each Visium dataset is loaded separately using Load10X_Spatial():

Expression counts

Spatial coordinates

Histology images

Each sample is assigned to a grade-specific Seurat object.

2. Quality Control Metrics

3. Spot Filtering

4. Normalization and Scaling

Each dataset undergoes:

- Log-normalization (NormalizeData)

- Z-score scaling (ScaleData)

5. Dataset Integration
   
6.Dimensionality Reduction & Clustering

7. Differential Expression Analysis
   
- Grade-Based DE
- Marker Identification

8. Spatially Variable Gene Analysis

9. Gene-Level Spatial Visualization

10. Functional Enrichment Analysis

Dependencies

- R ≥ 4.2
- Seurat v4 or v5
- ggplot2
- patchwork
- dplyr
- clusterProfiler
- org.Hs.eg.db
- enrichplot
- ReactomePA

Script 3: LASSO & Elastic Net Cox Regression

Builds gene-based survival prediction models for IDH-mutant gliomas using TCGA RNA-seq and clinical data.

Input Data

  TCGA-CDR-SupplementalTableS1.xlsx — clinical outcomes

  EBPlusPlusAdjustPANCAN_IlluminaHiSeq_RNASeqV2.geneExp.tsv — RNA-seq

  ijms-2057006_TableS2.xlsx — IDH mutation status

Key Steps

1. Harmonize TCGA barcodes

2. Filter for IDH-mutant tumors

3. Log₂-transform RNA-seq expression

4. Define biologically informed gene panels

5. Construct survival objects: OS and PFS

5. Train penalized Cox models:

  LASSO

  Elastic Net

6. Extract non-zero coefficients

7. Compute and scale risk scores

8. Evaluate models:

  Concordance index (C-index)

  Kaplan–Meier curves

  Log-rank tests

9. Fit grade-adjusted multivariable Cox models

Outputs

  Selected gene signatures

  Risk scores

  Kaplan–Meier plots

  Hazard ratios with 95% CIs
  
  Dependencies

- R ≥ 4.2

- glmnet

- survival

- survminer

- tidyverse

- readxl

- broom

- gridExtra


Project Structure
project-root/
│── data/
│   ├── Q3_Normalize_progression.xlsx
│   ├── TCGA_clinical.xlsx
│   ├── TCGA_expression.tsv
│── scripts/
│   ├── spatial_analysis.py
│   ├── deseq2_analysis.py
│   ├── survival_modeling.R
│── results/
│   ├── figures/
│   ├── tables/
│── README.md

Notes

GeoMx: Normalization is assumed complete for ROI analyses. DESeq2 requires raw integer counts.

Visium: Spatial normalization uses Seurat’s default log-normalization. Z-normalization occurs during ScaleData(). Scaling is recalculated after merging due to Seurat object structure. Differential expression uses normalized (log) data. Spatial variability uses Moran’s I statistic.

Survival models are restricted to IDH-mutant gliomas.

Risk scores are scaled for visualization only.
