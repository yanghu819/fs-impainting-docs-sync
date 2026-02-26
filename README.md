# FS Impainting Dossier

This repo is the experiment dossier / decision sync for validating Future-Seed on image inpainting.

## Included documents

- `CURRENT_STATUS.md`: current staged outcome and decision.
- `REPO_DETAILS.md`: upstream code location and local patch points.
- `ARTIFACT_INDEX.md`: where each stage artifact is stored.
- `REPO_TREE.txt`: tracked file tree snapshot.

## Latest snapshot (v3_20260226T134028Z)

- Stage0 (`FS0`) passed learnability gate: `best_maskacc_fg_val=0.940173`.
- smoke passed (metrics complete, `FS0` vs `FS1(alpha=-2)` both stable).
- coarse failed threshold (`>=0.008`): best FG delta stayed `0.000000`.
- one tangent round (`alpha -2 -> -1`) still no FG gain.
- confirm skipped by protocol (`coarse_gate_failed`).

See `CURRENT_STATUS.md` for full numbers and links.
