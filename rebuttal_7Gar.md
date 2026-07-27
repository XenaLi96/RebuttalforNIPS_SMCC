We thank the reviewer for identifying places where the manuscript did not sufficiently distinguish primary measurements, derived cell-aligned targets, and generated auxiliary fields, or explain the objective of each experiment. We provide the requested quantitative audit, expanded results, and validation below, and will revise the claims accordingly.

**Q1.** *Precisely quantify new in-house data versus HEST-1K and STimage-1K4M by samples, tissues, platforms, cells, and new-versus-aggregated fraction.*

**A1.** We agree. We audited every sample against HEST-1K v1.1.0 and the 2025-02-12 STimage-1K4M snapshot.

| Category | Slides/samples | Organs or cell-line origins | Platforms | Aligned target cells after QC |
|---|---:|---|---|---:|
| **New in-house primary data** | **21** | **mouse-embryo; cervical; lung** | **DBiC-seq** | **53,989** |
| Public, overlapping HEST-1K | 32 unique slides (33 records) | 22 species--organ strata | Xenium; Visium HD | 11,051,149 |
| Public, overlapping STimage-1K4M only | 2 | 2 species--organ strata | Visium HD | 213,405 |
| Other public data | 44 | 16 species--organ strata | Xenium; Visium HD | 16,996,704 |
| **Total sMMC-28M** | **99 unique samples (100 records)** | **25 tissue strata + 3 cell-line origins** | **Xenium; Visium HD; DBiC-seq** | **28,315,247** |

