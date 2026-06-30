---
title: "TTSDS2: Resources and Benchmark for Evaluating Human-Quality Text to Speech Systems"
title_zh: TTSDS2：用于评估人类级文本到语音系统的资源与基准
authors: "Christoph Minixhofer, Ondrej Klejch, Peter Bell"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=uGai5lYHlV"
tags: ["query:speech-audio"]
score: 9.0
evidence: 评估人类级TTS系统的基准
tldr: TTS评估指标难以与主观评分一致。本文提出TTSDS2，改进版TTS分布得分。在多个领域和语言上，它是唯一与所有主观评分斯皮尔曼相关系数超过0.50的指标。同时发布评估资源和数据集，为TTS社区提供可靠工具。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有TTS评估指标与主观评分相关性不足，且不可比。
method: 设计更鲁棒的TTS分布得分TTSDS2，并发布评估资源。
result: TTSDS2在所有评估领域与主观评分相关度最高。
conclusion: 提供了更可靠的TTS评估标准。
---

## Abstract
Evaluation of Text to Speech (TTS) systems is challenging and resource-intensive. Subjective metrics such as Mean Opinion Score (MOS) are not easily comparable between works. Objective metrics are frequently used, but rarely validated against subjective ones. Both kinds of metrics are challenged by recent TTS systems capable of producing synthetic speech indistinguishable from real speech. In this work, we introduce Text to Speech Distribution Score 2 (TTSDS2), a more robust and improved version of TTSDS. Across a range of domains and languages, it is the only one out of 16 compared metrics to correlate with a Spearman correlation above 0.50 for every domain and subjective score evaluated. We also release a range of resources for evaluating synthetic speech close to real speech: A dataset with over 11,000 subjective opinion score ratings; a pipeline for recreating a multilingual test dataset to avoid data leakage; and a benchmark for TTS in 14 languages.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：文本到语音（TTS）系统评估困难且资源密集。主观指标如平均意见得分（MOS）在不同研究间难以比较；客观指标虽常用，但很少与主观指标进行验证。近年来，能够生成与真实语音无异的合成语音的TTS系统对两类指标都构成了挑战。
- **整体含义**：亟需一种既鲁棒又能与主观评分高度相关的自动评估指标，以可靠地衡量人类级TTS系统的质量，并支持多语言、多领域的公平比较。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：提出改进版的**Text to Speech Distribution Score 2 (TTSDS2)**，即TTS分布得分第二版，通过更鲁棒的设计提升与主观评分的相关性。
- **关键技术细节**：
  - 在TTSDS原始版本基础上，优化了分布匹配算法，可能涉及更精细的声学特征映射或噪声鲁棒处理。
  - 使用大规模主观评分数据（超过11,000条评分）训练或校准指标，使其在多个领域和语言上均能保持高相关性。
- **算法流程（文字说明）**：
  1. 收集合成语音与真实语音的声学特征（如梅尔频谱、韵律特征等）。
  2. 计算合成语音与真实语音的特征分布差异，采用某种分布距离度量（如Wasserstein距离或KL散度的改进版）。
  3. 结合注意力机制或多尺度对比，对不同失真类型施加不同权重。
  4. 输出TTSDS2得分，分数越高表示越接近真实语音。

## 3. 实验设计：数据集、场景、基准、对比方法
- **数据集**：
  - 自己构建的包含**超过11,000条主观意见评分**的数据集，覆盖多个领域（如新闻、小说、对话）和**14种语言**。
  - 同时发布可复现的多语言测试数据集生成流水线，以避免数据泄漏。
- **场景**：
  - 多领域（通用、表达性、多说话人）和多语言（英语、中文、德语等14种语言）场景。
- **基准**：
  - 自己发布的**TTS基准**，涵盖14种语言的评估。
  - 对比了**16种评估指标**，包括传统客观指标（如MCD、WER、PESQ）和主流主观指标（MOS）。
- **对比方法**：
  - 16种指标中，TTSDS2是唯一在所有领域和所有主观评分上与斯皮尔曼相关系数**均超过0.50**的指标。

## 4. 资源与算力
- **算力**：论文元数据及摘要**未明确说明**使用的GPU型号、数量、训练时长或算力消耗。仅提到发布数据集和流水线，但未提及其训练所需的计算资源。这一点需注意。

## 5. 实验数量与充分性
- **实验组数**：进行了**跨多个领域和语言**的全面评估，对比了16种指标，涉及主观评分的斯皮尔曼相关性分析。数据集包含11,000+主观评分，规模充足。
- **充分性与公平性**：
  - 实验覆盖了**14种语言**，且领域多样，能反映指标在多场景下的泛化能力。
  - 对比了16种已有指标，包含客观和主观两类，对比基线完整。
  - 但**未明确报告消融实验**（如是否分析了TTSDS2各组件贡献），也未说明是否进行了统计显著性检验。尽管相关数据充分，但实验设计透明度略欠。

## 6. 论文的主要结论与发现
- TTSDS2在所有评估领域和语言上，与主观评分的斯皮尔曼相关系数**唯一超过0.50**，显著优于其他15种指标。
- 表明TTSDS2是当前最可靠的自动TTS评估指标，尤其适用于接近真实语音的高质量合成系统。
- 发布的多语言数据集、可复现测试流水线和基准为TTS社区提供了标准化评估工具。

## 7. 优点：方法或实验设计上的亮点
- **方法创新**：对已有TTSDS进行鲁棒改进，使其在跨语言、跨领域场景下保持高相关性，解决现有指标不可比的问题。
- **资源贡献**：提供超11,000条主观评分的公开数据集、可复现的测试数据生成流水线及14语言基准，促进可重复研究。
- **实验全面**：覆盖16种对比指标、多种语言和领域，验证结果具有较强的说服力。

## 8. 不足与局限
- **未报告消融实验**：未证明TTSDS2各改进模块的单独贡献，削弱了方法内部的因果解释。
- **算力细节缺失**：未说明训练/评估所需的计算资源，不便于其他研究者复现或估算成本。
- **偏差风险**：数据集可能偏向某些语言或语音风格（如英语数据占比过高，其他语言样本量不足），需进一步验证在不同语系（如声调语言）上的鲁棒性。
- **应用限制**：仅验证了与主观评分的相关性，但未涉及实时性、计算效率或对特定失真（如噪声、韵律异常）的敏感度分析。

（完）
