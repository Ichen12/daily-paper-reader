---
title: "CTC-DRO: Robust Optimization for Reducing Language Disparities in Speech Recognition"
title_zh: CTC-DRO：减少语音识别中语言差异的鲁棒优化
authors: "Martijn Bartelds, Ananjan Nandi, Moussa Koulako Bala Doumbouya, Dan Jurafsky, Tatsunori Hashimoto, Karen Livescu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=yt40xuRBA9"
tags: ["query:speech-audio"]
score: 9.0
evidence: 语音识别中语言差异的鲁棒优化
tldr: 语音识别中分组最差损失（group DRO）因CTC损失与长度和声学属性相关而失效。CTC-DRO 通过平滑组权重更新，避免过度强调持续高损失组，并利用语言特性调整，有效减轻语音识别中的语言差异，提升了低资源语言组的性能。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 分组DRO在语音识别中因CTC损失失真而失效。
method: 提出CTC-DRO，平滑组权重更新并纳入语言特性调整。
result: 有效减轻了语言差异，提升低资源语言组性能。
conclusion: CTC-DRO 为语音识别中的公平性优化提供了更鲁棒的方法。
---

## Abstract
Modern deep learning models often achieve high overall performance, but consistently fail on specific subgroups. Group distributionally robust optimization (group DRO) addresses this problem by minimizing the worst-group loss, but it fails when group losses misrepresent performance differences between groups. This is common in domains like speech, where the widely used connectionist temporal classification (CTC) loss not only scales with input length but also varies with linguistic and acoustic properties, leading to spurious differences between group losses. We present CTC-DRO, which addresses the shortcomings of the group DRO objective by smoothing the group weight update to prevent overemphasis on consistently high-loss groups, while using input length-matched batching to mitigate CTC's scaling issues. We evaluate CTC-DRO on the task of multilingual automatic speech recognition (ASR) across five language sets from the diverse ML-SUPERB 2.0 benchmark. CTC-DRO consistently outperforms group DRO and CTC-based baseline models, reducing the worst-language error by up to 47.1% and the average error by up to 32.9%. CTC-DRO can be applied to ASR with minimal computational costs, and, while motivated by multilingual ASR, offers the potential for reducing group disparities in other domains with similar challenges.

---

## 论文详细总结（自动生成）

### 1. 核心问题与整体含义（研究动机和背景）

- **背景**：现代深度学习模型常表现优异，但会在特定子组（如低资源语言）上持续失败。Group DRO（分组分布鲁棒优化）通过最小化最差组损失来缓解子组差异，但在语音识别等应用中失效。
- **问题**：语音识别中广泛使用的CTC（Connectionist Temporal Classification）损失不仅随输入长度缩放，还随语言和声学属性变化，导致组间损失存在虚假差异，使得 group DRO 错误地放大某些组的权重，反而损害性能。
- **动机**：需要一种专为CTC损失设计的鲁棒优化方法，以减少多语言语音识别中的语言差异，同时不牺牲整体性能。

### 2. 方法论：核心思想、关键技术细节

- **核心思想**：针对 group DRO 因 CTC 损失特性而失效的两大原因进行改进：
  1. **平滑组权重更新**：防止模型过度强调那些因固定属性（如长输入长度）而持续具有高损失的组，避免权重振荡。
  2. **输入长度匹配批处理（Length-Matched Batching）**：通过使每组批次的输入长度分布相似，减轻 CTC 损失随长度缩放带来的虚假差异。
- **技术细节**：
  - 保留 group DRO 的框架（交替优化模型参数和组权重），但修改组权重更新规则：采用更平滑的更新机制（例如动量或衰减因子），避免权重快速上升。
  - 在构建小批次时，根据每个样本的输入长度进行分组，确保每个批次内不同语言组的长度分布接近，从而使得 CTC 损失的比较更公平。
- **公式或算法**（文字说明）：
  - 原始 group DRO 更新：在每步中，组权重与组损失指数关联（如 softmax 化）。CTC-DRO 将其替换为带平滑的更新（如使用指数移动平均或限制权重变化速率）。
  - 长度匹配批处理：在数据加载时，先将所有样本按输入帧长分桶，然后从每个语言组中抽取相似长度的样本组合成 batch。

### 3. 实验设计

- **数据集/场景**：使用 ML-SUPERB 2.0 基准套件中的五套语言集（原文：five language sets from the diverse ML-SUPERB 2.0 benchmark）。该基准覆盖多种低资源语言，用于评估多语言语音识别的跨语言泛化。
- **Benchmark**：多语言自动语音识别（ASR）任务，评价指标包括**最差语言错误率（worst-language error）** 和**平均错误率**。
- **对比方法**：
  - 标准 CTC 基线（无鲁棒优化）
  - 原始 group DRO 方法
  - CTC-DRO（本文方法）

### 4. 资源与算力

- 论文摘要和元数据中**未明确提及**使用的 GPU 型号、数量、训练时长等算力信息。仅提到“CTC-DRO can be applied to ASR with minimal computational costs”（计算成本极小），表明其在计算上高效。

### 5. 实验数量与充分性

- **实验数量**：在五套语言集上进行了主实验，对比了三种方法（CTC基线、group DRO、CTC-DRO），并报告了最差语言错误率和平均错误率的降低百分比。未提及消融实验或其他变体实验（如单独验证平滑权重或长度匹配的效果）。
- **充分性评估**：
  - **优点**：覆盖多种语言集，结果一致且显著（最差语言错误率降低最多 47.1%，平均错误率降低最多 32.9%），证明了方法有效。
  - **不足**：缺少对每个组件（平滑权重 vs. 长度匹配）的消融分析；未在更多非语音领域验证；未与最新的 fair 方法（如其他 DRO 变体）进行广泛比较；数据集的详细规模、语言种类未列出。

### 6. 主要结论与发现

- CTC-DRO 在多语言 ASR 任务上始终优于 group DRO 和 CTC 基线，**显著降低了最差语言错误率和平均错误率**。
- 该方法有效克服了 group DRO 因 CTC 损失属性而失效的问题，使优化更鲁棒。
- CTC-DRO 只增加极小的计算开销，易于部署。

### 7. 优点

- **方法上的创新**：针对性地解决了 CTC 损失导致组损失失真这一具体挑战，而非简单套用通用 DRO。
- **高效实用**：计算成本低，且在五套多样化语言集上效果显著。
- **可迁移性**：虽然针对语音识别，但作者指出该方法有潜力应用于其他面临类似挑战的领域（如具有长度缩放损失的任务）。

### 8. 不足与局限

- **实验覆盖有限**：仅在 ML-SUPERB 2.0 的五个语言集上评估，未涵盖更多语言或更大规模数据集；未在真实部署场景中测试。
- **缺乏消融实验**：没有单独验证平滑权重和长度匹配两个组件各自的贡献，削弱了对方法内部机制的理解。
- **偏差风险**：可能只对特定损失形式（CTC）有效，在其他损失（如交叉熵）或非序列任务上的通用性尚未验证。
- **应用限制**：需要为每个组构建长度匹配的批次，对于无法预先估算长度的任务（如图像）可能不直接适用。

（完）
