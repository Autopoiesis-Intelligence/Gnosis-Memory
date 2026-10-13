# Gnosis-Memory

Gnosis-Memory is the project's durable research-context and memory repository.

## Purpose

- Preserve project context, decisions, and references that should outlive a single AI session.
- Organize research notes, definitions, provenance, and evidence pointers.
- Distinguish established facts from hypotheses, rejected directions, and unresolved questions.
- Help contributors recover working state without treating conversational memory as proof.

## Evidence discipline

Project state should be checked against the current repository, exact commit, tests, CI runs, and runtime evidence as applicable. A saved note or summary is a recovery aid, not independent proof that a feature exists or works.

Use these distinctions consistently:

- documented ≠ implemented;
- implemented ≠ reachable;
- tests passing ≠ runtime evidence;
- evidence ≠ execution authority;
- persistence ≠ ingestion or execution.

## Boundaries

Gnosis-Memory stores context and references; it does not grant authority to modify Gnosis-Core or execute protected operations. Core changes remain subject to the relevant implementation, verification, governance, and authorization contracts.

## Current status

This README describes the repository's intended role. Specific capabilities and completeness must be established from the files and evidence present on the current default branch; no end-to-end memory workflow is implied by this document alone.
