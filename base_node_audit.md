# Security Review — base/node PRs #1125 & #1113
**Auditor:** demarco2016 | **Date:** 2026-06-13

## PR #1125 — make max outbound peers configurable
**File:** execution-entrypoint, supervisord.conf

### [L-01] No upper bound on RETH_MAX_OUTBOUND_PEERS
Change: `--max-outbound-peers=${RETH_MAX_OUTBOUND_PEERS:-100}`
No cap on value. Operator setting 9999+ causes resource exhaustion / node instability.
Recommendation: Add validation or document safe range (e.g. 10-500).

### [I-01] Missing newline at EOF in supervisord.conf
Minor — can cause issues with some parsers.

---

## PR #1113 — expose Discv5 UDP port 9200
**File:** docker-compose.yml

### [L-02] Port 9200 hardcoded — no env variable
Port 9200 conflicts with Elasticsearch default. No way to remap without editing compose file.
Recommendation: Use `${DISCV5_PORT:-9200}:9200/udp` pattern.

### [I-02] No firewall/rate-limit guidance
Discv5 UDP exposed publicly. No documentation on rate limiting or firewall rules.
Recommendation: Add note in README about restricting port exposure in production.

---

## Summary
| Severity | Count |
|----------|-------|
| Low | 2 |
| Info | 2 |

Full report: https://github.com/demarco2016/tdx-audits
