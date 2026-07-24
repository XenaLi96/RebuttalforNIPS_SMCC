We thank the reviewer for identifying places where our framing ran ahead of the evidence. We agree that a useful benchmark must state the scientific question, evaluation unit, inputs, targets, metrics, and conclusion for each experiment. We will substantially reorganize the paper around these items and narrow claims where the target is computationally derived.

**1. In-house data scale. Please quantify precisely how much new in-house data is contributed relative to HEST-1K and STimage-1K4M (samples, tissues, platforms, spots/cells), ideally as a comparison table. What fraction of the resource is genuinely new vs. aggregated?**

We agree that the previous manuscript did not clearly separate new primary acquisition from aggregation and processing. We will revise the paper to make the release history and provenance explicit. Across three submission rounds, the resource has grown from sMMC-12M to sMMC-20M, then to the submitted sMMC-22M manifest, and now to a rebuttal-time sMMC-28M working manifest. This growth comes from two sources: continuously added public spatial-transcriptomics cohorts with sample-level provenance checks, and new in-house DBiC-seq data with paired morphology, RNA, and cellular context from our co-authors.

To quantify overlap with existing resources, we audited the submitted sMMC-22M manifest sample by sample against HEST-1K (v1.1.0) and STimage-1K4M (metadata snapshot 2025-02-12), assigning samples to mutually exclusive categories in the order HEST overlap, STimage-only overlap, other public data, and new in-house primary data. The updated rebuttal-time manifest additionally includes 12 Visium HD samples under other public data. The table below reports this sMMC-28M working manifest.

| Category | Slides/samples | Organs or cell-line origins | Platforms | Aligned target cells after QC |
|---|---:|---|---|---:|
| **New in-house primary data** | **21** | **mouse-embryo; cervical; lung** | **DBiC-seq** | **53,989** |
| Public, overlapping HEST-1K | 32 unique slides (33 records) | 22 species--organ strata | Xenium; Visium HD | 11,051,149 |
| Public, overlapping STimage-1K4M only | 2 | 2 species--organ strata | Visium HD | 213,405 |
| Other public data | 44 | 16 species--organ strata | Xenium; Visium HD | 16,996,704 |
| **Total sMMC-28M** | **99 unique samples (100 records)** | **25 tissue strata + 3 cell-line origins** | **Xenium; Visium HD; DBiC-seq** | **28,315,247** |

The 21 in-house samples contain 54,304 measured cells. Final paired-record QC removes 315 cells, leaving 53,989 aligned target cells.

This audit shows that sMMC is not simply a repackaging of HEST-1K or STimage-1K4M: overlap is explicitly tracked, and the newly added public cohorts have low overlap with those resources. At the same time, we will state clearly that most of the submitted scale comes from public-data aggregation and harmonization rather than new primary acquisition.

The new primary component is our in-house DBiC-seq collection. The submitted manifest includes the QC-frozen DBiC-seq records summarized in the table, and in-house acquisition has continued since submission. The broader DBiC-seq pool now contains approximately $200{,}000$ paired cells and will be reported separately in the final sMMC-28M manifest with exact sample counts, cell counts, checksums, and fractions.

For transparency, one HEST-overlapping breast slide appears as two versioned processing records, which is why the table distinguishes 99 unique samples from 100 records. Public Visium HD aligned-cell counts report canonical $2\,\mu\mathrm{m}$ bin-to-cell targets, whereas native Xenium and DBiC-seq records are already cell aligned.

**2. Clear purpose and biological meaning of the per-axis experiments. For each experiment, please lay out the benchmark objective, design, results, and significance cleanly. In particular: What task and metric does Fig. 2 use? In the resolution study, what exactly are STBoost, STBoost-Ref, and “Ours”; what is BLEEP; and what is “STBoosted BLEEP”? For the age-effect analysis, what is the precise claim, and why is it informative beyond the expected degradation under an age shift?**

We agree. Although the manuscript repeatedly emphasized scale, resolution, and rich context, it did not state the purpose, design, result, and significance of each experiment clearly enough. We will reorganize this section accordingly.

