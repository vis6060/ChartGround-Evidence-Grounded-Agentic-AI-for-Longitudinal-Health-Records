ChartGround is an evidence-grounded conversational assistant for longitudinal chart review.

Intended use:
- Help a healthcare professional reconstruct a synthetic patient's longitudinal record.
- Answer what changed, what evidence supports a conclusion, and what remains missing or conflicting.

Not intended use:
- Diagnosis
- Treatment recommendation
- Patient-facing medical advice
- Autonomous clinical decision-making

Core requirements:
- Every material claim must cite evidence.
- Retrieval must be patient-scoped.
- Cross-patient evidence is prohibited.
- Missing or conflicting information must trigger uncertainty, escalation, or abstention.
- Public demo must use synthetic data only.