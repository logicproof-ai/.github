# Support Guidelines — LogicProof AI

Welcome to LogicProof AI support. We are dedicated to providing enterprise engineering teams with responsive, expert technical support for our formal verification and SAST engine.

---

## 📚 Documentation & Self-Service Resources

Before reaching out, check our documentation hubs:

- **Official Documentation & Guides:** [logicproofai.com/docs](https://logicproofai.com)
- **Account & License Management:** [logicproofai.com/panel](https://logicproofai.com/panel)
- **GitHub Action Configuration:** [logicproof-action Repository](https://github.com/logicproof-ai/logicproof-action)
- **CLI Downloads & Releases:** [logicproof-cli Releases](https://github.com/logicproof-ai/logicproof-cli/releases)

---

## 💼 Support Channels

### 1. Enterprise Customers (Standard & Dedicated SLA)

If your organization has an active Enterprise subscription or pilot agreement:
- **Email:** [soporte@logicproof.ai](mailto:soporte@logicproof.ai)
- **Dedicated Slack / Teams Channel:** Use your private shared customer channel.
- **SLA:**
  - Critical issues (CI/CD pipeline blocked, verification failure): < 2 hours response time.
  - General inquiries, false-positive investigations, custom rule authoring: < 1 business day.

### 2. Community & General Inquiries

- **Bug Reports:** If you encounter unexpected parser crashes or SMT solver timeouts, please open an issue in the relevant repository using our [Bug Report Template](https://github.com/logicproof-ai/.github/issues/new?template=bug_report.md).
- **Feature Requests:** Share language parser requests, ORM integrations, or custom invariant ideas in our [Feature Request Template](https://github.com/logicproof-ai/.github/issues/new?template=feature_request.md).
- **General Support Email:** [soporte@logicproof.ai](mailto:soporte@logicproof.ai)

---

## 🔍 How to Prepare an Effective Support Request

To help us resolve your inquiry as quickly as possible without sharing proprietary source code:

1. **Do NOT share private business logic or secrets.**
2. Provide a **sanitized minimal reproducer**:
   - A 5-to-10 line snippet in TypeScript, Python, or Go reproducing the behavior.
3. Include your environment details:
   - OS & Architecture (e.g. `Linux x86_64`, `macOS arm64`)
   - CLI version (`./logicproof --version`) or Action version (`logicproof-action@v1`)
   - Framework and ORM versions in use (e.g. `Prisma 5.12`, `FastAPI 0.110`)
4. Any relevant CLI error outputs (e.g. `./logicproof . --json` output with code identifiers anonymized).
