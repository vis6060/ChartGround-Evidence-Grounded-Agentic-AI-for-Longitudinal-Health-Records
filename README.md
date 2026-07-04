# ChartGround — Evidence-Grounded Agentic AI for Longitudinal Health Records

ChartGround is a healthcare AI product/evaluation prototype for longitudinal chart review. It compares **RAG**, **tool-using agents**, and a **verified-agent architecture** across synthetic FHIR records and a private aggregate-only MIMIC-IV stress test.

The goal is not to build a diagnosis or treatment tool. The goal is to test what a safer clinical AI workflow should include: **patient-scoped retrieval, deterministic clinical tools, evidence citations, claim verification, uncertainty handling, escalation/abstention behavior, prompt-injection testing, cross-patient leakage checks, and clinician-review export**.

---

## Why this project matters

Clinical chart review often requires reconstructing a patient story from scattered labs, medications, radiology reports, notes, encounters, procedures, and conflicting or missing information.

A generic RAG system can retrieve plausible text, but in healthcare that is not enough. A healthcare assistant needs to answer questions such as:

- Is this evidence from the correct patient?
- Which source supports this claim?
- Is the evidence complete enough to answer?
- Are there conflicting notes or medication lists?
- Should the system answer, express uncertainty, escalate, or abstain?
- Did a note contain prompt-injection text that should be treated as evidence, not instruction?

ChartGround explores those product and safety questions in a public-safe synthetic benchmark and a private aggregate-only MIMIC stress test.

---

## What makes ChartGround different from generic RAG

ChartGround compares three architectures:

| Architecture | Description |
|---|---|
| `rag` | Patient-scoped retrieval with grounded answer generation/fallback |
| `tool_agent` | Routes questions to deterministic clinical tools before answering |
| `verified_agent` | Adds citation validation, claim-support checking, missing/conflict detection, and answer/uncertain/escalate/abstain policy |

The verified agent is designed around a safer clinical workflow:

```text
Question
  -> trusted case/patient context
  -> patient-scoped evidence retrieval
  -> deterministic clinical tools
  -> answer draft
  -> claim extraction
  -> citation validation
  -> claim-support checking
  -> missing/conflict detection
  -> answer / uncertain / escalate / abstain
```

---

## Intended use

ChartGround is intended for:

- synthetic longitudinal chart-review experiments;
- evidence-grounding and citation-validation research;
- architecture comparison across RAG, tool-agent, and verified-agent workflows;
- uncertainty, escalation, and abstention behavior testing;
- public-safe clinician-review package generation;
- private aggregate-only stress testing on restricted datasets.

---

## Not intended use

ChartGround is **not** intended for:

- diagnosis;
- treatment recommendations;
- patient-facing medical advice;
- autonomous clinical decision-making;
- clinical deployment;
- medical-device use;
- replacement of clinician judgment.

This project is a prototype and evaluation framework, not a validated clinical product.

---

## Key capabilities

- Source-neutral canonical clinical schema
- Public synthetic FHIR benchmark
- Patient-scoped evidence retrieval
- Deterministic clinical tools for:
  - timelines
  - lab trends
  - medication review
  - radiology comparison
  - note search
  - conflict detection
- RAG baseline
- Tool-using agent
- Verified agent
- Citation validation
- Claim-support checking
- Missing-information detection
- Conflict/escalation behavior
- Prompt-injection tests
- Cross-patient leakage tests
- Clinician-review export package
- Private MIMIC-IV / MIMIC-IV-Note stress test with aggregate-only reporting

---

## Public benchmark results

The public benchmark uses synthetic data only.

| Metric | Value |
|---|---:|
| Public synthetic cases | 37 |
| Authored public tasks | 141 |
| Evaluated tasks per architecture, including runtime adversarial checks | 151 |
| Architectures compared | 3 |
| Clinician-review tasks | 30 |
| Blinded clinician-review output rows | 90 |
| Cross-patient violations | 0 |
| Task errors | 0 |

Latest public architecture scorecard:

