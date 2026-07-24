# Submission 13175 Rebuttal Experiment TODO

> 目标：按“致命性 × reviewer 覆盖面 × 完成速度”逐条清空 central concerns。
>
> 状态快照：2026-07-23。
>
> Package readiness：`blocked`，直到 P0、P1A、P1B 的核心证据完成并核验。

## 使用规则

- `[ ]` 未开始；`[~]` 进行中或部分完成；`[x]` 已完成并核验；`[!]` 阻塞。
- 只有同时满足“实验/分析完成、结果人工核验、证据路径存在、rebuttal/正文位置确定”才能改为 `[x]`。
- 脚本写完、作业提交、单个 task 成功，都不等于整项完成。
- 所有计数以 P0 canonical manifest 为唯一来源；不得混用 source/segmentation cells 与最终 expression-matrix rows。
- Xenium native-cell 与 Visium HD bin-to-cell aligned 结果分开汇总；不得做 cell-weighted pooled average。
- P4 必须等待 P0 support matrix；P3 必须等待 P1B pipeline 稳定。
- 最终只为 P1A、P1B、P2 三张关键表补 uncertainty；不为所有旧实验重跑大量 seeds。

## 总优先级与依赖

| 顺位 | 项目 | 致命性 | Reviewer 覆盖 | 完成速度 | 当前状态 | 依赖 |
|---:|---|---|---|---|---|---|
| 1 | P0 canonical audit | Blocking | aSvP、vCPF、7Gar、kUmZ | 快，CPU/人工 | `[!]` 初版已生成，缺 verified donor/context metadata | 无 |
| 2 | P1A Visium HD boundary sensitivity | Blocking | vCPF、7Gar、kUmZ | 中，CPU | `[~]` 2/9 aggregation tasks 完成 | P0 样本定义 |
| 3 | P1B native Xenium benchmark | Blocking | 四位 reviewer | 中，GPU | `[~]` sample-held-out array 运行/排队中 | P0 donor 定义 |
| 4 | P2 eight-organ breadth | High | aSvP、vCPF、7Gar、kUmZ | 中，GPU | `[ ]` pipeline ready，未提交 | P0 样本/organ 定义 |
| 5 | P3 Xenium pseudo-spot control | High | kUmZ、7Gar、vCPF | 中，GPU | `[ ]` 未开始 | P1B pipeline 稳定 |
| 6 | P5 caption factuality audit | Medium | 7Gar、vCPF | 快，人工 | `[ ]` 未开始 | P0 分层字段 |
| 7 | P4 support-calibrated context-bias audit | Medium--High | 四位 reviewer | 慢，GPU/统计 | `[!]` 等待 support matrix | P0 完成 |
| Gate | P6 uncertainty | High | 四位 reviewer/AC | 快--中，统计 | `[ ]` 等 P1A/P1B/P2 final outputs | P1A、P1B、P2 |

## 当前并行队列

- `[~]` CPU-1：P0 manifest 已生成，但 author metadata override 尚未补齐。
- `[~]` CPU-2：P1A job `257454`；仅 task 0 (`k/strict_interior`) 和 task 2 (`ad/strict_interior`) 完成，其余 7 个 task 因 `/lv_scratch` 写权限失败。
- `[~]` GPU-1：P1B 原 job `257463` 仅 task 0 完成；tasks 1--11 已由 `257484` 重提，task 1--2 运行中、3--11 排队；summary job `257486` 等待中。
- `[ ]` GPU-2：P2 已有 eight-pair manifest 和提交脚本，尚无正式 job ID。

**下一轮按此顺序清空**

1. P0：作者补 source/study/sample/donor/context mapping，并重新生成三张表。
2. P1A：修正 output root，只重提失败的 7 个 tasks；完成后立即做 aggregation/marker/state/benchmark summary。
3. P1B：等待 `257484`，逐 fold 验证 alignment、gene panel 和 baselines；不要重复提交运行中的 tasks。
4. P2：有空闲 GPU 即提交最低版本 eight-organ UNI2-h + training mean。
5. P5：GPU 运行期间冻结抽样 seed、record IDs 和双人标注 rubric。

