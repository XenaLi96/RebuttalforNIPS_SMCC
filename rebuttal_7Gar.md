We thank the reviewer for identifying where our framing ran ahead of the evidence. We agree that each benchmark experiment must state its objective, design, inputs, targets, metrics, and biological meaning. We will reorganize the paper accordingly and narrow claims for computationally derived targets.

**1. In-house data scale: how much is new relative to HEST-1K and STimage-1K4M, and what fraction is genuinely new versus aggregated?**

We audited the working manifest sample by sample against HEST-1K v1.1.0 and the STimage-1K4M metadata snapshot (2025-02-12), assigning mutually exclusive categories in this order: HEST overlap, STimage-only overlap, other public, and new in-house primary data. The rebuttal-time manifest also adds 12 public Visium HD samples:

| Category | Slides/samples | Organs or cell-line origins | Platforms | Aligned target cells after QC |
|---|---:|---|---|---:|
| **New in-house primary data** | **21** | **mouse-embryo; cervical; lung** | **DBiC-seq** | **53,989** |
| Public, overlapping HEST-1K | 32 unique slides (33 records) | 22 species--organ strata | Xenium; Visium HD | 11,051,149 |
| Public, overlapping STimage-1K4M only | 2 | 2 species--organ strata | Visium HD | 213,405 |
| Other public data | 44 | 16 species--organ strata | Xenium; Visium HD | 16,996,704 |
| **Total sMMC-28M** | **99 unique samples (100 records)** | **25 tissue strata + 3 cell-line origins** | **Xenium; Visium HD; DBiC-seq** | **28,315,247** |

The 21 in-house samples contain 54,304 measured cells; paired-record QC removes 315, leaving 53,989. Thus, newly measured in-house cells are 0.19% of the current aligned-cell total, HEST-overlapping public data are 39.03%, STimage-only overlap is 0.75%, and other public cohorts are 60.03%. We will state plainly that most scale comes from public aggregation and harmonization, while the DBiC-seq component is new primary acquisition.

The resource grew from sMMC-12M to 20M, the submitted 22M manifest, and the rebuttal-time 28M working manifest through public cohort additions and new DBiC-seq acquisition. Our broader ongoing DBiC-seq pool contains approximately 200,000 paired cells with morphology, RNA, and cellular context; it will be included only after a separate QC-frozen manifest reports exact samples, cells, checksums, and fractions.

One HEST-overlapping breast slide has two versioned processing records, hence 99 unique samples but 100 records. Visium HD counts refer to computational targets aggregated from canonical $2\,\mu\mathrm{m}$ bins; Xenium and DBiC-seq records are natively cell aligned.

**2. Clarify the purpose and biological meaning of the scale, resolution, and rich-context experiments, including Fig. 2, STBoost terminology, and the age analysis.**

We agree that repeatedly naming the three axes did not explain each experiment clearly. We will restructure them as follows.

**Scale. Objective:** test whether additional training cells are useful to multiple pathology foundation models/image encoders under a fixed prediction task. **Design/metric:** Fig. 2 predicts gene expression from histology while holding the encoder, targets, preprocessing, and test set fixed and increasing the training-cell fraction from 0.05 to 0.25; the primary metric is gene-wise Pearson correlation across test cells. Representative organs are used because this is a controlled data-volume study, not the breadth benchmark. **Result/significance:** performance increases with training fraction for the evaluated encoders within the tested range. This supports the value of additional training data in these settings, not a universal scaling law or a guarantee for every organ.

**PENDING EXPERIMENT:** insert audited values, exact cell counts, repetitions, and uncertainty for every Fig. 2 training fraction.

**Resolution. Objective:** test whether a spot-level image-to-expression method can operate on cell-aligned records and how performance changes under patient/sample shift. **Definitions:** STBoost is the model-agnostic cell-aligned data interface; BLEEP is the published spot-level baseline; STBoosted BLEEP is BLEEP retrained through that interface; STBoost-Ref is our native retrieval-based reference predictor. “Ours” is STBoost-Ref and will be renamed. At inference, STBoost-Ref receives only local and context histology crops, never query expression. **Result/significance:** on the same 5,000 test cells and 18,085 genes, STBoost-Ref improves most continuous metrics for $A\!\to\!A$ and every reported metric for $A\!\to\!B$; for example, cross-patient F1 increases from 0.0857 to 0.1061 and cell Pearson from 0.0896 to 0.2820. This demonstrates feasibility of the interface, not solved single-cell expression recovery.

