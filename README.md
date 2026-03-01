# FS Impainting Dossier

This repo is the experiment dossier / decision sync for validating Future-Seed on image inpainting.

## Included documents

- `CURRENT_STATUS.md`: staged outcome and decision status.
- `REPO_DETAILS.md`: upstream code location and patch points.
- `ARTIFACT_INDEX.md`: artifact index by cycle/stage.
- `REPO_TREE.txt`: tracked file tree snapshot.

## Latest snapshot (v9b_20260301T031013Z)

- task setting: `mnist14b_colmajor + square migration (random-source init)`
- Stage0: pass (`best_maskacc_fg=0.9609`)
- smoke: pass
- coarse: fail (`delta_best_fg=+0.00438` < `0.008`, `alpha=-2`)
- confirm: skipped (coarse gate failed)

Reference cycles:
- `v9a_20260301T015231Z` square migration (random-source init) coarse pass but confirm fail, mean `+0.00950`.
- `v8_20260227T140325Z` square migration (prefix-source init) confirm pass, mean `+0.01873`.
- `v7_20260227T123631Z` random-mask ablation confirm pass, mean `+0.02924`.
- `v6_20260227T111807Z` positive-control (`left=right + col-major + prefix`) confirm pass, mean `+0.0873`.
- `v5_20260227T025027Z` natural prefix task confirm pass, mean `+0.2757`.
