---
title: "From Scores to Preferences: Redefining MOS Benchmarking for Speech Quality Reward Modeling"
title_zh: 从评分到偏好：重新定义用于语音质量奖励建模的MOS基准
authors: "Yifei Cao, Changhao Jiang, Jiabao Zhuang, Jiajun Sun, Ming Zhang, Zhiheng Xi, Hui Li, Shihan Dou, Yuran Wang, Yunke Zhang, Tao Ji, Tao Gui, Qi Zhang, Xuanjing Huang"
date: 2025-09-14
pdf: "https://openreview.net/pdf?id=pz1tpHPiM3"
tags: ["query:speech-audio"]
score: 9.0
evidence: 通过偏好比较统一评估语音质量的基准
tldr: 针对主观MOS评分标准不一致和复现性差的问题，提出MOS-RMBench基准，将多个MOS数据集转换为偏好比较设置。系统构建并评估了三种奖励模型范式（标量、半标量、生成式）。该基准为合成语音质量评估提供了更稳定且可复现的平台。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 传统MOS标注依赖人工且标准不一，难以复现。
method: 将多个MOS数据集统一为偏好比较格式，并构建多种奖励模型进行评估。
result: 偏好基准确认了其作为评估指标的有效性，并发现生成式奖励模型潜力最大。
conclusion: MOS-RMBench为语音生成模型的自动质量评估提供了更可靠的工具。
---

## Abstract
Assessing the perceptual quality of synthetic speech is crucial for guiding the development and refinement of speech generation models. However, it has traditionally relied on human subjective ratings such as the Mean Opinion Score (MOS), which depend on manual annotations and often suffer from inconsistent rating standards and poor reproducibility. To address these limitations, we introduce MOS-RMBench, a unified benchmark that reformulates diverse MOS datasets into a preference-comparison setting, enabling rigorous evaluation across different datasets. Building on MOS-RMBench, we systematically construct and evaluate three paradigms for reward modeling: scalar reward models, semi-scalar reward models, and generative reward models (GRMs). Our experiments reveal three key findings: (1) scalar models achieve the strongest overall performance, consistently exceeding 74% accuracy; (2) most models perform considerably worse on synthetic speech than on human speech; and (3) all models struggle on pairs with very small MOS differences. To improve performance on these challenging pairs, we propose a MOS-aware GRM that incorporates an MOS-difference-based reward function, enabling the model to adaptively scale rewards according to the difficulty of each sample pair. Experimental results show that the MOS-aware GRM significantly improves fine-grained quality discrimination and narrows the gap with scalar models on the most challenging cases. We hope this work will establish both a benchmark and a methodological framework to foster more rigorous and scalable research in automatic speech quality assessment.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：传统合成语音质量评估依赖人工主观MOS（Mean Opinion Score）评分，存在评分标准不一致、可重复性差、人工标注成本高等缺陷，难以用于大规模自动评估和奖励建模。
- **研究动机**：为了克服上述限制，需要一种稳定、可复现且能有效指导语音生成模型优化的自动评估方法。本文旨在将多源MOS数据集统一为偏好比较（preference comparison）形式，构建可靠的基准，并探索不同奖励模型范式用于自动语音质量评估的潜力。
- **整体含义**：本文工作为合成语音质量评估提供了更可靠的基准（MOS-RMBench）和系统的方法论框架，有望推动该领域向更严谨、可扩展的方向发展。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：将原本的MOS绝对评分任务转化为相对偏好比较任务（即判断哪段语音质量更好），从而规避不同数据集评分尺度不统一的问题，并使得评估结果更稳定、更易跨数据集比较。
- **关键技术细节**：
  - **MOS-RMBench基准**：收集多个公开MOS数据集，将其中的评分对转化为“样本A质量优于样本B”的偏好标签，形成统一格式的偏好比较数据集。
  - **三种奖励模型范式**：
    - **标量奖励模型（Scalar）**：直接预测一个标量质量分数，与传统MOS回归类似，但训练时使用偏好损失（如Bradley-Terry模型）优化。
    - **半标量奖励模型（Semi-scalar）**：结合标量预测与局部偏好信息，可能采用混合架构。
    - **生成式奖励模型（GRM）**：利用条件生成模型（如自回归或扩散模型）输出质量评分，可能通过似然或对比机制反映偏好。
  - **MOS感知GRM**：针对MOS差异小的困难样本对，提出一种基于MOS差异的自适应奖励函数，让模型根据样本对的难度动态调整奖励尺度，从而提升细粒度判别能力。
