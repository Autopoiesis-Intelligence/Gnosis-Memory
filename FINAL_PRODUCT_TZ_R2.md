# GNOZIS — FINAL PRODUCT ARCHITECTURE & TECHNICAL SPECIFICATION

Release: R2 / 2026-09-28
Status: canonical implementation target

## 1. Product definition

Gnozis is a distributed autopoietic product and knowledge network.

It has two trust-separated operating environments:

1. Public Network — unauthenticated public knowledge, evidence, relations and free tools.
2. Commercial Network (Genesis) — protected product factory, customer/partner work, private contracts, CRM, finance, marketing and specialized commercial kernels.

A cross-cutting Evidence Economy provides provenance, lineage, verification and hash-based state identification.

The system is a network of bounded kernels, not one universal computational core.

## 2. Product objectives

### Public
- build an evidence-backed, continuously updated corpus of public knowledge;
- connect knowledge across mathematics, physics, chemistry, biology, engineering, history, philosophy, culture and other domains;
- expose the corpus without mandatory authentication;
- provide free developer and cultural/scientific tools;
- publish selected research/testing kernels;
- make every published kernel traceable to its product lineage and evidence.

### Commercial
- discover opportunities;
- generate and test products;
- operate partner/customer workflows;
- provide CRM and accounting;
- provide marketing and market research;
- generate specialized kernels for CAD, simulation, engineering and other workloads;
- maintain private contracts and commercial evidence;
- expose authorized kernels for testing;
- produce commercial products through the autopoietic pipeline.

## 3. Canonical repository architecture

### Public repositories
**Gnozis**
- public product entry point;
- public registry;
- documentation and discovery;
- public APIs/connectors;
- public kernel catalog;
- public developer tools.

**Gnozis-Research-Memory**
- evidence-backed public memory;
- knowledge objects and relations;
- historical/scientific/philosophical/cultural corpus;
- machine-readable provenance references.

**Public kernel repositories**
- one repository per bounded domain/workload where independent development and release are useful;
- no repository becomes an authority root solely by being public.

### Commercial repositories
**Gnozis-Genesis**
- protected factory/control plane;
- evolution orchestration;
- private product generation;
- partner/customer contracts;
- commercial evidence.

**Gnozis-Exchange**
- typed exchange contracts;
- inter-kernel adapters;
- public/commercial boundary;
- no universal data store.

**Commercial kernel repositories**
- specialized private or dual-surface kernels.

## 4. Required commercial kernels

Genesis must support at least these bounded kernels:

### Product Kernel
Product discovery, specification, portfolio and lifecycle.

### Research Kernel
Market/technical research and evidence synthesis.

### Marketing Kernel
Market signals, segmentation, experiments, positioning, campaign planning and measurement. It may use external marketing services through adapters.

### Sales/Partner Kernel
Opportunities, partner/customer pipeline, contracts and commercial interactions.

### CRM Kernel
Scoped customer/team relationship state. Private by default.

### Accounting Kernel
Commercial accounting workflows, invoices, costs, revenue, budgets, financial evidence and reporting. It is a first-class kernel, not a generic Genesis function.

The Accounting Kernel must have a public/testing surface containing only safe synthetic/demo/public data and must never expose private financial records.

### Product Engineering Kernels
Examples: CAD/DFM, simulation, manufacturing, optimization and domain-specific engineering.

Additional kernels may be created by Genesis when justified by an explicit Kernel Manifest and evidence-backed product contract.

## 5. Owner Control Plane

The product owner operates Genesis through a dedicated control plane.

Owner controls include:
- kernel activation/deactivation;
- connector enablement;
- scopes and capabilities;
- resource budgets;
- data permissions;
- publication permissions;
- commercial-test permissions;
- contract approval thresholds;
- automation levels;
- audit/recovery policies.

Owner control is governance, not manual execution of every task.

The owner may configure the factory while specialized kernels perform bounded work.

## 6. Kernel Manifest

Every kernel must provide machine-readable:

- kernel_id
- product_id
- kernel_class
- domain
- parent_kernel
- version
- capabilities
- input_contract
- output_contract
- state_scope
- resource_budget
- trust_boundary
- connector_set
- provenance_root
- evidence_root
- research_hash
- commercial_hash (nullable)
- rights/contract reference
- public_test_status

No undeclared capability may be treated as authoritative.

## 7. Kernel lifecycle

Target lifecycle:

observe
-> candidate
-> specify
-> generate/build
-> test
-> runtime verify
-> evidence
-> public/test release
-> commercial contract (optional)
-> deploy
-> measure
-> evolve

A kernel may remain research-only.

A commercial kernel may expose a public/testing representation when authorized.

## 8. Public/testing representation of commercial kernels

The public/testing repository is a controlled projection, not a copy of private Genesis.

It may contain:
- public code;
- synthetic fixtures;
- public documentation;
- benchmark results;
- public evidence;
- research lineage;
- safe demo connectors.

It must not contain:
- customer secrets;
- private contracts;
- private financial records;
- private CRM;
- confidential partner data;
- private product strategy.

## 9. Evidence Economy

Three distinct technical identifiers are mandatory:

**Research Hash** — research/public lineage state.

**Evidence Hash** — concrete evidence package/state.

**Commercial Hash** — commercial contract/provenance state.

Evidence chain:

source
-> observation
-> candidate
-> implementation
-> test
-> CI
-> runtime
-> verification
-> contract
-> release
-> current state

Hash does not itself establish truth or legal ownership. It identifies a specific recorded state.

