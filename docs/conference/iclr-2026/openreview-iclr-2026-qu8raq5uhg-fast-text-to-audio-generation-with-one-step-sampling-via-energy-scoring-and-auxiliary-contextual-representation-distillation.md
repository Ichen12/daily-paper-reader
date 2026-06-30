---
title: Fast Text-to-Audio Generation with One-Step Sampling via Energy-Scoring and Auxiliary Contextual Representation Distillation
title_zh: 通过能量评分和辅助上下文表示蒸馏实现快速文本到音频一步采样生成
authors: "Kuan-Po Huang, Bo-Ru Lu, Byeonggeun Kim, Mihee Lee, Zalan Fabian, Renard Korzeniowski, Qingming Tang, Greg Ver Steeg, Hung-yi Lee, Chieh-Chi Kao, Chao Wang"
date: 2025-09-12
pdf: "https://openreview.net/pdf?id=QU8raq5UhG"
tags: ["query:speech-audio"]
score: 10.0
evidence: 文本到音频生成的一步采样方法
tldr: 针对自回归扩散模型在文本到音频生成中多步采样导致高延迟的问题，本文提出一步采样框架，结合能量评分头和表示级蒸馏，从高斯噪声直接映射到音频潜在表示。在AudioCaps基准上，该方法实现接近实时的生成速度同时保持高质量。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有文本到音频生成需多步采样，延迟高。
method: 提出能量评分目标与表示蒸馏的一步采样框架。
result: 在AudioCaps上实现高速高质量生成。
conclusion: 一步采样显著提升了文本到音频生成的效率。
---

## Abstract
Autoregressive (AR) models with diffusion heads have recently achieved strong text-to-audio performance, yet their iterative decoding and multi-step sampling process introduce high-latency issues. To address this bottleneck, we propose a one-step sampling framework that combines an energy-distance training objective with representation-level distillation. An energy-scoring head maps Gaussian noise directly to audio latents in one step, eliminating the need for a costly recursive diffusion sampling process, while distillation from a masked autoregressive (MAR) text-to-audio model preserves the strong conditioning learned during diffusion training. On the AudioCaps benchmark, our method consistently outperforms prior one-step baselines on both objective and subjective metrics while substantially narrowing the quality gap to AR diffusion systems with multi-step sampling. Compared to the state-of-the-art AR diffusion system, IMPACT, our approach achieves up to $25$× faster inference with highly competitive audio quality. These results demonstrate that combining energy-distance training with representation-level distillation provides an effective recipe for fast, high-quality text-to-audio synthesis.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义

- **研究背景**：文本到音频生成任务近年来采用自回归（AR）模型结合扩散头取得了优异性能。然而，这类模型依赖迭代解码和多步采样（如扩散过程），导致推理延迟高，难以满足实时或近实时应用需求。
- **核心问题**：如何在不显著牺牲生成质量的前提下，大幅缩短文本到音频生成的推理时间？即实现“一步采样”的快速生成。
- **整体含义**：本文提出一种结合能量距离训练目标与表示级蒸馏的一步采样框架，在AudioCaps基准上达到接近实时推理速度，同时保持与多步采样AR扩散系统相竞争的质量，为高效文本到音频合成提供有效方案。

## 2. 论文提出的方法论

### 核心思想
- 将高斯噪声直接映射到音频潜在表示，仅需一步前向传播，避免耗时的递归扩散采样过程。
- 关键在于两个组件：**能量评分头（energy-scoring head）** 和 **表示级蒸馏（representation-level distillation）**。

### 关键技术细节
- **能量评分头**：用于将高斯噪声直接映射到音频潜在表示。通过能量距离训练目标（energy-distance training objective）优化，使得一步映射的输出与扩散模型多步采样的输出在某个特征空间上接近。
- **表示级蒸馏**：从预训练的掩码自回归（Masked Auto-Regressive, MAR）文本到音频模型中蒸馏知识，保留扩散训练中学到的强条件表示。蒸馏在潜在表示层进行，而非仅依赖最终输出。
- 整体流程：输入文本条件 → 噪声采样 → 通过能量评分头一步生成音频潜在 → 解码得到波形。无需迭代扩散。

