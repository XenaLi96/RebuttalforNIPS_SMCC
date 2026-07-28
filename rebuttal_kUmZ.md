We thank the reviewer for emphasizing target provenance, breadth, and cell alignment. We separate native/derived targets, report all 25 categories, and retain difficult transfer results. We hope the reviewer will reconsider after our point-by-point response. Under the character limit, we give central evidence here and fuller responses during open comments.

**Q1 (P2, Q2, Q4, L1–L2).** *Are the molecular targets truly single-cell?*

**A1 — Molecular-target provenance.** The resource has three evidence types:

| Component | Evidence unit | Count | Status |
|---|---|---:|---|
| Public Xenium | Platform-segmented cells and transcripts | 16,314,129 cells | Native cell-resolved |
| Public Visium HD | Native 2-µm bins aggregated to cell masks | 7,629,697 derived cells | Derived cell-aligned |
| New in-house DBiC-seq | Paired cell morphology and RNA | 53,989 post-QC cells from 21 samples | Native cell-resolved |

- We added collaborator-provided DBiC-seq data: 53,989 post-QC cells/21 samples with paired morphology and RNA, available for [download](https://anonymous.4open.science/r/sMMC-22M-DB75/README.md).
- We will call Xenium/DBiC-seq “native cell-resolved” and HD “derived cell-aligned,” remove unqualified “single-cell predictor” claims, and retitle the work *sMMC: A Cell-Aligned Multimodal Resource for Spatial Transcriptomics*.

**A2 — Derived HD construction and audit.**

- Following 10x Genomics’ [documented mapping](https://www.10xgenomics.com/support/software/space-ranger/latest/analysis/segmented-outputs), HD registers native 2-µm bins to H&E-aligned CellViT contours, aggregates intersections, assigns conflicts to the nearest centroid, and excludes unsupported polygons. These are *derived cell-aligned targets*.
- We audited 3,000 polygons/dataset (9,000 total) under ±1-µm shifts:

| Visium HD example | Raw polygons with canonical bins | Shift-bin Jaccard | Shift-expression cosine | Median absolute change in UMI count |
|---|---:|---:|---:|---:|
| Human lung cancer | 49.4% | 0.727–0.733 | 0.954–0.957 | 11.1%–11.3% |
| Mouse brain | 97.5% | 0.806–0.816 | 0.936–0.939 | 6.0%–6.1% |
| Human pancreas | 50.0% | 0.706–0.714 | 0.994 | 13.0%–13.7% |

Expression direction is stable among supported polygons, while bin membership/UMIs remain boundary-sensitive. We will move this construction and QC into the main body.

**Q2 (P3, Q1).** *Are all 25 release categories evaluated?*

**A1 — 25-category results.** Yes; Table 1 reports all 25 categories under one protocol.

**Table 1. Per-organ benchmark results.** Results use the top-50 training-selected HVGs, four contiguous spatial holdouts, and a 5% train–test buffer.

| Release category (evaluated species) | Image Gene P | Coordinate Gene P | Spatial KNN Gene P | Gene Spearman | Cell Pearson | F1 |
|---|---:|---:|---:|---:|---:|---:|
| **Bone (mouse)** | 0.030 | **0.124** | 0.055 | 0.037 | 0.180 | 0.240 |
| Brain (human + mouse) | **0.399** | 0.010 | 0.036 | 0.371 | 0.570 | 0.752 |
| Breast (human) | **0.400** | 0.053 | 0.035 | 0.375 | 0.433 | 0.487 |
| Cervical (human) | **0.178** | 0.017 | 0.014 | 0.157 | 0.247 | 0.026 |
| **Colon (mouse)** | **0.615** | −0.137 | 0.072 | 0.588 | 0.739 | 0.744 |
| Colorectal (human) | **0.431** | 0.056 | 0.083 | 0.419 | 0.532 | 0.623 |
| **Embryo (mouse)** | **0.278** | 0.037 | 0.045 | 0.251 | 0.517 | 0.342 |
| Head (zebrafish) | **0.216** | 0.022 | 0.031 | 0.193 | 0.429 | 0.287 |
| Heart (human) | **0.292** | 0.010 | 0.021 | 0.266 | 0.688 | 0.304 |
| Kidney (human) | **0.402** | 0.019 | 0.017 | 0.361 | 0.471 | 0.460 |
| Liver (human) | −0.002 | 0.003 | **0.010** | −0.002 | 0.561 | 0.400 |
| Lung (human) | **0.359** | 0.065 | 0.053 | 0.317 | 0.402 | 0.457 |
| Lymph Node (human) | **0.141** | 0.043 | 0.013 | 0.131 | 0.164 | 0.023 |
| Ovarian (human) | **0.250** | 0.122 | 0.104 | 0.239 | 0.312 | 0.234 |
| Ovarian glands (human) | **0.402** | 0.059 | 0.043 | 0.391 | 0.559 | 0.756 |
| Pancreas (human) | **0.324** | 0.090 | 0.068 | 0.272 | 0.505 | 0.275 |
| Pancreatic (human) | **0.366** | 0.080 | 0.051 | 0.341 | 0.540 | 0.694 |
| Pancreatic duct gland (human) | **0.331** | 0.091 | 0.100 | 0.282 | 0.408 | 0.386 |
| Plant (*A. thaliana*) | −0.011 | 0.004 | **0.012** | −0.009 | 0.385 | 0.052 |
| Prostate (human) | **0.263** | −0.015 | 0.025 | 0.244 | 0.263 | 0.083 |
| Seed (soybean) | −0.020 | −0.003 | **0.008** | −0.018 | 0.314 | 0.037 |
| Skin (human) | **0.359** | 0.048 | 0.086 | 0.299 | 0.437 | 0.368 |
| **Small Intestine (mouse)** | **0.572** | −0.104 | 0.067 | 0.548 | 0.701 | 0.715 |
| Tonsil (human) | **0.294** | 0.028 | 0.018 | 0.249 | 0.462 | 0.399 |
| Xenograft (human + mouse) | **0.387** | 0.041 | 0.049 | 0.362 | 0.526 | 0.589 |
| **30-sample macro** | **0.324** | 0.046 | 0.053 | **0.291** | **0.442** | **0.413** |

**A2 — Cross-sample transfer.** We evaluated eight source→target organ pairs.

**Table 2. Eight-organ cross-sample benchmark (source→target sample).** TM is training mean; its Gene Pearson is undefined.

| Organ | Pair | UNI2-h Gene P | UNI2-h Cell P | UNI2-h F1 | TM Cell P | TM F1 |
|---|---|---:|---:|---:|---:|---:|
| Human breast | j→m | 0.0272 | 0.1109 | 0.1794 | 0.1108 | 0.1215 |
| Human ovary | ad→ak | 0.0267 | 0.4593 | 0.1161 | 0.4906 | 0.0561 |
| Human lung | k→d | 0.0143 | 0.2121 | 0.0960 | 0.1455 | 0.0236 |
| Human pancreas | g→ag | 0.0040 | 0.0090 | 0.0389 | 0.0023 | 0.0168 |
| Human tonsil | u→i | 0.0141 | 0.2769 | 0.0708 | 0.3264 | 0.0346 |
| Mouse brain | e→ah | 0.0243 | 0.2016 | 0.0623 | 0.2238 | 0.0179 |
| Mouse embryo | b→ai | 0.0048 | 0.0913 | 0.0369 | 0.1683 | 0.0134 |
| Mouse kidney | a→aj | 0.0049 | 0.2678 | 0.0513 | 0.4696 | 0.0159 |
| **Organ macro** | — | **0.0151** | **0.2036** | **0.0815** | **0.2422** | **0.0375** |

- Cross-sample results remain unsatisfactory (macro Gene Pearson 0.0151), reflecting sample/platform/composition and acquisition shifts. Table 1 tests spatially held-out regions; Table 2 tests complete-sample transfer.

**Q3 (P6, P7).** *What are the benchmark inputs, experimental design, and STBoost definitions?*

**A.**

- **Input/output:** histology alone predicts aligned-cell RNA; no molecular input is used at inference. Each target has local-cell and tissue-context crops.
- **Protocols/metrics:** spatial holdout, cross-sample, verified cross-patient, and cross-platform tests use fixed splits; metrics are gene/cell Pearson/Spearman and expression-detection F1.
- **STBoost:** hierarchical cell/context images and cell-resolved targets replace a method’s spot interface while its predictor is retained. Table BLEEP is STBoosted BLEEP; STBoost-Ref is our image-only reference.
- Because this ED-track submission is a dataset paper, we initially placed the detailed STBoost method, equations, and architecture in the Appendix. This reflected ED-track framing, not omitted methods. We will now define STBoost and BLEEP at first mention, replace “Ours,” and move a concise formulation/architecture into the main body.

**Q4 (Q3, L3).** *What does cell alignment add beyond spot-centered prediction?*

**A.** We thank the reviewer for this suggestion.

- Across six native-Xenium samples, fixed-pipeline cell/8/16/55-µm targets gave Gene Pearson values of 0.365, 0.365, 0.363, and 0.330. Yet, in two representative samples, 55-µm pseudo-spots mixed cell types in 55.5–66.4% of regions and affected 73.8–81.0% of cells in dense tissue, versus 3.5–4.1% and 7.0–8.3% in sparse tissue.
- Thus, even when global correlation changes little, cell alignment preserves cell identity and local heterogeneity needed for cell–cell communication; links morphology, RNA, and perturbation response at the cell unit for virtual-cell models; and permits tracing perturbation effects through interacting cells across spatial tissue. These analyses are ill-posed when heterogeneous cells are collapsed into one spot.

**Q5 (P4, P8, P9, L4).** *What do the context analysis and available metadata support?*

**A1 — Representativeness.** We agree representation cannot be assumed. Ethnicity is undocumented and context coverage varies. We will report missingness, infer no absent sensitive attributes, and avoid unsupported population-level fairness claims.

**A2 — Context-bias evaluation.** Average scores can hide failures by assay, dataset, site, age, or disease. Patient-CV/leave-one-site-out audits report Average/Worst/Gap:

| Domain | Task / encoder | Context; split | Avg. BA | Worst BA | Gap |
|---|---|---|---:|---:|---:|
| Single-cell | Bone marrow / 3 encoders | assay; patient-CV | 0.932–0.962 | 0.616–0.740 | 0.258–0.373 |
| Single-cell | Ten tissues / scGPT | dataset; patient-CV | 0.667–0.962 | 0.004–0.892 | max 0.975 |
| Pathology | LUAD/LGG / 3 encoders | site; leave-one-site | 0.499–0.747 | 0.375–0.476 | 0.471–0.542 |

Context materially changes performance; structured metadata make this measurable.

**A3 — Reporting scope.**

- Breadth uses 30 native-Xenium samples; HD transfer uses 16 samples/eight pairs. With incomplete donor/animal IDs, splits are sample-held-out unless independence is verified.
- Because age is nested within sample/patient and panels differ, AK/AD/AL is a *sample/age-confounded shift*, not causal. We will require multiple verified donors per subgroup.

**Q6 (P5, second Q4, minor remarks).** *How will naming, Figure 1, and presentation be revised?*

**A1 — Figure 1.** Figure 1 was not AI-generated. I began this resource in my first PhD year; over three years, the figure had two manually drawn Sketch versions and now has 162 editable vector layers. Layer-workspace screenshots are in our [anonymous repository](https://anonymous.4open.science/r/sMMC-22M-DB75) under `figure_evidence_not_AI/`.

**A2 — Revisions.**

- We will use the count-independent name *sMMC*, freeze counts in the manifest, and move category, task, split, and alignment/QC details into the main body.
- We will correct/expand Figure 1; define split, cell-unit, and ratio terms; fix Figure 3; make the requested cuts; and identify public components.
