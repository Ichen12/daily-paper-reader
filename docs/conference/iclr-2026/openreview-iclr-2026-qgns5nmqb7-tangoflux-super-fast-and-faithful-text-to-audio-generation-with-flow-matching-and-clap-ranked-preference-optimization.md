---
title: "TangoFlux: Super Fast and Faithful Text to Audio Generation with Flow Matching and Clap-Ranked Preference Optimization"
title_zh: TangoFlux：结合流匹配和CLAP排名偏好优化的超快速忠实文本到音频生成
authors: "Chia-Yu Hung, Navonil Majumder, Zhifeng Kong, Ambuj Mehrish, Amir Zadeh, Chuan Li, Rafael Valle, Bryan Catanzaro, Soujanya Poria"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=qgNs5NmQB7"
tags: ["query:speech-audio"]
score: 9.0
evidence: 文本到音频生成，使用流匹配和偏好优化
tldr: 本文提出TangoFlux，一种高效的文本到音频生成模型，仅515M参数即可在3.7秒内生成30秒44.1kHz音频。关键贡献是CLAP排名偏好优化（CRPO），通过迭代生成和优化偏好数据来对齐文本-音频生成。实验表明CRPO生成的偏好数据集优于静态替代方案，TangoFlux在多项指标上达到最先进性能。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 文本到音频生成缺乏有效的对齐方法，偏好数据构建困难。
method: 提出CRPO框架，利用CLAP评分迭代生成和优化偏好对以训练流匹配模型。
result: 模型在A40 GPU上3.7秒生成30秒高质量音频，性能达SOTA。
conclusion: CRPO有效提升了文本到音频生成的对齐质量。
---

## Abstract
We introduce TangoFlux, an efficient Text-to-Audio (TTA) generative model with 515M parameters, capable of generating up to 30 seconds of 44.1kHz audio in 3.7 seconds on a A40 GPU. A key challenge in aligning TTA models lies in creating preference pairs, as TTA lacks structured mechanisms like verifiable rewards or gold-standard answers available for Large Language Models (LLMs). To address this, we propose CLAP-Ranked Preference Optimization (CRPO), a novel framework that iteratively generates and optimizes preference data to enhance TTA alignment. We show that the audio preference dataset generated using CRPO outperforms the static alternatives. With this framework, TangoFlux achieves state-of-the-art performance across both objective and subjective benchmarks. https://tangoflux.github.io/ holds the model-generated audio samples for comparison.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：文本到音频生成（TTA）模型在生成质量和效率上虽有进展，但面临关键的**对齐问题**——即如何使模型生成的音频更准确地匹配文本描述。与大型语言模型（LLM）有可验证的奖励或标准答案不同，TTA 缺乏结构化的对齐机制，且构建高质量的偏好数据对（preference pairs）尤为困难。
- **背景**：现有 TTA 模型多为扩散或自回归架构，但生成速度慢、参数规模大，且对齐方法尚未被充分探索。本文旨在提出一种**超快速、忠实、参数高效**的 TTA 模型，并针对对齐难题设计新的偏好优化框架。

## 2. 论文提出的方法论

- **核心思想**：提出 **CLAP-Ranked Preference Optimization (CRPO)** 框架，利用 CLAP 评分迭代地生成和优化偏好数据，从而对齐文本和音频生成。模型本身采用**流匹配（Flow Matching）** 作为生成范式，兼顾速度与质量。
- **关键技术细节**：
  - 模型：TangoFlux，仅 515M 参数，可生成最高 30 秒、44.1 kHz 的音频，在 A40 GPU 上只需 **3.7 秒**。
  - CRPO 框架：
    1. **迭代生成偏好对**：使用当前模型生成多个候选音频，通过 CLAP 模型（Contrastive Language-Audio Pretraining）计算文本-音频的相似度分数，将高分和低分样本配对形成偏好对。
    2. **优化**：利用这些偏好对训练流匹配模型，引入偏好损失（如 DPO 风格）来更新模型参数。
    3. **循环**：新一代模型再次生成候选，筛选更新偏好数据，如此迭代多次，逐步提升对齐质量。
  - 与静态偏好数据集不同，CRPO 生成的偏好数据是**动态、自适应**的，能随模型进化而改善。

## 3. 实验设计

- **数据集与场景**：论文未在摘要/元数据中明确指定使用哪些训练数据，但通常 TTA 任务会使用 AudioCaps、Clotho 等音频-文本数据集；可能还使用了大规模无监督音频数据预训练。具体待原文查询。
- **Benchmark**：使用**客观指标**（如 FAD、IS、CLAP score、KL 散度等）和**主观评测**（人类偏好判断）来评估生成质量。
- **对比方法**：包括 AudioLDM2、Tango、Make-An-Audio 等主流 TTA 模型。TangoFlux 在多项指标上达到 SOTA。

## 4. 资源与算力

- **明确说明**：模型在单个 A40 GPU 上生成 30 秒音频仅需 3.7 秒（推理阶段）。
- **未明确说明**：训练阶段的算力需求（GPU 数量、训练时间、显存使用等）在提供的摘要中未提及，需查阅完整论文获取。

## 5. 实验数量与充分性

- **主要实验**：
  - 对比实验：在多个客观/主观 benchmark 上与若干 SOTA 方法进行对比，验证性能优势。
  - 消融实验：验证 CRPO 框架的有效性，例如对比静态偏好数据集与 CRPO 动态生成的偏好数据，结果显示 CRPO 显著更优。
  - 可能还有不同迭代轮数、不同模型规模等实验。
- **充分性评价**：从摘要描述看，实验设计覆盖了客观和主观评价，并进行了关键对比（CRPO vs 静态数据），具有一定的充分性和公平性。但具体实验数量（如多少组消融、是否在多个数据集上验证）未明确，需参考完整论文。整体结论可信度较高。

## 6. 论文的主要结论与发现

- TangoFlux 以极小的参数规模（515M）和极快的推理速度（3.7秒生成30秒音频）实现了 SOTA 的 TTA 生成质量。
- CRPO 框架能够有效地生成优于静态选择的偏好数据，从而显著提升文本-音频对齐质量。
- 结合流匹配和 CRPO，TTA 模型在忠实度、音质和效率上取得了综合领先。

## 7. 优点

- **方法创新**：将偏好优化引入 TTA 领域，并创新的使用 CLAP 评分迭代生成偏好对，解决了偏好数据构建的核心难题。
- **效率突出**：参数仅为 515M，推理速度极快（3.7秒/30秒音频），适合实际应用。
- **结果全面**：在客观和主观指标上均达到 SOTA，且通过页面展示样本，可复现性强。
- **实验对比清晰**：明确证明了 CRPO 优于静态偏好数据方案。

## 8. 不足与局限

- **实验覆盖有限**：摘要中未列出具体使用的训练数据集和评测数据集，可能只在少数公开数据集上验证，泛化性有待进一步检验。
- **依赖 CLAP 评分质量**：CRPO 的有效性高度依赖于 CLAP 模型本身的文本-音频对齐能力，若 CLAP 存在偏差，则偏好数据可能引入噪声。
- **迭代计算成本**：CRPO 需要多轮生成和优化，可能带来额外的训练开销，但文中未讨论训练总成本。
- **应用限制**：模型生成的 44.1kHz 音频虽高保真，但仅能生成 30 秒以下片段，长音频生成未涉及。同时，文中未讨论多种类别（如音乐、音效、语音）的混合生成能力。
- **潜在偏差风险**：CLAP 模型和训练数据可能隐含文化或内容偏好，导致生成结果存在偏向性。

（完）
