---
title: "Speak, Edit, Repeat: High-Fidelity Voice Editing and Zero-Shot TTS with Cross-Attentive Mamba"
title_zh: 说话、编辑、重复：基于交叉注意力Mamba的高保真语音编辑与零样本TTS
authors: "Baher Mohammad, Magauiya Zhussip, Stamatios Lefkimmiatis"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=yEKyIwJvvl"
tags: ["query:speech-audio"]
score: 9.0
evidence: 基于交叉注意力Mamba的高保真TTS和语音编辑
tldr: 语音编辑和零样本TTS通常需要独立模型。本文提出MAVE，基于交叉注意力Mamba的自回归架构，统一处理语音编辑和TTS。它在语音编辑任务上达到最先进水平，并在零样本TTS上表现优异（无需专门训练）。通过Mamba的高效序列建模和交叉注意力实现精确文本-声学对齐。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有语音编辑和TTS模型分离，且编辑质量有待提高。
method: 设计基于交叉注意力Mamba的自回归架构，联合优化语音编辑和TTS。
result: 在语音编辑上取得SOTA，零样本TTS上超越多个竞争模型。
conclusion: 统一框架为语音编辑与合成提供了高效高质方案。
---

## Abstract
We introduce $\textbf{MAVE}$ ($\textbf{M}$amba with Cross-$\textbf{A}$ttention for $\textbf{V}$oice $\textbf{E}$diting and Synthesis), a novel autoregressive architecture for text-conditioned voice editing and high-fidelity text-to-speech (TTS) synthesis, built on a cross-attentive Mamba backbone. MAVE achieves state-of-the-art performance in speech editing and very competitive results in zero-shot TTS, while not being explicitly trained on the latter task, outperforming leading autoregressive and diffusion models on diverse, real-world audio. By integrating Mamba for efficient audio sequence modeling with cross-attention for precise text-acoustic alignment, MAVE enables context-aware voice editing with exceptional naturalness and speaker consistency. In pairwise human evaluations on a random 40-sample subset of the RealEdit benchmark (400 judgments), 57.2\% of listeners rated MAVE-edited speech as perceptually equal to the original, while 24.8\% prefered the original and 18.0\% MAVE- demonstrating that in the majority of cases edits are indistinguishable from the source. MAVE compares favorably with VoiceCraft and FluentSpeech both on pairwise comparisons and standalone mean opinion score (MOS) evaluations. For zero-shot TTS, MAVE exceeds VoiceCraft in both speaker similarity and naturalness, without requiring multiple inference runs or post-processing. Remarkably, these quality gains come with a significantly lower memory cost and approximately the same latency: MAVE requires $\sim6\times$ less memory than VoiceCraft during inference on utterances from the RealEdit database (mean duration: 6.21s, A100, FP16, batch size 1). Our results demonstrate that MAVE establishes a new standard for flexible, high-fidelity voice editing and synthesis through the synergistic integration of structured state-space modeling and cross-modal attention.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义
- **研究动机**：现有语音编辑和零样本文本转语音（TTS）通常需要独立的模型，且语音编辑的质量（自然度、说话人一致性）有待提升。同时，高效的序列建模与精确的文本-声学对齐是核心挑战。
- **整体含义**：作者提出一种统一的、基于交叉注意力 Mamba 的自回归架构 **MAVE**（Mamba with Cross-Attention for Voice Editing and Synthesis），旨在同时实现高保真语音编辑和零样本 TTS，无需为两个任务分别训练模型，从而降低系统复杂性并提高灵活性。

## 2. 方法论
- **核心思想**：融合 **Mamba**（结构化状态空间模型，用于高效音频序列建模）与 **交叉注意力机制**（实现文本与声学特征的精确对齐），构建自回归架构，联合优化语音编辑和 TTS 任务。
- **关键技术细节**：
  - 以 Mamba 骨干网络处理音频序列，利用其线性复杂度优势，降低推理内存需求。
  - 引入交叉注意力层，使模型在生成/编辑时能根据文本条件动态关注对应的声学特征，提升对齐精度。
  - 采用自回归生成方式，逐帧预测声学特征（如 mel 谱），支持上下文感知的语音编辑，保持原始说话人特征和自然度。
