# Response to the Area Chair regarding Reviewer aSvP

We thank Reviewer aSvP for identifying two questions that the manuscript had not explained clearly enough: how the data promote research progress, and how increased heterogeneity affects downstream analysis. The revised response and manuscript are organized directly around these concerns.

**1. The resource promotes research by lowering practical barriers and enabling reproducible evaluation.**

Spatial-transcriptomics data are fragmented across platforms, panels, formats, segmentation conventions, and metadata schemas. sMMC supplies one cell-aligned interface for morphology, tissue context, molecular targets, coordinates, and available donor metadata. The 25-organ release can therefore be accessed through one pipeline while preserving provenance: native Xenium/DBiC-seq cells remain distinct from Visium HD targets derived within cell masks.

The accompanying benchmark uses image-only local-cell and tissue-context crops to predict RNA aligned to a cell. STBoost lets existing spot predictors use this hierarchical interface while retaining their prediction modules. Fixed targets and splits support:

- spatially separated within-sample prediction;
- complete-sample transfer;
- verified patient/donor holdout;
- panel-matched cross-platform transfer;
- 25-organ breadth and context-aware evaluation.

This infrastructure lets researchers study scaling, foundation encoders, normalization, adaptation, robust learning, and biological transfer without rebuilding the dataset. It does not claim that image-to-expression prediction is solved.

**2. Cell alignment enables downstream questions whose natural unit is the cell.**

Spots merge cells with different identities, states, and responses. Moving image and molecular targets toward the same cell preserves local heterogeneity and supports:

- cell type/state analysis linking morphology to identity and disease programs;
- cell–cell communication and the role of tissue architecture;
- virtual-cell and perturbation models linking morphology, context, state, and response;
- microenvironment analysis that distinguishes cells within a niche rather than assigning one averaged profile.

sMMC does not itself solve these tasks; it supplies a common multimodal substrate on which they can be trained and evaluated across organs and contexts.

**3. The revision explicitly documents heterogeneity and its downstream effects.**

We will distinguish:

- biological heterogeneity: organ, species, disease, composition, density, and donor;
- technical heterogeneity: platform, study, acquisition, segmentation, and resolution;
- molecular heterogeneity: panel overlap, coverage, sparsity, and target construction;
- context heterogeneity: uneven age, sex, disease, site, and patient support;
- scale imbalance across organs and studies.

These factors directly affect analysis: large organs can dominate pooled training; unmatched panels make gene metrics incomparable; platform/study effects can resemble biology; segmentation and density change targets and neighborhood mixing; missing donors make sample shift look like patient shift; and uneven context support confounds subgroup comparisons.

Accordingly, we will report organ-macro and platform-separated summaries, panel overlap, donor/sample support, metadata missingness, and Average/Worst/Gap; reserve “patient-held-out” for independent donors; separate native/derived targets and spatial/sample/patient/platform protocols; and label measured, processed, generated, and model-derived fields.

The revised claim is precise: sMMC does not remove heterogeneity. It makes heterogeneity auditable and provides a shared resource for studying how it affects generalization, bias, and cell-centered downstream research.
