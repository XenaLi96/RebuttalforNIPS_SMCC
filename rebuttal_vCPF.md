We sincerely thank the reviewer for the careful and constructive assessment. We agree with the three central requests: quantify uncertainty in Visium HD cell construction, demonstrate breadth beyond a few cases, and calibrate low correlations with biological and simple baselines. This resource has accompanied me since my first PhD year; as I approach graduation, I hope its unified release helps new students enter spatial transcriptomics with fewer practical barriers.

The resource is also continuing to grow through primary acquisition. Our in-house DBiC-seq collection contributes 21 samples and 54,304 measured cells; paired-record QC removes 315, leaving 53,989 aligned cells with paired morphology, RNA, and cellular context. The current working manifest contains 99 unique samples (100 processing records) and 28,315,247 aligned targets across Xenium, Visium HD, and DBiC-seq. A broader continuing DBiC-seq pool contains approximately 200,000 paired cells but is not counted here.

**1. For the Visium HD data, how reliable is the bin-to-cell aggregation? How are boundary bins assigned, and do segmentation errors noticeably affect expression profiles?**

We will distinguish native Xenium cells from *derived cell-aligned* Visium HD targets. Official transforms register native $2\,\mu$m bins to H&E. A closed bin square is assigned when it intersects a same-frame CellViT polygon; if several polygons claim it, the nearest full-resolution centroid wins, with raw-cell index breaking exact ties. Unsupported cells are removed. We will move this rule and a complete record example from Appendix S2 into the main text.

We audited 3,000 raw polygons per dataset (9,000 total) under 1-$\mu$m registration shifts and mask erosion/dilation:

| Visium HD sample | Raw polygons with canonical bins | Shift-bin Jaccard | Shift-expression cosine | Shift median $|\Delta\mathrm{UMI}|$ |
|---|---:|---:|---:|---:|
| Human lung cancer | 49.4% | 0.727--0.733 | 0.954--0.957 | 11.1%--11.3% |
| Mouse brain | 97.5% | 0.806--0.816 | 0.936--0.939 | 6.0%--6.1% |
| Human pancreas | 50.0% | 0.706--0.714 | 0.994 | 13.0%--13.7% |

Canonical-bin percentages use all detected polygons; unsupported polygons are excluded before release. Small shifts preserve expression direction but alter exact membership and UMIs. Erosion/dilation gives median bin Jaccard 0.462--0.720, expression cosine 0.871--0.993, and absolute UMI changes of 32.8%--66.6%. Thus, segmentation/assignment uncertainty is material. We will release assignment settings and per-sample retention/QC, avoid treating Visium HD aggregation as ground truth, and report native-Xenium evidence separately.

**2. The main experiments cover only a few representative cases. Can the authors summarize more organs or platforms?**

Yes. We completed a leakage-controlled native-Xenium benchmark across 30 samples, 17 organ labels, two species, and 360,000 spatially stratified cells, using four contiguous spatial holdouts, a 5% buffer, and training-selected genes:

| Model | Gene P Top-50 | Gene P Top-200 | Gene S Top-50 | Cell P | F1 |
|---|---:|---:|---:|---:|---:|
| **Image** | **0.324** | **0.202** | **0.291** | **0.442** | **0.413** |
| Coordinate only | 0.046 | 0.031 | 0.041 | 0.288 | 0.283 |
| Spatial KNN | 0.053 | 0.029 | 0.051 | 0.268 | 0.320 |
| Training mean | N/A | N/A | N/A | 0.305 | 0.253 |

Image prediction beats coordinate and KNN controls in 28/30 samples; top-50 Gene Pearson has a biological-sample bootstrap 95% CI of 0.277--0.368. Across 18 same-organ native-Xenium target samples held out in full, image prediction obtains mean top-50-HVG Gene Pearson 0.170 (range 0.016--0.372), Cell Pearson 0.275 versus 0.205 for training mean, and F1 0.125 versus 0.031. These are sample-, not donor-held-out, results.

For Visium HD breadth, eight source-sample→target-sample organ pairs give:

| Eight-organ macro result | Gene P | Cell P | F1 |
|---|---:|---:|---:|
| UNI2-h | 0.0151 | 0.2036 | 0.0815 |
| Training mean | N/A | 0.2422 | 0.0375 |

