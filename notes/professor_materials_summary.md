# Professor-Provided Materials Summary

Source reviewed: `/Users/garethmorgan/dev/personalised med`

These materials were inspected locally only. The professor-provided files themselves were not copied into this repository.

## High-Level Takeaways

- The materials are mostly example workflows for neuroblastoma, not breast cancer.
- Several scripts use TARGET neuroblastoma files under `nbl_target_2018_pub/`, so they need adaptation before use with TCGA-BRCA.
- Doxorubicin appears in the PRISM/DepMap signature script. The GDSC version currently uses `topotecan`.
- The p53 model uses a DNA-damage response input called `DDR` and outputs simulated p53 S15 and p53 S46 response scores across DDR doses.
- No LINCS-specific script or input file was found in the reviewed materials.
- JNK does not appear to be required for the current assignment plan and is excluded.

## File-by-File Summary

| File | What it appears to do | Dataset used | Assignment mapping |
| --- | --- | --- | --- |
| `nb_params_full.csv` | Prepared patient-level neuroblastoma table with clinical survival fields and selected p53 model input genes. | p53 model inputs, TARGET-style neuroblastoma patient data | Q1, Q5 as an example input table, but not directly TCGA-BRCA |
| `p53_parameters_summaryTable.txt` | Parameter table for the p53 ODE model, including fitted rates for doxorubicin and radiation-related ATM/p53 signalling terms. | p53 model inputs | Q1, Q2, Q5 |
| `p53s15DR_tp53.csv` | ODE-derived p53 S15 response scores for each patient across multiple `DDR_*` input levels. | p53 model outputs, neuroblastoma patient IDs | Q1, Q5 |
| `p53s46DR_tp53.csv` | ODE-derived p53 S46 response scores for each patient across multiple `DDR_*` input levels. | p53 model outputs, neuroblastoma patient IDs | Q1, Q5 |
| `ODE_example.ipynb` | Teaching example showing how to define and solve a small ODE system in Python, plot time courses, and make a simple dose-response curve. | Generic ODE teaching example | Q5 background only |
| `p53_model_updated.ipynb` | Implements the larger p53/DNA-damage response ODE model, personalises selected initial conditions/rates using patient gene expression, simulates across DDR doses, and writes p53 S15/S46 response tables. | p53 model inputs, `nb_params_full.csv`, fitted parameter table | Q1, Q2, Q5 |
| `preproc and optimal input.R` | Prepares neuroblastoma patient expression and survival data, writes `nb_params_full.csv`, merges ODE outputs, runs Cox models for p53 S15/S46 DDR scores, and plots Kaplan-Meier curves using selected thresholds. | TARGET-style neuroblastoma survival/expression plus p53 model outputs | Q1, Q5 |
| `ODE lecture/DepMap_signature.R` | Builds a lasso expression signature for doxorubicin response in neuroblastoma cell lines using DepMap/CCLE expression and PRISM primary screen log-fold-change data. | DepMap/CCLE, PRISM | Q2, Q3, Q5 |
| `ODE lecture/DepMap_signature_GDSC.R` | Builds a lasso expression signature for drug response using DepMap/CCLE expression and GDSC fitted dose-response data. It currently targets topotecan and neuroblastoma, not doxorubicin and breast cancer. | DepMap/CCLE, GDSC, plus TARGET-style gene intersection | Q2, Q3, Q5 |
| `ODE lecture/DepMap_survival.R` | Applies the PRISM-derived cell-line signature coefficients to patient expression data, then tests association with overall survival using Cox regression and Kaplan-Meier plots. | PRISM-derived signature, TARGET-style neuroblastoma survival/expression | Q3 |
| `ODE lecture/DepMap_survival_GDSC.R` | Applies the GDSC-derived cell-line signature coefficients to patient expression data, then tests association with overall survival using Cox regression and Kaplan-Meier plots. | GDSC-derived signature, TARGET-style neuroblastoma survival/expression | Q3 |

## Dataset Mapping

| Dataset category | Files using it |
| --- | --- |
| PRISM | `ODE lecture/DepMap_signature.R`, through `primary-screen-replicate-collapsed-treatment-info.csv` and `primary-screen-replicate-collapsed-logfold-change.csv` |
| GDSC | `ODE lecture/DepMap_signature_GDSC.R`, through `GDSC2_fitted_dose_response.csv` |
| DepMap/CCLE | `ODE lecture/DepMap_signature.R`, `ODE lecture/DepMap_signature_GDSC.R`, through `Model.csv` and `OmicsExpressionTPMLogp1HumanProteinCodingGenes.csv` |
| TCGA | No file directly uses TCGA-BRCA yet. Several comments say TCGA, but the actual paths point to `nbl_target_2018_pub/` neuroblastoma files. |
| p53 model inputs | `p53_parameters_summaryTable.txt`, `nb_params_full.csv`, `p53_model_updated.ipynb`, `p53s15DR_tp53.csv`, `p53s46DR_tp53.csv`, `preproc and optimal input.R` |
| LINCS | No LINCS file or workflow was found. |

## Assignment Question Mapping

