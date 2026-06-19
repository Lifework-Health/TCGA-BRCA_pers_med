# PRISM Data Validation Summary

This interim validation PR adds a lightweight checkpoint for the DepMap/CCLE + PRISM doxorubicin modelling table.

## Current Local Status

- Expected processed table: `data/processed/brca_prism_doxorubicin_modelling_table.csv`
- Current status: **present and validated**
- Final joined modelling table rows: **26**
- Final joined modelling table columns: **24**

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

## Validation Result

- `data/processed/brca_prism_doxorubicin_modelling_table.csv` exists.
- The table has 26 rows, so it passes the minimum size check.
- No cell-line names matched `example`, `mock`, `toy`, or `dummy`.
- PRISM doxorubicin LFC values are not all identical.
- Required response columns are present:
  - `prism_doxorubicin_lfc`
  - `relative_abundance_percent`
  - `doxorubicin_sensitivity_score`
- Formula checks passed:
  - `relative_abundance_percent = 100 * (2 ** prism_doxorubicin_lfc)`
  - `doxorubicin_sensitivity_score = -1 * prism_doxorubicin_lfc`
- All requested p53/DNA-damage expression genes are present:
  - `ATM`, `CHEK2`, `HIPK2`, `MDM2`, `PPM1D`, `SIAH1`, `TP53`, `WSB1`
  - `CDKN1A`, `BAX`, `BBC3`, `GADD45A`, `MDM4`, `ATR`, `CHEK1`, `CASP3`

## Remaining Assumption

Raw DepMap/PRISM files are not committed to the repository. They were downloaded locally into `data/raw/depmap_data/`, which is intentionally ignored by git.

The expression file used for this validation was a DepMap custom-download subset containing the requested p53/DNA-damage genes plus cell-line metadata, rather than the full expression matrix. This keeps the local workflow lightweight while preserving the needed columns for Q2/Q3/Q5.

## Interpretation Reminder

More negative PRISM doxorubicin LFC means lower relative abundance or viability versus control and therefore greater apparent sensitivity.

`doxorubicin_sensitivity_score` is defined as `-1 * prism_doxorubicin_lfc`, so higher values mean greater sensitivity by construction.

No modelling was added in this validation PR. The table is ready for the next Q2/Q3/Q5 modelling notebooks, subject to carrying forward the PRISM LFC interpretation above.
