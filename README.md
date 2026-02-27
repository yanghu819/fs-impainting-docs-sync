# FS Impainting Dossier

This repo is the experiment dossier / decision sync for validating Future-Seed on image inpainting.

## Included documents

- `CURRENT_STATUS.md`: staged outcome and final decision.
- `REPO_DETAILS.md`: upstream code location and local patch points.
- `ARTIFACT_INDEX.md`: stage artifact index.
- `REPO_TREE.txt`: tracked file tree snapshot.

## Latest snapshot (v5_20260227T025027Z)

- task setting: `mnist14b + col-major token order + prefix(0.5)`
- Stage0: pass (`best_maskacc_fg=0.3901`)
- smoke: pass
- coarse: pass (`delta_best_fg=+0.2905`, `alpha=-2`)
- confirm: pass (3-seed mean `+0.2757`, all seeds non-negative)

Previous cycle `v4_20260227T023816Z` (row-major prefix) failed at Stage0 (`best_maskacc_fg=0`).
