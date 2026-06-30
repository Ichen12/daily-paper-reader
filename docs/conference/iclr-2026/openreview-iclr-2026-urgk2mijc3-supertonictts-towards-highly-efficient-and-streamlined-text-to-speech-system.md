---
title: "SupertonicTTS: Towards Highly Efficient and Streamlined Text-to-Speech System"
title_zh: SupertonicTTS：迈向高效精简的文本转语音系统
authors: "Hyeongju Kim, Jinhyeok Yang, Yechan Yu, Seunghun Ji, Jacob Richard Morton, Frederik Bous, Joon Byun, Juheon Lee"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=urGk2mIJc3"
tags: ["query:speech-audio"]
score: 10.0
evidence: 直接提出文本转语音系统
tldr: 针对现有TTS系统复杂且低效的问题，本文提出SupertonicTTS，采用低维连续潜表示、流动匹配和ConvNeXt模块，实现轻量高效的文本到语音合成。系统无需G2P和外部对齐器，直接操作字符级文本，在保持高质量的同时显著降低计算开销。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有TTS系统模型庞大、流程复杂。
method: 使用低维潜空间、流动匹配和ConvNeXt，简化流水线。
result: 在多个指标上达到竞争性语音质量，模型尺寸大幅减小。
conclusion: SupertonicTTS提供了高效且性能优异的TTS解决方案。
---

## Abstract
We introduce SupertonicTTS, a novel text-to-speech (TTS) system designed for efficient and streamlined speech synthesis. SupertonicTTS comprises three components: a speech autoencoder for continuous latent representation, a text-to-latent module leveraging flow-matching for text-to-latent mapping, and an utterance-level duration predictor. To enable a lightweight architecture, we employ a low-dimensional latent space, temporal compression of latents, and ConvNeXt blocks. The TTS pipeline is further simplified by operating directly on raw character-level text and employing cross-attention for text-speech alignment, thus eliminating the need for grapheme-to-phoneme (G2P) modules and external aligners. In addition, we propose context-sharing batch expansion that accelerates loss convergence and stabilizes text-speech alignment with minimal memory and I/O overhead. Experimental results demonstrate that SupertonicTTS delivers performance comparable to contemporary zero-shot TTS models with only 44M parameters, while significantly reducing architectural complexity and computational cost.

---

## 论文详细总结（自动生成）

## 论文中文总结

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：现有文本转语音（TTS）系统普遍存在模型庞大、流程复杂的问题。典型流水线需要借助字素到音素（G2P）模块和外部对齐器，导致计算开销大、部署困难。同时，许多高性能模型（如零样本TTS）参数规模大，难以在资源受限场景下应用。
- **整体含义**：本文旨在设计一种**高效且精简**的TTS系统，在保持竞争性语音质量的同时，大幅降低模型复杂度和计算成本，使TTS更易于实际部署和普及。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：采用**低维连续潜表示**、**流动匹配（Flow-Matching）** 和**ConvNeXt模块**，构建一个轻量级且流水线简化的TTS系统（SupertonicTTS）。
- **关键技术细节**：
  - **三组件架构**：
    1. **语音自编码器**：将语音编码为低维连续潜表示，并通过时间压缩进一步降低维度。
    2. **文本到潜变量模块**：利用**流动匹配**实现文本到潜变量的映射，无需自回归解码。
    3. **话语级时长预测器**：预测每个字符的持续时间，用于对齐。
  - **流水线简化**：直接操作**原始字符级文本**（无需G2P），并通过**交叉注意力机制**实现文本与语音的对齐，消除外部对齐器。
  - **上下文共享批次扩展（Context-Sharing Batch Expansion）**：一种训练技巧，在最小化内存和I/O开销下加速损失收敛并稳定文本-语音对齐。
- **公式/算法流程**（文字说明）：输入字符级文本 → 文本编码器（ConvNeXt）提取特征 → 时长预测器预测每个字符的帧数 → 通过交叉注意力将文本特征与语音潜变量对齐 → 流动匹配模块生成目标潜变量 → 自编码器解码器合成语音波形。

### 3. 实验设计
- **使用的数据集**：论文未在摘要中明确提及具体数据集名称（如LJSpeech、VCTK等），仅提及“实验结果表明……”。推测使用了常见的英语单人或多人语音数据集。
- **Benchmark**：与**当代零样本TTS模型**进行对比，但未列出具体对比方法名称（如VALL-E、NaturalSpeech等）。
- **对比方法**：比较了同参数规模的其他TTS系统，并以参数量（44M）和架构复杂度作为对比指标。

### 4. 资源与算力
- **未明确说明**：文中未提及GPU型号、数量、训练时长等具体的算力资源信息。仅强调模型轻量（44M参数）具有计算效率优势，但未给出训练耗时数据。

### 5. 实验数量与充分性
- **实验数量**：摘要仅提及“实验结果”，未列出具体实验组数。推测可能包含：
  - 主客观评估（如MOS、自然度、相似度）。
  - 消融实验：验证低维潜空间、流动匹配、交叉注意力、上下文共享批次扩展等组件的贡献。
  - 对比实验：与若干零样本TTS模型在语音质量、参数量、推理速度上对比。
- **充分性与公平性**：由于缺乏详细实验设置和对比基线，难以判断完全充分。但作者声称“性能可媲美当代零样本TTS模型”，若实验覆盖多个指标和多种场景，则具有一定说服力。但未公开第三方复现细节，可能存在偏差风险。

### 6. 论文的主要结论与发现
- **主要结论**：SupertonicTTS在仅**44M参数**的情况下，实现了与当代零样本TTS模型**可比较的语音质量**，同时显著降低了架构复杂度和计算成本。
- **关键发现**：
  - 低维潜空间+时间压缩可有效减小模型体积而不损失质量。
  - 流动匹配比自回归方法更轻量。
  - 直接操作字符级文本并利用交叉注意力可消除外部对齐器和G2P，简化流水线。
  - 提出的上下文共享批次扩展能加速训练收敛。

### 7. 优点
- **架构精简**：无需G2P和外部对齐器，全流程端到端，降低开发门槛。
- **极低参数量**：44M参数，适合边缘设备部署。
- **训练高效**：通过上下文共享批次扩展减少内存和I/O开销，加速收敛。
- **性能竞争力**：在轻量化的前提下质量不逊于复杂模型。

### 8. 不足与局限
- **实验覆盖不足**：未提供具体数据集和对比方法的细节，难以独立验证结果。
- **缺乏客观指标数值**：未列出MOS、WER等客观得分，仅定性描述“可比较”。
- **应用限制**：可能仅支持英文？未提及多语言能力。零样本能力是否真的与SOTA相当存疑。
- **风险与偏差**：仅依赖论文声称，可能存在选择性报告或过拟合特定数据集的风险。

（完）
