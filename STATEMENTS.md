# Mathematical Statement Map

The authoritative Palomar statement is `Challenge.lean`. The completed proof
is exported by:

`FD1D.V5.Palomar.optimalDynamicMatchingUpperBound`

This document maps that declaration to Theorem 1 of *Optimal fully dynamic
matching on the line*.

## Model

For a natural number `m`:

- `Inventory m = Fin m -> unitInterval` is a labeled multiset of `m` supplies.
- `Noise = unitInterval x unitInterval` contains the current demand and the
  replacement supply.
- `noiseLaw` is the product of two uniform probability measures.
- `OnlinePolicy m` selects a live label from the current inventory and demand.
  Its selection and coordinate-update maps are measurable.
- `step P s z` deletes the selected coordinate and replaces it with the second
  noise coordinate.
- `trajectoryFromState P s0` is the homogeneous Markov path law from a fixed
  initial inventory.

The Lean policy is memoryless in the current state and demand. This is a
special case of the manuscript's policies, which may use the full history.

## Cost Clause From Arbitrary Initial Inventory

The one-period distance is:

```text
processCost P z =
  |z.inventory (P.select z.inventory z.demand) - z.demand|
```

`trajectoryRMSCostFromState P s0 t` is the square root of the expected square
of this distance at path time `t`. Path time zero corresponds to the
manuscript's first policy period.

For every `m >= 2`, the target produces a policy `P` such that, for every
fixed `s0`,

```text
limsup_t trajectoryRMSCostFromState P s0 t
  <= universalConstant * log(m+1) / m,
```

where:

```text
universalConstant = 72000 / log 2.
```

This is `PolicyGuarantees.arbitraryInitialRMS`. It represents part (i) of the
manuscript theorem and strengthens its existential constant by fixing an
explicit value.

## Initialized Finite-Horizon Clause

`initializationCost initial demand` is the sum of the first `m` distances when
the `j`-th initialization demand is matched to the `j`-th original labeled
supply. Every original supply is therefore replaced exactly once, and all
retained replenishments are iid uniform.

`uniformTrajectory P` is the independent post-initialization trajectory from
the resulting iid uniform inventory. For every `N >= 2m^2`, every original
inventory, and every fixed vector of initialization demands, the target proves:

```text
integral (initializedAverageCost P initial demand N)
  d(uniformTrajectory P)
  <= universalConstant * log(m+1) / m.
```

This is `PolicyGuarantees.initializedFiniteHorizon`. Universal quantification
over the fixed initialization demands is stronger than averaging over their
uniform law. Modeling the post-refresh trajectory separately is equivalent in
distribution because all retained replenishments and later noise coordinates
are independent and uniform.

The quantity divides initialization cost plus `N-m` policy-period costs by
`N`, exactly representing the manuscript's full horizon.

## Policy Quantifiers

The target structure has:

```text
forall m, 2 <= m ->
  exists P : OnlinePolicy m, PolicyGuarantees m P.
```

Thus the policy may depend on `m`, as in the manuscript. Each witness is the
hierarchical policy defined in `FD1D.V5.PaperStatements`, with regularization
and tree depth:

```text
regularization m = 2000 * Nat.clog 2 (m+1)
treeDepth m = floor(log_2(max(1, m / regularization m)))
```

The proof bridge identifies its Markov trajectory and costs definitionally
with the completed V5 continuous-process development.

## Resource Accounting Is Not Compared

The internal development defines:

```text
operationCount m = 20 * (treeDepth m + 1)
memoryCount m    = m + 4 * 2^(treeDepth m).
```

`Complexity.lean` proves explicit inequalities and the asymptotic claims:

```text
operationCount = O(log m)
memoryCount    = O(m).
```

These are mathematical real-arithmetic accounting functions for a root-to-leaf
implementation. They are not fields of the compared Palomar theorem because
the development does not supply an executable operational semantics connecting
evaluation of the noncomputable `OnlinePolicy` object to an exact number of
machine steps or memory words.

## Omitted Claims

The compared declaration does not claim:

- the lower bound from Kanoria's *Dynamic spatial matching*;
- the manuscript's resulting `Theta(log m / m)` optimality corollary;
- the corresponding optimal long-run expected-cost corollary;
- the balanced-initial-inventory corollary;
- the manuscript's implementation-complexity sentence;
- executable runtime verification.

Several supporting results and the balanced count-law estimate are present in
the proof development, but they are not Palomar targets.
