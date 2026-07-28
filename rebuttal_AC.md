# Response to the Area Chair

We thank the Area Chair and reviewers. **sMMC-22M advances spatial transcriptomics from spot averages toward cell-centered multimodal analysis by linking morphology, RNA, location, and biological context at scale.**

Below, we summarize each central question, our response, and the resulting evidence.

## Reviewer 7Gar: contribution, experimental purpose, and validation

- **Question—What is genuinely new? Response—** We completed a mutually exclusive provenance audit. The release contains **28,315,247 aligned targets from 99 unique samples**, including **53,989 newly measured native DBiC-seq cells from 21 in-house samples**. Only 32 samples overlap HEST-1K and two additional samples overlap STimage-1K4M only; approximately **70% is absent from HEST-1K**. This separates new measurements, public aggregation, and processed derivatives.

- **Question—What do the three experimental axes demonstrate? Response—**
  - *Scale* tests whether more paired data improve foundation encoders. Across seven pathology foundation encoders, using 5%, 10%, 25%, and 100% of the data increases mean Gene Pearson from **0.177 to 0.205, 0.241, and 0.278**. This validates the value of scale.
  - *Resolution* tests whether cell alignment preserves variation lost through regional averaging.
  - *Rich context* tests whether metadata reveal failures hidden by average scores. Across assay, dataset, and site contexts, **Average–Worst balanced-accuracy gaps reach 0.258–0.975**.

  Each axis now has an explicit question, design, metric, and conclusion.

- **Question—How are derived profiles and textual records validated? Response—** Visium HD targets use registered native 2-µm bins intersected with cell masks; conflicts are resolved deterministically and unsupported cells removed. Across **9,000 polygons** from lung, brain, and pancreas, ±1-µm perturbations give bin Jaccard **0.706–0.816** and expression cosine **0.936–0.994**. GPT-4o converts supplied metadata into readable descriptions; PLIP/CONCH audits give AUC **0.993/0.988** and **100% Top-3 retrieval**. Full prompts, provenance, QC, and examples will be added to the Appendix.

## Reviewer kUmZ: target validity, benchmark breadth, and biological utility

- **Question—Which targets are natively cell resolved? Response—** We separate **16,314,129 native Xenium cells** and **53,989 native DBiC-seq cells** from **7,629,697 derived Visium HD cell-aligned targets**, using *native cell-resolved* and *derived cell-aligned*.

- **Question—Does evaluation support the resource’s breadth? Response—** We now report complete benchmark results for **all 25 organ categories** under a unified evaluation protocol. We additionally provide **eight cross-sample transfer results** in which the complete test sample is unseen. Together, these experiments replace the original small set of representative cases with release-wide and cross-sample evidence.

- **Question—What does cell alignment add beyond spot evaluation? Response—** Across six native-Xenium samples, cell/8/16/55-µm targets yield Gene Pearson **0.365/0.365/0.363/0.330**. Yet 55-µm pseudo-spots mix cell types in **55.5–66.4%** of dense-tissue regions and affect **73.8–81.0%** of cells. **Cell alignment preserves cellular identity and local heterogeneity obscured by regional averaging.**

## Reviewers vCPF and aSvP

We appreciate their constructive suggestions. vCPF requested HD boundary validation, broader organ coverage, and spatial/metadata calibration; these are addressed by the audits, 25-category benchmark, and added mean, coordinate, spatial-KNN, and segmentation-metadata baselines. aSvP asked how scale and heterogeneity translate into research value; the scaling, release-wide, cell-versus-spot, and context-gap analyses provide this evidence.

## Summary

**sMMC-22M provides an audited, release-wide foundation connecting spatial transcriptomics with cell-centered computational biology.** Moving from spots toward cells enables cell-type/state prediction, cell–cell communication, morphology–RNA analysis, perturbation-response modeling, and spatially grounded virtual-cell development—questions difficult when heterogeneous cells are averaged into one spot.

Following the reviewers’ suggestions, we will clarify STBoost and the experimental objectives; distinguish native, processed, and generated records; add the release-wide results, audits, fixed splits, QC, prompts, and examples; and release the code and in-house data. **The new evidence answers the central questions about novelty, target validity, breadth, heterogeneity, context, and reproducibility. We hope it resolves the reviewers’ concerns and demonstrates the utility of sMMC-22M.** We thank the Area Chair and reviewers for recognizing its value and helping us strengthen it.