- **New in-house:** collaborators provided DBiC-seq native cells paired one-to-one with transcriptomic profiles, available for [download and preview](https://anonymous.4open.science/r/sMMC-22M-DB75/README.md).
- **Limited overlap:** only 32 samples overlap HEST-1K, all from its new Visium HD cohort; about 70% are absent from HEST-1K.

**Q2.** *Clear purpose and biological meaning of the per-axis experiments.*

**A1 — Scale.** We agree and will reorganize the three axes.

- **Objective:** not a benchmark, but a test of whether more training data improve image-to-expression models.
- **Design/metric:** Fig. 2 varies training fraction while fixing targets, preprocessing, head, and spatial tests; the metric is held-out gene-macro Pearson. We further tested seven frozen encoders on training-selected top-50 HVGs, four buffered holdouts, and 5%/10%/25%/100% fractions.

| Encoder | 5% | 10% | 25% | 100% |
|---|---:|---:|---:|---:|
| ResNet-50 | 0.140 | 0.160 | 0.190 | 0.221 |
| CTransPath | 0.162 | 0.186 | 0.222 | 0.259 |
| Phikon | 0.168 | 0.200 | 0.237 | 0.273 |
| CONCH | 0.178 | 0.213 | 0.256 | 0.292 |
| UNI | 0.188 | 0.218 | 0.259 | 0.295 |
| UNI2 | 0.197 | 0.225 | 0.257 | 0.295 |
| H-Optimus-0 | 0.205 | 0.233 | 0.269 | 0.307 |

- **Result:** every encoder improves with scale; mean Gene Pearson rises 0.177→0.205→0.241→0.278.

**A2 — Resolution (our central benchmark).**

- **Task:** predict aligned-cell RNA expression from histology alone, supporting cell-type/state and other cell-resolved analyses.
- **Protocols:** in-sample, cross-sample/patient, and cross-platform prediction test increasingly difficult shifts.
- **Definitions:** STBoost adapts spot-level methods to cells; every reported BLEEP result uses this adaptation. STBoost-Ref further adds diffusion loss and Appendix components. Its strong but imperfect results provide a reference for future methods on this dataset.

**Per-organ benchmark results.** Results use the top-50 training-selected HVGs, four contiguous spatial holdouts, and a 5% train–test buffer.

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

**A3 — Rich context.**

- **Purpose:** our rich-context metadata—including age, sex/gender where reported, disease, and underlying clinical conditions—enable us to test whether a model performs consistently across biologically and clinically meaningful subgroups. Average performance alone can hide severe subgroup failures, so such context-aware evaluation is an important dimension for assessing foundation models and other predictive methods.
- **Main-text example:** the ovarian experiment is one illustrative case. AK (60+)→AD (60+) versus AK (60+)→AL (<40) changes F1 from 0.289 to 0.072, gene Spearman from 0.036 to 0.006, cell Spearman from 0.206 to 0.095, and nonzero rate from 0.166 to 0.027, revealing a large age-associated performance disparity.
- **Broader evidence:** we conducted analogous audits for other foundation models and contexts. The table below shows substantial assay-, dataset-, and site-associated gaps for Geneformer, scGPT, CONCH, UNI, and H-Optimus-0:

| Domain | Task / encoder | Context; split | Avg. BA | Worst BA | Gap |
|---|---|---|---:|---:|---:|
| Single-cell | Bone marrow / Geneformer | assay; patient-CV | 0.938 | 0.669 | 0.327 |
| Single-cell | Bone marrow / scGPT | assay; patient-CV | 0.962 | 0.740 | 0.258 |
| Single-cell | Bone marrow / scVI-style | assay; patient-CV | 0.932 | 0.616 | 0.373 |
| Single-cell | Ten tissues / scGPT | dataset; patient-CV | 0.667--0.962 | 0.004--0.892 | max 0.975 |
| Pathology | LUAD KRAS / CONCH | site; leave-one-site | 0.499 | 0.375 | 0.542 |
| Pathology | LGG IDH / UNI | site; leave-one-site | 0.682 | 0.464 | 0.471 |
| Pathology | LGG IDH / H-optimus0 | site; leave-one-site | 0.747 | 0.476 | 0.524 |

- **Significance and boundary:** BA is balanced accuracy, and Gap is best-to-worst performance. The purpose of this axis is to make context-dependent performance bias measurable, not to claim that the resource itself removes bias. Because ovarian age is nested within sample/patient and the panels differ, this example supports an age-associated, sample-confounded shift rather than a causal age effect.

**Q3.** *How are bin2cell targets constructed and validated?*

**A1 — Construction.**

- Appendix S2 shows the official Visium HD registration of native $2\,\mu$m bins to H&E; intersecting same-frame cell polygons aggregate counts, unsupported cells are removed, and conflicts are resolved deterministically.
- We have moved this pipeline and an example to the main text and now label the outputs *derived cell-aligned targets*, not measured single-cell RNA.

**A2 — Validation.** We audited 3,000 raw CellViT polygons per dataset (9,000 total) under 1-$\mu$m registration shifts. Canonical-bin coverage is reported across all raw polygons; among supported polygons, expression direction remains stable (cosine 0.936--0.994), with Jaccard 0.706--0.816 and median $|\Delta\mathrm{UMI}|$ 6.0%--13.7%, indicating reasonable but not perfect robustness:

| Visium HD example | Raw polygons with canonical bins | Shift bin Jaccard | Shift expression cosine | Shift median $|\Delta\mathrm{UMI}|$ |
|---|---:|---:|---:|---:|
| Human lung cancer | 49.4% | 0.727--0.733 | 0.954--0.957 | 11.1%--11.3% |
| Mouse brain | 97.5% | 0.806--0.816 | 0.936--0.939 | 6.0%--6.1% |
| Human pancreas | 50.0% | 0.706--0.714 | 0.994 | 13.0%--13.7% |

**Q4.** *How are LMM-generated texts produced and validated?*

**A1 — Generation.** GPT-4o verbalizes supplied fields without adding attributes. We regret omitting the completed audit while shortening this section and will restore prompts, field checks, examples, and matrices:

| Model | Match $n$ | Match sim. | Mismatch sim. | Enrichment | Pairwise AUC | Top-1 / Top-3 |
|---|---:|---:|---:|---:|---:|---:|
| PLIP | 15 | 0.262 | 0.053 | 4.96× | 0.993 | 84.6% / 100% |
| CONCH | 29 | 0.040 | 0.0021 | 19.1× | 0.988 | 90.9% / 100% |

**A2 — Validation and scope.**

- Both VLMs reach AUC $\geq0.988$ and 100% Top-3 retrieval.
- These metrics assess image relevance, while source-field checks assess factuality.
- Text remains optional context, not ground truth.
