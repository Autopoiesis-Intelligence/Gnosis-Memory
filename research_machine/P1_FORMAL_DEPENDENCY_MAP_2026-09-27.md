# P1 Formal Dependency Map — 2026-09-27

Status: ANALYSIS-VERIFIED, not proof verification.

Source: Gnozis/main formal/ (19 Lean artifacts).
Target: Gnozis-Research-Memory.
Purpose: record the dependency/role map without rewriting or replacing source proofs.

## Artifact observations

formal/AdmissionCommit.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

def Invariant (I : Psi → Prop) (p : Psi) : Prop := I p

structure ProofObligation (I : Psi → Prop) (candidate : Psi) where
  passed : Bool
  invariant_ok : I candidate
  viable : Prop

def FundamentalAdmission (I : Psi → Prop) (candidate : Psi) (proof : ProofObligation I candidate) : Prop :=
  proof.passed = true ∧ proof.invariant_ok

def EvolutionaryAdmission (I : Psi → Prop) (candidate : Psi) (proof : ProofObligation I candidate) : Prop :=
  proof.passed = true ∧ proof.invariant_ok ∧ proof.viable

def Admission (I : Psi → Prop) (candidate : Psi) (proof : ProofObligation I candidate) : Prop :=
  EvolutionaryAdmission I candidate proof

def SemanticCommit (I : Psi → Prop) (previous candidate : Psi)
    (proof : ProofObligation I candidate) : Prop :=
  Admission I candidate proof ∧ I previous

theorem commit_requires_admission
    (I : Psi → Prop)
    (previous candidate : Psi)
    (proof : ProofObligation I candidate)
    (h : SemanticCommit I previous candidate proof) :
    Admission I candidate proof := by
  exact h.1

