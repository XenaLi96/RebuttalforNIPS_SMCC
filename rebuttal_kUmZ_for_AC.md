# Response to the Area Chair regarding Reviewer kUmZ

We thank Reviewer kUmZ for focusing on molecular provenance, evaluation breadth, and the value of cell alignment. We now separate native/derived targets, evaluate all release categories, and narrow the claims.

**1. Molecular provenance and alignment are now explicit.**

We separate native Xenium/DBiC-seq cells from HD targets derived by aggregating 2-µm bins within cell masks. DBiC-seq data are available in our [anonymous repository](https://anonymous.4open.science/r/sMMC-22M-DB75). We now document HD assignment/QC and provide a 9,000-polygon shift audit. We will use “native cell-resolved” and “derived cell-aligned,” remove unqualified single-cell claims, and adopt a count-independent title.

**2. The benchmark scope and the purpose of moving from spots toward cells are now clear.**

We provide complete results across all 25 release categories and eight cross-sample settings. The task uses histology-only local-cell/context crops to predict RNA aligned to a cell, and STBoost adapts existing spot predictors to this interface. These experiments clarify our central goal: to provide the data, task, and benchmark needed to move image-to-expression research from multi-cell spots toward the cell scale.

The reviewer questions our comparison with spot-level methods and suggests that cell-aligned prediction should achieve substantially higher accuracy than spot-centered prediction before we may claim to be “moving toward single-cell prediction.” We find this requirement difficult to understand. This is an ED-track dataset and benchmark paper: our responsibility is to formulate an important task, provide data and reproducible protocols, establish baselines, and make the unsolved problem available to the community. Requiring the first benchmark paper to solve the task before it may introduce it would undermine the purpose of releasing challenging benchmarks—there would be little left for subsequent methods to improve.

Moreover, the cell-level task is intrinsically harder, not easier. In representative settings, the prediction unit shrinks from a spot containing roughly twenty cells to one cell—approximately one-twentieth of the aggregated molecular material. Spot averaging reduces noise and biological heterogeneity, so it can produce higher correlations even when it hides cell-specific errors. We therefore do not believe cell-level accuracy must exceed spot-level accuracy before the task can legitimately be described as moving toward cell-resolved prediction.

The reviewer also questioned our statement that cell-level prediction is more biologically meaningful because it exposes errors hidden by spots. We believe the point is direct: a spot may contain multiple cell types, yet averaging assigns them one molecular profile. This hides within-spot cell-type differences, marker separation, cell-state variation, and the cellular interactions needed for communication analysis. A spot predictor can look accurate at the averaged level while being unable to distinguish the cells inside that spot.

We have deep respect for the spot-level literature and have implemented and evaluated nearly all leading spot-level approaches. STBoost was proposed precisely to preserve these effective methods while lifting their input/target interface toward cells, not to dismiss their contribution. At the same time, wet-lab biology is rapidly moving toward single-cell assays. Cell-type/state analysis, single-cell perturbation, virtual-cell modeling, and cell–cell communication all operate naturally at the cell level; cell-resolved spatial transcriptomics is the bridge from a virtual cell to a virtual tissue by showing how perturbed cells interact in space. This is why we believe the benchmark opens an important research direction even though current baselines do not solve it.

Context fields further enable bias audits but do not establish population representativeness; unsupported causal or fairness claims will be removed.

**3. Requested method details and Figure 1 authorship.**

The method details requested by the reviewer were not absent. Because this was submitted to the ED track as a dataset paper, we placed the complete STBoost design, equations, training objective, and architecture figure in the Appendix. We agree that a concise definition should also appear in the main body and will move it there.

I also want to address the statement that Figure 1 was “AI-generated,” together with the suggestion that I should at least adjust it manually. I began curating this resource in my first PhD year and am now approaching graduation. Figure 1 went through two complete versions in the Sketch app and the current source contains **161 editable layers**. Every circle, line, connector, label, and layout element was drawn and adjusted manually, one by one. The cumulative drawing and revision time exceeded one week. Screenshots of the editable layer workspace are available in the [anonymous repository](https://anonymous.4open.science/r/sMMC-22M-DB75) under `figure_evidence_not_AI/`; even modest magnification shows the individually constructed vector elements.

Having years of work, and this manually constructed figure, dismissed in one sentence as AI-generated was genuinely heartbreaking. Nevertheless, we appreciate the underlying concern about clarity. We will correct the labels, expand the caption, and improve the layout while retaining verifiable evidence of the figure’s manual construction.
