Spatial Transcriptomics Progression Analysis

This repository contains three complementary analysis scripts used to study astrocytoma biology, proliferation, spatial heterogeneity, and survival outcomes using:

Spatial transcriptomics–derived ROI data

Normalized expression matrices

Differential expression (DESeq2)

Gene set enrichment analysis (GSEA)

Penalized Cox regression survival modeling (TCGA)

Each script can be run independently but together form a complete analysis pipeline from exploratory visualization to survival prediction.

Script 1: Spatial ROI Expression Analysis & Visualization
Purpose

Performs exploratory and statistical analysis of ROI-level normalized gene expression with associated histopathological metadata. Focuses on proliferation (Ki67%), spatial context, and gene–phenotype correlations.

Input Data

Q3_Normalize_progression.xlsx

SegmentProperties (metadata)

TargetCountMatrix (normalized expression)

Key Steps

Load required Python packages (scanpy, pandas, seaborn, matplotlib)

Import normalized counts and metadata from Excel

Convert expression matrix to AnnData

Align and attach metadata to observations

Generate dot plots:
Ki67% by histology (core_edge)

genes of intereset by tumor grade

Dimensionality reduction:

PCA

Neighbors graph

UMAP

Leiden clustering

Linear regression analysis:

Gene expression vs Ki67%

ROI-level and grade-averaged regressions

Error bars (SEM) for grouped means

Outputs

Dot plots (Scanpy)

Scatterplots with regression lines

PCA/UMAP embeddings

Leiden cluster assignments

Dependencies

Python ≥ 3.9

scanpy

pandas

numpy

seaborn

matplotlib

scikit-learn

scipy

Script 2: Differential Expression & Gene Set Enrichment Analysis
Purpose

Identifies differentially expressed genes between tumor groups (e.g., Grade IV vs other) and performs functional enrichment analysis.

Input Data

Q3_Normalize_progression.xlsx

Raw count matrix

Segment metadata with comparison variable

Key Steps

Load raw counts and metadata

Prepare DESeq2-compatible count matrix

Define experimental design:

Single-factor comparison (IV vs other)

Run DESeq2 using PyDESeq2

Filter significant genes:

padj < 0.05

|log2FC| > 0.5

baseMean ≥ 10

Normalize and log-transform counts

Perform Gene Set Enrichment Analysis:

GO Biological Process

GO Molecular Function

Visualize enrichment results

Outputs

deseq2_IV_vs_other_results.csv

Bar plots of enriched GO terms

Dot plots of molecular function enrichment

Dependencies

Python ≥ 3.9

pydeseq2

numpy

pandas

gseapy

sanbomics

matplotlib

seaborn

Script 3: TCGA IDH-Mutant Glioma Survival Modeling

LASSO & Elastic Net Cox Regression

Purpose

Builds gene-based survival models to predict overall survival (OS) and progression-free survival (PFS) in IDH-mutant gliomas using TCGA data.

Input Data

TCGA-CDR-SupplementalTableS1.xlsx (clinical outcomes)

EBPlusPlusAdjustPANCAN_IlluminaHiSeq_RNASeqV2.geneExp.tsv (RNA-seq)

ijms-2057006_TableS2.xlsx (IDH mutation status)

Key Steps

Harmonize TCGA barcodes across datasets

Filter for IDH-mutant cases

Log₂-transform RNA-seq expression

Define biologically informed gene panels

Construct survival objects (OS, PFS)

Train penalized Cox models:

LASSO (α = 1)

Elastic Net (α optimized)

Extract non-zero coefficients

Compute and scale risk scores

Evaluate models:

Concordance index (C-index)

Kaplan–Meier curves

Log-rank tests

Perform grade-adjusted multivariable Cox analysis

Outputs

Selected gene signatures

Risk scores

Kaplan–Meier plots

Hazard ratios with 95% CIs

Model performance metrics

Dependencies

R ≥ 4.2

glmnet

survival

survminer

tidyverse

readxl

broom

gridExtra

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

Notes & Caveats

Normalization is assumed complete for Script 1.

DESeq2 analysis uses raw integer counts.

Survival models are restricted to IDH-mutant astrocytomas.

Risk scores are rescaled for visualization, not direct clinical interpretation.

Random train/test splits may introduce minor variability.

