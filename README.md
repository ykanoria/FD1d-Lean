# Optimal Fully Dynamic Matching on the Line in Lean

This repository formalizes the constructive upper bound from Yash Kanoria's
manuscript *Optimal fully dynamic matching on the line*. It is a standalone
Lean 4 project whose only direct dependency is Mathlib.

## Main Result

The Palomar-facing theorem is:

`FD1D.V5.Palomar.optimalDynamicMatchingUpperBound`

For every inventory size `m >= 2`, it constructs a measurable online policy
for a fixed inventory of `m` labeled supplies in `[0,1]`, with independent
uniform demand and replenishment in every period. With

`C = 72000 / log 2`,

the theorem proves:

1. From every fixed initial inventory, the limsup root-mean-square
   one-period matching distance is at most `C log(m+1)/m`.
2. After matching once to each original supply during an `m`-period
   initialization, expected average cost is at most `C log(m+1)/m` for every
   horizon `N >= 2m^2`.

The policy used in the proof is the manuscript's hierarchical dyadic selector.
Its deletion probabilities are produced recursively, demand is routed by
quantile transport, and replenishment updates the selected coordinate.

## Proof Structure

The proof development establishes local policy feasibility and a quadratic
Bellman inequality, lifts them through the dyadic tree, proves energy and
transport estimates, and connects finite count dynamics to a measurable
continuous-coordinate Markov trajectory. Finite-chain convergence gives the
arbitrary-initial-state limit. A pathwise scheduled replacement argument gives
the finite-horizon bound.

`DEPENDENCY-MAP.md` gives the corresponding Lean module graph, and
`STATEMENTS.md` maps every advertised clause to the statement vocabulary.

## Build

Install [elan](https://github.com/leanprover/elan), then run:

```sh
lake exe cache get
lake build
```

Lean is pinned in `lean-toolchain`; Mathlib and its transitive dependencies are
pinned in `lake-manifest.json`.

## Verification

`FD1D/V5/CompleteFormalizationAudit.lean` is part of the default build. It
rejects proof placeholders and dependencies on axioms other than:

- `propext`
- `Classical.choice`
- `Quot.sound`

The proof development contains no `sorry`, `admit`, custom `axiom`,
`postulate`, or `native_decide`. The single `sorry` in `Challenge.lean` is the
conventional Palomar statement hole. On Linux, run the independent statement
comparison and NanoDa kernel replay with:

```sh
./scripts/verify-comparator.sh
```

Publication metadata can be checked with:

```sh
ruby scripts/validate-formalization.rb
check-jsonschema \
  --schemafile https://raw.githubusercontent.com/mathlib-initiative/formalization.yaml/99c678e569c7c4c0772db297c5ddd5e4c9b6322e/schema/formalization.schema.json \
  formalization.yaml
sha256sum --check manuscript/SHA256SUMS
```

## Palomar Layout

- `Challenge.lean`: independent Mathlib-only statement
- `Solution.lean`: completed proof-side export
- `comparator.json`: target and permitted standard axioms
- `formalization.yaml`: authorship, provenance, automation, scope, and fidelity
- `STATEMENTS.md`: prose-to-Lean statement map
- `RELEASE-CHECKLIST.md`: immutable-release procedure

The challenge is 196 lines and under 8 KiB, below Palomar's preferred review
thresholds.

## Scope and Fidelity

The stochastic cost guarantees are linked to the concrete hierarchical
selector and its path-space law. The manuscript's implementation-complexity
sentence is outside the compared theorem. Internal modules prove asymptotic
bounds for declared operation and memory accounting functions, but there is no
operational cost semantics linking those counters to execution of the
noncomputable Lean policy.

The manuscript's lower bound comes from prior literature and is not formalized.
Consequently, its `Theta(log m / m)` optimality corollary is outside the
Palomar target. The balanced-initial-law corollary is also not compared.
`formalization.yaml` and `STATEMENTS.md` record the remaining representational
details.

## Source and Production

The named TeX source in `manuscript/` is a byte-for-byte copy from
`optimal_dynamic_matching_bundlev5.zip`; its checksum and the original bundle
digest are recorded in `manuscript/README.md`. The anonymous review wrapper
and generated PDFs are omitted. The manuscript has no external DOI or arXiv
identifier recorded as of 2026-09-11.

The manuscript discloses substantive OpenAI ChatGPT and Codex assistance in
the mathematical policy, proof, algebra checks, and drafting. Codex also
assisted with the Lean formalization and publication audit. No independent
human peer review of the Lean code is claimed.

## Licence

Copyright 2026 Yash Kanoria.

This repository is released under Apache-2.0. See `LICENSE` and `NOTICE`.

Before Palomar registration, use a public immutable commit and submit its full
40-character SHA at <https://submit.palomar-registry.org/>.
