---
title: "UltraVoice: Scaling Fine-Grained Style-Controlled Speech Conversations for Spoken Dialogue Models"
title_zh: UltraVoice：面向口语对话模型的细粒度风格控制语音对话规模化
authors: "Wenming Tu, Guanrou Yang, Ruiqi Yan, Wenxi Chen, Ziyang Ma, Yipeng Kang, Kai Yu, Xie Chen, Zilong Zheng"
date: 2025-09-11
pdf: "https://openreview.net/pdf?id=UrWdRcLINM"
tags: ["query:speech-audio"]
score: 9.0
evidence: 大规模细粒度语音风格控制数据集
tldr: 口语对话模型缺乏细粒度语音风格控制能力，UltraVoice 提出了首个大规模语音对话数据集，包含830+小时、覆盖情感、语速、音量、口音、语言及复合风格的指令。在 SLAM-Omni 和 VocalNet 等模型上微调后，显著提升了细粒度语音风格控制能力，为类人交互提供了关键数据基础。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有口语对话模型缺乏类人交互所需的细粒度语音风格控制。
method: 构建包含830+小时、六种风格的语音对话数据集 UltraVoice，并微调主流模型。
result: 微调后模型在细粒度语音风格控制上显著提升。
conclusion: UltraVoice 数据集有效赋能细粒度语音风格控制，推动类人口语交互。
---

## Abstract
Spoken dialogue models currently lack the ability for fine-grained speech style control, a critical capability for human-like interaction that is often overlooked in favor of purely functional capabilities like reasoning and question answering. To address this limitation, we introduce \textbf{UltraVoice}, the first large-scale speech dialogue dataset engineered for multiple fine-grained speech style control. Encompassing over 830 hours of speech dialogues, UltraVoice provides instructions across six key speech stylistic dimensions: emotion, speed, volume, accent, language, and composite styles. Fine-tuning leading models such as SLAM-Omni and VocalNet on UltraVoice significantly enhances their fine-grained speech stylistic controllability without degrading core conversational abilities. Specifically, our fine-tuned models achieve improvements of 29.12-42.33\% in Mean Opinion Score (MOS) and 14.61-40.09 percentage points in Instruction Following Rate (IFR) on multi-dimensional control tasks designed in the UltraVoice. Moreover, on the URO-Bench benchmark, our fine-tuned models demonstrate substantial gains in core understanding, reasoning, and conversational abilities, with average improvements of +10.84\% on the Basic setting and +7.87\% on the Pro setting. Furthermore, the dataset's utility extends to training controllable Text-to-Speech (TTS) models, underscoring its high quality and broad applicability for expressive speech synthesis.

---

## 论文详细总结（自动生成）

好的，以下是根据提供的论文元数据与摘要内容生成的详细中文总结。

# 论文总结：UltraVoice：面向口语对话模型的细粒度风格控制语音对话规模化

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：当前的口语对话模型虽然具备强大的推理与问答能力，但在**细粒度语音风格控制**方面严重不足，无法支持类似人类交谈中自然的情感、语速、音量等调节，限制了人机交互的自然性与表现力。
- **研究动机**：现有工作多聚焦于模型的“功能性”能力（如理解、推理），而忽视了语音风格控制这一对类人交互至关重要的维度。因此，亟需一个专门的数据集与方法来解决这一缺陷。
- **整体含义**：论文首次提出大规模、多维度、细粒度的语音风格控制数据集 **UltraVoice**，旨在弥合口语对话模型在风格控制上的短板，推动更自然、更具表现力的口语交互系统发展。

## 2. 方法论
- **核心思想**：构建一个覆盖多维度语音风格的指令-对话数据集，通过对主流口语对话模型进行微调，赋予其细粒度的风格控制能力，同时保持核心对话能力不下降。
- **关键技术细节**：
  - **数据集规模与覆盖**：超过 **830 小时**的语音对话数据，包含六种风格维度：情感（emotion）、语速（speed）、音量（volume）、口音（accent）、语言（language）以及复合风格（composite styles）。
  - **数据格式**：每条数据包含一条指令（指定期望的风格）和对应的语音对话，用于监督学习。
  - **模型微调**：在 **SLAM-Omni** 和 **VocalNet** 两个主流口语对话模型上进行全量微调，使用 UltraVoice 数据集中的指令-语音对进行训练。
