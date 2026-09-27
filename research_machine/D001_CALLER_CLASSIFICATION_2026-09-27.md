# D-001 Caller Classification — 2026-09-27

Status: CLASSIFIED / NO MAIN MERGE

| Caller | Classification | Required action |
|---|---|---|
| core/uroboros.py evolutionary path | CANONICAL Ψ adapter | retain only with PsiTransition |
| gnosis-terminal-bridge/src/core_evolution.py | BRIDGE/COMPATIBILITY | must not enter canonical Ψ authority |
| tests/test_core.py | GENERIC LEGACY TEST | migrate to LegacyEngine or remove |
| tests/examples/basic.py | EXAMPLE/LEGACY | migrate or explicitly mark compatibility |
| tests/test_legacy_transition_boundary.py | LEGACY BOUNDARY TEST | keep on LegacyEngine; do not use canonical Engine |
| tests/test_engine_usage_classification.py | GENERIC ENGINE TEST | migrate to LegacyEngine if testing callable semantics |
| tests/test_local_rule_self_modification.py | REVIEW | inspect transition type; canonicalize if PsiTransition |
| tests/test_adversarial_self_modification.py | REVIEW | inspect transition type; canonicalize if PsiTransition |
| tests/test_red_team_universal_state_sufficiency.py | REVIEW | inspect transition type; canonicalize if PsiTransition |

## Gate

The experimental branch must not be merged until every non-canonical callable use of Engine is either:
1. migrated to LegacyEngine/explicit compatibility API, or
2. converted to PsiTransition.

The canonical Uroboros path must remain PsiTransition-backed.

## Source snapshot

### core/uroboros.py (4ecb03c6c39a0f919258805447e3c8f9490280da)

```text
from __future__ import annotations

import hashlib
from dataclasses import dataclass, field
from typing import Iterable

from .engine import Engine
from .execution import CanonicalExecutor, Generator, Tester
from .execution_contract import execution_input_from_psi
from .relation import Relation
from .state import Psi, State
from .history import AppendOnlyHistory
from .psi_transition import PsiTransition
from .canonical_boundary import canonicalize_psi
from .legacy_engine import LegacyEngine
from .evolution import evolutionary_transition


def _unconfigured_transition(state: State) -> State:
    raise RuntimeError(
        "Uroboros has no transition configured; provide an Engine or use Uroboros.evolutionary()."
    )


def _transition_content_digest(transition: PsiTransition) -> str:
    payload = repr(transition.configuration).encode("utf-8")
    return hashlib.sha256(payload).hexdigest()


@dataclass(frozen=True)
class Uroboros:
    """Recursive GNOSIS/UROBOROS computational core.

    The PsiEngine/PsiTransition path is canonical. State-based evolution is
    retained as an explicit compatibility surface.
    """

    state: State = field(default_factory=State)
    engine: Engine = field(default_factory=lambda: Engine(transition=_unconfigured_transition))
    executor: CanonicalExecutor | None = None
    generate: Generator | None = None
    test: Tester | None = None
    psi_transition: PsiTransition | None = None

    @classmethod
    def canonical(
        cls,
        *,
        transition: PsiTransition,
        state: State,
        kernel_version: str = "gnozis-core",
        history: AppendOnlyHistory | None = None,
    ) -> "Uroboros":
        if not isinstance(state, State):
            raise TypeError("canonical Uroboros requires a State adapter input.")
        initial = canonicalize_psi(state.to_psi()).psi
        return cls(
            state=State.from_psi(initial),
            engine=Engine(transition=transition),
            executor=CanonicalExecutor(
                history=history or AppendOnlyHistory(),
                kernel_version=kernel_version,
            ),
            psi_transition=transition,
        )

    @classmethod
    def evolutionary(
        cls,
        *,
        generate: Generator,
        test: Tester,
        state: State | None = None,
        kernel_version: str = "gnozis-core",
        history: AppendOnlyHistory | None = None,
    ) -> "Uroboros":
        initial = state if state is not None else State(values={"x": (), "relations": ()})
        if not isinstance(initial, State):
            raise TypeError("evolutionary Uroboros requires a State adapter input.")
        initial.to_psi()
        return cls(
            state=initial,
            engine=LegacyEngine(transition=evolutionary_transition(generate, test)),
            executor=None,
            generate=generate,
            test=test,
        )

    def step(self) -> "Uroboros":
        if self.psi_transition is not None and self.executor is not None:
            canonical_input = canonicalize_psi(self.state.to_psi())
            execution_input = execution_input_from_psi(
                canonical_input.psi,
                input_type="psi_transition",
                content_digest=_transition_content_digest(self.psi_transition),
            )
            result = self.executor.step(
                canonical_input.psi,
                self.psi_transition,
                execution_input,
                test=self.test,
            )
            committed = result.psi
            return Uroboros(
                state=State.from_psi(committed),
                engine=self.engine,
                executor=self.executor,
                generate=self.generate,
                test=self.test,
                psi_transition=self.psi_transition,
            )

        if self.executor is not None:
            if self.generate is None or self.test is None:
                raise RuntimeError("Canonical executor requires generate and test.")
            canonical_input = canonicalize_psi(self.state.to_psi())
            result = self.executor.evolve(
                canonical_input.psi,
                self.generate,
                self.test,
            )
            committed = result.psi
            return Uroboros(
                state=State.from_psi(committed),
                engine=self.engine,
                executor=self.executor,
                generate=self.generate,
                test=self.test,
            )
        try:
            next_state = self.engine.step(self.state)
        except ValueError as exc:
            if self.executor is not None or self.generate is None or self.test is None:
                raise
            if str(exc) != "No valid candidate state passed the test":
                raise
            next_state = self.state
        return Uroboros(state=next_state, engine=self.engine, executor=self.executor, generate=self.generate, test=self.test, psi_transition=sel
```

