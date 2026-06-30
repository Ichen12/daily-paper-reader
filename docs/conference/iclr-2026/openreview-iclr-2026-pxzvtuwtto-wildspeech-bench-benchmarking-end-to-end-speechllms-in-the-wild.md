---
title: "WildSpeech-Bench: Benchmarking End-to-End SpeechLLMs in the Wild"
title_zh: WildSpeech-Bench：在真实环境中评估端到端语音大模型
authors: "Linhao Zhang, Jian Zhang, Bokai Lei, Chuhan Wu, Aiwei Liu, Wei Jia, Zhou Xiao"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=pxZvtuWTtO"
tags: ["query:speech-audio"]
score: 9.0
evidence: 面向语音大模型的综合基准
tldr: 端到端语音大模型缺乏专门评估基准。WildSpeech-Bench 首个系统性地构建了面向实际语音对话的综合基准，涵盖韵律、同音词、口吃等语音特有挑战，并收集真实场景聊天数据，为评估音频LLM的用户体验提供了标准化工具。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有评估方法忽视语音特有特性，缺乏专门基准。
method: 系统构建包含语音特有挑战的真实世界聊天数据基准。
result: 提供了综合评估语音大模型的标准化工具。
conclusion: WildSpeech-Bench 推动了语音大模型在真实场景下的评估与优化。
---

## Abstract
Recent multi-modal Large Language Models (LLMs) such as GPT-4o have demonstrated strong capabilities of direct speech interaction. However, the lack of specialized and comprehensive benchmarks for end-to-end speech LLM evaluation hinders optimizing the user experience of Audio LLMs in real-world applications. Existing evaluation methods often adapt text-based benchmarks, overlooking speech's unique characteristics and challenges, including prosody, homophones, stuttering, and differing user expectations. Here, we introduce the first comprehensive benchmark designed to systematically evaluate end-to-end speechLLMs in practical speech conversations. We systematically curate real-world chat data relevant to spoken scenarios, introduce diversity in speaker attributes and acoustic conditions, and augment the dataset with speech-specific phenomena. We further design a query-aware evaluation method to use customized evaluation checklists and prompts to enhance the accuracy of automatic evaluation. We conduct comprehensive testing and detailed analysis of various mainstream speech models, revealing significant differences in model performance across different speech scenarios. The use of query-aware evaluation further enables a finer-grained assessment under various speech-specific scenarios. Our benchmark can provide valuable insights for speech model development and evaluation.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：当前多模态大语言模型（如 GPT-4o）已具备直接的语音交互能力，但缺乏专门、全面的基准来评估端到端语音大模型（SpeechLLM）在真实应用中的用户体验。现有的评估方法多为文本基准的简单改编，忽略了语音特有的特性与挑战，如**韵律**、**同音词**、**口吃**以及**用户期望的差异**。
- **整体含义**：该工作旨在填补这一空白，通过构建一个贴近真实语音对话场景的综合基准（WildSpeech-Bench），系统评估语音大模型的实际表现，从而推动模型优化与评估标准化。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：从真实世界的对话数据出发，系统性地构建一个覆盖语音特有现象的评估基准，并设计**查询感知评估方法**（query-aware evaluation）以提高自动评估的准确性。
- **关键技术细节**：
  - **数据构建**：系统收集真实世界的聊天数据，引入**说话人属性多样性**（如口音、年龄、性别）、**声学条件多样性**（如噪声、混响），并人工增强数据集中的**语音特定现象**（如同音词换用、韵律变异、口吃等）。
  - **评估方法**：设计查询感知的评估流程，针对每个测试样本生成**定制化评估检查表**（customized evaluation checklists）和**提示**（prompts），使自动评估模型（如 GPT-4 作为评判）能够根据具体查询场景更准确地打分，而非使用通用模板。

### 3. 实验设计：数据集 / 场景、Benchmark、对比方法
- **数据集 / 场景**：使用**真实世界聊天数据**（具体名称未在摘要中给出），覆盖多种语音对话场景，如日常交流、任务对话等，并包含说话人属性和声学条件的多样性。
- **Benchmark**：作者提出的**WildSpeech-Bench** 作为首个端到端语音大模型综合基准，包含上述数据集和评估方法。
- **对比方法**：对多种主流语音模型（具体模型名称未列出，但提及“various mainstream speech models”）进行了全面测试，包括直接语音交互的端到端模型和可能使用的音频管道方案。

### 4. 资源与算力
- **未明确说明**：摘要中未提及训练或评估所使用的 GPU 型号、数量、训练时长或推理资源。因此无法总结具体算力信息。若需进一步了解，需查看论文全文。

### 5. 实验数量与充分性
- **实验数量**：摘要提到进行了“comprehensive testing and detailed analysis”，但未给出具体实验组数（如多少个场景、多少组消融实验）。
- **充分性评估**：
  - **优点**：覆盖了多种语音特有现象，并对比了多个主流模型，具有一定广度。
  - **不足**：缺乏明确的消融实验（如验证数据增强、查询感知评估的独立贡献），也未提供统计显著性分析。因此，实验设计的充分性需要论文全文补充细节。

### 6. 论文的主要结论与发现
- 不同语音模型在**不同语音场景下性能差异显著**，说明仅用文本基准无法反映真实对话中的表现。
- **查询感知评估方法**能够实现更细粒度的评估，帮助揭示模型在韵律、同音词等特定挑战上的弱点。
- WildSpeech-Bench 为语音模型开发和评估提供了**有价值的见解**，推动面向真实场景的优化。

### 7. 优点（方法或实验设计的亮点）
- **首个综合基准**：系统性地关注语音特有挑战（韵律、同音词、口吃等），弥补了现有评估的盲点。
- **真实场景贴近性**：使用真实聊天数据并引入说话人属性与声学多样性，提高了生态效度。
- **查询感知评估**：通过定制化检查表和提示，使自动评估更适应具体查询，减少通用模板带来的偏差。
- **公开可用**：预计将公开基准，促进社区标准化评估（从元数据看，已收录于 ICLR 2026，虽被拒但已有影响力）。

### 8. 不足与局限
- **实验覆盖有限**：未公布数据集的具体规模、语言覆盖（是否仅限英语）、场景类型数量，可能对非英语或低资源语音场景泛化不足。
- **偏差风险**：真实聊天数据的收集方式、说话人分布可能存在偏差（如性别、地域不平衡），论文未说明如何控制。
- **自动评估的可靠性**：使用基于 LLM 的自动评判，其准确性与提示设计密切相关；目前未提供人工一致性验证或与其他评估方法（如 MOS）的对比。
- **算力与复现资源**：未公开实验所需的算力，不利于复现。
- **模型对比范围**：仅提及“主流语音模型”，未具体列出，难以评估对比的全面性。

（完）
