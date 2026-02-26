# FS Impainting Dossier

This folder consolidates the current implementation, experiment summary, and repository details for the Future-Seed image inpainting validation work.

## Included documents

- `CURRENT_STATUS.md`: end-to-end status and gate outcomes.
- `REPO_DETAILS.md`: repository structure, key files, and code changes.
- `ARTIFACT_INDEX.md`: where all logs, manifests, and result tables are.

## Final decision snapshot

- Latest cycle uses `Stage0 Learnability Gate` first.
- Stage0 failed (`best_maskacc_fg = 0.0000 < 0.0200`), so smoke/coarse/confirm were skipped.
- FG diagnostics show `zero_fg_batches=0`, so failure is not caused by empty foreground batches.

See `CURRENT_STATUS.md` for full details.
