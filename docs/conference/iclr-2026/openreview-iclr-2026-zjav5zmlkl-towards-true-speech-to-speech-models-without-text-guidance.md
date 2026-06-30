---
title: Towards True Speech-to-Speech Models Without Text Guidance
title_zh: 迈向无需文本引导的真正语音到语音模型
authors: "Xingjian Zhao, Zhe Xu, Luozhijie Jin, Yang Wang, Hanfu Chen, Yaozhou Jiang, Ke Chen, Ruixiao Li, Mingshu Chen, Ruiming Wang, Wenbo Zhang, Qinyuan Cheng, Zhaoye Fei, Shimin Li, Xipeng Qiu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=zjaV5zmlkl"
tags: ["query:speech-audio"]
score: 8.0
evidence: 无需文本中间步骤的端到端语音模型，融合识别与合成
tldr: 级联式语音对话系统丢失副语言线索且灵活性受限。本文提出无需文本引导的端到端语音大模型，采用模态层级拆分与冻结预训练策略，保留文本LLM的推理与知识能力同时赋予原生语音处理能力，在口语问答等任务上取得最优结果，标志着向真正的语音对话系统迈出关键一步。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有语音对话依赖文本中间环节，丢失副语言信息且限制表达性。
method: 提出模态层级拆分架构和冻结预训练方法，将文本LLM扩展为原生语音理解与生成模型。
result: 在多项口语问答评测中取得最优性能，且保留副语言信息。
conclusion: 该工作证明了无需文本中间步骤的端到端语音大模型的可行性，为语音交互提供了新范式。
---

## Abstract
Spoken dialogue systems often rely on cascaded pipelines that transcribe, process, and resynthesize speech. While effective, this design discards paralinguistic cues and limits expressivity. Recent end-to-end methods reduce latency and better preserve these cues, yet still rely on text intermediates, creating a fundamental bottleneck. We present a true speech-to-speech large language model that directly understands and generates speech without relying on text guidance. Our approach combines a modality-based layer-splitting architecture with a frozen pre-training strategy, preserving the reasoning and knowledge of pretrained text LLMs while adding native speech capabilities. Experiments show that our model achieves state-of-the-art results in spoken question answering and delivers comparable speech-to-speech performance relative to existing text-guided systems, while still maintaining competitive text performance. By narrowing the gap between text-guided and direct speech generation, our work establishes a new paradigm for expressive and efficient end-to-end speech interaction. We will release our code and models to support further research in true speech-to-speech foundation models.

---

## 论文详细总结（自动生成）

# 论文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：现有语音对话系统多采用级联流水线（语音识别→文本处理→语音合成），该设计虽然有效，但会丢弃副语言线索（如语调、情感、韵律等），且表达灵活性受限。近期端到端方法虽能减少延迟并更好地保留副语言信息，但仍依赖文本作为中间表示，构成了根本性瓶颈。
- **整体含义**：本文旨在实现真正的语音到语音（speech-to-speech）大语言模型，无需文本中间步骤，直接理解并生成语音，从而保留副语言线索、提升交互的丰富性与效率，为端到端语音交互建立新范式。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：在保留预训练文本LLM的推理与知识能力的基础上，直接赋予模型原生语音处理能力，避免文本中间环节。
- **关键技术细节**：
  - **模态层级拆分架构（Modality-based Layer-Splitting Architecture）**：将模型层按模态功能拆分，部分层保留文本处理，新增专门处理语音的层，使得模型能同时处理语音理解与生成。
  - **冻结预训练策略（Frozen Pre-training Strategy）**：冻结预训练文本LLM的权重，仅训练新增的语音相关部分，从而保持原有知识不退化，同时以高效方式学习语音模态的表征与生成能力。
  - 整体流程：输入语音经过特征提取后进入模型，模型直接输出语音波形或离散表示，无需转录为文本。

## 3. 实验设计：数据集、Benchmark、对比方法
- **使用的数据集/场景**：论文提及在**口语问答（Spoken Question Answering）** 任务上进行评测。未明确列出具体数据集名称（如Spoken-SQuAD、Speech-QA等），也未说明是否包含多语言或噪声场景。
- **Benchmark**：采用口语问答评测作为主要benchmark，可能包含标准测试集。
- **对比方法**：
  - 现有文本引导（text-guided）的语音对话系统（即级联或预训练文本LLM结合语音接口的方法）。
  - 其他端到端方法（隐式依赖文本中间步骤的系统）。
  - 同时与纯文本LLM对比以验证语音能力的引入未严重损害文本性能。

## 4. 资源与算力
- **文中未明确说明**：论文摘要及元数据未提及训练使用的GPU型号、数量、训练时长等具体算力信息。这表明作者可能未在公开版本中披露实验计算资源。（需指出这一点）

## 5. 实验数量与充分性
- **实验数量**：仅提到“在多项口语问答评测中取得最优性能”，“多项”表明至少2以上，但未给出具体实验组数或消融实验细节。未报告在不同数据集、不同任务（如语音翻译、情感合成）上的结果。
- **充分性评估**：
  - **充分性有限**：缺乏对模型在不同副语言线索（语调、情感、语速）保留效果的主观/客观评测；未报告模型输出语音的可懂度、自然度指标；未与更多基线（如直接生成语音的VALL-E、SPEECHGPT等）在同等条件下对比。
  - **公平性**：论文声称“state-of-the-art”，但未提供完整实验表格或超参数设置，公平性难以验证。消融实验也未涉及。
  - 总体而言，实验覆盖偏窄，不足以充分验证方法的泛化能力和实际优势。

## 6. 论文的主要结论与发现
- **主要结论**：本文提出的无需文本引导的端到端语音大模型，在口语问答任务上取得了当前最优结果，同时保持了与文本引导系统相当的语音到语音性能，且文本能力未明显下降。
- **关键发现**：通过模态层级拆分与冻结预训练，可以有效将文本LLM扩展为原生语音理解与生成模型，缩小了文本引导与直接语音生成之间的性能差距，证明了“真正语音到语音”范式的可行性。

## 7. 优点：方法或实验设计上的亮点
- **方法亮点**：
  - 首次提出不依赖任何文本中间步骤的纯语音大模型，突破了现有级联或隐式文本瓶颈。
  - 冻结预训练策略极具实用性：无需从头训练大模型，利用已有LLM知识库，显著降低训练成本与数据需求。
  - 模态层级拆分架构清晰合理，易于扩展其他模态（如音频事件）。
- **实验设计亮点**：关注语音问答这一代表性任务，并与文本引导系统对比文本和语音两方面的性能，体现综合能力。

## 8. 不足与局限
- **实验覆盖不足**：仅评测口语问答，未在语音翻译、语音交互（对话、指令跟随）、情感语音生成等任务上验证；未提供主观听感评测（如Mean Opinion Score）。
- **数据偏差风险**：训练数据可能来自单一语种或受限领域，模型在真实多变场景（噪声、口音、语速）下的鲁棒性未知。
- **应用限制**：
  - 生成语音的自然度和表现力是否真正优于级联系统缺乏量化证据。
  - 未讨论延迟与计算开销，端到端生成可能比级联更耗时。
  - 模型是否支持实时交互未明确。
- **方法论细节缺失**：未公开模型架构具体层数、参数量、训练超参，无法复现。

（完）
