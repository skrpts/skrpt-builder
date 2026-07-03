# Release Notes

## v1.0.12
GH#745 — declare per-step `output: {name, type}` on every execution step (requirements/text, architecture_review/text, node_graph/text, nodes/text, validation_result/decision, review_feedback/text, assembled_skrpt/text). Lights up the #744 rich flow-map. Content-only; no bindings or logic changes.

## v1.0.11
GH#645 Row 3b — migrate to K-037 dep-referenced schema. Strip 5 inline shared-content files and declare 5 hub-shared deps (UUID id + slug name + version + checksum from `gen-dep-checksums.mjs`). Closes pre-Step-3 inline-vendoring for this bundle.

## v1.0.10
Wave 2: re-signed with canonical engine signing pipeline.

## v1.0.9
Tags migrated inline into manifest (GH#586). tags.yaml retired.

## v1.0.8
Bundle re-signed with canonical engine signing pipeline (Wave 2 migration).

## v1.0.7
Signature fix — RELEASE_NOTES.md now included in integrity checksum.

## v1.0.6
Validation checklist updated with current platform rules: loop completeness, analysis step type, local.* steps, British English.