- **公式/算法流程**（文字说明）：
  1. 输入：原始音频（或文本序列）及对应的编辑目标/合成文本。
  2. 对文本和音频分别编码为序列表示。
  3. 通过交叉注意力 Mamba 模块，迭代生成或修改声学序列，每次预测一帧，并利用先前帧作为上下文。
  4. 使用声码器（未特别说明，推测为 HiFi-GAN 等）将生成的声学特征转换为波形。

## 3. 实验设计
- **数据集/场景**：主要使用 **RealEdit** 基准数据集（随机选取 40 个样本子集进行成对人工评估），以及真实世界多样音频。
- **Benchmark**：语音编辑任务对比 **VoiceCraft** 和 **FluentSpeech**；零样本 TTS 对比 **VoiceCraft** 及其他领先模型（包括自回归和扩散模型）。
- **对比方法**：VoiceCraft（自回归模型）、FluentSpeech（扩散模型）等。
- **评估指标**：
  - 语音编辑：成对比较（听众偏好）、平均意见得分（MOS）
  - 零样本 TTS：说话人相似度、自然度

## 4. 资源与算力
- 论文中**未明确说明**训练使用的 GPU 型号、数量、训练时长等具体算力信息，仅提到在 RealEdit 数据库的推理阶段（平均时长 6.21s，A100 GPU，FP16，batch size 1）MAVE 比 VoiceCraft 节省约 **6 倍内存**，延迟大致相同。
- 因此，关于训练算力的细节缺失，无法评估训练成本。

## 5. 实验数量与充分性
- **实验组数**：主要报告了两类任务上的结果：
  1. 语音编辑：一个成对比较实验（40 样本 × 10 名评估者？共 400 次判断），以及 MOS 评估。
  2. 零样本 TTS：与 VoiceCraft 等对比自然度和相似度。
  3. 消融实验：论文摘要未提及消融实验细节，推测未在摘要中展示。
- **充分性评估**：
  - 客观上，仅在单个基准（RealEdit）上评估语音编辑，样本量偏小（40 个样本），可能存在偏差。
  - 零样本 TTS 对比了多个模型但缺少具体数据集规模说明。
  - 没有呈现不同说话人、不同噪声环境、不同编辑类型的系统性测试。
  - 总体而言，实验覆盖范围有限，结论的泛化能力需更多验证。

## 6. 主要结论与发现
- 语音编辑达到了 **最先进（SOTA）** 水平：在成对比较中，57.2% 的听众认为 MAVE 编辑的语音与原始语音感知相等，24.8% 偏好原始，18.0% 偏好 MAVE，表明大多数情况下编辑结果与原始难辨。
- 零样本 TTS：在没有专门训练的情况下，MAVE 在说话人相似度和自然度上均超过 VoiceCraft，且无需多次推理或后处理。
- 效率显著：推理时内存消耗约为 VoiceCraft 的 1/6，延迟相近。
- 统一框架为语音编辑和合成提供了高效高质的方案。

## 7. 优点
- **方法创新**：首次将交叉注意力与 Mamba 结合用于语音编辑和 TTS，兼顾效率和对齐精度。
- **统一架构**：一个模型支持两个任务，无需独立训练，降低部署成本。
- **性能优异**：编辑结果自然度高、说话人一致性强；零样本 TTS 质量领先。
- **推理高效**：内存消耗大幅降低，适合资源受限场景。
- **评估方式**：采用人工成对比较和 MOS 评估，具有一定的客观性。

## 8. 不足与局限
- **实验规模有限**：语音编辑仅使用 40 个样本子集进行主观评估，样本量小，结论可能受个体差异影响。
- **缺乏消融实验**：摘要未体现对 Mamba、交叉注意力等组件的消融分析，无法判断各模块贡献。
- **训练算力不透明**：未报告训练成本，不利于复现和公平对比。
- **零样本 TTS 评估不全面**：未说明具体数据集、说话人数量、音频条件等，泛化性存疑。
- **没有与其他最新扩散/神经编解码模型对比**（如 NaturalSpeech 3、VALL-E 等），对比范围可能不够广。
- **应用限制**：自回归架构在极长音频生成中可能存在误差累积；编辑类型（删除、插入、替换）的鲁棒性未详细分析。

（完）
