
# 🗂️ Data

[Spec](../README.md) / Features / Data

Reading [datasets](../glossary.md#dataset) of any format and structure, describing their [samples](../glossary.md#sample), transforming and augmenting them, and caching what is costly to read.
IDs: `DAT`.

## Recognising a dataset

### <a name="dat-01"></a>DAT-01 Any format, any structure
StoupyNet recognises the file format and the structure of a dataset it is pointed at, and loads it with no further code.
- Structure matters as much as format: a folder of images can hold one subfolder per class, pairs of images and masks, or the [tiles](../glossary.md#tile) of one slide.
- The aim is every common format and structure; the inventory and the survey of other frameworks draw up the list.
- A project whose folders follow a naming of its own, such as `slide_*` for its groups, states that naming instead of renaming its folders.
- Sources: TS, V. Priority: v1.

### <a name="dat-02"></a>DAT-02 An ambiguous dataset stops with its possible readings
When a dataset could be read in several ways, loading stops with an error that names each of them, and the user picks one explicitly.
- Applies the "Never guess" principle of [VISION.md](../../VISION.md#never-guess).
- Sources: V. Priority: v1.

## Samples

### <a name="dat-03"></a>DAT-03 Metadata travels with each sample to its prediction
The [metadata](../glossary.md#metadata) of a sample, such as its name, its [group](../glossary.md#group) and its position, travels with it to its prediction and never reaches the [model](../glossary.md#model).
- Every prediction can thus be traced back to its sample and combined per group.
- Sources: TS. Priority: v1.

### <a name="dat-04"></a>DAT-04 A sample's label and metadata are read without its content
A dataset gives the [label](../glossary.md#label) and the metadata of a sample without reading its content, so stratifying, checking folds or picking samples to explain never decodes the images.
- A dataset written by a project that cannot do so falls back to reading its samples, and a check too costly that way is skipped with a notice.
- Sources: TS. Priority: v1.

### <a name="dat-05"></a>DAT-05 A dataset from another library works as it is
A dataset from another library that yields pairs of an input and a label, such as an image folder of torchvision, works with StoupyNet without being rewritten.
- Without groups or positions, group metrics and overlap weighting are skipped, and everything else works.
- Sources: TS. Priority: v1.

### <a name="dat-06"></a>DAT-06 A bag lists its elements in reading order
The elements of a [bag](../glossary.md#bag) always come in the same order, top to bottom then left to right by their positions, since some models read that order as neighbourhood.
- Ordering by file name cannot do it: `1000-0` sorts before `800-0` as text.
- Elements without positions keep the order of their dataset.
- Task: multiple instance learning.
- Sources: TS. Priority: v1.

## Balancing

### <a name="dat-07"></a>DAT-07 Classes are balanced by undersampling
The training side can be balanced by drawing a share of each class, or of each finer subclass, equal by default or weighted by the project, a class too small for its share leaving the rest to the others.
- A class that falls under a minimum share of its target stops the run with an error giving the share of every class, instead of training on a skewed set.
- Applies to the training side only ([INV-06](../invariants.md#inv-06)).
- Sources: TS, HM. Priority: v1.