**Scale.** **Objective:** test whether the increased data volume is useful---that is, whether multiple pathology foundation models and image encoders improve when they are given more training cells under the same prediction task. **Design and metric:** Fig. 2 performs histology-image-to-gene-expression prediction while holding the encoder, gene targets, preprocessing, and test set fixed and increasing the fraction of available training cells from $0.05$ to $0.25$. The primary metric is gene-wise Pearson correlation across test cells. We use representative organ settings rather than all organs because this is a controlled test of the value of additional data, not a breadth benchmark. **Result and significance:** within the tested range, prediction performance increases with the training-cell fraction across the evaluated encoders. This is the intended conclusion of the experiment: pathology pretraining and additional data improve prediction in these representative settings. We do not claim that this establishes a universal scaling law or guarantees improvement for every organ.

**PENDING EXPERIMENT:** before submission, insert the audited per-ratio values, exact training-cell counts, repetitions, and uncertainty for Fig. 2.

**Resolution.** **Objective:** test whether a spot-level image-to-expression pipeline can be trained and evaluated on cell-aligned records and how it behaves under patient shift. STBoost is the model-agnostic cell-aligned data interface; BLEEP is the published spot-level baseline; STBoosted BLEEP is BLEEP retrained through this interface; and STBoost-Ref is our native retrieval-based reference predictor. “Ours” in the current figure is STBoost-Ref and will be renamed. At inference, STBoost-Ref receives only local and context histology crops, never query expression. On the same 5,000 test cells and 18,085 genes, STBoost-Ref improves most continuous metrics in $A\!\to\!A$ and every reported metric in $A\!\to\!B$; for example, cross-patient F1 increases from $0.0857$ to $0.1061$, and cell-wise Pearson from $0.0896$ to $0.2820$. This supports the feasibility of the cell-aligned interface, not a claim that true single-cell expression recovery is solved.

**Rich context.** **Objective:** provide a showcase of why rich metadata is needed to audit context-dependent performance bias. In standard machine-learning and computer-vision tasks, a strong pooled score is insufficient if performance collapses for a subgroup defined by age, sex, disease background, or another relevant context. We argue that histology-to-expression benchmarks should take the same issue seriously. **Design and result:** within the ovarian-cancer cohort, we compare transfer from AK (60+) to AD (60+) with transfer from AK (60+) to AL (40-). F1 changes from $0.289$ to $0.072$, gene Spearman from $0.036$ to $0.006$, cell Spearman from $0.206$ to $0.095$, and the predicted nonzero rate from $0.166$ to $0.027$. **Significance:** this large subgroup gap is the point of the showcase. It shows that an average score can hide severe worst-context failure and motivates reporting average performance, worst-context performance, the gap between them, and subgroup support. The contribution of rich context is to make such audits and future bias-mitigation studies possible; we do not claim to solve the bias here. Because age is currently nested within sample/patient and tissue context and the comparisons use unequal gene panels, we will describe this result as an age-associated, sample-confounded shift rather than a causal age effect.

**3. The derived cell profiles and generated text are not validated; describe the pipeline and show examples.**

Agreed. Xenium provides native transcripts/masks; Visium HD targets aggregate registered $2\,\mu\mathrm{m}$ bins over segmented H&E footprints, resolving conflicts by nearest centroid. Captions are generated annotations, not molecular truth or benchmark inputs.

For lung A/B and ovary, strict assignment retains 62.5%/56.1%/80.7% of cells and 16.0%/11.0%/20.5% of bins; dilation retains almost all cells but 181.9%--196.2% of bins. We will show a record, test expression/marker/state stability, and audit captions; until validated, captions remain optional.

**AUTHOR INPUT REQUIRED:** verify CellViT/registration versions and QC thresholds.

**4. The spot-to-cell component uses existing methodology and is not positioned against prior work.**

Agreed. We will not claim a new decomposition algorithm. We will emphasize harmonization, the cell-centered interface, controlled splits, and evaluation; explain construction first; cover Bin2Cell-related work; and state inherited assumptions.

**5. STBoost terminology, Table 2, and inference are unclear.**

Response 2 defines the terms and inference. We will define “cell unit,” introduce methods before use, define AK/AD/AL, and clarify Fig. 2.

**6. Data accessibility, reproducibility, and bibliography formatting are inadequate.**

Agreed. We will provide a versioned provenance manifest, splits, checksums, licenses, preprocessing commands, a minimal record, and an exact inventory; fix bibliography style; and verify appendix references.
