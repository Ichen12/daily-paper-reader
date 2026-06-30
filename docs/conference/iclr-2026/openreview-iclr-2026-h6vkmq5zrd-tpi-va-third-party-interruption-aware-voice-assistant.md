---
title: "TPI-VA: Third-Party Interruption-Aware Voice Assistant"
title_zh: TPI-VA：面向第三方打断感知的语音助手
authors: "Dongwook Lee, Heeseung Kim, Eunwoo Song, Che Hyun Lee, Sungroh Yoon"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=h6VKMq5ZrD"
tags: ["query:speech-audio"]
score: 8.0
evidence: 面向打断感知语音助手的大规模数据集与基准
tldr: 当前语音语言模型易受第三方打断干扰。本文构建了包含80K实例、覆盖26种打断场景的TPI-Train数据集，并设计TPI-Bench基准与两项互补评估指标，系统评测模型在打断下的响应策略与多说话人区分能力，为构建鲁棒语音助手提供标准评测工具集。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 语音助手在真实场景中易受第三方打断干扰，缺乏相应的训练与评测资源。
method: 构建80K实例覆盖26种打断场景的数据集TPI-Train，以及包含TPI-Test和Janus-Test的评测基准。
result: 实验揭示现有模型在打断场景下的不足，所提指标可有效衡量模型对打断的响应策略。
conclusion: TPI-VA为打断感知语音助手研究提供了全面的数据和评测框架。
---

## Abstract
While recent progress in Spoken Language Models (SLMs) has enabled increasingly natural voice-based interactions, they remain vulnerable to third-party interruptions (TPI). To address this challenge, we present a holistic framework for building and evaluating TPI-aware voice assistants. We first introduce TPI-Train, a large-scale dataset of 80K instances spanning 26 realistic interruption scenarios. For evaluation, we introduce TPI-Bench, which includes TPI-Test for measuring response strategies under interruptions and Janus-Test for probing whether models can distinguish true multi-speaker utterances from acoustically single-speaker yet textually misleading speech. To ensure reproducible and interpretable assessments, we also design two complementary metrics: Response Strategy Following (RSF) and Overall Helpfulness (OH). Experiments demonstrate that models fine-tuned with our approach achieve robust performance on TPI-Bench while preserving general dialogue capabilities on VoiceBench, effectively avoiding reliance on textual shortcuts. Human evaluations further confirm that both our dataset and trained models align with human preferences, establishing the first comprehensive solution for TPI-aware voice assistants. Our dataset will be publicly available, Demo samples: https://tpi-va.github.io/.

---

## 论文详细总结（自动生成）

基于提供的论文摘要和元数据，以下是对论文《TPI-VA: Third-Party Interruption-Aware Voice Assistant》的结构化、客观的详细中文总结。

## 1. 核心问题与整体含义（研究动机与背景）

- **研究背景**：近年来，语音语言模型（Spoken Language Models, SLMs）在自然语音交互方面取得了显著进展，但在真实场景中极易受到**第三方打断（Third-Party Interruptions, TPI）**的干扰。当前模型缺乏对打断情境的适应能力，且缺少针对性的训练数据和评测工具。
- **核心问题**：如何构建能够感知并合理应对第三方打断的语音助手，以及如何系统地评测这类模型的打断响应策略与多说话人区分能力。
- **整体含义**：本文首次提出了一套完整的框架，涵盖大规模数据集、标准化基准和互补性指标，旨在推动TPI感知语音助手的研究，提升语音助手在真实动态环境中的鲁棒性。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：通过构建包含多样化打断场景的大规模数据集（TPI-Train），设计专用的评测基准（TPI-Bench）和评估指标，使模型学会在打断情境下选择合适的响应策略，并具备区分真实多说话人语音与单说话人但文本误导性语音的能力。
- **关键技术细节**：
  - **TPI-Train数据集**：包含8万实例，覆盖26种真实的打断场景，作为模型微调的训练数据。
  - **TPI-Bench基准**：包含两个子测试集：
    - **TPI-Test**：用于衡量模型在打断下的响应策略。
    - **Janus-Test**：用于探测模型是否能区分真正的多说话人语音与“声学上为单说话人但文本上具有误导性”的语音（即避免依赖文本捷径）。
  - **评估指标**：设计了两个互补指标：
    - **Response Strategy Following (RSF)**：评估模型是否遵循正确的打断响应策略。
    - **Overall Helpfulness (OH)**：评估模型整体的有用性。
