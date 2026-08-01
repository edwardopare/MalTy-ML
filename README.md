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
cross-validated macro F1. Exact metric tables are generated when the notebook
is run; they are not duplicated here to avoid stale or unverifiable figures.

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

The saved notebook identifies Logistic Regression as the strongest candidate
by cross-validated macro F1 within this synthetic benchmark. This result shows
how well the model recovers patterns encoded by the data generator; it does
not demonstrate diagnostic performance in real patients.

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
