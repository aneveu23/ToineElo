# Mathematical design

## Model and identifiability

For event \(n\), candidate \(j\) has

\[
u_{nj}=f_{p_{nj}}(t_n)-\sum_r h_{r,g_{r,nj}}(x_{r,nj})+q_{nj}^T\beta,
\qquad
p(y_n=j\mid u_n)=\operatorname{softmax}(u_n)_j.
\]

Each nonparametric feature is one-dimensional and additive. The initial
identifiability convention is **reference-distribution centering**: after every
posterior update, each feature family is represented as a centered variation
whose weighted mean over its fitted training nodes is zero. Its removed constant
is not transferred to another latent component. This gauge transformation is
also applied to posterior summaries and predictions using stored training
weights. Player ability has a zero population intercept convention; an explicit
fixed reference component is required if parametric intercepts are enabled.
Because event probabilities ignore a common utility offset, centering changes
decompositions but not probabilities. Constant kernels in centered feature
components are redundant and will be rejected; conflicting intercepts produce
an error rather than jitter. Phase 3 tests will verify the constraint rank and
invariance for arbitrary feature counts.

## A coupled event and its incidence matrix

Let \(z\) contain each **distinct** scalar latent node touched by an event once.
A sparse \(B\in\mathbb R^{C\times L}\) maps nodes to utilities: \(u=Bz+q\).
Player entries have coefficient +1 and configured subtractive feature entries
have coefficient -1. If two candidates share a feature curve at the same input,
they point to the same column. If a candidate's construction reaches the same
node twice, coefficients are summed. Thus repeated variables are coalesced
before derivatives, never treated as independent copies.

With \(p=\operatorname{softmax}(u)\),

\[
g_u=e_y-p,\quad W_u=\mathrm{diag}(p)-pp^T,\quad
g_z=B^Tg_u,\quad W_z=B^TW_uB.
\]

The rank-deficient common-shift direction is expected. We retain the full
\(-pp^T\) coupling; diagonalizing \(W_u\) would redefine the likelihood and is
not an accepted fast path.

## Dense reference

For small data, concatenate all latent nodes, assemble their exact dense prior
covariance, and optimize the true log posterior
\(\sum_n\log\operatorname{softmax}(B_nz+q_n)_{y_n}-\tfrac12z^TK^{-1}z\).
Its Laplace covariance is the inverse full posterior negative Hessian. This
retains cross-event and cross-chain curvature and is the correctness oracle,
not the scalable implementation.

## Scalable cavity-Laplace projection

The scalable approximation stores scalar Gaussian natural-parameter messages
from event factors to latent nodes, while every latent chain combines those
messages exactly with its state-space prior.

For one event, remove its old messages to obtain independent-chain Gaussian
cavity marginals for the distinct \(z\): \(q^{\setminus n}(z)=N(m,V)\). `V` may
contain covariance between nodes on the same chain; cross-chain cavity blocks
are zero under the approximation. We then find the mode of

\[
\log \tilde p_n(z)=\log p(y_n\mid B_nz+q_n)
-\tfrac12(z-m)^TV^{-1}(z-m)
\]

by safeguarded Newton iterations using the complete \(B^TW B\). The local
Gaussian covariance is \(S=(V^{-1}+B^TWB)^{-1}\). This is a local Laplace
approximation to the tilted distribution, **not exact EP moment matching**.

To return messages to independent chains, match each chain-block marginal of
the local Gaussian and divide by its cavity block in natural coordinates.
Within-chain block sites are reduced to node messages using a documented
information projection that matches marginal means/variances; damping is
applied in natural space. Negative or non-finite projected precisions trigger
adaptive damping, then rejection with diagnostics--not silent clipping. The
event calculation therefore respects off-diagonal softmax curvature, while the
projection discards posterior correlations between distinct chains (and any
off-diagonal block information not representable by scalar messages). That is
the principal scalability approximation. A later tested option may retain
small multivariate event sites, but will be explicitly named.

Convergence reports cavity failures, rejected sites, maximum and percentile
natural-parameter change, approximate energy, and sweep count. Chunking changes
update scheduling and memory only; it does not create minibatch-specific priors
or likelihoods.

## Hyperparameter objective

Learning uses generalized EM. The E-step freezes the converged approximate
posterior moments from site sweeps and RTS smoothing. The M-step maximizes the
expected complete state-space prior log density

\[
\mathcal Q(\theta)=\sum_c E_q[\log p_\theta(x_{c,0})
+\sum_i\log p_\theta(x_{c,i+1}\mid x_{c,i})],
\]

including initial-state terms. Expectations use smoothed means, covariances,
and lag-one cross moments. Gradients pass through \(A_\theta(\Delta)\),
\(Q_\theta(\Delta)\), and \(P_{0,\theta}\), with posterior moments held fixed.
Parameters tied across curves/features sum their contributions before one
bounded optimizer update. This is a **generalized-EM surrogate**, not an exact
marginal likelihood, EP evidence, or ELBO; accepted steps must increase the
fixed-E-step surrogate, followed by a fresh E-step. No differentiation through
site fixed points is implied. Phase 6 will test whether a computable global
Laplace/EC energy is sufficiently reliable as an outer convergence diagnostic.

## Prediction

State interpolation/extrapolation yields component Gaussian moments. The first
API labels `softmax(posterior_mean)` as a plug-in probability. Optional
quasi-Monte-Carlo integration at prediction time will preserve candidate
correlation and be labeled posterior-integrated. Credible intervals refer to
latent utilities/components, not categorical probabilities unless explicitly
computed.

