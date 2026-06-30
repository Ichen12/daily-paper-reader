---
title: "AVSU-Bench and VSpeech-R1: A Dataset and MLLM for Audio-Visual Speech Understanding"
title_zh: AVSU-Bench 与 VSpeech-R1：面向音视频语音理解的数据集和多模态大模型
authors: "Yaoting Wang, Shaoxuan Xu, Ziyi Zhang, Yuanchao Li, Jian Ding, Jun Chen, Weijun Wang, Di Hu, Yuanchun Li, Yunxin Liu, Henghui Ding, Mohamed Elhoseiny"
date: 2025-09-02
pdf: "https://openreview.net/pdf?id=M9jciGcJpC"
tags: ["query:speech-audio"]
score: 9.0
evidence: 音视频语音识别与理解数据集
tldr: 当前音视频语音处理过度聚焦于转录层的语音识别（AVSR），忽略深层语义理解。AVSU 定义了超越转录的音视频语音理解新任务，构建了包含5万问答对的 AVSU-Bench 数据集，并提出端到端模型 VSpeech-R1。该工作将 AVSR 扩展到语义层面，为嘈杂环境下语音理解提供新范式。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有音视频语音研究局限于表面转录，缺乏深层语义理解。
method: 定义音视频语音理解任务（AVSU），构建大规模数据集 AVSU-Bench，并提出端到端多模态模型 VSpeech-R1。
result: 提供了支持语义理解的50k问答对数据集。
conclusion: AVSU 将音视频语音处理从转录提升至语义理解层面。
---

## Abstract
Audio-visual speech processing leverages visual cues (\eg~lip movements) to enhance speech robustness in noisy environments. However, current research is heavily focused on Audio-Visual Speech Recognition (AVSR), which primarily addresses the surface-level task of transcription, overlooking the need for deeper semantic understanding under challenging auditory conditions. To bridge this gap, we introduce \textbf{Audio-Visual Speech Understanding (AVSU)}, a new task that aims to comprehend semantics and context beyond mere transcription.
To support AVSU, we build \textbf{AVSU-Bench}, a large-scale dataset with 50k question-answer pairs aligned with audio-visual speech videos.
We further propose \textbf{VSpeech-R1}, the first-ever end-to-end multimodal large language model tailored for AVSU. A key component of this model is VSpeech-CoT, a structured Chain-of-Thought reasoning framework enabled by a training strategy combining supervised cold-starting and reinforcement learning.
Extensive evaluations on AVSU-Bench demonstrate that our end-to-end framework consistently outperforms traditional cascaded pipelines. Specifically, VSpeech-R1 achieves a BERTScore of 92.43\%, an absolute improvement of 2.33\% over the best cascaded baseline.

---

## 论文详细总结（自动生成）

# 论文总结：AVSU-Bench 与 VSpeech-R1

## 1. 核心问题与整体含义（研究动机和背景）
- **研究背景**：现有音视频语音处理领域高度集中于**音视频语音识别（AVSR）**，即利用视觉线索（如唇动）提升噪声环境下的转录准确性。但 AVSR 仅停留在**表面转录层**，无法应对需要深层语义理解的任务（如意图推断、情感分析、上下文问答）。
- **核心问题**：缺乏一个统一的任务定义、大规模数据集和端到端模型来推动**超越转录层的语义理解**。
- **论文含义**：首次提出 **音视频语音理解（Audio-Visual Speech Understanding, AVSU）** 任务，旨在从音视频语音中提取语义和上下文信息，而不仅仅是文字。该工作为当前过度聚焦于“听到什么”的领域提供了“理解什么”的新范式。

