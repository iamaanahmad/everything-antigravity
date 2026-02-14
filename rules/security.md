# Antigravity Security Rules

**Mandatory security guidelines for all code generated or reviewed in Antigravity.**

## 1. Secrets Management
- **NEVER** hardcode secrets (API keys, passwords, tokens) in code.
- Use environment variables (e.g., `.env`) for all sensitive data.
- Ensure `.env` is added to `.gitignore`.
- Scan committed code for accidental secret leaks.

## 2. Input Validation
- Validate ALL user inputs on both client and server sides.
- Sanitize data before using it in SQL queries, shell commands, or HTML rendering.
- Use parameterized queries or ORMs to prevent SQL injection.
- Encode output to prevent Cross-Site Scripting (XSS).

## 3. Authentication & Authorization
- Implement robust authentication (e.g., OAuth, JWT).
- Enforce strict authorization checks (RBAC/ABAC) on every sensitive endpoint.
- Use secure session management (HttpOnly, Secure cookies).
- Implement rate limiting to prevent brute-force attacks.

## 4. dependency Management
- Regularly update dependencies to patch known vulnerabilities.
- Audit `package.json` (or equivalent) for insecure packages.
- Pin dependency versions to avoid unexpected breaking changes or malicious updates.

## 5. Data Protection
- Encrypt sensitive data at rest and in transit (HTTPS/TLS).
- minimize data collection (Privacy by Design).
- Implement proper logging for security audits (without logging sensitive data).

## 6. Error Handling
- Do not expose stack traces or internal system details to end-users.
- Log errors securely for debugging purposes.
- Fail securely (e.g., deny access by default).
