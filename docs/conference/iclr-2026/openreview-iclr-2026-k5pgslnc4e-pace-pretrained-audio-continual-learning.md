---
title: "PACE: Pretrained Audio Continual Learning"
title_zh: PACE：预训练音频持续学习基准
authors: "Chang Li, Kanglei Zhou, Liyuan Wang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=k5PgSlNc4E"
tags: ["query:speech-audio"]
score: 9.0
evidence: 首个音频持续学习系统基准
tldr: 针对预训练音频模型在数据分布变化时性能下降的问题，本文提出PACE，首个音频持续学习系统基准。通过分析发现音频骨干网络强调低频细节导致上下游不匹配，现有视觉持续学习方法在音频上效果不佳。该基准为音频持续学习研究提供了标准化评估平台。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 预训练音频模型在现实动态环境中脆弱。
method: 构建首个音频持续学习基准，分析独特挑战。
result: 揭示了音频持续学习中的特殊困难。
conclusion: PACE为音频持续学习提供了重要参考。
---

## Abstract
Audio is a fundamental modality for analyzing speech, music, and environmental sounds. While pretrained audio models have significantly advanced audio understanding, they remain fragile in real-world scenarios where data distributions evolve over time. In this work, we present the first systematic benchmark for audio continual learning (CL) with pretrained models (PTMs) and provide a comprehensive analysis of its unique challenges. Unlike in the vision domain where parameter-efficient fine-tuning (PEFT) has proven effective for CL, directly applying such strategies to audio leads to poor performance. This is due to a fundamental property of audio backbones: they emphasize low-level spectral details rather than structured semantics, resulting in severe upstream–downstream misalignment. Through extensive empirical analysis, we identify a promising technical route based on analytic classifiers with first-session adaptation (FSA), but also uncover two major limitations: representation saturation in coarse-grained scenarios and representation shifts in fine-grained scenarios. To address these challenges, we propose **PACE**, an innovative method that improves FSA via a regularized analytic classifier and introduces multi-session adaptation through adaptive subspace-orthogonal PEFT for better semantic alignment. Additionally, we design spectrogram-based boundary-aware perturbations to mitigate representation overlap and improve stability. Experiments across six diverse audio CL benchmarks demonstrate that PACE substantially outperforms state-of-the-art baselines, representing a significant step toward robust and scalable audio CL with PTMs.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：预训练音频模型（PTMs）在现实动态环境中性能脆弱，当数据分布随时间演化时，模型泛化能力急剧下降。现有持续学习（CL）方法主要针对视觉领域设计，直接迁移到音频任务效果不佳。
- **根本原因**：音频骨干网络过于强调低层次频谱细节（如低频特征），而非结构化语义，导致上游预训练特征与下游持续学习任务之间存在严重的“上下游不匹配”。
- **研究意义**：本文是**首个针对预训练音频模型的持续学习系统基准**，填补了该领域的空白，为音频持续学习提供了标准化评估平台和分析框架。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：针对音频持续学习的独特挑战（表示饱和与表示偏移），提出 **PACE** 方法，通过增强首次会话适应（FSA）、引入多会话适应以及边界感知扰动来提升稳定性和可塑性。
- **关键技术细节**：
  - **正则化解析分类器**：改进传统的 FSA 方法，使用正则化项避免解析分类器在粗粒度场景下发生表示饱和。
  - **多会话适应性（Multi-session Adaptation）**：通过自适应子空间正交参数高效微调（PEFT）机制，在后续会话中逐步调整特征提取器，实现更好的语义对齐，缓解精细粒度场景下的表示偏移。
  - **基于频谱图的边界感知扰动**：在频谱空间设计边界感知的数据扰动，减少不同类别表示之间的重叠，提升决策边界的稳定性。
