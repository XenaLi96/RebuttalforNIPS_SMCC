# Response to the Area Chair

We thank the Area Chair and all four reviewers. Their comments converged on three questions: what is native versus derived, whether evaluation extends beyond a few examples, and whether performance survives sample/platform shift. We audited the release, ran new Xenium/HD experiments, added controls, and narrowed unsupported claims.

**1. We now separate native measurements from derived targets.**

- Public Xenium contributes **16,314,129 native cells**; in-house DBiC-seq adds **53,989 post-QC native cells from 21 samples** with paired morphology/RNA. Visium HD contributes **7,629,697 derived cell-aligned targets** from 2-µm bins aggregated within masks.
- We will no longer describe the entire resource as directly measured single-cell data. Xenium/DBiC-seq will be labeled *native cell-resolved* and Visium HD *derived cell-aligned*. We will use the count-independent title *sMMC: A Cell-Aligned Multimodal Resource for Spatial Transcriptomics*.
- HD bins are registered to H&E, aggregated by intersecting masks, deconflicted by nearest centroid, and excluded when unsupported. A new 9,000-polygon audit across lung, brain, and pancreas gave ±1-µm-shift Jaccard **0.706–0.816** and expression cosine **0.936–0.994**. Erosion/dilation gave Jaccard **0.462–0.720**, cosine **0.871–0.993**, and median absolute UMI change **32.8–66.6%**. We will release QC and avoid ground-truth language.

**2. Native-Xenium experiments establish breadth and image-associated biological signal.**

We reran one protocol on **30 native-Xenium samples (360,000 cells)**: top-50 training-selected HVGs, four contiguous spatial holdouts, and a 5% buffer. Across 25 release categories:

| Predictor | Gene Pearson | Gene Spearman | Cell Pearson | F1 |
|---|---:|---:|---:|---:|
| Image | **0.324** | **0.291** | **0.442** | **0.413** |
| Coordinate only | 0.046 | 0.041 | 0.288 | 0.283 |
| Spatial KNN | 0.053 | 0.051 | 0.268 | 0.320 |

Image prediction is strongest in 21/25 rows and 28/30 samples. Marker/HVG Gene Pearson is **0.201**, versus **0.031/0.028** for coordinate/KNN. Cell-type-stratified pseudobulk RMSE is **0.120**, versus **0.224/0.187/0.188** for coordinate/KNN/mean. As strata are expression-derived, this is aggregate calibration, not independent cell-type validation.

**3. Cell alignment preserves heterogeneity that spot averaging can hide.**

Across six native-Xenium samples, matched cell/8/16/55-µm targets gave Gene Pearson **0.365/0.365/0.363/0.330**. In dense lung, 55-µm pseudo-spots mixed cell types in **55.5–66.4%** of spots and affected **73.8–81.0%** of cells, versus **3.5–4.1%** and **7.0–8.3%** in a sparse sample. Averaging can therefore hide heterogeneity even when prediction becomes easier.

**4. Cross-sample transfer remains difficult; the revision makes that failure visible and adopts the reviewer-inspired baseline.**

On eight HD source→target organ pairs, UNI2-h obtains macro Gene Pearson **0.0151**, Cell Pearson **0.2036**, and F1 **0.0815**; the training mean reaches **0.2422/0.0375**. We retain these unfavorable results rather than present interpolation as generalization.

Following the reviewers’ suggestion, we added leakage-controlled coordinate, segmentation-metadata, and combined baselines on identical cells/genes. Metadata exclude expression-derived QC fields and improve Gene Pearson in **8/8 organs**, raising macro Gene/Cell Pearson to **0.0399/0.2344**. F1 is **0.0432**, below UNI2-h; we will use it as the principal non-image *correlation-calibration* baseline alongside the mean, not claim universal superiority. Sample/platform/processing shift remains open; this is not evidence of invalid source data.

**5. We will revise the framing and reporting.**

- Within-sample experiments use spatially separated block splits: left two-thirds for training, right one-third for testing, with a 5% boundary buffer.
- Ovarian results become an *age-associated, sample-confounded shift*, not a causal age effect. Context audits report Average/Worst/Gap, donor support, and missingness.
- Direct, processed, and generated fields are separated; captions are optional context, never molecular ground truth.
- STBoost, splits, cell units, HD construction, and Figure 1 will be clarified. Code, fixed splits, audits, and in-house data are in the [anonymous repository](https://anonymous.4open.science/r/sMMC-22M-DB75/README.md).

We do not claim that histology replaces molecular measurement or that transfer is solved. The new evidence establishes the resource’s validity and limits and makes the unresolved generalization gap an explicit benchmark.