### gnosis-terminal-bridge/src/core_evolution.py (4dec8153cca781d50426ebc17e1eb68353d78c57)

```text
from __future__ import annotations

from typing import Callable

from core import Engine, State

from .agency_context import AgencyContext


ContextualTransition = Callable[[State, AgencyContext], State]


def engine_from_agency_context(
    context: AgencyContext,
    transition: ContextualTransition,
) -> Engine:
    """Create a deterministic core Engine from one verified agency context.

    The context is captured at the bridge/core boundary. After construction,
    Engine.step() requires only State, so subsequent evolution is endogenous
    and does not need another authentication or external identity lookup.
    """
    if not context.identity.authenticated:
        raise ValueError("Agency identity is not authenticated")

    def bound_transition(state: State) -> State:
        return transition(state, context)

    return Engine(transition=bound_transition)

```

### tests/test_core.py (fdfd1292194d4a1c5a691a3de4e606c3feb85542)

```text
from core import Engine, Relation, State, Uroboros


def increment(state: State) -> State:
    value = state.values.get("value", 0)
    return State(values={"value": value + 1})


def test_state_creation():
    state = State(values={"value": 1})
    assert state.values["value"] == 1


def test_relation_creation():
    relation = Relation(source="a", target="b")
    assert relation.source == "a"
    assert relation.target == "b"


def test_engine_creation():
    engine = Engine(
        transition=increment
    )
    assert engine is not None


def test_engine_step():
    state = State(values={"value": 1})

    engine = Engine(
        transition=increment
    )

    next_state = engine.step(state)

    assert next_state.values["value"] == 2


def test_engine_run():
    state = State(values={"value": 1})

    engine = Engine(
        transition=increment
    )

    result = engine.run(state, steps=3)

    assert result.values["value"] == 4


def test_engine_trajectory():
    state = State(values={"value": 1})

    engine = Engine(
        transition=increment
    )

    trajectory = list(
        engine.trajectory(state, steps=3)
    )

    assert len(trajectory) == 4
    assert trajectory[0].values["value"] == 1
    assert trajectory[1].values["value"] == 2
    assert trajectory[2].values["value"] == 3
    assert trajectory[3].values["value"] == 4


def test_uroboros_initialization():
    uroboros = Uroboros()

    assert uroboros.state is not None
    assert uroboros.engine is not None


def test_uroboros_step():
    uroboros = Uroboros(
        state=State(values={"value": 1}),
        engine=Engine(
            transition=increment
        ),
    )

    next_uroboros = uroboros.step()

    assert next_uroboros.state.values["value"] == 2

```

