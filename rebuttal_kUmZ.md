We thank the reviewer for focusing on target provenance, experimental breadth, and the value of cell alignment. Because this rebuttal is length-limited, we answer concisely here and will respond to each point in greater detail once the open-comment period begins. We distinguish native from derived targets, report all 25 categories, and retain difficult transfer results.

**Q1 (P2, Q2, Q4, L1–L2).** *Are the molecular targets truly single-cell?*

**A1 — Molecular-target provenance.** The resource contains three evidence types:

| Component | Evidence unit | Count | Status |
|---|---|---:|---|
| Public Xenium | Platform-segmented cells and transcripts | 16,314,129 cells | Native cell-resolved |
| Public Visium HD | Native 2-µm bins aggregated to cell masks | 7,629,697 derived cells | Derived cell-aligned |
| New in-house DBiC-seq | Paired cell morphology and RNA | 53,989 post-QC cells from 21 samples | Native cell-resolved |

- Xenium and DBiC-seq are native cell-resolved; only HD aggregates native 2-µm bins to segmented cells using 10x Genomics’ [documented mapping](https://www.10xgenomics.com/support/software/space-ranger/latest/analysis/segmented-outputs). The in-house DBiC-seq data are available for [download](https://anonymous.4open.science/r/sMMC-22M-DB75/README.md).
- We will not describe the entire resource as directly measured single-cell data or use unqualified “single-cell predictor” claims. We will call Xenium/DBiC-seq “native cell-resolved,” HD “derived cell-aligned,” and retitle the work *sMMC: A Cell-Aligned Multimodal Resource for Spatial Transcriptomics*.

**A2 — Derived HD construction and audit.**

- For HD, official transforms register native 2-µm bins to H&E-aligned CellViT contours; intersecting bins are aggregated, conflicts go to the nearest centroid, and unsupported polygons are excluded. These are *derived cell-aligned targets*, not native single-cell measurements.
- We audited 3,000 raw polygons per dataset (9,000 total) under ±1-µm registration shifts:

| Visium HD example | Raw polygons with canonical bins | Shift-bin Jaccard | Shift-expression cosine | Shift median $|\Delta\mathrm{UMI}|$ |
|---|---:|---:|---:|---:|
| Human lung cancer | 49.4% | 0.727–0.733 | 0.954–0.957 | 11.1%–11.3% |
| Mouse brain | 97.5% | 0.806–0.816 | 0.936–0.939 | 6.0%–6.1% |
| Human pancreas | 50.0% | 0.706–0.714 | 0.994 | 13.0%–13.7% |

Among supported polygons, expression direction is stable, while bin membership and UMI counts remain boundary sensitive. We will move the construction rule, audit, and QC into the main body.

**Q2 (P3, Q1).** *Are all 25 release categories evaluated?*

**A1 — 25-category results.** Yes. Table 1 reports all 25 release categories under one fixed protocol.

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

- Cross-sample prediction is substantially more difficult than within-sample spatial holdout, and the results are not yet satisfactory: macro Gene Pearson is 0.0151, while UNI2-h improves F1 but not Cell Pearson over the mean.
- These results reflect the difficulty of sample/platform/composition and acquisition shifts for current models. We report them transparently beside Table 1: Table 1 evaluates spatially held-out signal recovery, whereas Table 2 evaluates transfer across complete samples.

**Q3 (P6, P7).** *What are the benchmark inputs, experimental design, and STBoost definitions?*

**A.**

- **Input/output:** the input is histology alone and the output is the RNA-expression vector of an aligned cell; no spot-level expression is supplied at inference. Each training target is paired with a local cell crop and a larger tissue-context crop.
- **Protocols/metrics:** within-sample spatial holdout, cross-sample, verified cross-patient, and cross-platform protocols test progressively harder shifts using fixed targets and splits. We report gene-wise and cell-wise Pearson/Spearman together with expression-detection F1.
- **STBoost:** this framework lifts existing spot-level image-to-expression methods to cell-level prediction by replacing their spot interface with hierarchical cell/context images and cell-resolved targets while retaining their prediction modules. Every BLEEP comparison in our tables is therefore STBoosted BLEEP; STBoost-Ref is our image-only reference predictor.
- Because this is a dataset paper, we originally placed the detailed STBoost formulation, equations, and architecture figure in the Appendix. We will define STBoost and BLEEP at first mention, remove the ambiguous label “Ours,” and move a concise architecture and formulation description into the main body.

**Q4 (Q3, L3).** *What does cell alignment add beyond spot-centered prediction?*

**A.** We thank the reviewer for this important suggestion.

- Across six native-Xenium samples, we held the pipeline fixed and changed only target scale (cell/8/16/55 µm): Gene Pearson was 0.365/0.365/0.363/0.330, showing averaging can retain a similar global score.
- In two representative samples, 55-µm pseudo-spots mixed cell types in 55.5–66.4% of regions and affected 73.8–81.0% of cells in dense tissue, versus 3.5–4.1% and 7.0–8.3% in sparse tissue.
- Cell alignment reveals heterogeneity hidden by averaging and enables cell-resolved studies such as cell–cell communication, virtual-cell perturbation prediction, and tracing perturbation effects through interactions across spatial tissue.

**Q5 (P4, P8, P9, L4).** *What do the context analysis and available metadata support?*

**A1 — Representativeness.** We agree with P9 that we should not assume existing datasets are representative of ethnicity, sex/gender, or other human traits. Ethnicity is undocumented in the upstream records, and other context fields have variable coverage. We will report support and missingness, never infer absent sensitive attributes, and avoid population-level fairness claims unsupported by the data.

**A2 — Why context-bias evaluation matters.** We nevertheless believe that context-aware audits are important for biomedical and pathology foundation models: a strong average score can hide substantial failure in a specific assay, dataset, acquisition site, age group, or disease context. We tested this with patient-CV or leave-one-site-out evaluation and report Average/Worst/Gap:

| Domain | Task / encoder | Context; split | Avg. BA | Worst BA | Gap |
|---|---|---|---:|---:|---:|
| Single-cell | Bone marrow / Geneformer | assay; patient-CV | 0.938 | 0.669 | 0.327 |
| Single-cell | Bone marrow / scGPT | assay; patient-CV | 0.962 | 0.740 | 0.258 |
| Single-cell | Bone marrow / scVI-style | assay; patient-CV | 0.932 | 0.616 | 0.373 |
| Single-cell | Ten tissues / scGPT | dataset; patient-CV | 0.667–0.962 | 0.004–0.892 | max 0.975 |
| Pathology | LUAD KRAS / CONCH | site; leave-one-site | 0.499 | 0.375 | 0.542 |
| Pathology | LGG IDH / UNI | site; leave-one-site | 0.682 | 0.464 | 0.471 |
| Pathology | LGG IDH / H-Optimus-0 | site; leave-one-site | 0.747 | 0.476 | 0.524 |

These examples show that context can materially change model performance and that structured metadata make such bias measurable.

**A3 — Reporting scope.**

- The breadth benchmark uses 30 native-Xenium samples; HD transfer uses 16 source/target samples in eight pairs. Upstream donor/animal IDs are incomplete, so these are sample-held-out unless donor independence is verified.
- Because age is nested within sample/patient and gene panels differ, AK/AD/AL supports a *sample/age-confounded context shift*, not a causal age effect. We will require multiple verified donors per subgroup comparison and report per-organ sample records beside cell counts.

**Q6 (P5, second Q4, minor remarks).** *How will naming, Figure 1, and presentation be revised?*

**A1 — Figure 1.** Figure 1 was not AI-generated. I began this resource in my first PhD year. Over the following three years, the figure went through two manually drawn Sketch versions and now contains 162 editable vector layers. Screenshots of the layer workspace are available in our [anonymous GitHub repository](https://anonymous.4open.science/r/sMMC-22M-DB75) under `figure_evidence_not_AI/`.

**A2 — Revisions.**

- We will use the count-independent name *sMMC*, with exact frozen counts in the manifest; move the 25-category summary, task input/target, splits, and alignment/QC into the main body; and remove repeated framing.
- We will correct Figure 1 text/blanks and expand its caption; define “study split,” “cell unit,” and the Figure 2b ratio; remove the Figure 3 border and rasterized text; make the requested line cuts; and state exactly which resource components are public.
