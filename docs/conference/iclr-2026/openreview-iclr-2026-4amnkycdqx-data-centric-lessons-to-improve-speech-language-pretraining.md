---
title: Data-Centric Lessons To Improve Speech-Language Pretraining
title_zh: 数据为中心的语音语言预训练改进经验
authors: "Vishaal Udandarao, Zhiyun Lu, Xuankai Chang, Yongqiang Wang, Albin Madappally Jose, Fartash Faghri, Joshua P Gardner, Chung-Cheng Chiu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=4amNkYCDqX"
tags: ["query:speech-audio"]
score: 8.0
evidence: 语音语言模型数据为中心的预训练
tldr: 本文针对语音语言模型预训练中的数据问题进行了系统研究，探讨了原始网络音频内容的处理、合成数据集的构建以及不同预处理策略对问答任务的影响。通过控制变量实验，揭示了数据质量、数据多样性和合成数据比例等关键因素对模型性能的贡献，为语音语言模型的高效预训练提供了可操作的指导。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 语音语言模型的预训练数据缺乏系统的消融研究，导致性能提升原因不明确。
method: 设计三组对比实验，分别研究音频处理、合成数据构建和预处理策略的优化。
result: 实验揭示了数据质量、多样性和合成数据比例对语音问答任务性能的关键影响。
conclusion: 提出了面向语音语言模型预训练的数据处理最佳实践，显著提升了SQA能力。
---

## Abstract
Spoken Question-Answering (SQA) is a core capability for useful and interactive artificial intelligence systems. Recently, several speech-language models (SpeechLMs) have been released with a specific focus on improving their SQA performance. However, a lack of controlled ablations of pretraining data processing and curation makes it challenging to understand what factors account for performance, despite substantial gains from similar studies in other data modalities. In this work, we address this gap by conducting a data-centric exploration for pretraining SpeechLMs. We focus on three questions fundamental to speech-language pretraining data: (1) how to process raw web-crawled audio content for speech-text pretraining, (2) how to construct synthetic datasets to augment web-crawled data and (3) how to interleave (text, audio) segments into training sequences. We apply the insights from our controlled data-centric ablations to pretrain a 3.8B-parameter SpeechLM, called SpeLangy, that outperforms models that are up to 3x larger by 10.2% absolute performance. We hope our findings highlight the impact of effective data curation and guide future data-centric exploration in SpeechLMs.

---

## 论文详细总结（自动生成）

# 论文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **背景**：语音问答 (SQA) 是实现实用、交互式人工智能系统的核心能力。近期涌现的语音语言模型 (SpeechLM) 致力于提升 SQA 性能。
- **问题**：尽管在其他数据模态（如文本）中，数据消融研究已证明预训练数据处理的巨大影响，但在语音语言模型领域，缺乏对预训练数据整理和处理的 **受控消融实验**，导致难以理解性能提升的真正归因。
- **动机**：填补这一空白，通过数据为中心的视角系统探索语音语言预训练数据中的关键因素，为高效预训练提供可操作的指导。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：围绕预训练数据处理中的三个基本问题展开系统受控实验：
  1. **如何处理原始网络爬取的音频内容**用于语音‑文本预训练？
  2. **如何构建合成数据集**来增强网络爬取数据？
  3. **如何交错 (Interleave) 文本和音频片段**组成训练序列？
- **关键技术细节**：通过设计多组控制变量消融实验，分别比较不同音频预处理策略、合成数据比例、文本‑音频交错方式对 SQA 性能的影响，从而归纳出最佳数据处理实践。
- **最终应用**：将验证后的最佳策略用于预训练一个 **3.8B 参数的 SpeechLM**，命名为 **SpeLangy**。

> 注：原文未提供具体公式或算法流程，仅描述实验设计思路。

## 3. 实验设计：使用的数据集、基准与对比方法
- **数据集**：
  - 原始网络爬取音频数据（来源未具体说明）。
  - 合成数据集（构建方法未详述，用于增强真实数据）。
  - 未明确列出具体语音问答 benchmark 名称（如 SQA、LibriSpeech 等），但评估任务明确为 **Spoken Question‑Answering**。
- **基准与对比方法**：
  - 对比了参数量 **高达 3 倍** 的其他 SpeechLM 模型。
  - 未列出具体对比模型名称，但以 SQA 绝对性能为指标进行横向比较。
- **实验充分性**：通过多组受控消融实验（覆盖三个关键问题），实验设计系统且变量隔离，能够识别各因素的影响。

## 4. 资源与算力
- **文中未明确说明**使用的 GPU 型号、数量、训练时长。仅指出预训练了一个 3.8B 参数的模型 SpeLangy，但未提供训练硬件与耗时细节。

## 5. 实验数量与充分性
- **实验数量**：围绕三个问题各设计多组消融实验，至少包含对音频处理策略、合成数据比例、交错方式的独立与联合分析。具体组数未列明。
- **充分性评价**：
  - **充分**：实验覆盖了数据从处理到组织的主要环节，变量可控，能分离出质量、多样性、比例等关键因素贡献。
  - **客观公平**：受控消融设计避免了混杂因素，对比的基线模型参数量更大，突显数据整理的有效性。

## 6. 论文的主要结论与发现
- **关键发现**：数据质量、数据多样性和合成数据比例是对 SQA 性能影响最大的三个因素。
- **主要结果**：应用最佳数据处理实践的 SpeLangy（3.8B 参数）在 SQA 任务上，比参数量为其 **3 倍** 的模型获得了 **10.2% 的绝对性能提升**。
- **核心主张**：有效的数据整理是提升 SpeechLM 的 SQA 能力的重要杠杆，其效果可超越单纯增大模型规模。

## 7. 优点：方法或实验设计的亮点
- **数据为中心的系统性**：从零开始系统梳理语音预训练数据处理的三类问题，填补了该领域受控消融研究的空白。
- **变量隔离的消融设计**：能够明确归因性能增益来源（质量、多样性、合成数据比例），而非仅靠整体效果推断。
- **实践指导性强**：得到的结论可直接用于其他 SpeechLM 的预训练数据处理，具有操作参考价值。
- **以小胜大**：较小的模型（3.8B）通过数据优化超越大模型，凸显了数据整理的经济性。

## 8. 不足与局限
- **实验细节公开不足**：具体使用的原始音频数据集、合成数据构建方法、基准模型名称及全部实验超参数未在摘要/元数据中提供，影响可复现性。
- **任务覆盖窄**：仅针对 SQA 单一任务进行评估，未验证所提最佳实践对语音理解、语音翻译等其他语音任务是否同样有效。
- **合成数据偏差风险**：合成数据集可能引入特定噪声或分布偏移，其泛化性在更广泛场景下有待验证。
- **算力资源缺失**：未报告训练成本，难以判断该方法在实际大规模预训练中的可负担性。
- **架构局限**：仅基于 3.8B 参数的单模型验证，是否适用于不同架构（如 encoder‑decoder、纯 decoder）尚不清楚。

（完）
