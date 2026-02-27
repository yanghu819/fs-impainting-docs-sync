# Current Status (梳理与总结)

## 目标

验证 `FUTURE_SEED=1` 是否在 image inpainting 设置下稳定提升前景恢复。

- 主指标: `maskacc_fg_val`
- coarse 门槛: `+0.8pp` (`0.008`)
- confirm 门槛: 三 seed 平均 `+1.5pp` (`0.015`) 且单 seed 不为负

## 最新主结论 (Cycle-5 / run_tag=v5_20260227T025027Z)

在 `mnist14b + col-major + prefix(0.5)` 下，Future-Seed 达到协议门槛：

1. Stage0: **PASS**
- gate: `best_maskacc_fg >= 0.02`
- result: `best_maskacc_fg=0.390065`

2. smoke: **PASS**
- `FS0` 与 `FS1(alpha=-2)` 指标链路完整

3. coarse: **PASS**
- `FS0 best_fg=0.390065`
- `FS1(alpha=-2) best_fg=0.680593`
- `delta_best_fg=+0.290528` (>= 0.008)

4. confirm: **PASS**
- seeds: `20260226, 20260227, 20260228`
- seed deltas: `[+0.290528, +0.259040, +0.277663]`
- mean delta: `+0.275744`
- all seeds non-negative: `true`

## 对照轮次

Cycle-4 (`v4_20260227T023816Z`): `row-major + prefix(0.5)`
- Stage0 fail (`best_maskacc_fg=0.0000`)
- smoke/coarse/confirm 全跳过

说明：仅改为 col-major 后，Future-Seed 增益被显著放大并稳定通过 confirm。

## 决策型汇报

`当前配置数｜通过数｜最佳配置｜风险｜下一步`

`10｜4(stage0+smoke+coarse+confirm)｜FS1(alpha=-2,prefix+col-major)｜前缀任务对训练阶段敏感(前期FG=0,后期抬升)｜进入正控(left=right)与random mask对照，验证机制边界与泛化`

## 证据路径 (latest)

- `results/mnist_fg_stage0_v5_20260227T025027Z/gate.json`
- `results/mnist_fg_smoke_v5_20260227T025027Z/gate.json`
- `results/mnist_fg_coarse_v5_20260227T025027Z/gate.json`
- `results/mnist_fg_coarse_v5_20260227T025027Z/prune_table.csv`
- `results/mnist_fg_confirm_v5_20260227T025027Z/gate.json`
- `results/mnist_fg_confirm_v5_20260227T025027Z/prune_table.csv`

补充报告:
- `results/mnist_fg_success_report_v5.md`
- `results/decision_report.txt`

## 历史轮次

- Cycle-4 (`v4_20260227T023816Z`): prefix(row-major) Stage0 fail。
- Cycle-3 (`v3_20260226T134028Z`): binary+square，Stage0/smoke pass，coarse fail。
- Cycle-2 (`v2_20260226T124341Z`): Stage0 fail。
- Cycle-1: smoke pass, coarse fail。