---

## P0 — Canonical manifest、计数与 support matrix

**Reviewer mapping**

- aSvP-2：heterogeneity 及其下游影响。
- vCPF-6：measured / processed / generated provenance。
- 7Gar-3、7Gar-6：in-house contribution、data access/reproducibility。
- kUmZ-5、kUmZ-6、kUmZ-8：patient/animal counts、demographics、22M/23M 命名。

**当前证据**

- `rebuttal_experiments/p0_manifest/canonical_manifest.tsv`
- `rebuttal_experiments/p0_manifest/label_context_donor_support.tsv`
- `rebuttal_experiments/p0_manifest/table1_source_provenance_counts.tsv`
- `rebuttal_experiments/p0_manifest/table2_organ_coverage.tsv`
- `rebuttal_experiments/p0_manifest/table3_context_donor_support.tsv`
- `rebuttal_experiments/p0_manifest/data_inconsistency_report.md`

**已核对事实**

- `[x]` 42 个 native Xenium source records，16,314,129 native target cells。
- `[x]` 25 个 Visium HD source records，7,629,697 source/segmentation cells。
- `[x]` canonical Visium HD target 使用 `square_002um` 与 CellViT contours；2 µm bin-to-cell matrix 实际包含 3,932,040 target cells。
- `[x]` 当前 native + derived expression targets 合计 20,246,169。
- `[x]` 53、66、67 的来源已追溯；它们不是可互换的 biological-study counts。
- `[x]` 8 µm 文件只用于 legacy QC/fallback，不是 canonical expression target。
- `[x]` 当前本地 manifest 没有证据确认 in-house biological samples；当前只能写“0 documented”，不能写“确定不存在”。

**待清空**

- `[!]` 由作者为 67 个 included source records 补充 evidence-backed：
  `study_id`、`biological_sample_id`、`patient_id/donor_id`、age、sex、disease、public/in-house。
- `[ ]` 区分并冻结四层计数：
  upstream source、biological samples、native targets、derived targets。
- `[ ]` 决定 manuscript headline 使用哪个数量；同步修正 22M/23.94M/20.25M 的含义，不得把 7.63M source count 当作 3.93M derived matrix rows。
- `[ ]` 根据真实 pipeline 全文统一 2 µm 与 8 µm 描述。
- `[ ]` 重新生成并人工核验三张 rebuttal 表：
  1. public aggregation / documented in-house biological samples / processed derivatives；
  2. 每 organ 的 samples、patients/donors、platforms、native/derived cells；
  3. 每个 context comparison 的 independent-donor support。
- `[ ]` support matrix 中的 label 从临时 `organ_proxy` 升级为 source-provided cell type 或预先冻结的 coarse state；若没有可靠 label，明确标为不支持。
- `[ ]` 为每个 sample 冻结可支持的 protocol：
  in-domain、cross-sample、cross-patient、cross-platform、context-shift。
- `[ ]` 在 `smCC_main.tex`、四份 rebuttal、表格、标题/摘要和 Data Availability 中统一所有计数与术语。

**完成门槛**

- `[ ]` 三张表中的总数可以从 canonical manifest 无歧义复算。
- `[ ]` 所有 cross-patient/context claims 都能追溯到独立 donor IDs；否则降级为 cross-sample 或 sample/context-confounded shift。
- `[ ]` P0 表格已写入对应 rebuttal 段落并标注来源。

---

## P1A — Visium HD bin-to-cell boundary / segmentation sensitivity

**Reviewer mapping**

- vCPF-1：boundary-bin assignment 与 segmentation error。
- 7Gar-2：derived profiles 缺少验证。
- kUmZ-1、kUmZ-4、kUmZ-9：single-cell target validity、alignment、limitations。

**冻结设计**

