# 学习进度

学习路径见 [`LEARNING_PATH.md`](LEARNING_PATH.md)。

## 当前进度

- 阶段：第 0 步，开始前要读的 Markdown
- 下一个：`06_strategy_definition/README.md`

## 已读

### 2026-11-10 `docs/what-this-is.md`

讲在投入时间之前，这个仓库能做什么、不能做什么，按代价分四级：

1. **直接复现**：大部分章节 notebook 用免费数据就能跑。9 个 case study 的训练、预测和回测结果可以直接下载（etfs 约 33 MB，全部约 3.1 GB），分析类 notebook 读的就是这些结果，不用重新训练。
2. **花点功夫就能修改**：换超参数、改回测设置只需改 YAML 配置（`setup.yaml`、训练菜单、preset），不用改代码。实验要放在单独的可写目录里，别覆盖官方结果。
3. **需要较多资源**：完整重训一个案例要很多算力和磁盘（9 个案例的 run log 共 73 GB）。深度学习部分按 CUDA GPU 写，CPU 上很慢，个别 notebook 没有 GPU 直接报错。
4. **不承诺**：没有任何策略号称能赚钱，有的还是故意演示失败。第 25 章的实盘对接只是接口演示。没有技术支持。

另外：重跑结果会和书里有小差异（库版本、硬件、CPU 与 GPU 的区别），两者不一致时以仓库为准。

**对我的要点：**

- 第一遍只读不跑，不需要重新训练。大部分 `.ipynb` 已保存运行输出。
- 需要运行 `20_strategy_analysis` 时，下载 etfs 的 artifacts 就够了。
- 数字和书对不上是正常的，结论不变就行。
- Mac 没有 CUDA，深度学习 notebook（etfs 08-12）会很慢，第一遍本来就跳过。
- 读完一遍后，挑 `06_linear` 或 `07_gbm` 改一个参数自己跑，检验是否真懂。

### 2026-11-10 `01_process_is_edge/README.md`

**核心观点：** 交易里能长期赚钱，靠的是有纪律的研究流程，而不是复杂的模型。市场一直在变，ML 交易要解决的是怎么适应变化，不是选哪个模型最好。

**1. 证据边界（最重要）**

| 机器学习名词 | 在边界哪一侧 | etfs 里的位置 |
|---|---|---|
| 训练集 | 探索侧，可反复使用 | 每折 10 年 |
| 验证集 | 探索侧，可反复使用 | 8 折 walk-forward，每折 1 年（约 2016-2023） |
| 测试集 | 确认侧，**只用一次** | holdout：2024-2025 |

守住边界的三个做法：

- **sealed holdout**：测试集在探索期间完全不碰。etfs 的 01-17 都不碰，18、19 才用。
- **trial logging**：每次尝试都登记，不能只报最好的那次（`run_log/registry.db`）。
- **selection-aware evaluation**：按尝试次数给结果打折（Deflated Sharpe Ratio）。

金融数据比普通 ML 多几条要求：

- 按时间切分，不能随机打乱。
- 集合之间留间隔，防止标签泄露（purge / embargo）。
- 所有数据 point-in-time 正确。
- 测试集之前，整套策略决策（标签、特征、模型、仓位、止损、成本）都要定死。

**2. 模型为什么会失效**

市场是非平稳的，历史上学到的规律迟早会部分失效：

| 术语 | 含义 |
|---|---|
| data drift | 特征 X 的分布变了 |
| concept drift | X 和收益 Y 的关系变了，最危险 |
| structural break | 规律突然、永久地改变 |
| regime | 市场在几种状态之间来回切换 |
| online detection | 实盘中实时发现上面这些变化 |

regime 用来风控，不用来择时：事先定好规则（比如高波动时降仓位），不拿它预测涨跌。

**3. 怎么判断结果是否稳定**

不看整体平均值，按 8 折逐段看：几折方向一致、最差那折多少、均值和中位数差多少、高波动和低波动下是否都成立。

- `05_evaluation` D 节：特征是否稳定
- `13_model_analysis` 第 3-6 节：模型是否稳定（热力图、`family_stats`、箱线图、regime 分组）
- `18`、`19`：holdout 上是否还成立

**可略读：** 1.3（新工具让纪律更重要，不能替代纪律），1.5（个人研究者要自己写文档、设检查点和停止条件），两个 GMM regime notebook。

**读 etfs 时带着两个问题：** 这一步有没有让未来信息混进来？这个结果在不同时间段稳定吗？

## 待读

- [x] `01_process_is_edge/README.md`
- [ ] `06_strategy_definition/README.md`
- [ ] `case_studies/README.md`
- [ ] `case_studies/etfs/README.md`
- [ ] （选读）`docs/running-notebooks.md` 的 "Case Study Notebooks" 一节
- [ ] （选读）`07_defining_the_learning_task/README.md`
