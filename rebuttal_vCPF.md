We thank the reviewer for requesting an HD-construction audit, broader evaluation, and biological calibration. We provide each below while separating native from derived targets and retaining negative transfer results.

**Q1.** *How reliable is Visium HD bin-to-cell aggregation?*

**A1 — Construction.**

- Official transforms register native $2\,\mu$m bins to H&E; intersecting CellViT polygons aggregate counts, conflicts go to the nearest centroid, and unsupported cells are removed. We will move this rule to the main text and label HD outputs *derived cell-aligned*, separately from native Xenium.

**A2 — Sensitivity audit.** We audited 3,000 raw polygons per dataset (9,000 total) under 1-$\mu$m registration shifts and mask erosion/dilation:

| Visium HD sample | Raw polygons with canonical bins | Shift-bin Jaccard | Shift-expression cosine | Shift median $|\Delta\mathrm{UMI}|$ |
|---|---:|---:|---:|---:|
| Human lung cancer | 49.4% | 0.727--0.733 | 0.954--0.957 | 11.1%--11.3% |
| Mouse brain | 97.5% | 0.806--0.816 | 0.936--0.939 | 6.0%--6.1% |
| Human pancreas | 50.0% | 0.706--0.714 | 0.994 | 13.0%--13.7% |

Across all raw polygons, coverage is sample dependent. Among supported polygons, shifts largely preserve expression direction but alter membership/UMIs; erosion/dilation gives Jaccard 0.462--0.720, cosine 0.871--0.993, and $|\Delta\mathrm{UMI}|$ 32.8%--66.6%. We will release settings/QC and avoid ground-truth language.

**Q2.** *Can the authors summarize more organs/platforms?*

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

- Image is strongest in 21/25 rows. Across 18 same-organ held-out targets, top-50-HVG Gene Pearson averages 0.170 (0.016--0.372), Cell Pearson is 0.275 versus 0.205 for the mean, and F1 is 0.125 versus 0.031. These are sample-, not donor-held-out.

**A3 — Visium HD transfer.** Q3 reports eight complete source→target pairs before and after the requested calibration controls.

**Q3.** *How should low transfer correlations be interpreted and calibrated?*

**A1 — Reviewer-inspired paired comparison.** We thank the reviewer for suggesting metadata/spatial controls. On the identical source→target cells and genes, we compare image-only UNI2-h directly with a segmentation-metadata baseline. Metadata include CellViT type/confidence, polygon/bbox morphology, status, and edge flag; `total_counts` and `n_genes_detected` are excluded.

**Table 2. Effect of the reviewer-suggested segmentation-metadata calibration on the same eight Visium HD transfer pairs.** $\Delta$ Gene P is Metadata minus UNI2-h; bold marks the better result for each metric.

| Organ | UNI2-h Gene P | Metadata Gene P | $\Delta$ Gene P | UNI2-h Cell P | Metadata Cell P | UNI2-h F1 | Metadata F1 |
|---|---:|---:|---:|---:|---:|---:|---:|
| Human breast | 0.0272 | **0.0847** | **+0.0575** | **0.1109** | 0.0785 | **0.1794** | 0.1479 |
| Human ovary | 0.0267 | **0.0643** | **+0.0376** | 0.4593 | **0.4645** | **0.1161** | 0.0615 |
| Human lung | 0.0143 | **0.0319** | **+0.0176** | **0.2121** | 0.1700 | **0.0960** | 0.0267 |
| Human pancreas | 0.0040 | **0.0212** | **+0.0172** | **0.0090** | 0.0043 | **0.0389** | 0.0179 |
| Human tonsil | 0.0141 | **0.0282** | **+0.0141** | 0.2769 | **0.3696** | **0.0708** | 0.0368 |
| Mouse brain | 0.0243 | **0.0502** | **+0.0259** | 0.2016 | **0.2314** | **0.0623** | 0.0223 |
| Mouse embryo | 0.0048 | **0.0176** | **+0.0128** | 0.0913 | **0.1662** | **0.0369** | 0.0146 |
| Mouse kidney | 0.0049 | **0.0213** | **+0.0164** | 0.2678 | **0.3910** | **0.0513** | 0.0175 |
| **Organ macro** | 0.0151 | **0.0399** | **+0.0248** | 0.2036 | **0.2344** | **0.0815** | 0.0432 |

