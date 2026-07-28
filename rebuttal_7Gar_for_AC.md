# Response to the Area Chair regarding Reviewer 7Gar

We thank Reviewer 7Gar for asking us to quantify what is new, clarify the purpose of each experimental axis, and validate the derived HD targets and generated text. We have completed the requested audits and will revise the framing accordingly.

**1. Dataset composition and novelty are now explicit.**

The audited working manifest contains **99 unique samples (100 processing records) and 28,315,247 aligned targets**. It includes:

- **21 new in-house DBiC-seq samples and 53,989 post-QC native cells**, paired one-to-one with morphology and RNA;
- 32 public samples overlapping HEST-1K, all from its Visium HD cohort;
- two samples overlapping STimage-1K4M only; and
- 44 other public samples.

Thus, approximately **70% of our samples are absent from HEST-1K**. The in-house data are available for [download and preview](https://anonymous.4open.science/r/sMMC-22M-DB75/README.md). We will distinguish new primary measurements, aggregated public data, processed derivatives, and generated auxiliary fields rather than presenting them as one undifferentiated collection.

**2. The three experimental axes now have distinct, testable objectives.**

- **Scale:** This is not intended as a leaderboard. It asks whether more training cells improve image-to-expression prediction while targets, preprocessing, head, and spatial tests are fixed. Across seven frozen encoders, mean Gene Pearson increases monotonically from **0.177→0.205→0.241→0.278** at 5%/10%/25%/100% training fractions; every encoder improves. We interpret this only within the tested range, not as a universal scaling law.
- **Resolution:** This is the central benchmark: histology-only prediction of an aligned-cell RNA profile, with no molecular input at inference. STBoost is the interface that adapts spot-level methods to hierarchical cell/context crops; STBoost-Ref is our image-only reference predictor. On 30 native-Xenium samples, the macro results are Gene Pearson **0.324**, Gene Spearman **0.291**, Cell Pearson **0.442**, and F1 **0.413**. Image prediction is strongest in 21/25 release-category rows.
- **Context:** This axis asks whether average performance hides subgroup collapse. The ovarian example changes F1 from **0.289 to 0.072** and gene/cell Spearman from **0.036/0.206 to 0.006/0.095**. Because age is nested within sample/patient and panels differ, we will call this an *age-associated, sample-confounded shift*, not a causal age effect. Complementary assay/dataset/site audits show large Average–Worst gaps across Geneformer, scGPT, CONCH, UNI, and H-Optimus-0. Context metadata make such failures measurable; they do not remove bias.

**3. Visium HD construction has been quantitatively audited.**

Official transforms register native 2-µm bins to H&E; intersecting same-frame cell polygons aggregate counts, conflicts are resolved deterministically, and unsupported polygons are removed. These outputs will be labeled *derived cell-aligned targets*, not measured single-cell RNA.

In a new audit of **9,000 raw CellViT polygons** across lung, brain, and pancreas, ±1-µm registration shifts gave bin Jaccard **0.706–0.816**, expression cosine **0.936–0.994**, and median absolute UMI change **6.0–13.7%** among supported polygons. Mask erosion/dilation was more disruptive, so we will release the settings/QC, report sample-dependent coverage, and avoid ground-truth language.

**4. Generated captions are auxiliary and separately validated.**

GPT-4o verbalizes supplied fields rather than generating molecular targets. PLIP/CONCH image–text audits give pairwise AUC **0.993/0.988** and **100% Top-3 retrieval**. These metrics test image relevance; source-field checks test factual consistency. We will restore prompts, field checks, examples, and matrices, while retaining captions only as optional generated context.

Together, these changes directly address the reviewer’s concern: novelty is numerically auditable, each experiment now has a defined scientific question, and neither derived HD profiles nor generated captions are conflated with primary molecular measurements.
