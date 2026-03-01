# Artifact Index

## Latest cycle (Cycle-9B)

Run tag:
- `v9b_20260301T031013Z`

## Stage0 (`results/mnist_fg_stage0_v9b_20260301T031013Z/`)

- `summary_rows.csv`
- `summary_agg.json`
- `prune_table.csv`
- `gate.json`
- `fs0_seed20260226.log/jsonl`

## Smoke (`results/mnist_fg_smoke_v9b_20260301T031013Z/`)

- `summary_rows.csv`
- `summary_agg.json`
- `prune_table.csv`
- `gate.json`
- `fs0_seed20260226.log/jsonl`
- `fs1a-2_seed20260226.log/jsonl`

## Coarse (`results/mnist_fg_coarse_v9b_20260301T031013Z/`)

- `summary_rows.csv`
- `summary_agg.json`
- `prune_table.csv`
- `gate.json`
- `fs0_seed20260226_r1.log/jsonl`
- `fs1a-2_seed20260226_r1.log/jsonl`

## Confirm (`results/mnist_fg_confirm_v9b_20260301T031013Z/`)

- `SKIPPED.txt` (`reason=coarse_gate_failed`)

## Reports

- `results/mnist_fg_hard_stop_report_v9b_square_migration_random_source_iters600.md`
- `results/mnist_fg_hard_stop_report_v9a_square_migration_random_source.md`
- `results/mnist_fg_success_report_v8_square_migration.md`
- `results/mnist_fg_success_report_v7_random.md`
- `results/mnist_fg_success_report_v6_left_right.md`
- `results/mnist_fg_success_report_v5.md`
- `results/decision_report.txt`

## Previous cycles

- Cycle-9A: `v9a_20260301T015231Z` (random-source square migration, confirm fail)
- Cycle-8: `v8_20260227T140325Z` (square migration from prefix source, confirm pass)
- Cycle-7: `v7_20260227T123631Z` (random mask ablation, confirm pass)
- Cycle-6: `v6_20260227T111807Z` (left=right positive-control, confirm pass)
- Cycle-5: `v5_20260227T025027Z` (prefix natural task, confirm pass)
- Cycle-4: `v4_20260227T023816Z` (row-major prefix fail)
- Cycle-3/2/1: older references

## Data / manifests

- `payload/data/mnist14b_bin/`
- `payload/data/mnist14b_colmajor_bin/`
- `payload/data/mnist14b_left_eq_right_colmajor_bin/`
- `results/run_manifest_v9a_20260301T015231Z.txt`
- `results/run_manifest_v9b_20260301T031013Z.txt`
- `artifacts/manifests/run_manifest_data_*.txt`
- `artifacts/manifests/sha256_data_*.txt`