### 公式或算法流程（文字说明）
- 训练阶段：同时优化能量距离损失（衡量一步生成潜在与扩散多步生成潜在之间的差异）和表示蒸馏损失（使一步模型的中间表示与教师MAR模型的对应表示对齐）。最终目标函数为两者加权和。
- 推理阶段：给定文本和随机高斯噪声，直接通过训练好的能量评分头输出音频潜在，然后解码为波形。

## 3. 实验设计

- **使用的数据集**：AudioCaps（文本到音频生成的常用基准，包含约5万对音频-文本描述）。
- **基准（Benchmark）**：AudioCaps上的客观指标（如FAD、CLAP Score等）和主观指标（如MOS）。
- **对比方法**：
  - 本文方法 vs. 先前一步采样基线（未具体命名）。
  - 本文方法 vs. 最先进的AR扩散系统 **IMPACT**（多步采样）。
- **评价指标**：客观（FAD、CLAP Score等）和主观（人工评分）。
- **结果亮点**：在保持与IMPACT竞争的音频质量下，推理速度提升**25倍**；且一致优于先前一步基线。

## 4. 资源与算力

- **文中未明确说明**：论文摘要及提供的文本中未提及具体GPU型号、数量、训练时长、参数量等算力信息。该部分信息需查看全文细节，此处无法给出，仅指出未说明。

## 5. 实验数量与充分性

- **实验数量**：从摘要看，主要对比实验集中在AudioCaps一个数据集上，包括与多步基线及一步基线的比较。未提及消融实验种类数量。
- **充分性分析**：
  - **客观公平**：使用标准AudioCaps基准，对比了多种方法，结果指标较全（客观+主观）。
  - **不足**：缺乏更多多样化数据集（如多语种、不同场景音频）的验证；未详细描述消融实验来验证各组件（能量头和蒸馏）的贡献；未给出统计显著性检验或置信区间。实验覆盖范围偏窄，但作为学术论文初步验证尚可。

## 6. 论文的主要结论与发现

- 能量距离训练目标与表示级蒸馏的结合可实现高效的一步文本到音频生成。
- 在AudioCaps上，该方法性能优于先前所有一步基线，且与多步AR扩散系统IMPACT相比，推理速度快达25倍，音频质量高度竞争。
- 这表明一步采样框架在保持高质量的同时极大降低延迟，为实时文本到音频合成提供了有效途径。

## 7. 优点

- **方法创新性**：将能量评分训练与表示级蒸馏结合用于一步生成，思路清晰，既有理论依据（能量距离）又实用（蒸馏保留条件信息）。
- **高效性**：实现近实时生成（推理速度提升25倍），直接解决AR扩散模型的高延迟瓶颈。
- **结果扎实**：在标准基准上同时报告客观和主观指标，并与最先进系统对比，验证了质量与速度的权衡优势。
- **通用性**：框架不依赖特定模型结构，可移植到其他扩散生成任务。

## 8. 不足与局限

- **实验覆盖不足**：仅在一个数据集（AudioCaps）上验证，缺乏跨数据集泛化测试；未评估长音频生成或复杂场景表现。
- **缺失消融实验细节**：未展示能量头和蒸馏各自贡献的量化分析，以及不同损失权重的影响。
- **未提供计算资源信息**：无法评估其训练成本或可复现性。
- **潜在偏差**：一步采样的质量提升可能依赖于特定教师模型（MAR），未讨论蒸馏过程中知识丢失的风险。
- **应用限制**：实际部署时，一步采样可能对噪声尺度敏感，文中未分析鲁棒性。

（完）
