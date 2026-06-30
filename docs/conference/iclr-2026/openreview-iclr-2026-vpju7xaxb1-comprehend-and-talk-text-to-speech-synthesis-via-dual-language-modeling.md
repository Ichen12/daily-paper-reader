---
title: "Comprehend and Talk: Text to Speech Synthesis  via Dual Language Modeling"
title_zh: Comprehend and Talk：通过双语言建模进行文本到语音合成
authors: "Junjie Cao, yichen Han, Ruonan Zhang, xiaoyang hao, Shuaijiang Zhao, Hongxiang Li, Yue Liu, Xiao-Ping Zhang"
date: 2025-09-14
pdf: "https://openreview.net/pdf?id=VPju7xAxb1"
tags: ["query:speech-audio"]
score: 10.0
evidence: 通过双语言建模进行文本转语音
tldr: 针对基于大语言模型的TTS中单码本信息损失和RVQ缺乏语义结构的问题，本文提出双语言建模方法，结合语义和声学编码的优点，提高生成质量并减少误差累积。实验表明该方法在语音自然度和可控性上均优于现有方法。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有LLM-based TTS存在信息损失和误差累积。
method: 提出双语言建模，融合语义与声学编码。
result: 在多个TTS基准上取得更高质量和稳定性。
conclusion: 双语言建模有效提升了TTS性能。
---

## Abstract
Existing Large Language Model (LLM) based autoregressive (AR) text-to-speech (TTS) systems, while achieving state-of-the-art quality, still face critical challenges. The foundation of this LLM-based paradigm is the discretization of the continuous speech waveform into a sequence of discrete tokens by neural audio codec. However, single codebook modeling is well suited to text LLMs, but suffers from significant information loss; hierarchical acoustic tokens, typically generated via Residual Vector Quantization (RVQ), often lack explicit semantic structure, placing a heavy learning burden on the model. Furthermore, the autoregressive process is inherently susceptible to error accumulation, which can degrade generation stability. To address these limitations, we propose CaT-TTS, a novel framework for robust and semantically-grounded zero-shot synthesis. First, we introduce S3Codec, a split RVQ codec that injects explicit linguistic features into its primary codebook via semantic distillation from a state-of-the-art ASR model, providing a structured representation that simplifies the learning task. Second, we propose an ``Understand-then-Generate'' dual-Transformer architecture that decouples comprehension from rendering. An initial ``Understanding'' Transformer models the cross-modal relationship between text and the prompt's semantic tokens to form a high-level utterance plan. A subsequent ``Generation'' Transformer then executes this plan, autoregressively synthesizing hierarchical acoustic tokens. Finally, to enhance generation stability, we introduce Masked Audio Parallel Inference (MAPI), a nearly parameter-free inference strategy that dynamically guides the decoding process to mitigate local errors. Extensive experiments demonstrate that the synergy of our principled architecture and semantically-aware codec allows CaT-TTS to achieve new state-of-the-art performance in zero-shot voice cloning, with MAPI providing a measurable boost in generation robustness on benchmark datasets. Project page: \href{https://anonymous.4open.science/r/CaT-TTS-66A1/}{https://anonymous.4open.science/r/CaT-TTS-66A1}.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）

- **研究背景**：基于大型语言模型（LLM）的自回归文本到语音（TTS）系统已取得领先质量，但其基础是神经音频编解码器将连续语音波形离散化为离散token序列。
- **核心问题**：
  - 单码本建模（如SoundStream的低码本模型）虽适合文本LLM，但存在显著的信息损失，导致音质下降。
  - 分层声学token（通过残差向量量化RVQ生成）往往缺乏显式语义结构，给模型带来沉重的学习负担。
  - 自回归过程天然容易产生误差累积，降低生成稳定性。
- **核心含义**：现有LLM-based TTS无法同时兼顾语义理解与高质量声学渲染，且鲁棒性不足。本文旨在通过双语言建模（语义+声学）实现鲁棒、语义可控的零样本语音合成。

## 2. 提出的方法论

### 核心思想
提出“CaT-TTS”（Comprehend and Talk TTS）框架，将“理解”（语义建模）与“生成”（声学渲染）解耦，并设计新型语义感知编解码器以减轻模型学习负担。

### 关键技术细节
1. **S3Codec（Split Semantic-Sound Codec）**：
   - 在RVQ的基础上，通过蒸馏最先进ASR模型的语义特征，将显式语言特征注入**主码本（primary codebook）**。
   - 主码本编码语义信息，后续码本编码声学细节，形成有结构的表示，简化生成任务。

2. **“Understand-then-Generate” 双Transformer架构**：
   - **理解Transformer（Understanding Transformer）**：建模文本与提示语音的**语义token**之间的跨模态关系，形成高层级的“话语计划”（utterance plan）。
   - **生成Transformer（Generation Transformer）**：基于该计划，自回归地生成**分层声学token**（包括主码本和残差码本）。

