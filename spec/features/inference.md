
# 🔮 Inference

[Spec](../README.md) / Features / Inference

Using a trained [run](../glossary.md#run) on new data: predictions, their aggregation per [group](../glossary.md#group), and exports.
IDs: `INF`.

## Operating point

### <a name="inf-01"></a>INF-01 A trained model carries its decision thresholds
A trained [model](../glossary.md#model) keeps the [decision thresholds](../glossary.md#decision-threshold) its run learned ([EVA-07](evaluation.md#eva-07)), so whoever loads it predicts at the same operating point.
- Its accuracy, sensitivity and specificity hold only at that operating point.
- Sources: TS. Priority: v1.

## Provenance

### <a name="inf-02"></a>INF-02 A trained model knows the groups it saw
A trained model keeps the list of the [groups](../glossary.md#group) it trained and validated on, so that whoever uses it knows which predictions fall on data it saw.
- A prediction on a group the model saw says so.
- A combination of models checks that list before being scored ([INV-07](../invariants.md#inv-07)).
- Sources: HM, V. Priority: v1.

