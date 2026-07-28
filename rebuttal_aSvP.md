We sincerely thank the reviewer for asking what research becomes possible because sMMC is larger, cell aligned, and context rich. This helped us organize the revision around scientific utility. We will follow the suggestions point by point and hope the reviewer will reconsider. The working release contains 28.3M aligned targets, including 53,989 new in-house DBiC-seq cells with paired morphology and RNA.

**Q1.** *How can the new data promote research progress?*

**A1 — A unified cell-level benchmark.**

- **Task:** local-cell and tissue-context histology crops predict aligned-cell RNA; no expression is supplied at inference.
- **Protocols:** spatial holdout (5% buffer), complete-sample transfer, verified patient/donor holdout, and panel-matched cross-platform transfer.
- **Metrics:** gene/cell correlations, F1, marker/HVG performance, and Average/Worst/Gap. STBoost lifts spot predictors through hierarchical cell/context inputs.

**A2 — Results across all 25 release categories.** Under one fixed spatial-holdout protocol, the image model gives macro Gene Pearson/Cell Pearson/F1 of **0.324/0.442/0.413**:

| Release category | Gene P | Cell P | F1 |
|---|---:|---:|---:|
| Bone (mouse) | 0.030 | 0.180 | 0.240 |
| Brain (human + mouse) | 0.399 | 0.570 | 0.752 |
| Breast (human) | 0.400 | 0.433 | 0.487 |
| Cervical (human) | 0.178 | 0.247 | 0.026 |
| Colon (mouse) | 0.615 | 0.739 | 0.744 |
| Colorectal (human) | 0.431 | 0.532 | 0.623 |
| Embryo (mouse) | 0.278 | 0.517 | 0.342 |
| Head (zebrafish) | 0.216 | 0.429 | 0.287 |
| Heart (human) | 0.292 | 0.688 | 0.304 |
| Kidney (human) | 0.402 | 0.471 | 0.460 |
| Liver (human) | -0.002 | 0.561 | 0.400 |
| Lung (human) | 0.359 | 0.402 | 0.457 |
| Lymph Node (human) | 0.141 | 0.164 | 0.023 |
| Ovarian (human) | 0.250 | 0.312 | 0.234 |
| Ovarian glands (human) | 0.402 | 0.559 | 0.756 |
| Pancreas (human) | 0.324 | 0.505 | 0.275 |
| Pancreatic (human) | 0.366 | 0.540 | 0.694 |
| Pancreatic duct gland (human) | 0.331 | 0.408 | 0.386 |
| Plant (*A. thaliana*) | -0.011 | 0.385 | 0.052 |
| Prostate (human) | 0.263 | 0.263 | 0.083 |
| Seed (soybean) | -0.020 | 0.314 | 0.037 |
| Skin (human) | 0.359 | 0.437 | 0.368 |
| Small Intestine (mouse) | 0.572 | 0.701 | 0.715 |
| Tonsil (human) | 0.294 | 0.462 | 0.399 |
| Xenograft (human + mouse) | 0.387 | 0.526 | 0.589 |
| **30-sample macro** | **0.324** | **0.442** | **0.413** |

Image Gene Pearson exceeds coordinate/KNN controls in 21/25 rows; on marker/HVG overlap it is 0.201 versus 0.031/0.028. We will release fixed splits, configurations, and code.

**A3 — Transfer and context.**

- Across 18 same-organ held-out targets, top-50-HVG Gene Pearson averages **0.170** (range **0.016–0.372**); these are sample-held-out unless donors are verified.
- Across eight HD source→target organ pairs, UNI2-h macro Gene Pearson, Cell Pearson, and F1 are **0.0151**, **0.2036**, and **0.0815**. This difficult result is important: sample, platform, panel, and composition shifts remain open problems.
- We will reserve *patient-held-out* for verified donors and report cross-platform results with matched panels and platform-separated summaries.
- Context audits using patient-CV or leave-one-site-out show Average–Worst balanced-accuracy gaps of **0.258–0.975** across single-cell and pathology encoders. The ovarian AK/AD/AL shift changes F1 from **0.289 to 0.072**, but will be labeled sample/age-confounded rather than causal.

**Q2.** *How does heterogeneity affect downstream analysis, and what does cell alignment add?*

**A1 — Measured consequence of resolution.** Across six native-Xenium samples, cell/8/16/55-µm targets give Gene Pearson **0.365/0.365/0.363/0.330**. Yet 55-µm pseudo-spots mix cell types in **55.5–66.4%** of dense-tissue regions, affecting **73.8–81.0%** of cells, versus **3.5–4.1%/7.0–8.3%** in sparse tissue.

Moving spatial transcriptomics from spots toward cells therefore changes what can be studied, not merely the benchmark resolution. It preserves cell identity and neighborhood heterogeneity for cell-type/state analysis and cell–cell communication; links morphology, RNA, and response at the cell unit for virtual-cell and perturbation models; and enables tracing how perturbations propagate through interacting cells across spatial tissue. These questions are poorly posed when heterogeneous cells are collapsed into one spot.

**A2 — Revisions enabled by the reviewer’s suggestions.** We will:

- place the benchmark task, STBoost definition, split construction, and 25-category results in the main text;
- separate spatial, sample-held-out, verified patient-held-out, and cross-platform claims;
- report organ-macro and platform-separated Average/Worst/Gap with support, panel overlap, and metadata missingness;
- label each field as measured, processed, generated, or model output and avoid causal or representativeness claims unsupported by donors.

We again thank the reviewer: these suggestions make the paper clearer about both the opportunities enabled by a unified cell-aligned resource and the substantial heterogeneity that future methods must solve.
