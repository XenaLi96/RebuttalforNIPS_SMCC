# Response to the Area Chair regarding Reviewer vCPF

We thank Reviewer vCPF for requesting a direct audit of Visium HD construction, broader evaluation, biological calibration of low correlations, and stronger non-image controls. We completed each analysis and retain unfavorable transfer results alongside the improved baseline.

**1. HD construction is now tested rather than assumed.**

Official transforms register native 2-µm bins to H&E; intersecting CellViT polygons aggregate counts, conflicts go to the nearest centroid, and unsupported polygons are removed. We will move this rule into the main text and label the outputs *derived cell-aligned*, separately from native Xenium.

We audited **3,000 raw polygons per dataset (9,000 total)** across lung, brain, and pancreas. Canonical-bin coverage was **49.4%, 97.5%, and 50.0%**. Among supported polygons, ±1-µm shifts gave bin Jaccard **0.706–0.816**, expression cosine **0.936–0.994**, and median absolute UMI change **6.0–13.7%**. Erosion/dilation was more disruptive (Jaccard **0.462–0.720**, cosine **0.871–0.993**, UMI change **32.8–66.6%**). We will release settings/QC, report sample-dependent support, and avoid ground-truth language.

**2. Evaluation now covers the full release breadth.**

A uniform native-Xenium benchmark covers 30 samples and all 25 release categories using top-50 training-selected HVGs, four contiguous spatial holdouts, and a 5% train–test buffer. Image prediction is strongest in **21/25 category rows**; macro Gene Pearson, Gene Spearman, Cell Pearson, and F1 are **0.324/0.291/0.442/0.413**, versus coordinate Gene Pearson **0.046** and spatial-KNN Gene Pearson **0.053**.

Across 18 complete same-organ held-out targets, top-50-HVG Gene Pearson averages **0.170** (range **0.016–0.372**), Cell Pearson is **0.275** versus **0.205** for the training mean, and F1 is **0.125** versus **0.031**. These are sample-held-out, not donor-held-out.

**3. We report the difficult transfer result before calibration.**

On eight complete Visium HD source→target organ pairs, UNI2-h gives macro Gene Pearson **0.0151**, Cell Pearson **0.2036**, and F1 **0.0815**. The training mean gives Cell Pearson **0.2422** and F1 **0.0375**. Thus, UNI2-h improves F1 but not Cell Pearson over the mean, and gene-wise transfer remains poor. We retain this negative result and do not conflate it with the easier spatial-interpolation endpoint.

**4. The reviewer-inspired metadata baseline improves calibration but does not solve transfer.**

On identical cells and genes, we added coordinate-only, segmentation-metadata-only, and combined ridge baselines. Segmentation metadata include CellViT type/confidence, polygon/bounding-box morphology, status, and edge flag; expression-derived `total_counts` and `n_genes_detected` are excluded.

Segmentation metadata improve Gene Pearson in **8/8 organs**, raising macro Gene/Cell Pearson from **0.0151/0.2036 to 0.0399/0.2344**. Coordinate-only and combined Gene Pearson are **−0.0023/0.0316**. However, metadata F1 is **0.0432**, below UNI2-h **0.0815**. We will therefore adopt segmentation metadata as the principal non-image *correlation-calibration* baseline alongside the mean, not claim universal superiority. Age/sex/disease cannot be separated with only one source/target pair per organ.

Complementary biological calibration is also favorable: marker/HVG Gene Pearson is **0.201** for image versus **0.031/0.028** for coordinate/KNN, and cell-type-stratified pseudobulk RMSE is **0.120** versus **0.224/0.187/0.188** for coordinate/KNN/mean. We thank the reviewer for this constructive suggestion and will add the paired transfer comparison and biological-calibration results to the revised manuscript.

**5. Claims will match the evaluation protocol.**

Within-sample experiments use spatial block splits rather than random cell splits: left two-thirds for training, right one-third for testing, with a 5% boundary buffer. The 30-sample benchmark applies four contiguous spatial holdouts under the same principle. The ovarian example will be called an *age-associated, sample-confounded shift*. Direct, processed, and generated fields will be separated, and generated captions will remain optional context rather than molecular truth.

The reviewer’s requested controls have improved the benchmark and provided a stronger calibration baseline, which we will incorporate into the revision.
