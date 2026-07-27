We thank the reviewer for identifying where our framing ran ahead of the evidence. We will reorganize each experiment around its question, unit, input, target, metric, and conclusion, and narrow claims for derived targets. This resource has accompanied me since my first PhD year; near graduation, I hope its unified release helps new students enter spatial transcriptomics.

**Q1.** *In-house data scale. Please quantify precisely how much new in-house data is contributed relative to HEST-1K and STimage-1K4M (samples, tissues, platforms, spots/cells), ideally as a comparison table. What fraction of the resource is genuinely new vs. aggregated?*

**A1.** We agree and will distinguish primary acquisition from aggregation. We audited each sample against HEST-1K v1.1.0 and the 2025-02-12 STimage-1K4M snapshot, using mutually exclusive categories; the rebuttal-time manifest also adds 12 public Visium HD samples.

| Category | Slides/samples | Organs or cell-line origins | Platforms | Aligned target cells after QC |
|---|---:|---|---|---:|
| **New in-house primary data** | **21** | **mouse-embryo; cervical; lung** | **DBiC-seq** | **53,989** |
| Public, overlapping HEST-1K | 32 unique slides (33 records) | 22 species--organ strata | Xenium; Visium HD | 11,051,149 |
| Public, overlapping STimage-1K4M only | 2 | 2 species--organ strata | Visium HD | 213,405 |
| Other public data | 44 | 16 species--organ strata | Xenium; Visium HD | 16,996,704 |
| **Total sMMC-28M** | **99 unique samples (100 records)** | **25 tissue strata + 3 cell-line origins** | **Xenium; Visium HD; DBiC-seq** | **28,315,247** |

**A2.** The audit establishes three boundaries:

- **New versus public:** the in-house samples contain 54,304 measured cells; QC removes 315, leaving 53,989: **0.1907% new primary versus 99.8093% public aggregated/harmonized targets**.
- **Current versus continuing acquisition:** the broader DBiC-seq pool contains approximately 200,000 paired cells but is not counted here; one versioned breast slide explains 99 unique samples versus 100 records.
- **Measurement status:** Visium HD entries are derived $2\,\mu\mathrm{m}$ bin-to-cell targets; Xenium and DBiC-seq are native cell-aligned records.

**Q2.** *Clear purpose and biological meaning of the per-axis experiments. For each experiment, please lay out the benchmark objective, design, results, and significance cleanly. In particular: What task and metric does Fig. 2 use? In the resolution study, what exactly are STBoost, STBoost-Ref, and “Ours”; what is BLEEP; and what is “STBoosted BLEEP”? For the age-effect analysis, what is the precise claim, and why is it informative beyond the expected degradation under an age shift?*

**A1 — Scale.** We agree that the three axes lacked clear objectives and conclusions.

- **Objective:** test whether more training cells improve fixed image-to-expression predictors.
- **Design/metric:** Fig. 2 varies the training fraction while fixing targets, preprocessing, and spatial test regions; the metric is gene-macro Pearson across held-out cells. On native-Xenium HHDX011, we tested seven frozen encoders, training-selected top-50 HVGs, four buffered edge holdouts (2,400 test cells each), and 5%/10%/25%/100% training fractions (445--447/890--895/2,226--2,237/8,912--8,948 cells).

| Encoder | 5% | 10% | 25% | 100% |
|---|---:|---:|---:|---:|
| ResNet-50 | 0.140 | 0.160 | 0.190 | 0.221 |
| CTransPath | 0.162 | 0.186 | 0.222 | 0.259 |
| Phikon | 0.168 | 0.200 | 0.237 | 0.273 |
| CONCH | 0.178 | 0.213 | 0.256 | 0.292 |
| UNI | 0.188 | 0.218 | 0.259 | 0.295 |
| UNI2 | 0.197 | 0.225 | 0.257 | 0.295 |
| H-Optimus-0 | 0.205 | 0.233 | 0.269 | 0.307 |

- **Result/significance:** all encoders improve monotonically; their mean rises 0.177→0.205→0.241→0.278. This supports data utility in this controlled range, not a universal scaling law. For breadth, we tested CONCH--Ridge on 30 native-Xenium samples (17 organ/tissue labels, 25 reported conditions, two species; up to 12,000 spatially sampled cells each):

| Model | Gene Pearson | Gene Spearman | Cell Pearson | Gene F1 |
|---|---:|---:|---:|---:|
| Image | 0.324 | 0.291 | 0.442 | 0.413 |
| Coordinate-only | 0.046 | 0.041 | 0.288 | 0.283 |
| Spatial KNN | 0.053 | 0.051 | 0.268 | 0.320 |
| Training mean | 0.000 | -- | 0.305 | 0.253 |

- **Claim boundary:** image gene Pearson is 0.324 (sample bootstrap 95% CI 0.277--0.368) and exceeds all controls (paired Holm $p\leq1.9\times10^{-8}$). These are sample-specific top-50-HVG, not full-panel or donor-held-out, results.

**A2 — Resolution.**

