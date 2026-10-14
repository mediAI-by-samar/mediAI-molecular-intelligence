# mediAI Day 03 — EGFR Bioactivity Classification Data Card

## Project question

Can a transparent Morgan-fingerprint Logistic Regression baseline classify
curated ChEMBL human EGFR activity labels, and how does performance change
between random and scaffold-based evaluation?

## Data source

- Source: ChEMBL public REST API
- Target: Human Epidermal Growth Factor Receptor (EGFR)
- Target ChEMBL ID: CHEMBL203
- Dataset audit date: 2026-10-14
- Measurement: standardized IC50
- Units: nM
- Relation: exact equals sign only
- Assay type: Binding assay
- Primary confidence rule: confidence score 9
- Primary relationship rule: direct target relationship

## Curation decisions

- Raw EGFR IC50 records: 19,646
- Conservative candidate records: 16,084
- High-confidence direct records: 12,070
- High-confidence unique molecules before label-stability filtering:
  8,106
- Molecules removed because repeat measurements crossed the activity threshold:
  320
- Final unique standardized structures: 7,757

## Label definition

- Active: median pChEMBL >= 6.0
- Inactive: median pChEMBL < 6.0
- Equivalent potency threshold: IC50 <= 1 micromolar
- Duplicate measurement aggregation: median pChEMBL per molecule

## Final class balance

- Active: 6,242
- Inactive: 1,515

## Molecular representation

- Fingerprint: Morgan fingerprint
- Radius: 2
- Fingerprint size: 2048 bits
- Unique Murcko scaffolds: 2,924

## Baseline model

- Model: Logistic Regression
- Class weighting: balanced
- Random seed: 42
- Classification threshold: 0.50

## Random split results

- ROC-AUC: 0.9097
- PR-AUC: 0.9715
- MCC: 0.6314
- Balanced accuracy: 0.8378
- Brier score: 0.0976
- False positives: 67
- False negatives: 129

## Scaffold split results

- ROC-AUC: 0.8906
- PR-AUC: 0.9643
- MCC: 0.5949
- Balanced accuracy: 0.8214
- Brier score: 0.1096
- False positives: 67
- False negatives: 135

## Limitations

1. This is internal validation on one public ChEMBL-derived dataset.
2. A scaffold split is stronger than a random split, but it is not external validation.
3. The active/inactive label depends on the predeclared pChEMBL 6.0 threshold.
4. ChEMBL measurements can vary across experiments and laboratories.
5. This model does not predict clinical safety, patient benefit, efficacy, dosage,
   pharmacokinetics, toxicity, or regulatory approval.
6. The results are for computational research and hypothesis generation only.

## Next steps

1. Compare a second classical baseline.
2. Perform threshold and calibration sensitivity analysis.
3. Inspect scaffold-split false positives and false negatives.
4. Prepare a reproducible GitHub report and video evidence.
