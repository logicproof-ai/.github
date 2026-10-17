<div align="center">

# LogicProof AI

### **Mathematical Business Logic & Vulnerability Verification Engine**
*Zero Guesses. Zero False Positives. Zero Code Retention.*

[![Engine](https://img.shields.io/badge/Engine-Microsoft_Z3_SMT_Solver-007acc.svg?style=flat-square)](https://github.com/Z3Prover/z3)
[![CI/CD](https://img.shields.io/badge/GitHub_Action-v1-2088FF.svg?style=flat-square&logo=githubactions&logoColor=white)](https://github.com/logicproof-ai/logicproof-action)
[![False Positives](https://img.shields.io/badge/False_Positives-0.0%25_on_Production_Corpora-success.svg?style=flat-square)](#-measured-precision-zero-false-positive-guarantee)
[![Architecture](https://img.shields.io/badge/Security-Zero_Code_Retention-brightgreen.svg?style=flat-square)](#-zero-retention-security-architecture)
[![Languages](https://img.shields.io/badge/Languages-TS_%7C_JS_%7C_Python_%7C_Go_%7C_Java_%7C_C%23-orange.svg?style=flat-square)](#-multi-language-coverage)
[![Website](https://img.shields.io/badge/Website-logicproofai.com-blueviolet.svg?style=flat-square)](https://logicproofai.com)

<p align="center">
  <a href="#-why-logicproof-ai">Why LogicProof</a> •
  <a href="#-the-9-vulnerability-classes">Vulnerability Classes</a> •
  <a href="#-business-logic-formal-verification">Business Invariants</a> •
  <a href="#-auto-poc-instant-reproduction">Auto-PoC Engine</a> •
  <a href="#-zero-retention-security-architecture">Zero-Retention Guarantee</a> •
  <a href="#-quickstart-in-30-seconds">Quickstart</a> •
  <a href="#-enterprise--contact">Enterprise</a>
</p>

---

</div>

## 🛡️ Executive Summary

**LogicProof AI is not another heuristic pattern matcher or linter.**

Traditional SAST scanners bombard engineering teams with endless speculative warnings (60% to 80% false positive rates), causing alert fatigue and friction. LogicProof AI replaces pattern guesswork with **automated theorem proving**:

1. **Translates execution paths into mathematical formulas in First-Order Logic.**
2. **Queries the Z3 SMT solver** (developed by Microsoft Research) to determine if any combination of user input can lead to an invariant violation or security breach.
3. **If a flaw is reported, it is mathematically proven to be exploitable**, accompanied by the exact counterexample parameters that trigger it and a ready-to-run **Auto-PoC** reproducer.
4. **If no violation can be mathematically constructed, the scanner stays silent.**

```
                     Traditional SAST (Heuristics)
  Source Code ──────▶ Regex / AST Pattern Matching ──────▶ "Suspicious code detected"
                                                           (60-80% False Positives)

                     LogicProof AI (SMT Formal Verification)
  Source Code ──────▶ Path & Taint Extraction ──────▶ Z3 SMT Solver ──────▶ Exact Counterexample & Auto-PoC
                                                                            (Mathematically Provable)
```

---

## ⚡ The 9 Vulnerability Classes (Taint & Symbolic Flow)

LogicProof AI tracks untrusted user inputs end-to-end across functions, files, modules, and database queries. It verifies not just that validation code exists, but that it is mathematically complete across every possible execution branch:

| # | Vulnerability Class | Mechanism & Coverage | Verified Frameworks / ORMs |
|:---:|:---|:---|:---|
| **1** | **SQL Injection (`sql`)** | Detects tainted parameters reaching query sinks without parameterized binding or strict sanitization. | Prisma (`$queryRawUnsafe`), TypeORM, Drizzle, Django ORM, Raw Knex, psycopg2, pg |
| **2** | **Server-Side Request Forgery (`ssrf`)** | Detects external inputs driving outbound HTTP/network requests to internal IP spaces (AWS metadata `169.254.169.254`, localhost, private subnets). | `fetch`, `axios`, `got`, Python `requests`, `urllib3`, `aiohttp` |
| **3** | **Command Injection / RCE (`comando`)** | Detects user input reaching shell interpreters without strict whitelist separation. | `child_process.exec`, `spawn(shell=True)`, Python `os.system`, `subprocess.Popen` |
| **4** | **Path Traversal (`ruta`)** | Detects path manipulation escaping application boundaries (`../`). Recognizes anchored path resolution. | `fs.readFile`, `path.resolve`, Python `open`, `os.path.join`, `pathlib.Path` |
| **5** | **IDOR / BOLA (`idor`)** | Cross-file object reference verification. Distinguishes raw request IDs from authenticated credential lookups and enforces ownership gates. | Prisma, TypeORM, Drizzle ORM, multi-tenant workspace/organization scoping |
| **6** | **Cross-Site Scripting (`xss`)** | Tracks untrusted input directly into HTML response rendering (SSR, template engines) lacking contextual escaping. | Express `res.send(html)`, Python `render_template_string`, `mark_safe`, Django `HttpResponse` |
| **7** | **Mass Assignment (`massassign`)** | Flags unvalidated raw request objects dumped into database writes (`Model.create(**req.body)`). Suppressed by schema DTOs / `pick` / `omit`. | Prisma `create/update({data})`, TypeORM `save(obj)`, Drizzle, Django `Model(**data)` |
| **8** | **Insecure Deserialization (`deserial`)** | Detects arbitrary object reconstruction without cryptographic authentication or safe loaders. Safe parsers (`json.loads`, `yaml.safe_load`) are clean by design. | Python `pickle.loads`, `dill`, `yaml.unsafe_load`, Node `node-serialize` |
| **9** | **Code Evaluation (`eval`)** | Detects runtime string evaluation receiving tainted input. | `eval()`, `exec()`, `Function()`, `vm.runInContext()` |

---

## 🎯 Business Logic Formal Verification

Security is not just about OWASP top 10; the most damaging enterprise breaches exploit **broken business rules** that traditional SAST cannot see:
- E-commerce orders fulfilled without verified payment confirmations.
- Digital wallets ending up with negative balances via race conditions or rounding errors.
- Privilege escalation via manipulated subscription tier attributes.

### Declarative Business Invariants (`logicproof.yaml`)
Define your domain invariants once in your repository:

```yaml
# logicproof.yaml
reglas:
  - regla: No order is dispatched without confirmed payment
    siempre: "@dispatched == true implies @payment_confirmed == true"

  - regla: Account balance can never drop below zero
    nunca: "@balance < 0"

  - regla: Promotional discount must never exceed the subtotal
    siempre: "@discount <= @subtotal"
```

The `@` token models a domain **concept**, matching variables like `saldo`, `balance`, `account.balance`, or `wallet_balance` across the entire codebase.

### Certified Auto-Patching
When LogicProof finds a breach, it doesn't just print a warning:
1. It synthesizes a candidate code patch.
2. It **re-attacks the patched code** through the SMT solver.
3. It only presents the patch if the solver mathematically certifies that the breach is completely closed without side effects.

---

## 🔬 Auto-PoC: Instant Responsible Reproduction

For critical security findings, LogicProof AI automatically generates a **responsible reproduction kit**:
- An exact `curl` command with local placeholder parameters (`localhost:3000`).
- A self-contained, runnable reproduction script (`poc.py` / `poc.js`) demonstrating the exploit chain safely.

No more days lost arguing between security and development teams over whether a finding is theoretical or real.

```bash
# Generated Auto-PoC snippet (High Confidence Critical IDOR)
curl -X POST http://localhost:3000/api/documents/delete \
  -H "Authorization: Bearer <VICTIM_SESSION>" \
  -H "Content-Type: application/json" \
  -d '{"documentId": "foreign_resource_uuid"}'
```

---

## 🔒 Zero-Retention Security Architecture

> **Your source code NEVER leaves your infrastructure.**

```
  ┌──────────────────────────────────────────────────────────────┐
  │                 CUSTOMER INFRASTRUCTURE                      │
  │  (Self-Hosted Runner / GitHub Actions Runner / Dev Machine)  │
  │                                                              │
  │   Source Code ──────▶ [ LogicProof Native Binary ]           │
  │                                │                             │
  │                       (Compiled Machine Code                 │
  │                        with embedded Z3 SMT)                 │
  │                                │                             │
  │                                ▼                             │
  │                      SARIF / PR Comments                     │
  └───────────────────────────────┬──────────────────────────────┘
                                  │ License check ONLY
                                  │ (API Key hash, no code)
                                  ▼
                    ┌───────────────────────────┐
                    │  LogicProof License Server │
                    └───────────────────────────┘
```

- **Standalone Native Binary:** Pre-compiled machine code (C/C++ runtime with embedded Z3 engine). No Python runtime, no Docker daemon, no third-party cloud processing.
- **Strict Privacy Guarantee:** Only the licensing handshake travels over the network (API Key hash and timestamp). **Zero AST nodes, zero file names, zero code lines, and zero telemetry data ever leave your machine.**
- **Compliance Ready:** Designed specifically for regulated industries (Fintech, Healthtech, Banking, Defense) governed by SOC2, ISO 27001, HIPAA, and GDPR data residency requirements.

---

## 📊 Measured Precision: Zero False Positive Guarantee

We measure our engine continuously against massive real-world open-source production applications:

| Target Open-Source Repository | Scope | SMT Analysis Result | False Positives |
|:---|:---|:---:|:---:|
| **Twenty CRM** (`twentyhq/twenty`) | 1,767 Files (GraphQL, NestJS, TypeORM) | Clean | **0** |
| **Cal.com** (`calcom/cal.com`) | Full API / Trpc / Prisma routes | Audited Clean | **0** |
| **Directus** (`directus/directus`) | Core Knex / Service layers | Audited Clean | **0** |
| **Formbricks** (`formbricks/formbricks`) | Next.js App Router / Server Actions | Audited Clean | **0** |
| **Saleor** (`saleor/saleor`) | 940+ Graphene / Django endpoints | Audited Clean | **0** |
| **Synthetic Formal Benchmark** | Full test battery across 9 vectors | 33 / 33 Caught | **0** |
| **Full Regression Suite** | Continuous integration tests | **3,289 Passed** | **0 Regressions** |

---

## 💻 Multi-Language Coverage

LogicProof AI provides unified formal verification across the modern enterprise stack:

| Language | Parsers & Frontends | Frameworks Supported |
|:---|:---|:---|
| **TypeScript & JavaScript** | Official TypeScript AST Compiler, Node.js AST | Express, NestJS, Next.js (App & Pages Router), Fastify, Koa, TRPC, GraphQL |
| **Python** | Python 3.10-3.13 Native AST, Type Hint Inference | FastAPI, Django, Django REST Framework (DRF), Flask, Celery, Graphene |
| **Go** | Tree-Sitter High-Performance Grammar | `net/http`, Gin, Fiber, Echo, `database/sql` |
| **Java** | High-performance Tree-Sitter AST Engine | Spring Boot, Jakarta EE, Hibernate, JPA |
| **C#** | High-performance Tree-Sitter AST Engine | ASP.NET Core, Entity Framework Core |

---

## 🚀 Quickstart in 30 Seconds

### Option A: GitHub Actions (Recommended for CI/CD)

Add LogicProof to your repository in 5 lines of YAML:

```yaml
# .github/workflows/logicproof.yml
name: LogicProof AI
on: [pull_request]

jobs:
  verify:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
      security-events: write # Enables native GitHub Code Scanning SARIF tab
    steps:
      - uses: actions/checkout@v4
      - uses: logicproof-ai/logicproof-action@v1
        with:
          api-key: ${{ secrets.LOGICPROOF_API_KEY }}
          sarif: results.sarif
```

### Option B: Local CLI & Pre-Push Hook (For Developers)

```bash
# 1. Download native binary (Linux x86_64, macOS arm64/Intel, Windows x64)
curl -fsSL -o logicproof https://logicproofai.com/descargas/logicproof-linux-x64
chmod +x logicproof

# 2. Activate your license
./logicproof --activar <YOUR_API_KEY>

# 3. Analyze current repository
./logicproof .

# 4. Install pre-push git hook (prevent vulnerabilities before they leave your laptop)
./logicproof --instalar-hook
```

---

## 📦 Pinned Repositories & Ecosystem

- [`logicproof-action`](https://github.com/logicproof-ai/logicproof-action): Official composite GitHub Action for continuous PR verification.
- [`logicproof-cli`](https://github.com/logicproof-ai/logicproof-cli): Distribution hub and release repository for standalone native binaries and checksums.

---

## 🤝 Enterprise & Contact

- 🌐 **Website:** [logicproofai.com](https://logicproofai.com)
- 🔑 **Customer Portal & API Keys:** [logicproofai.com/panel](https://logicproofai.com/panel)
- 💼 **Enterprise Pilots & Licensing:** [sales@logicproof.ai](mailto:sales@logicproof.ai)
- 🔒 **Security Advisory & Vulnerability Reports:** [security@logicproof.ai](mailto:security@logicproof.ai) (See [SECURITY.md](SECURITY.md))
- 💬 **Technical Support:** [soporte@logicproof.ai](mailto:soporte@logicproof.ai) (See [SUPPORT.md](SUPPORT.md))

<div align="center">
  <sub>© 2026 LogicProof AI. Mathematical verification and automated proof engine. All rights reserved.</sub>
</div>
