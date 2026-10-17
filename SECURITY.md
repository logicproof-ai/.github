# Security Policy — LogicProof AI

LogicProof AI takes the security of our platform, algorithms, binaries, and customer integrations with the highest degree of diligence and mathematical rigor.

## Zero Code Retention Pledge

LogicProof AI is designed from the ground up on the principle of **Zero Code Retention**:
- The LogicProof analysis engine runs **locally** on your execution environment (CI runner or developer workstation).
- **No customer source code, abstract syntax trees (ASTs), file paths, or vulnerability finding details are ever transmitted over the network.**
- The only network traffic initiated by the engine is a lightweight cryptographic handshake to validate license activation.
- The licensing payload contains only an encrypted hash of your API key and timestamp.

---

## Supported Versions

We provide security updates and patches for the following versions of LogicProof distribution assets:

| Asset | Version | Supported |
|---|---|---|
| `logicproof-action` | `v1` (tag `v1`, `v1.x`) | :white_check_mark: |
| `logicproof-cli` | `v1.3.x` (Latest Release) | :white_check_mark: |
| `logicproof-cli` | `< v1.3.0` | :x: (Please upgrade to latest) |

---

## Reporting a Security Vulnerability

If you believe you have discovered a vulnerability in LogicProof AI (including our GitHub Action, CLI distribution binaries, license validation protocol, or web portal), we appreciate your responsible disclosure.

### How to Report

1. **Private GitHub Security Advisory (Preferred):**
   Use GitHub's private vulnerability reporting feature on our official repositories:
   - [Report in `logicproof-action`](https://github.com/logicproof-ai/logicproof-action/security/advisories/new)
   - [Report in `logicproof-cli`](https://github.com/logicproof-ai/logicproof-cli/security/advisories/new)

2. **Encrypted Email:**
   If you prefer email or have a finding concerning infrastructure:
   - **Email:** [security@logicproof.ai](mailto:security@logicproof.ai)
   - **Subject Line:** `[SECURITY] Potential vulnerability in <component>`

Please include in your report:
- A clear description of the vulnerability and potential impact.
- Step-by-step reproduction instructions or a minimal Proof of Concept (PoC).
- Affected versions, operating system, and architecture.
- Any suggested mitigations or patches.

---

## Our Security Commitments & SLA

- **Acknowledgment:** We commit to acknowledging receipt of your vulnerability submission within **24 hours**.
- **Initial Triage:** We will assess and confirm reproducibility within **48 hours**.
- **Remediation & Updates:** We will keep you regularly informed as we develop, test, and release a fix.
- **Credit & Attribution:** With your consent, we will publicly acknowledge your contribution in our release notes and Hall of Fame.

---

## Safe Harbor Policy

LogicProof AI supports ethical security research conducted in good faith. If you conduct your research in compliance with this policy:
- We will not pursue legal action against you.
- We will work collaboratively with you to understand and resolve the issue quickly.
- We will not request law enforcement intervention for authorized, responsible research.

We ask that you:
- Do not exploit the vulnerability beyond what is strictly necessary to prove its existence.
- Do not access, destroy, or modify customer data.
- Do not perform Denial of Service (DoS) attacks or degrade system availability.
- Allow us a reasonable timeframe (standard 90 days or mutually agreed) to resolve the issue before public disclosure.

---

## Security Inquiries

For general security questions, compliance inquiries (SOC2, ISO 27001), or vendor security questionnaires:
- **Email:** [security@logicproof.ai](mailto:security@logicproof.ai)
