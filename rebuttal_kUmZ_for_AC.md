# Response to the Area Chair regarding Reviewer kUmZ

We thank Reviewer kUmZ for focusing on the manuscript’s central issue: whether the resource and benchmark are genuinely cell resolved on the molecular side, what cell alignment adds beyond spot analysis, and whether evidence extends beyond three examples. We now separate native from derived targets, provide full-category and cross-sample results, and narrow the claims.

**1. “Native cell-resolved” and “derived cell-aligned” are now separated.**

| Component | Count | Molecular status |
|---|---:|---|
| Public Xenium | 16,314,129 cells | Native cell-resolved |
| Public Visium HD | 7,629,697 targets | Derived cell-aligned |
| In-house DBiC-seq | 53,989 post-QC cells from 21 samples | Native cell-resolved |

Xenium uses platform cell boundaries and transcript coordinates; DBiC-seq pairs cell morphology with RNA. Only HD uses official transforms to register native 2-µm bins to H&E-aligned CellViT contours, aggregate intersecting bins, assign conflicts to the nearest centroid, and exclude unsupported polygons. These are derived cell-aligned targets, not native single-cell measurements. In a **9,000-polygon** audit, ±1-µm shifts gave Jaccard **0.706–0.816**, expression cosine **0.936–0.994**, and median absolute UMI change **6.0–13.7%**. We will move this construction rule and QC into the main body, avoid unqualified “single-cell predictor” claims, and use the count-independent title *sMMC: A Cell-Aligned Multimodal Resource for Spatial Transcriptomics*.

**2. Benchmark results now cover all 25 release categories.**

Under one fixed protocol using training-selected top-50 HVGs, four contiguous spatial holdouts, and a 5% train–test buffer, image macro Gene Pearson, Gene Spearman, Cell Pearson, and F1 are **0.324/0.291/0.442/0.413**; image prediction is strongest in **21/25 category rows**. These results establish image-associated signal under leakage-controlled spatial holdouts, not patient generalization.

We separately report eight source→target organ pairs. Cross-sample prediction remains difficult and the results are not yet satisfactory: UNI2-h macro Gene Pearson/Cell Pearson/F1 are **0.0151/0.2036/0.0815**, while the mean reaches Cell Pearson/F1 **0.2422/0.0375**. We report these results transparently beside the spatial-holdout benchmark.

**3. The task and STBoost interface will be defined in the main body.**

The benchmark predicts a cell-aligned RNA profile from histology alone; no molecular input is supplied at inference. Each profile is paired with local-cell and tissue-context crops. STBoost lifts spot-level methods to cell-level prediction through these hierarchical inputs while retaining their prediction modules. “STBoosted BLEEP” is published BLEEP retrained through this interface; “STBoost-Ref” is our image-only reference. We will move a concise formulation/architecture from the Appendix into the main body.

**4. A controlled pseudo-spot experiment shows what cell alignment adds.**

Across six native-Xenium samples, fixed-pipeline cell/8/16/55-µm targets give Gene Pearson **0.365/0.365/0.363/0.330**. In two representative samples, 55-µm pseudo-spots mix cell types in **55.5–66.4%** of regions and affect **73.8–81.0%** of cells in dense tissue, versus **3.5–4.1%** and **7.0–8.3%** in sparse tissue. Cell alignment reveals heterogeneity hidden by averaging and supports cell–cell communication, virtual-cell perturbation prediction, and perturbation analysis across spatial tissue.

**5. Context representativeness and bias are treated separately.**

We agree that existing datasets cannot be assumed representative of ethnicity, sex/gender, or other human traits. We will report missingness, never infer undocumented sensitive attributes, and avoid unsupported population-level claims. Nevertheless, context-bias evaluation remains important: patient-CV/leave-one-site-out audits of Geneformer, scGPT, CONCH, UNI, and H-Optimus-0 show substantial Average–Worst gaps (**0.258–0.975**). The ovarian result will be called a *sample/age-confounded context shift*, not causal.

Figure 1 was not AI-generated. I began this resource in my first PhD year; over the following three years, the figure went through two manually drawn Sketch versions and now contains 162 editable vector layers. Evidence is in the [anonymous repository](https://anonymous.4open.science/r/sMMC-22M-DB75) under `figure_evidence_not_AI/`. We will also correct its text, expand the caption, define split/cell-unit/ratio terminology, and state exactly which components are public.

The revision therefore distinguishes native from derived evidence, makes the benchmark and alignment auditable, and clarifies the scientific value of cell-resolved evaluation.
