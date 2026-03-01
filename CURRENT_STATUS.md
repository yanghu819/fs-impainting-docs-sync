# Current Status (梳理与总结)

## 目标

验证 `FUTURE_SEED=1` 是否在 image inpainting / infilling 任务中稳定提升前景恢复。

- 主指标: `maskacc_fg_val`
- coarse 门槛: `+0.8pp` (`0.008`)
- confirm 门槛: 三 seed 平均 `+1.5pp` (`0.015`) 且单 seed 不为负

## 最新主结论 (Cycle-9B / run_tag=v9b_20260301T031013Z)

在 `mnist14b_colmajor + square migration(random-source init, iters600)` 下：

1. Stage0: **PASS**
- `best_maskacc_fg=0.960901`

2. smoke: **PASS**
- `FS0` / `FS1(alpha=-2)` 指标链路完整
- `avg_delta_best_fg=+0.001117` (smoke 仅作链路验证)

3. coarse: **FAIL**
- `FS0 best_fg=0.967030`
- `FS1(alpha=-2) best_fg=0.971415`
- `delta_best_fg=+0.004385` (< 0.008)

4. confirm: **SKIPPED**
- reason: `coarse_gate_failed`

## 与前几轮成功任务的对比

- Cycle-5 (`v5`, natural prefix): mean delta `+0.275744`
- Cycle-6 (`v6`, left=right prefix 正控): mean delta `+0.087307`
- Cycle-7 (`v7`, random mask): mean delta `+0.029239`
- Cycle-8 (`v8`, square migration): mean delta `+0.018732`
- Cycle-9A (`v9a`, square migration + random-source init): mean delta `+0.009501` (confirm fail)
- Cycle-9B (`v9b`, square migration + random-source init + iters600): coarse delta `+0.004385` (coarse fail)

结论：在 random-source 设定下，仅增加训练步数到 600 未改善门槛达成，反而 coarse 即被剪枝；square 主线按协议硬停。

## 决策型汇报

`当前配置数｜通过数｜最佳配置｜风险｜下一步`

`9｜2(stage0+smoke)｜FS1(alpha=-2,square-migration+random-source-init,iters600)｜coarse增益仅+0.44pp(<+0.8pp)，确认阶段被剪枝｜主线硬停；回退到v8结论并转Plan C`

## 硬停输出（Cycle-9B）

- 结论：`coarse fail`，按协议直接剪枝并跳过 confirm。
- 证据：`results/mnist_fg_coarse_v9b_20260301T031013Z/gate.json`，`avg_delta_best_fg=0.0043849`。
- 教训：在 square + random-source 线中，增加训练步数不会带来有效增益，FS 优势已接近可观测下限。
- 下轮门槛：若继续 square 分支，需先引入新信息源（非仅增加步数）再重开 coarse；否则转回已过 confirm 的 `v8` 作为稳定结论。

## 证据路径 (latest)

- `results/mnist_fg_stage0_v9b_20260301T031013Z/gate.json`
- `results/mnist_fg_smoke_v9b_20260301T031013Z/gate.json`
- `results/mnist_fg_coarse_v9b_20260301T031013Z/gate.json`
- `results/mnist_fg_coarse_v9b_20260301T031013Z/prune_table.csv`
- `results/mnist_fg_confirm_v9b_20260301T031013Z/SKIPPED.txt`
- `results/run_manifest_v9b_20260301T031013Z.txt`

补充报告:
- `results/mnist_fg_hard_stop_report_v9b_square_migration_random_source_iters600.md`
- `results/mnist_fg_hard_stop_report_v9a_square_migration_random_source.md`
- `results/mnist_fg_success_report_v8_square_migration.md`
- `results/mnist_fg_success_report_v7_random.md`
- `results/mnist_fg_success_report_v6_left_right.md`
- `results/mnist_fg_success_report_v5.md`
- `results/decision_report.txt`

## 历史轮次

- Cycle-9B (`v9b_20260301T031013Z`): square migration(random-source,iters600) coarse fail。
- Cycle-9A (`v9a_20260301T015231Z`): square migration(random-source init) confirm fail。
- Cycle-8 (`v8_20260227T140325Z`): square migration confirm pass。
- Cycle-7 (`v7_20260227T123631Z`): random mask confirm pass。
- Cycle-6 (`v6_20260227T111807Z`): 正控 left=right confirm pass。
- Cycle-5 (`v5_20260227T025027Z`): 自然 prefix confirm pass。
- Cycle-4 (`v4_20260227T023816Z`): row-major+prefix Stage0 fail。
- Cycle-3 (`v3_20260226T134028Z`): binary+square coarse fail。
- Cycle-2 (`v2_20260226T124341Z`): Stage0 fail。
- Cycle-1: smoke pass, coarse fail。
