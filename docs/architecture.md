# Architecture

## Data flow

```text
CSV / in-memory arrays
  -> compact ChoiceDataset (event offsets and flat candidate arrays)
  -> fitted feature definitions and integer dictionaries
  -> coalesced event-to-latent incidence mappings B
  -> coupled Plackett--Luce factors and projected Gaussian messages
  -> independent ordered state-space chains
  -> Kalman filters and RTS smoothers
  -> generalized-EM kernel updates
  -> prediction, decomposition, and curve extraction
```

The layers are separate modules: `data`, `features`, `kernels`, `statespace`,
`likelihoods`, `inference`, `learning`, `prediction`, `progress`, and
`serialization`. NFL preprocessing belongs in examples, not the core.

## Numerical stack decision

The reference path uses NumPy/SciPy in float64: they provide auditable small
matrix algebra and dense checks without a heavyweight object per curve. The
interfaces use an array-backend boundary. Phase 6 will prototype JAX for
automatic differentiation of the low-dimensional M-step; Phase 8 will profile
before selecting Numba or native loops for filters and site sweeps. JAX is not
required in the core until compile latency, irregular batching, memory, and
serialization tradeoffs are measured. GPU acceleration is not assumed.

Immutable compact arrays store event offsets, winner-local indices, player IDs,
group IDs, feature values, curve IDs, node IDs, and incidence coefficients.
Sorting, deduplication, transformations, gaps, and incidence coalescing happen
once during preparation.

## Exactness boundary

Exact: supported kernel covariance-to-state-space representations and Gaussian
filter/smoother operations (up to floating-point arithmetic). Approximate: the
non-Gaussian event posterior, factor-to-chain projection, generalized-EM
surrogate, and integrated predictive probabilities. These labels must be
returned in diagnostics and documentation.

