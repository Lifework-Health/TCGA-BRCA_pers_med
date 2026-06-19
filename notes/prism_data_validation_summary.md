# PRISM Data Validation Summary

This interim validation PR adds a lightweight checkpoint for the DepMap/CCLE + PRISM doxorubicin modelling table.

## Current Local Status

- Expected processed table: `data/processed/brca_prism_doxorubicin_modelling_table.csv`
- Current status: **not present in the local workspace at validation time**
- Detailed row-level validation cannot be completed until the processed table exists.

## Validation Added

The new notebook section in `notebooks/00_data_preparation.ipynb` checks:

- whether the processed modelling table exists;
- row and column counts;
- cell-line ID column name;
- first few cell-line names;
- PRISM response columns;
- p53/DNA-damage expression columns present;
- possible mock/example/toy/dummy cell-line names;
- whether the table has fewer than five rows;
- whether all PRISM doxorubicin LFC values are identical;
- response transformation formulas:
  - `relative_abundance_percent = 100 * (2 ** prism_doxorubicin_lfc)`
  - `doxorubicin_sensitivity_score = -1 * prism_doxorubicin_lfc`
- five most and least sensitive cell lines by `doxorubicin_sensitivity_score`.

The notebook writes the validation summary to `tables/prism_data_validation_summary.csv`.

## Current Blocker

The processed table has not yet been generated or committed. To complete validation:

1. Place the raw DepMap/CCLE and PRISM files under `data/raw/depmap_data/`.
2. Rerun `notebooks/00_data_preparation.ipynb` to create `data/processed/brca_prism_doxorubicin_modelling_table.csv`.
3. Rerun the validation checkpoint section.

## Interpretation Reminder

More negative PRISM doxorubicin LFC means lower relative abundance or viability versus control and therefore greater apparent sensitivity.

`doxorubicin_sensitivity_score` is defined as `-1 * prism_doxorubicin_lfc`, so higher values mean greater sensitivity by construction.

No modelling was added in this validation PR.
