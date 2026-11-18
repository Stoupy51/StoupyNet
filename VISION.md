
# 🧭 StoupyNet vision

StoupyNet aims to train any model, on any data, for any task, in a few lines of code, and to make every new method from the literature easy to add.
Every design decision is weighed against the principles below.

## 🎯 Goal

A user describes their data, their task and their model, and gets a complete experiment: training, evaluation, metrics, plots, interpretability, and a tracked run that can be reproduced.
This holds equally for a folder of photos and for tiled gigapixel slides, for a classifier and for an instance segmenter.
When the defaults are not enough, any part can be replaced, from the loss to the training loop, without forking StoupyNet.
A method from a paper is added as one self-contained piece of code, after which anyone can apply it to their own data and task.

## 🧱 Principles

### A few lines for the common case, full control for the rest

The common case needs no subclass and no boilerplate.
Going further reveals one layer at a time: a setting, then a swapped component, then a new component, then the underlying libraries, which stay directly usable.

### The more it offers, the better, as long as everything stays easy to find

Breadth is a goal: models, losses, metrics, data formats, training techniques.
Each addition must leave StoupyNet as easy to learn and to search as before.
An option that only some tasks read never shows up for the others.

### Sound science by default

The rigorous protocol is the default one, so the shortest code also runs the correct experiment.
Samples of one group, such as a patient or a slide, never end up on both sides of a split.
Every reported score states where it comes from (validation, out of fold, test), and nothing is ever tuned on the test set.

### Never guess

When an input can be read in several ways, StoupyNet stops with an error that names the possible readings.
A dataset whose format is recognised loads in one line; one that several loaders could read raises an error listing them, and the user picks the one that fits.

### Designed before written

Every capability is specified before it is implemented, and finds its place among the existing concepts.
One that fits nowhere triggers a review of the design instead of a patch on the side.

### Dependencies

StoupyNet builds on very popular, actively maintained and stable projects, such as Lightning and timm, instead of rewriting what they already do well.
It never takes a dependency for only one or two of its features.

### Credit and licenses

Every method names the paper it implements, and every pretrained weight states its license and what that license allows.
Code taken from another project must carry a license compatible with MIT.

### Documentation you can navigate

Text-only documentation is painful to search, so every concept has its own page with a diagram.
Pages link to each other wherever one concept relies on another, and each question has one obvious place to look.

### Code for the next reader

The code is strictly typed, and its names and structure say what it does.
Nothing is written twice: a capability has one implementation, and a better one replaces it instead of living beside it.
A test exists to protect something users rely on, as [TESTING.md](TESTING.md) details.

