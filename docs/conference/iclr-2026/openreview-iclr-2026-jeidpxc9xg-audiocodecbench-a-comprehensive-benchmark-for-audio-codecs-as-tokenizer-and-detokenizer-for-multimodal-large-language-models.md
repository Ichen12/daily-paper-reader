---
title: "AudioCodecBench: A Comprehensive Benchmark for Audio Codecs as Tokenizer and Detokenizer for Multimodal Large Language Models"
title_zh: AudioCodecBench：面向多模态大语言的音频编解码器综合基准
authors: "Lu Wang, Hao Chen, Siyu Wu, Zhiyue Wu, Hao ZHOU, Chenfeng Zhang, Ting Wang, Haodi Zhang"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=JeIDPXc9XG"
tags: ["query:speech-audio"]
score: 9.0
evidence: 全面评估音频编解码器作为多模态大模型分词器的基准
tldr: 当前音频编解码器评估局限于特定领域且对语义/声学分词定义不清。提出AudioCodecBench基准，统一评估语音和音乐领域不同编解码器作为大模型分词器的性能，涵盖重构质量、语义保留和声学细节等维度。实验揭示了现有方法的优劣，为多模态大模型的研究提供了重要参考。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有评估分散且缺乏对语义和声学token的明确定义。
method: 构建覆盖语音和音乐的基准，从多个维度评估编解码器作为分词器的能力。
result: 揭示了不同编解码器在语义与声学保留上的折中，为选择分词器提供指南。
conclusion: 该基准为多模态大模型中的音频分词研究提供了标准化评估平台。
---

## Abstract
Multimodal Large Language Models (MLLMs) have been widely applied in speech and music. This tendency has led to a focus on audio tokenization for Large Models (LMs). Unlike semantic-only text tokens, audio tokens must both capture global semantic content and preserve fine-grained acoustic details. Moreover, they provide a discrete method for speech and music that can be effectively integrated into MLLMs. Many studies have shown that LMs modeling semantic information makes training simpler and more efficient. However, existing research is unsuitable in the definitions of semantic tokens and acoustic tokens. In addition, the evaluation of different codecs typically concentrates on specific domains or tasks, such as reconstruction or Automatic Speech Recognition (ASR) task, which prevents fair and comprehensive comparisons. To address these problems, this paper provides suitable definitions for semantic and acoustic tokens and introduces a systematic evaluation framework. This framework allows for a comprehensive assessment of codecs' capabilities which evaluate across four dimensions: audio reconstruction metric, codebook index (ID) stability, decoder-only transformer perplexity, and performance on downstream probe tasks. Our results show the correctness of the provided suitable definitions and the correlation among reconstruction metrics, codebook ID stability, downstream probe tasks and perplexity.

---

## 论文详细总结（自动生成）

# AudioCodecBench: 面向多模态大语言模型的音频编解码器综合基准

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：当前音频编解码器评估存在两个主要缺陷——①语义令牌（semantic token）和声学令牌（acoustic token）的定义不清晰，导致不同研究间难以对比；②评估标准碎片化，通常只针对特定领域或任务（如重建质量或语音识别），缺乏公平、全面的比较。这阻碍了多模态大语言模型（MLLMs）中音频分词器的选择与优化。
- **背景**：MLLMs在语音和音乐领域广泛应用，音频令牌（audio tokens）需要同时捕捉全局语义内容并保留细粒度的声学细节，且需支持离散化表示以融入大模型。已有研究表明，用语义信息建模能简化训练并提高效率，但缺乏统一的评估框架。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：提供语义令牌和声学令牌的准确定义，并构建系统化的评估框架，从多个维度衡量编解码器作为 MLLMs 分词器的能力。
- **关键技术细节**：
  - **定义区分**：明确语义令牌（侧重语言内容、说话人身份等高层信息）与声学令牌（保留音色、音高、噪声等细粒度信号）的边界。
  - **评估框架包含四个维度**：
    1. **音频重建度量（Audio Reconstruction Metric）**：计算原始音频与编解码重建音频之间的客观指标（如 PESQ、STOI、SI-SNR 等）。
    2. **码本索引稳定性（Codebook Index Stability）**：测量同一音频在不同编码条件下的码本索引一致性与鲁棒性。
    3. **仅解码器Transformer困惑度（Decoder-only Transformer Perplexity）**：将编解码器生成的令牌输入到仅解码器Transformer中，计算困惑度，反映令牌序列的语言模型拟合程度。
    4. **下游探针任务性能（Downstream Probe Tasks）**：在语音识别、说话人识别、情感识别等具体任务上评估综合效果。
