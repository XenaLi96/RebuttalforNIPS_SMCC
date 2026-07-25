We thank the reviewer for identifying three places where our previous presentation was insufficiently precise: the distinction between native cell-resolved and derived cell-aligned molecular targets, the breadth of the experimental evaluation, and the evidence for what cell-level evaluation adds beyond spot-level prediction. We clarify these distinctions below, report the validation and cross-organ results that are now available, and identify the matched analyses that remain in progress. We will also calibrate the title and claims and correct all presentation issues noted by the reviewer.

This resource is also personally meaningful to me. I began assembling it during the first year of my PhD, when I was new to spatial transcriptomics. The data were fragmented across platforms, most available resources were still organized at the spot level, and working with individual datasets required learning different software stacks, from Scanpy-based workflows to Xenium Ranger and other platform-specific tools. I therefore started organizing the datasets I encountered into a consistent single-cell and cell-aligned collection. Over the following years, I continued this curation alongside my research. Across successive submission rounds, the collection grew from 12 million cells to 20 million and then to the 22-million-cell version submitted here; our rebuttal-time working manifest has since reached 28 million. It also expanded from Xenium-only data to Xenium plus Visium HD and, most recently, to a growing in-house DBiC-seq collection generated with our collaborators. Along the way, it has supported several of my published studies and many projects by my co-authors. Watching the resource mature has paralleled my own growth as a researcher and has exposed me to recurring challenges across platforms, resolution, and biological context. These experiences are why, as I approach graduation, I hope to make the resource broadly available soon in a unified, accessible form, particularly for students entering the field who face the same practical barriers that I did.

**1. Native cell-resolved versus derived cell-aligned targets (P2, Q2, Q4 and L1–L2).**

The premise that all source datasets provide only spot-level expression is not correct, although we agree that our presentation did not foreground the distinction sufficiently. The submitted public inventory contains 23,943,826 upstream/source records: 16,314,129 Xenium cells defined by platform-provided cell boundaries and transcript coordinates, and 7,629,697 Visium HD source/segmentation records. After bin-to-cell aggregation, the corresponding native-plus-derived expression matrices contain 20,246,169 target rows: 16,314,129 native Xenium cells and 3,932,040 derived Visium HD cell-aligned profiles. We will report these counting layers separately rather than calling every record a directly measured single cell.

**Native-cell validation.** We evaluated four native-Xenium examples using a fixed 70/10/20 random-cell split (seed 42) and ten training-selected biological genes. Gene Pearson/Spearman was 0.541/0.544 for colon HCCD035 (77,636 test cells), 0.512/0.585 for breast HBCA041 (114,907), 0.406/0.459 for ovary HOCW023 (4,000), and 0.509/0.384 for skin HSDX009 (13,696). The corresponding cell Pearson values were 0.666, 0.546, 0.644, and 0.508, with F1 scores of 0.760, 0.766, 0.515, and 0.310. Against a coordinate-only baseline, image-model gene Pearson was 0.541 versus 0.238, 0.512 versus 0.116, 0.406 versus 0.285, and 0.509 versus 0.257; the 100-replicate spatial-block bootstrap intervals did not overlap in any sample. These results establish within-sample feasibility on genuine segmented-cell targets, but they do not establish sample-held-out generalization.

We therefore also ran bidirectional sample-held-out native-Xenium tests in lung and ovary (two held-out samples per organ). This harder result is substantially less favorable: UNI2-h all-gene Pearson averages were −0.0003 in lung and 0.0020 in ovary, and coarse-state balanced accuracies were 0.139 and 0.159. Metadata-only baselines reached all-gene Pearson of 0.104 and 0.142 and balanced accuracy of 0.220 and 0.252, respectively. ResNet50 and H-Optimus-0 were likewise near zero on all-gene Pearson. We will report these negative results: native target validity and cross-sample image-to-expression learnability are distinct questions, and the latter remains unsolved by the evaluated predictors.

Accordingly, we will use “native cell-resolved” for Xenium, “derived cell-aligned” for Visium HD, and “toward cell-level prediction” for claims spanning both platforms. We will not imply that every target is an independently measured whole-transcriptome single-cell ground truth. A conservative revised title is *sMMC: A Cell-Aligned Multimodal Resource for Spatial Transcriptomics*.

