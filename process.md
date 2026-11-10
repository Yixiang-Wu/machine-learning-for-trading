# 学习进度

学习路径见 [`LEARNING_PATH.md`](LEARNING_PATH.md)。

## 当前进度

- 阶段：第 0 步，开始前要读的 Markdown
- 下一个：`01_process_is_edge/README.md`

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

## 待读

- [ ] `01_process_is_edge/README.md`
- [ ] `06_strategy_definition/README.md`
- [ ] `case_studies/README.md`
- [ ] `case_studies/etfs/README.md`
- [ ] （选读）`docs/running-notebooks.md` 的 "Case Study Notebooks" 一节
- [ ] （选读）`07_defining_the_learning_task/README.md`
