# Security Audit Report — base/contracts PR #261
**Target:** feat: add TDX verifier and TDX-aware TEE prover registry
**Repo:** github.com/base/contracts
**Date:** 2026-06-13
**Auditor:** demarco2016

## Scope
- src/L1/proofs/tee/TDXVerifier.sol (+267 lines)
- src/L1/proofs/tee/TEEProverRegistry.sol (+100 lines)
- src/L1/proofs/AggregateVerifier.sol (+73 lines)
- interfaces/L1/proofs/tee/ITDXVerifier.sol (+92 lines)

## Summary
- Medium: 5
- Low: 4
- Informational: 1

## Findings

### [M-01] Assembly offset assumption in _derivePublicKeyHash
File: TDXVerifier.sol | Severity: Medium
The offset 0x21 assumes a 32-byte ABI length prefix plus 1-byte 0x04 prefix.
If publicKey is encoded differently, hash silently computes over wrong bytes.
Recommendation: Add explicit validation before assembly block.

### [M-02] Default-allow risk in allowedTcbStatuses
File: TDXVerifier.sol | Severity: Medium
New TDXTcbStatus enum values are not blocked by default.
Attacker can exploit unblocked status before admin denies it.
Recommendation: Deny all statuses by default, require explicit opt-in.

### [M-03] Unvalidated impl address in _getExpectedImageHash
File: TEEProverRegistry.sol | Severity: Medium
No check that impl != address(0). Staticcall to address(0) may return success=true.
Recommendation: Add require(impl != address(0)) before staticcall.

### [M-04] uint64 overflow in _getStartingIntermediateRootAndL2SequenceNumbers
File: AggregateVerifier.sol | Severity: Medium
Large intermediateRootIndex causes silent uint64 wraparound.
Could allow invalid proofs to pass verification.
Recommendation: Use checked arithmetic or add explicit bounds.

### [M-05] Misleading revert when blockhash returns zero
File: AggregateVerifier.sol | Severity: Medium
blockhash() returns 0 for blocks older than 256. Reverts with L1OriginTooOld instead of meaningful error.
Recommendation: Add dedicated BlockhashUnavailable revert.

### [L-01] Centralized proofSubmitter - no multisig or timelock
File: TDXVerifier.sol | Severity: Low
Single EOA controls proof submission. Compromised submitter = full control.
Recommendation: Use multisig or add timelock on changes.

### [L-02] Unbounded pcrs array - DoS risk
File: TEEProverRegistry.sol | Severity: Low
No length cap on pcrs[]. Large array causes gas exhaustion.
Recommendation: Add require(pcrs.length <= MAX_PCRS).

### [L-03] Signer TEE type can be silently overwritten
File: TEEProverRegistry.sol | Severity: Low
No check if signer already registered with different TEEType.
Recommendation: Add already registered check or warning event.

### [L-04] EIP-2935 contract deployment not validated
File: AggregateVerifier.sol | Severity: Low
No check that EIP2935_CONTRACT is deployed. Misleading revert on unsupported chains.
Recommendation: Add deployment check at initialization.

### [I-01] No input validation in _setZkConfiguration
File: TDXVerifier.sol | Severity: Informational
zkConfig stored without validating config fields.
Recommendation: Add sanity checks on config fields before storage.

## Conclusion
PR #261 introduces significant attack surface via TEE proof verification.
Critical: M-04 (uint64 overflow) and M-03 (unvalidated impl address) could allow invalid proofs to pass.
Recommend addressing all Medium findings before mainnet merge.

---
Report by: demarco2016 | Independent Security Review

## Additional Findings (Post-Code Review)

### [L-05] Missing zkCoprocessor in AttestationSubmitted event
File: TDXVerifier.sol | Severity: Low
Off-chain indexers cannot distinguish Risc0 vs Succinct proofs from event logs.
Recommendation: Add ZkCoProcessorType to AttestationSubmitted event.

### [L-06] Single ZK backend at constructor time
File: TDXVerifier.sol | Severity: Low
Only one ZK coprocessor configured at deploy. Second backend requires post-deploy owner call.
Recommendation: Accept array of ZkCoProcessorConfig at constructor.

### [I-02] No secp256k1 curve point validation
File: TDXVerifier.sol | Severity: Informational
Only length + prefix checked. X,Y coordinates not validated to be on curve.
Recommendation: Add curve point validation or document assumption.

## Updated Summary
- Medium: 5
- Low: 6
- Informational: 2
