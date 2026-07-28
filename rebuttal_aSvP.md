We sincerely thank the reviewer for identifying two questions that the manuscript had not explained clearly enough: how the data promote research, and how increased heterogeneity affects downstream analysis. We will organize the revision around these questions, revise point by point, and hope the reviewer will reconsider.

**Q1 (Weakness).** *How can the new data truly promote relevant research progress?*

**A1 — Lowering practical barriers.** Existing data are fragmented across platforms, panels, formats, segmentation, and metadata schemas. sMMC provides one cell-aligned interface for morphology, context, molecular targets, coordinates, and available donor metadata. The 25-organ release becomes usable through one pipeline while preserving provenance: native Xenium/DBiC-seq cells remain distinct from Visium HD targets derived within cell masks. Data loading, preprocessing, splitting, and evaluation can therefore be reused across studies.

**A2 — Enabling reproducible method development.** Image-only local-cell and tissue-context crops predict RNA aligned to a cell. STBoost lets existing spot predictors use this hierarchical interface while retaining their prediction modules.

The resource supports several complementary research settings:

- spatially separated regions for within-sample prediction;
- complete-sample transfer for acquisition/composition shifts;
- verified patient/donor holdout for biological transfer;
- panel-matched cross-platform transfer;
- 25-organ breadth and context-aware evaluation.

Together, these protocols ask whether additional data help, whether encoders capture morphology rather than location, and whether gains persist across organs, donors, technologies, and contexts. A fixed interface also lets new architectures, normalization methods, and adaptation strategies be compared without rebuilding the dataset.

Fixed splits and code let future methods use identical targets, supporting studies of scaling, foundation encoders, adaptation, robust learning, and biological transfer. This is infrastructure for comparing progress, not a claim that prediction is solved.

**A3 — Expanding cell-centered downstream research.** Spots merge cells with different identities, states, and responses. Moving image and molecular targets toward the same cell preserves local heterogeneity and supports:

- **cell type/state:** connect morphology with identity and disease programs;
- **cell–cell communication:** study neighboring-cell interactions and tissue architecture;
- **virtual-cell/perturbation modeling:** link morphology, context, state, and response, then trace effects through interacting cells;
- **microenvironment analysis:** distinguish cells within a niche rather than assign one averaged profile.

sMMC does not solve these tasks; it supplies a common substrate for training and evaluating them across organs and contexts. We will state these opportunities explicitly.

**Q2 (Heterogeneity).** *What heterogeneity is introduced by increased scale, resolution, and context, and how may it influence downstream analysis?*

**A1 — Sources of heterogeneity.** We agree that a larger resource contains more variation. We will document:

- **biological:** organ, species, disease, composition, density, and donor;
- **technical:** platform, study, acquisition, segmentation, and resolution;
- **molecular:** panel overlap, coverage, sparsity, and target construction;
- **context:** uneven support for age, sex, disease, site, and patient;
- **scale:** unequal samples/cells across organs and studies.

**A2 — Downstream influence.** Cell-weighted pooling can let large organs dominate; unmatched panels make gene metrics incomparable; platform/study effects can resemble biology; segmentation and density change targets and neighborhood mixing; incomplete donors make sample shift look like patient shift; uneven context support confounds subgroup comparisons. A model may therefore optimize dominant tissues, learn technical signatures, or appear robust because averaging hides rare-context failures. Heterogeneity defines deployment shifts, while the unified manifest supports normalization, panel alignment, adaptation, calibration, and robust generalization.

**A3 — Controls and manuscript revisions.** Following the reviewer’s advice, we will:

- report organ-macro/platform-separated summaries and panel overlap;
- separate native/derived targets and spatial/sample/patient/platform protocols;
- report Average/Worst/Gap, donor support, and missingness;
- reserve “patient-held-out” for independent donors;
- label measured, processed, generated, and model-derived fields.

These changes do not remove heterogeneity; they make its downstream effects auditable. We again thank the reviewer: these two questions make the resource’s purpose, opportunities, and limitations clearer.