## 10. Inter-kernel contracts

No unrestricted shared state.

A contract contains:
- request_id;
- source_kernel;
- target_kernel;
- capability;
- input schema;
- output schema;
- scope;
- resource budget;
- evidence requirements;
- provenance requirements;
- replay/expiration rules where required.

A result becomes usable by another kernel only through the target kernel's admissibility rules.

## 11. Public knowledge model

Knowledge is represented as objects plus relations, not isolated encyclopedia pages.

Example:

Math concept
<-> physical law
<-> chemical model
<-> historical discovery
<-> scientist/public figure
<-> philosophical interpretation

Each relation must have provenance/evidence status.

The system stores public information only. Non-public personal/customer information does not enter the public knowledge memory.

## 12. Retrieval and synthesis

Public users can interact without mandatory authentication through:
- search engines;
- public web interfaces;
- AI-terminal interfaces;
- GitHub;
- future APIs/connectors.

Retrieval may route requests to specialized kernels.

Synthesis must preserve source/evidence references and must not silently convert an uncertain claim into verified knowledge.

## 13. External connectors

Connectors are adapters, not kernels.

Examples:
- GPT/AI assistant connectors;
- GitHub;
- CRM;
- accounting services;
- marketing platforms;
- cloud storage;
- CAD/engineering systems;
- analytics services.

Only the minimum authorized data required for a task crosses a connector boundary.

ACCESS != COPY != TRAIN != PUBLISH != REUSE

## 14. Commercial factory loop

Marketing/Research may produce an opportunity.

Product Kernel formalizes it.

Genesis selects a candidate architecture.

Specialized Engineering Kernel builds/tests it.

Evidence Kernel/Layer records results.

Owner Control Plane governs promotion.

Sales/Partner Kernel manages commercial engagement.

CRM and Accounting maintain scoped business state.

Marketing receives measured feedback.

The loop repeats.

## 15. Accounting boundary

Accounting is an independent kernel because financial state has distinct authority and audit requirements.

Accounting inputs must be typed and scoped.

Examples:
- invoice;
- expense;
- revenue;
- contract obligation;
- budget;
- payment state.

Accounting outputs require evidence/provenance.

Marketing, CRM and Product kernels may request accounting information, but they do not gain write authority to accounting state merely by reading it.

## 16. Security and privacy

Public and Commercial Networks have separate trust boundaries.

Private data is never promoted to public memory by default.

A connector cannot grant itself additional authority.

No external AI is an authority root.

No repository becomes trusted merely because Genesis writes to it.

## 17. Autopoiesis

Autopoiesis is implemented as a governed network loop:

environment
-> observation
-> internal candidate
-> test
-> selection
-> verification
-> commitment
-> new capability/product
-> new observations

The system must preserve human governance at defined promotion boundaries.

Autopoiesis creates opportunities and products; it does not remove owner control.

## 18. Performance architecture

Heavy workloads are distributed to specialized kernels.

The coordination layer must not become a universal bottleneck.

Kernels have resource budgets and can be independently scaled, paused, tested or replaced.

A client can test a specialized kernel before purchasing/contracting the corresponding commercial capability, when the kernel's rights/public-test policy permits.

## 19. Repository governance

Every repository must declare:
- role;
- trust class;
- owner;
- kernel_id if applicable;
- public/private status;
- evidence root;
- upstream/downstream contracts;
- write authority.

Controlled write path:

proposal
-> authorization
-> scoped write request
-> target repository
-> commit
-> CI
-> evidence
-> provenance

## 20. Implementation order

Phase 0 — preserve and finish current Core/R2/E7 durable replay evidence.

Phase 1 — Kernel Manifest.

Phase 2 — Inter-Kernel Contract.

Phase 3 — Evidence/Research/Commercial Hash schemas.

Phase 4 — repository classification and governance manifests.

Phase 5 — Gnozis-Exchange.

Phase 6 — first Public Knowledge Kernel and relation graph.

Phase 7 — first public/testing commercial kernel.

Phase 8 — Genesis Product/Research/Marketing/Sales/CRM/Accounting kernels.

Phase 9 — first Genesis-generated commercial product.

Phase 10 — owner-configurable factory and connector ecosystem.

Phase 11 — continuous autopoietic product/evidence loop.

## 21. Definition of done

A mechanism is complete only when:

Definition
-> Implementation
-> Call Site
-> Reachability
-> Authority
-> Runtime Effect
-> Persistence
-> Audit Evidence
-> CI Evidence

is demonstrated.

For a kernel, additionally:

Manifest
-> Contract
-> Tests
-> Runtime
-> Evidence
-> Public/private boundary
-> Provenance
-> Recovery

must be demonstrated.

## 22. Non-goals

- one universal mega-kernel;
- unrestricted shared memory between kernels;
- silent ingestion of private information into public memory;
- treating hashes as automatic legal ownership;
- making every workload run continuously;
- making the owner manually execute every business operation;
- giving external connectors implicit authority.

## 23. Final product statement

Gnozis is a networked autopoietic factory and public knowledge system.

The Public Network builds and exposes an interconnected evidence-backed corpus and reusable tools.

Genesis is a protected commercial factory in which specialized kernels discover opportunities, build products, operate customer/partner workflows, market them, account for them and evolve them.

Commercial kernels can have authorized public/testing surfaces.

Evidence Economy makes origin, evolution, testing, verification and commercial state machine-verifiable.

The product scales by adding bounded kernels and contracts rather than making one Core perform everything.
