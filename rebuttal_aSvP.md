We sincerely thank the reviewer for recognizing the potential value of sMMC and for identifying the central question we had not answered clearly enough: what research becomes possible because the resource is larger, cell aligned, and context rich? This resource has accompanied me since my first PhD year, when I was new to spatial transcriptomics. As I approach graduation, I hope its unified release helps new students enter the field with fewer practical barriers.

We also clarify the new primary component. The current working manifest contains 99 unique samples (100 processing records) and 28,315,247 aligned targets. Our in-house DBiC-seq collection contributes 21 samples and 54,304 measured cells; paired-record QC removes 315, leaving 53,989 aligned cells. These records pair cell morphology, RNA, and cellular context. A broader continuing DBiC-seq pool now contains approximately 200,000 paired cells but is not counted in the table below.

| Component | Samples | Platforms | Aligned cells after QC |
|---|---:|---|---:|
| **New in-house primary data** | **21** | **DBiC-seq** | **53,989** |
| **Total sMMC-28M working manifest** | **99 unique (100 records)** | **Xenium; Visium HD; DBiC-seq** | **28,315,247** |

**Q1.** *The extent to which this new data can truly promote relevant research progress has not been fully explained.*

**A1 — Scale.** We agree. The resource is valuable not because one predictor has solved histology-to-expression inference, but because it makes previously difficult evaluations measurable.

- **Objective:** test the value of additional training cells under a fixed task.
- **Design:** on native-Xenium sample HHDX011, we fixed the spatial test regions, training-selected top-50 HVGs, and regression protocol while varying training cells for seven frozen encoders:

| Training fraction | 5% | 10% | 25% | 100% |
|---|---:|---:|---:|---:|
| Seven-encoder mean Gene Pearson | 0.177 | 0.205 | 0.241 | 0.278 |

- **Result/significance:** every encoder improved monotonically. This shows that additional cells are useful within this controlled range; we do not claim a universal scaling law.

**A2 — Breadth and biological calibration.**

- **Objective/design:** we now report all 25 release categories under the matched top-50-HVG, four-spatial-holdout protocol. The final row retains the primary 30-sample native-Xenium macro.

| Release category (evaluated species) | Image Gene P | Coordinate Gene P | Spatial KNN Gene P | Image Gene S | Image Cell P | Image F1 |
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

*Bold category labels denote mouse-only evaluations; mixed-species and non-mouse categories are explicit. Bold values in the three Gene-Pearson columns mark the best baseline. The 30-sample macro uses 360,000 spatially stratified cells, a 5% train--test buffer, and training-selected genes; its top-200 image Gene Pearson is 0.202 and top-50 bootstrap 95% CI is 0.277--0.368.*

- **Reproducibility:** the updated benchmark code, fixed splits, configurations, and evaluation scripts for this table are now available in our [anonymous GitHub repository](https://anonymous.4open.science/r/sMMC-22M-DB75).

- **Result:** image prediction exceeds coordinate and KNN controls in 28/30 samples. On marker/HVG overlap, image Gene Pearson is 0.201 versus 0.031 for coordinates and 0.028 for KNN. Cell-type-stratified pseudobulk RMSE is 0.120 for image prediction versus 0.224/0.187/0.188 for coordinates/KNN/training mean.
- **Claim boundary:** because cell-type labels are expression derived, this is aggregate biological calibration, not independent ground truth.

**A3 — Resolution.**

- **Design/result:** on six native-Xenium samples, matched cell/8/16/55-$\mu$m supervision gives Gene Pearson 0.365/0.365/0.363/0.330. In dense lung, 55.5%--66.4% of 55-$\mu$m pseudo-spots mix predicted cell types and contain 73.8%--81.0% of evaluated cells, whereas sparse heart shows only 3.5%--4.1% mixed spots and 7.0%--8.3% affected cells.
- **Significance:** cell-level evaluation exposes density-dependent mixing hidden by coarse averaging; it is not claimed to be universally easier.

**A4 — Overall scientific value.** Together, these experiments turn scale, resolution, and context from descriptive properties into testable variables:

- data scaling and leakage-controlled breadth across organs;
- organ/platform transfer and context-specific failure;
- aggregation-induced oversmoothing and spatial leakage.

**Q2.** *The increase of data scale, resolution, and contexts typically associates with increased heterogeneity, which may pose critical challenges for data analysis. Please clarify the heterogeneity and how it may influence downstream analysis.*

**A1 — Observed heterogeneity.** We agree and will make heterogeneity an explicit benchmark variable rather than a hidden nuisance.

| Dimension | Observed heterogeneity | Downstream consequence |
|---|---|---|
| Organ size | 26,366--3,559,793 aligned cells (135-fold) | Cell-weighted pooling can be dominated by large organs. |
| Gene coverage | 450--72,302 genes | Full-panel metrics are not directly comparable across panels. |
| Platform | Only 10/25 public tissue strata occur on both Xenium and Visium HD | Platform and biology can be confounded. |
| Spatial density | 55-$\mu$m mixing affects 7.0%--8.3% of cells in sparse heart but 73.8%--81.0% in dense lung | Spot averaging hides tissue-dependent heterogeneity. |
| Sample transfer | Across 18 same-organ held-out targets, top-50-HVG Gene Pearson averages 0.170 but ranges 0.016--0.372 | Mean performance can conceal severe target-sample failures. |
| Context | Ovarian AK (60+)→AD (60+) versus AK→AL (40-) changes F1 0.289→0.072 | Pooled scores can hide subgroup collapse. |

**A2 — Interpretation and protocol changes.**

- The ovarian comparison is age associated but sample/patient confounded, not causal. Incomplete donor identity also means the 18-target result is sample-held-out, not patient-held-out.
- We will report organ-macro and platform-separated results rather than only cell-weighted averages.
- We will distinguish spatial in-domain, sample-held-out, and verified patient-held-out protocols.
- We will report Average/Worst/Gap with sample support and disclose gene-panel overlap and metadata missingness.
- We will label every field as directly measured, processed, generated, or model output, and will not infer missing sensitive attributes.

**A3 — Significance.** These revisions clarify the scientific value: sMMC does not remove heterogeneity, but makes its downstream effects visible, measurable, and reproducible across organs, platforms, spatial resolutions, and biological contexts.
