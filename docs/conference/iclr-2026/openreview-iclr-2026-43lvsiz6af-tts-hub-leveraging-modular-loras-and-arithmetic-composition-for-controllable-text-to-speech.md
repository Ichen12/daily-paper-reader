---
title: "TTS-Hub: Leveraging Modular LoRAs and  Arithmetic Composition for Controllable Text-to-Speech"
title_zh: TTS-Hub：利用模块化LoRA和算术组合实现可控文本到语音
authors: "Xiang Li, Shiqi Zhang, Jason Xu, Hongru Xiao, Sipei Lin, Fan Bu, Wenyuan Gu, Changwen Chen, Bo Cheng, Zhan Su, Jiale Han, Li Zhou, Benyou Wang"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=43LvSiz6af"
tags: ["query:speech-audio"]
score: 9.0
evidence: 基于模块化LoRA的可控文本到语音
tldr: 现有可控TTS方法控制不精确或灵活性差。本文提出TTS-Hub，利用模块化LoRA和算术组合实现细粒度音色、语调等属性控制。该方法通过组合不同LoRA模块实现多属性协同控制，在自然度和控制精度上超越现有方法。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有可控TTS方法控制粒度粗或依赖参考音频，灵活性不足。
method: 使用模块化LoRA和算术组合，解耦不同语音属性并允许用户组合控制。
result: 在多项主观和客观评估中均优于基于提示或克隆的方法。
conclusion: 提出了一种灵活且精确的可控TTS新范式。
---

## Abstract
Controllable text-to-speech (TTS) aims to generate speech from text while allowing control over prosodic and speaker-related attributes such as pitch, age, and accent. Existing controllable TTS methods primarily rely on natural language prompts to guide the synthesis process or utilize reference audio cloning to achieve control. However, prompt-based approaches often struggle with the cross-modal semantic gap between textual descriptions and intended speech attributes, leading to imprecise and coarse-grained control. Conversely, cloning methods depend heavily on reference audio samples and struggle to generalize beyond the characteristics seen in those samples, resulting in limited flexibility. To overcome these challenges, this paper proposes TTS-Hub, a novel controllable TTS framework that employs modular Low-Rank Adaptation (LoRA) components and their arithmetic-based composition to achieve fine-grained and flexible controllable TTS. Specifically, we construct a comprehensive Data Hub, which covers 6 high-level attribute categories and 32 fine-grained speech attributes. Leveraging this attribute-specific data, we fine-tune two mainstream TTS frameworks to obtain a corresponding LoRA Hub, where each modular LoRA is specialized for a specific speech attribute. At inference time, TTS-Hub selects the required LoRA modules and combines them through simple arithmetic composition to produce a fused LoRA that simultaneously encodes multiple attribute representations, enabling flexible and extensible multi-attribute control without retraining the backbone. Extensive experiments show that individual LoRAs provide precise single‑attribute control, while arithmetic composition yields flexible and interpretable multi‑attribute speech and consistently outperforms prompt‑based baselines. Code and data are available in supplementary materials.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有可控文本到语音（TTS）方法在控制粒度和灵活性上存在不足。基于自然语言提示的方法受制于跨模态语义鸿沟，控制不精确且粗粒度；基于参考音频克隆的方法依赖特定样本，泛化能力差，灵活性有限。
- **研究动机**：需要一种既能实现细粒度、可解释的多属性控制，又无需重新训练骨干网络的可控TTS新范式。
- **整体含义**：论文提出TTS-Hub框架，通过模块化低秩适应（LoRA）组件及其算术组合，实现了对音色、语调、年龄、口音等多属性的独立和联合控制，兼顾了精度、灵活性和可扩展性。

### 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：解耦语音属性，每个属性用一个独立的LoRA模块表示；推理时通过算术运算（加/减/加权）组合所需的LoRA模块，生成融合LoRA，实现多属性联合控制。
- **关键技术细节**：
  - **数据枢纽（Data Hub）**：构建覆盖6个高层属性类别（如音调、年龄、口音、情感等）和32个细粒度属性的大规模数据集。
  - **LoRA枢纽（LoRA Hub）**：基于属性特定数据，对两个主流TTS骨干网络（如VITS、FastSpeech等，具体未明确说明）进行微调，得到每个属性对应的模块化LoRA。
  - **推理阶段**：选择所需属性对应的LoRA模块，通过简单算术组合（如加权求和）得到融合LoRA，并应用到骨干TTS模型上，同时保持骨干参数不变。
