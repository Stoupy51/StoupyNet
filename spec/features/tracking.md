
# 🧾 Tracking

[Spec](../README.md) / Features / Tracking

Recording [runs](../glossary.md#run), with their configuration, metrics, artefacts and versions, and comparing or resuming them within an [experiment](../glossary.md#experiment).
IDs: `TRK`.

## Comparing runs

### <a name="trk-01"></a>TRK-01 A comparison says when runs were not tested on the same groups
Comparing runs on their test scores says when their test [groups](../glossary.md#group) differ, since the gap may then come from the groups drawn rather than from the models.
- Runs drawn with the same seed share their splits ([SPL-07](splitting.md#spl-07)), and a fixed test set ([SPL-06](splitting.md#spl-06)) holds them for good.
- Sources: HM, V. Priority: v1.