**Rich context. Objective:** show why metadata are needed to audit context-dependent bias. A pooled score is insufficient when performance collapses in a subgroup defined by age, sex, disease background, or another relevant context. **Design/result:** within the ovarian-cancer cohort, we compare AK (60+) $\to$ AD (60+) with AK (60+) $\to$ AL (40-). F1 changes from 0.289 to 0.072, gene Spearman from 0.036 to 0.006, cell Spearman from 0.206 to 0.095, and predicted nonzero rate from 0.166 to 0.027. **Significance:** the gap shows why benchmarks should report Average, Worst-context, Gap, and subgroup Support. Rich context enables such audits; we do not claim to solve bias. Because age is nested within sample/patient and tissue context and the gene panels differ, we will call this an *age-associated, sample-confounded shift*, not a causal age effect.

**3. The derived cell profiles and generated text are not validated; describe the pipeline and show examples.**

We agree and will distinguish measurements from derived fields. Xenium provides native transcript coordinates and cell masks. For Visium HD, registered $2\,\mu\mathrm{m}$ bin counts are aggregated over segmented H&E footprints; these are cell-aligned computational targets. Captions are metadata-grounded generated annotations, not molecular ground truth and not inputs to the core benchmark. A complete record example will show provenance, local/context crops, coordinates, processed expression, metadata, generated text, and split key.

The audited rule assigns an intersecting closed bin square to a CellViT polygon; multi-cell claims go to the nearest centroid, with raw-cell index breaking exact ties. The $8\,\mu\mathrm{m}$ files are legacy QC/fallback artifacts.

Across lung A, lung B, and ovary, strict-interior assignment retains 62.5%/56.1%/80.7% of default cells and 16.0%/11.0%/20.5% of bins. One-bin dilation retains essentially all cells but increases assigned bins to 181.9%--196.2% of default, demonstrating material boundary sensitivity.

**AUTHOR INPUT REQUIRED:** verify the CellViT version, registration implementation, QC thresholds, and per-step removals.

**PENDING EXPERIMENT:** add matched-cell expression agreement, HVG/marker-rank stability, state concordance, and fixed-checkpoint downstream sensitivity with uncertainty for strict/eroded/dilated targets.

**PENDING EXPERIMENT:** add a blinded caption factuality audit with stratified records, two raters, a supported/unsupported/uncertain rubric, error types, and agreement. Until then, captions will be optional unvalidated fields.

**4. The spot-to-cell component uses existing methodology and is not positioned against prior work.**

Agreed. We will not claim a new decomposition algorithm. The contribution is provenance-preserving harmonization, a cell-centered multimodal interface, leakage-controlled splits, and evaluation across organs, platforms, and contexts. The construction will precede downstream experiments, related work will cover Bin2Cell and other spot-to-cell/super-resolution methods, and Visium HD targets will explicitly inherit their assumptions.

**5. STBoost terminology, Table 2, and inference are unclear.**

The definitions and inference protocol are given in Response 2 above. Table 2 will define a “cell unit” as one aligned multimodal cell record and point to construction and splits. We will introduce BLEEP/STBoost before first use, define AK/AD/AL, add task/metric/training-ratio definitions to Fig. 2, and remove repetitive framing.

**6. Data accessibility, reproducibility, and bibliography formatting are inadequate.**

Agreed. The revision and release will provide a versioned manifest separating measured, processed, generated, and model-output fields; split files; checksums; licenses; preprocessing commands; and a minimal record. We will inventory what is available at rebuttal time and state the timetable for restricted assets. We will also regenerate the bibliography in the required style so author lists are not uniformly truncated and verify every appendix reference.
