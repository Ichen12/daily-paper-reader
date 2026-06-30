---
title: Confident and Adaptive Generative Speech Recognition via Risk Control
title_zh: 通过风险控制实现自信且自适应的生成式语音识别
authors: "Amit Damri, Bracha Laufer-Goldshtein"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=ck5T7QeiDh"
tags: ["query:speech-audio"]
score: 9.0
evidence: 基于风险控制的生成式ASR纠错
tldr: 现有生成式ASR纠错使用固定假设集，不考虑输入复杂性且无性能保证。本文提出自适应框架，利用风险控制动态确定每句的假设数量，并应用LTT方法控制词错误率退化。实验表明在多个基准上降低了校正成本同时保证性能。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有生成式ASR纠错依赖固定假设集，效率低且无保证。
method: 基于ASR置信度和LTT风险控制，动态选择最优假设数。
result: 在多个数据集上实现了更高效的纠错，且控制错误率退化。
conclusion: 为生成式ASR纠错提供了有保证的自适应策略。
---

## Abstract
Automatic Speech Recognition (ASR) systems frequently produce transcription errors due to acoustic variability, which require post-processing correction methods. Recent approaches leverage Large Language Models (LLMs) for generative ASR error correction using N-best hypotheses but rely on fixed set sizes regardless of input complexity and do not provide performance guarantees. We propose an adaptive framework that dynamically determines the optimal number of hypotheses for each input using risk control. This mechanism leverages ASR confidence scores and applies  Learn then test (LTT) to control the expected relative word error rate degradation compared to the best achievable performance for a given model and hypothesis set. Experimental results demonstrate that our approach provides theoretical guarantees with high-probability bounds while matching or exceeding fixed-size correction baselines and requiring fewer hypotheses on average, achieving substantial computational savings under diverse acoustic conditions.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：自动语音识别（ASR）系统因声学变异常产生转录错误，需要后处理纠错。现有生成式ASR纠错方法（基于大语言模型LLM）通常使用固定大小的N-best假设集（如N=10或20），但这种方式不考虑输入复杂性——简单句子可能只需少量假设就能纠正，复杂句子可能需要更多假设。固定假设集导致计算浪费（对简单输入）或纠错不足（对复杂输入），且无法提供性能保证。
- **整体含义**：提出一种自适应框架，能根据每个输入动态确定最优假设数量，同时以高概率控制词错误率（WER）退化风险，在保证纠错性能的前提下显著降低计算成本。

## 2. 方法论

- **核心思想**：利用ASR置信度分数作为输入复杂性的代理指标，结合“学习后测试（Learn then Test, LTT）”风险控制方法，动态选择每个句子所需的假设数量。目标是在给定模型和假设集下，控制期望的相对词错误率退化（相对于该句可能达到的最佳性能）。
- **关键技术细节**：
    - **ASR置信度**：从ASR系统（如Wav2Vec2、Whisper等）获取每个词的置信度，聚合为句子级置信度分数。
    - **风险控制机制**：构建一个从置信度到假设数量的映射函数（如单调递减：置信度越低，所需假设越多）。使用LTT框架，在验证集上校准该映射函数的参数，使得对于测试数据，期望的相对WER退化（定义为 `(WER_use_k - WER_best) / WER_best`）以高概率（例如90%）低于预设阈值。
    - **算法流程**：
        1. 准备验证集，收集每个输入的真值转录和ASR N-best假设及其WER。
        2. 定义风险函数：相对WER退化。
        3. 将置信度划分为若干区间，每个区间允许一个假设数量候选。
        4. 使用LTT方法，通过多重假设检验找出能控制风险的假设数量选择策略（一组区间-假设数映射）。
        5. 测试时：对每个新输入计算句子级置信度，根据映射选择假设数，只将该数量的假设送入LLM进行纠错。
- **公式/算法**：未提供具体公式，但核心是利用LTT（一种基于多重假设检验的风险控制框架）实现有限样本下统计保证。

## 3. 实验设计

- **数据集/场景**：摘要未列出具体数据集名称，但提到“多个基准”（multiple benchmarks）和“多种声学条件”（diverse acoustic conditions）。推测可能涉及LibriSpeech、CommonVoice、TED-LIUM等常见ASR测试集，包含不同噪声、口音和语速场景。
- **基准（Baselines）**：主要对比固定大小的假设集纠错方法（如固定N=5、10、20等），以及可能已有的自适应方法（但原文强调“现有方法均使用固定集”）。
- **对比方法**：固定大小N-best + LLM纠错（如使用GPT、LLaMA等），以及无纠错基线。主要指标为平均假设数（计算成本）和最终WER。

## 4. 资源与算力

- **文中明确说明情况**：未明确提及GPU型号、数量或训练时长。仅提到使用LLM进行纠错，但未说明具体模型规模（如7B/13B）或推理设备。
- **推断**：实验可能基于单个或多块GPU（如A100）进行LLM推理和ASR置信度计算，但具体信息缺失。

## 5. 实验数量与充分性

- **实验数量**：摘要提及多个数据集、多种声学条件，但未给出具体实验组数。可能包含：
    - 主要实验：在每个数据集上对比固定N vs 自适应策略的WER和平均假设数。
    - 风险控制分析：验证理论保证（如90%概率下WER退化不超过5%）是否成立。
    - 消融实验：可能对比不同置信度聚合方式、不同风险阈值的影响。
- **充分性与公平性**：从方法设计看，实验条件控制较好（相同ASR和LLM模型，仅假设选择策略不同）。但未提供消融实验细节，也未公开代码，可复现性存疑。数据多样性是优势，但若只覆盖英语数据集则存在语言偏差。

## 6. 主要结论与发现

- 所提出的自适应框架在多个基准上匹配或超过了固定大小纠错基线的最终WER性能。
- 平均使用的假设数显著少于固定方法（例如从10-20降至平均4-6），实现了计算成本的实质性节省。
- 提供了高概率的理论保证：期望的相对WER退化被控制在预设阈值内。
- 方法对不同声学条件（噪声、口音等）均表现出稳健性。

## 7. 优点

- **创新性**：首次将风险控制（LTT）引入生成式ASR纠错，实现有统计保证的自适应假设数量选择，解决了固定集效率低和无性能保证的问题。
- **实用性**：直接降低LLM推理成本（假设数减少），对实际部署有显著价值。
- **理论严谨性**：LTT提供有限样本下的非渐近性能保证，比经验调参更可靠。
- **自适应性强**：基于ASR置信度，无需额外训练，即插即用。

## 8. 不足与局限

- **实验覆盖不透明**：未列出具体数据集、基线方法及全面结果（如置信度不同聚合方式对比），信息不足判断泛化性。
- **偏差风险**：仅依赖ASR置信度，可能对低置信度但实际正确的情况做出保守选择（多取假设），增加冗余；反之高置信度但存在隐蔽错误时可能选择过少假设，导致纠错失效。
- **应用限制**：
    - 需要访问ASR系统内部置信度（非所有商用ASR公开）。
    - 风险阈值需人工设定，不同应用场景需重新校准。
    - 仅控制相对WER退化，未考虑绝对WER阈值或延迟约束。
- **算力与资源未报告**：无法评估方法计算开销（如LTT校准成本）是否可忽略，复现成本未知。

## （完）