### tests/examples/basic.py (61912927759a1a0e0e0f1eb067dfe9a5a09e1580)

```text
from core import Engine, State, Uroboros


def increment(state: State) -> State:
    value = state.values.get("x", 0)

    return state.evolve(
        values={
            **state.values,
            "x": value + 1,
        }
    )


engine = Engine(transition=increment)

system = Uroboros(
    state=State(values={"x": 0}),
    engine=engine,
)

for _ in range(10):
    system = system.step()

print("Final state:", system.state.values)


```

### tests/test_legacy_transition_boundary.py (08b34865e0e0f07a0affef0d245811bb4eb3b457)

```text
from __future__ import annotations

import pytest

from core.engine import Engine
from core.state import State


def test_legacy_state_transition_remains_explicit_compatibility_path() -> None:
    def legacy_transition(state: State) -> State:
        return state.evolve(values={"x": state.values.get("x", 0) + 1})

    result = Engine(transition=legacy_transition).step(State(values={"x": 0}))

    assert result.values["x"] == 1
    assert isinstance(result, State)


def test_legacy_transition_cannot_return_non_state() -> None:
    def invalid_transition(_state: State):
        return {"x": 1}

    with pytest.raises(TypeError, match="must return a State"):
        Engine(transition=invalid_transition).step(State(values={"x": 0}))

```

### tests/test_engine_usage_classification.py (acd13820b97638127b26647900d055a33410d3db)

```text
from core.engine import Engine
from core.state import State


def test_generic_engine_is_not_canonical_psi_evidence() -> None:
    def transition(state: State) -> State:
        return state.evolve(values={"x": state.values.get("x", 0) + 1})

    result = Engine(transition=transition).step(State(values={"x": 0}))

    assert result.values["x"] == 1
    assert isinstance(result, State)

```

### tests/test_local_rule_self_modification.py (8b5ac105a11d3f0931b9ffb6ed5a8a8ca6f5f5ad)

```text
from core.engine import Engine
from core.evolution import evolutionary_transition
from core.state import State


def test_local_rule_evolution_is_independent_of_disconnected_component():
    state_a = State(values={
        "nodes": {"a": 1, "b": 2, "z": 10},
        "relations": (("a", "b"),),
        "rules": {"a": "add_neighbor"},
    })
    state_b = State(values={
        "nodes": {"a": 1, "b": 2, "z": 9999},
        "relations": (("a", "b"),),
        "rules": {"a": "add_neighbor"},
    })

    def generator(state):
        nodes = dict(state.values["nodes"])
        rules = dict(state.values["rules"])
        nodes["a"] += nodes["b"]
        rules["a"] = "add_neighbor"
        return (state.evolve(values={**state.values, "nodes": nodes, "rules": rules}),)

    def tester(state):
        return all("->" in r or isinstance(r, tuple) for r in state.values["relations"])

    transition = evolutionary_transition(generator, tester)
    result_a = Engine(transition).step(state_a)
    result_b = Engine(transition).step(state_b)

    assert result_a.values["nodes"]["a"] == result_b.values["nodes"]["a"] == 3
    assert result_a.values["rules"]["a"] == result_b.values["rules"]["a"] == "add_neighbor"


def test_local_rule_candidate_is_selected_from_local_neighborhood():
    initial = State(values={
        "nodes": {"a": 1, "b": 2, "z": 100},
        "relations": (("a", "b"),),
        "rules": {"a": "add_neighbor"},
    })
    valid = initial.evolve(values={
        "nodes": {"a": 3, "b": 2, "z": 100},
        "relations": (("a", "b"),),
        "rules": {"a": "add_neighbor"},
    })
    bad_remote = initial.evolve(values={
        "nodes": {"a": 999, "b": 2, "z": 100},
        "relations": (("a", "b"),),
        "rules": {"a": "remote_z_dependent"},
    })

    def generator(state):
        return (bad_remote, valid)

    def tester(state):
        return state.values["rules"]["a"] == "add_neighbor" and state.values["nodes"]["a"] == 3

    result = Engine(evolutionary_transition(generator, tester)).step(initial)
    assert result.values["rules"]["a"] == "add_neighbor"
    assert result.values["nodes"]["a"] == 3

```

