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

**Rich context.** **Objective:** show why metadata are needed to audit context-dependent bias: a strong pooled score is insufficient if performance collapses for a subgroup. **Design/result:** in ovarian cancer, AK (60+) $\to$ AD (60+) versus AK (60+) $\to$ AL (40-) changes F1 from $0.289$ to $0.072$, gene Spearman from $0.036$ to $0.006$, cell Spearman from $0.206$ to $0.095$, and predicted nonzero rate from $0.166$ to $0.027$.

To test whether this was an isolated example, we applied the same Average/Worst/Gap audit to frozen biomedical encoders in complementary context-rich public cohorts:

| Domain | Task / encoder | Context; split | Avg. BA | Worst BA | Gap |
|---|---|---|---:|---:|---:|
| Single-cell | Bone marrow / Geneformer | assay; patient-CV | 0.938 | 0.669 | 0.327 |
| Single-cell | Bone marrow / scGPT | assay; patient-CV | 0.962 | 0.740 | 0.258 |
| Single-cell | Bone marrow / scVI-style | assay; patient-CV | 0.932 | 0.616 | 0.373 |
| Single-cell | Ten tissues / scGPT | dataset; patient-CV | 0.667--0.962 | 0.004--0.892 | max 0.975 |
| Pathology | LUAD KRAS / CONCH | site; leave-one-site | 0.499 | 0.375 | 0.542 |
| Pathology | LGG IDH / UNI | site; leave-one-site | 0.682 | 0.464 | 0.471 |
| Pathology | LGG IDH / H-optimus0 | site; leave-one-site | 0.747 | 0.476 | 0.524 |

BA is balanced accuracy; Gap is best-to-worst context, not Avg.-Worst. High averages coexist with collapse: scGPT reaches Worst BA 0.004, and site gaps for CONCH/UNI/H-optimus0 are 0.471--0.542. Thus, ovarian is an sMMC example of a recurring cross-modal problem. Rich context enables Average/Worst/Gap/Support audits, not bias removal. Since ovarian age is nested within sample/patient and panels differ, we call it age-associated and sample-confounded, not causal.

**3. Validation of generated data. How are the bin2cell-derived cell-level profiles and the LMM-generated textual records validated? Please describe the generation pipeline in the main text and show representative example records.**

We agree; this was omitted during revision and will be restored and summarized in the main text. **Bin-to-cell:** Appendix S2 shows the 10x workflow: official transforms register native bins to H&E; same-frame masks collect overlapping counts; unsupported cells are removed. These are deterministic cell-aligned---not direct single-cell---profiles; boundary audits show assignment sensitivity.

**Text:** GPT-4o only verbalizes existing fields; no new attributes are requested. We will restore the prompt, field checks, and side-by-side source/image/expression/text examples. A PLIP sample matrix (10 runs) and a CONCH organ matrix ($k=10$ captions/organ) favor matched/semantic pairs over mismatches. They test image relevance; field checks test factuality. Text is optional, not ground truth.

**4. The spot-to-cell component uses existing methodology and is not positioned against prior work.**

Agreed: no decomposition novelty is claimed; related work will be expanded.

**5. STBoost terminology, Table 2, and inference are unclear.**

See Response 2; the manuscript will match.

**6. Data accessibility, reproducibility, and bibliography formatting are inadequate.**

Add manifest/checksums/licenses/inventory; fix references.
