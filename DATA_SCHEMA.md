# ChartGround Milestone 1 Data Schema

Milestone 1 creates a source-neutral case folder for each selected public synthetic patient under `data/public_synthetic/cases/<case_id>/`.

The append pipeline preserves existing case folders and can add new case folders such as `cg-syn-006/` and `cg-syn-007/` while keeping the same per-case file contract.

Required files per case:
- `raw_fhir_bundle.json`: exact copied Synthea source bundle.
- `overlays.json`: deterministic synthetic additions used only when the source is too thin for required note coverage.
- `patients.jsonl`: one patient row with `case_id`, `patient_id`, demographics, and source id.
- `encounters.jsonl`: source-neutral encounter rows.
- `events.jsonl`: clinical timeline rows derived from conditions, observations, medications, procedures, diagnostic reports, and selected related resources.
- `notes.jsonl`: deterministic synthetic clinical notes with explicit sentence ids.
- `evidence.jsonl`: indexed evidence rows with `evidence_id`, `source_id`, `source_kind`, source-neutral labels, dates, values, and patient scope.
- `provenance.jsonl`: one row per note sentence linking the sentence to an evidence row and source id.
- `tasks.jsonl`: 2-3 evaluation tasks per case with required evidence, event, and note references.
- `expected_behavior.json`: task-level expected behavior and grounding principles.
- `case_card.md`: human-readable case summary.

Important invariants:
- Every material note sentence has sentence-level provenance.
- Every provenance `source_id` exists in `evidence.jsonl`.
- Every evidence, event, note, provenance, and task row belongs to the case patient.
- Task references must resolve to existing evidence, event, and note ids.
- Public cases are generated only from Synthea bundles and local deterministic overlays; MIMIC and cloud APIs are not used.
