# Small implementation roadmap

1. **Phase 2 -- foundations:** kernel protocol and required exact kernels;
   filter/RTS sufficient statistics; dense covariance and gradient oracles.
2. **Phase 3 -- representation:** compact dataset, fitted transforms, missing
   policies, sharing/tying registry, node construction, coalesced sparse `B`.
3. **Phase 4 -- likelihood:** stable Plackett--Luce derivatives, dense global
   Laplace reference, cavity-Laplace projection and quadrature comparisons.
4. **Phase 5 -- inference:** scheduled site sweeps, chain smoothing, damping,
   callbacks, live progress, convergence history, synthetic recovery.
5. **Phase 6 -- learning:** fixed-q state-prior generalized EM, bounded/tied
   gradients, finite differences, initial-state contributions.
6. **Phase 7 -- API/data:** CSV diagnostics, prediction/curve APIs, unknown-label
   policies, versioned save/load.
7. **Phase 8 -- optimization:** benchmark parsing, sites, smoothing, M-step,
   outer iteration, prediction, serialization, memory, and scaling; profile and
   compile only proven bottlenecks.
8. **Phase 9 -- communication:** methods/API docs, generic and NFL examples,
   Colab with progress, measured runtime-estimation guidance.

Every phase ends with pytest, Ruff, mypy, exact/approximate status, performance
where meaningful, and unresolved-risk review. Foundational tests are not relaxed
to advance a phase.

