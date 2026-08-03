# Generic feature-system design

`CurveFeature` is declarative: name, input source/scope, sharing resolver,
kernel, fitted transform, missing policy, centering convention, and optional
parameter tying key. Adding a feature changes data, not inference code.

Candidate arrays are flat and aligned to candidate IDs. Event scalars are
explicitly tagged and broadcast during preparation; ambiguous 2-D/ragged shapes
are rejected. Sharing accepts `global`, `player`, one categorical field, a tuple
of fields, a precomputed compact group ID, or a serializable resolver protocol.
The default is one shared population curve (optionally grouped), never a curve
per player. Kernel parameters default to one set per feature; explicit tying can
cross features, and explicit group parameter maps can separate selected groups.

Transforms implement `fit(training_values)`, `transform(values)`, and versioned
`to_dict`; built-ins are identity, standardization, min-max, log1p composition,
and negation. Fit is called only on the training split. Serialized state includes
statistics, domain checks, and dtype. User transforms must satisfy the fitted,
serializable protocol.

Missing policies are explicit:

* `error`: fail with event/candidate/feature context.
* `drop_event`: remove the whole event and record it.
* `impute_constant`: use a configured value before fitting/transforming.
* `indicator`: impute a configured value and add an explicit parametric missing
  indicator contribution.
* `prior_only`: omit that candidate-feature incidence; this is allowed only when
  the documented semantics are “unknown contribution with prior mean”, and
  prediction variance must include the corresponding prior uncertainty.

Mappings preserve training labels and define prediction policies (`error`,
`prior` where valid, or explicit unknown bucket). Serialization stores mapping
versions and preprocessing counts. One-dimensional additive curves are the only
nonparametric feature claim; interactions require user-constructed features.