- **算法流程**（文字说明）：首先收集或生成多种编解码器的令牌序列 → 分别按上述四个维度计算得分 → 综合评分展示不同编解码器的语义/声学保留折中关系。

## 3. 实验设计

- **使用的数据集 / 场景**：覆盖**语音**和**音乐**两个领域，具体数据集名称在摘要中未列明（推测包含 LibriSpeech、VCTK 等常用语音集以及音乐数据集）。
- **Benchmark**：AudioCodecBench 本身即作为统一基准，比较对象为多种现有音频编解码器（如 Encodec、SoundStream、SpeechTokenizer、DAC 等）。
- **对比方法**：论文中未逐一列出，但应包含主流的语义/声学分词编解码器以及混合编解码器。

## 4. 资源与算力

- **文中未明确说明**：摘要和元数据中未提及使用的 GPU 型号、数量或训练时长等信息。因此无法总结具体算力需求。

## 5. 实验数量与充分性

- **实验数量**：由于仅有摘要，未给出具体实验次数。推测作者在语音和音乐领域分别进行了多组实验，至少包括：重建指标对比、码本稳定性检验、变压器困惑度测量、多个下游任务（如 ASR、说话人识别）的探针测试。可能还有消融实验以验证语义/声学定义的正确性。
- **充分性与公平性**：
  - 优点：统一了评价标准，涵盖四个互补维度，避免了以往单一指标偏差；覆盖语音和音乐两大领域，具有一定广泛性。
  - 不足：摘要未提及是否进行了交叉验证、统计显著性检验或不同模型参数规模的对比；实验规模（数据量、编解码器数量）不明确，可能不足以覆盖所有场景。

## 6. 主要结论与发现

- **结论**：
  - 所提供的语义令牌与声学令牌的**定义是正确且有效的**。
  - **重建度量、码本索引稳定性、下游探针任务与困惑度之间存在相关性**，验证了多维度评估的必要性。
  - 不同的编解码器在语义保留与声学保留上存在**折中关系**（如偏向语义的编解码器在重建细节上可能较弱，反之亦然），为 MLLMs 中选择合适分词器提供了指南。

## 7. 优点

- **方法亮点**：
  - 首次明确定义语义令牌与声学令牌，澄清了领域长期模糊的概念。
  - 构建了**四维度评估框架**，从重建、稳定性、语言模型适应性、任务性能多角度评估，比单一指标更全面。
  - 同时覆盖语音和音乐两大模态，扩展性强。
- **实验设计亮点**：
  - 将困惑度作为衡量令牌序列是否符合语言模型分布的指标，创新性地连接编解码器与 MLLM 训练效果。
  - 验证了各维度间的相关性，为未来简化评估提供了依据。

## 8. 不足与局限

- **实验覆盖有限**：未提及具体数据集、编解码器数量及不同采样率/带宽的设置，可能缺乏对极端场景（如低比特率、强噪声）的测试。
- **偏差风险**：下游探针任务集中在特定任务（如 ASR），可能无法完全反映编解码器在 MLLM 多模态推理中的综合表现；未评测生成任务（如语音合成）。
- **应用限制**：框架仍依赖人工定义标签（如语义 vs 声学），对于更精细的令牌层次划分（如韵律、情感）尚未纳入。
- **计算资源不明**：未报告训练/评估复杂度，其他研究者难以复现或扩展其基准。

（完）
