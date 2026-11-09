# ML4T 学习路径（个人笔记）

主线：以 `case_studies/etfs/` 为主干，按编号顺序读，不懂的概念跳回对应章节。

全书核心思想："Process is your edge"，流程纪律比模型复杂度重要。要掌握的是三条纪律：

- **证据边界**：调参用的数据和最终评估用的数据（holdout）严格分开。
- **walk-forward 交叉验证**，所有数据 point-in-time 正确，防止未来数据泄露。
- **防过拟合和多重检验**：Deflated Sharpe Ratio、BH-FDR。

---

## 第 0 步：开始前要读的 Markdown

### 必读（约 400 行，1 小时内）

- [ ] `docs/what-this-is.md`：哪些能直接复现，哪些需要 GPU 或额外数据，哪些不承诺
- [ ] `01_process_is_edge/README.md`：全书总纲，整体工作流和证据边界
- [ ] `06_strategy_definition/README.md`：交易设定、评估指标、walk-forward 协议、baseline、run logging
- [ ] `case_studies/README.md`：9 个案例共用的流水线阶段和目录结构
- [ ] `case_studies/etfs/README.md`：etfs 参数和 notebook 与章节对照表

### 选读

- [ ] `docs/running-notebooks.md` 的 "Case Study Notebooks" 一节（约第 199-360 行）：案例配置、run log、重跑结果为何与书不完全一致
- [ ] `07_defining_the_learning_task/README.md`：标签、IC、多重检验（也可读到 etfs 02 时再看）

### 暂时不读，用到再看

| 文件 | 什么时候看 |
|---|---|
| `docs/installation.md` | 要装环境运行代码时 |
| `data/README.md` | 下载数据或配置 API key 时 |
| `case_studies/RUN_LOG.md` | 读到 06_linear 或 13_model_analysis，对 `registry.db` 有疑问时 |

> 各章 README 只是导读，正文在书里。手边有书的话，第 1、6 章读原书。

---

## 第 1 步：etfs 案例参数速记

| 项目 | 值 |
|---|---|
| 资产 | 100 只 ETF，9 个类别（股票、债券、商品、外汇、地产） |
| 频率 | 日线数据，月末决策 |
| 历史 | 2006-2025 |
| 主标签 | `fwd_ret_21d`（21 日远期收益） |
| CV | 8 折，10 年训练 + 1 年验证 |
| 成本 | 每边 5-15 bps |
| 策略 | long-only，排序后调仓 |

本案例的教学重点：IC 最好的模型家族，不一定是实际交易 Sharpe 最好的；组合构建决定哪套预测能活下来。

---

## 第 2 步：按顺序读 etfs notebook

在 VS Code 里打开 `.ipynb`（大部分已保存运行结果，不装环境也能看图表）。`.py` 只有代码，没有输出。

| 顺序 | Notebook | 对应章节 | 内容 | 第一遍 |
|---|---|---|---|---|
| 01 | `01_feasibility_analysis` | Ch6 | 宇宙广度、point-in-time 资格、波动 vs 成本、walk-forward 演示 | ✅ 必读 |
| 02 | `02_labels` | Ch7 | 21 日 / 5 日远期收益标签 | ✅ 必读 |
| 03 | `03_financial_features` | Ch8 | 动量、波动率、跨资产排序特征 | ✅ 必读 |
| 04 | `04_model_based_features` | Ch9 | ARIMA、HMM、频谱特征 | ✅ 必读 |
| 05 | `05_evaluation` | Ch7-9 | 特征与标签的 IC 诊断 | ✅ 必读 |
| 06 | `06_linear` | Ch11 | Ridge / LASSO / ElasticNet baseline | ✅ 必读 |
| 07 | `07_gbm` | Ch12 | LightGBM + Optuna | ✅ 必读 |
| 08 | `08_tabular_dl` | Ch12 | TabM | ⏭ 跳过 |
| 09 | `09_dl_lstm` | Ch13 | LSTM | ⏭ 跳过 |
| 10 | `10_dl_tsmixer` / `10a_dl_nlinear` | Ch13 | TSMixer / NLinear | ⏭ 跳过 |
| 11 | `11_latent_factors` + `11a`-`11e` | Ch14 | PCA、IPCA、自编码器、SDF | ⏭ 跳过 |
| 12 | `12_causal_dml` | Ch15 | Double ML 因果检验 | ⏭ 跳过 |
| 13 | `13_model_analysis` | Ch11-15 | 所有模型家族 IC 横向比较 | ✅ 必读 |
| 14 | `14_backtest` | Ch16 | 回测，与等权基准对比证伪 | ✅ 必读 |
| 15 | `15_portfolio_management` | Ch17 | 分数加权、风险平价、MVO、HRP 等 | ✅ 必读 |
| 16 | `16_risk_management` | Ch19 | 止损、移动止损、时间退出 | ✅ 必读 |
| 17 | `17_costs` | Ch18 | 交易成本对收益的侵蚀 | ✅ 必读 |
| 18 | `18_holdout_predictions` | Ch20 | 在 holdout 前的历史上重新拟合 | ✅ 必读（无保存输出） |
| 19 | `19_holdout_backtest` | Ch20 | holdout 回测 | ✅ 必读 |
| 20 | `20_strategy_analysis` | Ch20 | 端到端策略结论 | ✅ 必读（无保存输出，需自己运行） |

注意：

- 08-12 只是把模型换成别的家族，第一遍跳过。13 会把所有家族放在一起比较。
- `11d`、`18`、`20` 没有保存输出。`20` 是最终结论，需要装环境、下载 artifacts 后运行。
- 只看不跑可以随意跳。要运行就必须按顺序，前面的 notebook 生成后面要用的文件（labels → features → `run_log/registry.db`）。

---

## 第 3 步：不懂时跳回的章节

| 遇到的问题 | 回到 |
|---|---|
| 整体流程、证据边界 | `01_process_is_edge/` |
| 交易设定、评估协议、walk-forward | `06_strategy_definition/` |
| 标签、IC、多重检验、Deflated Sharpe | `07_defining_the_learning_task/` |
| 价格特征 | `08_financial_features/` |
| Kalman、GARCH、HMM 等模型特征 | `09_model_based_features/` |
| 线性 baseline、SHAP、conformal | `11_ml_pipeline/` |
| GBM、Optuna、TreeSHAP | `12_gradient_boosting/` |
| 回测、过拟合控制 | `16_strategy_simulation/` |
| 组合构建 | `17_portfolio_construction/` |
| 交易成本 | `18_transaction_costs/` |
| 风险管理 | `19_risk_management/` |
| 从预测到策略的总结 | `20_strategy_synthesis/` |

第一遍可以不碰：第 2-5 章（数据基础设施）、第 10 章（文本特征）、第 21-26 章（RL、RAG、Agent、实盘、MLOps）、其余 8 个 case study。

---

## 第 4 步：需要运行时

```bash
# 安装（macOS Apple Silicon 推荐本地 uv）
uv sync
uv run python scripts/verify_installation.py

# 最小数据集（约 75 MB）
uv run python data/download_all.py --free-only --skip-firm-characteristics

# etfs 预计算结果（约 33 MB），不用重新训练
uv run python scripts/download_artifacts.py --cs etfs

# 启动 Jupyter
ML4T_DATA_PATH="${ML4T_DATA_PATH:-$PWD/data}" uv run jupyter lab

# 或直接跑分析
uv run python case_studies/etfs/13_model_analysis.py
uv run python case_studies/etfs/20_strategy_analysis.py
```

完整环境约需 16 GB 磁盘（环境 11 GB，免费数据 4 GB）。
