# Kernel and state-space design

`StateSpaceKernel` exposes state dimension, measurement row, initial moments,
transition and process covariance for a nonnegative gap, direct covariance for
tests, domain validation, parameter metadata, and parameter derivatives. It is
immutable; constrained parameters are derived from bounded unconstrained
coordinates. Fixed/learnable status and explicit tying keys are metadata rather
than duplicated parameters.

Exact kernels planned for Phase 2:

| Kernel | State dimension | Notes |
|---|---:|---|
| Constant | 1 | deterministic state; zero process noise |
| Exponential / Matérn 1/2 | 1 | stationary OU representation |
| Matérn 3/2 | 2 | stationary companion SDE |
| Matérn 5/2 | 3 | stationary companion SDE |
| Wiener | 1 | nonstationary, domain origin recorded |
| Affine | 2 | random intercept and slope, deterministic transition |
| Piecewise constant | configured | explicit change-point increments |
| Sum | sum of dimensions | block transition/covariance; concatenated H |

`Matern12`, `Matern32`, and `Matern52` alias `Matern(nu=.5, 1.5, 2.5)`.
Unsupported `nu` values fail clearly. Kernel addition is supported;
multiplication is not promised. Periodic exponential is deferred until its
construction and conditioning are independently verified and, if introduced,
will be experimental.

Nodes are sorted and deduplicated per curve. Zero gaps use identity transition
and exact zero process covariance. Stable formulas/series handle tiny gaps;
long stationary gaps approach stationary covariance. Covariances are explicitly
symmetrized only to remove roundoff. PSD-aware factorizations support genuinely
deterministic components; jitter is a last, recorded stabilization with kernel,
curve, node, eigenvalue, and attempted magnitude in diagnostics.

The filter uses square-root or Joseph-form covariance updates selected after
Phase 2 numerical comparison. RTS output includes state means, state second
moments, and lag-one cross moments. Dense covariance and Gaussian-conditioning
tests cover irregular/repeated coordinates, extreme gaps, and stationary
invariance in float64 before float32 is enabled.

