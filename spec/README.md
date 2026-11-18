
# 📋 StoupyNet specification

What StoupyNet must do, whatever the organisation of its code.
[VISION.md](../VISION.md) states the goal and the principles, this folder states the capabilities, and the architecture, designed from both, decides how to build them.

## Files

| File | Holds | IDs |
|---|---|---|
| [glossary.md](glossary.md) | The vocabulary of the spec, and a map of how its terms relate | terms |
| [use-cases.md](use-cases.md) | Canonical use cases, each with its budget of user code | `UC` |
| [features/data.md](features/data.md) | Reading datasets of any format and structure, transforming and augmenting them | `DAT` |
| [features/splitting.md](features/splitting.md) | Training, validation and test splits, folds, groups | `SPL` |
| [features/training.md](features/training.md) | Fitting a model | `TRN` |
| [features/evaluation.md](features/evaluation.md) | Scoring predictions, per split and per group | `EVA` |
| [features/interpretation.md](features/interpretation.md) | Explaining predictions | `INT` |
| [features/search.md](features/search.md) | Searching hyperparameters | `SRH` |
| [features/tracking.md](features/tracking.md) | Recording and comparing runs | `TRK` |
| [features/inference.md](features/inference.md) | Using a trained run on new data | `INF` |
| [invariants.md](invariants.md) | Properties that falsify a result without any error once broken | `INV` |
| [cross-cutting.md](cross-cutting.md) | Requirements on the framework as a whole, each with a number or a check | `XC` |
| [interfaces.md](interfaces.md) | Formats read and written, external services | `IFC` |
| [extensions.md](extensions.md) | Kinds of addition the framework must welcome, and what each may cost | `EXT` |
| [open-questions.md](open-questions.md) | Questions still to settle | `Q` |

The files under `features/` follow the stages of a user's journey, to ease reading.
That order is no structure for the code: the architecture draws its boundaries from [extensions.md](extensions.md) and [invariants.md](invariants.md).

## Item format

```markdown
### <a name="spl-01"></a>SPL-01 Short title
The need, in one sentence.
- What that sentence cannot hold.
- Guarantees [INV-01](../invariants.md#inv-01).
- Sources: TS, HM. Priority: v1.
```

- The heading holds an anchor, the ID and a short title. Other files link to an item with `<file>#<ID in lowercase>`.
- The first line states the item in one sentence, the one a search lands on.
- Bullets add what that sentence cannot hold: conditions, examples, the errors it raises, what it may cost, and links to the items it relies on. [SPL-01](features/splitting.md#spl-01) holds its impossible fold counts and its memory bound this way.
- A feature item names the invariants it keeps with `Guarantees`, and an invariant names the items that keep it with `Kept by`.
- An invariant adds a bullet starting with `Broken,` that says which result turns wrong without it, as in [INV-02](invariants.md#inv-02).
- The last bullet gives the sources and the priority.

A key behaviour, anything that significantly changes what a run does ([XC-01](cross-cutting.md#xc-01)), puts its statement in an `IMPORTANT` callout and ends its last bullet with `Key behaviour.`, as [SPL-02](features/splitting.md#spl-02) does:

```markdown
### <a name="spl-02"></a>SPL-02 Short title
> [!IMPORTANT]
> The behaviour, in one sentence.
- Sources: TS, HM, V. Priority: v1. Key behaviour.
```

## Tags

- **Sources.** `TS`: a capability of TilerSwift, a private deep learning framework. `HM`: a need of HistoMicIA, a private project built on it. `V`: the maintainers' vision. `L`: the literature or another framework.
- **Priority.** `v1`: implemented in the first version. `planned`: the architecture must welcome it without changing, and nothing of it is implemented in v1. `out`: outside the scope, with the reason.
- **Task or modality.** Named only when an item holds for some tasks or modalities, as in `Task: segmentation`. An item that names none holds for all of them.

## Writing rules

- An item states a need, never a mechanism: no class, module or file name appears outside its sources.
- One item holds one need, and two needs that can change independently are two items.
- Each item links to the items it relies on, and the first mention of a glossary term in each file of items links to its definition.
- An item is never deleted: one that leaves the scope takes the priority `out` with its reason, so its ID is never reused.
- No item is added, kept or dropped without the maintainers' agreement, once the case for and against has been made.

## Changing the spec

A new capability enters the spec before any code, with its ID, its tags and its place among the [extension scenarios](extensions.md).
A capability that fits no extension scenario calls for a review of the architecture, never for a patch beside it.

