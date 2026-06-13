cat > ~/tdx-audits/README.md << 'EOF'
# TDX Audits — Smart Contract Security Reviews

Independent security audits by [@demarco2016](https://github.com/demarco2016)

## Reports
| Target | PR | Findings | Date |
|--------|----|----------|------|
| base/contracts TDX Verifier | #261 | 5M / 6L / 2I | 2026-06-13 |

## Methodology
Manual code review + static analysis on L1 proof verification contracts.
EOF

git add . && git commit -m "docs: add README"
git push
