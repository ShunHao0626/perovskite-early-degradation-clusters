# 1842 条曲线四簇分析：精简复现包

[English README](README.md) · [详细流程](WORKFLOW_CN.md)

本目录保留**固定的 1842 条输入曲线**、其原始 CSV、聚类代码和环境信息；本次生成的拟合结果保存在 `results/`。

研究范围是从论文中提取的钙钛矿太阳能电池 **0–200 小时**早期稳定性曲线。**本文件夹实际采用密度加权 K-means，而非 SOM（自组织映射）**；四簇是探索性的形状分组。此前的输入筛选及方法选择曾受暂定形状候选影响，只有在固定输入上的本次拟合不使用类别标签。

核心分析步骤与参数解释见 [`WORKFLOW_CN.md`](WORKFLOW_CN.md)。

## 最核心的内容

| 路径 | 用途 |
| --- | --- |
| `data/input_index.csv` | 1842 条输入的固定 ID 顺序、来源与归一化点文件位置 |
| `data/points/` | 每条曲线的早期归一化观测点；用于质量筛查 |
| `data/shape_inputs.npz` | 同序的 64 点序位形状输入；模型直接读取 `raw_rank` |
| `data/raw_csv/`、`data/curve_index.csv` | 1842 份原始提取 CSV、来源及 SHA-256 对照；用于追溯，模型不直接读取 |
| `config.json`、`run.py` | 固定参数及四簇拟合代码 |
| `render_si_zoom.py` | 依据本次拟合结果生成 zoom in 补充图 S1 |
| `verify.py` | 输入完整性和可选输出核验 |
| `requirements.txt` | 复现环境中的 Python 库版本 |
| `WORKFLOW_CN.md` | 核心分析流程、参数及方法边界 |
| `results/` | 本次复现的簇号、模型、图和核验记录 |
| `results/cluster_1/` ～ `results/cluster_4/` | 按簇复制的原始 DOI 文件夹；各目录的 `members.csv` 列出本簇曲线 |
| `results/dashboard.html` | 可离线打开的交互式结果浏览页面 |

## 运行

建议使用 Python 3.9 和隔离环境；先安装依赖：

```bash
python3 -m pip install -r requirements.txt
```

在**本目录**执行：

```bash
python3 verify.py
OPENBLAS_NUM_THREADS=1 OMP_NUM_THREADS=1 python3 run.py
python3 verify.py --check-results
```

`run.py` 默认在本目录下新建 `results/`，写入逐曲线簇号、数值模型、代表曲线表、图和摘要；其中 `figures/Supplementary_Fig_S1_zoom.png` 和 `.svg` 是 zoom in 图。该图使用本次拟合的曲线和簇号重新生成：Cluster 1 的中位趋势线仅为展示使用 `sigma=3.0` 平滑，其余簇使用 `sigma=1.2`，各面板分别放大纵轴。这些绘图设置不改变聚类。输出位置可在 `config.json` 的 `output_dir` 中修改。预期纳入 1842 条，Cluster 1–4 的大小依次为 `1710/44/21/67`。

每次运行还会在 `results/` 下生成 `cluster_1/` 至 `cluster_4/`：完整复制含本簇曲线的 `data/raw_csv/` DOI 文件夹。一个 DOI 文件夹可能有不同簇的曲线，因此会出现在多个簇目录；**以各簇的 `members.csv` 判断真正属于该簇的 CSV**。再次运行会覆盖这四个目录。

直接用浏览器打开 `results/dashboard.html`，可按簇和关键词浏览全部 1842 条曲线，查看 DOI、图号、曲线图例、论文原图、replot 图及原始 CSV。页面中的命名为 **Cluster 1 = Slope、Cluster 2 = Bridge、Cluster 3 = Valley、Cluster 4 = Hill**。

交付给别人时，把本目录连同 `results/` 一起交付；对方也可以运行上述命令自行生成结果。

## 范围与限制

本包重跑的是**已经固定的 1842 条曲线**，不是从论文图片重新数字化、从更大的候选池重新筛选曲线的端到端流程。原始 CSV 保留原横轴单位；模型实际读取预先整理的早期归一化点与 64 点序位输入，原始 CSV 不能直接替代它们。输入筛选曾使用旧的暂定形状候选，因此 Cluster 1–4 是探索性分析，不能作为独立验证的真实类别或分类准确率。原始交付中的噪声敏感性分析及结果表不属于此精简包。
