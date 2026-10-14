# FIREWATCH Web Security Agent v7.0.0

FIREWATCH v7 is a security-correctness and regression quality release. It adds large, deterministic adversarial matrices and fixes issues that those tests uncovered in URL scope enforcement, redaction, report rendering, header validation, API discovery and technology fingerprinting.

## Release highlights

- Expanded tests cover adversarial scope paths and ports, recursively encoded nested URL secrets, effective OpenAPI security declarations, URL-like API parameters, output-sink injection, CSV formula handling, exact-origin credential isolation, HTTP redirect behavior, crawler HTML/forms/JavaScript behavior, input schema limits, ZAP YAML serialization, read-only authorization review and benign GET canary limits.
- Security/correctness fixes: path wildcard exclusions no longer overmatch siblings; nested URL redaction decodes enough layers to recognize nested absolute URLs; Markdown reports escape raw HTML and structural Markdown characters; GraphQL classification uses path segments; URL-like OpenAPI parameter review is wider; whitespace-padded prohibited header names and port zero are rejected; PHP and Next.js technology markers are case-insensitive.
- Application and report version metadata is centralized and aligned to v7.0.0.
- Test manifest and matrix are generated from pytest collection and the explicit assessment plan remains separate from software regression tests.
- CI covers Python 3.11/3.12/3.13, a minimum 80% code-coverage gate, repository checks, frontend JavaScript, publisher shell scripts, Docker Compose YAML parsing, CodeQL, Bandit and dependency auditing.

## Verification limits

The local full regression run passed **2,763 / 2,763 tests across 41 modules**, with zero failures, errors or skips and **84.18% statement coverage**. The release verification record contains the full results. Tools or services unavailable locally—such as Docker Engine, live ZAP, remote CodeQL, or a hosted GitHub CI run—are marked not run rather than passed. Software test totals and statement coverage are not vulnerability detection rates, and passing tests do not establish that a scanned target is secure.
