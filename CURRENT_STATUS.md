# Current Status (梳理与总结)

## 目标

验证 `FUTURE_SEED=1` 是否在 image inpainting / infilling 风格任务中稳定提升前景恢复。

- 主指标: `maskacc_fg_val`
- coarse 门槛: `+0.8pp` (`0.008`)
- confirm 门槛: 三 seed 平均 `+1.5pp` (`0.015`) 且单 seed 不为负

## 最新主结论 (Cycle-6 / run_tag=v6_20260227T111807Z)

正控任务 `left=right + col-major + prefix(0.5)` 已通过全流程：

1. Stage0: **PASS**
- `best_maskacc_fg=0.767722`

2. smoke: **PASS**
- `FS0` / `FS1(alpha=-2)` 指标链路完整

3. coarse: **PASS**
- `FS0 best_fg=0.767722`
- `FS1(alpha=-2) best_fg=0.806199`
- `delta_best_fg=+0.038476` (>= 0.008)

4. confirm: **PASS**
- seeds: `20260226, 20260227, 20260228`
- seed deltas: `[+0.038476, +0.025387, +0.198059]`
- mean delta: `+0.087307`
- all seeds non-negative: `true`

## 与上一成功轮次的关系

Cycle-5 (`v5_20260227T025027Z`, 自然任务 `mnist14b + col-major + prefix`) 也已 confirm 通过：
- mean delta: `+0.275744`

Cycle-6 的意义是“正控机制验证”：在明确 future-dependent 的构造任务里，FS 增益仍可稳定观察。

## 决策型汇报

`当前配置数｜通过数｜最佳配置｜风险｜下一步`

`10｜4(stage0+smoke+coarse+confirm)｜FS1(alpha=-2,left=right+col-major+prefix)｜正控任务接近高分区，增益幅度可能受天花板影响｜进入random-mask对照，检验FS是否仅对强future-dependent任务有效`

## 证据路径 (latest)

- `results/mnist_fg_stage0_v6_20260227T111807Z/gate.json`
- `results/mnist_fg_smoke_v6_20260227T111807Z/gate.json`
- `results/mnist_fg_coarse_v6_20260227T111807Z/gate.json`
- `results/mnist_fg_coarse_v6_20260227T111807Z/prune_table.csv`
- `results/mnist_fg_confirm_v6_20260227T111807Z/gate.json`
- `results/mnist_fg_confirm_v6_20260227T111807Z/prune_table.csv`

补充报告:
- `results/mnist_fg_success_report_v6_left_right.md`
- `results/mnist_fg_success_report_v5.md`
- `results/decision_report.txt`

## 历史轮次

- Cycle-5 (`v5_20260227T025027Z`): 自然任务 confirm pass。
- Cycle-4 (`v4_20260227T023816Z`): row-major+prefix Stage0 fail。
- Cycle-3 (`v3_20260226T134028Z`): binary+square，coarse fail。
- Cycle-2 (`v2_20260226T124341Z`): Stage0 fail。
- Cycle-1: smoke pass, coarse fail。