- **Objective:** test cell-aligned prediction and what aggregation hides.
- **Definitions:** STBoost is our model-agnostic cell-aligned interface; BLEEP is the published spot baseline; STBoosted BLEEP retrains BLEEP through this interface; STBoost-Ref is our retrieval predictor and will replace “Ours” in the figure. It uses only histology at inference.
- **Result:** on 5,000 cells and 18,085 genes, STBoost-Ref improves most $A\!\to\!A$ metrics and all $A\!\to\!B$ metrics; cross-patient F1 rises 0.0857→0.1061 and cell Pearson 0.0896→0.2820. We also constructed matched pseudo-spots from six native-Xenium samples under identical splits/modeling:

| Evaluation unit | Cell | 8 $\mu$m | 16 $\mu$m | 55 $\mu$m |
|---|---:|---:|---:|---:|
| Gene Pearson | 0.365 | 0.365 | 0.363 | 0.330 |

- **Significance:** cell and 8/16-$\mu$m scores are similar, but 55-$\mu$m aggregation drops performance and mixes predicted types in dense lung (55.5%--66.4% of spots; 73.8%--81.0% of cells), versus sparse heart (3.5%--4.1%; 7.0%--8.3%). Thus, the evidence is density-dependent heterogeneity concealment, not universally easier cell prediction. Biological calibration gives:

| Biological calibration | Image | Coordinate | Spatial KNN | Mean |
|---|---:|---:|---:|---:|
| Marker/HVG gene Pearson $\uparrow$ | 0.201 | 0.031 | 0.028 | 0.000 |
| Cell-type-stratified pseudobulk RMSE $\downarrow$ | 0.120 | 0.224 | 0.187 | 0.188 |

- **Claim boundary:** markers overlap training-selected HVGs; pseudobulk uses molecularly predicted cell-type annotations. We therefore claim recoverable aggregate structure, not exact cell-wise recovery or independent ground truth.

**A3 — Rich context.**

- **Objective:** expose subgroup collapse hidden by pooled scores.
- **Result:** in ovarian cancer, AK (60+)→AD (60+) versus AK (60+)→AL (40-) changes F1 0.289→0.072, gene Spearman 0.036→0.006, cell Spearman 0.206→0.095, and nonzero rate 0.166→0.027. Across 18 same-organ native-Xenium target samples, top-50-HVG Pearson averages 0.170 (range 0.016--0.372); incomplete donor IDs make this sample-, not patient-held-out. Average/Worst/Gap audits on other context-rich cohorts show the same issue:

| Domain | Task / encoder | Context; split | Avg. BA | Worst BA | Gap |
|---|---|---|---:|---:|---:|
| Single-cell | Bone marrow / Geneformer | assay; patient-CV | 0.938 | 0.669 | 0.327 |
| Single-cell | Bone marrow / scGPT | assay; patient-CV | 0.962 | 0.740 | 0.258 |
| Single-cell | Bone marrow / scVI-style | assay; patient-CV | 0.932 | 0.616 | 0.373 |
| Single-cell | Ten tissues / scGPT | dataset; patient-CV | 0.667--0.962 | 0.004--0.892 | max 0.975 |
| Pathology | LUAD KRAS / CONCH | site; leave-one-site | 0.499 | 0.375 | 0.542 |
| Pathology | LGG IDH / UNI | site; leave-one-site | 0.682 | 0.464 | 0.471 |
| Pathology | LGG IDH / H-optimus0 | site; leave-one-site | 0.747 | 0.476 | 0.524 |

- **Significance and boundary:** BA is balanced accuracy; Gap is best-to-worst. High averages coexist with Worst BA 0.004 and site gaps 0.471--0.542. Rich context enables such audits, not bias removal. Ovarian age is nested within sample/patient and panels differ; we therefore claim an age-associated, sample-confounded shift, not causality.

**Q3.** *How are bin2cell targets constructed and validated?*

**A1 — Construction.**

- Appendix S2 shows the official Visium HD registration of native $2\,\mu$m bins to H&E.
- Intersecting same-frame cell polygons aggregate counts; unsupported cells are removed and conflicts resolved deterministically.
- We will move this pipeline and an example to the main text and label outputs *derived cell-aligned targets*, not measured single-cell RNA.

**A2 — Validation.** We audited 3,000 raw CellViT polygons per dataset (9,000 total) under 1-$\mu$m registration shifts and erosion/dilation:

| Visium HD example | Raw polygons with canonical bins | Shift bin Jaccard | Shift expression cosine | Shift median $|\Delta\mathrm{UMI}|$ |
|---|---:|---:|---:|---:|
| Human lung cancer | 49.4% | 0.727--0.733 | 0.954--0.957 | 11.1%--11.3% |
| Mouse brain | 97.5% | 0.806--0.816 | 0.936--0.939 | 6.0%--6.1% |
| Human pancreas | 50.0% | 0.706--0.714 | 0.994 | 13.0%--13.7% |

- Canonical-bin percentages use all raw polygons; unsupported polygons are excluded before release.
- Shifts preserve expression direction but alter membership/UMIs; erosion/dilation yields Jaccard 0.462--0.720, cosine 0.871--0.993, and $|\Delta\mathrm{UMI}|$ 32.8%--66.6%.
- We will release parameters and per-sample QC, avoid calling aggregation ground truth, and report native-Xenium evidence separately.

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