| Question | Supported by reviewed materials | Notes |
| --- | --- | --- |
| Q1: p53/ODE model and patient survival | Partly supported | `preproc and optimal input.R` demonstrates Cox/Kaplan-Meier testing of ODE-derived p53 S15/S46 scores against survival, but with TARGET neuroblastoma data. Needs TCGA-BRCA adaptation. |
| Q2: p53/ODE model and DNA-damaging drug response in cell lines | Partly supported | DepMap signature scripts model drug response from cell-line expression. PRISM version uses doxorubicin; GDSC version uses topotecan unless changed. Direct ODE-to-cell-line drug response is not implemented. |
| Q3: cell-line regression/signature model applied to patient survival | Supported as an example | `DepMap_survival.R` and `DepMap_survival_GDSC.R` apply cell-line-derived coefficients to patient expression and survival. Needs breast cancer/TCGA adaptation. |
| Q4: patient Cox model/signature applied to cell-line drug sensitivity | Not directly implemented | The reverse transfer direction is not present. We would need to build it later if required. |
| Q5: mechanistic p53 model versus ML/regression approaches | Partly supported | The ODE model and lasso signature workflows exist separately. A direct comparison framework is not implemented. |
| Q6: p53 model and doxorubicin-induced expression signatures | Weakly supported | Doxorubicin appears in PRISM/DepMap drug response. No LINCS doxorubicin expression-signature analysis was found. |

## Packages and Libraries

### Python notebooks

Observed imports:

- `numpy`
- `pandas`
- `scipy.integrate`
- `scipy.io`
- `pylab` / `matplotlib`
- `os`

Main solver used:

- `scipy.integrate.odeint`

### R scripts

Observed packages:

- `tidyverse`
- `data.table`
- `glmnet`
- `stringr`
- `rstudioapi`
- `dplyr`
- `survival`
- `survminer`

`progeny` is mentioned but commented out.

## Expected Inputs

### p53 model and survival inputs

- `p53_parameters_summaryTable.txt`
- `nb_params_full.csv`
- `p53s15DR_tp53.csv`
- `p53s46DR_tp53.csv`
- `nbl_target_2018_pub/data_mrna_seq_rpkm.txt`
- `nbl_target_2018_pub/data_clinical_patient.txt`
- `nbl_target_2018_pub/data_clinical_sample.txt`

Key p53 model input genes used in `preproc and optimal input.R`:

- `ATM`
- `CHEK2`
- `HIPK2`
- `MDM2`
- `PPM1D`
- `SIAH1`
- `TP53`
- `WSB1`

### DepMap/CCLE and PRISM inputs

- `depmap_data/Model.csv`
- `depmap_data/OmicsExpressionTPMLogp1HumanProteinCodingGenes.csv`
- `depmap_data/primary-screen-replicate-collapsed-treatment-info.csv`
- `depmap_data/primary-screen-replicate-collapsed-logfold-change.csv`

### GDSC inputs

- `depmap_data/GDSC2_fitted_dose_response.csv`
- `depmap_data/Model.csv`
- `depmap_data/OmicsExpressionTPMLogp1HumanProteinCodingGenes.csv`
- `nbl_target_2018_pub/data_mrna_seq_rpkm.txt` for intersecting genes with patient data

## Generated Outputs

| File/script | Outputs |
| --- | --- |
| `p53_model_updated.ipynb` | `p53s15DR_tp53.csv`, `p53s46DR_tp53.csv`; dose-response plots of p53 S46 curves |
| `preproc and optimal input.R` | `nb_params_full.csv`; Cox model summaries; forest plots; p-value threshold plots; Kaplan-Meier plots |
| `ODE lecture/DepMap_signature.R` | `NB_cell_lines_coefs.csv`; glmnet path plot; actual-vs-predicted plot |
| `ODE lecture/DepMap_signature_GDSC.R` | `NB_cell_lines_coefs_GDSC.csv`; glmnet path plot; actual-vs-predicted plot |
| `ODE lecture/DepMap_survival.R` | Cox model summary; forest plot; p-value threshold plot; Kaplan-Meier plot |
| `ODE lecture/DepMap_survival_GDSC.R` | Cox model summary; forest plot; p-value threshold plot; Kaplan-Meier plot |
| `ODE_example.ipynb` | ODE time-course plots and simple dose-response plots |

## Immediate Blockers and Confusing Assumptions

1. The course scripts are neuroblastoma examples, while this assignment is TCGA-BRCA breast cancer.
2. The patient files expected under `nbl_target_2018_pub/` are not part of this repository and should not be committed unless explicitly allowed.
3. The DepMap and GDSC/PRISM files expected under `depmap_data/` are also not in this repository.
4. No TCGA-BRCA data-loading workflow is present yet.
5. No LINCS workflow is present yet.
6. The GDSC signature script currently uses `target_drug <- "topotecan"`, not doxorubicin.
7. The PRISM signature script uses doxorubicin but filters for neuroblastoma cell lines.
8. Several file paths are hardcoded relative paths and will need careful replacement with simple notebook paths later.
9. The ODE notebook writes output CSVs directly into the working directory.
10. Some modelling choices are manual or exploratory, including hardcoded lambda values and threshold searches for Kaplan-Meier groups.
11. The p53 model uses selected genes and fitted parameters from prior material; the biological assumptions need to be explained clearly before reuse.
12. Q4 is not covered directly by the provided scripts.
13. Q6 is not covered directly because LINCS signatures are absent.

## Suggested Use for This Assignment

- Treat the professor materials as templates, not files to run unchanged.
- Adapt the p53 model inputs to TCGA-BRCA using the same small gene set where possible.
- Use the PRISM doxorubicin script as the closest template for cell-line drug response, but switch from neuroblastoma to breast cancer cell lines.
- Use the GDSC script only after confirming doxorubicin is available in the selected GDSC release.
- Keep Q4 and Q6 as new notebook analyses because the reviewed materials do not provide direct implementations.
