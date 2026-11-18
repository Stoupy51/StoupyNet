
# 🗂️ Data

[Spec](../README.md) / Features / Data

Reading [datasets](../glossary.md#dataset) of any format and structure, describing their [samples](../glossary.md#sample), transforming and augmenting them, and caching what is costly to read.
IDs: `DAT`.

## Recognising a dataset

### <a name="dat-01"></a>DAT-01 Any format, any structure
StoupyNet recognises the file format and the structure of a dataset it is pointed at, and loads it with no further code.
- Structure matters as much as format: a folder of images can hold one subfolder per class, pairs of images and masks, or the [tiles](../glossary.md#tile) of one slide.
- The aim is every common format and structure; the inventory and the survey of other frameworks draw up the list.
- Sources: V. Priority: v1.

### <a name="dat-02"></a>DAT-02 An ambiguous dataset stops with its possible readings
When a dataset could be read in several ways, loading stops with an error that names each of them, and the user picks one explicitly.
- Applies the "Never guess" principle of [VISION.md](../../VISION.md#never-guess).
- Sources: V. Priority: v1.

