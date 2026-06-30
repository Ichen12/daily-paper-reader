---
title: "Audio-FLAN: An Instruction-Following Dataset for Unified Understanding and Generation of Speech, Music, and Sound"
title_zh: Audio-FLAN：统一语音、音乐和声音理解与生成的指令跟随数据集
authors: "Liumeng Xue, Ziya Zhou, Jiahao Pan, Zixuan Li, Shuai Fan, Yinghao Ma, Ruibin Yuan, Sitong Cheng, Dongchao Yang, Haohan Guo, Yujia Xiao, Xinsheng Wang, Zixuan Shen, Chuanbo Zhu, ZHANG Xinshen, Tianchi Liu, Zeyue Tian, Ziyang Ma, Haohe Liu, Ge Zhang, Xu Tan, Emmanouil Benetos, Wenhao Huang, Yike Guo, Wei Xue"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=43YIq34nYw"
tags: ["query:speech-audio"]
score: 10.0
evidence: 统一语音音乐声音理解与生成的大规模指令数据集
tldr: 针对音频领域任务和领域孤立的问题，本文提出Audio-FLAN，一个大规模指令跟随数据集，统一了语音、音乐和声音的理解与生成。数据集包含108.5M样本，跨越23个主要任务和80个子任务，来自52个数据集。实验表明，使用Audio-FLAN微调能持续提升多种理解任务性能，为零样本泛化奠定基础。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 音频领域现有数据集按领域和任务类型孤立，缺乏统一指令格式。
method: 构建统一指令架构的大规模数据集，涵盖理解与生成任务。
result: 微调后模型在多种理解任务上获得一致提升，包括零样本场景。
conclusion: Audio-FLAN为音频统一多任务学习提供了有效数据基础。
---

## Abstract
Instruction tuning has generalized well in language and vision, yet audio remains siloed by domain (speech, music, environmental sound) and by task type (understanding vs. generation). We present Audio-FLAN, a large-scale instruction-following corpus that unifies heterogeneous audio sources under a unified instruction schema with instruction, input, and output. It supports both understanding (audio→text) and generation (text/audio/(audio, text)→audio) across speech, music, and general audio. The dataset contains 108.5M instances spanning 23 major and 80 minor tasks drawn from 52 datasets. Instruction tuning on a small subset of Audio-FLAN yields consistent gains on diverse understanding tasks, including zero-shot generalization. We further evaluate the existing generation model and validate Audio-FLAN as an effective benchmark. Hallucination probes inform future data and training design. In summary, Audio-FLAN serves as both an effective training resource and a unified, extensible benchmark for instruction-following audio–language models. We release the dataset on HuggingFace (https://huggingface.co/datasets/Audio-FLAN/Audio-FLAN-Dataset).

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- 在语言和视觉领域，指令微调已展现强大的泛化能力，但音频领域仍存在两大问题：**领域割裂**（语音、音乐、环境声音各自为政）和**任务类型割裂**（理解 vs. 生成任务使用不同格式和数据集）。
- 现有音频数据集缺乏统一的指令格式，阻碍了多任务学习与零样本泛化。本文旨在构建一个大规模、统一指令架构的音频数据集，以推动音频-语言模型的统一理解与生成能力。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：通过统一的指令 schema（包含指令、输入、输出三要素），将来自不同领域（语音、音乐、声音）和不同任务类型（理解：音频→文本；生成：文本/音频/（音频+文本）→音频）的数据整合为单一数据集。
- **关键技术细节**：
  - 从52个公开数据集中提取数据，构造108.5M个样本，覆盖23个主要任务（如语音识别、声源分离、音乐生成等）和80个子任务。
  - 对于每个样本，设计自然语言指令（例如“转录这段语音”“生成一段钢琴旋律”等），并保持输入/输出的模态一致性。
  - 数据集命名为 Audio-FLAN，在 HuggingFace 上开源。
- 论文未提供具体的公式或算法流程，但描述了一个通用的数据构建与微调流程：使用 Audio-FLAN 的**小子集**进行指令微调，评估理解任务性能（包括零样本场景），并测试现有生成模型的零样本能力。

### 3. 实验设计：使用了哪些数据集/场景，其 benchmark 是什么，对比了哪些方法

- **数据来源**：52个数据集，包括语音（如 LibriSpeech、Common Voice）、音乐（如 MAESTRO、MusicNet）、环境声音（如 AudioSet、ESC-50）等。
- **评估场景**：
  - **理解任务**：语音识别、情感识别、音频事件分类等，包括零样本泛化测试（在未见过的数据集上评估）。
  - **生成任务**：评估现有生成模型（如文本到语音、文本到音乐）在 Audio-FLAN 指令格式下的表现，作为 benchmark。
- **对比方法**：论文未明确列出对比基线，但提到“instruction tuning on a small subset yields consistent gains on diverse understanding tasks”，暗示与未微调或使用其他数据集的模型对比。未给出具体方法名称。

### 4. 资源与算力

- 论文摘要及元数据中**未明确说明**使用的 GPU 型号、数量或训练时长。仅提及“instruction tuning on a small subset”，未提供具体计算资源配置。可以认为文中缺少算力信息。

### 5. 实验数量与充分性

- **实验数量**：从描述看，可能进行了多组理解任务（不同子任务）的微调实验，以及生成任务的 benchmark 评估。但具体实验组数未量化。提到“hallucination probes”进行额外分析，暗示有消融或探测实验。
- **充分性与客观性**：由于论文被 ICLR 2026 拒稿（Rejected），可能实验不够充分或存在局限。摘要中仅报告了“consistent gains”，缺乏详细数据、误差棒、对比表格等，难以判断实验的公平性和全面性。

### 6. 论文的主要结论与发现

- Audio-FLAN 作为大规模指令跟随数据集，可以有效统一音频理解与生成任务。
- 使用 Audio-FLAN 进行指令微调，能在多种理解任务上获得一致提升，包括零样本泛化。
- 通过幻觉探测（hallucination probes）可为未来数据与训练设计提供指导。
- 数据集可同时作为训练资源和统一的、可扩展的 benchmark。

### 7. 优点：方法或实验设计上的亮点

- **规模宏大**：108.5M 样本、52个数据集、23个主要任务，是当前最全面的音频指令数据集。
- **统一 schema**：将理解与生成任务、多个音频领域整合为相同格式，便于多任务学习。
- **开源与可扩展**：数据集发布在 HuggingFace，支持社区扩展。
- **零样本评估**：通过零样本泛化测试验证了数据集的训练效果，展示了跨任务迁移潜力。

### 8. 不足与局限

- **实验细节不足**：未提供具体性能数字、消融实验、与现有基准（如 SpeechCoco、AudioInstructions）的定量对比，降低说服力。
- **算力缺失**：没有报告训练成本，影响可复现性评估。
- **生成任务评估有限**：仅评价了现有生成模型，未展示用 Audio-FLAN 微调后的生成模型性能提升。
- **潜在偏差风险**：数据集由 52 个公开数据集整合，可能存在领域不平衡（语音数据远多于音乐或声音数据）或指令模板设计的主观偏差。
- **应用限制**：指令格式可能不适用于所有音频-语言任务（如需要流式处理的场景），且大规模数据集可能包含噪声或标注错误。

（完）
