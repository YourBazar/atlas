# Security Remediation Plan

1. Remove full environment logging from Centaurus.
2. Redact request body logging, especially passwords and tokens.
3. Stop returning request body in validation errors.
4. Configure explicit CORS origins per environment.
5. Move frontend auth toward httpOnly secure cookies or short-lived tokens with refresh strategy.
6. Ensure `.env` files are examples only unless intentionally local.
7. Add CI checks for API spec drift and dependency audits.
