# Lean Coverage

Lean version: `4.33.0`

Main Palomar declaration:

`FD1D.V5.Palomar.optimalDynamicMatchingUpperBound`

## Compared Coverage

| Manuscript component | Lean representation | Status |
|---|---|---|
| Uniform demand and replenishment | `noiseLaw`, `processKernel` | Proved measurable Markov model |
| Online hierarchical policy | `hierarchicalPolicy` | Constructed and measurable |
| Arbitrary initial inventory | `trajectoryFromState` | Exact fixed-state path law |
| Long-run RMS bound | `PolicyGuarantees.arbitraryInitialRMS` | Proved with explicit constant |
| Scheduled replacement | `initializationCost`, `initializedAverageCost` | Pathwise initialization plus post-refresh law |
| Every horizon `N >= 2m^2` | `PolicyGuarantees.initializedFiniteHorizon` | Proved |
| `O(log m)` implementation claim | `hierarchicalOperationCount_isBigO_log` | Internal accounting only; not compared |
| `O(m)` implementation claim | `hierarchicalMemoryCount_isBigO_linear` | Internal accounting only; not compared |

## Non-Compared Material

The development also contains finite count-state dynamics, symmetry,
stationary and balanced laws, energy estimates, quantile transport, continuous
trajectory bridges, and an internal `FullFormalization` bundle. These support
the target or document related results, but Comparator records only the
paper-facing upper-bound declaration.

The following source claims are not Palomar targets:

- the literature lower bound;
- the `Theta(log m / m)` optimality corollary;
- the balanced-initial-inventory corollary;
- the manuscript's implementation-complexity sentence;
- executable complexity certification.

## Closure

The default build imports `FD1D.V5.CompleteFormalizationAudit`. It applies
`assert_no_sorry` and checks that each audited dependency closure uses only:

- `propext`;
- `Classical.choice`;
- `Quot.sound`.

The proof development has zero placeholders and zero custom axioms.
`Challenge.lean` contains one conventional target `sorry`, which is excluded
from those counts and replaced by the proved declaration in `Solution.lean`.
