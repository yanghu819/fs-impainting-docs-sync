# Repo Details (repo详细)

## Source snapshot

- Downloaded source snapshot:
  - `payload/src/future-seed-main.tar.gz`
- Extracted workspace repo root:
  - `payload/src/future-seed-main/`
- Core experiment script:
  - `payload/src/future-seed-main/rwkv-diff-future-seed/rwkv_diff_future_seed.py`

## Repository layout (relevant parts)

Top level:
- `README.md`, `RESULTS.md`: project explanation and reported benchmark outcomes.
- `run.sh`, `run_qa.sh`: baseline task reproductions.
- `rwkv-diff-future-seed/`: main training/eval implementation.
- `tools/`: helper scripts (dataset bin build, summary helpers).
- `paper/`: PDF reports and latex sources.

Inside `rwkv-diff-future-seed/`:
- `rwkv_diff_future_seed.py`: all task switches, data loading, model, train/eval loops.
- `run_sudoku.sh`, `run_kvsort_*.sh`, `run_permfill_*.sh`: task-specific launch scripts.
- `logs/*.log`: existing sample logs from the upstream repo.

## Local code changes made

File changed:
- `payload/src/future-seed-main/rwkv-diff-future-seed/rwkv_diff_future_seed.py`

Key changes:
1. Added new env switches:
- `MASKACC_FG_EVAL`
- `FG_TOKEN_THRESHOLD`

2. Extended `maskacc_eval`:
- Added `fg_only` and `fg_token_threshold`.
- Foreground mask logic:
  - `eval_mask = mb & (yb > fg_token_threshold)`

3. Extended training eval logging:
- Prints and records:
  - `maskacc_{split}` (existing)
  - `maskacc_fg_{split}` (new when enabled)

4. Extended non-train eval path:
- Added `maskacc_fg_{split}` output when `MASKACC_FG_EVAL=1`.

5. Added run metadata fields:
- `future_seed_alpha_init`
- `maskacc_fg_eval`
- `fg_token_threshold`

## Utility scripts in this workspace

- `scripts/autodl_push_data.sh`
  - local data push to remote with manifest + sha.
- `scripts/autodl_build_and_push_whl.sh`
  - local wheel download/build + upload.
- `scripts/autodl_remote_install_whl.sh`
  - remote offline wheel install.

## Experiment document in this workspace

- `EXPERIMENT_MINIMAL.md`
  - contract, stage gate rules, and report template.