**AUTHOR INPUT REQUIRED:** confirm the final title and audit every title/abstract use of “single-cell,” retaining it only for native measurements or appropriately qualified goals.

**2. Cross-organ evaluation and independent biological units (P3, Q1, P8 and L4).**

We agree that the main text emphasized representative organs without a compact cross-organ summary. We therefore report the in-domain and cross-patient results together below. Table 1 is the same-sample, spatially held-out in-domain comparison of BLEEP, GHIST, sCellST, and our method across eight organs. Table 2 uses a separate source sample and target sample for each organ under a matched transfer protocol.

*Table 1. Eight-organ in-domain cell-level comparison. BL, GH, and SC denote BLEEP, GHIST, and sCellST evaluated under the same cell-aligned protocol; Ours denotes STBoost-Ref. BL is the cell-aligned BLEEP instantiation termed STBoosted BLEEP elsewhere in the paper. Bold marks the best result within each organ and metric.*

| Organ | Gene P BL | Gene P GH | Gene P SC | Gene P Ours | Gene S BL | Gene S GH | Gene S SC | Gene S Ours | Cell P BL | Cell P GH | Cell P SC | Cell P Ours |
|---|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|---:|
| Human ovary | 0.4239 | 0.4523 | 0.0800 | **0.6451** | 0.4396 | 0.4813 | 0.0640 | **0.6049** | 0.8212 | 0.7382 | 0.6858 | **0.8379** |
| Human lung | 0.4241 | 0.4754 | 0.1255 | **0.5178** | 0.4310 | 0.4814 | 0.1748 | **0.5077** | 0.5622 | 0.5787 | 0.4704 | **0.6042** |
| Human breast | 0.0079 | 0.0487 | 0.0032 | **0.5118** | 0.0051 | 0.0495 | 0.0070 | **0.5849** | 0.1282 | 0.1832 | 0.1240 | **0.5465** |
| Mouse kidney | 0.3356 | 0.3040 | 0.0236 | **0.3643** | **0.3468** | 0.2781 | 0.0264 | 0.3313 | **0.5369** | 0.5285 | 0.2601 | 0.5112 |
| Human tonsil | 0.1809 | 0.1591 | 0.0088 | **0.3817** | 0.1794 | 0.1488 | 0.0084 | **0.3354** | 0.7808 | 0.6107 | 0.7199 | **0.8193** |
| Human pancreas | 0.2147 | 0.2692 | 0.0513 | **0.4926** | 0.2035 | 0.2411 | 0.0427 | **0.4682** | 0.6143 | 0.5826 | 0.4371 | **0.7258** |
| Mouse brain | 0.2864 | 0.2417 | 0.0395 | **0.3972** | 0.2719 | 0.2253 | 0.0318 | **0.3765** | 0.4827 | 0.4691 | 0.2945 | **0.5536** |
| Mouse embryo | 0.2431 | 0.2105 | 0.0174 | **0.3489** | 0.2286 | 0.1964 | 0.0132 | **0.3207** | 0.4519 | 0.4273 | 0.2368 | **0.5084** |

*Table 2. Eight-organ cross-patient Visium HD benchmark, implemented as source-sample-to-target-sample transfer. TM is the training-mean baseline; its gene-wise Pearson is undefined because its prediction is constant across test cells.*

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
| **Organ-macro mean** | — | **0.0151** | **0.2036** | **0.0815** | **0.2422** | **0.0375** |

The cross-patient table uses a frozen UNI2-h encoder, seed 42, 20,000 source-reference cells, 2,000 validation cells, at most 5,000 target-test cells, five epochs, all shared genes, and an identical training-mean baseline. Every organ uses 5,000 target-test cells.