### tests/test_adversarial_self_modification.py (77eafdeb58c8200c6029d3831bbc4f2d5bbcbb40)

```text
from core.engine import Engine
from core.evolution import evolutionary_transition
from core.state import State


def test_invalid_rule_candidate_is_rejected_before_next_generation():
    initial = State(values={"x": 0, "relations": ("a->b",), "rule": "stable"})
    invalid = initial.evolve(values={"x": 99, "relations": ("a->b", "b->c"), "rule": "corrupt"})
    valid = initial.evolve(values={"x": 1, "relations": ("a->b", "b->c"), "rule": "extend"})

    def generator(state):
        return (invalid, valid)

    def tester(state):
        return state.values["rule"] in {"stable", "extend"}

    result = Engine(evolutionary_transition(generator, tester)).step(initial)

    assert result.values["rule"] == "extend"
    assert result.values["x"] == 1


def test_no_valid_rule_cannot_enter_recursive_cycle():
    initial = State(values={"x": 0, "relations": (), "rule": "stable"})
    invalid = initial.evolve(values={"x": 99, "relations": ("broken",), "rule": "corrupt"})

    def generator(state):
        return (invalid,)

    def tester(state):
        return state.values["rule"] in {"stable", "extend"}

    transition = evolutionary_transition(generator, tester)

    try:
        Engine(transition).step(initial)
    except (ValueError, RuntimeError):
        return

    raise AssertionError("invalid self-modifying rule escaped Test/Select")

```

### tests/test_red_team_universal_state_sufficiency.py (43ab79202bed2777d4e81dcc46ff4bd034c9c927)

```text
import pytest

from core.engine import Engine
from core.state import State


@pytest.mark.xfail(reason="Current State->State compatibility API permits hidden closure state; canonical Ψ contract is tested separately.", strict=True)
def test_current_transition_api_does_not_guarantee_x_r_sufficiency():
    """Red-team counterexample: a transition closure can depend on hidden state.

    This is intentionally an expected failure.  The current API accepts an
    arbitrary callable(State) -> State, so nothing in the type/contract stops
    the callable from closing over information outside State.
    """
    state_a = State(values={"x": 1, "relations": (("a", "b"),)})
    state_b = State(values={"x": 1, "relations": (("a", "b"),)})

    hidden = {"value": 0}

    def transition(state):
        return state.evolve(values={"x": state.values["x"] + hidden["value"]})

    result_a = Engine(transition).step(state_a)

    hidden["value"] = 100
    result_b = Engine(transition).step(state_b)

    # Same (X, R), different transition => current API does not enforce
    # E = E(X, R). This assertion is expected to fail until the kernel
    # contract eliminates hidden transition state.
    assert result_a.values["x"] == result_b.values["x"]


@pytest.mark.xfail(reason="Current transition API permits hidden closure state; kernel contract must eliminate this.", strict=True)
def test_kernel_contract_should_forbid_hidden_transition_dependency():
    """Contract target for the future minimal-state kernel."""
    state_a = State(values={"x": 1, "relations": (("a", "b"),)})
    state_b = State(values={"x": 1, "relations": (("a", "b"),)})

    hidden = {"value": 0}

    def transition(state):
        return state.evolve(values={"x": state.values["x"] + hidden["value"]})

    first = Engine(transition).step(state_a)
    hidden["value"] = 100
    second = Engine(transition).step(state_b)

    assert first.values["x"] == second.values["x"]

```

No claim of test execution is made by this artifact.