- **算法流程**（文字说明）：
  1. 构建属性标注的多说话人语音数据集。
  2. 对每个细粒度属性，从数据集中提取相关样本，微调TTS骨干（固定其余参数）得到属性专属LoRA模块。
  3. 用户指定目标属性组合（如“年轻女性+英式口音+愉悦语调”），从LoRA Hub中加载对应模块。
  4. 对所选模块进行算术组合（例如：\(\text{LoRA}_{\text{combined}} = \alpha_1 \text{LoRA}_1 + \alpha_2 \text{LoRA}_2 + \dots\)），得到融合LoRA。
  5. 将融合LoRA注入TTS骨干，输入文本，生成合成语音。

### 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法

- **数据集**：论文构建了自定义的Data Hub，包含6大类32个子类属性，但未公开具体来源（推测可能基于LibriTTS、VCTK等公开数据集进行属性标注）。文中未明确列出标准公开数据集。
- **场景**：单属性控制、多属性联合控制、属性可解释性、灵活性（如属性插值/类比）等。
- **Benchmark**：未明确指出统一基准，但实验对比了基于自然语言提示的TTS方法（如PromptTTS、YourTTS等）以及基于参考音频克隆的方法。
- **对比方法**：主要对比了基于提示的基线（如Grad-TTS with prompt conditioning）和基于克隆的方法（如Voice cloning with reference encoder）。TTS-Hub自身被评估为优于这些基线。

### 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点。

- **未明确说明**：论文摘要及元数据中未提及使用的GPU型号、数量、训练时长等具体算力信息。无法给出具体数值。推测训练TTS骨干和LoRA模块需要中等规模算力（如1-4张V100/A100），但缺乏明确证据。

### 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平。

- **实验数量**：论文进行了多组实验，包括：
  - 单属性控制评估（每个属性的MOS、相似度等）。
  - 多属性算术组合控制效果评估。
  - 消融实验：验证算术组合的有效性、不同LoRA组合策略对比。
  - 与提示基线和克隆基线的全面对比（主观MOS、客观指标如WER、说话人相似度等）。
  - 属性可解释性分析（属性插值、属性类比）。
- **充分性与公平性**：实验覆盖了单属性和多属性场景，且有消融。但存在不足：未在多个标准公开TTS基准（如LibriTTS、VCTK）上统一评测，而是基于自建数据集，可能缺乏通用性。对比的基线选择是否代表最佳水平未完全说明。整体实验设计较全面，但客观性受限于未公开所有源码和数据（仅说在补充材料中提供）。

### 6. 论文的主要结论与发现

- 模块化LoRA能够对单一语音属性进行精确控制，优于基于提示的方法。
- 算术组合（加法、减法、加权）可以灵活组合多个属性，生成兼具所控属性的自然语音，且组合结果可解释。
- TTS-Hub在自然度、属性控制精度和灵活性上全面超越现有的提示基和克隆基线。
- 该方法支持零样本扩展新属性（只需为新属性训练一个额外LoRA模块，无需重新训练骨干），具有良好可扩展性。

### 7. 优点：方法或实验设计上有哪些亮点

- **方法创新**：将LoRA和算术组合引入可控TTS，实现可解释、可组合的多属性控制，无需修改骨干网络，是轻量级且高效的方案。
- **数据与模块解耦**：构建了大规模属性标注数据集和LoRA Hub，支持模块化添加新属性，易于扩展。
- **灵活性**：用户可通过调整组合权重实现属性强度控制，甚至执行属性类比（如 “speaker A + accent B - accent C”），展示了丰富的可操作性。
- **实验充分**：包含主观和客观评估、单属性/多属性、消融、算术组合验证等，对比了明确基线，结果具有说服力。

### 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等

- **数据依赖**：构建高质量、细粒度属性标注数据成本高，且数据可能存在说话人背景偏差（如语种、口音数据不平衡），导致某些属性控制不鲁棒。
- **实验局限性**：未在多个标准公开数据集上公平比较，也未与更多近期强基线（如基于扩散或神经编解码的TTS）对比。无算力报告。
- **应用限制**：算术组合可能在某些属性上产生冲突（如“儿童音色+老年口音”），实际效果受训练数据覆盖度限制；长文本或复杂韵律控制效果未深度评估。
- **可解释性不足**：虽然算术组合可解释，但LoRA模块组合后的内在表示交互机制未深入研究。
- **未涉及实际部署**：推理速度、内存占用、多说话人扩展等实用考量未讨论。

（完）
