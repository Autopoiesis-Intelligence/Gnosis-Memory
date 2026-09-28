# GNOZIS — FINAL REPOSITORY ARCHITECTURE RELEASE R1

**Release:** 2026-09-28  
**Status:** canonical target architecture

## 1. Architectural decision

Gnozis is a network of bounded, interoperable kernels rather than one universal Core serving every workload.

Two primary networks are separated by trust boundary:

- Public Network — public, unauthenticated, research/knowledge/tooling surface.
- Commercial Network — protected Genesis environment for private work, partner contracts, commercial kernels, product generation and CRM.

A cross-cutting Evidence Economy records identity, provenance, evolution, tests, runtime evidence, contracts and hashes. It is not a computational super-core and does not become a universal data store.

## 2. Repository topology

### Gnozis — public product surface
Repository: Gnozis

Role:
- public entry point;
- machine-readable public registry;
- public contracts and opportunities;
- validated/public kernel releases;
- public evidence and provenance references;
- developer-facing tools;
- discovery surfaces.

Rules:
- no private customer data;
- no private Genesis implementation;
- no secret commercial contracts;
- public access must not require user authentication;
- public artifacts must be traceable to evidence/provenance.

### Gnozis-Research-Memory — public knowledge/evidence memory
Repository: Gnozis-Research-Memory

Role:
- machine-readable historical and scientific memory;
- evidence records;
- definitions and formal relations;
- public knowledge corpus;
- cross-domain relation graph;
- research context for future kernels.

The corpus is broader than STEM: mathematics, physics, chemistry, biology, history, philosophy, culture and other domains may be represented when evidence and provenance are sufficient.

This repository is not an authority root for Genesis. It is a public evidence/context source subject to eligibility and verification contracts.

### Gnozis-Genesis — protected factory / commercial coordination
Repository: Gnozis-Genesis

Role:
- protected coordination kernel;
- Module Factory;
- commercial kernel generation;
- partner development;
- private contracts;
- commercial CRM/product workflows;
- controlled evolution proposals;
- private evidence and commercial provenance.

Genesis does not perform every domain computation. It coordinates specialized kernels and creates specialized products under governed contracts.

### Gnozis-Exchange — bounded exchange surface
Repository: Gnozis-Exchange

Role:
- inter-kernel/public-to-commercial exchange contracts;
- typed artifact exchange;
- capability and scope declarations;
- evidence/provenance package transport;
- connector/adapter surfaces.

It is a transport/contract boundary, not a source of truth.

### Specialized kernel repositories
The remaining repositories are specialized-kernel slots, not additional canonical authority roots.

A kernel repository may represent a domain or workload such as mathematics, physics, chemistry, history, philosophy/culture, CAD/engineering, simulation, retrieval/synthesis, CRM or another bounded workload.

A repository receives a formal domain role only through a Kernel Manifest and evidence-backed contract. Empty repositories are not treated as completed products.

## 3. Kernel contract

Every kernel must carry a machine-readable manifest containing at minimum:

kernel_id
product_id
kernel_class
domain
parent_kernel
version
capabilities
input_contract
output_contract
state_scope
resource_budget
trust_boundary
provenance_root
evidence_root
research_hash
commercial_hash (nullable)
rights reference

A kernel must never silently acquire authority outside its declared scope.

## 4. Kernel lineage

A kernel evolves through:

Research observation
-> candidate kernel
-> implementation
-> test
-> runtime evidence
-> public/testing release
-> commercial specialization (optional)
-> further governed evolution

The lineage is persistent.

A public research kernel and a privately funded commercial specialization may share ancestry while having different scopes, evidence roots and contractual rights.

## 5. Evidence Economy

Evidence Economy is a cross-cutting provenance system.

It distinguishes at least:

- Research Hash — identifies a public research/provenance state.
- Evidence Hash — identifies a concrete evidence package/state.
- Commercial Hash — identifies a commercial contract/provenance state.

A hash identifies a specific artifact/state. A hash alone does not prove the truth of an underlying claim or automatically establish legal ownership.

The evidence chain should support:

origin
-> source
-> change
-> test
-> CI
-> runtime
-> verification
-> contract
-> release
-> current state