- Samples：lung `k`、lung `d`、ovary `ad`。
- Default：CellViT polygon 与 closed 2 µm bin square 相交；冲突按最近 cell centroid。
- Variants：
  1. default；
  2. `strict_interior`：丢弃 boundary-touching bins；
  3. `erode1`：mask erosion 一个 native 2 µm grid unit；
  4. `dilate1`：mask dilation 一个 native 2 µm grid unit，冲突按冻结规则解决。

**当前证据**

- `rebuttal_experiments/p1a_visium_boundary/experiment_manifest.json`
- `rebuttal_experiments/p1a_visium_boundary/slurm_array_manifest.tsv`
- Slurm job `257454`

**待清空**

- `[x]` `k/strict_interior` aggregation 完成。
- `[x]` `ad/strict_interior` aggregation 完成。
- `[!]` 修正 scratch output root；不得继续写无权限的 `/lv_scratch`。
- `[ ]` 重跑 `d/strict_interior`。
- `[ ]` 完成 `k,d,ad × erode1`。
- `[ ]` 完成 `k,d,ad × dilate1`。
- `[ ]` 为每个 sample/variant 报告 retained cells、assigned bins、UMI、detected genes。
- `[ ]` 在相同 cell 与 gene 交集上计算相对 default 的 per-cell Pearson/Spearman。
- `[ ]` 计算 HVG rank stability 和预定义 marker-gene rank stability；marker list 必须在看结果前冻结。
- `[ ]` 计算 coarse cell-state assignment concordance。
- `[ ]` 在固定 test cells、固定 checkpoint/gene panel 上重算 image-to-gene metrics；不得因 variant 改变 test cohort。
- `[ ]` 分 sample 报告结果，并给 macro summary；不做 cell-weighted pooling。
- `[ ]` 检查最敏感 sample/variant 的失败是否来自低 UMI、cell loss、mask conflict 或 tissue-specific morphology。

**完成门槛**

- `[ ]` 3 samples × 4 variants 全部可追溯，default 不被覆盖。
- `[ ]` 一张 robustness 主表 + 一张 marker/state stability 补充表完成。
- `[ ]` 结论区分“aggregation 对合理 boundary 变化稳定”与“derived target 等同真实 single-cell ground truth”；后者不得声称。
- `[ ]` 结果写入 vCPF-1、7Gar-2、kUmZ-1/4/9。

---

## P1B — Native Xenium biology-calibrated sample-held-out benchmark

**Reviewer mapping**

- aSvP-1、aSvP-2：科学价值、heterogeneity。
- vCPF-3、vCPF-4：低 correlation 的生物解释、simple baselines、spatial autocorrelation。
- 7Gar-1、7Gar-2：resolution experiment 的意义、native/derived target validation。
- kUmZ-1、kUmZ-2：single-cell target 真实性与 breadth。

**冻结设计**

- 优先 organs：human lung、human ovary；若 P0 表明 ovary 缺独立 sample/donor support，则换 human breast。
- 当前 protocol：same-organ sample-held-out；在 donor IDs 未核实前不得称 patient-held-out。
- Encoders：ResNet50、UNI2-h、H-Optimus-0。
- Baselines：training-set mean、coordinate-only、metadata-only。
- Targets：common native panel、training-only HVGs、预定义 marker genes、预定义 gene programs、source cell type 或冻结的 coarse states。
- 汇总：held-out sample 的 Average、Worst、Gap；不按 cell 数加权。

**当前证据**

- `rebuttal_experiments/p1b_xenium_native/runs.tsv`
- `rebuttal_experiments/p1b_xenium_native/README.md`
- Slurm jobs：`257463`、重提 `257484`、summary `257486`
- 已有 within-sample random-cell supporting evidence：
  `rebuttal_experiments/p1b_xenium_native/existing_examples_rebuttal.md`
- 已有四样本 CPU baseline/CI outputs：
  `rebuttal_experiments/outputs/p1b_xenium_existing_reanalysis/`

**待清空**

