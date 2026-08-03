# Inference and delivery risk register

| Risk | Consequence | Mitigation / acceptance gate |
|---|---|---|
| Scalar projection loses event-induced cross-chain dependence | biased uncertainty/sites | full `BᵀWB` locally; dense oracle; synthetic calibration; document approximation |
| Local Laplace is not EP moment matching | inaccurate skewed tilted moments | label precisely; compare quadrature on tiny events; consider deterministic cubature |
| Site precision becomes negative after cavity division | unstable filters | natural damping, rollback, diagnostics; never silently clip |
| Softmax gauge and additive offsets | singular/drifting decompositions | stored centering operators, rank tests, reject redundant constants/intercepts |
| Deterministic kernel states | Cholesky failure | PSD-aware algebra and recorded stabilization; extreme-gap tests |
| Generalized EM surrogate does not track predictive quality | misleading convergence | monotonic fixed-q line search plus held-out log score and outer diagnostic |
| Parameter tying is applied inconsistently | invalid gradients | central registry and finite-difference tests for tied and untied cases |
| Python event/curve dispatch dominates | misses scaling goal | compact arrays first; profile Phase 7; compile only measured hotspots |
| Millions of stored dense event sites | excessive memory | scalar natural messages, chunked rebuilds, memory benchmarks |
| `prior_only` semantics under shared nodes | understated uncertainty | restrict policy and test predictive variance/component accounting |
| Unseen labels | accidental leakage or arbitrary reuse | explicit prediction policy and persisted dictionaries |
| float32 cancellation | invalid covariance/site updates | float64 default; equivalence gates before opt-in float32 |
| Reference licensing/provenance | accidental code derivation | clean implementation; provenance ledger; no copied source or tests |