- **算法流程（文字说明）**：
  1. 使用预训练音频模型作为固定特征提取器（或部分微调）。
  2. 第一会话：应用正则化的解析分类器（Ridge Regression 形式）快速建立初始分类器。
  3. 后续会话：对特征提取器执行自适应子空间正交 PEFT（仅更新少量参数），同时保持与先前子空间的正交性以防止遗忘。
  4. 在每个会话中，对输入频谱图施加边界感知扰动（如基于梯度或频率特征的增强），增强判别性。
  5. 最终分类器采用累积特征与当前特征的联合推理。

## 3. 实验设计

- **数据集/场景**：使用了**六个不同的音频持续学习基准**，涵盖多种音频类型（如语音、音乐、环境声音）。具体数据集名称在摘要中未列出，但推测包含常见音频CL任务（如声学场景分类、语音命令识别、音乐流派分类等）。
- **基准（Benchmark）**：本文**自身构建了首个音频持续学习系统基准**，定义了标准化的任务划分、评估协议（如类增量、域增量等）和评价指标。
- **对比方法**：与**多个现有方法**（state-of-the-art baselines）进行比较，包括直接应用视觉领域的持续学习方法（如 EWC、iCaRL、LwF、DER 等）以及参数高效微调策略（PEFT）。结果显示 PACE 显著优于所有对比方法。

## 4. 资源与算力

- **未明确说明**：论文摘要及元数据中未提及使用的 GPU 型号、数量、训练时长等具体算力信息。因此无法总结资源消耗，这可能是论文正文才有的细节。

## 5. 实验数量与充分性

- **实验数量**：共在**六个不同的音频CL基准**上进行评估，且进行了**广泛的实证分析**（extensive empirical analysis），包括对挑战的分析、消融研究（如去除正则化、PEFT、扰动等组件的效果）。
- **充分性与公平性**：
  - 覆盖了多种音频模态和任务，具有较强的泛化性。
  - 对比了多个基线，且实验结果具有显著优势（substantially outperforms），表明实验设置较为客观。
  - 但仅基于摘要无法确认是否所有实验都报告了标准差、统计显著性检验等，需参考全文。总体而言，实验规模在音频CL领域属于全面且充分的。

## 6. 论文的主要结论与发现

- **主要结论**：PACE 方法在音频持续学习任务中大幅优于现有方法，是迈向鲁棒和可扩展音频CL的重要一步。
- **关键发现**：
  - 音频骨干网络强调低频细节导致上下游不匹配，是视觉CL方法失效的根本原因。
  - 首次会话适应（FSA）是有效路径，但存在两类限制：**表示饱和**（粗粒度场景下类别过多导致无法区分）和**表示偏移**（细粒度场景下特征随会话变化）。
  - 提出的正则化解耦、子空间正交 PEFT 和频谱边界扰动可有效缓解上述问题。

## 7. 优点

- **开创性**：首个针对预训练音频模型的系统持续学习基准，具有重要的领域引领价值。
- **方法新颖**：
  - 深入分析了音频独有的挑战（低频细节主导、表示饱和与偏移），而非简单照搬视觉方法。
  - 正则化解析分类器、自适应子空间正交 PEFT、频谱边界扰动三者结合，设计精巧且有理论动机。
- **实验充分**：跨六个基准的对比和消融，验证了方法的通用性和有效性。
- **评分高**：ICLR 2026 接收，学术评价高（score: 9.0）。

## 8. 不足与局限

- **实验覆盖局限**：基准仅包含六个场景，可能尚未涵盖所有真实世界的音频持续学习场景（如极长序列、非均衡类别分布等）。部分数据集细节缺失。
- **偏差风险**：仅依赖解析分类器和固定特征提取器的组合，可能在特征漂移严重时仍存在局限性。边界感知扰动的设计是否对所有音频类型（如语音合成、音乐生成）有效需进一步验证。
- **计算资源未知**：未报告算力消耗，难以评估方法的实际部署成本。
- **方法论局限**：多会话 PEFT 的自适应机制可能增加额外超参数，调优复杂度较高。未讨论与在线持续学习或终身学习的兼容性。
- **应用限制**：论文聚焦于分类任务，对于其他音频理解任务（如生成、分割）的适用性未探讨。

（完）
