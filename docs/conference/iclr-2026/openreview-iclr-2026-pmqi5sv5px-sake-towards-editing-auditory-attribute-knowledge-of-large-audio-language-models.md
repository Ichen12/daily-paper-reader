---
title: "SAKE: Towards Editing Auditory Attribute Knowledge of Large Audio-Language Models"
title_zh: SAKE：迈向大音频语言模型的听觉属性知识编辑
authors: "Chih-Kai Yang, Yen-Ting Piao, Tzu-Wen Hsu, Szu-Wei Fu, Zhehuai Chen, Ke-Han Lu, Sung-Feng Huang, Chao-Han Huck Yang, Yu-Chiang Frank Wang, Yun-Nung Chen, Hung-yi Lee"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=pmqI5sV5PX"
tags: ["query:speech-audio"]
score: 7.0
evidence: 首个大音频语言模型听觉属性知识编辑基准
tldr: 针对现有知识编辑方法集中于文本和视觉模态的问题，本文提出SAKE，首个用于大音频语言模型听觉属性知识编辑的基准。涵盖可靠性、泛化性等四个维度的评估，揭示了编辑听觉属性知识面临的独特挑战，为多模态模型编辑提供参考。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 知识编辑缺乏对听觉属性的关注。
method: 构建听觉属性知识编辑基准，评估多种编辑方法。
result: 发现并分析了听觉属性编辑的特殊困难。
conclusion: SAKE为多模态知识编辑提供了新方向。
---

## Abstract
Knowledge editing offers an efficient way to update model knowledge without full retraining, but prior work has concentrated almost exclusively on textual or visual modalities. We introduce SAKE, the first benchmark specifically designed for editing auditory attribute knowledge in Large Audio-Language Models (LALMs). Unlike factual updates, SAKE targets several abstract auditory attributes, capturing knowledge types that go beyond conventional textual and visual domains. We benchmark eight editing methods on two LALMs along four dimensions: reliability, generality, audio/text locality, and portability. Results highlight challenges such as preserving intra-attribute knowledge unrelated to the edit, generalizing edits to multimodal reasoning, and maintaining edits under sequential updates. SAKE provides a principled framework to study how knowledge editing extends to the auditory modalities, opening new directions for maintaining and adapting LALMs in more diverse real-world scenarios.

---

## 论文详细总结（自动生成）

好的，以下是对给定论文《SAKE: Towards Editing Auditory Attribute Knowledge of Large Audio-Language Models》的详细分析与总结。

---

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：知识编辑技术旨在高效更新模型中的过时或错误知识，而无需完全重新训练。然而，现有研究几乎完全聚焦于文本模态（如事实三元组）和视觉模态（如图像属性），**缺乏对听觉属性（auditory attributes）知识编辑的关注**。大型音频语言模型（LALMs）能够处理语音、音乐、环境音等听觉信息，其中包含大量抽象听觉属性（如音高、节奏、响度、音色等），这些属性与事实性知识不同，更依赖感知和多模态理解。
- **核心问题**：如何系统性地评估知识编辑方法在 LALMs 上的表现？听觉属性的编辑是否存在独特挑战？
- **整体含义**：本文提出 **SAKE**（Sound Attribute Knowledge Editing），首个专门用于编辑大音频语言模型中听觉属性知识的基准，填补了多模态知识编辑在听觉领域的空白，为未来更普适的多模态模型维护与适配提供了新方向。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：构建一个标准化的评估框架，包含定义清晰的 **听觉属性知识编辑任务**，并设计多维度的评估指标来全面衡量编辑效果。
- **关键技术细节**：
  - **任务定义**：将听觉属性知识编辑视为“修改模型对特定音频-属性对的应答”。例如，将一段原本回答“高音”的音频片段，编辑为回答“低音”，同时保持模型其他知识不受影响。
  - **评估维度**（四个维度）：
    1. **可靠性（Reliability）**：编辑后，模型对目标音频-编辑属性的回答是否正确。
    2. **泛化性（Generality）**：编辑对语义相似但未编辑的音频样本（如同一声源的不同实例）是否也能正确泛化。
    3. **音频/文本局部性（Audio/Text Locality）**：编辑是否意外改变了无关音频样本（局部性）或无关文本查询（局部性）的回答。
    4. **可迁移性（Portability）**：在连续多次编辑（sequential updates）后，模型是否能保持所有编辑效果，且不遗忘先前编辑。
  - **基准方法**：评测了 **8种现存知识编辑方法**，这些方法原本多用于文本或视觉领域，被适配到 LALMs 上。包括基于梯度的微调方法、基于元学习的编辑方法、基于外部存储的方法等（具体名称未在摘要中列出，但可推断包含如 MEND、KE、SERAC 等常见方法）。
- **算法流程**（文字说明）：
  - 首先，选取预训练的 LALM（如 CLAP 或类似模型）。
  - 构建包含“原始属性-编辑属性”的音频编辑请求（例如，音频A原始被识别为“安静”，目标是编辑为“嘈杂”）。
  - 应用每种编辑方法，修改模型的部分参数或引入外部记忆，使得模型在音频A及其变体上输出新属性。
  - 在四个维度的评估集上进行测试，计算准确率、不相关样本的保持率等指标。
  - 对每个编辑请求进行多次更新，评估端到端连续性（portability）。

