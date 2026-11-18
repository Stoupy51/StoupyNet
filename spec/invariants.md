
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

### <a name="inv-07"></a>INV-07 A combination of models is never scored on what its models saw
A model that combines others, such as a cascade or an extractor followed by a classifier, is never scored on a group that one of its models trained or validated on.
- Training counts every use of a group, with or without its labels, such as a self-supervised pretraining, unless the user declares that an unlabeled use does not count, a choice the run records.
- Weights trained outside StoupyNet carry no list of the groups they saw, so the check covers the models StoupyNet trained.
- Broken, the combination's score is optimistic, since one of its models already saw the groups it is scored on.
- Kept by [INF-02](features/inference.md#inf-02).
- Sources: HM, V. Priority: v1.

### <a name="inv-08"></a>INV-08 A group keeps its split as the dataset changes
With the same seed and proportions, adding or removing groups never moves another group from one split to another, except a group moved by the rule that fills an empty split.
- Broken, a test group silently becomes a training or validation group, and a model trained after the change has seen what earlier models were tested on.
- Kept by [SPL-07](features/splitting.md#spl-07).
- Sources: HM, V. Priority: v1.

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

## Stored results

### <a name="inv-09"></a>INV-09 A stored result is reused only when nothing it came from changed
A prepared result is read back only when its source data, the code that computes it and every setting that shapes it, such as the size, the interpolation or the backbone, are the ones it was prepared with.
- Checking it reads no file content, and costs a small fraction of what the store saves.
- A dataset the user declares frozen is checked by its group folders alone ([DAT-12](features/data.md#dat-12)), which misses a file rewritten in place.
- Broken, a run trains on views composited with other settings, or from slides cleaned since, and nothing says so.
- Kept by [DAT-11](features/data.md#dat-11).
- Sources: TS, HM, V. Priority: v1.

