
# ⚙️ Cross-cutting requirements

[Spec](README.md) / Cross-cutting

Requirements on the framework as a whole: ergonomics, performance, platforms, typing, tests, documentation, dependencies and licenses.
Each one gives a number or a check wherever one exists.
IDs: `XC`.

## Documentation

### <a name="xc-01"></a>XC-01 Key behaviours stand out
Anything that significantly changes what a [run](glossary.md#run) does is documented with a strong visual marker, and listed in a category of its own, apart from ordinary settings.
- A default that decides when training stops is a [key behaviour](glossary.md#key-behaviour); the value of a learning rate is an ordinary setting.
- The documentation shows each key behaviour in a highlighted callout wherever it appears, and gathers them all on one page.
- Sources: V. Priority: v1.

## Tests

### <a name="xc-02"></a>XC-02 The light tests stay light
The tests that run on GitHub Actions take under 5 minutes on a GitHub-hosted runner, installation excluded, and need no network, no pretrained weights, no real data and no GPU.
- Faster is better: 5 minutes is a ceiling, never a target.
- How tests are sorted into tiers: [TESTING.md](../TESTING.md#two-tiers).
- Sources: V. Priority: v1.

### <a name="xc-03"></a>XC-03 The heavy tests stay under 30 minutes
The tests that need pretrained weights, real data, the network or a GPU run by hand and rarely, never on GitHub Actions, and take under 30 minutes in total on the maintainer's machine.
- Sources: V. Priority: v1.

### <a name="xc-04"></a>XC-04 Every test names the items it protects
Every test names the spec items it protects, every named item exists, and every invariant and every use case has at least one test, all checked automatically on each run.
- How: [TESTING.md](../TESTING.md#every-test-names-what-it-protects).
- Sources: V. Priority: v1.

