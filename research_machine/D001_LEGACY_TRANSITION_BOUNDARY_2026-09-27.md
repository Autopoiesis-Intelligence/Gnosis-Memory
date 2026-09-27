# D-001 Legacy Transition Boundary — 2026-09-27

Status: OPEN / CHARACTERIZED

## Source evidence

`core/legacy_engine.py` explicitly defines a compatibility-only State→State engine. Its transition is an arbitrary `Callable[[State], State]`.

`core/engine.py` still accepts both `Callable[[State], State]` and `PsiTransition`. The implementation dispatches to `PsiTransition.on_state()` when the transition is canonical, otherwise it executes the State callable directly.

`tests/test_markov_state_representation.py` explicitly characterizes the compatibility limitation: a generic State→State callable may close over external data not represented by State.

## Consequence

This is not evidence that the canonical Ψ transition itself has hidden history. It is evidence that the repository still exposes a separate compatibility transition surface whose semantics are weaker than the canonical Ψ contract.

Therefore D-001 cannot be closed by documentation alone.

## Closure conditions

1. Remove the State-callable path from the canonical Engine API, OR
2. isolate it behind an explicit compatibility adapter that cannot be mistaken for canonical Ψ execution and whose use is mechanically distinguishable.
3. Add regression tests proving canonical callers cannot route through the compatibility surface.
4. Run full CI against the resulting change.

No architectural status upgrade is made in this record.
