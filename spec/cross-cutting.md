
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

