# CAFA 6 — Protein Function Prediction (Kaggle)

> **Status:** Active competition work-in-progress.  
> **Model/code release:** This repo only contains the basic setup. The full pipeline and trained model will be made **public after the competition ends**. 
## Overview
Predict **Gene Ontology (GO)** terms (BP/CC/MF) for protein sequences. 
This repo outlines my starting strategy for the [**CAFA 6 – Protein Function Prediction** challenge](https://www.kaggle.com/competitions/cafa-6-protein-function-prediction/overview.). 
Follow my progress up the leaderboard (📈220/597📈)


**Starting setup**
- **Similarity transfer**: DIAMOND hits from test → training; transfer labels with identity×coverage weights.  
- **GOA (NOT-free) overlap**: Direct mappings from GOA UniProt (negations removed).  
- **Taxon-aware priors**: Smoothed counts (global + taxon) for recall on hard/novel cases.  
- **Blending & caps**: `SIM ∩ GOA → SIM → GOA → prior`


## Data
- `train_sequences.fasta`, `train_terms.tsv`, `train_taxonomy.tsv`
- `go-basic.obo` (GO DAG)
- `IA.tsv` (information accretion weights)
- `testsuperset.fasta`
- `goa_uniprot_all.gaf.gz` → filtered into `goa_test_pairs.tsv.gz` (NOT-free)

> Files are from the Kaggle competition and EBI
## Quickstart 
1. **Mount Drive, set paths & check data structure**  

2. **Build inputs**
   - **SIM**: Create `hits_full.m8` with DIAMOND (12-column outfmt).
   - **GOA pairs**: Stream-filter `goa_uniprot_all.gaf.gz` to `goa_test_pairs.tsv.gz` (NOT-free + valid GO).

3. **Verify**  
   Count eligible SIM queries and loads caches.

4. **Local validation**  
   Create a 2.5k stratified **val split**, run SIM on the split, then evaluate IA-P/R/F1 to tune:
   - similarity thresholds (PID/QCOV/TCOV),
   - caps per ontology,
   - SIM∩GOA boost,
   - prior blending strength.

5. **Write submission**  




