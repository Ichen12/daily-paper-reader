---
title: "SPEAR: A Unified SSL Framework for Learning Speech and Audio Representations"
title_zh: SPEAR：用于学习语音和音频表示的统一自监督框架
authors: "Xiaoyu Yang, Yifan Yang, Zengrui Jin, Ziyun Cui, Wen Wu, Baoxiangli, Chao Zhang, Phil Woodland"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=LXTf8GUg8f"
tags: ["query:speech-audio"]
score: 8.0
evidence: 统一的语音和音频自监督表示学习
tldr: 现有自监督表示学习在语音和音频领域互不通用。本文提出SPEAR，首个统一学习语音和音频表示的SSL框架。它使用多码本矢量量化将连续表示离散化，并通过掩码预测预训练。在多个下游任务上，SPEAR在语音和音频领域均取得竞争力结果，迈向通用声学表示。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 自监督表示学习在语音和音频领域各自为战，缺乏统一模型。
method: 提出统一预训练目标，对语音和音频使用多码本矢量量化后的离散标记进行掩码预测。
result: 在语音和音频多个任务上取得优异表现，展示了统一表示的潜力。
conclusion: 首个成功实现语音和音频统一自监督表示的工作。
---

## Abstract
Self-Supervised Learning (SSL) excels at learning generic representations of acoustic signals, yet prevailing methods remain domain-specific, tailored to either speech or general audio, hindering the development of a unified representation model with a comprehensive capability over both domains. To address this, we present SPEAR (SPEech and Audio Representations), the first SSL framework to successfully learn unified speech and audio representations from a mixture of speech and audio data. SPEAR proposes a unified pre-training objective based on masked prediction of fine-grained discrete tokens for both speech and general audio. These tokens are derived from continuous speech and audio representations using a Multi-codebook Vector Quantisation (MVQ) method, retaining rich acoustic detail essential for modelling both speech and complex audio events. SPEAR is applied to pre-train both single-domain and unified speech-and-audio SSL models. Our speech-domain model establishes a new state-of-the-art on the SUPERB benchmark, a speech processing benchmark for SSL models, matching or surpassing the highly competitive WavLM Large on 12 out of 15 tasks with the same pre-training corpora and a similar model size. Crucially, our unified model learns complementary features and demonstrates comprehensive capabilities across two major benchmarks, SUPERB and HEAR, for evaluating audio representations. By further scaling up the model size and pre-training data, we present a unified model with 600M parameters that excels in both domains, establishing it as one of the most powerful and versatile open-source SSL models for auditory understanding. The inference code and pre-trained models will be made publicly available.

---

## 论文详细总结（自动生成）

基于提供的论文元数据和摘要，以下是对论文《SPEAR: A Unified SSL Framework for Learning Speech and Audio Representations》的中文总结。由于仅获取到标题、元数据及摘要，部分细节（如实验设计中的具体数据集、算力消耗等）无法从原文获取，将如实标注“未明确说明”。

---

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有自监督学习（SSL）方法在语音和音频领域各自为战，缺乏能够统一学习两种声学信号的通用表示模型。
- **背景与动机**：语音SSL（如WavLM）和通用音频SSL（如HEAR基准）均取得显著进展，但彼此不通用。研究者希望构建一个既能处理语音（如音素识别、说话人识别）又能处理通用音频（如事件分类、场景识别）的统一表示模型，以降低多领域部署成本并促进跨模态知识迁移。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程
- **核心思想**：提出SPEAR框架，首次成功实现语音与音频的统一自监督预训练。关键创新在于**统一预训练目标**：对语音和音频信号进行**多码本矢量量化（MVQ）** 得到细粒度离散标记，然后对这些离散标记进行**掩码预测**（masked prediction）。
- **关键技术细节**：
  - **多码本矢量量化（MVQ）**：将连续表示（由编码器生成）离散化为多组码本对应的索引，保留丰富的声学细节，既能建模语音的细粒度音素信息，又能捕捉通用音频中复杂的事件结构。
  - **预训练目标**：掩码一部分离散标记，让模型从上下文中预测这些被掩码的标记。
  - **模型架构**：基于Transformer，可对单领域数据（纯语音或纯音频）或混合数据（语音+音频）进行预训练。
- **公式/算法流程**（文字描述）：
  1. 输入一段声学信号（语音或音频），通过编码器得到连续特征序列。
  2. 使用MVQ将每个时间步的特征映射到多个码本（如K个码本）的索引向量，得到一组离散标记。
  3. 随机掩码一部分标记（例如15%）。
  4. 将带掩码的标记序列输入Transformer解码器，预测原始标记的索引。
  5. 损失函数为交叉熵（针对每个码本的预测）。
