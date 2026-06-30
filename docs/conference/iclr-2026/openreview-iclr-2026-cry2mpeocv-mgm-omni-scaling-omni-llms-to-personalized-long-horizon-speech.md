---
title: "MGM-Omni: Scaling Omni LLMs to Personalized Long-Horizon Speech"
title_zh: MGM-Omni：将全模态大模型扩展到个性化长时语音
authors: "Chengyao Wang, Zhisheng Zhong, Bohao PENG, Senqiao Yang, Yuqi Liu, Haokun GUI, Bin Xia, Jingyao Li, Bei Yu, Jiaya Jia"
date: 2025-09-13
pdf: "https://openreview.net/pdf?id=Cry2mpEocV"
tags: ["query:speech-audio"]
score: 9.0
evidence: 统一全模态大模型支持流式语音生成和零样本声音克隆
tldr: "针对级联流水线将语音合成割裂的问题，提出MGM-Omni统一模型，采用\"大脑-嘴巴\"双轨道架构解耦多模态推理与实时语音生成。分块并行解码加速推理，支持流式零样本声音克隆和个性化长时语音生成。实验证明其在理解与生成任务上均达到先进水平。"
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有级联方法割裂了语音合成与其他模态的交互，难以支持实时流式生成。
method: 提出双轨道token化架构，解耦推理与语音生成；采用分块并行解码和双音频编码器。
result: 实现了低延迟流式语音生成和零样本声音克隆，并在多模态理解基准上表现优异。
conclusion: MGM-Omni为统一全模态理解与生成提供了高效方案，尤其适合需要实时语音交互的场景。
---

## Abstract
We present MGM-Omni, a unified Omni LLM for omni-modal understanding and expressive, long-horizon speech generation. Unlike cascaded pipelines that isolate speech synthesis, MGM-Omni adopts a “brain–mouth” design with a dual‑track, token-based architecture that cleanly decouples multimodal reasoning from real-time speech generation. This design enables efficient cross-modal interaction and low-latency, streaming speech generation. For understanding, a unified training strategy coupled with a dual audio encoder design enables long-form audio perception across diverse acoustic conditions. For generation, a chunk-based parallel decoding scheme narrows the text–speech token-rate gap, accelerating inference and supporting streaming zero-shot voice cloning with stable timbre over extended durations. Compared to concurrent work, MGM-Omni achieves these capabilities with markedly data-efficient training. Extensive experiments demonstrate that MGM-Omni outperforms existing open source models in preserving timbre identity across extended sequences, producing natural and context-aware speech, and achieving superior long-form audio and omnimodal understanding. MGM-Omni establishes an efficient, end-to-end paradigm for omnimodal understanding and controllable, personalized long-horizon speech generation.

---

## 论文详细总结（自动生成）

好的，以下是根据您提供的论文信息生成的中文总结。

# 论文总结：MGM-Omni：将全模态大模型扩展到个性化长时语音

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：现有级联式（cascaded）流水线将语音合成与其他模态（如文本、图像、音频等）的处理割裂开来，导致跨模态交互效率低下，难以支持实时、流式（streaming）的语音生成，尤其是在个性化长时（long-horizon）对话场景中，难以保持音色稳定。
- **整体含义**：论文提出一种统一的全模态大模型（Omni LLM）——MGM-Omni，旨在实现多模态理解与表达性、长时语音生成的端到端统一，从而为需要实时语音交互的应用（如数字人、语音助手）提供高效、可控的解决方案。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：采用“大脑-嘴巴”（brain–mouth）双轨道（dual-track）的基于token的架构，将多模态推理（理解）与实时语音生成（生成）解耦。
- **关键技术细节**：
  - **双轨道token化架构**：一个轨道负责多模态理解（大脑），另一个轨道负责流式语音生成（嘴巴），两者通过跨模态交互高效协同。
  - **统一训练策略与双音频编码器**：用于理解部分，使模型能够感知不同声学条件下的长音频。
  - **分块并行解码（chunk-based parallel decoding）**：用于生成部分，缩小文本与语音之间的token速率差距，加速推理，并支持流式零样本声音克隆，在长时间内保持稳定的音色。
- **算法流程**：输入多模态数据（文本、图像、音频）后，首先由双轨道架构中的“大脑”部分进行推理与特征提取；然后“嘴巴”部分根据推理结果分块并行生成语音token，通过解码器输出流式语音。整个过程无需级联中间模块，实现端到端处理。

## 3. 实验设计：数据集/场景、Benchmark、对比方法
- **数据集/场景**：文献未明确列出具体使用的数据集名称。实验场景涵盖多模态理解（长音频感知、跨模态问答）和语音生成（零样本声音克隆、长时个性化语音）两类任务。
- **Benchmark**：未详细说明具体的基准测试集，但提及在“保持长序列音色身份”、“生成自然且上下文感知的语音”、“长音频与全模态理解”等指标上进行了评估。
- **对比方法**：与现有开源模型进行了比较（但未列出具体模型名称），声称MGM-Omni在这些任务上优于现有开源模型。

## 4. 资源与算力
- 文献中**未明确说明**所使用的GPU型号、数量、训练时长等算力信息。仅提到与同类工作相比，MGM-Omni在数据效率上显著更高（“markedly data-efficient training”），但未量化具体资源消耗。

## 5. 实验数量与充分性
- **实验数量**：摘要仅提及“Extensive experiments”，未给出具体实验组数、消融实验细节。由于缺乏实验设计表、数据集和对比方法的详细描述，无法判断实验数量的充分性。
- **充分性评估**：从现有信息看，实验覆盖面较广（涵盖理解与生成两大类），但缺乏具体消融实验、跨数据集验证、与多种基线方法的定量对比表格，因此实验的**客观性与公平性**难以验证。需要补充更多细节才能做出完整评估。

## 6. 主要结论与发现
- MGM-Omni在保持长序列音色一致性上优于现有开源模型。
- 能够生成自然且上下文感知的语音。
- 在长音频感知和全模态理解任务上达到先进水平。
- 提出了一种高效、端到端的范式，用于全模态理解与可控的、个性化的长时语音生成。

## 7. 优点：方法或实验设计上的亮点
- **架构创新**：双轨道解耦设计清晰分离了推理与生成，既保留了多模态推理的灵活性，又实现了实时流式语音输出。
- **效率提升**：分块并行解码有效解决了文本与语音token速率不匹配的问题，加速推理，并支持零样本声音克隆的流式化。
- **数据高效**：在训练数据量相对较小的条件下仍能达到优秀性能（相比同类工作）。
- **一体化方案**：统一了多模态理解与语音生成，避免了级联系统的误差累积和延迟。

## 8. 不足与局限
- **实验细节缺失**：未公开具体使用的数据集、基准测试、对比方法清单、量化结果表格，使得复现和客观评价困难。
- **算力信息缺失**：未报告训练所需的GPU型号、数量、时长，不利于其他研究者评估资源门槛。
- **应用限制**：摘要未讨论模型在真实复杂环境（如噪声、多人对话、情感控制等）下的鲁棒性，也未给出可处理的语音长度上限、延迟具体数值等。
- **偏差风险**：仅与“现有开源模型”比较，未与闭源的最新商业系统对比；且可能存在选定的基准测试对自身有利的潜在偏差。

（完）
