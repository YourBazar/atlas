# Security Audit Context

## Existing Automation

Security audit workflows exist in root and submodules.

Checks include:

- npm production dependency audit for Cygnus.
- pip-audit for Centaurus.
- gitleaks secret scanning through Docker.

## Recent Remediation

- Cygnus production dependencies were updated and production audit reached zero vulnerabilities locally.
- Cygnus build and test passed locally after package updates.
- Centaurus vulnerable Python package pins were updated from the pip-audit report.
- gitleaks invocation was changed from a missing binary call to Docker-based execution.

## Remaining Security Work

- Remove or sanitize sensitive request body logging.
- Remove environment dumps from backend logs.
- Restrict CORS origins in production.
- Replace local storage token storage with a safer auth strategy.
- Rotate any credentials that were ever reused outside local development.
- Add security review gates before payment, document, and support workflows launch.
