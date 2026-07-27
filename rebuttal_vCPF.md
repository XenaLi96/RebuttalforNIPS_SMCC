We sincerely thank the reviewer. The three requests sharpen the revision: audit Visium HD construction, demonstrate breadth, and biologically calibrate low correlations. This resource has accompanied me since my first PhD year; near graduation, I hope its release helps newcomers enter spatial transcriptomics.

Our 21-sample in-house DBiC-seq collection adds 54,304 measured/53,989 post-QC cells with paired morphology, RNA, and context. The working manifest contains 99 unique samples (100 records) and 28,315,247 targets; a broader continuing DBiC-seq pool of approximately 200,000 paired cells is not counted here.

**Q1.** *For the Visium HD data, how reliable is the bin-to-cell aggregation? How are boundary bins assigned, and do segmentation errors noticeably affect expression profiles?*

**A1 — Construction.**

- We will distinguish native Xenium cells from *derived cell-aligned* Visium HD targets.
- Official transforms register native $2\,\mu$m bins to H&E. A closed bin square is assigned when it intersects a same-frame CellViT polygon; if several polygons claim it, the nearest full-resolution centroid wins, with raw-cell index breaking exact ties.
- Unsupported cells are removed. We will move this rule and a complete record example from Appendix S2 into the main text.

**A2 — Sensitivity audit.** We audited 3,000 raw polygons per dataset (9,000 total) under 1-$\mu$m registration shifts and mask erosion/dilation:

| Visium HD sample | Raw polygons with canonical bins | Shift-bin Jaccard | Shift-expression cosine | Shift median $|\Delta\mathrm{UMI}|$ |
|---|---:|---:|---:|---:|
| Human lung cancer | 49.4% | 0.727--0.733 | 0.954--0.957 | 11.1%--11.3% |
| Mouse brain | 97.5% | 0.806--0.816 | 0.936--0.939 | 6.0%--6.1% |
| Human pancreas | 50.0% | 0.706--0.714 | 0.994 | 13.0%--13.7% |

**A3 — Interpretation.**

- Percentages use all detected polygons; unsupported polygons are excluded.
- Shifts preserve expression direction but alter membership/UMIs; erosion/dilation gives Jaccard 0.462--0.720, cosine 0.871--0.993, and $|\Delta\mathrm{UMI}|$ 32.8%--66.6%.
- Uncertainty is material. We will release settings/QC, avoid ground-truth language, and report Xenium separately.

**Q2.** *The main experiments cover only a few representative cases. Can the authors summarize more organs or platforms?*

**A1 — Release-wide breadth.** Yes. Table 1 reports results for all 25 release categories.

**Table 1. Per-organ benchmark results.** Results use the top-50 training-selected HVGs, four contiguous spatial holdouts, and a 5% train–test buffer.

| Release category (evaluated species) | Image Gene P | Coordinate Gene P | Spatial KNN Gene P | Gene Spearman | Cell Pearson | F1 |
|---|---:|---:|---:|---:|---:|---:|
| **Bone (mouse)** | 0.030 | **0.124** | 0.055 | 0.037 | 0.180 | 0.240 |
| Brain (human + mouse) | **0.399** | 0.010 | 0.036 | 0.371 | 0.570 | 0.752 |
| Breast (human) | **0.400** | 0.053 | 0.035 | 0.375 | 0.433 | 0.487 |
| Cervical (human) | **0.178** | 0.017 | 0.014 | 0.157 | 0.247 | 0.026 |
| **Colon (mouse)** | **0.615** | -0.137 | 0.072 | 0.588 | 0.739 | 0.744 |
| Colorectal (human) | **0.431** | 0.056 | 0.083 | 0.419 | 0.532 | 0.623 |
| **Embryo (mouse)** | **0.278** | 0.037 | 0.045 | 0.251 | 0.517 | 0.342 |
| Head (zebrafish) | **0.216** | 0.022 | 0.031 | 0.193 | 0.429 | 0.287 |
| Heart (human) | **0.292** | 0.010 | 0.021 | 0.266 | 0.688 | 0.304 |
| Kidney (human) | **0.402** | 0.019 | 0.017 | 0.361 | 0.471 | 0.460 |
| Liver (human) | -0.002 | 0.003 | **0.010** | -0.002 | 0.561 | 0.400 |
| Lung (human) | **0.359** | 0.065 | 0.053 | 0.317 | 0.402 | 0.457 |
| Lymph Node (human) | **0.141** | 0.043 | 0.013 | 0.131 | 0.164 | 0.023 |
| Ovarian (human) | **0.250** | 0.122 | 0.104 | 0.239 | 0.312 | 0.234 |
| Ovarian glands (human) | **0.402** | 0.059 | 0.043 | 0.391 | 0.559 | 0.756 |
| Pancreas (human) | **0.324** | 0.090 | 0.068 | 0.272 | 0.505 | 0.275 |
| Pancreatic (human) | **0.366** | 0.080 | 0.051 | 0.341 | 0.540 | 0.694 |
| Pancreatic duct gland (human) | **0.331** | 0.091 | 0.100 | 0.282 | 0.408 | 0.386 |
| Plant (*A. thaliana*) | -0.011 | 0.004 | **0.012** | -0.009 | 0.385 | 0.052 |
| Prostate (human) | **0.263** | -0.015 | 0.025 | 0.244 | 0.263 | 0.083 |
| Seed (soybean) | -0.020 | -0.003 | **0.008** | -0.018 | 0.314 | 0.037 |
| Skin (human) | **0.359** | 0.048 | 0.086 | 0.299 | 0.437 | 0.368 |
| **Small Intestine (mouse)** | **0.572** | -0.104 | 0.067 | 0.548 | 0.701 | 0.715 |
| Tonsil (human) | **0.294** | 0.028 | 0.018 | 0.249 | 0.462 | 0.399 |
| Xenograft (human + mouse) | **0.387** | 0.041 | 0.049 | 0.362 | 0.526 | 0.589 |
| **30-sample macro** | **0.324** | 0.046 | 0.053 | **0.291** | **0.442** | **0.413** |

