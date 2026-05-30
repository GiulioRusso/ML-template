# Machine Learning Boilerplate

Customizable notebook template for classical ML pipelines. Supports classification
and regression out of the box. Starting point — not a definitive recipe.

---

## Quickstart

```bash
pip install -r requirements.txt
jupyter notebook main.ipynb
```

Place your dataset at `data/data.csv`, set the Config cell, run all cells.

---

## Customization map

Work through this list in order — everything else runs automatically.

### 1 — Point at your data
In the **Config** cell:
```python
FILE_PATH  = "data/your_file.csv"
TARGET_COL = "your_target_column"
```

### 2 — Pick your task
```python
TASK = "classification"   # or "regression"
```

### 3 — Pick a model
```python
MODEL = "random_forest"   # see full list below
```

Available models:

| Key | Type | Algorithm |
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

To add a model, append an entry to the `MODELS` dict in the registry cell:
```python
"my_model": (MyEstimator(...), {"model__param": [v1, v2]}),
```

### 4 — Configure preprocessing
```python
ENCODE_CATEGORICAL = True          # one-hot encode object/category columns
REDUCTION = "none"                 # "none" | "pca" | "kbest"
NUM_FEATURES = 10                  # kbest: number of features to keep
VARIANCE = 0.95                    # pca: variance to retain
```

### 5 — Tune evaluation settings
```python
TRAIN_SIZE = 0.8
NUM_FOLD   = 5
RANDOM_SEED = 42
```

### 6 — Save results (optional)
```python
SAVE_EXPERIMENT = True
```

Writes to `experiments/<timestamp>/`:
```
config.txt          resolved config values
best_params.txt     best hyperparameters from grid search
metrics.txt         test metrics
plots/              evaluation figures (confusion matrix / residuals / heatmap)
```

---

## Directory structure

```
project/
├── data/
│   └── data.csv          your dataset goes here
├── experiments/          run outputs (auto-created, gitignored)
├── main.ipynb
├── requirements.txt
├── README.md
└── plan.md               improvement plan and design rationale
```

---

## License

MIT