For commercial development:

public/research lineage
-> private specialization
-> partner contract
-> commercial evidence
-> commercial hash

## 6. Public research and commercial testing

Commercial kernels may have a public/testing representation when the owner and contract permit it.

Genesis/private development
        |
        +--> protected commercial artifact
        |
        +--> public research/testing artifact

The public/testing artifact exposes only authorized material. Its provenance can still connect it to the product lineage.

Commercial use is governed by the applicable contract/rights record; the public repository is not the legal authority.

## 7. Public Network

The Public Network is horizontally composable.

Public Retrieval
      |
      +--> Mathematics Kernel
      +--> Physics Kernel
      +--> Chemistry Kernel
      +--> History Kernel
      +--> Philosophy/Culture Kernel
      +--> Relation Kernel
      +--> Synthesis

A mathematical relation may reference a physical relation and a chemical relation without copying the entire domain corpus into one kernel.

The public layer is intended to be usable without authentication through ordinary web/search discovery, AI-terminal/search interfaces, GitHub repositories and future public APIs/connectors.

## 8. Commercial Network

Commercial work is distributed:

Genesis Coordination
      |
      +--> Partner Kernel
      +--> Product Kernel
      +--> CRM Kernel
      +--> CAD/Engineering Kernel
      +--> Physics/Simulation Kernel
      +--> Domain-specific kernels

Heavy workloads execute in the specialized kernel that owns the required capability.

The coordination layer must not become a computational bottleneck.

## 9. Inter-kernel contract

Kernels communicate through explicit contracts, not unrestricted shared state.

A contract defines:
- request identity;
- source kernel;
- target kernel;
- allowed capability;
- input schema;
- output schema;
- scope;
- resource budget;
- evidence requirement;
- provenance requirement;
- expiration/replay rules where applicable.

Core rule:

State of Kernel A is not automatically state of Kernel B.

Cross-kernel results become admissible only through the target kernel's declared input and authority rules.

## 10. Data boundary

Public knowledge contains only information appropriate for public preservation.

Private information that is not already publicly available does not enter the public knowledge corpus merely because a commercial kernel processed it.

Partner data follows:

ACCESS != COPY != TRAIN != PUBLISH != REUSE

Each transition requires an explicit contract/policy.

## 11. Coordination vs computation

The canonical Gnozis engineering Core is a coordination/evolution mechanism, not an obligation to execute every workload.

It owns reusable foundational semantics such as state and transition contracts, verification, governed evolution, provenance, persistence semantics, authorization, evidence and recovery/replay contracts.

Specialized kernels own domain computation.

This permits multiple kernels to operate concurrently without routing all work through one universal Core.

## 12. Autopoietic product generation

Genesis is intended to generate not only proposals but new product/kernel candidates.

Target loop:

observe opportunity
-> formalize contract
-> select architecture
-> generate specialized kernel
-> test
-> verify
-> create evidence
-> expose for testing
-> commercialize when authorized
-> monitor results
-> evolve

The first commercial kernels should themselves be products produced through this governed evolutionary pipeline.

Autopoiesis therefore operates at the network/product level, not merely as self-modification of one source tree.

## 13. Developer ecosystem

Public kernels and tools should be reusable by independent developers where their contracts permit.

Examples:
- GitHub catalog/search tools;
- domain knowledge catalogs;
- relation explorers;
- evidence/provenance viewers;
- public APIs/connectors;
- repository/project integration tools.

A developer may consume a public kernel without becoming part of the commercial network.

## 14. Trust hierarchy

Public information
  !=
research evidence
  !=
kernel capability
  !=
commercial contract
  !=
authority to modify Genesis

No downstream repository becomes trusted merely because Genesis wrote to it.

Workflow write permission is not equivalent to external authority.

## 15. Repository write contract

A controlled write path remains explicit:

proposal
-> authorization
-> scoped write request
-> target repository
-> commit
-> CI
-> evidence
-> provenance

Direct uncontrolled writes from external repositories into protected Genesis are prohibited by architecture.

## 16. Migration strategy

Do not perform a destructive repository rewrite to reach this architecture.

Phase order:

1. freeze the current proven Core/R2 contracts;
2. publish this architecture contract;
3. define Kernel Manifest schema;
4. define Inter-Kernel Contract schema;
5. define Evidence/Research/Commercial hash schemas;
6. classify existing repositories;
7. assign formal roles only after classification;
8. create the first public knowledge kernel;
9. create the first public/testing commercial-kernel representation;
10. create the first Genesis-generated commercial kernel;
11. prove end-to-end lineage and evidence;
12. only then consider repository renames/splits.

Historical repositories remain preserved as provenance.

## 17. Current implementation priority

Priority A — finish current Core/E7 runtime and durable replay proof.

Priority B — implement repository/kernel manifests and evidence schemas.

Priority C — establish Gnozis-Exchange contracts.

Priority D — construct the public knowledge graph/corpus.

Priority E — create the first domain kernel.

Priority F — make Genesis generate and verify the first product kernel.

Priority G — commercial testing and commercial hash lifecycle.

## 18. Completion model

A layer is complete only when:

Definition
-> implementation
-> call site
-> reachability
-> authority
-> runtime effect
-> persistence
-> audit evidence
-> CI evidence

is demonstrated for the relevant mechanism.

## 19. Final architectural statement

Gnozis is a distributed autopoietic product and knowledge network.

The Public Network accumulates and interconnects evidence-backed public knowledge and free developer capabilities.

Genesis operates the protected commercial network and generates specialized kernels, products and partner contracts.

Specialized kernels perform bounded domain work.

Gnozis-Exchange carries explicit inter-kernel contracts.

Evidence Economy records provenance, evolution, verification and commercial states.

No single kernel is required to perform all work.

No private information is silently promoted into public memory.

No hash is treated as a substitute for evidence or contract.

No external model or repository becomes an authority root merely by participating.

The network evolves by producing bounded candidates, testing them, preserving evidence, and promoting only what the relevant governance/evidence contract permits.


## 20. Master Evolution Core — authority root

The canonical Gnozis evolution Core is a **private repository and protected authority root**.

It is not the Public Network and is not exposed as an ordinary public knowledge repository.

The Master Evolution Core:
- observes admissible external/public inputs through explicit ports;
- maintains canonical evolutionary state;
- proposes and evaluates changes;
- coordinates bounded kernels;
- may initiate controlled updates to downstream public and commercial repositories;
- records evidence and provenance for each admitted downstream change.

Downstream repositories are **materialized/public/commercial surfaces**, not authority roots.

The authority direction is:

Master Evolution Core
-> proposal
-> authorization/policy
-> scoped repository write
-> downstream CI
-> evidence
-> provenance
-> new downstream state

A downstream repository MUST NOT gain authority over the Master Evolution Core merely because it receives a write from it.

Public repositories, commercial kernels, external connectors and generated artifacts are therefore treated as untrusted or separately governed downstream inputs when they return information to the Master Evolution Core.

## 21. Master Core write network

The Master Evolution Core must not use unrestricted repository credentials or direct arbitrary writes.

Required write contract:

1. identify target repository;
2. identify target path/artifact;
3. identify intended state transition;
4. bind the write to an authorized proposal;
5. enforce scope/capability;
6. execute the write through a controlled adapter;
7. run target CI/gates;
8. persist resulting commit/evidence;
9. bind provenance to the resulting downstream state;
10. make the new state admissible to the Master Core only after verification.

This contract is the primary implementation bridge between the current R2/E7 Core and the final distributed Gnozis architecture.

## 22. Master Core and load distribution

The Master Evolution Core is a coordination/evolution authority, not a universal workload executor.

It may delegate domain computation to specialized kernels.

Heavy workloads such as CAD, physical simulation, accounting calculations, indexing or large-scale synthesis must execute in bounded specialized kernels.

The Master Core receives bounded results/evidence rather than becoming the storage and compute bottleneck for every operation.

## 23. Final trust hierarchy

Master Evolution Core
  >
Genesis Control Plane
  >
Kernel contracts
  >
Downstream repositories/kernels
  >
External connectors and public sources

This hierarchy does not imply that every result from a lower layer is true. It defines authority to change system state.

Evidence is required before lower-layer observations can influence canonical evolutionary state.
