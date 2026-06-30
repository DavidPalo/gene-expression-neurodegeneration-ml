# Gene expression classification of Alzheimer's disease and vascular dementia (GSE122063)

Machine-learning practical — MSc Bioinformatics, University of Murcia.
Author: **David Palomino Plantón**

Exploratory analysis and supervised classification of brain gene-expression data to distinguish between **Alzheimer's disease**, **vascular dementia** and **healthy controls**, using the public **GSE122063** dataset. The project combines dimensionality reduction (PCA), two classification algorithms (Random Forest and SVM) and a biological interpretation of the most informative genes.

---

## Overview

Using a publicly available microarray expression dataset, this work:

1. Downloads and preprocesses the data directly from GEO, keeping the most variable genes.
2. Runs an exploratory **PCA** to assess how separable the diagnostic groups are, and interprets the leading genes biologically.
3. Trains and compares two classifiers — **Random Forest** and **SVM** — under cross-validation, selecting the best model by balanced accuracy.
4. Identifies the genes most relevant to the chosen model and discusses their link to neurodegeneration.

---

## Data availability

This project is **fully reproducible**: the dataset is **public** and is downloaded automatically by the code — no manual download or access agreement required.

- **Dataset:** GEO accession **GSE122063** (brain tissue gene expression; Alzheimer's disease, vascular dementia and controls).
- **Link:** https://www.ncbi.nlm.nih.gov/geo/query/acc.cgi?acc=GSE122063
- The notebook retrieves it via `GEOquery::getGEO("GSE122063")` at runtime.

The intermediate `.rds` files (cached expression matrix and trained models) are **not committed** — they are regenerated when the notebook runs. This keeps the repository lightweight while remaining fully reproducible.

---

## Repository structure

```
gene-expression-neurodegeneration-ml/
├── README.md
├── .gitignore
└── gene_expression_classification.Rmd   # full analysis (R Markdown)
```

> Optional: if you knit the document, you can also add the rendered `gene_expression_classification.html` so reviewers can see the figures without running the code.

---

## Methods

**Preprocessing**
- Download via `GEOquery`; extraction of the expression matrix and sample metadata.
- Definition of the response variable from the clinical diagnosis (Alzheimer's / vascular dementia / control).
- Selection of the top 5,000 most variable genes and transposition to a samples × genes matrix.
- A fixed random seed (`set.seed(123)`) for reproducibility.

**Exploratory analysis**
- **Principal Component Analysis (PCA)** with visualisation of the first components, explained/cumulative variance (scree plots) and inspection of the top-loading genes per component, interpreted in the context of synaptic function and neuronal integrity.

**Classification (caret)**
- **Random Forest** and **SVM**, with hyperparameter tuning via cross-validation and **balanced accuracy** as the selection metric (appropriate for the class imbalance present in the data).
- Parallel processing with `doParallel` to speed up training.
- A `saveRDS`/`readRDS` caching pattern (training chunks set to `eval=FALSE`) so the report can be re-knit without retraining.
- Model comparison on the validation folds and on a held-out test set, plus a paired t-test between models.
- Feature-importance analysis of the best model, mapping probes to gene symbols for biological interpretation.

---

## Key results

<!-- Cifras tomadas de tu propio informe; revísalas y ajústalas si lo ves necesario. -->
- In cross-validation, **Random Forest** reached a mean balanced accuracy of **~93%**, slightly above **SVM** (**~90%**).
- The paired t-test showed the difference was **not statistically significant** (p ≈ 0.23), though the trend consistently favoured Random Forest across metrics.
- PCA's first component was dominated by genes linked to synaptic function and neuronal integrity (e.g. *SYP*, *UNC13A*, *RTN1*), processes strongly implicated in neurodegenerative disease.

---

## Tech stack

**R:** GEOquery, caret, ggplot2, patchwork, doParallel, DT

---

## Author

**David Palomino Plantón** — MSc in Bioinformatics, University of Murcia.
<!-- TODO: añade tu LinkedIn / email si quieres que te contacten desde el repo. -->