- The reviewer-suggested baseline improves Gene Pearson in **8/8 organs**, from 0.0151 to 0.0399 macro (**+0.0248; 2.64×**), and improves Cell Pearson in 5/8 organs, from 0.2036 to 0.2344 macro (**+0.0308**).
- The gain is metric-specific: F1 decreases from 0.0815 to 0.0432. We will therefore adopt segmentation metadata as the principal non-image *correlation-calibration* baseline, not claim universal superiority. Coordinate-only and metadata+coordinate Gene Pearson are −0.0023/0.0316; age/sex/disease are not identifiable from one source/target pair per organ.

**A2 — Biological calibration.** Complementary 30-sample spatial holdouts give:

| Metric | Image | Coordinate | Spatial KNN | Mean |
|---|---:|---:|---:|---:|
| Marker/HVG Gene Pearson $\uparrow$ | 0.201 | 0.031 | 0.028 | N/A |
| Cell-type-stratified pseudobulk RMSE $\downarrow$ | 0.120 | 0.224 | 0.187 | 0.188 |

**A3 — Boundary.** Marker overlap and expression-derived strata provide aggregate biological calibration, not independent labels. The original low UNI2-h transfer column remains visible; the reviewer-inspired baseline improves correlation calibration but does not solve patient/platform generalization.

**Q4.** *Could in-domain results reflect spatial interpolation?*

**A.** We thank the reviewer for raising this important concern. Random cell splits can be inflated by spatial autocorrelation; ours is spatially separated.

- The original in-domain split trains on the left two-thirds and tests on the right one-third of a section; a 5% buffer removes boundary cells. Train/test cells are therefore neither interleaved nor directly neighboring. The 30-sample benchmark uses four analogous contiguous holdouts.
- Under identical splits, image macro Gene Pearson is 0.324 versus 0.046 for coordinate-only regression and 0.053 for spatial KNN, and image is strongest in 28/30 samples. Local interpolation alone therefore cannot explain the performance.
- Because both partitions remain from the same sample, we will call this *within-sample spatially held-out prediction*, not cross-sample/patient generalization. Complete-sample transfer is reported separately and remains substantially harder.

**Q5.** *Is the ovarian context analysis too narrow?*

**A1 — Scope of the ovarian result.**

- AK (60+)→AD (60+) versus AK→AL (40-) changes F1 0.289→0.072 and gene/cell Spearman 0.036/0.206→0.006/0.095. Because age is nested within sample/patient and panels differ, this is an *age-associated, sample-confounded shift*, not causal.

**A2 — Broader audit.**

| Task / encoder | Context; split | Average BA | Worst BA | Gap |
|---|---|---:|---:|---:|
| Bone marrow / scGPT | assay; patient-CV | 0.962 | 0.740 | 0.258 |
| Ten tissues / scGPT | dataset; patient-CV | 0.667--0.962 | 0.004--0.892 | max 0.975 |
| LUAD KRAS / CONCH | site; leave-one-site | 0.499 | 0.375 | 0.542 |
| LGG IDH / H-Optimus-0 | site; leave-one-site | 0.747 | 0.476 | 0.524 |

Rich metadata enable worst-context audits; they do not remove bias.

**Q6.** *Which fields are measured, processed, or generated?*

**A1 — Provenance classes.**

| Provenance | Examples |
|---|---|
| Direct | H&E; Xenium transcript coordinates/platform masks; Visium HD $2\,\mu$m bin counts; source metadata as reported. |
| Processed | Registration; CellViT footprints; Visium HD bin-to-cell matrices; crops; normalized matrices; splits; molecular-profile summaries. |
| Generated | Metadata-grounded GPT-4o captions and model predictions; neither is molecular ground truth. |

**A2 — Text scope.** GPT-4o verbalizes supplied fields; PLIP/CONCH audits give AUC 0.993/0.988 and 100% Top-3 retrieval. We will restore audit materials; captions remain optional context, not ground truth.
