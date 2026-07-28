# Response to the Area Chair regarding Reviewer kUmZ

We thank Reviewer kUmZ for focusing on molecular provenance, evaluation breadth, and the value of cell alignment. We now separate native/derived targets, evaluate all release categories, and narrow the claims.

**1. Molecular provenance and alignment are now explicit.**

We separate native Xenium/DBiC-seq cells from HD targets derived by aggregating 2-µm bins within cell masks. DBiC-seq data are available in our [anonymous repository](https://anonymous.4open.science/r/sMMC-22M-DB75). We now document HD assignment/QC and provide a 9,000-polygon shift audit. We will use “native cell-resolved” and “derived cell-aligned,” remove unqualified single-cell claims, and adopt a count-independent title.

**2. The benchmark scope, task, and limitations are now clear.**

We report one protocol across all 25 categories and separate complete-sample transfer. Histology-only local-cell/context crops predict aligned-cell RNA; STBoost adapts spot predictors to this interface. Difficult transfer results remain visible rather than being presented as patient generalization.

Cell alignment preserves heterogeneity hidden by spot aggregation and supports cell type/state, communication, virtual-cell, and perturbation research. Context fields enable bias audits but do not establish representativeness; unsupported causal/fairness claims will be removed.

**3. Requested method details and Figure 1 authorship.**

The method details requested by the reviewer were not absent. Because this was submitted to the ED track as a dataset paper, we placed the complete STBoost design, equations, training objective, and architecture figure in the Appendix. We agree that a concise definition should also appear in the main body and will move it there.

I also want to address the statement that Figure 1 “feels AI-generated.” I began curating this resource in my first PhD year and am now approaching graduation. Figure 1 went through two complete versions in the Sketch app and the current source contains **161 editable layers**. Every circle, line, connector, label, and layout element was drawn and adjusted manually, one by one. The cumulative drawing and revision time exceeded one week. Screenshots of the editable layer workspace are available in the [anonymous repository](https://anonymous.4open.science/r/sMMC-22M-DB75) under `figure_evidence_not_AI/`; even modest magnification shows the individually constructed vector elements.

Having years of work, and this manually constructed figure, dismissed in one sentence as AI-generated was genuinely heartbreaking. Nevertheless, we appreciate the underlying concern about clarity. We will correct the labels, expand the caption, and improve the layout while retaining verifiable evidence of the figure’s manual construction.
