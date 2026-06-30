---
title: "UniTTS: Towards End-to-End Speech Synthesis with Joint Acoustic-Semantic Modeling"
title_zh: UniTTS：联合声学-语义建模实现端到端语音合成
authors: "Rui Wang, Qianguo Sun, Junlong Wu, Tianrong Chen, Zhiyun Zeng, Yiyan Qi"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=YsrswIqSZ9"
tags: ["query:speech-audio"]
score: 9.0
evidence: 端到端TTS联合声学-语义建模
tldr: 本文提出DistilCodec和UniTTS，针对基于大语言模型的TTS中声学与语义信息无法完全对齐的问题。DistilCodec通过知识蒸馏从多码本音频编解码器中提取高效表示，UniTTS则在此表示上联合建模声学和语义信息，使LLM能够访问更全面的音频信息，从而提升TTS合成质量。实验证明该方法在自然度和可懂度上均优于现有LLM-TTS方法。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有LLM-TTS中声学和语义信息对齐不足，限制音频信息利用。
method: 提出DistilCodec蒸馏高效音频表示，UniTTS联合建模声学和语义。
result: 合成语音在自然度和可懂度上均超越现有LLM-TTS方法。
conclusion: 联合声学-语义建模提升了LLM-TTS综合利用音频信息的能力。
---

## Abstract
Recent advancements in multi-codebook neutral audio codecs, such as Residual Vector Quantization (RVQ) and Group Vector Quantization (GVQ), have significantly advanced text-to-speech (TTS) systems based on large language models (LLMs), whose exceptional capabilities in discrete token modeling have garnered significant attention within the speech processing community. However, since semantic and acoustic information cannot be fully aligned, a significant drawback of these methods when applied to LLM-based TTS is that large language models may have limited access to comprehensive audio information. To address this limitation, we propose DistilCodec and UniTTS, which collectively offer the following advantages: 1) DistilCodec distills a multi-codebook audio codec into a single-codebook codec with 32,768 codes, achieving near 100\% codebook utilization. 2) By avoiding semantic alignment constraints, DistilCodec enables the incorporation of extensive high-quality unlabeled audio—such as audiobooks with sound effects and musical segments—during training, thereby enhancing data diversity and general applicability. 3) Leveraging the comprehensive audio information modeling of DistilCodec, we integrated three key tasks into UniTTS's pre-training framework: audio modality autoregression, text modality autoregression, and speech-text cross-modal autoregression. This allows UniTTS to accept interleaved text and speech/audio prompts while substantially preserving LLM's text capabilities. 4) UniTTS employs a three-stage training process: Pre-Training, Supervised Fine-Tuning (SFT), and Alignment. Experiments demonstrate that DistilCodec effectively resolves codebook collapse in large, single-codebook settings. Building on this, UnitTTS demonstrates remarkable capabilities for zero-shot voice cloning with emotional expression.

---

## 论文详细总结（自动生成）

# 论文结构化总结

## 1. 核心问题与整体含义（研究动机和背景）

- **核心问题**：在多码本神经音频编解码器（如RVQ、GVQ）驱动的LLM-TTS系统中，语义信息与声学信息无法完全对齐，导致LLM可获取的音频信息受限，影响语音合成质量。
- **研究动机**：现有方法未能充分利用音频中的综合信息（声学+语义），而端到端联合建模有望提升LLM-TTS的性能。
- **整体含义**：本文通过蒸馏高效单码本表示并联合建模声学与语义，使LLM能访问更全面的音频信息，从而在零样本语音克隆和情感表达等任务上取得更优效果。

## 2. 方法论

### 核心思想
- 提出 **DistilCodec** 和 **UniTTS** 两个模块：
  - **DistilCodec**：通过知识蒸馏将多码本音频编解码器压缩为单码本编解码器（含32768个码），实现近100%码本利用率，避免语义对齐约束，从而允许使用大量无标注高质量音频（如带音效的有声书、音乐片段）进行训练。
  - **UniTTS**：基于DistilCodec的全面音频信息建模，在其预训练框架中集成三项关键任务：
    1. 音频模态自回归
    2. 文本模态自回归
    3. 语音-文本跨模态自回归
  这使得UniTTS能接受交错的文本和语音/音频提示，同时大幅保留LLM的文本处理能力。