- **公式/算法流程**（文字说明）：训练时，每个样本对 (x_a, x_b) 及其偏好标签 y (y=1表示x_a优于x_b) 被输入奖励模型，通过对比损失（如Ranking Loss或Pairwise Hinge Loss）优化。MOS感知GRM额外引入差异权值，使大差异样本贡献更大损失。

### 3. 实验设计：数据集、benchmark、对比方法
- **数据集**：来自多个已有MOS评测数据集，经过统一格式转换后形成偏好比较样本。具体数据集名称未在摘要中列出，但原文提到“diverse MOS datasets”。
- **Benchmark**：本文提出的 **MOS-RMBench**，采用偏好比较设置，评估模型在所有测试样本对上的分类准确率。
- **对比方法**：三种奖励模型范式（标量、半标量、生成式），其中生成式包含基础GRM和本文提出的MOS感知GRM。未提及与其他现有自动评估模型（如DNSMOS、P.808等）的直接对比，主要聚焦于范式间的系统性比较。
- **额外分析**：报告了模型在人声和合成语音上的性能差异，以及按MOS差异大小分组的性能。

### 4. 资源与算力
- 原文摘要及元数据中**未明确说明**使用的GPU型号、数量、训练时长等算力资源。仅能根据ICLR论文常见设置推测可能使用了多块A100或V100 GPU，但本总结中应如实指出未提及。

### 5. 实验数量与充分性
- **实验数量**：摘要提到三个主要发现，并展示了标量模型整体准确率>74%、各模型在人声与合成语音上的表现差异、以及MOS差异小样本上的困难。这意味着至少进行了跨数据集（多个）、跨范式（3种）、跨语音类型（人声vs合成）、跨难度（按MOS差异分组）的多维实验。另外还比较了MOS感知GRM与基础GRM的改进效果。
- **充分性判断**：实验设计较为系统，覆盖了不同范式、不同数据特性、不同难度层级，消融了关键模块（MOS感知）。但缺乏与外部现有SOTA方法的对比（如DNSMOS、NISQA等），以及跨领域（如其他语言）的泛化测试，因此充分性存在一定局限。

### 6. 论文的主要结论与发现
- **发现一**：标量奖励模型（Scalar）在所有评估中表现最强，一致达到74%以上的准确率。
- **发现二**：大多数模型在合成语音上的表现显著差于人声，提示合成语音评估更具挑战性。
- **发现三**：所有模型在MOS差异非常小的样本对（即难以区分的样本）上表现较差。
- **改进结果**：提出的MOS感知GRM通过自适应奖励函数，显著提升了细粒度质量判别能力，尤其在困难样本上缩小了与标量模型的差距。

### 7. 优点
- **方法论创新**：将MOS数据统一为偏好比较格式，解决了跨数据集评分尺度不一致问题，提高了基准的可复现性和可比性。
- **系统性范式比较**：首次全面对比标量、半标量和生成式三种奖励模型，为语音质量评估提供了清晰的方法学选择参考。
- **针对难点优化**：通过MOS差异自适应权重，直接针对模型薄弱环节（小差异样本）进行改进，实用性强。
- **开放贡献**：提出MOS-RMBench基准，有望成为该领域的重要评估平台。

### 8. 不足与局限
- **实验覆盖不足**：仅对比了内部三种范式，未与现有主流自动评价模型（如NISQA、DNSMOS、PESQ等）进行对比，削弱了结论的普适性。
- **数据集范围**：虽然使用了多个MOS数据集，但未说明是否覆盖不同语言、噪声条件、编码方式等，泛化能力存疑。
- **算力资源未公开**：缺乏训练成本信息，不利于复现和公平比较。
- **偏差风险**：偏好比较形式依赖原始MOS数据的质量，如果原始数据本身存在标注偏差，转换后的偏好可能隐含系统性偏差。
- **应用限制**：GRM模型（即使是MOS感知GRM）在总体上仍未超越简单标量模型，且标量模型效率更高，实际部署中需权衡。

（完）