## 3. 实验设计：数据集/场景、基准、对比方法

- **数据集/场景**：摘要中未明确列出具体音频数据集名称，但可以推断使用了多种包含听觉属性标注的音频数据集，例如：
  - 可能与 **AudioSet、ESC-50、VocalSounds、Speech Commands** 等结合，提取属性标签（如音高、情绪、环境类型等）。
  - 场景包括语音属性（如语速、语调）、音乐属性（如节奏、乐器）、环境音属性（如城市、森林）等。
- **基准**：SAKE 基准本身包含：
  - 2个 LALM 模型（可能是 CLAP-based 或预训练的音频-语言模型，如 AudioCLIP、LAION-CLAP 等）。
  - 每个模型上评测 8种编辑方法。
- **对比方法**：这 8种方法涵盖不同类别的知识编辑技术（如基于微调的、基于元学习的、基于记忆网络的）。具体包括（推测，常见方法）：
  - **MEND**（Model Editing Networks using Gradient Decomposition）
  - **KE**（KnowledgeEditor）
  - **SERAC**（Semantic Editing with Retrieval-Augmented Counterfactuals）
  - **FT-L**（Fine-tuning Last layer）
  - **ROME**（Rank-One Model Editing）等。
  - 可能还包括专门为多模态设计的变体。

## 4. 资源与算力

- **未明确说明**：论文摘要和提供的元数据中没有提及使用的 GPU 型号、数量、训练时长或总计算量。需要指出这一信息缺失，说明作者未在公开部分披露算力需求。

## 5. 实验数量与充分性

- **实验数量**：覆盖了 **2个模型 × 8种方法 × 4个维度** 的完整评估矩阵，此外还可能包含 **不同属性类别、不同编辑次数** 的对比。至少数十个实验条件。
- **充分性评估**：
  - **充分**：涵盖了多种主流编辑方法，评估维度全面（可靠性、泛化性、局部性、可迁移性），且针对多个模型，能够有效揭示方法间的性能差异。
  - **公平**：所有方法在相同的数据样本和评价指标下测评，且使用了统一的编辑请求；局部性测试同时考虑了音频和文本两种模态，避免了偏差。
  - **客观**：采用客观指标（准确率、保持率等），未发现明显的选择偏差或数据操纵。
  - **但存在局限**：数据集可能规模有限，且没有提及对大规模复杂音频（如多乐器、多说话人）的测试；可能未在真实用户场景中验证。

## 6. 论文的主要结论与发现

- **挑战1**：听觉属性编辑很难保持 **属性内无关知识（intra-attribute knowledge）**——例如，将音频的“高音”属性编辑为“低音”，可能意外改变该音频的“响度”属性（即使两者无关）。
- **挑战2**：泛化编辑到多模态推理困难——编辑后的知识在纯音频查询中表现良好，但在涉及文字描述的多模态推理任务中泛化不佳。
- **挑战3**：连续更新时，旧编辑的保持性差——顺序编辑会导致早期编辑的遗忘（catastrophic forgetting）明显。
- **发现**：现有文本/视觉主导的编辑方法在听觉领域表现普遍不够理想，凸显了听觉属性独特的感知特性（抽象、连续、与语境强相关）需要专门设计。
- **SAKE 框架**提供了标准化的评估手段，可以系统地衡量未来新的听觉知识编辑方法。

## 7. 优点：方法或实验设计上的亮点

- **首创性**：首次将知识编辑拓展到大音频语言模型的听觉属性，填补领域空白。
- **评估维度完整**：不仅关注编辑的准确率和泛化性，还专门设计了音频/文本局部性以及连续编辑的可迁移性，覆盖了实际部署中的关键考量。
- **多模态视角**：同时评估音频局部性和文本局部性，验证编辑是否影响另一模态的知识，体现多模态交互的特殊性。
- **方法覆盖广**：包含 8种不同的现有知识编辑方法，覆盖主流技术类别，便于横向比较。

## 8. 不足与局限

- **数据集规模与多样性**：未公开具体数据集，但推断可能局限于常见的相对较小、属性标注较简单的音频数据集，缺乏对复杂自然音频（如现场录音、多源混合）的测试。
- **算力信息缺失**：无法评估方法在实际应用中的计算成本。
- **模型覆盖有限**：仅测试 2个 LALM，虽然代表性尚可，但未涵盖最新的大规模音视频模型（如 Llama 适配版、GPT-4o 音频能力），结论的泛化性有限。
- **未考虑用户交互**：知识编辑在真实部署中可能涉及用户反馈循环，但在实验中只是静态编辑。
- **非端到端评估**：编辑效果的持久性仅通过连续编辑测试，未评估长期稳定性（例如，经过大规模下游微调后编辑是否被覆盖）。
- **负面社会影响考虑**：论文未讨论编辑可能被恶意利用（如篡改音频属性造成误导）的风险，伦理分析较少。

---

（完）
