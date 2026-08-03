# References and provenance

Phase 1 reviewed these primary references conceptually:

* Maystre, Kristof, and Grossglauser, [Pairwise Comparisons with Flexible
  Time-Dynamics](https://arxiv.org/abs/1903.07746): factor-graph Gaussian
  approximation, state-space GP inference, and linear-time motivation.
* [Python Kickscore](https://github.com/lucasmaystre/kickscore): public API,
  covariance families, filtering/smoothing organization, and pairwise factor
  design.
* [Go Kickscore](https://github.com/lucasmaystre/gokick): compact implementation
  and deployment-oriented design lessons.

The Kickscore repositories are MIT-licensed, which is compatible with this
project's Apache-2.0 license when attribution/license obligations apply.
ChoiceState contains no copied or translated Kickscore source. Mathematical
facts (Kalman/RTS recursions, Matérn SDEs, softmax derivatives) are implemented
from published descriptions and independently tested. If future code is adapted,
the file, upstream commit, original license, and modifications must be recorded
here and in source headers.

The paper and both repositories focus on pairwise/outcome factors; their
algorithms do not establish equivalence for native winner-from-set likelihoods.
ChoiceState's coupled event factor and projection are therefore a new,
explicitly approximate design described in `mathematical-design.md`.

Environment note: during this run, direct HTTP retrieval was denied by the
execution environment (HTTP 403), so repository license text and detailed source
inspection could not be independently refreshed. The known MIT metadata must be
re-verified against the linked upstream LICENSE files before any adaptation or
release. This is an open Phase 1 provenance gate, not a claim that inspection
succeeded.

