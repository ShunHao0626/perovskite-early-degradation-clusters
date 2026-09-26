# Perovskite early degradation: four curve patterns

[中文](README_CN.md) · [Detailed method (中文)](docs/WORKFLOW_CN.md)

A reproducible analysis of **1,842 literature-mined perovskite solar-cell stability curves** over the **0–200 h** early period. The included results group curve shapes into four exploratory patterns: **Slope, Bridge, Valley, and Hill**.

## Core result: zoomed-in view of the four clusters

<p align="center"><a href="results/figures/Supplementary_Fig_S1_zoom.png"><img src="results/figures/Supplementary_Fig_S1_zoom.png" alt="Zoomed-in curves and median trends for the four clusters" width="440"></a></p>

The pale lines are individual curves; black lines show smoothed median trends for display. Each panel has its own zoomed vertical scale, and the horizontal axis is **relative observation rank**, not elapsed hours. [Open the vector figure](results/figures/Supplementary_Fig_S1_zoom.svg) or [browse individual curves](results/dashboard.html).

> **Method:** The code uses five shape features and density-weighted K-means. It does not implement a self-organizing map (SOM). The fixed input set was curated earlier with provisional shape candidates, so the four groups are exploratory rather than independently validated classes.

## Repository layout

| Folder | Contents |
| --- | --- |
| [`code/`](code/) | Analysis, verification, plotting, dashboard builder, pinned dependencies, and configuration |
| [`data/`](data/) | Fixed input index, 0–200 h observations, 64-point shapes, extracted CSVs, and source figures |
| [`results/`](results/) | Included four-cluster output, figures, source copies, and offline [dashboard](results/dashboard.html) |
| [`docs/`](docs/) | Detailed method and interpretation |

## Reproduce the results

Use **Python 3.9**. From the repository root (macOS/Linux):

```bash
python3 -m venv .venv
.venv/bin/python -m pip install -r code/requirements.txt
.venv/bin/python code/verify.py
OPENBLAS_NUM_THREADS=1 OMP_NUM_THREADS=1 .venv/bin/python code/run.py
.venv/bin/python code/verify.py --check-results
```

The run writes to `results/` and replaces the generated cluster folders. Expected cluster sizes are **1,710 / 44 / 21 / 67**. Open `results/dashboard.html` locally to browse all curves and source figures. Parameters are in [`code/config.json`](code/config.json); the full workflow is in [`docs/WORKFLOW_CN.md`](docs/WORKFLOW_CN.md).

**Scope:** This repository reproduces clustering from a fixed set of already extracted curves. It does not repeat paper-figure digitization or selection from the larger candidate collection. Clustering uses observation rank, not elapsed hours, as its shape axis.
