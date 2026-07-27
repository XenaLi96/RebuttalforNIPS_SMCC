We thank the reviewer for identifying the central issues: molecular-target provenance, experimental breadth, and what cell alignment adds beyond spot prediction. We now distinguish native cell-resolved measurements from derived Visium HD targets, report all 25 release categories, and retain weak transfer results alongside positive findings.

**Q1 (P2, Q2, Q4, L1–L2).** *Are the molecular targets truly single-cell?*

**A1 — Molecular-target provenance.** The resource contains three evidence types:

| Component | Evidence unit | Count | Status |
|---|---|---:|---|
| Public Xenium | Platform-segmented cells and transcripts | 16,314,129 cells | Native cell-resolved |
| Public Visium HD | Native 2-µm bins aggregated to cell masks | 7,629,697 source records → 3,932,040 profiles | Derived cell-aligned |
| New in-house DBiC-seq | Paired cell morphology and RNA | 53,989 post-QC cells from 21 samples | Native cell-resolved |

- All non-HD data are native cell-resolved measurements: Xenium and DBiC-seq assign an expression profile to each segmented cell. Only the Visium HD component uses bin-to-cell aggregation.
- We will therefore not describe the entire collection as directly measured single-cell data. We will consistently use “native cell-resolved” for Xenium/DBiC-seq and “derived cell-aligned” for Visium HD, and retitle the work *sMMC: A Cell-Aligned Multimodal Resource for Spatial Transcriptomics*.

**A2 — Native-cell validation.** Our strongest new validation uses 30 native-Xenium samples (360,000 cells), four contiguous spatial holdouts, a 5% train–test buffer, and training-only gene selection. Images beat both coordinate-only and spatial-KNN controls in 28/30 samples and 15/17 analysis labels; all failures are retained in Table 1. Thus the native-cell evidence does not depend on HD bin assignment, while HD remains separately labeled derived evidence.

**Q2 (P3, Q1).** *Are all 25 release categories evaluated?*

**A1 — 25-category breadth.** Yes. Table 1 reports every release category under one fixed protocol, including negative results.

**Table 1. Per-organ cell-aligned breadth benchmark.** Rows follow the 25-category release taxonomy. Results use top-50 training-selected HVGs, four contiguous spatial holdouts, and a 5% train–test buffer; the final row is the 30-sample native-Xenium macro.

| Release category (evaluated species) | Image Gene P | Coordinate Gene P | Spatial KNN Gene P | Image Gene S | Image Cell P | Image F1 |
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

*Among category rows, bold names are mouse-only. Bold Gene-Pearson values mark the best of image, coordinate, and spatial KNN. Native-Xenium top-200 image Gene Pearson is 0.202; the top-50 95% CI is 0.277–0.368.*

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

**Q3 (P4, Q3, L3).** *What does cell-level evaluation add beyond spot-level prediction?*

**A.**

- Spot averaging makes prediction easier, so cell-level correlation need not be higher.
- In six native-Xenium samples, Gene Pearson was 0.365 at native-cell and 8-µm supervision, 0.363 at 16 µm, and 0.330 at 55 µm; all declined at 55 µm.
- In dense lung HLCX022, 55-µm grids mixed labels in 55.5–66.4% of test spots, versus 3.5–4.1% in sparse HHDX011.
- Thus cell alignment exposes density-dependent heterogeneity; we do not claim universal cell-over-spot accuracy.

**Q4 (P6, Q4).** *How reliable are HD localization and expression assignment?*

**A.**

- Xenium uses platform boundaries/transcripts. HD uses the official transform, CellViT footprints, and native 2-µm bins; conflicts go to the nearest centroid.
- Across two lungs and one ovary, strict boundary exclusion retained 56.1–80.7% of cells but 11.0–20.5% of bins; erosion retained 37.9–72.0% of cells, while dilation retained 181.9–196.2% of default bins.
- A 3,000-polygon audit found no filtered-bin intersection for 50.6% lung, 2.5% brain, and 50.0% pancreas polygons.
- We will expose coverage/settings and keep native Xenium separate from derived HD evidence.

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