The contrast is clear. In-domain, our method is best in 22 of 24 organ–metric comparisons; the two exceptions are kidney gene Spearman and kidney cell Pearson, where BLEEP is best. Cross-patient transfer is much harder. UNI2-h improves organ-macro F1 over the training-mean baseline (0.0815 versus 0.0375) and produces nonzero gene-wise correlation, but it does not beat that baseline in macro cell Pearson (0.2036 versus 0.2422). Its cross-patient gene Pearson ranges from 0.0040 (pancreas) to 0.0272 (breast), cell Pearson from 0.0090 (pancreas) to 0.4593 (ovary), and F1 from 0.0369 (embryo) to 0.1794 (breast).

We do not hide this negative result or claim that the current predictor solves cross-patient generalization. The same construction, preprocessing, and target QC support the in-domain and transfer protocols, so this performance drop should not by itself be interpreted as evidence that the resource is invalid. Rather, it exposes a central unsolved problem: morphology, cell-state composition, acquisition conditions, and molecular distributions can change substantially across patients, and cross-platform transfer introduces an additional shift. Such patient and platform biases are also well recognized in single-cell analysis. We currently do not have a method that removes these shifts reliably. An important purpose of this resource is therefore to make the failure measurable across organs and to enable future work to develop stronger patient- and platform-robust methods.

The rebuttal-time working manifest contains 28,315,247 post-QC aligned targets across 99 unique source samples (100 versioned processing records), including 21 in-house DBiC-seq samples with 54,304 measured cells and 53,989 after QC. However, “sample” is not interchangeable with “donor.” The eight-organ experiment uses eight distinct source–target sample pairs (16 source samples). Table 2 uses “cross-patient” to match the manuscript’s task terminology, but operationally the current audit verifies a sample-held-out split; we will call it patient-held-out only where donor identity is verified. The revised manifest will report patient/donor/animal, specimen, and section support and keep repeated sections from one biological subject in the same split.

**AUTHOR INPUT REQUIRED:** insert verified human donor/patient and animal totals and per-organ grouping from source metadata; do not infer missing identities.

**3. Why cell-level evaluation matters (P4, Q3 and L3).**

We agree that a direct comparison is needed, but higher raw correlation at cell level is not the correct success criterion. Spot averaging reduces biological and measurement variance and therefore generally makes prediction numerically easier. The relevant question is whether averaging conceals within-region cellular heterogeneity that cell-level evaluation can detect.

Our completed preliminary lung diagnostic illustrates the first point but is not yet the matched test needed for the second. We aggregated sample k into 8,781 pseudo-spots, each formed from a $7\times7$ grid of native $8\,\mu\mathrm{m}$ blocks, and evaluated 1,630 right-third test pseudo-spots. Original BLEEP pseudo-spot Pearson was 0.416 for IGKC, 0.721 for MT-CO3, and 0.570 for SCGB1A1, compared with 0.014, 0.336, and −0.006, respectively, in the previous K→D cell-level output. Because these outputs use different training/evaluation settings and the cell targets are derived from Visium HD, this is evidence that aggregation can inflate raw correlation, not evidence that cell-level prediction is more accurate.

The decisive experiment will instead aggregate native Xenium cells into matched pseudo-spots and use identical tissue regions, genes, encoder/head, and splits for spot- and native-cell-supervised predictors. We will report within-pseudo-spot expression-variance recovery, marker separation among neighboring cell types, within-spot cell-pair ordering, coarse-state balanced accuracy, and prediction oversmoothing. Until that matched experiment is complete, we will remove the unsupported claim that our present results already demonstrate specific errors hidden by spot averaging.

**PENDING EXPERIMENT:** complete the matched native-Xenium pseudo-spot experiment with sample/donor bootstrap uncertainty and insert its direct heterogeneity results.

**4. Cell localization, expression assignment, and quality control (P6 and Q4).**

For Xenium, we use platform-provided cell boundaries and transcript coordinates. For Visium HD, we apply the official spatial transform to register the native bin grid to histology, obtain H&E cell footprints using CellViT, and aggregate bin counts over cell footprints. In the audited implementation, a closed native $2\,\mu\mathrm{m}$ bin square is assigned when it intersects a polygon. If more than one cell claims a bin, the nearest CellViT centroid in full-resolution coordinates wins; an exact tie uses the lower raw-cell index. We will state these rules, registration, segmentation, and QC separately by platform before the experiments.

