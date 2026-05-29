# Epilepsy or Variant?

## Visual EEG Encoding for Interpretable Sharp Transient Classification

[![Python](https://img.shields.io/badge/Python-3.10%2B-234461?style=flat-square)](https://www.python.org/)
[![Notebook](https://img.shields.io/badge/Jupyter-notebook-F37626?style=flat-square)](spikes_validation_framework.ipynb)
[![Data](https://img.shields.io/badge/Data-Dryad-4C8C4A?style=flat-square)](https://doi.org/10.5061/dryad.xsj3tx99w)
[![License](https://img.shields.io/badge/Code-Apache%202.0-6E7781?style=flat-square)](LICENSE)

This repository contains the companion analysis notebook for:

**Epilepsy or Variant? Development and Internal Validation of Visual EEG Encoding for Interpretable Sharp Transient Classification in Routine EEG**

The notebook implements a two-branch visual EEG encoding workflow for clip-level classification of epileptiform versus non-epileptiform sharp transients, with internal validation, IFCN criterion alignment, timing checks, calibration plots, and interpretability figures.

---

## Data Source

The analysis uses the public Dryad dataset associated with the clinical validation study of IFCN criteria for interictal epileptiform discharges:

Kural, M. A., Duez, L., Sejer Hansen, V., Larsson, P. G., Rampp, S., Schulz, R., Tankisi, H., Wennberg, R., Bibby, B., Scherg, M., & Beniczky, S. (2020). *Criteria for defining interictal epileptiform discharges in EEG: a clinical validation study* [Dataset]. Dryad. https://doi.org/10.5061/dryad.xsj3tx99w

Place the dataset files in the notebook working directory with this layout:

```text
/content/
  S01.edf
  S02.edf
  ...
  Demographics_&_gold_standard.xlsx
  IFCN_criteria.xlsx
```

---

## Repository Contents

```text
.
|-- LICENSE
|-- README.md
|-- requirements.txt
`-- spikes_validation_framework.ipynb
```

| File | Purpose |
|---|---|
| `spikes_validation_framework.ipynb` | Full code-only analysis notebook, cleared of prior outputs. |
| `requirements.txt` | Python packages required by the notebook. |
| `README.md` | Reproducibility guide and data citation. |
| `LICENSE` | Apache License 2.0 for repository code. |

---

## Analysis Overview

The workflow follows five validation levels:

| Level | Aim | Main output |
|---|---|---|
| L1 | Binary discrimination | AUC, precision-recall, reliability |
| L2 | IFCN count alignment | Correlation with neurologist criterion count |
| L3 | Criterion-level alignment | Per-criterion performance and agreement |
| L4 | Mechanistic timing and interpretability | Saliency timing, HiResCAM visualizations |
| L5 | Selective prediction | Risk-coverage behavior |

Model structure:

```text
EDF clips -> preprocessing -> 4 s sliding windows
                 |                    |
                 v                    v
       Branch A: channel-mean GASF    Branch B: per-channel GASF
                 |                    |
                 v                    v
          frozen ResNet-18 features   frozen ResNet-18 features
                 |                    |
                 v                    v
          fold-safe validation heads -> logit-space ensemble
```

---

## Environment

Create the environment and install the requirements:

```bash
python -m venv .venv
.venv\Scripts\activate
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

Launch Jupyter:

```bash
jupyter notebook spikes_validation_framework.ipynb
```

---

## Run Order

1. Open `spikes_validation_framework.ipynb`.
2. Confirm the dataset paths in the first configuration cell.
3. Run the notebook from top to bottom.
4. Review the generated tables, figures, and CSV summaries in the output directory.

The notebook is deterministic where random seeds are specified. The primary analysis uses nested cross-validation with fold-safe preprocessing and validation heads.

---

## Key Outputs

The notebook writes analysis artifacts to the configured output directory:

| Output type | Examples |
|---|---|
| Summary tables | discrimination metrics, IFCN alignment, timing summaries |
| Figures | ROC/PR/reliability, IFCN count alignment, per-criterion plots, interpretability examples |
| CSV files | out-of-fold predictions, criterion-level summaries, timing validation results |

---

## Citation

For the dataset, cite:

Kural, M. A., Duez, L., Sejer Hansen, V., Larsson, P. G., Rampp, S., Schulz, R., Tankisi, H., Wennberg, R., Bibby, B., Scherg, M., & Beniczky, S. (2020). *Criteria for defining interictal epileptiform discharges in EEG: a clinical validation study* [Dataset]. Dryad. https://doi.org/10.5061/dryad.xsj3tx99w

For this code companion, cite the associated manuscript:

*Epilepsy or Variant? Development and Internal Validation of Visual EEG Encoding for Interpretable Sharp Transient Classification in Routine EEG.*

---

## License

Code in this repository, including the analysis notebook and repository support files, is licensed under the Apache License 2.0. See [`LICENSE`](LICENSE).

The Dryad EEG dataset is not redistributed in this repository. Dataset access and reuse are governed by the Dryad dataset record and its associated terms: https://doi.org/10.5061/dryad.xsj3tx99w