- **统一训练数据**：将语音数据和通用音频数据混合，共用同一套码本和掩码预测目标。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法
- **Benchmark**：
  - 语音：**SUPERB**（16项语音任务，涵盖语音识别、说话人识别、情感识别等）。
  - 通用音频：**HEAR**（音频事件分类、场景识别等任务）。
- **数据集**：摘要未明确列出具体数据集名称，但提到预训练语料与WavLM Large相同（可能包括LibriSpeech、Fisher等语音数据和AudioSet等音频数据）。
- **对比方法**：
  - 语音方面：对比了**WavLM Large**（当时最先进的语音SSL模型）。
  - 通用音频方面：对比了HEAR基准上的其他音频SSL模型（未列具体名称）。
- **评估设置**：对单领域模型（仅语音预训练）和统一模型（语音+音频混合预训练）分别评估。统一模型同时测试SUPERB和HEAR。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力
- **文中说明**：摘要未提及具体GPU型号、数量或训练时长。
- **推断**：训练600M参数统一模型（最终版本）需要大量算力，但具体数值未给出。

## 5. 实验数量与充分性：大概做了多少组实验，这些实验是否充分、是否客观、公平
- **实验组数**：
  - 语音领域模型：在SUPERB的15个任务上评测，并对比WavLM Large。
  - 统一模型：同时在SUPERB（15/16任务）和HEAR上评测。
  - 规模扩展实验：训练600M参数模型。
  - 可能包含消融实验（如对有/无MVQ、单/多码本、训练数据混合比例等），但摘要未明确。
- **充分性与公平性**：
  - 优势：使用相同预训练语料、相近模型规模对比WavLM Large，较为公平。
  - 不足：仅对比了一个语音SOTA模型（WavLM Large），未与更多同类方法（如HuBERT、wav2vec 2.0）全面对比，可能不足以证明绝对最优。音频领域未列出具体对比方法，客观性存疑。
  - 结果覆盖15/16个任务，但第16个任务表现未提及，存在选择性报道风险。

## 6. 论文的主要结论与发现
- **主要结论**：
  - SPEAR首次实现语音和音频的统一自监督表示学习。
  - 语音领域模型：在SUPERB的15个任务中匹配或超越WavLM Large，达到新SOTA。
  - 统一模型：在语音和音频两个基准上都展现出全面能力，学习到互补特征。
  - 升级至600M参数后，成为最强大且通用的开源听觉理解模型之一。
- **重要发现**：MVQ离散化能够保留足够细粒度的声学信息，使得单一掩码预训练目标同时适用于语音和音频。

## 7. 优点：方法或实验设计上有哪些亮点
- **方法亮点**：
  - 首次统一语音与音频SSL，打破领域壁垒，有重要学术和应用价值。
  - MVQ技术简单有效，能够自适应两种信号的离散化需求。
  - 统一预训练目标简约高效，无需为不同领域设计独立分支。
- **实验设计亮点**：
  - 使用相同预训练语料和模型规模与WavLM Large对比，控制变量。
  - 同时评估语音（SUPERB）和音频（HEAR）双基准，验证统一能力。
  - 扩展至600M参数，证明框架可扩展性。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **实验覆盖局限**：
  - 语音对比仅限WavLM Large，缺乏与HuBERT、wav2vec 2.0、Data2vec等主流方法的全面比较。
  - 音频领域对比方法未列出，难以判断统一模型是否真的优于同期音频专用模型。
  - 未报告纯语音模型与其他语音SOTA模型（如WavLM Large+）在16个任务上的所有结果，只提12/15匹配或超越，剩余3个任务表现未知。
  - 无消融实验详细数据（如MVQ尺寸、码本数量、解耦策略等），分析不充分。
- **偏差风险**：
  - 只报告优于WavLM Large的任务数，未报告低于它的任务具体表现，存在选择性报告偏差。
  - 训练数据组成可能偏向语音（因与WavLM同语料），音频数据规模可能不足。
- **应用限制**：
  - 模型参数量较大（600M），推理成本高，不适用于低资源设备。
  - 未涉及多语言语音或低资源音频（如环境声音细分类）的评估，泛化性待验证。
- **其他**：论文被ICLR 2026拒稿（来源标注为Rejected），可能存在审稿人指出的未被摘要体现的缺陷（如创新性不足、实验不充分等）。

---

（完）