We tested the default rule, strict exclusion of all boundary-touching bins, one-bin mask erosion, and one-bin dilation on lung A, lung B, and ovary. Their default assignments contained 126,629, 162,215, and 82,174 cells, respectively, with 1,650,497, 2,007,114, and 1,366,939 assigned bins. Strict-interior assignment retained 62.5%, 56.1%, and 80.7% of cells but only 16.0%, 11.0%, and 20.5% of bins; erosion retained 46.0%, 37.9%, and 72.0% of cells. Dilation retained essentially all cells and increased assigned bins to 184.4%, 196.2%, and 181.9% of default. These results reveal material boundary sensitivity; they do not yet justify a robustness claim. We will therefore expose assignment settings and report sensitivity rather than treating the default aggregation as ground truth.

**PENDING EXPERIMENT:** for the completed perturbations, insert matched-cell UMI/gene retention, expression and marker-rank concordance, coarse-state agreement, and fixed-checkpoint downstream changes with donor/sample or spatial-block uncertainty.

**AUTHOR INPUT REQUIRED:** verify the CellViT and registration versions, QC thresholds, and segmentation/centroid consistency statistics.

**5. Scope of the context analysis and demographic metadata (P4 and P9).**

We agree that the ovarian experiment neither validates molecular single-cell resolution nor supports a causal age claim. Its narrower purpose is to show a metadata-defined transfer failure hidden by pooled evaluation. Comparing AK (60+)→AD (60+) with AK (60+)→AL (40−), F1 decreases from 0.289 to 0.072, gene Spearman from 0.036 to 0.006, cell Spearman from 0.206 to 0.095, and predicted nonzero rate from 0.166 to 0.027. Because age is nested within sample/patient and tissue context and the panels differ, we will rename this a *sample/age-confounded context shift*, not an age effect, and use it only to motivate Average, Worst-context, Gap, and Support reporting.

Our manifest audit also shows that donor/patient identity, age, sex, and disease are not consistently documented across source studies, and ethnicity is absent or undocumented in the current source manifests. We will report coverage and missingness rather than infer sensitive attributes from images or names, remove the statement that no demographic bias is anticipated, and stratify performance only where independent-donor support and consent permit responsible analysis.

**PENDING EXPERIMENT:** rerun the ovarian comparison on one shared gene panel with matched preprocessing and uncertainty; add the source-reported context coverage/missingness table and only sufficiently supported subgroup analyses.

**6. Terminology, figures, and presentation (P5, P7 and minor comments).**

We will define STBoost at first use as a model-agnostic interface for training histology-to-expression methods on cell-aligned records. STBoost-Ref is our image-only retrieval predictor, whereas STBoosted BLEEP is the published BLEEP architecture retrained through the same interface. To remove the ambiguity noted by the reviewer, revised figures and prose will use these explicit names rather than a bare “Ours” label. The compact in-domain table retains “Ours” only because of its width and defines it directly as STBoost-Ref in the caption. At inference, STBoost-Ref receives local and wider-context histology crops, never query expression, and returns a cell-aligned expression vector.

Figure 1 was manually designed and edited by the authors in Sketch; it was not AI-generated. We will nevertheless correct “Hierarchical” and “Resolution (Single …),” complete the LLM-agent labels, improve the layout and caption, remove the Figure 3 border, and export figure text as vector elements. We will also define “study split,” “cell unit,” and Figure 2’s training ratio; remove the identified redundant framing text; correct all typographical and citation/link issues; and state precisely which data, metadata, and processed artifacts are released. The revised limitations will state that within-sample results may exploit spatial autocorrelation, sample-held-out tests expose but do not solve acquisition and biological shifts, native Xenium and derived Visium HD targets have different evidentiary status, boundary assignment propagates uncertainty, metadata coverage is uneven, and predictions are not substitutes for molecular measurement.

**Closing.** In summary, the revised framing will distinguish native Xenium measurements from derived Visium HD targets, support breadth claims with per-organ and independent-sample results, and ground the value of cell-level evaluation in direct cell-versus-spot evidence rather than terminology alone.