- **算法/流程**（文字描述）：
  - 首先，基于真实场景收集并构建TPI-Train数据集。
  - 然后，使用该数据集对语音语言模型进行监督微调。
  - 接着，在TPI-Bench上评估微调后模型的RSF和OH得分，并在VoiceBench上验证其通用对话能力。
  - 最后，通过人类评价验证模型行为与人类偏好的对齐程度。

## 3. 实验设计

- **使用的数据集/场景**：
  - **训练**：TPI-Train（80K实例，26种打断场景）。
  - **评测**：
    - TPI-Bench（含TPI-Test和Janus-Test）。
    - VoiceBench（用于评估通用对话能力）。
- **基准与对比方法**：
  - 论文未明确列出具体的对比基线模型名称，但通过实验证明了“模型使用TPI-Train微调后”相比未微调版本在TPI-Bench上表现更优。
  - 对比实验隐含在消融研究（如是否使用TPI-Train训练）以及与传统语音模型/语言模型的对比中。
- **人类评估**：额外进行了人类偏好评价，验证数据集和微调模型与人类判断的一致性。

## 4. 资源与算力

- **论文中未明确说明所使用的GPU型号、数量、训练时长等算力资源。** 摘要和元数据均未提及此信息，因此无法总结。需要指出这一缺失。

## 5. 实验数量与充分性

- **实验数量**：从摘要中可识别的主要实验包括：
  - 在TPI-Bench上的性能评估（TPI-Test和Janus-Test）。
  - 在VoiceBench上的通用对话能力评估。
  - 消融实验（推测，虽然未明确说明，但提到“模型通过我们的方法微调后…避免依赖文本捷径”，暗示有对比）。
  - 人类评价实验。
- **充分性与客观公平性**：
  - **优点**：覆盖了打断场景下的多种维度的评测（策略、多说话人区分、通用能力、人类偏好），且指标设计互补。
  - **不足**：论文摘要未提供具体的数值结果、置信区间或与多个基线模型的详细对比表格，难以完全判断实验的公平性。此外，未提及训练/测试集划分细节、数据偏差分析等。总体而言，实验设计较为全面，但呈现的细节有限。

## 6. 主要结论与发现

- **主要发现**：
  - 使用TPI-Train微调的模型在TPI-Bench上取得了鲁棒性能，同时未牺牲VoiceBench上的通用对话能力。
  - 模型能够有效避免利用文本捷径（即不依赖单说话人语音中误导性的文本内容来判断多说话人）。
  - 人类评价表明，所提出的数据集和微调模型与人类偏好对齐。
- **结论**：本文首次为TPI感知的语音助手提供了全面的训练数据和评测框架（数据集、基准、指标），为构建鲁棒的打断感知语音助手奠定了基础。

## 7. 优点（方法或实验设计上的亮点）

- **大规模场景覆盖**：TPI-Train包含80K实例、26种打断场景，数据规模与多样性在同类工作中领先。
- **双维度评测基准**：TPI-Test侧重响应策略，Janus-Test侧重多说话人区分能力，相互补充。
- **可解释性强的指标**：RSF和OH指标设计直观，便于结果复现和比较。
- **人类验证**：通过人工评价确认模型行为符合人类直觉，增强了结果的可信度。
- **开源承诺**：数据集将公开，有利于社区进一步研究。

## 8. 不足与局限

- **算力资源未报告**：无法评估方法的训练成本与可复现性。
- **实验细节缺失**：未提供基线模型的详细配置、训练超参数、消融实验的完整列表，以及统计显著性检验。
- **偏差风险**：数据集的26种打断场景可能未覆盖所有真实复杂情况（如多种语言、不同嘈杂环境、文化差异等），存在域外泛化风险。
- **文本捷径检测的局限性**：Janus-Test的设计可能无法完全排除其他形式的捷径（如声学线索）。
- **应用限制**：当前框架主要针对语音语言模型，对于端到端或模块化语音助手系统的适用性尚未探讨。
- **未说明语言范围**：数据集是否仅为英语？多语言场景下的适用性未知。

（完）
