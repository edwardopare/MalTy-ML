# Malaria, Typhoid, and Co-Infection Classification

This repository explores a machine-learning workflow for classifying synthetic
records into three labels:

- `Malaria`
- `Typhoid`
- `Both` (co-infection)

The predictors are reported symptom flags plus temperature and heart rate. The
project is a methodological demonstration built on synthetic data; it is not a
clinically validated diagnostic system and must not be used to diagnose or
treat patients.

## Repository contents

```text
dataset/
  generate_dataset.ipynb
  synthetic_malaria_typhoid_dataset.csv
modelling/
  Notebook.ipynb
  plots/
requirement.txt
README.md
```

- `dataset/generate_dataset.ipynb` generates the synthetic dataset.
- `dataset/synthetic_malaria_typhoid_dataset.csv` contains 50,000 synthetic
  records.
- `modelling/Notebook.ipynb` contains exploratory analysis, model selection,
  locked-holdout evaluation, and an in-memory inference check.
- `modelling/plots/` contains existing SHAP plots from earlier experiments.
  They are not outputs of the final three-model evaluation workflow.

## Modelling workflow

The final workflow in `modelling/Notebook.ipynb`:

1. detects binary symptom columns;
2. uses those symptoms with `Temperature` and `Heart_Rate`;
3. excludes `Platelet_Count`, `Noise`, `Symptom_Count`, `Total_Symptoms`, and
   `Severity_Index` from modelling;
4. creates a stratified 80/20 training and locked-holdout split;
5. performs randomized hyperparameter search with five-fold stratified
   cross-validation on the training partition; and
6. evaluates the selected candidates on the holdout partition.

The final comparison includes:

- Logistic Regression
- Random Forest
- LightGBM

Macro F1 is the cross-validation selection metric because the `Both` class is
rare. The notebook also reports balanced accuracy, macro precision and recall,
ROC AUC, average precision, log loss, per-class metrics, and a confusion
matrix. In the saved notebook run, Logistic Regression was selected by
cross-validated macro F1. The results preserved in that notebook run are
summarized below.

Although the notebook retains some imports from earlier experiments (including
XGBoost, SVM, gradient boosting, and scikit-learn's `MLPClassifier`), those
models are not part of the final comparison described above.

## Technology

The implemented workflow primarily uses:

- Python
- pandas and NumPy
- scikit-learn
- LightGBM
- SciPy
- Matplotlib and seaborn
- Jupyter

TensorFlow and Keras are not used by the repository. There is also no
blood-smear image model or OpenCV processing pipeline in the current project.

## Setup

Python 3.11 or another version compatible with the pinned dependencies is
recommended.

```sh
git clone https://github.com/edwardopare/MalTy-ML.git
cd MalTy-ML
python -m venv .venv
```

Activate the environment:

```powershell
# Windows PowerShell
.\.venv\Scripts\Activate.ps1
```

```sh
# macOS or Linux
source .venv/bin/activate
```

Install the repository's pinned environment:

```sh
python -m pip install -r requirement.txt
```

Start Jupyter and open the notebooks from the repository:

```sh
jupyter notebook
```

The modelling notebook resolves the dataset path when run from either the
repository root or the `modelling` directory.

## Results and interpretation

### Final model comparison

The following values are taken from the outputs stored in
`modelling/Notebook.ipynb`. Model selection was based only on five-fold
cross-validated macro F1 from the training partition; holdout results were
calculated afterward on 10,000 locked records.

| Model | CV macro F1 | CV SD | Holdout accuracy | Balanced accuracy | Macro precision | Macro recall | Macro F1 | Macro ROC AUC | Macro average precision | Log loss |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| **Logistic Regression** | **0.8115** | 0.0143 | **0.9272** | **0.8282** | 0.8387 | **0.8282** | **0.8333** | **0.9821** | **0.8721** | **0.1860** |
| LightGBM | 0.7809 | 0.0083 | 0.9162 | 0.7391 | 0.8323 | 0.7391 | 0.7743 | 0.9770 | 0.8406 | 0.2283 |
| Random Forest | 0.6845 | 0.0147 | 0.8926 | 0.6420 | **0.9061** | 0.6420 | 0.6813 | 0.9433 | 0.7570 | 0.2931 |

Higher values are better for every column except log loss. Bold values mark
the strongest result in each column. Logistic Regression is the final model
because it achieved the highest cross-validated macro F1, not because of its
holdout performance.

### Findings

- Logistic Regression produced the best cross-validated macro F1 and retained
  the best overall balance on the locked holdout set.
- LightGBM ranked second, while Random Forest had the highest macro precision
  but substantially lower macro recall and macro F1.
- The selected model generalized consistently in this experiment: its holdout
  macro F1 was 0.8333, compared with a cross-validated estimate of
  0.8115 +/- 0.0143.
- Performance remained weakest for the rare `Both` class. Logistic Regression
  achieved 0.667 precision, 0.640 recall, and 0.653 F1 on 100 holdout
  co-infection records.
- The same model achieved 0.945 F1 for `Malaria` (6,400 holdout records) and
  0.902 F1 for `Typhoid` (3,500 holdout records). The overall 0.927 accuracy
  therefore should not be read without the per-class and macro metrics.
- In permutation importance measured by the decrease in holdout macro F1,
  `Yellow_Eyes`, `Abdominal_Pain`, `Anemia`, `Fatigue`, and `Temperature` were
  the five highest-ranked features in the saved run.

These findings describe the fitted model's behavior on data created by this
repository's synthetic generator. Feature importance does not establish
causality or clinical relevance, and the performance figures do not demonstrate
diagnostic accuracy in real patients.

Important limitations include:

- all records come from a single synthetic generation process;
- the target set has no `Neither` or other-febrile-illness class;
- important clinical variables and laboratory confirmation are absent;
- there is no external, temporal, or site-level validation;
- probabilities have not been clinically calibrated; and
- no decision threshold has been evaluated against real clinical harms.

Before any clinical use could be considered, the workflow would require
representative and ethically governed patient data, laboratory-confirmed
reference labels, external validation, calibration and subgroup analysis, and
prospective safety evaluation.

## Contributors

- Edward Opare-Yeboah
- Prince Acquah Rockson
- Eric Sena Semordzi

## License

No license file is currently included in this repository. Unless one is added,
do not assume that the project is distributed under the MIT License.
