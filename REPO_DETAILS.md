# Repo Details (repo详细)

## Source snapshot

- Source snapshot: `payload/src/future-seed-main.tar.gz`
- Extracted root: `payload/src/future-seed-main/`
- Core script: `payload/src/future-seed-main/rwkv-diff-future-seed/rwkv_diff_future_seed.py`

## Relevant upstream layout

Top-level:
- `README.md`, `RESULTS.md`
- `run.sh`, `run_qa.sh`
- `rwkv-diff-future-seed/`
- `tools/`
- `paper/`

Inside `rwkv-diff-future-seed/`:
- `rwkv_diff_future_seed.py` (data/mask/model/train/eval all-in-one)
- `run_*.sh` task launchers
- `logs/*.log` sample logs

## Local patch points (code)

Changed file:
- `payload/src/future-seed-main/rwkv-diff-future-seed/rwkv_diff_future_seed.py`

### 1) FG metric switch & compatibility

- Added env:
  - `MASKACC_FG_EVAL` (default `0`)
  - `FG_TOKEN_THRESHOLD` (default `0`)
- Keeps original `maskacc_val` unchanged when FG eval is off.

### 2) `maskacc_eval` FG-only path + de-bias

- Added `fg_only` + `fg_token_threshold` route:
  - `eval_mask = mb & (yb > fg_token_threshold)`
- Added `return_stats` option.
- For FG-only eval, batches with `fg_masked_cnt==0` are excluded from FG average denominator.

### 3) Structured diagnostics to log / jsonl

Printed stats:
- `maskacc_fg_stats_{split} fg_masked_cnt=... masked_cnt=... fg_cnt=... zero_fg_batches=... used_batches=... total_batches=...`

JSONL fields:
- `maskacc_fg_{split}`
- `fg_masked_cnt_{split}`
- `masked_cnt_{split}`
- `fg_cnt_{split}`
- `zero_fg_batches_{split}`
- `used_batches_{split}`
- `total_batches_{split}`

### 4) Square mask support

Added env support:
- `BIN_MASK_MODE=square`
- `BIN_SQUARE_SIZE`
- `BIN_SQUARE_TOP`, `BIN_SQUARE_LEFT`

## Experiment-side setup evolution

- Cycle-4: row-major + prefix(0.5) -> Stage0 fail.
- Cycle-5: `mnist14b_colmajor_bin` + prefix(0.5) -> confirm pass.
- Cycle-6 (positive control): `mnist14b_left_eq_right_colmajor_bin` + prefix(0.5) -> confirm pass.

Datasets added in workspace:
- `payload/data/mnist14b_colmajor_bin/*`
- `payload/data/mnist14b_left_eq_right_colmajor_bin/*`

## Workspace utility scripts

- `scripts/autodl_push_data.sh`
- `scripts/autodl_build_and_push_whl.sh`
- `scripts/autodl_remote_install_whl.sh`

## Experiment protocol doc

- `EXPERIMENT_MINIMAL.md`
