# Lean Dependency Map

The compared result is proved through the following dependency chain.

```text
V5.LocalPolicy
  -> V5.LocalInvariants
  -> V5.LocalBellman
  -> V5.TreePolicy
  -> V5.Dynamics
  -> V5.Energy
  -> V5.Transport
  -> V5.QuantileSquared
  -> V5.SquaredCost
  -> V5.CostBounds
```

The stochastic-process branch is:

```text
V5.ContinuousState
  -> V5.ContinuousProcess
  -> V5.InitialProcess
  -> V5.TrajectoryBounds
  -> V5.JoinedTrajectory
```

The publication endpoint combines these branches:

```text
V5.Parameters + V5.Complexity
             \
V5.JoinedTrajectory + V5.Balanced
             -> V5.Main

Mathlib-only V5.StatementModel + V5.Main
             -> V5.PaperStatements
             -> V5.CompleteFormalizationAudit
             -> Solution
```

`FD1D.V5.PaperStatements.hierarchicalPolicy` instantiates the compact policy
model with `spatialSelectedLabel`. Definitional bridge lemmas identify:

- the fixed-state trajectory law;
- the iid-initialized trajectory law;
- the one-period process cost;
- the RMS trajectory cost;
- the scheduled-initialization average cost.

The two cost fields are then discharged by:

- `FD1D.V5.limsup_trajectoryRMSCostFromState_le_log_succ`;
- `FD1D.V5.finite_horizon_expected_joinedTrajectoryCost_le_log_succ`.

`FD1D.V5.hierarchicalPolicy_resourceGuarantees` is audited as an internal
arithmetic accounting result but is not a field of the compared theorem.

`FD1D.V5.CompleteFormalizationAudit` checks the dependency closures of the
paper-facing theorem, both cost producers, the resource producer, and the
broader internal `FD1D.V5.full_formalization` bundle.
