# Milestone 1 Acceptance Tests

Run:

```powershell
pytest
```

The tests regenerate the five public synthetic cases from `data/public_synthetic/raw_fhir/`, validate required files, check that the selected patients are the top five by the configured complexity score, enforce patient-scoped evidence/provenance/task references, and ensure generated public artifacts do not reference MIMIC.

To regenerate without running the full test suite:

```powershell
python scripts/generate_public_synthetic_cases.py
```

To append exactly two additional cases without overwriting existing cases:

```powershell
python scripts/build_synthetic_cases.py --input data/public_synthetic/raw_fhir --output data/public_synthetic/cases --append --additional 2
```
