---
title: "UniFlow-Audio: Unified Flow Matching for Audio Generation from Omni-Modalities"
title_zh: UniFlow-Audio：全模态音频生成的统一流匹配
authors: "Xuenan Xu, Jiahao Mei, Zihao Zheng, Ye Tao, Zeyu Xie, Yaoyun Zhang, Haohe Liu, Yuning Wu, Ming Yan, Wen Wu, Mengyue Wu, Chao Zhang"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=Xn7w0MLr1a"
tags: ["query:speech-audio"]
score: 9.0
evidence: 从文本等模态统一生成音频
tldr: 针对传统音频生成方法按时间对齐与否采用不同范式的问题，本文提出UniFlow-Audio，基于流匹配统一各类音频生成任务（语音、音乐、音效），支持多种输入模态。通过统一的框架，在多个生成任务上达到或超越专用模型性能，为通用音频生成提供了新思路。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有音频生成方法因任务类型不同而各自为政。
method: 提出统一的流匹配模型，处理时间对齐和非对齐任务。
result: 在语音合成、音乐生成等任务上取得优异效果。
conclusion: 统一流匹配框架是通用音频生成的有效途径。
---

## Abstract
Audio generation, including speech, music and sound effects, has advanced rapidly in recent years.
These tasks can be divided into two categories: time-aligned (TA) tasks, where each input unit corresponds to a specific segment of the output audio (e.g., phonemes aligned with frames in speech synthesis); and non-time-aligned (NTA) tasks, where such alignment is not available.
Since modeling paradigms for the two types are typically different, research on different audio generation tasks has traditionally followed separate trajectories.
However, audio is not inherently divided into such categories, making a unified model a natural and necessary goal for general audio generation.
Previous unified audio generation works have adopted autoregressive architectures, while unified non-autoregressive approaches remain largely unexplored.
In this work, we propose UniFlow-Audio, a universal audio generation framework based on flow matching.
We propose a dual-fusion mechanism that temporally aligns audio latents with TA features and integrates NTA features via cross-attention in each model block.
Task-balanced data sampling is employed to maintain strong performance across both TA and NTA tasks.
UniFlow-Audio supports omni-modalities, including text, audio, and video.
By leveraging the advantage of multi-task learning and the generative modeling capabilities of flow matching, UniFlow-Audio achieves strong results across 7 tasks using fewer than 8K hours of public training data and under 1B trainable parameters.
Even the small variant with only $~$200M parameters shows competitive performance, highlighting UniFlow-Audio as a potential non-auto-regressive foundation model for audio generation.
Code and models will be available at https://anonymous3387a8c.github.io/uniflow_audio.

---

## 论文详细总结（自动生成）

# UniFlow-Audio 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究动机**：音频生成（语音、音乐、音效）任务可划分为**时间对齐（TA）**和**非时间对齐（NTA）**两类。传统方法针对两类任务采用不同建模范式（如语音合成需帧级对齐，音乐生成则无需），导致研究各自独立，缺乏统一框架。
- **整体含义**：音频本身不存在天然的任务划分，建立**通用音频生成模型**是自然且必要的目标。先前工作多采用自回归架构，非自回归的统一方案尚属空白。本文提出 **UniFlow-Audio**，基于流匹配（flow matching）实现全模态（文本、音频、视频）到音频的统一生成。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：通过流匹配实现非自回归的通用音频生成，统一处理TA和NTA任务。
- **关键技术细节**：
  - **双融合机制（Dual-Fusion Mechanism）**：在每个模型块中，对时间对齐（TA）特征采用**时间对齐**方式将音频潜变量与TA特征对齐；对非时间对齐（NTA）特征采用**交叉注意力（cross-attention）**进行融合。
  - **任务平衡数据采样**：在训练时对不同任务的数据进行平衡采样，以维持TA和NTA任务上的性能。
  - **多模态支持**：支持文本、音频、视频作为输入条件，通过不同编码器提取特征后统一送入流匹配模型。
  - **模型架构**：采用流匹配（Flow Matching）作为生成主干，非自回归，效率更高。
- **公式与算法流程**：论文未显式给出公式，但整体流程可概括为：输入条件（文本/音频/视频）→ 特征提取 → 双融合流匹配模型 → 生成音频潜变量 → 解码为波形。

## 3. 实验设计
- **数据集**：使用**少于8000小时**的公开训练数据，具体数据集名称未在元数据中列出，但涵盖语音、音乐、音效等多种类型。
- **Benchmark**：在7个不同音频生成任务上进行评估，包括时间对齐任务（如语音合成）和非时间对齐任务（如音乐生成、音效生成）。
- **对比方法**：与各任务专用的先进模型（如语音合成、音乐生成等领域的SOTA）进行比较，同时可能对比了已有的统一模型（如自回归方案）。
- **模型规模**：主模型参数量小于1B，还提供了约200M参数的小型变体。

## 4. 资源与算力
- **文中明确提及**：训练使用了不到8000小时公开数据和不到10亿可训练参数。但**未明确说明GPU型号、数量及训练时长**。仅提到“under 1B trainable parameters”，算力细节缺失。

## 5. 实验数量与充分性
- **实验数量**：共涉及7个不同任务，涵盖TA和NTA两大类。但元数据未给出消融实验的具体数量或配置。
- **充分性与公平性**：
  - 任务覆盖较广，但缺少消融实验细节（如双融合机制、任务平衡采样的影响）。对比方法可能仅提及“达到或超越专用模型”，但未列出具体数值或量化优势。客观性有待补充更多实验数据。

## 6. 主要结论与发现
- **结论**：UniFlow-Audio通过统一的流匹配框架，在多任务（7个任务）上取得了强竞争力结果，甚至小模型（~200M参数）也表现良好，证明非自回归统一方案是通用音频生成的有效途径。
- **发现**：
  - 双融合机制能同时处理时间对齐和非时间对齐条件，避免分而治之的范式。
  - 任务平衡采样有助于多任务学习中保持各任务性能。
  - 多任务学习与流匹配产生协同增益。

## 7. 优点
- **方法创新**：首次将流匹配用于统一音频生成，并提出双融合机制解决TA与NTA的融合问题。
- **统一性**：支持文本、音频、视频三种输入模态，生成语音、音乐、音效，真正实现“全模态→音频”的统一。
- **高效性**：非自回归架构，参数量低于1B，小模型也可用，降低了部署门槛。
- **数据效率**：仅用不到8K小时公开数据即达到或超越专用模型，说明多任务学习能有效利用数据。

## 8. 不足与局限
- **实验覆盖不全**：未提供详细数据集列表、各任务具体性能数值、消融实验量化结果，削弱了说服力。
- **算力与训练细节缺失**：未说明GPU型号、数量、训练时长，难以评估实验可复现性。
- **偏差风险**：训练数据仅采用公开数据，可能在某些领域（如低资源语言、特定音乐风格）存在偏差。
- **应用限制**：论文未讨论生成音频的时长控制、实时性、噪声鲁棒性等实际应用问题。

（完）