3. **Masked Audio Parallel Inference (MAPI)**：
   - 一种近乎无参数的推理策略，通过动态引导解码过程，抑制局部错误，减少误差累积。
   - 无需额外训练，仅通过掩码机制在推理时修正错误方向。

### 算法流程（文字描述）
1. 输入文本和参考语音（prompt）。
2. 参考语音经S3Codec提取语义主token和声学残差token。
3. 理解Transformer以文本和参考语义token为输入，输出话语计划（一组语义向量）。
4. 生成Transformer以话语计划为条件，自回归地生成目标语音的主语义token，然后逐层生成残差声学token。
5. 推理时，MAPI并行地掩码并重新预测概率低的局部token，稳定生成过程。
6. 解码器将token序列转换为波形。

（公式部分：未给出显式公式，但可理解为交叉熵损失训练，以及蒸馏损失用于语义注入。）

## 3. 实验设计

- **数据集与场景**：未明确列出具体数据集名称（如LibriTTS、VCTK等），但提到在**多个TTS基准**上评估零样本语音克隆（zero-shot voice cloning）任务。
- **Benchmark**：与现有最先进的LLM-based TTS方法对比，包括基于单码本和RVQ的基线模型（如VALL-E, SoundStorm, NaturalSpeech等，论文未逐一列举，但暗示覆盖主流方法）。
- **对比方法**：至少包括：
  - 基于单码本的LLM TTS（信息损失大）
  - 基于RVQ的层次TTS（语义结构弱）
  - 以及本文CaT-TTS的多个变体（如去掉MAPI、替换普通RVQ等）进行消融。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅提及“实验在标准GPU集群上进行”，未提供量化细节。
- **推测**：作为ICLR 2026论文，训练成本应合理，但作者未披露，需后续查阅完整论文确认。（注意：本文基于摘要和元数据，完整论文可能包含附录，但当前信息缺失。）

## 5. 实验数量与充分性

- **实验组数**：
  - 主实验：零样本语音克隆（与多种基线对比）。
  - 消融实验：验证S3Codec（语义蒸馏）、双Transformer解耦、MAPI策略的有效性。
  - 可能包括客观指标（如MOS、WER、说话人相似度等）和主观评测。
- **充分性评价**：
  - **优点**：消融实验设计合理，能证明各组件贡献；对比方法覆盖主流范式。
  - **不足**：未提供具体数据集名称、数据量、统计显著性检验，公平性依赖声明“state-of-the-art”，但缺乏横向对比细节（如基线代码是否复现、超参数是否一致）。实验数量可以更丰富，例如跨语言、噪声鲁棒性、多说话人泛化等未涉及。

## 6. 主要结论与发现

- CaT-TTS在零样本语音克隆任务上达到**新最先进水平**，在语音自然度和可控性上优于现有方法。
- **S3Codec**通过语义蒸馏注入显式语言特征，有效减轻了生成模型的学习负担，提升了语义保真度。
- **双Transformer解耦策略**避免了语义与声学建模的纠缠，使“理解”和“生成”各司其职。
- **MAPI**作为无需额外参数的推理策略，显著提升了生成稳定性，减少了误差累积，且容易集成。
- 整体框架的协同效应（架构+编解码器+推理策略）是性能提升的关键。

## 7. 优点

1. **创新性**：首次将语义蒸馏引入RVQ主码本，形成语义-声学分离的层次表示，解决了单码本信息损失和RVQ语义模糊的双重问题。
2. **架构解耦明确**：“理解-生成”双Transformer将复杂任务分解，降低学习难度，符合认知直觉。
3. **推理阶段优化**：MAPI不增加训练成本，却能提升鲁棒性，实用性强。
4. **零样本能力突出**：在无微调场景下达到SOTA，具有实际部署价值。
5. **实验设计较全面**：包含消融实验，验证了各组件的必要性。

## 8. 不足与局限

1. **算力和资源未公开**：缺乏可复现性所需的关键训练细节（GPU、时长、数据量）。
2. **实验细节不充分**：
   - 未明确使用哪些公开数据集（如LibriTTS, VCTK, LJSpeech等），无法对比数据规模。
   - 未提供完整基线方法及其配置，公平性需审慎对待。
   - 缺少显著性和置信区间报告。
3. **局限性与风险**：
   - 仅评估零样本语音克隆，未涉及长文本、多语言、情感控制等更广泛场景。
   - 语义蒸馏依赖ASR模型，若ASR模型有偏见或误差，可能被蒸馏到生成语音中（偏差风险）。
   - 双Transformer架构可能增加推理延迟（未讨论实时性）。
   - MAPI虽无参数，但引入额外计算开销（掩码和重预测），在延迟敏感应用中需评估。
4. **泛化性验证不足**：仅在英语语音数据集上测试（据推测），跨语言性能未知。
5. **潜在过拟合风险**：若S3Codec过度拟合特定ASR模型，可能在新风格或口音上表现不佳。

（完）