**A2 — Independent-sample evidence.**

- Image is strongest in 21/25 category rows; the four failures remain visible rather than being removed.
- Across 18 complete same-organ held-out targets, image top-50-HVG Gene Pearson averages 0.170 (0.016--0.372), Cell Pearson is 0.275 versus 0.205 for training mean, and F1 is 0.125 versus 0.031. These are sample-, not donor-held-out, results.
- The updated benchmark code, fixed splits, configurations, and evaluation scripts are now available in our [anonymous GitHub repository](https://anonymous.4open.science/r/sMMC-22M-DB75).

**A3 — Visium HD transfer.** Eight source-sample→target-sample organ pairs give:

| Eight-organ macro result | Gene P | Cell P | F1 |
|---|---:|---:|---:|
| UNI2-h | 0.0151 | 0.2036 | 0.0815 |
| Training mean | N/A | 0.2422 | 0.0375 |

- UNI2-h improves F1 but not Cell Pearson over the mean baseline; this transfer failure is intentionally not hidden.
- We will report organ-macro and per-organ values, separate Xenium from Visium HD, and avoid cell-weighted pooled claims.

**Q3.** *Some gene-wise correlations are low, especially under transfer. How should they be interpreted biologically, and could marker/cell-type genes or spatial-only/metadata-only baselines calibrate them?*

**A1 — Interpretation.**

- Low full-panel correlations show that histology is not a replacement for molecular measurement.
- They combine sparse or weakly morphology-linked genes with sample, composition, and acquisition shifts.
- A restricted training-selected HVG endpoint is easier and must not be conflated with all-gene transfer.

**A2 — Biological calibration.** The 30-sample spatially held-out predictions give:

| Metric | Image | Coordinate | Spatial KNN | Mean |
|---|---:|---:|---:|---:|
| Marker/HVG Gene Pearson $\uparrow$ | 0.201 | 0.031 | 0.028 | N/A |
| Cell-type-stratified pseudobulk RMSE $\downarrow$ | 0.120 | 0.224 | 0.187 | 0.188 |

**A3 — Hard-transfer boundary.**

- The marker inventory overlaps training-selected HVGs, and cell-type strata are expression-derived; these results show recoverable aggregate structure, not independent cell-type validation.
- Under the harder bidirectional all-gene lung/ovary transfer, image Gene Pearson averages are $-0.0003/0.0020$, whereas metadata-only baselines reach 0.104/0.142; coarse-state balanced accuracy is 0.139/0.159 for image prediction versus 0.220/0.252 for metadata.
- This negative result demonstrates context dependence and motivates stronger patient/platform-robust methods rather than supporting unrestricted cell-wise recovery.

**Q4.** *In-domain results may reflect local interpolation and spatial autocorrelation rather than robust generalization.*

**A.**

- We will call same-sample results *spatial interpolation diagnostics*.
- The 30-sample experiment uses contiguous holdouts and a 5% train--test buffer, while the 18-target and eight-organ experiments hold out complete samples; their lower and heterogeneous performance quantifies the generalization gap.
- “Patient-held-out” will be used only when donor identity is verified.

**Q5.** *The ovarian context analysis is narrow and does not establish systematic evaluation of all variables.*

**A1 — Scope of the ovarian result.**

- Ovarian AK (60+)→AD (60+) versus AK→AL (40-) changes F1 0.289→0.072, gene Spearman 0.036→0.006, cell Spearman 0.206→0.095, and nonzero rate 0.166→0.027.
- Because age is nested within sample/patient and panels differ, we will call this an *age-associated, sample-confounded shift*, not a causal age effect.

**A2 — Broader context audit.** To show the broader purpose of rich context, we applied Average/Worst/Gap audits to complementary cohorts:

| Task / encoder | Context; split | Average BA | Worst BA | Gap |
|---|---|---:|---:|---:|
| Bone marrow / scGPT | assay; patient-CV | 0.962 | 0.740 | 0.258 |
| Ten tissues / scGPT | dataset; patient-CV | 0.667--0.962 | 0.004--0.892 | max 0.975 |
| LUAD KRAS / CONCH | site; leave-one-site | 0.499 | 0.375 | 0.542 |
| LGG IDH / H-Optimus-0 | site; leave-one-site | 0.747 | 0.476 | 0.524 |

**A3 — Significance.** Rich metadata enable subgroup-support and worst-context audits; they do not themselves remove bias.

**Q6.** *Users need clear guidance on which fields are direct measurements and which are generated or post-processed.*

**A1 — Provenance classes.**

| Provenance | Examples |
|---|---|
| Direct | H&E; Xenium transcript coordinates/platform masks; Visium HD $2\,\mu$m bin counts; source metadata as reported. |
| Processed | Registration; CellViT footprints; Visium HD bin-to-cell matrices; crops; normalized matrices; splits; molecular-profile summaries. |
| Generated | Metadata-grounded GPT-4o captions and model predictions; neither is molecular ground truth. |

**A2 — Generated-text validation and scope.**

- GPT-4o only verbalizes supplied fields.
- PLIP/CONCH audits give AUC 0.993/0.988 and 100% Top-3 retrieval.
- We will restore prompts, field checks, examples, and matrices in the Appendix.
- Captions remain optional context, not molecular ground truth.