- `[x]` lung `HLCD033 -> HLCX022` / UNI2-h fold 完成。
- `[~]` 其余 11 个 sample-held-out encoder folds 正在运行或排队。
- `[ ]` summary job 完成，并检查所有 fold 使用相同 H&E alignment、gene harmonization 和 target transform。
- `[ ]` 核对 inverse `*_he_imagealignment.csv` 映射；抽样可视化 cell centroid 与 H&E crop。
- `[ ]` training mean、coordinate-only、metadata-only 都只能使用 training-split information。
- `[ ]` 每 fold 报 all genes、training-only HVGs、预定义 markers、gene programs。
- `[ ]` 完成 source cell-type 或预定义 coarse-state evaluation；报告 balanced accuracy，而非只报 overall accuracy。
- `[ ]` 每 organ 报 Average、Worst sample、Gap。
- `[ ]` 明确区分已有 within-sample random-cell examples 与新的 sample-held-out evidence。
- `[ ]` 若 P0 提供 donor IDs，将 split 升级/验证为 patient-held-out；否则全文保持 sample-held-out。
- `[ ]` 检查 ovary support；不足时按预先规则换 breast，不得根据结果好坏换 organ。

**完成门槛**

- `[ ]` 至少两个 organs、每 organ 至少两个独立 held-out folds。
- `[ ]` 三个 encoders + 三个 simple baselines 都有完整结果或明确、可解释的失败记录。
- `[ ]` 主结论由 marker/program/state 与 Worst/Gap 支撑，而不是只挑选高相关基因。
- `[ ]` native Xenium 与 derived Visium HD 结果分别成表。
- `[ ]` 结果写入四份 rebuttal，并替换相应红色实验占位。

---

## P2 — Eight-organ lightweight breadth benchmark

**Reviewer mapping**

- vCPF-2：compact summary across more organs/platforms。
- kUmZ-2：per-organ performance。
- aSvP-2：heterogeneity 的下游影响。
- 7Gar-1：scale/breadth experiment 的明确目的。

**当前最小设计**

| Organ | Pair | Species | Platform |
|---|---|---|---|
| Breast | `j -> m` | Human | Visium HD |
| Ovary | `ad -> ak` | Human | Visium HD |
| Lung | `k -> d` | Human | Visium HD |
| Pancreas | `g -> ag` | Human | Visium HD |
| Tonsil | `u -> i` | Human | Visium HD |
| Brain | `e -> ah` | Mouse | Visium HD |
| Embryo | `b -> ai` | Mouse | Visium HD |
| Kidney | `a -> aj` | Mouse | Visium HD |

**当前证据**

- `rebuttal_experiments/p2_hd_breadth/pair_manifest.tsv`
- `rebuttal_experiments/p2_hd_breadth/submit_hd_breadth.py`
- `rebuttal_experiments/p2_hd_breadth/summarize_hd_breadth.py`

**待清空**

- `[x]` eight-pair manifest、UNI2-h runner、training-mean summary 已准备。
- `[ ]` 正式提交 eight-organ UNI2-h array，并记录 job IDs。
- `[ ]` 每个 sample 固定、等量抽样；保存实际 train/test cell counts。
- `[ ]` 每 organ 一行，报告 organ-macro mean、median/IQR、worst organ。
- `[ ]` training-mean 的 gene-wise Pearson/Spearman 因 constant prediction 为 undefined，必须写 `NA`，不得写 0。
- `[ ]` 记录 gene intersection/panel size；不得在不同 organ 间比较未说明的不同 panel。
- `[ ]` 明确当前 P2 是 Visium HD-only；它不能单独支持“两平台 breadth”。
- `[ ]` 用 P1B 的 Xenium organ-macro 表作为独立 native-platform summary，或将 rebuttal claim 限定为“eight-organ Visium HD + two-organ native Xenium”。
- `[ ]` 资源允许时再扩展 ResNet50、CONCH、H-Optimus-0；最低交付不等待理想版本。

**完成门槛**

