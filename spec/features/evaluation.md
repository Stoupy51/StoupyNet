
# 📊 Evaluation

[Spec](../README.md) / Features / Evaluation

Scoring predictions with [metrics](../glossary.md#metric), per split and per [group](../glossary.md#group), with thresholds, calibration, plots and reports.
IDs: `EVA`.

## Groups

### <a name="eva-01"></a>EVA-01 Metrics per group
When [samples](../glossary.md#sample) belong to groups, every metric is also computed per group: the predictions of a group's samples combine into one prediction, and the metric scores the groups.
- A slide that yields 900 [tiles](../glossary.md#tile) then weighs as much as one that yields 40, and the score matches the decision a user takes, on a slide or on a patient.
- Group metrics sit beside the sample metrics, named after their [level](../glossary.md#level), such as `patient`.
- A split gets group metrics only when grouping changes something there: it holds several groups, and one of them at least holds several samples. A leave-one-out [fold](../glossary.md#fold), which validates on a single group, gets none, and a final evaluation that gets none says why.
- Sources: TS, HM. Priority: v1.

### <a name="eva-02"></a>EVA-02 Metrics at every level of nested groups
When groups nest, such as the [views](../glossary.md#view) of a slide and the slides of a patient, metrics are computed at every level the user names.
- Each level combines the predictions of the level below it: a patient's prediction combines those of its slides, so each slide counts once whatever its number of views.
- Sources: TS, HM, V. Priority: v1.

### <a name="eva-03"></a>EVA-03 How a group's predictions combine is a choice
The way the predictions of a group's samples combine is picked among several strategies, each stating the evidence it assumes, and the user can add new ones.
- Built in: the mean, for a signal spread over the whole group; the maximum, when one clear sample decides; the noisy-or, when each sample is weak and independent evidence; the share of samples above a threshold, when enough samples must agree.
- A strategy defined for binary tasks only stops with an error on any other task, and an unknown strategy stops with an error listing the known ones.
- Sources: TS. Priority: v1.

### <a name="eva-04"></a>EVA-04 By default, a group's prediction is the mean of its samples'
> [!IMPORTANT]
> Unless the user picks a strategy, a group's prediction is the mean of its samples' predictions, weighted as [EVA-05](#eva-05) states, so one wrong sample moves it by its share of the group at most.
- One noisy tile among the 100 of a negative slide moves the slide's score by 0.01 at most, instead of deciding it.
- A group of a single sample keeps that sample's prediction, so small groups still rank and calibrate.
- The mean holds for every task that has groups, binary or not.
- Sources: TS, HM, V. Priority: v1. Key behaviour.

### <a name="eva-05"></a>EVA-05 Overlapping samples count once per zone
> [!IMPORTANT]
> When the samples of a group overlap, such as views cut from one slide, each zone of the group weighs once in the group's prediction, however many samples cover it.
- Each sample weighs the area it alone covers, plus an equal share of each zone it shares with other samples, so combining the samples gives the result of combining a prediction map of the whole group.
- Positions are compared within a group only, since two slides have unrelated coordinates.
- Samples without a position all weigh the same, and positions that do not match the samples stop with an error.
- Guarantees [INV-05](../invariants.md#inv-05).
- Sources: TS. Priority: v1. Key behaviour.

### <a name="eva-06"></a>EVA-06 A group's label comes from its samples' labels
A group's [label](../glossary.md#label) derives from the labels of its samples: positive when one sample at least is positive in a binary task, the highest grade in an ordinal one.
- Samples of one group that disagree on an unordered class stop with an error naming the group, unless the user gives the rule.
- Folds stratify groups by this label ([SPL-03](splitting.md#spl-03)).
- Sources: TS, HM, V. Priority: v1.

## Decision thresholds

### <a name="eva-07"></a>EVA-07 Thresholds are learned on validation, per level
> [!IMPORTANT]
> Metrics that depend on a [decision threshold](../glossary.md#decision-threshold), such as accuracy or sensitivity, are also computed at a threshold learned on the validation split, one for the samples and one per level of groups.
- By default the threshold maximises the Youden index, which weighs sensitivity and specificity equally.
- Without a validation split, the threshold is learned on the [out-of-fold scores](../glossary.md#out-of-fold-score) of the cross-validation.
- The user can have it learned on another split, never on the test split ([INV-04](../invariants.md#inv-04)).
- Both readings stay side by side under distinct names, at the plain decision and at the learned threshold, and metrics that sweep every threshold, such as the area under the ROC curve, are computed once.
- The learned thresholds travel with the trained [model](../glossary.md#model) ([INF-01](inference.md#inf-01)).
- Task: binary classification.
- Sources: TS. Priority: v1. Key behaviour.

### <a name="eva-08"></a>EVA-08 The objective a threshold maximises is a choice
The user can have the threshold learned with another objective than the Youden index, such as a minimum sensitivity for a screening test.
- Task: binary classification.
- Sources: TS. Priority: planned.

