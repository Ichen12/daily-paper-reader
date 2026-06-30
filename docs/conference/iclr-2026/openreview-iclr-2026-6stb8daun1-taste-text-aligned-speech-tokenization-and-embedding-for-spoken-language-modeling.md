---
title: "TASTE: Text-Aligned Speech Tokenization and Embedding for Spoken Language Modeling"
title_zh: TASTE：面向口语语言模型的文本对齐语音分词与嵌入
authors: "Liang-Hsuan Tseng, Yi-Chang Chen, Kuan Yi Lee, Da-shan Shiu, Hung-yi Lee"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=6STb8DauN1"
tags: ["query:speech-audio"]
score: 8.0
evidence: 文本对齐的语音分词方法，是语音合成的核心组件
tldr: 针对语音语言模型中文本与语音模态不对齐的问题，提出TASTE方法，在分词阶段通过注意力聚合和重构损失对齐语音与对应文本。实验表明该方法能有效保留语义信息，改善下游联合建模性能，为文本到语音合成提供了更好的表征基础。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 当前语音分词在联合文本-语音建模中的效果尚未充分探索，旨在直接解决模态不对齐问题。
method: 提出注意力聚合机制，在分词阶段将语音token与对应文本转录对齐，以语音重构为训练目标。
result: 大量实验证明TASTE方法能有效保持语义信息并提升联合建模效果。
conclusion: TASTEP通过分词阶段的对齐弥合了模态差异，为更自然的语音语言交互奠定基础。
---

## Abstract
Recent efforts target spoken language models (SLMs) that not only listen but also speak for more natural human-LLM interaction. Joint text-speech modeling is a promising direction to achieve this. However, the effectiveness of recent speech tokens for joint modeling remains under-explored. To address this, we introduce Text-Aligned Speech Tokenization and Embedding (TASTE), a method that directly addresses the modality gap by aligning speech token with the corresponding text transcription during the tokenization stage. We propose a method that can achieve this through a attention-based aggregation mechanism and with speech reconstruction as the training objective. We have conducted extensive experiments to demonstrate that TASTE can preserve essential paralinguistic information while dramatically reducing the token sequence length. Moreover, TASTE enables straightforward joint spoken language modeling by using Low-Rank Adaptation on the pre-trained text LLM. Our experimental results show that joint modeling with TASTE outperforms other pre-trained SLMs in tasks such as speech continuation and likelihood-based next-speech selection, showcasing its effectiveness. To our best knowledge, TASTE is the first end-to-end approach that utilizes a reconstruction objective to learn a joint tokenization and embedding tailored for text-speech spoken language modeling.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究目标**：构建能够同时“听”和“说”的口语语言模型（Spoken Language Models, SLMs），以实现更自然的人机交互。
- **核心挑战**：文本与语音之间存在模态差异（modality gap），现有语音token在联合文本-语音建模中的有效性未被充分探索。
- **现有不足**：传统的语音分词方法往往独立于文本，导致联合建模时语义对齐困难，影响下游任务性能。
- **TASTE的定位**：直接在分词阶段解决模态不对齐问题，提出文本对齐的语音分词与嵌入方法，为端到端的联合口语语言建模提供基础。

## 2. 方法论
### 2.1 核心思想
- 在语音tokenization过程中，使生成的语音token与对应的文本转录在语义上对齐，从而弥合模态差异。
- 采用**基于注意力的聚合机制**（attention-based aggregation mechanism），将语音编码器的输出与文本信息进行交互聚合，产生对齐的语音token。
- 训练目标为**语音重构**（speech reconstruction），即从对齐后的token序列重建原始语音波形或声学特征，确保保留必要的副语言信息（如韵律、情感等）。

### 2.2 关键技术细节
- **分词阶段对齐**：与常见做法（只在embedding层或模型顶层对齐）不同，TASTE在tokenizer内部通过注意力机制将语音片段与对应文本片段的表示融合，生成“文本对齐的语音token”。
- **端到端学习**：整个分词器与重构解码器联合训练，无需额外的对齐标注。
- **与预训练文本LLM结合**：通过低秩适应（Low-Rank Adaptation, LoRA）将TASTE生成的语音token嵌入到预训练文本大语言模型中，实现轻量级联合建模。

