# Master Core → Kernel Distribution Bridge

Status: implementation bridge
Release: R2
Date: 2026-09-28

## Purpose

Connect the existing Gnozis execution/evidence contracts to the final distributed-kernel architecture without turning Master Evolution Core into a universal scheduler.

## Canonical path

Master Evolution Core
-> admissible execution proposal
-> Kernel Contract
-> Capacity/Distribution decision
-> selected kernel
-> bounded execution
-> result identity
-> Evidence
-> downstream state
-> recovery/replay

## Authority

Master Core owns evolutionary authority and proposal admissibility.

The Distribution Layer owns runtime placement within declared policy.

The target Kernel owns execution of its bounded capability.

No layer may silently acquire authority belonging to another layer.

## Required binding

A distributed execution request must bind at minimum:

- proposal/execution identity;
- kernel_id;
- capability;
- input identity;
- state identity;
- content identity;
- capacity/distribution decision;
- scope;
- resource budget;
- evidence requirements.

The binding must be verifiable at runtime.

## Failure conditions

Execution is inadmissible when:

- kernel capability does not match the request;
- scope is missing or exceeded;
- resource budget is exceeded;
- execution identity is not bound;
- input/state/content identity is inconsistent;
- distribution target is not authorized;
- evidence requirements cannot be satisfied.

## Result boundary

A kernel result is an observation/result candidate until admitted by the appropriate verification rules.

A successful execution does not by itself authorize:

- canonical state mutation;
- repository writes;
- public publication;
- commercial commitment.

Those require their respective contracts.

## Implementation sequence

1. Locate the current ExecutionInput/CanonicalExecutor binding.
2. Define a minimal KernelExecutionContract adapter around the existing execution path.
3. Bind kernel_id/capability and capacity decision to execution identity.
4. Add runtime rejection tests for mismatched capability and unauthorized target.
5. Emit evidence for selected target and execution result.
6. Verify persistence and replay after restart.
7. Only then connect the mechanism to actual distributed workers.

## Completion criterion

The bridge is complete only when a real execution demonstrates:

proposal
-> admissible kernel contract
-> distribution decision
-> selected target
-> runtime execution
-> evidence
-> persistence
-> restart/replay

without requiring Master Core to route every individual operation.
