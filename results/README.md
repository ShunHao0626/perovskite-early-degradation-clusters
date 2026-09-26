# 1842 条曲线四簇复现结果

运行日期：2026-09-26。使用本目录上级的固定输入、`config.json` 和 `run.py`，执行：

```bash
python3 verify.py
OPENBLAS_NUM_THREADS=1 OMP_NUM_THREADS=1 python3 run.py
python3 verify.py --check-results
```

输入核验和结果核验均通过，1842 条曲线全部纳入，排除 0 条。Cluster 1、2、3、4 分别包含 1710、44、21、67 条曲线。版本信息见 `environment.json`。

主要文件：`cluster_assignments.csv` 为逐曲线簇号；`model.npz` 为数值模型及中间数组；`representatives.csv` 为代表曲线；`summary.json` 为摘要及输入、配置和代码的 SHA-256；`cluster_four_groups.png` 和 `figures/Supplementary_Fig_S1_zoom.png`/`.svg` 为生成图。`verify_inputs.json`、`verify_results.json` 和 `run.log` 保存运行记录。

`cluster_1/` 至 `cluster_4/` 完整复制对应的 `data/raw_csv/` DOI 文件夹。每个目录的 `members.csv` 列出真正属于该簇的曲线 CSV；同一个 DOI 文件夹可能含不同簇的曲线，因此会在多个簇目录中出现。

直接打开 `dashboard.html` 可浏览全部曲线及论文原图、replot 图。页面中的名称为 Cluster 1 = Slope、Cluster 2 = Bridge、Cluster 3 = Valley、Cluster 4 = Hill；`dashboard_data.json` 是对应的结构化数据。

本次复现从已固定的 1842 条输入曲线开始，不包含论文图片重新数字化或候选池重新筛选。Cluster 1–4 属于探索性分析，不能解释为独立验证的真实类别或分类准确率。
