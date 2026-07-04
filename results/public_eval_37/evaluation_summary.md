# ChartGround Public Evaluation Summary

- Cases evaluated: 37 (cg-syn-001, cg-syn-002, cg-syn-003, cg-syn-004, cg-syn-005, cg-syn-006, cg-syn-007, cg-syn-008, cg-syn-009, cg-syn-010, cg-syn-011, cg-syn-012, cg-syn-013, cg-syn-014, cg-syn-015, cg-syn-016, cg-syn-017, cg-syn-018, cg-syn-019, cg-syn-020, cg-syn-021, cg-syn-022, cg-syn-023, cg-syn-024, cg-syn-025, cg-syn-026, cg-syn-027, cg-syn-028, cg-syn-029, cg-syn-030, cg-syn-031, cg-syn-032, cg-syn-033, cg-syn-034, cg-syn-035, cg-syn-036, cg-syn-037)
- Unique tasks: 151
- Result rows: 453

| Architecture | Action Accuracy | Citation Precision | Citation Completeness | Cross-Patient Violations | Task Error Rate |
| --- | ---: | ---: | ---: | ---: | ---: |
| rag | 0.079 | 0.094 | 0.311 | 0 | 0.000 |
| tool_agent | 0.901 | 0.299 | 0.667 | 0 | 0.000 |
| verified_agent | 1.000 | 0.299 | 0.667 | 0 | 0.000 |

## Current Recommendation

Use `verified_agent` for guarded demos where citation validation and abstention behavior matter most.

## Limitations

- Public synthetic cases are not clinically validated.
- Deterministic adversarial tasks are lightweight smoke tests, not a complete safety benchmark.
- Local LLM use is optional; no cloud APIs are required or called.
- Claim metrics are marked not_applicable for architectures that do not emit claims.
