# P1 Proof ↔ Conformance Reconciliation — 2026-09-27

Source: Gnozis/main, current HEAD.
Status: RECONCILED AS AUDIT EVIDENCE; not a universal proof claim.

## Key findings

1. `docs/PROOF_MATRIX.md` explicitly states it is an audit map, not a proof claim.
2. PM-22 Machine-checked proof is only PARTIAL and points to `formal/MinimalCore.lean`.
3. The matrix identifies PM-02, PM-04 and PM-06 as unresolved formal/architectural gaps.
4. `docs/CORE_CONFORMANCE_MATRIX.md` still records the canonical Ψ transition as YELLOW because the legacy State-callable compatibility path remains.
5. `docs/MUTATION_ENTRYPOINT_MATRIX.md` confirms the inspected canonical path is gated locally, but explicitly rejects the universal claim that all semantic mutation paths require Admission.
6. `docs/OPEN_DEFECTS.md` keeps D-001, D-002, D-007–D-012 open/partial and requires implementation + regression + full-suite verification before closure.

## Formal corpus relationship

The 19 Lean artifacts under `formal/` are evidence targets. Their presence does not by itself close PM-22 or any conformance row. The repository documentation itself requires actual machine checking and integration evidence.

## Immediate engineering gates

- PM-22: connect formal definitions to actual Core definitions and replace placeholder proof targets.
- PM-02/D-001: constrain or remove the legacy State-callable semantic transition path.
- PM-04: establish explicit Admission separation in the complete live path.
- PM-06: establish universal endogenous-selection boundary, including adversarial caller coverage.
- D-011: obtain current CI run evidence for current HEAD.
- D-012: recover primary-source protected-memory specification before implementation.

## Boundary decision

Research-Memory records these findings as provenance/evidence. It does not authorize changes to Core.

No status has been upgraded merely because a document or Lean file exists.
