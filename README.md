# Machine Learning Template with Scikit-learn

Notebook-based classical ML pipeline. Supports classification and regression out of the box. Follow this guide top-to-bottom to adapt it to your dataset.

---

## Prerequisites

- Python 3.10+
- pip
- (Optional but recommended) a virtual environment

---

## Installation

```bash
# 1. Clone the repo
git clone <your-repo-url>
cd ML-template

# 2. Create and activate a virtual environment
python -m venv .venv
source .venv/bin/activate        # Windows: .venv\Scripts\activate

# 3. Install dependencies
pip install -r requirements.txt
```

---

## Smoke test — verify everything works

Before touching any config, confirm the notebook runs end-to-end with the sample data:

```bash
jupyter notebook main.ipynb
```

Open the notebook, run all cells (`Cell → Run All`). Expected: all cells execute without errors, evaluation plots appear in the output.

---

## Project layout

```
data/
  data.csv              your dataset goes here

experiments/            run outputs (auto-created, gitignored)

main.ipynb              single entrypoint — all pipeline stages
requirements.txt
README.md
```

---

## Experiment output

When `SAVE_EXPERIMENT = True`, each run writes a timestamped directory:

```
experiments/2026-01-15_14-30-00/
  config.txt            resolved config values
  best_params.txt       best hyperparameters from grid search
  metrics.txt           test metrics
  plots/
    confusion_matrix.png        (classification)
    residuals.png               (regression)
    feature_importance.png
```

---

## Customisation guide

Work through the config options **in this order** inside the **Config** cell of `main.ipynb`. Everything else runs automatically.

### Step 1 — Point at your data

```python
FILE_PATH  = "data/your_file.csv"
TARGET_COL = "your_target_column"
```

Place your CSV at the path above. `TARGET_COL` must match a column name in the file.

### Step 2 — Pick your task

```python
TASK = "classification"   # or "regression"
```

This controls which models are available and which metrics are computed.

### Step 3 — Pick a model

```python
MODEL = "random_forest"   # see table below
```

| Key | Task | Algorithm |
|-----|------|-----------|
| `random_forest` | classification | RandomForestClassifier |
| `svc` | classification | SVC |
| `xgboost` | classification | XGBClassifier |
| `mlp` | classification | MLPClassifier |
| `linear` | regression | LinearRegression |
| `ridge` | regression | Ridge |
| `lasso` | regression | Lasso |
| `rf_regressor` | regression | RandomForestRegressor |
| `svr` | regression | SVR |
| `gbr` | regression | GradientBoostingRegressor |

To add a custom model, append an entry to the `MODELS` dict in the **Model Registry** cell:

```python
"my_model": (MyEstimator(...), {"model__param": [v1, v2]}),
```

### Step 4 — Configure preprocessing

```python
ENCODE_CATEGORICAL = True     # one-hot encode object/category columns
REDUCTION = "none"            # "none" | "pca" | "kbest"
NUM_FEATURES = 10             # kbest: number of features to keep
VARIANCE = 0.95               # pca: variance to retain
```

### Step 5 — Tune evaluation settings

```python
TRAIN_SIZE  = 0.8    # fraction of data used for training
NUM_FOLD    = 5      # cross-validation folds
RANDOM_SEED = 42
```

### Step 6 — Save results

```python
SAVE_EXPERIMENT = True
```

Set to `True` to write the experiment output (config, metrics, plots) to `experiments/<timestamp>/`.

---

## Config reference

All options live in the **Config** cell of `main.ipynb`. Quick summary:

| Variable | Default | Description |
|----------|---------|-------------|
| `FILE_PATH` | `"data/data.csv"` | Path to input CSV |
| `TARGET_COL` | `"target"` | Name of the label column |
| `TASK` | `"classification"` | `"classification"` or `"regression"` |
| `MODEL` | `"random_forest"` | Key from the model registry |
| `ENCODE_CATEGORICAL` | `True` | One-hot encode categorical columns |
| `REDUCTION` | `"none"` | Feature reduction: `"none"`, `"pca"`, `"kbest"` |
| `NUM_FEATURES` | `10` | Features to keep (kbest) |
| `VARIANCE` | `0.95` | Variance to retain (pca) |
| `TRAIN_SIZE` | `0.8` | Train/test split ratio |
| `NUM_FOLD` | `5` | Cross-validation folds |
| `RANDOM_SEED` | `42` | Global random seed |
| `SAVE_EXPERIMENT` | `False` | Write output to `experiments/` |
