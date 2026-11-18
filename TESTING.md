
# 🧪 Testing

A test exists to protect a promise of the [specification](spec/README.md) that users rely on.
This page says when a test is worth writing, how it names what it protects, and where it runs.

## When a test is worth writing

A test is worth its cost when all three of these hold:

1. It protects an item of the spec, such as a feature, an [invariant](spec/invariants.md), a [use case](spec/use-cases.md) or a [cross-cutting requirement](spec/cross-cutting.md), and reaches it through the public API.
2. A plausible change could break that item silently, or in a way that takes long to diagnose.
3. It is deterministic, and as fast as its tier allows: CPU, a tiny input, a fixed seed.

The worst bugs of a learning framework give wrong numbers without crashing: a group on both sides of a split, a metric averaged over the wrong samples, an out-of-fold score given by a model that trained on the sample.
Tests against those come first.

## What is not worth a test

- Restating the implementation, such as asserting which internal function was called or the shape of an intermediate tensor.
- Checking what a dependency guarantees, such as PyTorch, Lightning or timm behaving as documented.
- Dataclass fields, trivial getters and constant values.
- Freezing an internal detail so tightly that a refactor breaks the test without any bug.
- Repeating what a doctest already checks.

Tests use real objects.
Only input and output at the boundary, such as the network or a remote service, may be replaced by a fake, and StoupyNet's own code never is.

## Every test names what it protects

A test carries the IDs of the spec items it protects:

```python
@pytest.mark.spec("INV-02", "SPL-04")
def test_a_group_never_crosses_a_split() -> None:
	...
```

The suite checks, on every run, that:

- every test carries the marker, and every ID it names exists in the spec;
- every [invariant](spec/invariants.md) and every [use case](spec/use-cases.md) has at least one test.

A marker is read by the tooling without parsing any text, and a misspelt marker name fails the run.
Doctests carry no marker: they document the function they sit in, and protect that function.

A test that protects no item is a candidate for removal, decided by the maintainers.
A behaviour that changes or leaves the spec takes its tests with it, in the same change.

## Two tiers

| Tier | How it runs | What it holds | Budget |
|---|---|---|---|
| Light | `pytest`, locally and on GitHub Actions | Tests on the CPU with tiny synthetic inputs, without network, pretrained weights or real data | [XC-02](spec/cross-cutting.md#xc-02) |
| Heavy | `pytest -m heavy`, by hand and rarely, never on GitHub Actions | Tests that need pretrained weights, real data, the network or a GPU | [XC-03](spec/cross-cutting.md#xc-03) |

- A light test that takes more than 20 seconds is made smaller, or moves to the heavy tier.
- A heavy test whose resource is missing, such as a GPU, the network or a login for gated weights, is skipped with that reason, so the summary of the run shows what went unchecked.
- Weights and data that heavy tests download stay in the cache of the library that fetched them, never in the repository.

## Kinds of tests

These kinds are expected, and the architecture settles how each one is organised:

- **Invariant tests**, at least one per [invariant](spec/invariants.md).
- **Contract tests**, run on every registered implementation of an extension point, so that a new loss or backbone is tested without anyone writing a test for it.
- **Use-case tests**, each running a [canonical use case](spec/use-cases.md) end to end on tiny data.
- **Numerical regression tests**, comparing a run with a fixed seed, on the CPU and a tiny input, against a stored result.
- **Architecture tests**, guarding the dependency rules and the conventions of the code.
- **Documentation tests**: doctests, and links that resolve.

