# Current Status (梳理与总结)

## 目标

Validate whether `FUTURE_SEED=1` provides stable gain for image inpainting under a strict staged protocol:

- `smoke -> coarse -> confirm`
- primary metric: `maskacc_fg_val`
- coarse threshold: `+0.8pp` (`0.008`)
- confirm threshold: `+1.5pp` mean gain across 3 seeds

## 已完成实现

1. Added foreground metric support in model script:
- `MASKACC_FG_EVAL` (default `0`)
- `FG_TOKEN_THRESHOLD` (default `0`)
- `maskacc_fg_{split}` logging and JSONL records

2. Kept backward compatibility:
- Existing `maskacc_{split}` behavior remains unchanged.

3. Executed staged runs on remote `connect.bjb2.seetacloud.com:19708` with fixed settings:
- dataset: `mnist14`
- `SEQ_LEN=196`
- `BIN_MASK_MODE=span`
- `BIN_SPAN_LEN=98`
- single 5090

## 阶段结果

## Stage A: smoke

- Gate: pass (metrics were produced for both FS0 and FS1).
- Delta: `avg_delta_best_fg = 0.000000`.
- Reference:
  - `results/mnist_fg_smoke/gate.json`
  - `results/mnist_fg_smoke/prune_table.csv`

## Stage B: coarse

- Round 1 (`alpha=-2`): failed threshold.
  - `delta_best_fg = 0.000819`
- Tangent round (`alpha=-1`): still below threshold.
- Gate: fail.
- Reference:
  - `results/mnist_fg_coarse/gate.json`
  - `results/mnist_fg_coarse/prune_table.csv`
  - `results/mnist_fg_coarse/summary_agg.json`

## Stage C: confirm

- Skipped by hard-stop rule due to coarse failure.
- Reference:
  - `results/mnist_fg_confirm/SKIPPED.txt`

## 决策结论

`当前配置数｜通过数｜最佳配置｜风险｜下一步`

`5｜0｜FS1(alpha=-2)在coarse仅+0.0819pp FG增益｜FG指标长期接近0，当前任务可学习性不足｜先提升可学习性门槛(FS0 best_maskacc_fg_val>=0.02)后再评估FS增益`

## 测试与验收

1. FG metric logic check:
- `results/fg_metric_logic_check.txt` (`PASS True`)

2. Regression compatibility (`MASKACC_FG_EVAL=0`):
- `results/mnist_fg_smoke/regression_check.log` only prints `maskacc_val`, no `maskacc_fg_val`.

## 硬停说明

- Hard stop report:
  - `results/mnist_fg_hard_stop_report.md`
