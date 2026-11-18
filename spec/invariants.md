
# 🛡️ Invariants

[Spec](README.md) / Invariants

Properties that falsify a result without raising any error once broken, such as a [group](glossary.md#group) present on both sides of a [split](glossary.md#split).
Each invariant is protected by at least one dedicated test ([TESTING.md](../TESTING.md)).
IDs: `INV`.

## Splits and folds

### <a name="inv-01"></a>INV-01 Every sample validates in exactly one fold
The validation parts of a cross-validation partition its training split: each [sample](glossary.md#sample) validates in exactly one [fold](glossary.md#fold) and trains in all the others.
- Broken, some samples are never scored out of fold, or are scored twice, and the pooled [out-of-fold score](glossary.md#out-of-fold-score) misreads the model.
- Kept by [SPL-01](features/splitting.md#spl-01).
- Sources: TS. Priority: v1.

### <a name="inv-02"></a>INV-02 No group on both sides
No group has samples on two sides of a split, nor on both sides of a fold.
- A split has three sides, training, validation and test, and a fold has two, training and validation.
- Broken, the score overrates the model: one that trained on views of a slide recognises the slide's other views.
- Kept by [SPL-02](features/splitting.md#spl-02).
- Sources: TS, HM, V. Priority: v1.

### <a name="inv-03"></a>INV-03 Cutting groups differently leaves the folds unchanged
Two runs whose samples cut the same groups differently, such as [views](glossary.md#view) at another scale or with another overlap, get the same folds from the same seed.
- Broken, a search ranks its [trials](glossary.md#trial) on different validation data, and the best trial may only have drawn easier folds.
- Kept by [SPL-04](features/splitting.md#spl-04).
- Sources: TS. Priority: v1.

### <a name="inv-06"></a>INV-06 Resampling stays on the training side
Undersampling, oversampling and synthetic samples apply to the training side of a split or a fold once it is drawn, and every reported score covers all the samples of its part at their real proportions.
- A cheaper validation during training is allowed when it is drawn once, independently of the training settings, and the reported scores are then computed on the whole part.
- Broken, precision, F1 and calibration describe the resampled data instead of the real one, and synthetic samples made from training samples leak into validation ([Santos et al., 2018](https://doi.org/10.1109/MCI.2018.2866730)).
- Kept by [DAT-07](features/data.md#dat-07).
- Sources: TS, HM, L. Priority: v1.

### <a name="inv-07"></a>INV-07 A group that tests one model never trains another
Within a [project](glossary.md#project), a group on the test side of one model is on the training or validation side of none.
- Broken, a cascade that feeds the output of one model to another is scored on groups one of its models trained on, and two models scored on different groups cannot be compared.
- Kept by [SPL-06](features/splitting.md#spl-06).
- Sources: HM. Priority: v1.

## Decisions

### <a name="inv-04"></a>INV-04 Nothing a run decides reads the test split
No decision of a [run](glossary.md#run) reads the test split: neither a [decision threshold](glossary.md#decision-threshold), nor the epoch training stops at, nor the model or the hyperparameters it keeps.
- Broken, the test score measures the tuning as much as the model.
- Kept by [EVA-07](features/evaluation.md#eva-07).
- Sources: TS, V. Priority: v1.

## Groups

### <a name="inv-05"></a>INV-05 A zone counts once in its group
When the samples of a group overlap, every zone of the group carries the same weight in the group's prediction however many samples cover it, so a group's weights add up to the area its samples cover.
- Broken, the zones tiled most densely, often the richest ones, silently outweigh the rest of their group.
- Kept by [EVA-05](features/evaluation.md#eva-05).
- Sources: TS. Priority: v1.

