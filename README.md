Spatial Transcriptomics Progression Analysis

Script 1 – Normalization, Visualization, Regression, Differential Expression, and Enrichment of GeoMx Data

Overview

This script performs an integrated analysis of spatial transcriptomics data derived from an Excel workbook (Q3_Normalize_progression.xlsx). The workflow includes:
  Loading normalized and raw count data
  Constructing an AnnData object using Scanpy
  Integrating ROI-level metadata
  Visualizing marker expression across histology and grade
  Dimensionality reduction and clustering
  Correlation and regression analyses between gene expression and Ki67%
  Differential expression analysis using DESeq2 (PyDESeq2)
  Gene Ontology enrichment analysis using GSEApy
  The analysis focuses on tumor progression, proliferation (Ki67%), and candidate genes.

Input Data
Required file:
Q3_Normalize_progression.xlsx

Sheets used:
  SegmentProperties
  Contains ROI-level metadata (e.g., Grade, core_edge, comparison)
  TargetCountMatrix
  Raw and normalized gene expression counts (genes × ROIs)

  Output Files
    deseq2_results.csv
    Differential expression results (Grade 4 vs other)

Figures generated inline:
  Dot plots
  Scatter plots with regression lines
  Error bar plots
  Enrichment barplots

Key Libraries & Dependencies
Python
  Python ≥ 3.9

Core Packages
  pandas
  numpy
  matplotlib
  seaborn
  scanpy
  scikit-learn
  scipy
Differential Expression
  pydeseq2
  sanbomics
Enrichment Analysis
  gseapy

Usage
Ensure all dependencies are installed:
pip install pandas numpy matplotlib seaborn scanpy scikit-learn scipy pydeseq2 sanbomics gseapy
Place Q3_Normalize_progression.xlsx in the working directory.
Run the script:
python script1.py


Notes & Assumptions
Normalization is assumed to be precomputed in the Excel file.
Metadata indices must match ROI/sample names in the count matrix.
The script is designed for ROI-level spatial transcriptomics, not single-cell data.
