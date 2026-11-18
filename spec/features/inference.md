
# 🔮 Inference

[Spec](../README.md) / Features / Inference

Using a trained [run](../glossary.md#run) on new data: predictions, their aggregation per [group](../glossary.md#group), and exports.
IDs: `INF`.

## Operating point

### <a name="inf-01"></a>INF-01 A trained model carries its decision thresholds
A trained [model](../glossary.md#model) keeps the [decision thresholds](../glossary.md#decision-threshold) its run learned ([EVA-07](evaluation.md#eva-07)), so whoever loads it predicts at the same operating point.
- Its accuracy, sensitivity and specificity hold only at that operating point.
- Sources: TS. Priority: v1.

