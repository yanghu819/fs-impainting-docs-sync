# FS Impainting Dossier

This repo is the experiment dossier / decision sync for validating Future-Seed on image inpainting.

## Included documents

- `CURRENT_STATUS.md`: staged outcome and decision status.
- `REPO_DETAILS.md`: upstream code location and patch points.
- `ARTIFACT_INDEX.md`: artifact index by cycle/stage.
- `REPO_TREE.txt`: tracked file tree snapshot.

## Latest snapshot (v6_20260227T111807Z)

- task setting (positive control): `mnist14b_left_eq_right + col-major + prefix(0.5)`
- Stage0: pass (`best_maskacc_fg=0.7677`)
- smoke: pass
- coarse: pass (`delta_best_fg=+0.0385`, `alpha=-2`)
- confirm: pass (3-seed mean `+0.0873`, all seeds non-negative)

Reference successful natural-task cycle:
- `v5_20260227T025027Z` (`mnist14b + col-major + prefix`) confirm pass with mean `+0.2757`.
