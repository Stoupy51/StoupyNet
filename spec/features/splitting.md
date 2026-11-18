
# ✂️ Splitting

[Spec](../README.md) / Features / Splitting

Dividing data into training, validation and test parts ([splits](../glossary.md#split)) and into cross-validation [folds](../glossary.md#fold), keeping each [group](../glossary.md#group) together, and holding test sets fixed in advance.
IDs: `SPL`.

## Training, validation and test splits

### <a name="spl-05"></a>SPL-05 Splits are drawn per group, class by class
The training, validation and test splits are drawn group by group within each class, in the proportions the user asks for, so that every split keeps the class distribution.
- The group kept whole is the one folds keep whole ([SPL-02](#spl-02)), such as the patient, and it counts under its own [label](../glossary.md#label) ([EVA-06](evaluation.md#eva-06)).
- Each requested split gets one group at least, and a request that cannot be filled, such as three splits from two groups, stops with an error.
- A class that holds no group stops with an error, instead of yielding an empty split that trains on nothing.
- A validation share of zero holds no validation split, which a cross-validation then replaces ([SPL-01](#spl-01)).
- Guarantees [INV-02](../invariants.md#inv-02): no group on two sides of a split.
- Sources: TS, HM. Priority: v1.

### <a name="spl-06"></a>SPL-06 A test set fixed in advance
The user can fix the test set in advance, such as the slides of another center or a list of patients, and the drawn splits then share the rest between training and validation.
- Every model tested on it is then scored on the same groups, whatever the seed or the proportions of its own splits.
- A listed slide brings every slide of its patient into the test set, and a listed name that matches no group stops with an error.
- A class absent from the fixed test set stops with an error, instead of scoring the model on the classes it happens to hold.
- The run then records a drawn test share of zero, so it never reports a proportion it did not apply.
- Sources: TS, HM. Priority: v1.

### <a name="spl-07"></a>SPL-07 A group's split depends on that group alone
> [!IMPORTANT]
> The split a group lands in follows from its name and the seed alone, so adding or removing other groups never moves it.
- No order of the names, such as the arrival order of the slides, can bias the draw.
- The proportions hold on average rather than exactly: 100 groups drawn at 20 % give about 16 to 24 test groups.
- A class whose draw leaves a requested split empty moves into it the group whose draw comes closest, so that each requested split gets one group at least. The run lists the groups it moved, since such a group returns to its own split once its class grows.
- Guarantees [INV-08](../invariants.md#inv-08).
- Sources: HM, V, L ([Géron, 2022](https://github.com/ageron/handson-ml3), chapter 2). Priority: v1. Key behaviour.

## Cross-validation

### <a name="spl-01"></a>SPL-01 K folds, or one fold per group
A cross-validation divides the training split into K parts, or into one part per group for leave-one-out, and each fold validates on one part after training on all the others.
- K counts groups: 5 folds over 40 patients validate on 8 patients each, however many slides each patient has.
- Asking for fewer than two folds, or for more folds than there are groups, stops with an error that states the limit, which the rarest class sets when the folds are stratified ([SPL-03](#spl-03)).
- The folds refer to the [samples](../glossary.md#sample) of the training split without copying them, so a cross-validation holds the dataset in memory once, whatever K.
- Guarantees [INV-01](../invariants.md#inv-01): every sample validates in exactly one fold.
- Sources: TS, HM. Priority: v1.

### <a name="spl-02"></a>SPL-02 Folds keep each group whole
> [!IMPORTANT]
> Folds are drawn over groups, never over samples: all the samples of a group validate in the same fold.
- The user names the group that folds keep whole, such as the patient rather than one of the patient's slides, since a model that trained on one slide of a patient recognises the others.
- That group can be wider than the groups [metrics](../glossary.md#metric) are reported for ([EVA-02](evaluation.md#eva-02)).
- A dataset that names no group makes each sample its own group, so leave-one-out then leaves out one sample at a time.
- Guarantees [INV-02](../invariants.md#inv-02): no group on both sides of a fold.
- Sources: TS, HM, V. Priority: v1. Key behaviour.

### <a name="spl-03"></a>SPL-03 Folds are stratified
Each fold holds, for every class, the same number of groups as the other folds to within one, so that every fold keeps the label distribution of the training split.
- A group counts under its own [label](../glossary.md#label) ([EVA-06](evaluation.md#eva-06)).
- The user can stratify on a finer key than the label, such as a grade or a subclass.
- Stratifying reads the labels only, never the content of the samples, so it stays cheap on millions of [tiles](../glossary.md#tile).
- Task: classification. Other tasks wait on [Q-04](../open-questions.md#q-04).
- Sources: TS, HM. Priority: v1.

### <a name="spl-04"></a>SPL-04 Folds depend only on the groups, their labels and the seed
The same groups, labels and seed always give the same folds, and another seed gives other folds.
- Two runs that cut the same groups into other samples, such as [views](../glossary.md#view) at another scale, thus train and validate on the same folds, so a search compares its [trials](../glossary.md#trial) on the same data.
- Guarantees [INV-03](../invariants.md#inv-03).
- Sources: TS. Priority: v1.

