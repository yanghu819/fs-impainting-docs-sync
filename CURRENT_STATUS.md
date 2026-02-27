# Current Status (梳理与总结)

## 目标

验证 `FUTURE_SEED=1` 是否在 image inpainting / infilling 任务中稳定提升前景恢复。

- 主指标: `maskacc_fg_val`
- coarse 门槛: `+0.8pp` (`0.008`)
- confirm 门槛: 三 seed 平均 `+1.5pp` (`0.015`) 且单 seed 不为负

## 最新主结论 (Cycle-7 / run_tag=v7_20260227T123631Z)

在 `mnist14b_colmajor + random mask` 下，Future-Seed 通过全流程：

1. Stage0: **PASS**
- `best_maskacc_fg=0.606053`

2. smoke: **PASS**
- `FS0` / `FS1(alpha=-2)` 指标链路完整
- `avg_delta_best_fg=0.0` (smoke 预算内未分化)

3. coarse: **PASS**
- `FS0 best_fg=0.606053`
- `FS1(alpha=-2) best_fg=0.664236`
- `delta_best_fg=+0.058183` (>= 0.008)

4. confirm: **PASS**
- seeds: `20260226, 20260227, 20260228`
- seed deltas: `[+0.058183, +0.013889, +0.015646]`
- mean delta: `+0.029239`
- all seeds non-negative: `true`

## 与前两轮成功任务的对比

- Cycle-5 (`v5`, natural prefix): mean delta `+0.275744`
- Cycle-6 (`v6`, left=right prefix 正控): mean delta `+0.087307`
- Cycle-7 (`v7`, random mask): mean delta `+0.029239`

结论：FS 在 random mask 仍有效，但增益显著小于 prefix 系列，符合“对强 future-dependent 任务更敏感”的边界判断。

## 决策型汇报

`当前配置数｜通过数｜最佳配置｜风险｜下一步`

`10｜4(stage0+smoke+coarse+confirm)｜FS1(alpha=-2,random-mask+col-major)｜增益较prefix任务显著减弱，部分seed接近门槛下沿｜保留prefix为主验证任务，继续做square迁移验证`

## 证据路径 (latest)

- `results/mnist_fg_stage0_v7_20260227T123631Z/gate.json`
- `results/mnist_fg_smoke_v7_20260227T123631Z/gate.json`
- `results/mnist_fg_coarse_v7_20260227T123631Z/gate.json`
- `results/mnist_fg_coarse_v7_20260227T123631Z/prune_table.csv`
- `results/mnist_fg_confirm_v7_20260227T123631Z/gate.json`
- `results/mnist_fg_confirm_v7_20260227T123631Z/prune_table.csv`

补充报告:
- `results/mnist_fg_success_report_v7_random.md`
- `results/mnist_fg_success_report_v6_left_right.md`
- `results/mnist_fg_success_report_v5.md`
- `results/decision_report.txt`

## 历史轮次

- Cycle-6 (`v6_20260227T111807Z`): 正控 left=right confirm pass。
- Cycle-5 (`v5_20260227T025027Z`): 自然 prefix confirm pass。
- Cycle-4 (`v4_20260227T023816Z`): row-major+prefix Stage0 fail。
- Cycle-3 (`v3_20260226T134028Z`): binary+square coarse fail。
- Cycle-2 (`v2_20260226T124341Z`): Stage0 fail。
- Cycle-1: smoke pass, coarse fail。
