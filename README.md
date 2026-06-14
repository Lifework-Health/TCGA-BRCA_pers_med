# TCGA-BRCA Personalised Medicine Assignment

This repository contains a simple Jupyter-based analysis project for a personalised medicine assignment. The overall aim is to study breast cancer response to doxorubicin using a transparent workflow that combines biological modelling, patient data, cell-line drug response data, and perturbation signatures.

The project is intentionally kept small and readable. The analysis will be developed in notebooks, with only a minimal helper file if repeated code becomes useful.

## Assignment Aim

The assignment will ask whether simple p53 and DNA-damage response modelling can help explain or predict doxorubicin response in breast cancer, and how this compares with standard survival and machine-learning approaches.

## Professor Materials Review

Professor-provided course materials are being reviewed in `notes/professor_materials_summary.md` before new analysis is implemented. JNK has been excluded from the assignment plan.

## Planned Questions

1. In TCGA-BRCA, are p53 or DNA-damage response features associated with patient survival?
2. In breast cancer cell lines, are p53 or DNA-damage response features associated with doxorubicin sensitivity?
3. Can models trained in cell-line data transfer useful information to patient data?
4. Can patient-derived molecular patterns be compared with cell-line drug response patterns?
5. How does a simplified p53 ODE model compare with machine-learning models for explaining doxorubicin response?
6. Can LINCS perturbation signatures help connect doxorubicin response with p53 and DNA-damage response biology?

## Why Breast Cancer and Doxorubicin?

Breast cancer is a clinically important cancer type with large public datasets available through TCGA-BRCA and cancer cell-line resources. Doxorubicin is a widely used chemotherapy that causes DNA damage, making it biologically relevant to p53 signalling, DNA-damage response pathways, cell-cycle arrest, and apoptosis.

This makes the topic suitable for a personalised medicine assignment because it connects patient outcomes, drug response, molecular biomarkers, and a mechanistic model that can be explained clearly.

## Planned Datasets

- **TCGA-BRCA**: breast cancer patient molecular and clinical outcome data.
- **DepMap/CCLE**: cancer cell-line molecular data, including breast cancer cell lines.
- **GDSC or PRISM**: drug response data for doxorubicin or related compounds.
- **LINCS**: perturbation gene-expression signatures for doxorubicin and related DNA-damaging treatments.

No data is downloaded into this repository yet. Raw data files should be kept outside git or placed in ignored data folders.

## Planned Modelling Approaches

- A simplified p53/DNA-damage response ODE model.
- Cox regression for survival analysis in TCGA-BRCA.
- Elastic net models for interpretable prediction.
- Random forest or gradient boosting models for non-linear prediction.
- Simple transfer analyses comparing patient and cell-line settings.

The modelling will be implemented later in notebooks with clear explanations and simple code.

## Repository Structure

```text
TCGA-BRCA_pers_med/
├── README.md
├── environment.yml
├── .gitignore
├── data/
│   ├── raw/
│   ├── interim/
│   └── processed/
├── notebooks/
├── src/
│   └── simple_helpers.py
├── figures/
├── tables/
└── submission/
```

## Create the Conda Environment

Create the environment from `environment.yml`:

```bash
conda env create -f environment.yml
```

Activate it:

```bash
conda activate tcga-brca-pers-med
```

Register the environment as a Jupyter kernel:

```bash
python -m ipykernel install --user --name tcga-brca-pers-med --display-name "Python (TCGA-BRCA pers med)"
```

## Run the Notebooks

Start Jupyter from the repository root:

```bash
jupyter notebook
```

Run the notebooks in order:

1. `notebooks/00_data_preparation.ipynb`
2. `notebooks/01_p53_ode_model.ipynb`
3. `notebooks/02_q1_tcga_survival.ipynb`
4. `notebooks/03_q2_cellline_drug_response.ipynb`
5. `notebooks/04_q3_q4_transfer_models.ipynb`
6. `notebooks/05_q5_ode_vs_ml.ipynb`
7. `notebooks/06_q6_lincs_signature.ipynb`
8. `notebooks/final_report.ipynb`

At this stage, the notebooks only contain placeholder sections. Data downloading, modelling, and final analysis will be added later.

## Export the Final Report to PDF

After completing `notebooks/final_report.ipynb`, export it to PDF with:

```bash
jupyter nbconvert --to pdf notebooks/final_report.ipynb --output-dir submission
```

Generated PDFs are ignored by default and can be intentionally added later if needed for submission.