theorem admitted_commit_preserves_invariant
    (I : Psi → Prop)
    (previous candidate : Psi

---

formal/CanonicalBoundary.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

/-- Canonical semantic transition surface. -/
structure CanonicalTransition where
  run : Psi → Psi

/-- Generic compatibility transitions are intentionally a different type. -/
structure CompatibilityTransition where
  run : Psi → Psi

def IsCanonical (t : CanonicalTransition) : Prop := True

def SemanticCommitPath
    (t : CanonicalTransition) (before after : Psi) : Prop :=
  t.run before = after

/-- A compatibility transition is not itself a canonical semantic proof. -/
theorem compatibility_not_canonical_by_type
    (t : CompatibilityTransition) :
    ¬ (CanonicalTransition := ⟨t.run⟩) = t := by
  intro h
  cases h

end Gnozis


---

formal/ConcreteInvariant.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

def I_projection (p : Psi) : Prop :=
  ∀ q, p = q → p = q

def TestValid (passed : Bool) : Prop :=
  passed = true

def I_test_gate (p : Psi) : Prop := True
def I_selection (p : Psi) : Prop := True
def I_locality (p : Psi) : Prop := True
def I_causal (p : Psi) : Prop := True

def I (p : Psi) : Prop :=
  I_projection p ∧
  I_test_gate p ∧
  I_selection p ∧
  I_locality p ∧
  I_causal p

theorem projection_invariant (p : Psi) :
    I_projection p := by
  intro q h
  exact h

theorem test_valid_implies_true (passed : Bool)
    (h : TestValid passed) : passed = true := by
  exact h

theorem invariant_decomposition (p : Psi) :
    I p →
    I_projection p ∧
    I_test_gate p ∧
    I_selection p ∧
    I_locality p ∧
    I_causal p := by
  intro h
  exact h

end Gnozis


---

formal/DurableAuthorization.lean
namespace Gnozis

/-- A durable authorization is consumed at most once.
    Consumption is modeled as a set membership fact, independent of Psi state. -/
def Consumed (a : String) (consumed : String → Prop) : Prop := consumed a

/-- Recovery preserves the durable consumption fact. -/
def RecoveryPreservesConsumption
    (before after : String → Prop) : Prop :=
  ∀ a, before a → after a

theorem recovered_authorization_remains_consumed
    (before after : A → Prop)
    (hRecovery : RecoveryPreservesConsumption before after)
    (a : A)
    (hConsumed : Consumed a before) :
    Consumed a after := by
  exact hRecovery a hConsumed

/-- A consumed authorization cannot be executable again. -/
def Executable (a : A) (consumed : A → Prop) : Prop :=
  ¬ Consumed a consumed

theorem recovery_cannot_reauthorize_consumed
    (before after : A → Prop)
    (hRecovery : RecoveryPreservesConsumption before after)
    (a : A)
    (hConsumed : Consumed a before) :
    ¬ Executable a after := by
  intro hExec
  exact hExec (recovered_authorization_remains_consumed before after hRecovery a hConsumed)

end Gnozis


/-- The SQLite binding maps the authorization identity and its pre-transition
    state

---

formal/FullTransition.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

structure Sigma where
  psi : Psi
  W : Type
  K : Type

def J (I : Psi → Prop) (root : K → Prop) (s : Sigma) (k : K) : Prop :=
  I s.psi ∧ root k

structure ProofObligation (I : Psi → Prop) (candidate : Psi) where
  passed : Bool
  invariant_ok : I candidate
  viable : Prop

def FundamentalAdmission (I : Psi → Prop) (candidate : Psi)
    (proof : ProofObligation I candidate) : Prop :=
  proof.passed = true ∧ proof.invariant_ok

def EvolutionaryAdmission (I : Psi → Prop) (candidate : Psi)
    (proof : ProofObligation I candidate) : Prop :=
  proof.passed = true ∧ proof.invariant_ok ∧ proof.viable

inductive AdmissionRegime
  | fundamental
  | evolutionary

def Admission (regime : AdmissionRegime) (I : Psi → Prop) (candidate : Psi)
    (proof : ProofObligation I candidate) : Prop :=
  match regime with
  | .fundamental => FundamentalAdmission I candidate proof
  | .evolutionary => EvolutionaryAdmission I candidate proof

structure CertifiedTransition
    (regime : AdmissionRegime)
    (I : Psi → Prop) (root : K → Prop)
    (s : Sigma) where
  candidate : Psi
  next : Sigma
  nextK : K
  proof : ProofObligation I ca

---

formal/Locality.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

/-- Locality: equal declared semantic inputs imply equal transition outputs.
    No undeclared external channel is part of the transition relation. -/
def Local (T : Psi → Psi) : Prop :=
  ∀ p q, p = q → T p = T q

theorem locality_implies_extensionality
    (T : Psi → Psi)
    (h : Local T) :
    ∀ p q, p = q → T p = T q := by
  exact h

/-- Causal closure at the semantic boundary is represented by the fact that
    the transition's domain is Psi itself, not an ambient external context. -/
def CausallyClosed (T : Psi → Psi) : Prop := Local T

end Gnozis


---

formal/MetaAdmission.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

def K0 (P : Psi) : Prop :=
  ∃ witness : P.X → Prop, ∀ x, witness x → witness x

structure RefinementProof (K : Psi → Prop) (P Q : Psi) where
  statement : Prop
  preserves : K P → K Q

def admitted {K : Psi → Prop} {P Q : Psi}
    (proof : RefinementProof K P Q) (h : K P) : Prop :=
  K Q

theorem meta_admission_preserves_K0
    (K : Psi → Prop)
    (proof : RefinementProof K P Q)
    (h : K P) :
    K Q := by
  exact proof.preserves h

end Gnozis


---

formal/MinimalCore.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

/-- K0 is an explicit protected predicate; the concrete Core predicate
    remains to be refined from root_invariant.py. -/
def K0 (P : Psi) : Prop :=
  ∃ witness : P.X → Prop, ∀ x, witness x → witness x

structure Transition (P : Psi) where
  next : Psi
  preserves_K0 : K0 P → K0 next

theorem accepted_transition_preserves_K0
    (t : Transition P)
    (h : K0 P) : K0 t.next := by
  exact t.preserves_K0 h

def Transition.compose (a : Transition P) (b : Transition a.next) : Transition P where
  next := b.next
  preserves_K0 := by
    intro h
    exact b.preserves_K0 (a.preserves_K0 h)

end Gnozis


---

formal/PoolCorrespondence.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

/-- A finite executable candidate pool is represented by membership. -/
def InPool (pool : List Psi) (p : Psi) : Prop :=
  p ∈ pool

def Viable
    (I : Psi → Prop)
    (candidate : Psi)
    (pool : List Psi) : Prop :=
  ∃ continuation : Psi,
    InPool pool continuation ∧
    continuation ≠ candidate ∧
    I continuation

/-- List membership gives the exact witness needed by the depth-1 proof. -/
theorem executable_pool_has_witness
    (I : Psi → Prop)
    (candidate continuation : Psi)
    (pool : List Psi)
    (hmem : InPool pool continuation)
    (hne : continuation ≠ candidate)
    (hinv : I continuation) :
    Viable I candidate pool := by
  exact ⟨continuation, hmem, hne, hinv⟩

end Gnozis


---

formal/ProofGate.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

def InPool (pool : List Psi) (p : Psi) : Prop := p ∈ pool

def Viable (I : Psi → Prop) (candidate : Psi) (pool : List Psi) : Prop :=
  ∃ continuation : Psi,
    InPool pool continuation ∧ continuation ≠ candidate ∧ I continuation

def ProofPasses (I : Psi → Prop) (candidate : Psi) (pool : List Psi) : Prop :=
  I candidate ∧ Viable I candidate pool

theorem proof_passes_implies_candidate_invariant
    (I : Psi → Prop) (candidate : Psi) (pool : List Psi)
    (h : ProofPasses I candidate pool) :
    I candidate := by
  exact h.1

theorem proof_passes_implies_viability
    (I : Psi → Prop) (candidate : Psi) (pool : List Psi)
    (h : ProofPasses I candidate pool) :
    Viable I candidate pool := by
  exact h.2

end Gnozis


---

formal/PsiInvariants.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

/-- Executable contract: the declared semantic projection is the transition input. -/
def Extensional (F : Psi → Psi) : Prop :=
  ∀ P Q, P = Q → F P = F Q

/-- Generate/Test/Select accepts only tested candidates. -/
def Tested (test : Psi → Bool) (candidate : Psi) : Prop :=
  test candidate = true

/-- A selected candidate is valid only if it passed the test. -/
def SelectionValid (test : Psi → Bool) (selected : Psi) : Prop :=
  Tested test selected

theorem selection_requires_test
    (test : Psi → Bool) (selected : Psi)
    (h : SelectionValid test selected) :
    test selected = true := by
  exact h

/-- Extensionality is preserved compositionally by function composition. -/
theorem extensional_identity :
    Extensional (fun P : Psi => P) := by
  intro P Q h
  exact h

end Gnozis


---

formal/README.md
# Minimal Formal Core

This directory is the first machine-proof target, not a claim that Gnozis is formally verified.

Target:
1. encode Psi=(X,R);
2. encode accepted transitions;
3. encode K0 as an invariant;
4. prove preservation for admitted transitions;
5. progressively replace placeholder propositions with actual definitions from Core.

The Lean file is intentionally dependency-free and minimal. It does not prove the full Gnozis architecture yet.


---

formal/RootInvariant.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

def K0 (K : Psi → Prop) : Prop := ∀ P, K P → K P

structure Kernel where
  sealed : Bool

/-- Canonical K0 corresponding to the runtime protected-kernel condition:
    the kernel must remain sealed. -/
def canonicalK0 (k : Kernel) : Prop :=
  k.sealed = true

/-- Exact logical shape of Python preserve_root:
    the protected predicate holds before and after. -/
def preserves (K : Psi → Prop) (before after : Psi) : Prop :=
  K before ∧ K after

structure Transition (K : Psi → Prop) (P : Psi) where
  next : Psi
  preserves_proof : K P → K next

theorem preserve_root
    (K : Psi → Prop)
    (t : Transition K P)
    (h : K P) : K t.next := by
  exact t.preserves_proof h

def Transition.compose
    (a : Transition K P)
    (b : Transition K a.next) : Transition K P where
  next := b.next
  preserves_proof := by
    intro h
    exact b.preserves_proof (a.preserves_proof h)

theorem compose_preserves_root
    (K : Psi → Prop)
    (a : Transition K P)
    (b : Transition K a.next)
    (h : K P) :
    K (Transition.compose a b).next := by
  exact (Transition.compose a b).preserves_proof h

end Gnozis


theorem canonicalK0

---

formal/RuntimeConformance.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

/-- Runtime conformance is represented by a semantic projection from the
    executable boundary into the formal Psi domain. -/
structure RuntimeState where
  semantic : Psi

def project (s : RuntimeState) : Psi := s.semantic

def runtimePsi (x : Type) (R : x → x → Prop) : Psi :=
  { X := x, R := R }

def Conforms (runtime : RuntimeState) (formal : Psi) : Prop :=
  project runtime = formal

theorem conformance_reflects_semantics
    (runtime : RuntimeState)
    (formal : Psi)
    (h : Conforms runtime formal) :
    project runtime = formal := by
  exact h

/-- If two runtime states have the same canonical semantic projection,
    they are indistinguishable at the formal Psi boundary. -/
def SemanticallyEquivalent
    (a b : RuntimeState) : Prop :=
  project a = project b

theorem equivalent_from_same_projection
    (a b : RuntimeState)
    (h : project a = project b) :
    SemanticallyEquivalent a b := by
  exact h

end Gnozis


/-- Canonical projection has no auxiliary runtime fields: only X and R
    participate in the formal semantic boundary. -/
theorem canonical_projection_is_semantic
    (runtime : RuntimeSt

---

formal/SelectionCommitBridge.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

structure ProofObligation (I : Psi → Prop) (candidate : Psi) where
  passed : Bool
  invariant_ok : I candidate
  viable : Prop

def Admission
    (I : Psi → Prop)
    (candidate : Psi)
    (proof : ProofObligation I candidate) : Prop :=
  proof.passed = true ∧ proof.invariant_ok ∧ proof.viable

/-- Selection is represented as choosing one member of the admitted candidate
    relation; it cannot independently manufacture an unadmitted candidate. -/
def Selected
    (I : Psi → Prop)
    (candidate : Psi)
    (proof : ProofObligation I candidate) : Prop :=
  Admission I candidate proof

structure SelectionCommit
    (I : Psi → Prop)
    (candidate : Psi)
    (proof : ProofObligation I candidate) where
  selected : Selected I candidate proof
  committed : Psi
  commit_eq : committed = candidate

theorem selection_requires_admission
    (I : Psi → Prop)
    (candidate : Psi)
    (proof : ProofObligation I candidate)
    (s : SelectionCommit I candidate proof) :
    Admission I candidate proof := by
  exact s.selected

theorem commit_is_selected_candidate
    (I : Psi → Prop)
    (candidate : Psi)
    (proof : ProofObl

---

formal/Sigma.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

structure Sigma where
  psi : Psi
  W : Type
  K : Type

/-- Current semantic obligations are documented separately from the
    protected-root predicate. Concrete I(Psi) remains to be refined from
    executable contracts; no hidden state is admitted here. -/
def I (P : Psi) : Prop := True

def Root (r : K → Prop) (k : K) : Prop := r k

def J (Inv : Psi → Prop) (r : K → Prop) (s : Sigma) (k : K) : Prop :=
  Inv s.psi ∧ Root r k

def Adm (A : Sigma → Prop) (s : Sigma) : Prop := A s

structure CertifiedTransition
    (Inv : Psi → Prop) (r : K → Prop)
    (s : Sigma) where
  next : Sigma
  nextK : K
  preserves_J : J Inv r s s.K → J Inv r next nextK

theorem certified_preservation
    (Inv : Psi → Prop) (r : K → Prop)
    (s : Sigma) (t : CertifiedTransition Inv r s)
    (h : J Inv r s s.K) :
    J Inv r t.next t.nextK := by
  exact t.preserves_J h

end Gnozis


---

formal/StatePsi.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

structure State where
  psi : Psi

def toPsi (s : State) : Psi := s.psi

def fromPsi (p : Psi) : State := ⟨p⟩

theorem from_to_psi (s : State) :
    fromPsi (toPsi s) = s := by
  cases s
  rfl

theorem to_from_psi (p : Psi) :
    toPsi (fromPsi p) = p := by
  rfl

def SemanticExtensional (F : State → Psi) : Prop :=
  ∀ s, F s = toPsi s

theorem canonical_projection :
    SemanticExtensional toPsi := by
  intro s
  rfl

end Gnozis


---

formal/TestAdmissionBridge.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

def TestValid (passed : Bool) : Prop :=
  passed = true

structure ProofObligation (I : Psi → Prop) (candidate : Psi) where
  passed : Bool
  invariant_ok : I candidate
  viable : Prop

def ProofPasses
    (I : Psi → Prop)
    (candidate : Psi)
    (proof : ProofObligation I candidate) : Prop :=
  proof.passed = true ∧ proof.invariant_ok ∧ proof.viable

def Admission
    (I : Psi → Prop)
    (candidate : Psi)
    (proof : ProofObligation I candidate) : Prop :=
  ProofPasses I candidate proof

theorem proof_passes_implies_test_valid
    (I : Psi → Prop)
    (candidate : Psi)
    (proof : ProofObligation I candidate)
    (h : ProofPasses I candidate proof) :
    TestValid proof.passed := by
  exact h.1

theorem admission_implies_test_valid
    (I : Psi → Prop)
    (candidate : Psi)
    (proof : ProofObligation I candidate)
    (h : Admission I candidate proof) :
    TestValid proof.passed := by
  exact h.1

theorem admission_implies_invariant
    (I : Psi → Prop)
    (candidate : Psi)
    (proof : ProofObligation I candidate)
    (h : Admission I candidate proof) :
    I candidate := by
  exact h.2.1

end Gnozis


---

formal/Viability.lean
namespace Gnozis

structure Psi where
  X : Type
  R : X → X → Prop

/-- A candidate is viable at depth 1 when the supplied pool contains
    a distinct continuation satisfying the same invariant. -/
def DistinctContinuation
    (candidate continuation : Psi) : Prop :=
  continuation ≠ candidate

def Viable
    (I : Psi → Prop)
    (candidate : Psi)
    (pool : Psi → Prop) : Prop :=
  ∃ continuation : Psi,
    pool continuation ∧
    DistinctContinuation candidate continuation ∧
    I continuation

/-- A proof-passing candidate must itself satisfy the invariant and have
    an invariant-valid distinct continuation. -/
def ProofPasses
    (I : Psi → Prop)
    (candidate : Psi)
    (pool : Psi → Prop) : Prop :=
  I candidate ∧ Viable I candidate pool

theorem viable_witness
    (I : Psi → Prop)
    (candidate : Psi)
    (pool : Psi → Prop)
    (h : Viable I candidate pool) :
    ∃ continuation : Psi,
      pool continuation ∧
      continuation ≠ candidate ∧
      I continuation := by
  exact h

end Gnozis


## Boundary rule

This map is descriptive evidence. It does not assert that the Lean corpus proves the full Gnozis architecture. Formal proof status must remain tied to actual Lean checking/CI evidence.

## Transfer state

The 19 source artifacts remain preserved in Gnozis/main. Physical exact-content transfer to Research-Memory is still pending for 17 files because the available write path rejected the Git-object batch operation. No reconstructed substitute is canonical.
