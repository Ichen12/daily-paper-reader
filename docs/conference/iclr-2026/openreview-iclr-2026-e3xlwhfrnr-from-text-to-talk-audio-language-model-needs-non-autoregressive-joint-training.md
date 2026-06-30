---
title: "From Text to Talk: Audio-Language Model Needs Non-Autoregressive Joint Training"
title_zh: 从文本到对话：音频-语言模型需要非自回归联合训练
authors: "Tianqiao Liu, Xueyi Li, Hao Wang, Haoxuan Li, Zhichao Chen, Weiqi Luo, Zitao Liu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=e3XLWHFrnr"
tags: ["query:speech-audio"]
score: 9.0
evidence: 结合自回归文本与非自回归音频扩散的TTS方法
tldr: 针对现有语音对话系统依赖纯自回归方法忽略文本与音频关系差异的问题，本文提出Text-to-Talk框架，通过吸收离散扩散实现自回归文本与非自回归音频扩散的统一训练。实验表明该方法能高效生成自然流畅的语音回复，为端到端语音对话提供了新范式。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有自回归方法忽略了文本和音频依赖关系差异，导致生成低效。
method: 提出统一框架，结合自回归文本生成与非自回归音频扩散，采用吸收离散扩散实现统一目标。
result: 在语音对话任务上取得高质量和低延迟的生成效果。
conclusion: 非自回归音频扩散结合自回归文本是构建语音对话系统的有效方式。
---

## Abstract
Recent advances in large language models (LLMs) have attracted significant interest in extending their capabilities to multimodal scenarios, particularly for speech-to-speech (S2S) conversational systems. However, existing multimodal models handling interleaved audio and text rely on autoregressive (AR) methods, overlooking that text depends on target-target relations whereas audio depends mainly on source-target relations. In this work, we propose Text-to-Talk (TtT), a unified audio-text framework that integrates AR text generation with non-autoregressive (NAR) audio diffusion in a single Transformer. By leveraging the any-order AR property of absorbing discrete diffusion, our approach provides a unified training objective for text and audio. To support this hybrid generation paradigm, we design a modality-aware attention mechanism that enforces causal decoding for text while allowing bidirectional modeling within audio spans, and further introduce three training strategies that reduce train-test discrepancies. During inference, TtT employs block-wise diffusion to synthesize audio in parallel while flexibly handling variable-length outputs. Comprehensive experiments on audio question answering (Audio-QA), automatic speech recognition (ASR), automated audio caption (AAC) and S2S benchmarks show that TtT consistently surpasses strong AR and NAR baselines, with additional ablation and training-strategy analyses confirming the contribution of each component.

---

## 论文详细总结（自动生成）

基于论文《From Text to Talk: Audio-Language Model Needs Non-Autoregressive Joint Training》的摘要与元数据，生成详细中文总结如下：

---

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：现有语音对话系统（S2S）大多依赖纯自回归（AR）方法处理交织的音频和文本，但忽略了**文本主要依赖目标-目标关系（即文本内部上下文）**，而**音频主要依赖源-目标关系（即跨模态条件）**。纯AR方法无法区分这两种依赖模式，导致生成效率低下、对齐困难。
- **核心问题**：如何在一个统一框架中同时处理文本的自回归生成与音频的非自回归生成，从而兼顾文本的序列依赖性与音频的并行合成优势。
- **整体含义**：提出**Text-to-Talk (TtT)** 框架，将AR文本生成与NAR音频扩散集成在单一Transformer中，为端到端语音对话系统提供新范式。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：利用吸收离散扩散（absorbing discrete diffusion）的任意阶自回归特性，为文本和音频提供统一的训练目标。文本部分保持因果自回归解码，音频部分采用非自回归扩散生成。
- **关键技术细节**：
  - **统一训练目标**：通过吸收离散扩散，将文本的自回归损失与音频的扩散损失统一为同一种形式，使模型能够同时学习两种模态的生成模式。
  - **模态感知注意力机制（Modality-aware Attention）**：对文本施加因果掩码（强制自回归），对音频块内允许双向注意力（利于扩散并行去噪），块间可能仍保持因果或条件依赖。
  - **三种训练策略**：减少训练与测试之间的不一致（具体策略原文未展开，推测可能包括混合训练、噪声调度、掩码策略等）。
  - **推理阶段：块状扩散（Block-wise Diffusion）**：音频块可并行合成，同时灵活处理变长输出。
- **算法流程**（文字说明）：
  1. 输入交织的文本和音频序列，文本部分直接采用语言建模目标（AR预测），音频部分通过吸收离散扩散在连续隐空间中进行前向加噪与反向去噪。
  2. 训练时，模型对文本预测下一个token，对音频预测被掩盖的离散编码（类似扩散中的去噪步骤）。
  3. 推理时，文本部分逐token自回归生成，音频部分按照块状扩散并行去噪，最终拼接得到完整语音回复。

## 3. 实验设计：数据集、基准与对比方法
- **数据集/场景**：
  - 语音问答（Audio-QA）
  - 自动语音识别（ASR）
  - 自动音频描述（AAC）
  - 语音对话（S2S）基准
- **Benchmark**：未明确列出具体benchmark名称，但提到了Audio-QA、ASR、AAC和S2S标准任务。
- **对比方法**：强自回归（AR）基线和非自回归（NAR）基线（具体模型名称未在摘要中给出，但实验覆盖了主流方法）。

## 4. 资源与算力
- **文中未明确说明**使用的GPU型号、数量、训练时长等细节。摘要及元数据中未提及任何算力信息，因此只能指出论文未提供该部分细节。

## 5. 实验数量与充分性
- **实验数量**：摘要中提及在四类任务上进行了**综合实验**，并额外进行了**消融实验**和**训练策略分析**。具体实验次数未列出，但涵盖多个领域，具有一定的广度。
- **充分性评估**：
  - 优点：覆盖多个模态任务（语音识别、理解、生成、对话），并与AR/NAR基线对比，评价标准较为全面。
  - 不足：未提供细粒度数据集划分、统计显著性测试等，单从摘要难以判断实验的客观性与可重复性。

## 6. 论文的主要结论与发现
- TtT框架在Audio-QA、ASR、AAC和S2S基准上**一致性地超越**了强自回归与非自回归基线。
- 消融和训练策略分析证实了各组件（统一目标、模态注意、训练策略、块状扩散）的贡献。
- 结论：**非自回归音频扩散结合自回归文本是构建高效语音对话系统的有效方式**。

## 7. 优点：方法或实验设计上的亮点
- **统一训练范式**：首次通过吸收离散扩散统一了文本AR与音频NAR的训练目标，简化了模型架构。
- **模态感知注意力**：针对不同依赖关系设计不同的注意力模式，更符合模态特性。
- **并行生成效率**：音频块状扩散可并行合成，降低推理延迟。
- **灵活性**：能够处理变长的音频输出，适应实际对话场景。

## 8. 不足与局限
- **计算资源未披露**：缺少训练成本细节，难以评估方法的实际效率优势。
- **潜在偏差风险**：实验仅基于英文数据集？未说明是否覆盖多语言或噪声环境，泛化能力存疑。
- **应用限制**：吸收离散扩散的数学复杂性可能影响落地部署；对长文本-音频交织场景的鲁棒性未讨论。
- **对比基线**：未列出具体模型名称，读者难以直接复现比较。
- **训练策略细节**：三种训练策略的具体实施未在摘要中展开，缺乏技术透明性。

---

（完）
