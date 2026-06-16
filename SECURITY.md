# Security Policy

## Supported Versions

Only the latest release of Keepra receives security fixes.

| Version | Supported |
|---------|-----------|
| 1.0.x   | Yes       |
| < 1.0   | No        |

## Reporting a Vulnerability

**Do not open a public GitHub issue for security vulnerabilities.**

Email **support@keepra.tech** with:

- A description of the vulnerability
- Steps to reproduce it
- The platform(s) affected (Web, Windows, Android)
- Your assessment of the impact

You will receive an acknowledgement within 72 hours and a status update within 7 days.

Once the vulnerability is confirmed and fixed, we will:

1. Release a patched version
2. Credit the reporter in the release notes (unless you prefer to remain anonymous)
3. Publish a security advisory on this repository

## Scope

In scope:
- Authentication and session handling
- Vault encryption (data at rest and in transit)
- Cloud sync (zero-knowledge E2E)
- MCP API key validation and scope enforcement
- XSS, injection, or privilege escalation in the web or desktop app

Out of scope:
- Social engineering attacks
- Bugs in third-party dependencies (report those upstream)
- Issues that require physical access to an unlocked device

## Encryption model

Keepra's Vault uses **AES-256-GCM** with keys derived via **PBKDF2 (600,000 iterations, SHA-256)**. The master password or Secret Key is never stored. It is derived into the encryption key in memory only. Firestore only ever receives encrypted ciphertext.
