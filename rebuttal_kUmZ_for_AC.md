# Response to the Area Chair regarding Reviewer kUmZ

We thank Reviewer kUmZ for focusing on the manuscript’s central issue: whether the resource and benchmark are genuinely cell resolved on the molecular side, what cell alignment adds beyond spot analysis, and whether evidence extends beyond three examples. We now separate native from derived targets, provide full-category and cross-sample results, and narrow the claims.

**1. “Native cell-resolved” and “derived cell-aligned” are now separated.**

| Component | Count | Molecular status |
|---|---:|---|
| Public Xenium | 16,314,129 cells | Native cell-resolved |
| Public Visium HD | 7,629,697 targets | Derived cell-aligned |
| In-house DBiC-seq | 53,989 post-QC cells from 21 samples | Native cell-resolved |

Xenium uses platform cell boundaries and transcript coordinates; DBiC-seq pairs cell morphology with RNA. Only HD aggregates native 2-µm bins within segmented masks using the documented 10x workflow. We will not describe the entire resource as directly measured single-cell data or use unqualified “single-cell predictor” claims. We will use the count-independent title *sMMC: A Cell-Aligned Multimodal Resource for Spatial Transcriptomics*.

**2. Native-Xenium validation now covers all 25 release categories.**

We reran the benchmark on 30 native-Xenium samples using training-selected top-50 HVGs, four contiguous spatial holdouts, and a 5% train–test buffer. Image macro Gene Pearson, Gene Spearman, Cell Pearson, and F1 are **0.324/0.291/0.442/0.413**; image prediction is strongest in **21/25 category rows**. These results establish image-associated signal under leakage-controlled spatial holdouts, not patient generalization.

We separately report eight Visium HD source→target organ pairs. UNI2-h macro Gene Pearson is only **0.0151**, Cell Pearson **0.2036**, and F1 **0.0815**; the training mean reaches Cell Pearson **0.2422** and F1 **0.0375**. We retain this failure and will call it sample-held-out unless donor independence is verified. Positive in-domain breadth will therefore not obscure the unresolved sample/platform/composition shift.

**3. The task and STBoost interface will be defined in the main body.**

The benchmark predicts a cell-aligned RNA profile from histology alone; no spot-level expression is supplied at inference. Each profile is paired with a local cell crop and a larger tissue-context crop. STBoost replaces a spot crop with these hierarchical inputs while retaining a method’s prediction modules. “STBoosted BLEEP” means published BLEEP retrained through this interface; “STBoost-Ref” is our image-only reference predictor. We will define both at first mention and remove the ambiguous label “Ours.”

For HD, intersecting registered bins are aggregated, conflicts go to the nearest centroid, and unsupported polygons are excluded. In a new **9,000-polygon** audit, ±1-µm shifts gave bin Jaccard **0.706–0.816** and expression cosine **0.936–0.994**; erosion/dilation gave Jaccard **0.462–0.720**, cosine **0.871–0.993**, and median absolute UMI change **32.8–66.6%**. These findings support derived targets with material boundary uncertainty, not molecular ground truth.

**4. A controlled pseudo-spot experiment shows what cell alignment adds.**

Across six native-Xenium samples, matched cell/8/16/55-µm targets give Gene Pearson **0.365/0.365/0.363/0.330**. We do not claim universal cell-level accuracy superiority. Instead, coarse aggregation hides heterogeneity: in dense lung, **55.5–66.4%** of 55-µm pseudo-spots mix cell types and involve **73.8–81.0%** of cells, versus **3.5–4.1%** and **7.0–8.3%** in a sparse sample. We will replace the vague limitation criticized by the reviewer with this measured density-dependent result.

**5. Context and presentation claims will be constrained.**

Context is an orthogonal robustness axis, not proof of cell-level validity. The ovarian result will be called a *sample/age-confounded context shift*, not causal. Donor/animal IDs are incomplete; we will report missingness, never infer ethnicity or other sensitive attributes, and require verified donor support for subgroup claims.

Figure 1 was manually created in two Sketch versions with 162 editable vector layers; evidence is available in the [anonymous repository](https://anonymous.4open.science/r/sMMC-22M-DB75). We will nevertheless correct its wording/blanks, expand the caption, define split/cell-unit/ratio terminology, move task and alignment details into the main body, and state exactly which components are public.

The revised claim is therefore narrower and better supported: the resource combines native cell-resolved and explicitly labeled derived cell-aligned data, enables evaluation at cellular coordinates, and reveals heterogeneity hidden by averaging, while cross-sample and cross-platform molecular prediction remain open challenges.
