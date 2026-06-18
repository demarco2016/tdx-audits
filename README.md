# TDX Audits — Smart Contract Security Reviews

Independent security audits by [@demarco2016](https://github.com/demarco2016).

[![X Follow](https://img.shields.io/twitter/follow/Demarco639?style=social&label=Follow)](https://x.com/Demarco639)

---

## Reports

| Target | Type | Findings | Date |
|--------|------|----------|------|
| [base/contracts PR #261](tdx_audit_report.md) — TDX Verifier | Solidity | 5M / 6L / 2I | 2026-06-13 |
| [base/node PRs #1125 & #1113](base_node_audit.md) — Node config | Rust / Infra | 2L / 2I | 2026-06-13 |

**Legend:** M = Medium, L = Low, I = Informational

## Methodology

Manual code review with static analysis. Each finding includes:
- **Location** — exact file and line
- **Impact** — severity rationale
- **Recommendation** — actionable fix

Focus areas: L1 proof verification, TEE integration, access controls, input validation.

## Scope

Primarily auditing Base ecosystem contracts and infrastructure. Open to reviewing other EVM-compatible protocols — reach out on X.

---

<sub>Reports published by [@demarco2016](https://github.com/demarco2016) for transparency and community benefit.</sub>