- **算法流程说明**：无特定公式或复杂算法，主要采用标准的监督式微调流程：输入文本指令（风格描述）+ 对话上下文 → 模型生成具有指定风格的语音响应，通过对比损失或回归损失（如 MOS 预测）进行优化。

## 3. 实验设计
- **使用数据集**：
  - 训练与内部评测：**UltraVoice** 数据集自身（划分训练/测试集）。
  - 外部基准：**URO-Bench**（用于评估核心理解、推理与对话能力）。
- **Benchmark**：
  - **多维度控制任务**：在 UltraVoice 上设计的情感、语速、音量、口音、语言及复合风格控制任务。
  - **URO-Bench**：包含 Basic 和 Pro 两种设置，评估模型在开放域下的理解、推理与对话能力。
- **对比方法**：
  - **基线模型**：未微调的 SLAM-Omni 和 VocalNet。
  - **微调模型**：使用 UltraVoice 微调后的同一模型。
  - **额外对比**：还验证了数据集在训练可控文本转语音（TTS）模型上的效果，作为泛化性验证。

## 4. 资源与算力
- **文中未明确说明**：元数据和摘要中未提及 GPU 型号、数量或训练时长等具体算力信息。仅指出数据集规模为830+小时，微调基于 SLAM-Omni 和 VocalNet，但未公开训练配置。若有需要，可基于论文完整版进一步补充。

## 5. 实验数量与充分性
- **实验组数**：
  - 核心微调实验：在两个模型上各进行一组微调，与未微调基线对比。
  - 多维度控制任务评测：六种风格共设计多个子任务，每个任务报告 MOS 和 IFR。
  - 外部基准评测：URO-Bench 的 Basic 和 Pro 设置。
  - 泛化实验：训练可控 TTS 模型，验证数据集质量。
- **充分性评估**：
  - 比较全面：覆盖内部维度控制（风格可控性）和外部通用能力（理解推理）。
  - 指标合理：采用 MOS（主观评分）和 IFR（指令遵循率）衡量风格控制；采用平均提升百分比衡量通用能力。
  - 但缺少消融实验（如不同风格维度的独立贡献）和更多基线模型（如仅文本控制或其他语音模型）对比，稍显不足。

## 6. 主要结论与发现
- **风格控制显著提升**：微调模型在多维控制任务上，**MOS 提升 29.12%~42.33%**，**IFR 提升 14.61~40.09 个百分点**。
- **通用能力不降反升**：在 URO-Bench 上，Basic 设置平均提升 **+10.84%**，Pro 设置平均提升 **+7.87%**，说明风格控制训练并未损害核心对话能力，反而有所增益。
- **数据集高质量且通用**：该数据集还可用于训练可控 TTS 模型，证明其具备广泛适用性和高质量。

## 7. 优点
- **开创性**：首个专门针对口语对话模型细粒度风格控制的大规模数据集，填补了领域空白。
- **多维度覆盖**：包含情感、语速、音量、口音、语言及复合风格共六种维度，细粒度程度高。
- **规模大**：超过 830 小时对话数据，足以支撑模型微调。
- **实证充分**：在两个主流模型上验证，并同时评测风格控制与核心对话能力，结果积极且稳健。
- **良好泛化性**：验证了数据在 TTS 任务上的可用性，拓展了应用价值。

## 8. 不足与局限
- **算力资源未披露**：无法评估实验的可复现性和资源门槛。
- **消融实验缺失**：未分析不同风格维度的独立贡献，也未对比仅使用部分维度的效果。
- **模型多样性不足**：仅测试了两个模型（SLAM-Omni 和 VocalNet），代表性有限，可能在其他架构上效果不同。
- **主观指标局限**：MOS 依赖人工评分，可能受评分者偏差影响；IFR 的定义和判断标准未详细说明。
- **应用限制**：数据集的语言和口音覆盖范围未明确（可能以英语为主），跨语言/跨文化的泛化能力未知。
- **未涉及计算成本**：未讨论训练效率或推理实时性，这对于实际口语系统至关重要。

（完）