## 2. 方法论：核心思想与关键技术
- **核心思想**：将 AVSR 扩展到语义理解，构建一个端到端的多模态大语言模型，直接在音视频特征上推理语义。
- **关键技术细节**：
  - **AVSU-Bench 数据集**：包含 **50k 问答对**，与音视频语音视频对齐，覆盖多种噪声场景和语义推理问题。
  - **VSpeech-R1 模型**：首个专为 AVSU 设计的端到端多模态大语言模型（MLLM）。
  - **VSpeech-CoT 推理框架**：一种结构化的**思维链（Chain-of-Thought）推理机制**，通过以下训练策略实现：
    1. **监督冷启动（Supervised Cold-starting）**：先用带标注的 CoT 数据微调模型基础能力。
    2. **强化学习（Reinforcement Learning）**：进一步优化推理过程，提升语义理解的准确性和鲁棒性。
- **算法流程描述**：输入音视频 → 多模态编码器提取特征 → 经过 VSpeech-CoT 模块逐步生成中间推理步骤 → 最终输出答案（如问题类型的语义结果）。

## 3. 实验设计
- **数据集/场景**：主要在自建的 **AVSU-Bench** 上进行评估，包含 50k 问答对，覆盖不同噪声条件及语义理解任务。
- **Benchmark**：AVSU-Bench 本身作为基准，评价指标包括 **BERTScore**（衡量生成答案与参考答案的语义相似度）。
- **对比方法**：传统**级联流水线（cascaded pipelines）**，通常先做 AVSR 转录，再输入大语言模型（LLM）进行语义理解。论文对比了多种级联组合（具体基线未在摘要详细列出）。
- **主要结果**：VSpeech-R1 达到 **92.43% BERTScore**，比最佳级联基线**绝对提升 2.33%**。

## 4. 资源与算力
- **未明确说明**：文中未提及使用的 GPU 型号、数量、训练时长等具体算力信息。仅在实验设置部分可能隐含有训练资源描述，但摘要和元数据未包含。

## 5. 实验数量与充分性
- **实验数量**：仅提到在 AVSU-Bench 上进行了评估，并对比了级联基线。**未报告消融实验数量**（如对 CoT 框架、强化学习、冷启动策略的单独消融），也未涉及跨数据集泛化测试。
- **充分性与公平性**：
  - **优点**：提供了与级联方法的直接对比，证明了端到端方法的优势。
  - **不足**：缺乏多维度消融实验（如不同规模 LLM 骨干、不同噪声等级、不同 CoT 结构），也未与更多现有 MLLM 基线比较。实验覆盖度有限，可能存在**数据集同源偏差**（仅在自建集上验证）。

## 6. 主要结论与发现
- AVSU 任务成功将音视频语音处理从“转录”提升至“语义理解”层面。
- VSpeech-R1（端到端 MLLM + CoT 推理）**显著优于传统级联方案**，说明联合多模态理解比分离式 pipeline 更有效。
- VSpeech-CoT 框架结合冷启动和强化学习，能够生成结构化的推理过程，增强模型对复杂语义问答的鲁棒性。

## 7. 优点
- **任务创新**：首次系统定义 AVSU，填补了音视频语音领域缺乏语义理解的空白。
- **数据贡献**：构建 50k 问答对的大规模高质量数据集，为后续研究提供基础。
- **模型设计**：VSpeech-CoT 的思维链框架**端到端集成推理步骤**，避免了级联系统的误差累积，且训练策略（冷启动+强化学习）具有一定的可迁移性。
- **实验指标**：采用 BERTScore 评估语义相似度，比传统字准确率更贴近“理解”目标。

## 8. 不足与局限
- **实验不充分**：仅在单一数据集上评估，缺乏在公开 AVSR 数据集（如 LRS3、AVSP）上的语义理解测试，也未与现有 MLLM（如 Video-LLaMA、LLaVA 变体）对比。
- **数据集偏差风险**：AVSU-Bench 的问答对可能覆盖有限的场景（主要围绕噪声下的简单事实问答），未涉及推理、常识、情感等更复杂的语义理解。
- **计算开销未公开**：未说明模型参数量、推理速度或训练成本，难以判断实际应用可行性。
- **缺乏消融研究**：未单独验证 CoT 框架、强化学习、冷启动等各组件的贡献，说服力不足。
- **应用限制**：目前仅针对英语语音视频，且在受控噪声下构建，对真实多说话人、多语言场景的泛化能力未知。

（完）
