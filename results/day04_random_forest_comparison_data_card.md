# mediAI Day 04 — Fixed Random Forest Comparison Data Card

## Project question

On the frozen Day 03 ChEMBL EGFR dataset and unchanged evaluation splits,
does one predeclared Random Forest baseline improve on the transparent
Logistic Regression reference model?

## Frozen Day 03 inputs

- Source target: human EGFR (`CHEMBL203`) from ChEMBL public data
- Final standardized structures: 7,757
- Active labels: 6,242
- Inactive labels: 1,515
- Molecular representation: radius-2 Morgan fingerprints, 2,048 bits
- Random split: 6,205 train / 1,552 test
- Scaffold split: 6,339 train / 1,418 test
- Random seed: 42
- Classification threshold: 0.50

## Models compared

### Transparent reference

- Logistic Regression
- Class weighting: balanced
- Solver: liblinear
- Maximum iterations: 2,000

### Fixed Random Forest

- Estimators: 500
- Maximum features per split: sqrt
- Minimum samples per leaf: 2
- Class weighting: balanced_subsample
- Random seed: 42
- Parallel workers: all available CPU cores

No hyperparameter selection was performed from held-out test results.

## Test-set performance

| Split | Model | ROC-AUC | PR-AUC | MCC | Balanced accuracy | Precision | Recall | F1 | Brier score |
|---|---|---:|---:|---:|---:|---:|---:|---:|---:|
| Random | Logistic Regression | 0.9097 | 0.9715 | 0.6314 | 0.8378 | 0.9436 | 0.8967 | 0.9195 | 0.0976 |
| Random | Random Forest | 0.9398 | 0.9830 | 0.6731 | 0.8435 | 0.9407 | 0.9279 | 0.9343 | 0.0787 |
| Scaffold | Logistic Regression | 0.8906 | 0.9643 | 0.5949 | 0.8214 | 0.9373 | 0.8813 | 0.9084 | 0.1096 |
| Scaffold | Random Forest | 0.9410 | 0.9837 | 0.6618 | 0.8399 | 0.9391 | 0.9217 | 0.9303 | 0.0816 |

## Fixed-threshold error audit

| Split | Model | False positives | False negatives | Total errors |
|---|---|---:|---:|---:|
| Random | Logistic Regression | 67 | 129 | 196 |
| Random | Random Forest | 73 | 90 | 163 |
| Scaffold | Logistic Regression | 67 | 135 | 202 |
| Scaffold | Random Forest | 68 | 89 | 157 |

## Result summary

- Random Forest improved ROC-AUC, PR-AUC, MCC, balanced accuracy, recall,
  F1, and Brier score on both frozen evaluation schemes.
- At the fixed 0.50 threshold, Random Forest made
  33 fewer errors on the random test split and
  45 fewer errors on the scaffold test split.
- The largest change was fewer false negatives:
  39
  fewer on the random split and
  46
  fewer on the scaffold split.
- Random Forest is the leading internal predictive baseline in this
  fixed comparison. Logistic Regression remains the simpler,
  more interpretable reference model.

## Saved artifacts

- `day04_model_comparison_metrics.csv`
- `day04_rf_random_split_test_predictions.csv`
- `day04_rf_scaffold_split_test_predictions.csv`
- `day04_model_comparison_error_summary.csv`
- `day04_model_comparison_confusion_matrices.png`
- `day04_model_comparison_evaluation_curves.png`

## Limitations

1. This is one internal comparison on one curated public ChEMBL dataset.
2. The test splits were used only for final evaluation, not parameter tuning,
   but model selection across baselines is still an internal research step.
3. A scaffold split is stronger than a random split but is not external,
   temporal, prospective, or experimental validation.
4. Lower Brier scores are favourable probability-score results; they do not
   prove perfect calibration.
5. The activity label depends on the predeclared median pChEMBL 6.0 threshold.
6. This workflow does not predict clinical efficacy, safety, dosing,
   pharmacokinetics, toxicity, patient benefit, or regulatory approval.

## Next steps

1. Archive the verified Day 04 artifacts.
2. Publish the Day 04 notebook and selected results to GitHub.
3. Use the documented results—not hype—to create the EGFR flagship video.
