# Current Status (梳理与总结)

## 目标

验证 `FUTURE_SEED=1` 是否能在 image inpainting 上稳定提升前景恢复能力。

- 主指标：`maskacc_fg_val`
- coarse 门槛：`+0.8pp` (`0.008`)
- confirm 门槛：3 个 seed 平均 `+1.5pp`

## 最新状态（Cycle-2，当前主结论）

本轮新增 `Stage0 Learnability Gate` 与 FG 诊断后，结果如下：

1. Stage0（FS0 单跑，门槛 `best_maskacc_fg_val >= 0.02`）失败。
2. 因 Stage0 失败，`smoke/coarse/confirm` 按协议全部跳过。
3. FG 诊断显示 `zero_fg_batches=0`，说明不是“前景样本为空”导致指标为 0。

关键数值：
- `best_maskacc_fg_val = 0.0000`
- Stage0 gate: fail (`0.0000 < 0.0200`)

证据：
- `results/mnist_fg_stage0_v2_20260226T124341Z/gate.json`
- `results/mnist_fg_stage0_v2_20260226T124341Z/prune_table.csv`
- `results/mnist_fg_stage0_v2_20260226T124341Z/summary_agg.json`
- `results/mnist_fg_stage0_v2_20260226T124341Z/fs0_seed20260226.log`

## 之前历史（Cycle-1，保留）

历史轮次曾执行 `smoke -> coarse`（无 Stage0）：

- smoke：通过指标链路
- coarse：失败（`avg_delta_best_fg = 0.000819 < 0.008`）
- confirm：跳过

历史证据：
- `results/mnist_fg_smoke/`
- `results/mnist_fg_coarse/`
- `results/mnist_fg_confirm/`

## 本轮新增实现

在 `rwkv_diff_future_seed.py` 上新增：

1. FG 统计诊断输出：
- `fg_masked_cnt`
- `masked_cnt`
- `fg_cnt`
- `zero_fg_batches`
- `used_batches`
- `total_batches`

2. FG 评估去偏：
- 对 `fg_only` 路径，当 batch 内 `fg_masked_cnt==0` 时，不计入 FG 平均分母。

3. JSONL 新字段（eval event）：
- `fg_masked_cnt_{split}`
- `masked_cnt_{split}`
- `fg_cnt_{split}`
- `zero_fg_batches_{split}`
- `used_batches_{split}`
- `total_batches_{split}`

## 决策结论

`当前配置数｜通过数｜最佳配置｜风险｜下一步`

`1(Stage0)｜0｜FS0(stage0) best_maskacc_fg=0.0000｜fg样本存在但前景恢复完全未学到(zero_fg_batches=0)｜先改任务可学习性(mask设计/量化)后再重开FS对比`

## 相关报告

- `results/mnist_fg_stage0_hard_stop_report.md`
- `results/mnist_fg_hard_stop_report.md` (Cycle-1)
- `results/decision_report.txt`
- `results/fg_metric_logic_check.txt`
