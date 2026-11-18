
# 🗂️ Data

[Spec](../README.md) / Features / Data

Reading [datasets](../glossary.md#dataset) of any format and structure, describing their [samples](../glossary.md#sample), transforming and augmenting them, and caching what is costly to read.
IDs: `DAT`.

## Recognising a dataset

### <a name="dat-01"></a>DAT-01 Any format, any structure
StoupyNet recognises the file format and the structure of a dataset it is pointed at, and loads it with no further code.
- Structure matters as much as format: a folder of images can hold one subfolder per class, pairs of images and masks, or the [tiles](../glossary.md#tile) of one slide.
- The aim is every common format and structure; the inventory and the survey of other frameworks draw up the list.
- Folders that follow a naming of their own, such as `slide_*` for groups, are read once the user states that naming, with no renaming.
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
- A dataset the user wrote that cannot do so falls back to reading its samples, and a check too costly that way is skipped with a notice.
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

### <a name="dat-10"></a>DAT-10 Training draws part of each bag, evaluation reads it whole
Each read of a bag in training draws a fixed number of its elements, while evaluation and prediction read all of them, since a group's score must cover the group.
- The draw gives every bag the same shape, so bags batch together, and it shows the model another part of the group at each epoch.
- A bag smaller than that number is drawn with repetition, so a small group is sampled rather than dropped.
- The drawn elements keep their reading order ([DAT-06](#dat-06)).
- Whole bags differ in size, so they are read one at a time, and a larger batch asked for them falls back to one with a notice.
- Task: multiple instance learning.
- Sources: TS. Priority: v1.

## Balancing

### <a name="dat-07"></a>DAT-07 Classes are balanced by undersampling
The training side can be balanced by drawing a share of each class, or of each finer subclass, equal by default or weighted by the user, a class too small for its share leaving the rest to the others.
- A class that falls under a minimum share of its target stops the run with an error giving the share of every class, instead of training on a skewed set.
- Inside a cross-validation, each fold balances its own training side.
- Applies to the training side only ([INV-06](../invariants.md#inv-06)).
- Sources: TS, HM. Priority: v1.

### <a name="dat-09"></a>DAT-09 Each epoch can draw a fresh share of the training side
Each epoch can keep some training samples every time and draw the others afresh from their pools, such as new negatives for a segmentation, the draws repeating from one run to the next with the same seed.
- Inside a cross-validation, each fold draws from its own training side.
- Applies to the training side only ([INV-06](../invariants.md#inv-06)).
- Sources: TS, HM. Priority: v1.

## Streams

### <a name="dat-08"></a>DAT-08 A dataset of unknown length
A dataset read as a stream, whose length is unknown and whose samples cannot be reached by position, such as one streamed from the network, trains and evaluates like any other.
- Splits drawn from the names of the groups alone ([SPL-07](splitting.md#spl-07)) work on a stream as they are.
- Folds, stratification and balancing need every group in advance, which a stream only learns as it reads.
- Sources: TS, V. Priority: planned.

## Caching

### <a name="dat-11"></a>DAT-11 What is costly to read is prepared once
What is costly to read, such as views composited from tiles or features from a frozen backbone, is prepared once and stored, so a run whose store is complete decodes no image.
- Work shared by the folds of a cross-validation, or by several runs, is prepared once, and an interrupted preparation resumes where it stopped.
- Guarantees [INV-09](../invariants.md#inv-09).
- Sources: TS, HM. Priority: v1.

### <a name="dat-12"></a>DAT-12 A dataset declared frozen is checked by its folders alone
The user can declare a dataset frozen, and checking its stored results then looks at the folders of its groups instead of their files, a choice the run records.
- On slow storage, such as a network share, the check then costs one lookup per group instead of one per file.
- A file added, removed or renamed is still detected, and a file rewritten in place is not.
- Sources: V. Priority: v1.

