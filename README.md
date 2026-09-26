# Early degradation patterns in perovskite solar cells

[中文说明](README_CN.md) · [Detailed workflow (中文)](WORKFLOW_CN.md)

This repository is a reproducibility package for exploring the **0–200 h** behavior of literature-mined perovskite solar-cell stability curves. It contains a fixed set of **1,842 curves**, their source CSV files and paper figures, the inputs used for the fit, analysis code, and the generated four-cluster results.

**Method note:** This package fits a **density-weighted K-means** model to five shape features derived from each curve. It does **not** implement a self-organizing map (SOM). The four groups are exploratory patterns, not independently validated material or degradation classes. Earlier input curation and method selection were informed by provisional shape candidates; only the fit on the fixed inputs is label-free.

## What is included

| Path | Purpose |
| --- | --- |
| `data/input_index.csv` | Fixed IDs, order, and provenance for the 1,842 curves |
| `data/points/` | Normalized observations within 0–200 h for input quality checks |
| `data/shape_inputs.npz` | Aligned 64-point observation-rank shapes read by the model |
| `data/raw_csv/`, `data/curve_index.csv` | Extracted source CSVs, paper images, replots, provenance, and SHA-256 hashes |
| `config.json`, `run.py` | Fixed parameters and four-cluster analysis |
| `verify.py` | Input and output integrity checks |
| `results/` | Assignments, model arrays, plots, summaries, source copies grouped by cluster, and verification reports |
| `results/dashboard.html` | Offline browser for the 1,842 curves and source images |

The `results/cluster_1/` through `results/cluster_4/` directories contain copies of source DOI folders. A DOI folder can occur in more than one cluster; each cluster's `members.csv` identifies which CSVs belong to it.

## Reproduce

Use Python 3.9 in an isolated environment. From this directory:

```bash
python3 -m pip install -r requirements.txt
python3 verify.py
OPENBLAS_NUM_THREADS=1 OMP_NUM_THREADS=1 python3 run.py
python3 verify.py --check-results
```

The included result has **1,710 / 44 / 21 / 67** curves in clusters 1–4. The dashboard names these patterns **Slope / Bridge / Valley / Hill**. Open `results/dashboard.html` locally to inspect individual curves, figures, and CSVs. Runtime versions are recorded in `results/environment.json`.

## Scope and interpretation

This package starts from the **already selected** 1,842 curves. It does not redigitize figures from papers or repeat selection from the larger candidate collection. Raw CSVs retain their original time units; fitting uses the aligned 64-point observation-rank shapes, so elapsed hours establish the early window and observation order but are **not** a distance dimension in clustering. The groups describe relative curve shapes and should not be interpreted as verified labels or a classification accuracy result.

For the feature construction, weighting, fit, output definitions, and methodological boundaries, see [WORKFLOW_CN.md](WORKFLOW_CN.md).
