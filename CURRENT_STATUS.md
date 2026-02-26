# Current Status (梳理与总结)

## 目标

验证 `FUTURE_SEED=1` 是否在 image inpainting 设置下稳定提升前景恢复。

- 主指标: `maskacc_fg_val`
- coarse 门槛: `+0.8pp` (`0.008`)
- confirm 门槛: 三个 seed 平均 `+1.5pp` 且单 seed 不为负

## 最新结论 (Cycle-3 / run_tag=v3_20260226T134028Z)

本轮先通过 Stage0 学习性，再走 `smoke -> coarse -> confirm`：

1. Stage0: **PASS**
- gate: `best_maskacc_fg >= 0.02`
- result: `best_maskacc_fg=0.940173`

2. smoke: **PASS**
- `FS0` 与 `FS1(alpha=-2)` 都产出完整指标 (`maskacc_val` / `maskacc_fg_val` / FG stats)
- `avg_delta_best_fg=0.0`

3. coarse: **FAIL (剪枝)**
- baseline (`FS0`) best FG: `0.940173`
- candidate (`FS1, alpha=-2`) best FG: `0.940173`, `delta=0.000000`
- tangent round (`alpha=-1`) best FG: `0.940173`, `delta=0.000000`
- gate: `avg_delta_best_fg=0.0 < 0.008`

4. confirm: **SKIPPED**
- reason: `coarse_gate_failed`

## 固定设置与最小改动说明

本轮主线为“提升可学习性后再评 FS 增益”，关键冻结项：

- 数据: `mnist14b` (binary foreground)
- `SEQ_LEN=196`
- `BIN_MASK_MODE=square`, `BIN_SQUARE_SIZE=6`
- 单卡 `RTX 5090`
- 只改小变量: `FUTURE_SEED`, `FUTURE_SEED_ALPHA_INIT`

## 解读

- Stage0 已通过，说明任务可学习。
- 但在当前 binary + square 设定下，`FS1` 相对 `FS0` 未给出可测的 FG 增益（coarse 与切线都为 0）。
- 因 coarse 未达门槛，按协议不进入 confirm，多 seed 对比不再继续烧卡。

## 决策型汇报

`当前配置数｜通过数｜最佳配置｜风险｜下一步`

`6｜2(stage0+smoke)｜FS0/FS1 best_maskacc_fg 持平(0.940173)｜任务已可学但FS增益不可辨识，可能进入“饱和区”｜切到更具未来依赖的mask/任务，再重开coarse`

## 证据路径 (latest)

- `results/mnist_fg_stage0_v3_20260226T134028Z/gate.json`
- `results/mnist_fg_stage0_v3_20260226T134028Z/summary_agg.json`
- `results/mnist_fg_smoke_v3_20260226T134028Z/gate.json`
- `results/mnist_fg_smoke_v3_20260226T134028Z/summary_agg.json`
- `results/mnist_fg_coarse_v3_20260226T134028Z/gate.json`
- `results/mnist_fg_coarse_v3_20260226T134028Z/prune_table.csv`
- `results/mnist_fg_coarse_v3_20260226T134028Z/summary_agg.json`
- `results/mnist_fg_confirm_v3_20260226T134028Z/SKIPPED.txt`

## 历史轮次

- Cycle-2 (`v2_20260226T124341Z`): Stage0 fail，后续全跳过。
- Cycle-1 (no Stage0 gate): smoke 过、coarse 不达门槛、confirm 跳过。
