We thank the reviewer for identifying the central issues: molecular-target provenance, experimental breadth, and what cell alignment adds beyond spot prediction. We now distinguish native cell-resolved measurements from derived Visium HD targets, report all 25 release categories, and retain weak transfer results alongside positive findings.

**Q1 (P2, Q2, Q4, L1–L2).** *Are the molecular targets truly single-cell?*

**A1 — Molecular-target provenance.** The resource contains three evidence types:

| Component | Evidence unit | Count | Status |
|---|---|---:|---|
| Public Xenium | Platform-segmented cells and transcripts | 16,314,129 cells | Native cell-resolved |
| Public Visium HD | Native 2-µm bins aggregated to cell masks | 7,629,697 derived cells | Derived cell-aligned |
| New in-house DBiC-seq | Paired cell morphology and RNA | 53,989 post-QC cells from 21 samples | Native cell-resolved |

- All non-HD data are native cell-resolved measurements: Xenium and DBiC-seq assign an expression profile to each segmented cell. Only Visium HD uses bin-to-cell aggregation, following 10x Genomics’ [officially documented mapping](https://www.10xgenomics.com/support/software/space-ranger/latest/analysis/segmented-outputs) from native 2-µm barcodes to segmented cells. The new in-house DBiC-seq data are available for [download and preview](https://anonymous.4open.science/r/sMMC-22M-DB75/README.md).
- We will therefore not describe the entire collection as directly measured single-cell data. We will consistently use “native cell-resolved” for Xenium/DBiC-seq and “derived cell-aligned” for Visium HD, and retitle the work *sMMC: A Cell-Aligned Multimodal Resource for Spatial Transcriptomics*.

**A2 — Native-cell validation.** To directly address this concern, we reran the benchmark using only native Xenium data; Table 1 reports the 25-organ results. Table 2 separately reports cross-sample organ transfer on Visium HD.

**Q2 (P3, Q1).** *Are all 25 release categories evaluated?*

**A1 — 25-organ results.** Table 1 reports results for all 25 organs under one fixed protocol.

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

**A2 — Cross-sample transfer.** We additionally evaluated sample-to-sample transfer across eight organs.

**Table 2. Eight-organ cross-sample Visium HD benchmark (source→target sample).** TM is training mean; its Gene Pearson is undefined.

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

- The cross-sample Gene Pearson is indeed low (organ macro 0.0151), and UNI2-h improves F1 but not Cell Pearson over the training mean. We do not claim to have solved this setting.
- The low Pearson is not evidence that the dataset is invalid: the in-domain benchmark and separate construction audit establish usable targets. Instead, this negative result exposes the still-unsolved difficulty of single-cell generalization under patient, platform, composition, acquisition, and morphology shifts.
- The dataset makes this failure measurable and provides a basis for future solutions. We will use “cross-patient” only for verified donors and otherwise say “sample-held-out.”

**Q3 (Q4).** *What techniques were used to interpolate or match spot-level gene expression to individual cells?*

**A.**

- This premise does not apply to our main benchmark: its input is histology alone and its target is a cell-resolved RNA profile. No spot-level gene-expression measurement is supplied to the model. Training directly pairs each cell's RNA profile with two cell-centered histology crops: a local crop around the cell and a larger crop capturing tissue context.
- STBoost is therefore not a spot-to-cell expression-interpolation method. It adapts existing image-to-spot predictors to cell-level prediction by replacing the spot-centered image input with these two hierarchical cell-centered crops, fusing their representations, and retaining the corresponding spot-level method's downstream prediction modules.
- Because this is primarily a dataset paper, we placed the full architecture and training details in the Appendix. We agree that the distinction was not sufficiently clear and will add a concise description of the task input, target, training, and inference procedure to the main body.

**Q4 (P5, P6, minor remarks).** *Was Figure 1 AI-generated, and how will the writing and figure-presentation issues be addressed?*

**A1 — Figure 1 provenance.** Figure 1 was not AI-generated. I began collecting this resource in the first year of my PhD and have continued developing it for several years. I manually drew and revised two versions of Figure 1 in Sketch; together they contain 162 editable layers and required more than one week of work. Screenshots documenting the editable layers are available in our [anonymous GitHub repository](https://anonymous.4open.science/r/sMMC-22M-DB75) under `figure1_design_evidence/`.

**A2 — Presentation revisions.** We thank the reviewer for identifying these issues and will address them individually:

- correct the capitalization, missing letter/space, and blank elements in Figure 1, and expand its caption;
- define “study split,” “cell unit,” and the Figure 2b training ratio at first use;
- remove the Figure 3 border, render its labels as vector text, make the requested line-level cuts, and state precisely which parts of the resource are publicly released.

**Q5 (P4, P9).** *What can the context analysis support, and how complete is the demographic metadata?*

**A1 — Context claim.**

- The ovarian AK/AD/AL comparison cannot establish an age effect because age is nested within sample/patient and panels differ.
- We will rename it “sample/age-confounded context shift.”
- Its role is to motivate Average/Worst/Gap/Support auditing, not causal attribution.

**A2 — Metadata boundary.**

- Donor, age, sex, and disease are incompletely reported across sources, and ethnicity is undocumented.
- We will report missingness, never infer sensitive attributes, and analyze subgroups only with adequate independent-donor support.

**Q6 (P5, P7, minor comments).** *Please clarify terminology and correct the remaining presentation issues.*

**A1 — Terminology.**

- STBoost is the model-agnostic cell-aligned interface.
- STBoosted BLEEP is published BLEEP retrained through it.
- STBoost-Ref is our image-only retrieval reference predictor; at inference it receives local/context histology, never query expression.

**A2 — Presentation and limitations.**

- We will use these names consistently, define every split/unit, and correct Figure 1 and Figure 3.
- We will state the limits of spatial autocorrelation, sample/platform shift, HD boundary assignment, metadata missingness, and prediction as a substitute for measurement.

**Q7 (Q3, L3).** *What does cell-aligned evaluation add beyond spot-centered prediction?*

**A1 — Matched resolution control.**

- We constructed 8-, 16-, and 55-µm pseudo-spots from the same native-Xenium cells while holding the image representation, target genes, and evaluation protocol fixed across six samples.
- Mean Gene Pearson was 0.365 at cell level, 0.365 at 8 µm, 0.363 at 16 µm, and 0.330 at 55 µm. We therefore do not claim that cell-level prediction is universally more accurate than spot-level prediction.

**A2 — What averaging hides.**

- In full-density lung HLCX022, 55-µm aggregation mixed cell types in 55.5–66.4% of test pseudo-spots, containing 73.8–81.0% of evaluated cells. In the much sparser HHDX011 sample, only 3.5–4.1% of pseudo-spots were mixed, containing 7.0–8.3% of cells.
- The benefit of cell alignment is therefore density dependent: it preserves within-region cellular heterogeneity that coarse averaging can conceal, even when aggregate correlation changes little. We will replace the vague limitation identified by the reviewer with this measured statement.

**Q8 (P6).** *How are cells detected and aligned when the platform does not provide cell boundaries, and how reliable is the resulting Visium HD assignment?*

**A1 — Platform-specific construction.**

- Xenium supplies native cell boundaries and transcript coordinates; no spot-to-cell expression inference is used.
- For Visium HD, official spatial transforms register native 2-µm bins to H&E. A published CellViT model supplies same-frame cell contours; intersecting bins are aggregated within each contour, conflicts are assigned to the nearest full-resolution cell centroid, and unsupported polygons are excluded. Thus there is no single spot center that is simply matched to a cell center.

**A2 — Assignment sensitivity.** We audited 3,000 raw CellViT polygons in each of three Visium HD samples (9,000 total) under ±1-µm registration shifts:

| Visium HD sample | Polygons with canonical bins | Shift-bin Jaccard | Shift-expression cosine | Shift median absolute UMI change |
|---|---:|---:|---:|---:|
| Human lung cancer | 49.4% | 0.727–0.733 | 0.954–0.957 | 11.1–11.3% |
| Mouse brain | 97.5% | 0.806–0.816 | 0.936–0.939 | 6.0–6.1% |
| Human pancreas | 50.0% | 0.706–0.714 | 0.994 | 13.0–13.7% |

- Among supported polygons, small shifts largely preserve expression direction, but coverage is sample dependent. Mask erosion/dilation is more disruptive (Jaccard 0.462–0.720; expression cosine 0.871–0.993; median absolute UMI change 32.8–66.6%).
- We will move the segmentation, transform, conflict-resolution, filtering, and sensitivity details into the main body; release the corresponding coverage/QC fields; and continue to report native Xenium separately from derived HD evidence.

**Q9 (P8, P9, L4).** *How many independent patients or animals support the experiments, and how complete is the demographic metadata?*

**A1 — Verified evaluation units.**

| Evaluation | Verifiable sample support | Patient/animal interpretation |
|---|---:|---|
| Native-Xenium breadth benchmark | 30 H&E-aligned samples | Independent donor/animal IDs are not consistently reported upstream |
| Eight-organ HD transfer | 16 source/target samples in 8 pairs | Sample-held-out; donor relationships are unverified |

- We cannot responsibly convert sample records into a unique patient or animal count when upstream identifiers are absent. We will add per-organ sample-record counts alongside cell counts, mark unavailable donor/animal fields explicitly, and use “patient-held-out” only where independent donor identity is verified.

**A2 — Demographic coverage.**

- As stated in Q5, age, sex, and disease are retained only when reported by the source, while ethnicity is undocumented in the current manifest. We will report field-level missingness rather than infer sensitive attributes, and no demographic comparison will be presented as independently supported without multiple verified donors per group.

**Q10 (P7, second Q4).** *How will STBoost and the changing resource size be named?*

**A.**

- We will define STBoost at its first appearance in both the abstract and Introduction, introduce BLEEP before the first comparison, and use STBoost, STBoosted BLEEP, and STBoost-Ref consistently instead of the undefined label “Ours.”
- To avoid 22M/23M becoming obsolete as the resource grows, we will use the count-independent name *sMMC* and report the exact frozen release version and platform-resolved counts in the manifest.

**Q11 (P3–P5, Q1).** *How will the revised main body distinguish the three experimental purposes and reduce redundancy?*

**A.**

- The resolution evidence—native-Xenium targets, the 25-category benchmark, and the matched pseudo-spot control—addresses whether cell-aligned evaluation preserves cell-specific molecular heterogeneity.
- The context experiment is orthogonal: it tests whether a trained model remains reliable across recorded biological contexts; it is not offered as evidence that the targets are single-cell.
- We will place a compact 25-category result summary, the histology-only input and cell-expression target, split definitions, and platform-specific alignment/QC in the main body, while removing repeated scale/resolution/context framing and the line-level redundancies identified by the reviewer.