- `[ ]` 8/8 pairs 有结果或可解释的数据失败；不能静默删除失败 organ。
- `[ ]` 所有汇总为 organ-macro，无 cell-weighted pooled average。
- `[ ]` Xenium 与 Visium HD 分表。
- `[ ]` 结果写入 vCPF-2、kUmZ-2、aSvP-2、7Gar-1。

---

## P3 — Native Xenium pseudo-spot control

**Reviewer mapping**

- kUmZ-1、kUmZ-7、kUmZ-9：cell-specific target、spot comparison、spot averaging 隐藏的错误。
- 7Gar-1：resolution experiment 的生物意义。
- vCPF-3：低 correlation 的 biological calibration。

**依赖**

- `[!]` 等 P1B data/alignment/training pipeline 稳定后启动。

**待清空**

- `[ ]` 预先选择 native Xenium lung；若数据/QC 不满足，再按 P0 支持选择 breast。
- `[ ]` 冻结 pseudo-spot 空间邻域规则、spot size、最小 cell 数和 train/test split。
- `[ ]` 聚合真实 native cells 得到 pseudo-spot expression 和 spot-centered image。
- `[ ]` 用相同 encoder/head/gene targets 分别训练 spot-supervised 与 native-cell-supervised predictor。
- `[ ]` 报告 within-pseudo-spot true variance 与 recovered variance。
- `[ ]` 报告同一 pseudo-spot 内不同 cell types 的 marker separation。
- `[ ]` 报告 within-spot cell-pair expression ordering。
- `[ ]` 报告 prediction-derived coarse cell-state balanced accuracy。
- `[ ]` 量化 spot predictor 是否将邻近异质 cells 预测得过度相似。
- `[ ]` 不把“spot correlation 更高/更低”作为核心成功标准；averaging 本身会让 spot task 更容易。

**完成门槛**

- `[ ]` 结果直接证明或否定“cell-level evaluation exposes errors hidden by spot averaging”。
- `[ ]` 所有结论限于 native Xenium，不外推为 Visium HD derived target validation。
- `[ ]` 结果写入 kUmZ-7，并在 7Gar-1/vCPF-3 交叉引用。

---

## P4 — Support-calibrated foundation-model context-bias audit

**Reviewer mapping**

- aSvP-2：heterogeneity。
- vCPF-5：context analysis 过窄。
- 7Gar-1：context experiment 的意义。
- kUmZ-3、kUmZ-6：age case study 与 demographic coverage。

**硬性 gate**

- `[!]` P0 必须先确认同一 label 在多个 context 中有足够 independent-donor support。
- `[!]` 若 age 与 sample/donor 完全嵌套，只能称 sample/age-confounded shift。

**待清空**

- `[ ]` 先审计 technical context：study、sample、platform、processing。
- `[ ]` biological context（disease、age、sex）只有 support 合格时进入主结论。
- `[ ]` 模型：ResNet50、CONCH、UNI2-h、H-Optimus-0。
- `[ ]` 优先任务：native Xenium cell-state/cell-type、marker-program、histology-to-gene stratified performance。
- `[ ]` 每项报告 `Average / Worst / Gap / Support / Probe`。
- `[ ]` donor-held-out context probe。
- `[ ]` within-cell-type context probe。
- `[ ]` context shuffle。
- `[ ]` cell-type composition matching。
- `[ ]` donor bootstrap。
- `[ ]` low-level image/QC baseline。
- `[ ]` 将 context recoverability 与 harmful performance collapse 分开：probe 可恢复 context 不能单独证明 bias。
- `[ ]` 重新评估 AK/AD/AL；若缺多个 independent donors，则只保留为 sample/age-confounded case study。

**完成门槛**

- `[ ]` 主结果必须是 support-calibrated worst-context collapse，不是单独的 context probe accuracy。
- `[ ]` 每个 context comparison 都附 donor/sample support。
- `[ ]` 结果写入 aSvP-2、vCPF-5、7Gar-1、kUmZ-3/6；不支持的比较改为 limitation。

---

## P5 — Caption factuality audit

**Reviewer mapping**

