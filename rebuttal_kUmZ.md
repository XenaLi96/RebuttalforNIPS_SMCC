We thank the reviewer for focusing on the central validity question: a cell-centered image does not by itself make its expression target a direct single-cell measurement. We agree and will sharpen this distinction, broaden evaluation, and add the missing construction, cohort, and limitation details.

This resource is also personally meaningful to us. The first author began collecting it during the first year of PhD training, when entering spatial transcriptomics required navigating heterogeneous data formats and learning tools such as Scanpy alongside platform-specific software for Xenium, Visium HD, and earlier technologies. Now approaching graduation, the first author has continued to expand the collection, which has supported several projects undertaken by us and our co-authors. We hope to make it broadly available soon as a unified, accessible resource, especially for new students facing the same practical barriers that originally motivated this work.

**1. The imaging side is cell centered, but spot-level or interpolated expression does not establish true single-cell prediction. Please soften the title and abstract.**

Agreed. Xenium supplies natively cell-resolved transcript locations and platform-defined masks. Visium HD supplies native $2\,\mu\mathrm{m}$ bin counts, which we register to H&E and aggregate over segmented cell footprints. We will call the latter *cell-aligned computational targets*, not direct single-cell ground truth, and use “toward cell-level prediction” or “cell-aligned prediction” where appropriate. A conservative title is *sMMC: A Cell-Aligned Multimodal Resource for Spatial Transcriptomics*.

For lung A, lung B, and ovary, strict-interior assignment retains 62.5%/56.1%/80.7% of default cells and 16.0%/11.0%/20.5% of bins. One-bin erosion retains 46.0%/37.9%/72.0% of cells, whereas dilation retains essentially all cells but increases assigned bins to 181.9%--196.2% of default. This sensitivity reinforces the cell-aligned rather than direct-single-cell claim.

**PENDING EXPERIMENT:** add matched-cell expression/marker/state agreement and fixed-checkpoint downstream sensitivity with confidence intervals for these perturbations.

**AUTHOR INPUT REQUIRED:** approve the final title and audit every use of “single-cell,” retaining it only for native measurements or appropriately qualified goals.

**2. The experiments emphasize few organs. If all 25 were evaluated, report per-organ and summary performance.**

We did not train and validate one complete benchmark on all 25 organs and will not imply otherwise. We can add this directly comparable five-organ Visium HD in-domain top-50-gene diagnostic:

| Organ | Gene $r$ | Gene $\rho$ | Cell $r$ | Cell $\rho$ |
|---|---:|---:|---:|---:|
| Ovary | 0.4523 | 0.4813 | 0.8379 | 0.7785 |
| Lung | 0.5178 | 0.5077 | 0.6042 | 0.5452 |
| Breast | 0.3366 | 0.2631 | 0.2345 | 0.2393 |
| Kidney | 0.3643 | 0.3313 | 0.5112 | 0.4834 |
| Tonsil | 0.3817 | 0.3354 | 0.8193 | 0.5670 |

The wide range argues against a cell-weighted pooled score. Separately, native-Xenium within-sample diagnostics give gene Pearson/Spearman of 0.541/0.544 (colon; 77,636 test cells), 0.512/0.585 (breast; 114,907), 0.406/0.459 (ovary; 4,000), and 0.509/0.384 (skin; 13,696), recovering TFF3/EPCAM, MUC1/GATA3, and DES/MYH11/MYLK signals. Image-model versus coordinate-only gene Pearson is 0.541/0.238, 0.512/0.116, 0.406/0.285, and 0.509/0.257; their 100-replicate spatial-block bootstrap intervals do not overlap. These results show native-target within-sample feasibility, not held-out-sample generalization.

**PENDING EXPERIMENT:** add platform-separated lightweight per-organ and native-Xenium sample-held-out summaries with organ-macro averages, baselines, support, and uncertainty.

**3. The ovarian age experiment demonstrates distribution shift but does not advance the single-cell claim.**

Agreed. We will present it only as a metadata-defined performance-bias showcase. AK, AD, and AL are anonymized ovarian sample IDs. Comparing AK (60+) $\to$ AD (60+) against AK (60+) $\to$ AL (40-), F1 changes from 0.289 to 0.072, gene Spearman from 0.036 to 0.006, cell Spearman from 0.206 to 0.095, and predicted nonzero rate from 0.166 to 0.027. This motivates Average, Worst-context, Gap, and Support reporting, but it neither validates cell-target accuracy nor establishes a causal age effect. Age is currently nested within patient/sample and tissue context, and panels differ.

