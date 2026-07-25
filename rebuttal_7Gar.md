We thank the reviewer for identifying places where our framing ran ahead of the evidence. We agree that a useful benchmark must state the scientific question, evaluation unit, inputs, targets, metrics, and conclusion for each experiment. We will substantially reorganize the paper around these items and narrow claims where the target is computationally derived.

This resource is personally meaningful: it has accompanied me since my first PhD year, when I was new to spatial transcriptomics. It has supported my research and collaborations. Near graduation, I hope its unified release helps students enter the field.

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

**Scale.** **Objective:** test whether the increased data volume is useful---that is, whether multiple pathology foundation models and image encoders improve when given more training cells under the same prediction task. **Design and metric:** Fig. 2 performs histology-image-to-gene-expression prediction while holding the encoder, gene targets, preprocessing, and test regions fixed and varying the training-cell fraction. The primary metric is gene-macro Pearson correlation across held-out cells. We now audited this trend on native Xenium sample HHDX011 using seven frozen encoders, top-50 HVGs selected only from each training partition, four contiguous edge holdouts, and a 5% spatial buffer. Each holdout contained 2,400 test cells; the 5%/10%/25%/100% settings used 445--447/890--895/2,226--2,237/8,912--8,948 training cells.

| Encoder | 5% | 10% | 25% | 100% |
|---|---:|---:|---:|---:|
| ResNet-50 | 0.140 | 0.160 | 0.190 | 0.221 |
| CTransPath | 0.162 | 0.186 | 0.222 | 0.259 |
| Phikon | 0.168 | 0.200 | 0.237 | 0.273 |
| CONCH | 0.178 | 0.213 | 0.256 | 0.292 |
| UNI | 0.188 | 0.218 | 0.259 | 0.295 |
| UNI2 | 0.197 | 0.225 | 0.257 | 0.295 |
| H-Optimus-0 | 0.205 | 0.233 | 0.269 | 0.307 |

Every encoder improves monotonically; the seven-encoder mean increases from 0.177 to 0.205, 0.241, and 0.278. This is the intended scale conclusion: under a fixed spatially held-out task, additional training cells improve all evaluated encoders. It is not presented as a universal parametric scaling law.

To separate this controlled scale question from dataset breadth, we also evaluated the same CONCH--Ridge predictor on 30 native-Xenium samples spanning 17 organ/tissue labels, 25 dataset-reported condition labels, and two species. Each sample contributed up to 12,000 spatially stratified cells and was summarized as one unit:

| Model | Gene Pearson | Gene Spearman | Cell Pearson | Gene F1 |
|---|---:|---:|---:|---:|
| Image | 0.324 | 0.291 | 0.442 | 0.413 |
| Coordinate-only | 0.046 | 0.041 | 0.288 | 0.283 |
| Spatial KNN | 0.053 | 0.051 | 0.268 | 0.320 |
| Training mean | 0.000 | -- | 0.305 | 0.253 |

The image model's sample-macro gene Pearson is 0.324 (10,000-bootstrap 95% CI 0.277--0.368) and exceeds all three controls in sample-level paired tests (Holm-adjusted $p\leq1.9\times10^{-8}$). These are sample-specific, training-selected top-50-HVG results, not full-panel correlations or donor-held-out estimates.

**Resolution.** **Objective:** test whether a spot-level image-to-expression pipeline can be trained and evaluated on cell-aligned records and what information changes under spatial aggregation. STBoost is the model-agnostic cell-aligned data interface; BLEEP is the published spot-level baseline; STBoosted BLEEP is BLEEP retrained through this interface; and STBoost-Ref is our native retrieval-based reference predictor. “Ours” in the current figure is STBoost-Ref and will be renamed. At inference, STBoost-Ref receives only local and context histology crops, never query expression. On the same 5,000 test cells and 18,085 genes, STBoost-Ref improves most continuous metrics in $A\!\to\!A$ and every reported metric in $A\!\to\!B$; for example, cross-patient F1 increases from $0.0857$ to $0.1061$, and cell-wise Pearson from $0.0896$ to $0.2820$.

We additionally constructed matched 8/16/55-$\mu$m pseudo-spots from six native-Xenium samples, using identical held-out regions, training-only gene selection, encoder, and regression head:

| Evaluation unit | Cell | 8 $\mu$m | 16 $\mu$m | 55 $\mu$m |
|---|---:|---:|---:|---:|
| Gene Pearson | 0.365 | 0.365 | 0.363 | 0.330 |