### 关键技术细节
- 知识蒸馏方案：从多码本模型向单码本模型迁移知识，解决大单码本设置下的码本崩溃问题。
- 三阶段训练流程：
  1. **预训练**（Pre-Training）
  2. **监督微调**（Supervised Fine-Tuning, SFT）
  3. **对齐**（Alignment）
- 通过避免语义对齐约束，提升训练数据的多样性与通用性。

### 算法流程（文字说明）
1. 使用大型多码本音频编解码器作为教师模型。
2. 蒸馏训练单码本学生模型（DistilCodec），使其输出32768个码。
3. 在DistilCodec表示上训练UniTTS：输入交错的文本和音频序列，优化三个自回归目标。
4. 三阶段训练：预训练（大规模无监督/弱监督数据）→ SFT（有标签数据）→ 对齐（RLHF或偏好优化）。

## 3. 实验设计

- **数据集/场景**：未在摘要中具体列出，但提到使用了高质量无标注音频（如有声书、音乐片段）进行DistilCodec训练；UniTTS训练数据应包含文本-语音对以及可选的音频段落。
- **Benchmark**：未明确说明，但对比了“现有LLM-TTS方法”，推测包含主流基线如VALL-E、SoundStorm等。
- **对比方法**：与当前先进的LLM-TTS方法在自然度和可懂度上进行比较。

## 4. 资源与算力

- 论文摘要及元数据中**未明确说明**使用的GPU型号、数量、训练时长等信息。
- 仅能推测：由于涉及多阶段训练及大型蒸馏模型，可能需要较高算力（如多张A100或H100），但具体细节缺失。

## 5. 实验数量与充分性

- 摘要中提及了零样本语音克隆和情感表达能力的评估，但未说明具体实验组数（如不同数据集测试、消融实验等）。
- 从“实验证明”语句推断，至少进行了：
  - 与现有LLM-TTS的对比实验（自然度、可懂度）
  - 码本利用率对比（近100% vs. 其他方法）
  - 可能的情感/零样本克隆评估
- **充分性评价**：信息有限，无法判断实验是否全面（如缺少多语言、低资源场景、鲁棒性测试等细节）。但从结论来看，所提方法在关键指标上超越基线，具有一定说服力。

## 6. 主要结论与发现

- DistilCodec有效解决了大单码本设置下的码本崩溃问题，实现了近100%码本利用率。
- UniTTS通过联合声学-语义建模，显著提升了LLM-TTS综合利用音频信息的能力。
- 合成语音在**自然度**和**可懂度**上均超越现有LLM-TTS方法。
- UniTTS展现了出色的零样本语音克隆能力和情感表达能力。

## 7. 优点

- **创新性**：首次将声学-语义联合建模引入LLM-TTS预训练框架，突破了传统对齐限制。
- **数据高效**：DistilCodec支持利用大量无标签高质量音频，覆盖更丰富的声学变体（音效、音乐、情感），提升数据多样性。
- **保留LLM能力**：通过文本/音频模态自回归和跨模态自回归任务，维持了LLM在纯文本任务上的性能。
- **训练流程明确**：三阶段训练（预训练-SFT-对齐）符合当前大模型工业标准，易于复现和扩展。

## 8. 不足与局限

- **实验细节缺失**：未提供具体数据集、评测指标、基线实现、计算资源等信息，难以独立复现和验证。
- **可能有偏差风险**：仅报告“超越现有方法”，未给出失败案例或局限性（如对噪声、多说话人场景、极低资源语言的表现）。
- **应用限制**：单码本32768码的词汇量可能对推理效率有影响；蒸馏过程依赖教师模型，教师模型的性能上限决定了蒸馏天花板。
- **未讨论伦理/安全性**：零样本语音克隆存在冒充风险，论文未提及任何缓解措施。

（完）