**PENDING EXPERIMENT:** rerun with one shared gene panel and matched preprocessing, report cells/samples and uncertainty, and retain only the claim of an *age-associated, sample-confounded shift* unless independent-donor support permits more.

**4. The paper repeats framing but omits alignment details; STBoost is defined late, and Figure 1 contains errors.**

We will replace repetition with construction details. For Visium HD, the audited rule assigns a closed native $2\,\mu\mathrm{m}$ bin square that intersects a CellViT polygon. Multiple claims go to the nearest centroid in full-resolution coordinates; exact ties use the lower raw-cell index. We will state registration, segmentation, and QC separately by platform.

STBoost will be introduced in the abstract/introduction as a model-agnostic cell-aligned data interface. STBoost-Ref will be defined before its first result as an image-only retrieval predictor: at inference it receives local and wider-context histology crops, never query expression, and returns a cell-aligned vector.

**AUTHOR INPUT REQUIRED:** verify CellViT/registration versions, cell--bin center-distance statistics, QC thresholds, and retention by platform.

We will redraw Figure 1 and correct its labels/caption; remove the Figure 3 border and rasterized text; define “study split” in Table 1 and “cell unit” as one aligned multimodal record in Table 2; define Fig. 2’s task, metric, and training ratio; make the suggested textual cuts; and state the public release scope explicitly.

**5. Report patient/animal counts and the cross-patient train/test composition.**

Agreed: millions of cells do not imply millions of independent biological units. The canonical manifest will report studies, specimens, sections, verified patients/donors or animals, and missing identifiers by organ/platform. Every transfer experiment will list train/validation/test biological units and cell counts; repeated sections from one subject will remain in one split.

**AUTHOR INPUT REQUIRED:** complete these identifiers from source metadata. We will call a split “cross-patient” only where patient identity is verified; otherwise it will be “sample-held-out.”

**6. Audit sex, ethnicity, and other traits that may be imbalanced or missing.**

Agreed. We will report coverage and missingness using only source-reported metadata and will not infer sensitive attributes from images or names. Stratified results will be shown only where support and consent permit responsible analysis.

**PENDING ANALYSIS:** add platform/organ coverage for sex, age, ethnicity where reported, disease state, and missingness, with subgroup sample counts and sufficiently supported stratified performance.

**7. Compare cell-aligned and spot-centered performance.**

This is an important control. In native Xenium lung or breast, we will spatially aggregate measured cells into pseudo-spots, construct spot-centered images and expression, and train the same encoder/head under spot and native-cell supervision. Because averaging makes spot prediction easier, the key question is not whether cell correlation exceeds spot correlation. We will test whether cell supervision recovers within-pseudo-spot expression variance, marker separation among neighboring cell types, within-spot expression ordering, and coarse-state balanced accuracy, and whether the spot predictor makes heterogeneous neighboring cells artificially similar.

**PENDING EXPERIMENT:** complete this matched pseudo-spot study with identical regions, genes, splits, encoder, metrics, spatial-buffer control, and donor/sample bootstrap uncertainty.

**8. Explain how expression is matched to cells and consider renaming sMMC-22M.**

The matching rule is summarized in Response 4 and will appear before downstream experiments. We will say “bin-to-cell aggregation,” not “interpolation,” unless interpolation is performed. The rebuttal-time working manifest contains 28,315,247 post-QC aligned targets across 99 unique samples (100 versioned processing records), including 21 in-house DBiC-seq samples with 54,304 measured cells and 53,989 after paired-record QC. It also includes 12 newly added public Visium HD samples. All counting layers and changes from the submitted manifest will be explicit.

**AUTHOR INPUT REQUIRED:** choose either a stable versioned brand or consistent renaming to sMMC-28M across manuscript, repository, figures, and release files.

**9. The limitations are vague about transfer, errors hidden by spot averaging, and metadata/alignment coverage.**

We will state explicitly that: (i) within-sample results may exploit spatial autocorrelation; (ii) held-out tests expose but do not solve biological/acquisition shifts; (iii) native Xenium and derived Visium HD targets have different evidentiary status; (iv) segmentation and boundary rules propagate expression uncertainty; (v) spot averaging may conceal within-region cell-state errors, which Response 7 will test; (vi) metadata/demographic coverage is uneven; and (vii) predictions are not substitutes for molecular measurement.

**PENDING EXPERIMENT:** add a distance-buffered split and spatial-only baseline to quantify within-sample spatial autocorrelation.
