# Response to the Area Chair regarding Reviewer aSvP

We thank Reviewer aSvP for asking what research progress is enabled by a resource that is larger, cell aligned, and context rich, and how its heterogeneity affects downstream analysis. We now answer both questions with controlled experiments and explicit protocol changes.

**1. The contribution is an evaluation resource, not a claim that prediction is solved.**

The working manifest contains **99 unique samples (100 records), 28,315,247 aligned targets, and 25 tissue strata plus three cell-line origins**. The new primary component comprises **21 in-house DBiC-seq samples and 53,989 post-QC native cells** paired with morphology, RNA, and cellular context.

The value of this scale is tested under a fixed task. Holding the spatial test regions, training-selected top-50 HVGs, and regression protocol constant, the seven-encoder mean Gene Pearson rises from **0.177→0.205→0.241→0.278** at 5%/10%/25%/100% training fractions; every encoder improves. This supports a benefit of additional cells within the tested range, not a universal scaling law.

**2. Breadth and biological calibration now extend beyond the original examples.**

We ran one leakage-controlled protocol across all 25 release categories using 30 native-Xenium samples, four contiguous spatial holdouts, and a 5% train–test buffer. The image macro results are Gene Pearson **0.324**, Gene Spearman **0.291**, Cell Pearson **0.442**, and F1 **0.413**; image prediction exceeds coordinate and spatial-KNN controls in **28/30 samples**.

On marker/HVG overlap, image Gene Pearson is **0.201**, versus **0.031/0.028** for coordinate/KNN controls. Cell-type-stratified pseudobulk RMSE is **0.120**, versus **0.224/0.187/0.188** for coordinate/KNN/training mean. Because cell-type strata are expression-derived, this is aggregate biological calibration rather than independent label validation. Code, fixed splits, configurations, and evaluations are available in the [anonymous repository](https://anonymous.4open.science/r/sMMC-22M-DB75).

**3. Cell alignment exposes heterogeneity that coarse aggregation can hide.**

Across six native-Xenium samples, matched cell/8/16/55-µm supervision gives Gene Pearson **0.365/0.365/0.363/0.330**. We do not argue that cell-level prediction must always produce a higher correlation. Instead, the controlled pseudo-spot analysis shows what averaging removes: in dense lung, **55.5–66.4%** of 55-µm pseudo-spots mix cell types and involve **73.8–81.0%** of cells, versus **3.5–4.1%** and **7.0–8.3%** in a sparse sample. Cell-level evaluation therefore preserves tissue-dependent heterogeneity that may be numerically smoothed at spot resolution.

**4. Heterogeneity is now a benchmark variable rather than a hidden nuisance.**

The audit identifies:

- a **135-fold** organ-size range (**26,366–3,559,793 cells**), which makes cell-weighted pooling misleading;
- gene-panel ranges of **450–72,302 genes**, which prevent naïve full-panel comparisons;
- only **10/25** public tissue strata represented on both Xenium and Visium HD, allowing platform and biology to be confounded;
- 18 same-organ held-out targets with mean top-50-HVG Gene Pearson **0.170** but range **0.016–0.372**; and
- context-associated failures, including ovarian F1 **0.289→0.072**.

Accordingly, the revision will report organ-macro and platform-separated results; distinguish spatial interpolation, sample-held-out, and verified patient-held-out protocols; report Average/Worst/Gap with support; disclose panel overlap and metadata missingness; and never infer missing sensitive attributes. The ovarian comparison will be labeled sample/age-confounded rather than causal.

The revised contribution is therefore precise: sMMC does not eliminate biological and technical heterogeneity. It provides sufficient scale, alignment, and context to measure how that heterogeneity changes model behavior across organs, platforms, resolutions, samples, and subgroups.
