# Gnosis-Memory

**Purpose:** preserve and organize research context, knowledge models, architectural records, and evidence-oriented project documentation for the Gnosis ecosystem.

This repository is a knowledge and research-context surface. Its documents can inform requirements and future implementations, but a document in this repository does **not** by itself establish that a feature is implemented, reachable, running, persisted, audited, or CI-proven in `Gnosis-Core` or another runtime repository.

## Repository boundaries

- **Gnosis** — public product and evidence/discovery surface.
- **Gnosis-Core** — canonical engineering foundation; implementation and runtime claims must be checked against code, tests, CI, and exact commit.
- **Gnosis-Memory** — research context, knowledge architecture, historical records, and evidence/provenance design.
- **Gnosis-Genesis** — separate private repository; its exact factory, commercial, and authority responsibilities remain unresolved until confirmed from current repository evidence.
- **Gnosis-Exchange** — intended bounded exchange/adapter contract surface; not a universal source of truth.

This map is an initial role description, not proof that every planned integration is implemented. See the canonical organization repository map when available.

## Start here

1. [Project state](PROJECT_STATE.md) — concise historical state record. Check its date and reconcile it with current repositories before relying on implementation claims.
2. [Gnozis master audit](GNOZIS_MASTER_AUDIT.md) — detailed research history, reasoning, counterexamples, and status vocabulary.
3. [Architecture release R1](ARCHITECTURE_RELEASE_R1.md) — intended repository topology and trust boundaries.
4. [Final product specification R2](FINAL_PRODUCT_TZ_R2.md) — product target and planned responsibilities.

## Evidence and freshness rules

Use the following evidence ladder when describing capability status:

`DESIGN → IMPLEMENTED → REACHABLE → RUNTIME → PERSISTED → AUDITED → CI_PROVEN`

Do not promote a design document or historical report into a current implementation claim. For current engineering status, record the exact repository, branch, commit SHA, relevant tests/CI run, and runtime evidence. If the sources disagree, mark the item **RECONCILIATION REQUIRED** until resolved.

Historical documents are preserved as historical records; update their status or add a dated reconciliation note rather than silently rewriting history.

## Naming

The current product name is **Gnosis**. `Gnozis` and `Gnozis-V2` are historical spellings/names and must not be used as current product or repository identities. Repository links must use canonical GitHub names such as `Autopoiesis-Intelligence/Gnosis-Core`. Do not reintroduce obsolete paths such as `Mikhail-Kucheriavyi-23/Gnozis-V2` as current locations.