| Architecture | Evaluated Tasks | Action Accuracy / Expected-Action Agreement | Citation Precision | Citation Completeness | Cross-Patient Violations | Task Error Rate |
|---|---:|---:|---:|---:|---:|---:|
| RAG | 151 | 0.079 | 0.094 | 0.311 | 0 | 0.0 |
| Tool Agent | 151 | 0.901 | 0.299 | 0.667 | 0 | 0.0 |
| Verified Agent | 151 | 1.000 | 0.299 | 0.667 | 0 | 0.0 |

**Important:** These are synthetic benchmark results with deterministic expected actions. They are useful for evaluating architecture behavior, but they do **not** establish clinical safety, efficacy, or real-world performance.

---

## Private MIMIC stress test

ChartGround includes a private local adapter for MIMIC-IV / MIMIC-IV-Note stress testing.

The private MIMIC stress test is used only to check whether the same architecture can run on real deidentified clinical-record structure and note complexity. Patient-level MIMIC data, generated private cases, raw notes, identifiers, and private evaluation rows are **not committed**.  This is to comlpy with data use agreement of PhysioNet (https://www.physionet.org/) - as data access is given after identity verification and this MIMIC data only under MIT license can be re-distribued.

Public-safe aggregate summary:

| Metric | Value |
|---|---:|
| Private pilot cases | 10 |
| Private tasks | 50 |
| Architecture-result rows | 150 |
| Architectures compared | 3 |
| Task errors | 0 |
| Cross-patient violations | 0 |
| Citation validity rate | 1.0 |
| Privacy leak scan | Passed |

The MIMIC stress test is a realism check, not clinical validation.

---

## Clinician-review package

ChartGround generates a blinded synthetic-data package for structured clinician feedback.

The package includes:

| File | Purpose |
|---|---|
| `clinician_review_blinded_outputs.csv` | Clinician-facing review file |
| `clinician_review_rubric.csv` | Scoring rubric |
| `clinician_review_instructions.md` | Reviewer instructions |
| `clinician_review_answer_key_private.csv` | Private architecture answer key; do not send to reviewers |

Current clinician-review export:

| Metric | Value |
|---|---:|
| Selected review tasks | 30 |
| Blinded output rows | 90 |
| Blank system actions | 0 |
| Weak/empty evidence contexts | 0 |

Clinicians score outputs for correctness, omissions, citation support, uncertainty handling, escalation appropriateness, clarity/usefulness, correction burden, and potential harm.

This is structured expert feedback, not clinical validation.

---

## Architecture overview

ChartGround uses a source-neutral canonical schema:

```text
Synthea FHIR / private MIMIC adapter
  -> ChartGround canonical case files
  -> evidence store
  -> retrieval + deterministic tools
  -> RAG / tool_agent / verified_agent
  -> evaluation + clinician-review export
```

Canonical case files:

```text
patients.jsonl
encounters.jsonl
events.jsonl
notes.jsonl
evidence.jsonl
provenance.jsonl
tasks.jsonl
expected_behavior.json
case_card.md
```

---

## Deterministic clinical tools

ChartGround intentionally avoids asking the LLM to calculate structured clinical facts when deterministic tools are available.

Implemented tools include:

- `build_patient_timeline`
- `get_lab_trend`
- `get_medication_changes`
- `compare_radiology_reports`
- `search_clinical_notes`
- `detect_record_conflicts`

These tools provide structured inputs to the tool-agent and verified-agent workflows.

---

## Reviewer quick path

For someone reviewing the project quickly:

1. Read this README.
2. Open:
   - `docs/product-brief.md`
   - `docs/architecture.md`
   - `docs/evaluation-summary.md`
   - `docs/safety-and-privacy.md`
   - `docs/openai-interview-story.md`
3. Run the public scorecard:
   ```powershell
   python scripts/print_scorecard.py --results results/public_eval_37
   ```
4. Inspect the clinician-review package:
   ```text
   results/clinician_review/
   ```
5. Review the private MIMIC aggregate-only summary:
   ```text
   results/mimic_private/public_safe_summary/
   ```

---

## Demo commands

Run a patient-scoped RAG demo:

```powershell
python scripts/rag_demo.py --case-id cg-syn-001 --question "What changed during this encounter?"
```

Run the verified-agent demo:

```powershell
python scripts/agent_demo.py --case-id cg-syn-001 --architecture verified_agent --question "What changed during this encounter?"
```

Compare architectures:

```powershell
python scripts/compare_architectures.py --case-id cg-syn-001 --question "What changed during this encounter?"
```

Run public evaluation:

```powershell
python scripts/run_public_evaluation.py --architectures rag tool_agent verified_agent --output results/public_eval_37
```

Print scorecard:

```powershell
python scripts/print_scorecard.py --results results/public_eval_37
```

Generate clinician-review package:

```powershell
python scripts/create_clinician_review_package.py --results results/public_eval_37 --cases data/public_synthetic/cases --output results/clinician_review --task-count 30 --seed 6060
```

Export portfolio summary:

```powershell
python scripts/export_portfolio_summary.py
```

Run privacy scan:

```powershell
python scripts/check_mimic_privacy_leaks.py --paths README.md docs results/portfolio_summary results/mimic_private/public_safe_summary
```

Run tests:

```powershell
python -m pytest -v
```

---

## Repository structure

```text
data/public_synthetic/       Public synthetic source data and generated cases
src/chartground/             Canonical loaders, evidence store, tools, agents, verification
scripts/                     Generation, demo, evaluation, export, and privacy-check scripts
tests/                       Pytest coverage for cases, retrieval, agents, evaluation, reporting
docs/                        Product, architecture, evaluation, safety, and portfolio docs
results/public_eval_37/      Public synthetic evaluation outputs
results/clinician_review/    Synthetic clinician-review package outputs
results/portfolio_summary/   Public-safe portfolio metrics
```

Private restricted-data outputs are excluded from Git:

```text
MIMICdata/
results/mimic_private/
*.duckdb
```

---

## Safety and privacy

ChartGround uses several safety controls:

- patient-scoped retrieval before ranking;
- internal patient ID enforcement;
- cross-patient evidence checks;
- prompt-injection handling;
- citation validation;
- claim-support checking;
- deterministic clinical tools;
- answer / uncertain / escalate / abstain policy;
- public synthetic benchmark;
- private MIMIC aggregate-only reporting;
- privacy leak scanner for public docs and summaries.

No raw MIMIC notes, patient identifiers, or patient-level MIMIC outputs should be committed or published.

---

## Limitations

- Synthetic benchmark performance does not establish clinical safety or efficacy.
- MIMIC stress-test task labels are heuristic and not clinician-validated.
- Clinician review package is prepared, but clinician-scored results may still need to be collected.
- Deterministic tools cover a limited set of chart-review workflows.
- Citation precision/completeness depend on gold evidence quality and task design.
- Local LLM support is optional; the project can run with deterministic fallbacks.
- This is not a medical device and should not be used for patient care.

---

## Future work

- Collect clinician-review scores from 1–3 clinicians.
- Expand deterministic clinical tools.
- Improve evidence compression and citation selection.
- Add more nuanced local model support while preserving privacy constraints.
- Expand private MIMIC stress testing under aggregate-only reporting.
- Add a lightweight Streamlit UI for demo purposes.
- Add richer longitudinal reasoning tasks and failure-mode dashboards.

---

## OpenAI relevance

ChartGround is designed around the product and safety questions that matter for healthcare AI:

- How should an assistant ground answers in patient-specific evidence?
- When should it avoid answering?
- How should it handle uncertainty and missing information?
- How do tool-using and verified-agent workflows compare to generic RAG?
- How can evaluation be structured before clinical deployment?
- How can restricted health data be used responsibly without publishing patient-level outputs?

The project demonstrates product judgment across healthcare AI workflow design, safety boundaries, evaluation methodology, clinician feedback preparation, and privacy-aware real-world stress testing.