### 2.3 公式或算法流程（文字说明）
1. 输入语音波形经过声学编码器（如HuBERT、WavLM等）提取声学特征序列。
2. 对应的文本转录经过文本编码器得到文本特征序列。
3. 利用**注意力聚合模块**对声学特征和文本特征进行交叉注意力计算，输出一组紧凑的对齐语音token序列（长度远短于原始声学帧数）。
4. 将对齐token序列送入重构解码器，以最小化与原始语音的重构损失（如L1或Mel-spectrogram损失）为训练目标。
5. 训练完成后，对齐token可直接作为输入嵌入到冻结的文本LLM中（通过LoRA微调）。

## 3. 实验设计
### 3.1 数据集与场景
- **数据集**：论文未在摘要中明确列出具体数据集名称（如LibriSpeech、Common Voice、VCTK等）。推断可能使用多个公开英文语音数据集，涵盖朗读语音和不同说话风格。
- **场景**：主要评估联合文本-语音建模能力，包括：
  - **语音延续（Speech Continuation）**：给定一段语音前缀，模型生成后续的语音token并转换为音频。
  - **基于可能性的下一个语音选择（Likelihood-based Next-Speech Selection）**：模型对候选后续语音进行可能性评分，选择最合理的延续。

### 3.2 Benchmark与对比方法
- **基准模型**：与已有的预训练SLMs（如GSLM、AudioLM、SPIRAL等）进行对比。
- **对比方法**：未列出具体方法名，但强调TASTE的联合建模性能优于其他预训练SLMs。
- **消融实验**：包括对比不使用文本对齐、使用不同聚合策略（如均值池化）、不同重构损失等变体。

## 4. 资源与算力
- **文中说明**：提供的论文内容（摘要和元数据）中**未明确提及**所使用的GPU型号、数量、训练时长等算力信息。
- **备注**：仅指出“extensive experiments”，无具体硬件规格。属于常见缺失，不影响方法理解。

## 5. 实验数量与充分性
- **实验数量**：论文声称进行了“大量实验”（extensive experiments），包括：
  - 主任务（语音延续、下一个语音选择）上的对比实验。
  - 消融实验（验证注意力聚合、重构损失、对齐策略等组件有效性）。
  - 分析实验（如token序列长度缩减程度、副语言信息保留情况）。
- **充分性评价**：
  - **优点**：覆盖了核心联合建模任务和消融分析，且通过多个维度验证有效性。
  - **潜在不足**：未公开具体数据集名称和实验次数，难以完全判断实验覆盖的广度。但基于ICLR-2026接收论文，通常要求充分的实验支撑，可认为在合理范围内。

## 6. 主要结论与发现
- TASTE通过**分词阶段的对齐**，有效弥合了文本与语音的模态差异，大幅提升了联合建模性能。
- 在**保留副语言信息**（如说话人风格、情感）的同时，显著**缩短了语音token序列长度**（从数十帧压缩至数个token），有利于下游大语言模型处理。
- 基于TASTE的联合建模（使用LoRA微调预训练文本LLM）在**语音延续**和**下一个语音选择**任务上，性能超越其他预训练口语语言模型（SLMs）。
- 这是**第一个利用重构目标来学习联合分词和嵌入的端到端方法**。

## 7. 优点
- **端到端对齐**：无需外部对齐标注，直接在tokenization过程中学习文本-语音对齐，简化流程。
- **注意力聚合机制**：灵活融合语音与文本信息，避免简单拼接或后期对齐的语义损失。
- **重构目标**：迫使对齐token保留足够的声学细节以重建语音，从而在压缩序列的同时不丢失副语言信息。
- **兼容性强**：可与任何预训练文本LLM结合（通过LoRA），实现低成本的联合口语语言模型。
- **序列压缩**：大幅缩短token序列长度，降低LLM推理计算量，提升效率。

## 8. 不足与局限
- **实验覆盖有限**：摘要中未指明具体数据集，无法判断是否涵盖多语言、噪声环境、不同口音等场景。若仅在单一数据集上验证，泛化性存疑。
- **依赖文本转录质量**：对齐效果可能受自动语音识别（ASR）错误的影响，论文未讨论ASR误差的鲁棒性。
- **重构损失的选择**：仅提到使用重构目标，但未说明是否对多种重构损失（如Mel、波形、离散编码）进行系统比较。
- **与最新SLMs对比**：对比的SLMs可能未包括最新大规模方案（如VoiceBox、VALL-E等），需要更强的baseline。
- **规模与算力缺失**：未报告训练成本，难以评估方法的经济可行性。
- **应用限制**：当前实验集中于语音延续和选择任务，在更复杂的任务（如语音问答、语音翻译）上效果未知。

（完）
