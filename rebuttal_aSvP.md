We thank the reviewer for recognizing the value of a cell-aligned spatial-transcriptomics resource at this scale. We agree that scale alone is insufficient; the benchmark must expose concrete challenges and support analyses that smaller, less diverse collections cannot.

**1. How can the larger, higher-resolution, context-rich dataset promote research progress?**

The contribution is not that one predictor solves histology-to-expression inference. sMMC makes three previously difficult evaluations practical:

1. **Breadth and transfer:** models can be evaluated by organ, platform, study, and patient rather than only through pooled random splits.
2. **Cell-aligned resolution:** every record links a cell-centered image, wider context image, coordinates, and expression target. Xenium measurements are natively cell resolved; Visium HD targets are boundary-aware aggregations of native $2\,\mu\mathrm{m}$ bins and are described as *cell-aligned computational targets*, not error-free single-cell ground truth.
3. **Context-aware failure analysis:** metadata enable subgroup and shift analyses that pooled scores would hide.

As a breadth diagnostic, we applied the same image-only predictor, STBoost-Ref, to five Visium HD organs using a common in-domain top-50-gene protocol:

| Organ | Gene $r$ | Gene $\rho$ | Cell $r$ | Cell $\rho$ |
|---|---:|---:|---:|---:|
| Ovary | 0.4523 | 0.4813 | 0.8379 | 0.7785 |
| Lung | 0.5178 | 0.5077 | 0.6042 | 0.5452 |
| Breast | 0.3366 | 0.2631 | 0.2345 | 0.2393 |
| Kidney | 0.3643 | 0.3313 | 0.5112 | 0.4834 |
| Tonsil | 0.3817 | 0.3354 | 0.8193 | 0.5670 |

The wide range is the point: a pooled mean would conceal large tissue-dependent differences. These are in-domain diagnostics, not evidence that all 25 organs are equally validated.

In a separate lung analysis of 111,772 cells, coarse marker-derived state prediction from inferred expression reached 0.5935 accuracy and 0.5922 balanced accuracy (ARI 0.1362; NMI 0.2154). Thus, predictions retain biological signal but are not substitutes for molecular measurements.

We also audited four evaluations on native Xenium segmented-cell targets. Under a fixed 70/10/20 within-sample random-cell split and training-selected genes, gene Pearson/Spearman was 0.541/0.544 in colon (77,636 test cells), 0.512/0.585 in breast (114,907), 0.406/0.459 in ovary (4,000), and 0.509/0.384 in skin (13,696). Recovered genes included TFF3/EPCAM, IGKC/JCHAIN, MUC1/GATA3, and a DES/MYH11/MYLK/ACTG2/ACTA2 smooth-muscle program. Image-model versus coordinate-only gene Pearson was 0.541/0.238, 0.512/0.116, 0.406/0.285, and 0.509/0.257, respectively; image and coordinate-only 95% CIs did not overlap across 100 spatial-block bootstrap replicates. Because the split is within-sample, these results demonstrate native-target feasibility and marker recovery, not sample-held-out generalization.

**Pending before submission:** add the ongoing native-Xenium sample-held-out benchmark with training-mean, coordinate-only, and metadata-only baselines; prespecified marker/program and coarse-state metrics; per-sample values; and uncertainty.

**2. How heterogeneous is the resource, and how does heterogeneity affect downstream analysis?**

We agree that increased scale and resolution also increase heterogeneity. The updated working manifest contains 28,315,247 post-QC aligned cells across 99 unique samples (100 processing records), 25 tissue strata, and Xenium, Visium HD, and DBiC-seq. The 21 in-house DBiC-seq samples contribute 53,989 post-QC aligned cells; the remaining scale is public aggregation and harmonization. One HEST-overlapping slide appears as two versioned processing records, so records are not equivalent to independent donors or studies.

Among the public tissue strata, aligned-cell counts range from 26,366 for human heart to 3,559,793 for human breast, a 135-fold range; the median is 384,121 (IQR 225,906--880,627). Gene coverage ranges from 450 to 72,302, and only 10 of 25 strata occur on both platforms. Tissue state, morphology, donor composition, and metadata completeness also vary. Consequently, cell-weighted pooled results can be dominated by large organ/platform strata, while random-cell splits can exploit within-sample spatial autocorrelation.

We will make these consequences operational:

- report platform-separated and organ-macro results, not only cell-weighted pooled averages;
- distinguish in-domain interpolation from sample-held-out transfer, and use “cross-patient” only when patient IDs are verified;
- report gene-panel overlap, subgroup support, and metadata missingness for each evaluation;
- label every field as directly measured, processed, generated, or model output.

**Author input required:** complete donor/patient, specimen, tissue-state, and missingness fields where supported by source metadata. We will not infer missing sensitive attributes.

**Pending before submission:** add stratified downstream results across platforms, organs, verified patients/studies, panel-overlap bins, and metadata-completeness bins, using macro averages and uncertainty rather than one pooled score.

We will revise the limitations to state that the resource inherits the composition and missingness of its source studies, that uncommon organs and demographic groups may be underrepresented, and that processed Visium HD profiles inherit segmentation and aggregation uncertainty. This turns heterogeneity into an explicit benchmark variable and documented limitation.
