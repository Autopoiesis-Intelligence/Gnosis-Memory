# Gnozis World Model + Self-Evolution Architecture Baseline

Status: THEORETICAL BASELINE REGISTERED
Date: 2026-09-30
Scope: World Model, epistemic processing, async runtime, self-evolution boundary, R2 integration.

## 1. Core invariants

- Nothing becomes knowledge by declaration.
- Observation, Representation, Measurement, Evidence, Relation and Tension are distinct concepts.
- Authoritative history is immutable; state changes are represented by transitions.
- Projection, cache and index are derived and non-authoritative.
- Context is a computational/semantic scope, not an absolute epistemic boundary.
- Fast ingestion must not perform synchronous epistemic validation.
- Tension identifies unresolved epistemic conflict; it does not itself select a resolution.
- Candidate is a testable change hypothesis, not a committed change.
- No system evolution may occur without an immutable causal chain from evidence through candidate, test, verification, governance and commit.
- External input, LLM output, projections and repository state do not directly possess Core authority.
- World Model evolution must pass the existing authorization/execution/commit boundary.

## 2. Canonical primitives

WorldObservation
Representation
Measurement
Evidence
Relation
Tension
EpistemicTransition
StateRecord
Context

## 3. Epistemic states

OBSERVED
SUPPORTED
ACCEPTED
UNRESOLVED
REJECTED
SUPERSEDED

State is derived from an admissible transition chain. A requested state value from an external caller is not authoritative.

## 4. Transition model

OBSERVED -> SUPPORTED | UNRESOLVED | REJECTED
SUPPORTED -> ACCEPTED | UNRESOLVED | REJECTED
ACCEPTED -> SUPERSEDED | UNRESOLVED
UNRESOLVED -> SUPPORTED | ACCEPTED | REJECTED

Transitions are immutable records carrying basis/provenance. Domain-specific reason codes and exact repository enforcement remain implementation work.

## 5. Context semantics

context_ref is the primary computational locality index. It MUST NOT be treated as an absolute semantic isolation boundary.

Cross-context evaluation proceeds conceptually:
local context -> explicitly related contexts -> broader/global search.

Tension requires semantic compatibility; equal property names alone do not establish contradiction.

## 6. Runtime model

FAST PATH:
canonical ingestion + durable persistence + acknowledgement.

MEDIUM PATH:
indexes, projections and hot views.

SLOW PATH:
epistemic evaluation, tension detection, transition generation and system findings.

AUTHORITATIVE COMMIT:
the only boundary that changes authoritative evolution/history.

Projection loss must be recoverable by replay of authoritative records.

## 7. Performance status

The architecture supports append-oriented ingestion, asynchronous processing, projection-based reads, localized search and immutable history.

Previously proposed numeric values such as 500k operations/sec, <10 microseconds ingestion, <100ns hot lookup, <1ms tension detection and 3-5x compression are TARGET BENCHMARKS only. They are not repository-proven characteristics.

## 8. World Model -> Candidate boundary

Tension != Candidate.

World-level tension remains in epistemic/world resolution.

Only a System Finding may generate a Candidate for Core evolution.

Canonical evolution chain:
Finding -> Candidate -> Test -> Select -> Verify -> Governance -> Commit -> resulting Core state/version.

Candidate generation does not confer authority to modify the Core.

## 9. LLM and external actors

LLM/external actors may propose interpretations, hypotheses, candidates or tests.

They MUST NOT directly establish authoritative epistemic state, authorization, commit or Core mutation.

## 10. R2 integration

Canonical conceptual path:

World Model -> Candidate -> ExecutionInput -> Authorization -> CanonicalExecutor -> Commit -> Evidence/Audit.

The World Model must not bypass existing R2 authority boundaries.

## 11. Implementation classification

Future repository audit findings are classified as:
MATCH / GAP / CONTRADICTION / UNIMPLEMENTED / UNPROVEN.

Repository implementation and benchmark evidence are intentionally NOT claimed by this theoretical baseline.

## 12. Next phase

Repository audit and mapping:
Primitive -> existing type -> persistence -> call site -> reachability -> authority -> runtime effect -> persistence/audit evidence -> CI evidence.

This document freezes the theoretical baseline for the modernization cycle; implementation findings may refine repository contracts but must not silently redefine the architecture.
