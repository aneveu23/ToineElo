# ChoiceState

ChoiceState is a from-scratch Python library for fast Bayesian dynamic
winner-from-set choice models. It is currently in **Phase 1: mathematical and
architectural design**; no inference API is claimed yet.

The intended backend combines exact finite-dimensional state-space
representations of supported Gaussian-process kernels with approximate
inference for native Plackett--Luce event factors. See the
[mathematical design](docs/mathematical-design.md), [architecture](docs/architecture.md),
and [roadmap](docs/roadmap.md).

```bash
python -m pip install -e '.[dev]'
pytest
ruff check .
mypy src
```

The project is independent of ChoiceGP Dynamics. No code or dependency from
that project is used.

