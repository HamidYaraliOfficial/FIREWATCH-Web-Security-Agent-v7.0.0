# Testing Guide — FIREWATCH v7.0.0

## Automated regression suite

The v7 suite contains **2,763 collected pytest cases across 41 test modules**. The local full-suite run passed all 2,763 with zero failures, errors, or skips. `pytest-cov` measured **84.18% statement coverage** for `backend/app`, above the enforced 80% floor. Test totals and statement coverage measure the software under test; they are not a percentage of vulnerabilities discovered and do not establish that any target is secure.

Install development dependencies and reproduce the test/coverage gate:

```bash
python -m pip install -r backend/requirements-dev.txt
python scripts/build_test_manifest.py
python scripts/export_assessment_plan.py
cd backend
PYTHONPATH=. python -m pytest tests -q --junitxml=../test-results.xml --cov=app --cov-report=term-missing --cov-report=xml:../coverage.xml --cov-fail-under=80
```

To run all required local release checks (Python compilation, JavaScript and shell syntax, Compose YAML parsing, repository hygiene, regenerated assessment/test catalogs, the full suite, and coverage):

```bash
python scripts/verify_release.py
```

Optional Bandit, `pip-audit`, and Docker checks are run only when their executables are available. The release script prints explicit skips; a skip must not be described as a successful check.

## Generated test inventory

[`TEST_MATRIX_V7.md`](TEST_MATRIX_V7.md) lists all 2,763 collected cases and the intent of each of the 41 modules. [`TEST_MANIFEST_V7.json`](TEST_MANIFEST_V7.json) is the machine-readable inventory. The v7-specific suites add adversarial scope/path inputs, recursive secret redaction, OpenAPI effective security, report output-sink handling, exact-origin credential isolation, crawler inventory, ZAP plan serialization, schema boundary/fuzz matrices, read-only authorization comparisons, and low-impact parameter probe policies. The suite also retains API-contract, worker, unit, rule, config, report, and local demo integration regressions.

## Separate target assessment plan

The **79-case assessment catalog** is separate from software regression tests: 22 automated passive observations, 13 automated review candidates requiring validation, 38 manual/test-account/workflow cases, and 6 specialist-tool-required checks. Regenerate JSON, CSV and Markdown with:

```bash
python scripts/export_assessment_plan.py
```

The API serves this plan at `GET /api/v1/assessment-plan`. Manual or specialist-tool cases are not automatically executed by the built-in safe scanner.

## Static checks and CI

CI tests Python 3.11, 3.12 and 3.13, enforces at least 80% statement coverage, validates regenerated catalogs and manifests, checks dashboard JavaScript and publisher shell syntax, parses Docker Compose YAML, runs repository hygiene, and configures Bandit and dependency auditing. CodeQL runs in a separate workflow. Hosted results should be considered verified only after the real GitHub Actions jobs complete.

## Manual browser and deployment validation

Real browser discovery requires Playwright's Chromium binary. Docker image building, PostgreSQL production concurrency/load, externally deployed workers, live ZAP automation, GitHub-hosted CI/CodeQL and publishing to an owner's GitHub account are separate checks; they are not implied by passing the local suite. Confirm credentials never cross origins, forms are not submitted by default, downloads are not accepted automatically, out-of-scope redirects are not followed, and sensitive material never reaches evidence/report sinks.

## Demo interpretation

The included local demo target is intentionally weak to exercise detector paths. Finding counts can change as checks evolve; count alone is not proof of coverage. Tests should assert rule behavior, transport boundaries, evidence sanitization, report integrity and end-to-end pipeline completion.
