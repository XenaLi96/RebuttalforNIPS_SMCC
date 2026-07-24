We thank the reviewer for the careful assessment. We agree with the three central requests: quantify uncertainty in the Visium HD construction, demonstrate breadth beyond a few cases, and calibrate low correlations using biological and simple baselines. We will also distinguish in-domain interpolation from sample-held-out generalization.

**1. For Visium HD, how reliable is bin-to-cell aggregation? How are boundary bins assigned, and do segmentation errors affect expression profiles?**

Xenium supplies natively cell-resolved transcript locations and platform-defined cell masks. Visium HD instead supplies native $2\,\mu\mathrm{m}$ bins; we register them to H&E, segment cell footprints, and aggregate bin counts over the aligned footprints. We will therefore call the latter *cell-aligned computational targets*, not direct single-cell measurements.

The audited rule assigns a bin when its closed $2\,\mu\mathrm{m}$ square intersects a CellViT polygon. If several polygons claim it, the bin is assigned to the nearest centroid in full-resolution coordinates; exact ties use the lower raw-cell index. The $8\,\mu\mathrm{m}$ files are legacy QC/fallback artifacts, not the canonical targets.

**AUTHOR INPUT REQUIRED:** confirm the CellViT model/version, registration implementation, and released-pipeline QC thresholds.

We completed boundary perturbations for lung A, lung B, and ovary. Relative to the default rule, strict-interior assignment retained 62.5%/56.1%/80.7% of cells and 16.0%/11.0%/20.5% of bins; one-bin erosion retained 46.0%/37.9%/72.0% of cells and 18.9%/12.2%/26.0% of bins. One-bin dilation retained 100.0%--100.1% of cells but increased assigned bins to 181.9%--196.2% of default. Thus, assignment is materially boundary-sensitive and must not be presented as error-free single-cell ground truth.

**PENDING EXPERIMENT:** on identical common cells/genes, add expression Pearson/Spearman, HVG/marker-rank stability, coarse-state concordance, and fixed-checkpoint image-to-gene sensitivity with confidence intervals. We will report Xenium and Visium HD separately.

**2. The experiments cover few cases. Can the authors summarize more organs or platforms?**

We have a directly comparable five-organ Visium HD analysis using the same in-domain top-50-gene protocol:

| Organ | STBoost-Ref gene $r$ | STBoost-Ref cell $r$ | BLEEP gene $r$ | BLEEP cell $r$ |
|---|---:|---:|---:|---:|
| Ovary | 0.4523 | 0.8379 | 0.4239 | 0.8212 |
| Lung | 0.5178 | 0.6042 | 0.4241 | 0.5622 |
| Breast | 0.3366 | 0.2345 | 0.0079 | 0.1282 |
| Kidney | 0.3643 | 0.5112 | 0.3356 | 0.5369 |
| Tonsil | 0.3817 | 0.8193 | 0.1809 | 0.7808 |

This supports breadth across five tissues while exposing substantial tissue-dependent heterogeneity; it does not imply equal validation of all 25 organs. We will place gene selection, split, and sample size beside the table.

**PENDING EXPERIMENT:** add the lightweight benchmark over the final organ set and both platforms, with per-organ/platform values, organ-macro averages, sample/cell counts, gene-panel overlap, and uncertainty. Xenium and Visium HD will remain separate.

**3. How should low gene-wise correlations be interpreted biologically? Could marker/cell-type genes and spatial-only or metadata-only baselines calibrate them?**

We agree that the low full-panel correlations are important negative results. Sparse or weakly morphology-linked genes depress gene-wise correlation, and sample transfer also changes morphology, composition, and acquisition. In lung, STBoost-Ref obtains gene Pearson/Spearman and cell Pearson of 0.0259/0.0268/0.4112 in-domain and 0.0124/0.0137/0.2820 sample-held-out over the full panel. Histology alone is therefore not a replacement for measured expression.

An explicitly post-hoc selected 50-gene diagnostic gives 0.5117/0.5069/0.5979 in-domain and 0.2986/0.2405/0.3451 held-out. Separately, marker-derived states from 111,772 lung cells achieve 0.5922 balanced accuracy (ARI 0.1362). These results indicate recoverable biological signal but caution against cell-by-cell interpretation.

On native Xenium targets, existing 70/10/20 within-sample random-cell evaluations with training-only gene selection give:

| Organ | Test cells | Genes | Gene $r/\rho$ | Coordinate-only $r$ | Cell $r/\rho$ |
|---|---:|---:|---:|---:|---:|
| Colon | 77,636 | 10 | 0.541/0.544 | 0.238 | 0.666/0.637 |
| Breast | 114,907 | 10 | 0.512/0.585 | 0.116 | 0.546/0.519 |
| Ovary | 4,000 | 10 | 0.406/0.459 | 0.285 | 0.644/0.543 |
| Skin | 13,696 | 10 | 0.509/0.384 | 0.257 | 0.508/0.443 |

Recovered markers include TFF3/EPCAM, IGKC/JCHAIN, MUC1/GATA3, and the DES/MYH11/MYLK smooth-muscle program. Across 100 spatial-block bootstrap replicates, image-model gene-$r$ 95% CIs were [0.518, 0.558], [0.487, 0.530], [0.377, 0.424], and [0.467, 0.525]; none overlapped the corresponding coordinate-only CIs. These quantify within-sample feasibility, not held-out-sample transfer.

**PENDING EXPERIMENT:** fixed-panel sample-held-out native-Xenium evaluation with training-mean, coordinate-only, and metadata-only baselines; marker/program/coarse-state metrics; Average, Worst, and Gap.

**4. In-domain splits may reflect local interpolation and spatial autocorrelation rather than robust generalization.**

We agree. We will relabel in-domain results as interpolation diagnostics and use sample-held-out/cross-study transfer for generalization claims. The lung full-panel cell Pearson drop from 0.4112 to 0.2820 already illustrates this gap.

**PENDING EXPERIMENT:** add a distance-buffered split and coordinate-only/spatial-neighbor baselines, reporting performance against minimum train--test distance.

**5. The ovarian age-shift analysis is narrow and does not establish systematic evaluation of all context variables.**

Agreed. It is a showcase of how metadata can expose subgroup disparity, not evidence that every context variable has been validated. AK, AD, and AL are anonymized ovarian sample identifiers. The current comparisons also differ in patient/tumor context and gene-panel size, so they cannot identify a causal age effect.

**PENDING EXPERIMENT:** rerun on the identical shared gene panel and preprocessing, add sample/cell counts and uncertainty, and describe the finding as an *age-associated, sample-confounded shift* unless independent-donor support permits a stronger claim.

**6. Which released fields are direct measurements, processed artifacts, or generated outputs?**

| Provenance | Examples |
|---|---|
| Directly measured | H&E; Xenium transcript coordinates and platform cell masks; Visium HD $2\,\mu\mathrm{m}$ bin counts; source metadata as reported. |
| Processed | Registered coordinates; segmented footprints; Visium HD bin-to-cell matrices; local/context crops; normalized matrices; split indices; communication summaries computed from molecular profiles. |
| Generated | Metadata-grounded captions and model predictions; neither is molecular ground truth. |

The revision will include a field-level manifest, release version, licenses, checksums, split definitions, and an exact inventory of available raw, processed, and generated files.

**PENDING EXPERIMENT:** a stratified human factuality audit of generated captions with rubric, error categories, agreement, and supported/unsupported rates. Unless validated, captions will be labeled optional machine-generated annotations and excluded from the core benchmark.
