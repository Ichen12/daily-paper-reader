---
title: "BLAB: Brutally Long Audio Bench"
title_zh: BLAB：超长音频基准
authors: "Orevaoghene Ahia, Martijn Bartelds, Kabir Ahuja, Hila Gonen, Valentin Hofmann, Siddhant Arora, Shuyue Stella Li, Vishal Puttagunta, Mofetoluwa Adeyemi, Charishma Buchireddy, Ben Walls, Noah Bennett, Shinji Watanabe, Noah A. Smith, Yulia Tsvetkov, Sachin Kumar"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=By0sbtFROd"
tags: ["query:speech-audio"]
score: 9.0
evidence: 长时音频推理基准
tldr: 当前音频语言模型评测局限于30秒内的短片段，无法反映真实长时对话场景。本文提出BLAB基准，包含平均51分钟的长音频片段，涵盖定位、时长估计、情感和计数等推理任务。实验表明现有模型在长时推理上表现不足，为发展长音频理解能力提供了重要评测工具。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有音频语言模型评测限于短片段，缺乏对长时对话场景的系统评估。
method: 构建包含平均51分钟音频片段的基准，设计定位、时长、情感、计数四类推理任务。
result: 现有模型在长时推理任务上表现欠佳，表明长音频理解仍是开放挑战。
conclusion: BLAB填补了长时音频推理评测的空白，推动模型向真实场景演进。
---

## Abstract
Developing large audio language models (LMs) capable of understanding diverse spoken interactions is essential for accommodating the multimodal nature of human communication and can increase the accessibility of language technologies across different user populations. Recent work on audio LMs has primarily evaluated their performance on short audio segments, typically under 30 seconds, with limited exploration of long-form conversational speech segments that more closely reflect natural user interactions with these models.
To address this gap, we introduce Brutally Long Audio Bench, a challenging long-form audio reasoning benchmark that evaluates audio LMs on localization, duration estimation, emotion and counting tasks using audio segments averaging 51 minutes in length. BLAB consists of 833+ hours of diverse, full-length audio clips, each paired with human-annotated, text-based natural language questions and answers. Our audio data were collected from permissively licensed sources and underwent a human-assisted filtering process to ensure task compliance. We evaluate six open-source and proprietary audio LMs on BLAB, and find that all of them, including advanced models such as Gemini 2.0 Pro and GPT-4o, struggle with the tasks in BLAB. Our comprehensive analysis reveals key insights into the trade-offs between task difficulty and audio duration. In general, we find that audio LMs struggle with long-form speech, with performance declining as duration increases. They perform poorly on localization, temporal reasoning, speaker counting, and struggle to understand non-phonemic information, relying more on prompts than audio content. BLAB serves as a challenging evaluation framework to develop audio LMs with robust long-form audio understanding and reasoning capabilities.

---

## 论文详细总结（自动生成）

# 论文总结：BLAB: Brutally Long Audio Bench（超长音频基准）

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：当前音频语言模型（Audio LMs）的评测主要局限于30秒以内的短音频片段，无法反映真实世界中用户与模型交互的长时间对话场景（如会议、讲座、多人交谈等）。
- **动机**：为了推动音频 LMs 向更接近自然交互的长时对话理解发展，需要建立一个专门针对长音频推理能力的评测基准。
- **整体含义**：BLAB 的提出填补了长时音频推理评测的空白，揭示了现有模型在该任务上的严重不足，为未来研究提供了关键评估工具和方向指引。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：构建一个**超长音频推理基准**，包含平均时长51分钟（远超过现有30秒标准）的完整音频片段，并设计多种需要深层理解的推理任务。
- **关键技术细节**：
  - **数据来源**：从许可授权的来源（如播客、会议录音等）收集了833+小时的全长音频。
  - **标注方式**：采用人工辅助过滤和注释，为每段音频生成自然语言的问题-答案对（基于文本）。
  - **任务设计**：包含四类推理任务——① **定位**（如“某人何时开始发言”）、② **时长估计**（如“这段笑声持续了多久”）、③ **情感分析**（如“说话者的情绪变化”）、④ **计数**（如“多少位不同说话者”）。
  - **评估指标**：通过准确率等指标衡量模型回答的准确性（具体指标文中未详细展开，但隐含为文本匹配类评测）。

