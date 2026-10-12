# FlowMessagePassingRules

[![CI](https://github.com/ReactiveBayes/FlowMessagePassingRules.jl/actions/workflows/CI.yml/badge.svg?branch=main)](https://github.com/ReactiveBayes/FlowMessagePassingRules.jl/actions/workflows/CI.yml)
[![Docs: stable](https://img.shields.io/badge/docs-stable-blue.svg)](https://reactivebayes.github.io/FlowMessagePassingRules.jl/stable/)
[![Docs: dev](https://img.shields.io/badge/docs-dev-blue.svg)](https://reactivebayes.github.io/FlowMessagePassingRules.jl/dev/)
[![Coverage](https://codecov.io/gh/ReactiveBayes/FlowMessagePassingRules.jl/graph/badge.svg)](https://codecov.io/gh/ReactiveBayes/FlowMessagePassingRules.jl)
[![Code style: Runic](https://img.shields.io/badge/code_style-%E1%9A%B1%E1%9A%A2%E1%9A%BE%E1%9B%81%E1%9A%B2-black)](https://github.com/fredrikekre/Runic.jl)

The `Flow` node for the ReactiveMP ecosystem: the deterministic node `out = f(in)` for an
invertible normalising-flow model `f`, with the models it is built from. A model is a tuple of
layers, `FlowModel`, given its parameters by `compile`: additive coupling layers holding planar
or radial flows, and permutation layers. The node's rules push a multivariate normal through the
compiled model, forwards or backwards, by linearisation or by the unscented transform, under
`FlowApproximation`, which a model must give.

```julia
using FlowMessagePassingRules, MessagePassingRulesBase, ExponentialFamily

model = compile(FlowModel(2, (AdditiveCouplingLayer(PlanarFlow()),)))
m_in = MvNormalMeanCovariance([1.0, 2.0], [1.0 0.0; 0.0 1.0])
result = @call_message_update_rule(node = Flow, target = :out, m = (in = m_in,), algorithm = FlowApproximation(model))
getresult(result)   # an MvNormalMeanCovariance, the linearised image of m_in
```

```julia
import Pkg; Pkg.add("FlowMessagePassingRules")
```

- Documentation: <https://reactivebayes.github.io/FlowMessagePassingRules.jl/stable/>.
  `make docs` builds it locally, into `docs/build`.
- Tests: `make test` runs the suite as CI does, ending with the rule-coverage gate: every rule
  must be selected by some test. `make test test_args="name:rules"` selects items by tag, by
  name or by file; `make help` lists the other targets.
- Depends on MessagePassingRulesBase and MessagePassingRulesApproximations, with BayesBase,
  ExponentialFamily, Distributions, LinearAlgebra, Random and TupleTools. Julia 1.10 or later.
  MIT licence.
