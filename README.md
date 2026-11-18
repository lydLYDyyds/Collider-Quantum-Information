# Collider Quantum Information

本项目记录我用 MadGraph5_aMC@NLO 研究对撞机末态自旋纠缠的过程。第一个算例是 13 TeV 质子碰撞中的胶子融合子过程 $gg\to t\bar{t}$。我把顶夸克与反顶夸克的自旋作为两个量子比特，从逐事件自旋密度矩阵计算并发度（concurrence），并用最低阶 QCD 的解析结果检验数值分析。

## 已完成的算例

| 项目 | 设置 |
| --- | --- |
| 运行标签 | `test_002` |
| 事件生成 | MadGraph5_aMC@NLO 3.8.1，树级 $gg\to t\bar{t}$ |
| 质子束能量 | 每束 6.5 TeV |
| 顶夸克质量 | 173 GeV |
| PDF | NNPDF2.3 NLO（MadGraph 标签 `nn23nlo`） |
| 样本量 | 50,000 个无权事件 |

从 LHE 四动量重建顶夸克对的质心系。横轴为折叠散射角 $2\theta_{\mathrm{fold}}/\pi$，其中 $\theta_{\mathrm{fold}}=\arccos(|\cos\theta^*|)$，$\theta^*$ 是顶夸克相对束轴的散射角。纵轴为顶夸克在该质心系中的速率 $\beta=\sqrt{1-4m_t^2/M_{t\bar{t}}^2}$，这里使用 $c=1$。

[查看并发度相图（PDF）](test_002_gg_phase.pdf)

左图为 LO QCD 解析预言，右图将逐事件并发度划入 $40\times40$ 个格子并取格内平均。1,600 格中有 1,427 格采到了事件，右图白格表示该格无事件。在同一事件运动学点上，模拟与解析并发度的最大绝对差为 $1.95\times10^{-8}$。这个差值用于核对数值实现。本项目当前覆盖树级的产生过程，尚未纳入顶夸克衰变和探测器响应。

## 仓库文件

| 文件 | 内容 |
| --- | --- |
| [test_002_gg_phase.pdf](test_002_gg_phase.pdf) | 理论与模拟的并发度相图 |
| [test_002_gg_analysis.zip](test_002_gg_analysis.zip) | Python 分析脚本、逐事件计算结果、分箱表和统计摘要 |

解压分析包后，`events.csv.gz` 可直接用于核对逐事件的 `C_MG`、`C_LO` 和差值 `delta`。`bins.csv` 记录每格的事件数和平均并发度，`stats.json` 记录样本覆盖和数值差异。

## 从原始事件重跑

本步骤需要带逐事件密度矩阵的 `unweighted_events.lhe.gz`。原始 LHE 需要另行取得，仓库中提供的是从它导出的分析数据。将 LHE 放到解压后的分析目录，在该目录中执行下列命令。Python 3.12 环境已验证。

```bash
python -m pip install numpy matplotlib
python gg_phase.py unweighted_events.lhe.gz --bins 40 --out reproduced
```

脚本会重新生成相图、逐事件表、分箱表和统计摘要。

## 参考文献

本算例参考论文图 3 的并发度相图。LO 解析式及零并发度边界采用 arXiv v1 的式 (18)–(20)。论文图使用 100 万个无权事件，本项目的相图由 50,000 个事件生成。

Valentin Durupt, Fabio Maltoni, and Olivier Mattelaer, *Automated computation of spin-density matrices and quantum observables for collider physics*, JHEP 04 (2026) 103. [DOI](https://doi.org/10.1007/JHEP04%282026%29103) · [arXiv:2510.17730](https://arxiv.org/abs/2510.17730)