The result does not show that cell-level prediction is universally easier: 8- and 16-$\mu$m aggregation changes little. Instead, it shows that coarse aggregation can conceal heterogeneity in a density-dependent manner. In dense lung sample HLCX022, 55.5%--66.4% of 55-$\mu$m pseudo-spots mixed predicted cell types and contained 73.8%--81.0% of evaluated cells across four spatial holdouts; the corresponding values in sparse heart sample HHDX011 were 3.5%--4.1% and 7.0%--8.3%.

We further calibrated whether the spatially held-out predictions preserve biological structure rather than relying only on pooled full-panel correlation:

| Biological calibration | Image | Coordinate | Spatial KNN | Mean |
|---|---:|---:|---:|---:|
| Marker/HVG gene Pearson $\uparrow$ | 0.201 | 0.031 | 0.028 | 0.000 |
| Cell-type-stratified pseudobulk RMSE $\downarrow$ | 0.120 | 0.224 | 0.187 | 0.188 |

The marker set is the overlap between training-selected HVGs and the available marker universe; pseudobulk strata use the available molecularly predicted cell-type annotations. Thus, these results support recoverable marker and cell-type aggregate structure, while neither claiming exact cell-wise recovery nor treating predicted annotations as independent ground truth.

**Rich context.** **Objective:** show why metadata are needed to audit context-dependent bias: a strong pooled score is insufficient if performance collapses for a subgroup. **Design/result:** in ovarian cancer, AK (60+) $\to$ AD (60+) versus AK (60+) $\to$ AL (40-) changes F1 from $0.289$ to $0.072$, gene Spearman from $0.036$ to $0.006$, cell Spearman from $0.206$ to $0.095$, and predicted nonzero rate from $0.166$ to $0.027$. A complementary native-Xenium same-organ analysis likewise shows substantial sample heterogeneity: across 18 held-out target samples from six organ/tissue groups, training-selected top-50-HVG Pearson averages 0.170 but ranges from 0.016 to 0.372. Because donor identity is incomplete, we describe this strictly as sample-held-out, not patient-held-out, evidence.

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

**3. How are bin2cell targets constructed and validated?**

Appendix S2 shows the Visium HD construction: official transforms register native $2\,\mu$m bins to H&E; a closed bin square is assigned when it intersects a same-frame cell polygon; unsupported cells are removed; and overlapping claims are resolved deterministically. We will move this description and a complete record example into the main text and label the outputs *derived cell-aligned targets*, not directly measured single-cell RNA.

We also completed a deterministic boundary-sensitivity audit on 3,000 raw CellViT polygons per dataset (9,000 total), with 1-$\mu$m x/y registration shifts and 1-$\mu$m mask erosion/dilation:

| Visium HD example | Raw polygons with canonical bins | Shift bin Jaccard | Shift expression cosine | Shift median $|\Delta\mathrm{UMI}|$ |
|---|---:|---:|---:|---:|
| Human lung cancer | 49.4% | 0.727--0.733 | 0.954--0.957 | 11.1%--11.3% |
| Mouse brain | 97.5% | 0.806--0.816 | 0.936--0.939 | 6.0%--6.1% |
| Human pancreas | 50.0% | 0.706--0.714 | 0.994 | 13.0%--13.7% |

The first percentage uses all raw detected polygons; polygons without supported tissue bins are excluded before constructing released targets. Among supported polygons, small registration shifts preserve expression direction well but change exact bin membership and UMI totals. The more severe erosion/dilation audit gives median bin Jaccard 0.462--0.720, expression cosine 0.871--0.993, and absolute UMI changes of 32.8%--66.6%. We will therefore expose assignment parameters and per-sample retention/QC rather than presenting Visium HD aggregation as error-free ground truth. Native Xenium results remain reported separately because they use platform-defined cell boundaries and native per-cell counts.

**4. How are LMM-generated texts produced and validated?**

GPT-4o only verbalizes supplied fields and adds no attributes. Detailed audits were completed but omitted while this secondary section was shortened across revisions. This was a serious oversight; we apologize. We will restore the prompt, field checks, examples, and matrices:

| Model | Match $n$ | Match sim. | Mismatch sim. | Enrichment | Pairwise AUC | Top-1 / Top-3 |
|---|---:|---:|---:|---:|---:|---:|
| PLIP | 15 | 0.262 | 0.053 | 4.96× | 0.993 | 84.6% / 100% |
| CONCH | 29 | 0.040 | 0.0021 | 19.1× | 0.988 | 90.9% / 100% |

Blue boxes define matches. AUC separates matched from mismatched scores; Top-$k$ asks whether a match is among the $k$ nearest captions. Both VLMs reach AUC $\geq0.988$ and 100% Top-3. These are similarity—not Pearson-correlation—audits of image relevance; source-field checks test factuality. Text remains optional context, not ground truth.
