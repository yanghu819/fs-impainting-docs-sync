# FS Impainting Dossier

This repo is the experiment dossier / decision sync for validating Future-Seed on image inpainting.

## Included documents

- `CURRENT_STATUS.md`: staged outcome and decision status.
- `REPO_DETAILS.md`: upstream code location and patch points.
- `ARTIFACT_INDEX.md`: artifact index by cycle/stage.
- `REPO_TREE.txt`: tracked file tree snapshot.

## Latest snapshot (v7_20260227T123631Z)

- task setting: `mnist14b_colmajor + BIN_MASK_MODE=random`
- Stage0: pass (`best_maskacc_fg=0.6061`)
- smoke: pass
- coarse: pass (`delta_best_fg=+0.0582`, `alpha=-2`)
- confirm: pass (3-seed mean `+0.02924`, all seeds non-negative)

Reference cycles:
- `v6_20260227T111807Z` positive-control (`left=right + col-major + prefix`) confirm pass, mean `+0.0873`.
- `v5_20260227T025027Z` natural prefix task confirm pass, mean `+0.2757`.
