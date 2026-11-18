# SHP2 Inhibitor SAR Analysis and Machine Learning Validation

**A cheminformatics and machine-learning study of 35 published SHP2 allosteric inhibitors**

**Author:** Yijie Zhu  
**Research area:** Medicinal Chemistry | Computational Drug Discovery | Structure–Activity Relationships

## 1. Project Overview

This project investigates how structural modifications influence the inhibitory activity of SHP2 allosteric inhibitors and evaluates whether machine-learning models can predict their experimentally measured activity.

Using 35 published compounds from a medicinal chemistry study, I developed a Python-based cheminformatics workflow integrating molecular structure curation, physicochemical descriptors, molecular fingerprints, similarity analysis, and machine-learning validation.

The primary prediction endpoint is **percentage inhibition of SHP2 at 10 μM**, rather than IC50.

The project emphasizes not only predictive performance but also the limitations of applying machine-learning models to small medicinal chemistry datasets.

## 2. Research Objectives

1. Curate molecular structures and experimental activity measurements for 35 SHP2 inhibitors.
2. Investigate structure–activity relationships, including the effects of stereochemistry and structural modifications.
3. Represent molecular structures using RDKit descriptors and chirality-aware Morgan fingerprints.
4. Compare random forest regression with simple baseline methods.
5. Evaluate model generalization using structural-family holdout and connectivity-grouped cross-validation.
6. Investigate whether nested hyperparameter selection improves prediction performance without directly using held-out test data.

## 3. Computational Methods

**Data preparation:** Published structures and activity measurements were curated and checked for consistency. Stereochemical information was preserved, and a discrepancy affecting compounds 20–24 was documented.

**Molecular representation:** RDKit was used to calculate physicochemical descriptors and 2048-bit Morgan fingerprints (radius 2, with chirality enabled). Tanimoto similarity was used to compare molecular fingerprints.

**Prediction models:**
- Training-mean baseline
- Tanimoto nearest-neighbor regression
- Random forest regression
- Random forest regression with nested hyperparameter selection

**Validation strategies:**

- **Leave-one-structural-family-out:** Evaluates performance when an entire predefined structural-modification family is excluded from training.
- **Repeated connectivity-grouped five-fold cross-validation:** Evaluates predictions across related chemical series while keeping compounds with identical molecular connectivity in the same fold.
- **Nested cross-validation:** Selects random forest hyperparameters using only the outer training data.

Model performance was assessed primarily using mean absolute error (MAE), expressed in percentage points.

## 4. Key Results

| Model | Family-held-out MAE (pp) | Repeated grouped CV MAE (pp) |
|---|---:|---:|
| Training-mean baseline | 27.09 | 24.54 ± 0.62 |
| Tanimoto nearest neighbor | 30.80 | 16.91 ± 1.50 |
| Original random forest | **25.59** | 18.89 ± 0.69 |
| Nested random forest | 29.00 | **13.02 ± 0.94** |

In repeated connectivity-grouped cross-validation, nested hyperparameter selection reduced random forest MAE from 18.89 to 13.02 percentage points, an improvement of approximately 31%.

However, the same procedure increased family-held-out MAE from 25.59 to 29.00 percentage points.

This contrast suggests that the model is more effective at predicting activity among related compounds than at generalizing across held-out structural-modification families.

## 5. Medicinal Chemistry Insights

The analysis also examined experimentally measured structure–activity relationships.

For example, compounds 29 (SDUY104) and 34 are enantiomers with the same reported mean inhibition of 99% at 10 μM, but their enzymatic IC50 values differ by approximately 14.29-fold.

This example illustrates both the importance of stereochemistry and the limitations of using single-concentration inhibition as a prediction endpoint.

## 6. Project Structure

- `notebooks/SHP2_SAR_Workbook.ipynb` — Molecular data analysis, SAR visualization, fingerprint generation, baseline modeling, and error analysis.
- `notebooks/SHP2_Step_8_Nested_Validation.ipynb` — Nested hyperparameter selection and additional validation.
- `data/` — Curated experimental measurements and verified molecular structures.
- `figures/` — Selected visualizations.
- `results/` — Prediction tables, evaluation metrics, and analysis outputs.
- `environment.yml` — Python environment specification.

## 7. Tools and Libraries

Python, RDKit, NumPy, pandas, scikit-learn, Matplotlib, and JupyterLab.

## 8. Limitations and Future Directions

This is a retrospective analysis of 35 related compounds from one published study.

The validation results do not establish reliable predictive performance on entirely new chemical series. The repeated cross-validation estimates also reuse the same compounds and should not be interpreted as independent external validation.

Furthermore, the prediction target is single-concentration inhibition, which does not capture all aspects of compound potency.

Future work could incorporate larger and more chemically diverse datasets, additional experimental measurements, and prospective evaluation of newly synthesized analogues.

## 9. Data Source and Reference

Zhu et al. *Journal of Medicinal Chemistry* **2026**, *69*, 14774–14803.

DOI: https://doi.org/10.1021/acs.jmedchem.6c00836

All experimental activity measurements originate from the published study. The computational curation, SAR analysis, molecular representation, modeling, and validation presented here constitute the subsequent computational project.