The transfer result is intentionally not hidden: UNI2-h improves F1 but not Cell Pearson over the mean baseline. We will report organ-macro and per-organ values, separate Xenium from Visium HD, and avoid cell-weighted pooled claims.

**3. Some gene-wise correlations are low, especially under transfer. How should they be interpreted biologically, and could marker/cell-type genes or spatial-only/metadata-only baselines calibrate them?**

Low full-panel correlations show that histology is not a replacement for molecular measurement. They combine sparse/weakly morphology-linked genes with sample, composition, and acquisition shifts. A restricted training-selected HVG endpoint is easier and must not be conflated with all-gene transfer.

Biological calibration of the 30-sample spatially held-out predictions gives:

| Metric | Image | Coordinate | Spatial KNN | Mean |
|---|---:|---:|---:|---:|
| Marker/HVG Gene Pearson $\uparrow$ | 0.201 | 0.031 | 0.028 | N/A |
| Cell-type-stratified pseudobulk RMSE $\downarrow$ | 0.120 | 0.224 | 0.187 | 0.188 |

The marker inventory overlaps training-selected HVGs, and cell-type strata are expression-derived; these results show recoverable aggregate structure, not independent cell-type validation. Under the harder bidirectional all-gene lung/ovary transfer, image Gene Pearson averages are $-0.0003/0.0020$, whereas metadata-only baselines reach 0.104/0.142; coarse-state balanced accuracy is 0.139/0.159 for image prediction versus 0.220/0.252 for metadata. This negative result demonstrates context dependence and motivates stronger patient/platform-robust methods rather than supporting unrestricted cell-wise recovery.

**4. In-domain results may reflect local interpolation and spatial autocorrelation rather than robust generalization.**

We agree. We will call same-sample results *spatial interpolation diagnostics*. The 30-sample experiment uses contiguous holdouts and a 5% train--test buffer, while the 18-target and eight-organ experiments hold out complete samples. Their lower and heterogeneous performance quantifies the generalization gap. “Patient-held-out” will be used only when donor identity is verified.

**5. The ovarian context analysis is narrow and does not establish systematic evaluation of all variables.**

Agreed. Ovarian AK (60+)→AD (60+) versus AK→AL (40-) changes F1 0.289→0.072, gene Spearman 0.036→0.006, cell Spearman 0.206→0.095, and nonzero rate 0.166→0.027. Because age is nested within sample/patient and panels differ, we will call this an *age-associated, sample-confounded shift*, not a causal age effect.

To show the broader purpose of rich context, we applied Average/Worst/Gap audits to complementary cohorts:

| Task / encoder | Context; split | Average BA | Worst BA | Gap |
|---|---|---:|---:|---:|
| Bone marrow / scGPT | assay; patient-CV | 0.962 | 0.740 | 0.258 |
| Ten tissues / scGPT | dataset; patient-CV | 0.667--0.962 | 0.004--0.892 | max 0.975 |
| LUAD KRAS / CONCH | site; leave-one-site | 0.499 | 0.375 | 0.542 |
| LGG IDH / H-Optimus-0 | site; leave-one-site | 0.747 | 0.476 | 0.524 |

Rich metadata enable subgroup-support and worst-context audits; they do not themselves remove bias.

**6. Users need clear guidance on which fields are direct measurements and which are generated or post-processed.**

| Provenance | Examples |
|---|---|
| Direct | H&E; Xenium transcript coordinates/platform masks; Visium HD $2\,\mu$m bin counts; source metadata as reported. |
| Processed | Registration; CellViT footprints; Visium HD bin-to-cell matrices; crops; normalized matrices; splits; molecular-profile summaries. |
| Generated | Metadata-grounded GPT-4o captions and model predictions; neither is molecular ground truth. |

GPT-4o only verbalizes supplied fields. Independent PLIP/CONCH image--caption audits give matched-versus-mismatched AUC 0.993/0.988 and 100% Top-3 retrieval under both models. We will restore prompts, field checks, full examples, and audit matrices in the Appendix. Captions remain optional context and are excluded from the molecular ground truth.