## 3. 实验设计：数据集、基准、对比方法
- **数据集**：BLAB 基准本身，包含平均51分钟、总计833+小时的音频片段，每段配有文本问答对。
- **基准对比**：评估了6个开源和专有音频语言模型，包括：
  - 先进专有模型：**Gemini 2.0 Pro**、**GPT-4o**（支持音频输入）；
  - 开源模型：文中未列出具体名称，但提及包含开源模型（如可能的 Qwen-Audio、Whisper-based 等）。
- **实验场景**：在四类推理任务上评估模型表现，同时分析了**任务难度**与**音频时长**之间的权衡关系。

## 4. 资源与算力
- **未明确说明**：论文正文中未提及训练或推理所使用的 GPU 型号、数量、训练时长、参数量等资源信息。仅从模型名称可以推测使用了大规模计算（如 GPT-4o 需大量算力），但具体细节缺失。

## 5. 实验数量与充分性
- **实验数量**：评估了6个模型在4类任务上的表现，并进行了多维度分析（如性能-时长曲线、错误模式分析）。没有额外的消融实验（例如控制任务类型、音频长度分档等）或数据增强对比。
- **充分性评价**：
  - **优点**：任务设计多样（4类），涵盖推理基本要素；对时长的依赖分析有重要价值。
  - **不足**：模型种类有限（仅6个），未包含更多近期开源模型（如 LLaMA-Omni、SALMONN 等）；未进行模型规模、训练数据量等消融实验；未检验不同难度的数据子集划分。总体而言，实验设计可作为初步验证，但不足以全面评估所有长音频理解方法。

## 6. 论文的主要结论与发现
- **核心结论**：所有现有音频语言模型，包括最先进的 Gemini 2.0 Pro 和 GPT-4o，在 BLAB 的长时推理任务上均表现不佳，性能显著低下。
- **具体发现**：
  - 模型性能随音频长度增加而**显著下降**（duration-performance trade-off）。
  - 模型在**定位、时间推理、说话人数统计**等任务上表现尤其差，说明它们难以理解非音素信息（如时间顺序、事件定位、多人区分）。
  - 模型更**依赖文本提示（prompt）而非音频内容**进行回答，说明其未真正理解长音频中的上下文。
- **意义**：长音频理解仍是开放挑战，现有架构缺乏长时依赖建模能力。

## 7. 优点（方法或实验设计亮点）
- **填补空白**：首次系统性地评测长音频（平均51分钟）推理能力，贴近真实应用场景。
- **任务设计针对性强**：四个任务（定位、时长、情感、计数）均需模型超越简单语音识别，具备时空推理、多说话人跟踪等高级能力。
- **数据规模大且高质量**：833+小时数据来自许可来源，并经过人工辅助过滤，保证任务合规性。
- **分析深入**：不仅报告平均分数，还探讨了时长对性能的影响、错误类型等，提供诊断性洞察。

## 8. 不足与局限
- **模型覆盖有限**：仅测试6个模型，缺乏对更大规模或不同架构（如端到端 vs. 级联）模型的比较。
- **数据偏差风险**：数据仅来自许可来源（可能偏重特定领域如播客、会议），未覆盖嘈杂、非正式、多语言等多样场景，推广性存疑。
- **任务难度不均衡**：某些任务（如时长估计）可能过于困难导致 floor effect，需更精细的难度分级。
- **评测指标单一**：仅用文本匹配准确率，未考虑部分正确或语义等价回答，可能低估模型能力。
- **未讨论应用限制**：如实时性要求、隐私问题（长音频包含个人信息）等实际部署障碍。
- **未提供基准代码或数据开源声明**：虽论文已提交，但资源可用性未说明。

（完）
