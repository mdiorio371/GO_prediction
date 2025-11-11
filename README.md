# CAFA 6 — Protein Function Prediction (Kaggle)

> **Status:** Active competition work-in-progress.  
> **Model/code release:** This repo only contains the basic setup. The full pipeline and trained model will be made **public after the competition ends**. 
## Overview
My starting strategy for the [**CAFA 6 – Protein Function Prediction** challenge](https://www.kaggle.com/competitions/cafa-6-protein-function-prediction/overview.)
Predict **Gene Ontology (GO)** terms (BP/CC/MF) for protein sequences in the test superset. 


**Starting setup**
- **Similarity transfer**: DIAMOND/MMseqs2 hits from test → training; transfer labels with identity×coverage weights.  
- **GOA (NOT-free) overlap**: Direct mappings from GOA UniProt (negations removed).  
- **Taxon-aware priors**: Smoothed counts (global + taxon) for recall on hard/novel cases.  
- **Blending & caps**: `SIM ∩ GOA → SIM → GOA → prior`


## Data
- `train_sequences.fasta`, `train_terms.tsv`, `train_taxonomy.tsv`
- `go-basic.obo` (GO DAG)
- `IA.tsv` (information accretion weights)
- `testsuperset.fasta`
- (Optional) `goa_uniprot_all.gaf.gz` → filtered into `goa_test_pairs.tsv.gz` (NOT-free)

> Files are from the Kaggle competition and EBI
## Quickstart 
1. **Mount Drive & set paths**  
   Save all competition files under `MyDrive/kaggle/CAFA6/`.

2. **Build inputs**
   - **SIM**: Create `hits_full.m8` with DIAMOND (12-column outfmt).
   - **GOA pairs**: Stream-filter `goa_uniprot_all.gaf.gz` to `goa_test_pairs.tsv.gz` (NOT-free + valid GO).

3. **Reload & verify**  
   A single cell checks file health, counts eligible SIM queries, and loads caches.

4. **Local validation**  
   Create a 2.5k stratified **val split**, run SIM on the split, then evaluate IA-P/R/F1 to tune:
   - similarity thresholds (PID/QCOV/TCOV),
   - caps per ontology,
   - SIM∩GOA boost,
   - prior blending strength.

5. **Write submission**  