- 7Gar-2：LMM-generated textual records 缺少 factuality/human evaluation。
- vCPF-6：direct measurement 与 generated artifact 边界。

**待清空**

- `[ ]` 从最终 release manifest 随机抽取 100--200 records，按 organ 与 platform 分层。
- `[ ]` 冻结抽样 seed 和 record IDs。
- `[ ]` 对 organ/platform/disease 等结构化事实做 metadata exact match。
- `[ ]` 标注 caption 是否包含 metadata 中不存在的事实。
- `[ ]` 两位作者独立评估 morphology statements：
  `supported / unsupported / uncertain`。
- `[ ]` 报告错误率、percent agreement 和 inter-rater agreement。
- `[ ]` 展示 3--5 个完整 record examples，包含 measured / processed / generated provenance。
- `[ ]` 若 factuality 不足，不重训 caption model；将 captions 降级为 optional derived artifacts，并明确不参与主 benchmark。

**完成门槛**

- `[ ]` 抽样、标注 rubric、双人结果和 examples 可公开核验。
- `[ ]` 结果写入 7Gar-2、vCPF-6，并同步 field-level provenance manifest。

---

## P6 — Final uncertainty gate

**原则**

- 只补 rebuttal 最关键的三张表：
  1. P1A boundary/segmentation robustness；
  2. P1B native Xenium biology-calibrated benchmark；
  3. P2 eight-organ breadth benchmark。

**待清空**

- `[ ]` P1A：以 sample 为主要 unit；若补 spatial in-domain metric，使用 spatial-block bootstrap。
- `[ ]` P1B：以 held-out sample/donor 为 unit，报告 Average、Worst、Gap 与 CI。
- `[ ]` P2：以 organ/sample 为 unit做 macro summary 与 bootstrap；不对 cells 独立 bootstrap。
- `[ ]` 对关键结论报告 effect size 和 CI，不只报 p-value。
- `[ ]` 只有计算成本允许且单位合理时跑 3 seeds；否则固定模型后做 donor/sample bootstrap。
- `[ ]` 在 rebuttal 表注中写明 uncertainty unit、replicate 数和 CI 方法。

**完成门槛**

- `[ ]` 三张关键表都有与设计匹配的 uncertainty。
- `[ ]` 不使用 cell-level pseudo-replication 夸大显著性。

---

## 不做 / 暂缓

- `[x]` 不训练完整 25-organ STBoost。
- `[x]` 不设计新 architecture。
- `[x]` 不扩展未经证据支持的 scaling-law claim。
- `[x]` 不在 P0 support matrix 完成前启动 context-bias 主实验。
- `[x]` 不在 P1B pipeline 稳定前启动 P3。
- `[x]` 不用 cell-weighted pooled average。
- `[x]` 不把 Visium HD derived targets 表述为直接 single-cell ground truth。

## 每日清空模板

每次更新只填写下面五项，并同步修改上方 checkbox：

```text
Date:
Item:
Status change:
Evidence / output path:
Next blocking action:
Rebuttal location updated:
```

## 最终提交前总 gate

- `[ ]` P0：所有 headline counts、study/sample/donor definitions 与 2 µm pipeline 全文一致。
- `[ ]` P1A：derived Visium HD target robustness 有直接实验。
- `[ ]` P1B：native Xenium sample/donor-held-out biological signal 有直接实验。
- `[ ]` P2：breadth claim 有 organ-macro、platform-separated evidence。
- `[ ]` P3：若完成，用于 resolution claim；若未完成，删除“exposes errors hidden by spot averaging”的强结论。
- `[ ]` P4：只有 support 合格的 context 进入主结论；其余降级为 confounded case study。
- `[ ]` P5：captions 有 human factuality audit，或明确降级为 optional unvalidated derived artifacts。
- `[ ]` P6：三张主表均有正确 uncertainty unit。
- `[ ]` 四份 rebuttal 中所有红色实验占位已被真实结果替换或改为明确 limitation。
- `[ ]` 每个结果都有输出路径、协议、样本量、指标定义和 manuscript/rebuttal location。
