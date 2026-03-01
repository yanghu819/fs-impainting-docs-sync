# Current Status (梳理与总结)

## 目标

验证 `FUTURE_SEED=1` 是否在 image inpainting / infilling 任务中稳定提升前景恢复。

- 主指标: `maskacc_fg_val`
- coarse 门槛: `+0.8pp` (`0.008`)
- confirm 门槛: 三 seed 平均 `+1.5pp` (`0.015`) 且单 seed 不为负

## 最新主结论 (Cycle-9A / run_tag=v9a_20260301T015231Z)

在 `mnist14b_colmajor + square migration(random-source init)` 下：

1. Stage0: **PASS**
- `best_maskacc_fg=0.960901`

2. smoke: **PASS**
- `FS0` / `FS1(alpha=-2)` 指标链路完整
- `avg_delta_best_fg=+0.001117` (smoke 仅作链路验证)

3. coarse: **PASS**
- `FS0 best_fg=0.960901`
- `FS1(alpha=-2) best_fg=0.970402`
- `delta_best_fg=+0.009501` (>= 0.008)

4. confirm: **FAIL**
- seeds: `20260226, 20260227, 20260228`
- seed deltas: `[+0.009501, +0.009501, +0.009501]`
- mean delta: `+0.009501`
- all seeds non-negative: `true`
- fail reason: `mean_delta_best_fg < 0.015`

## 与前几轮成功任务的对比

- Cycle-5 (`v5`, natural prefix): mean delta `+0.275744`
- Cycle-6 (`v6`, left=right prefix 正控): mean delta `+0.087307`
- Cycle-7 (`v7`, random mask): mean delta `+0.029239`
- Cycle-8 (`v8`, square migration): mean delta `+0.018732`
- Cycle-9A (`v9a`, square migration + random-source init): mean delta `+0.009501` (confirm fail)

结论：仅更换迁移源到 random 后，FS 增益进一步收敛；虽然 coarse 仍过线，但 confirm 门槛未达，主线按协议硬停。

## 决策型汇报

`当前配置数｜通过数｜最佳配置｜风险｜下一步`

`9｜3(stage0+smoke+coarse)｜FS1(alpha=-2,square-migration+random-source-init)｜confirm均值仅+0.95pp(<+1.5pp)，主线门槛未达｜执行v9-B：仅增加迁移训练步数(MAX_ITERS 300->600)，其余冻结`

## 硬停输出（Cycle-9A）

- 结论：`confirm fail`，本轮不进入“有效增益”结论。
- 证据：`results/mnist_fg_confirm_v9a_20260301T015231Z/gate.json`，`mean_delta_best_fg=0.0095006`。
- 教训：square 任务在高基线区间（FS0≈0.9609）下，FS 增益上限偏小；仅换迁移源不足以抬到 confirm 门槛。
- 下轮门槛：`v9-B` 只改 `MAX_ITERS=600`，其余冻结；coarse 需 `>=0.008` 且 confirm 均值需 `>=0.015`。

## 证据路径 (latest)

- `results/mnist_fg_stage0_v9a_20260301T015231Z/gate.json`
- `results/mnist_fg_smoke_v9a_20260301T015231Z/gate.json`
- `results/mnist_fg_coarse_v9a_20260301T015231Z/gate.json`
- `results/mnist_fg_coarse_v9a_20260301T015231Z/prune_table.csv`
- `results/mnist_fg_confirm_v9a_20260301T015231Z/gate.json`
- `results/mnist_fg_confirm_v9a_20260301T015231Z/prune_table.csv`
- `results/run_manifest_v9a_20260301T015231Z.txt`

补充报告:
- `results/mnist_fg_hard_stop_report_v9a_square_migration_random_source.md`
- `results/mnist_fg_success_report_v8_square_migration.md`
- `results/mnist_fg_success_report_v7_random.md`
- `results/mnist_fg_success_report_v6_left_right.md`
- `results/mnist_fg_success_report_v5.md`
- `results/decision_report.txt`

## 历史轮次

- Cycle-9A (`v9a_20260301T015231Z`): square migration(random-source init) confirm fail。
- Cycle-8 (`v8_20260227T140325Z`): square migration confirm pass。
- Cycle-7 (`v7_20260227T123631Z`): random mask confirm pass。
- Cycle-6 (`v6_20260227T111807Z`): 正控 left=right confirm pass。
- Cycle-5 (`v5_20260227T025027Z`): 自然 prefix confirm pass。
- Cycle-4 (`v4_20260227T023816Z`): row-major+prefix Stage0 fail。
- Cycle-3 (`v3_20260226T134028Z`): binary+square coarse fail。
- Cycle-2 (`v2_20260226T124341Z`): Stage0 fail。
- Cycle-1: smoke pass, coarse fail。
