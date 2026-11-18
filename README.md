# BT4012 Group Project: Payment-Card Fraud Detection

Group 04 | Members: Melvin Loo, Lim Yu Cheng, Whelan Goh Eu Kiat | BT4012 Fraud Analytics, AY2026/27 Sem 1

Delay-aware comparison of a rule baseline, logistic regression (L1/L2), a feedforward network, an
autoencoder and LightGBM on the course card-transaction dataset, plus a personalised-PageRank
customer-terminal graph feature and a daily review-queue (business) view.

## Reproduce the results

1. Download `transactions_df.csv` from Canvas (BT4012 > Files) and place it in `data/`.
   The data is course material and is **not** included in this repository.
2. Install dependencies (Python 3.10+):
   ```bash
   pip install -r requirements.txt
   ```
3. Run the notebook top to bottom (about 10 to 20 minutes on a laptop CPU):
   ```bash
   jupyter nbconvert --to notebook --execute bt4012_group_project.ipynb --output executed.ipynb
   ```
   or open `bt4012_group_project.ipynb` and choose *Run All*. To use a different data location set
   `BT4012_DATA=/path/to/transactions_df.csv`.

Outputs are written to `results/` (tables, CSV) and `figures/` (PNG). The report quotes these files.
All random seeds are fixed (`SEED = 42`). Two runs on the same machine give identical tables. On another
platform, LightGBM results can differ in the third decimal (on Apple ARM, LightGBM 4.3 to 4.7 all give
the same scores, which differ slightly from an earlier run on another machine); the other models
reproduce exactly. The tables in `results/` come from the Apple ARM machine.

## Layout

| Path | Purpose |
|---|---|
| `bt4012_group_project.ipynb` | Full pipeline: profiling, folds, models, graph feature, business view |
| `results/` | Tables used in the report |
| `figures/` | Figures used in the report |
| `data/` | Put `transactions_df.csv` here (git-ignored) |

## Evaluation design (short)

* Three untouched test folds (train 28 days, delay 7 days, test 7 days), sliding forward in time.
  Fold 0 is used only to pick hyper-parameters.
* Test cards already known to be compromised are removed (course delay-aware protocol).
* Metrics: AUC-ROC, Average Precision, Card Precision@100, recall and amount-recall of the top
  100 transactions per day.
* `TX_FRAUD_SCENARIO`, IDs and time columns are never used as model inputs.

## AI assistance acknowledgement

Claude (Anthropic) assisted in drafting the code and report. The group reviewed, ran and is
responsible for all submitted work. The delay-aware split and Card Precision@k come from the
course notebook `04_Deep_Learning_Fraud_Detection.ipynb`.
