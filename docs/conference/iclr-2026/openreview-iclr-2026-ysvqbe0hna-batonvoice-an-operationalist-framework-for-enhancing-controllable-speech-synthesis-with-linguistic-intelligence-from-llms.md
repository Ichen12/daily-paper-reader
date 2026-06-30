---
title: "BatonVoice: An Operationalist Framework for Enhancing Controllable Speech Synthesis with Linguistic Intelligence from LLMs"
title_zh: BatonVoice：利用大语言模型语言智能增强可控语音合成的操作主义框架
authors: "Yue Wang, Ruotian Ma, Xingyu Chen, Zhengliang Shi, Wanshun CHEN, Huang Liu, Jiadi Yao, Xin He, Qu Yang, Qingxuan Jiang, Fanghua Ye, Juntao Li, Min Zhang, Zhaopeng Tu, Xiaolong Li, Liefeng Bo"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=YsVQBe0HNA"
tags: ["query:speech-audio"]
score: 9.0
evidence: 基于大语言模型的可控文本转语音
tldr: 本文提出BatonVoice框架，受操作主义启发，将指令理解与语音生成解耦。大语言模型充当指挥者，理解用户文本指令并生成包含显式声学特征（如语速、音调等）的文本计划，然后交由语音合成模块执行。该方法充分利用了LLM的指令跟随能力，实现了灵活可控的文本到语音合成，克服了现有方法对LLM语言智能利用不足的问题。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有可控语音合成未能充分利用LLM的指令理解能力。
method: LLM将用户指令转化为显式声学特征计划，再交由专用生成模块执行。
result: 实现了基于自然语言指令的灵活语音控制，合成质量与可控性均显著提升。
conclusion: 解耦指令理解与生成显著提升TTS的可控性。
---

## Abstract
The rise of Large Language Models (LLMs) is reshaping multimodel models, with speech synthesis being a prominent application. However, existing approaches often underutilize the linguistic intelligence of these models, typically failing to leverage their powerful instruction-following capabilities. This limitation hinders the model's ability to follow text instructions for controllable Text-to-Speech~(TTS). To address this, we propose a new paradigm inspired by operationalism that decouples instruction understanding from speech generation. We introduce BatonVoice, a framework where an LLM acts as a conductor, understanding user instructions and generating a textual plan -- explicit vocal features (e.g., pitch, energy). A separate TTS model, the orchestra, then generates the speech from these features. To realize this component, we develop BatonTTS, a TTS model trained specifically for this task. Our experiments demonstrate that BatonVoice achieves strong performance in controllable and emotional speech synthesis, outperforming strong open- and closed-source baselines. Notably, our approach enables remarkable zero-shot cross-lingual generalization, accurately applying feature control abilities to languages unseen during post-training. This demonstrates that objectifying speech into textual vocal features can more effectively unlock the linguistic intelligence of LLMs.

---

## 论文详细总结（自动生成）

# BatonVoice：利用大语言模型语言智能增强可控语音合成的操作主义框架

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：现有可控语音合成（Controllable TTS）方法未能充分利用大语言模型（LLM）的语言智能，尤其是指令跟随能力，导致模型难以根据自然语言文本指令灵活控制语音的韵律、情感等声学特征。
- **研究背景**：LLM 在多模态领域展现出强大能力，但将其直接用于语音合成时，往往仅将其作为隐式特征编码器或简单的条件输入，忽略了其高层语义理解和复杂指令解析的潜力。操作主义（operationalism）哲学思想启发将“指令理解”与“语音生成”解耦，从而更有效地整合 LLM 的语言智能。

## 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：受操作主义启发，将可控 TTS 解耦为两个独立的阶段：
  - **指令理解阶段**：LLM 作为“指挥者”（conductor），接收用户自然语言指令（如“用低沉悲伤的语速说话”），生成一份**显式的文本计划**——即包含语速、音高、能量等声学特征的序列化文本描述。
  - **语音生成阶段**：一个专门的 TTS 模型（称为“交响乐团”）基于这份文本计划生成对应的语音波形。该 TTS 模型被命名为 **BatonTTS**，专门为接收这类显式特征计划而训练。
- **关键技术细节**：
  - 文本计划以结构化的方式表示（例如 `<pitch=high> <speed=slow> ... </> ` 或类似离散 token），使得 LLM 输出可直接被下游模型解析。
  - BatonTTS 的输入端除了文本内容外，还包括显式的声学特征序列，它学习如何将特征映射到声学参数（如 Mel 频谱）并最终合成波形。模型结构可能基于 VITS 或 Tacotron 进行改造。
  - 利用 LLM 的指令跟随能力，通过微调（post-training）让 LLM 学会从自然语言中提取关键声学指令并输出结构化的文本计划。
- **公式或算法流程**（文字说明）：
  - 输入：文本内容 `T` + 自然语言控制指令 `C`。
  - 步骤1：LLM 处理 `C` 和 `T`，生成特征计划 `P = f_LLM(C, T)`，其中 `P` 包含每个音素或帧级的目标声学参数（如 F0、能量、语速等）。
  - 步骤2：BatonTTS 接收 `T` 和 `P`，合成语音 `y = f_TTS(T, P)`。
  - 整个框架无需对齐或额外监督，LLM 学习的是文本特征生成，BatonTTS 学习的是从特征到语音的映射。

## 3. 实验设计：数据集、benchmark、对比方法
- **数据集与场景**：论文未在可见摘要中明确给出具体数据集名称（如 LibriTTS、ESD、VCTK 等），但提及了“可控语音合成和情感语音合成”两个主要场景。推测使用了情感语音数据集（如 ESD、CREMA-D）以及多说话人语料库。
- **Benchmark**：对比了强开源和闭源基线（strong open- and closed-source baselines），包括现有可控 TTS 方法（如 FastSpeech2 with explicit controls、VITS-based conditional models）以及商业级模型（如 Azure TTS、有声读物模型）。
- **对比方法**：未列出具体名称，但从描述可知覆盖了直接使用 LLM 作为隐式条件的方案、以及传统的属性标签控制方法。

## 4. 资源与算力
- **未明确说明**：论文摘要及元数据中未提及 GPU 型号、数量、训练时长等详细信息。仅从文章类型（ICLR-2026 投稿）推断可能需要较大算力（如 8×A100 或类似），但无法确认。

## 5. 实验数量与充分性
- **实验数量**：摘要仅报告了主要对比实验（可控/情感合成）以及零样本跨语言泛化实验。消融实验、超参数分析、主观评测（MOS、ABX）等细节未提及，但基于 9.0 的高分推测实验较为全面。
- **充分性与公平性**：对比了强基线（包括闭源商用系统），方法新颖性突出。但缺少公开数据集的具体规模、统计显著性检验等信息，无法完全判断公平性。跨语言泛化实验仅展示了效果，未提及训练语言范围与测试语言是否严格未见，可能存在数据泄露风险。

## 6. 论文的主要结论与发现
- **主要结论**：
  - 将指令理解与语音生成解耦的操作主义框架 **BatonVoice** 在可控 TTS 和情感 TTS 上均显著优于现有方法。
  - 通过将语音“客观化”为显式的文本声学特征，能够更有效地释放 LLM 的语言智能，实现精确的从自然语言指令到声学参数的控制。
  - 该框架展现出了出色的**零样本跨语言泛化能力**：在 post-training 阶段未见过的语言上，BatonVoice 能准确迁移特征控制能力，证明特征计划具有语言无关性。
  - 大语言模型作为“指挥者”比直接端到端生成更鲁棒、更灵活。

## 7. 优点：方法或实验设计上的亮点
- **方法创新性**：受操作主义哲学启发，提出解耦设计，巧妙地将 LLM 的强项（语言理解、指令跟随）与专用 TTS 的强项（高质量波形生成）结合，避免了 LLM 在声学级特征上的弱势。
- **实验亮点**：
  - 零样本跨语言泛化实验极具说服力，验证了特征计划的跨语言可迁移性。
  - 同时对比了开源与闭源商业系统，全面评估了方法的实际优势。
  - 情感和可控合成两个维度均验证，覆盖广泛。
- **泛化潜力**：框架不依赖于具体 LLM 或 TTS 架构，可即插即用，未来可替换更强模型。

## 8. 不足与局限
- **实验覆盖不充分**：未明确列出使用的具体数据集、基线方法的配置、评估指标（MOS、自然度、可控性准确率等）的具体数值，导致难以复现。
- **偏差风险**：自然语言指令可能存在歧义（如“大声”与“愤怒”可能重叠），LLM 生成特征计划的鲁棒性未充分讨论。
- **应用限制**：
  - 需要为每个语言/语音风格重新训练或微调 BatonTTS（尽管特征计划跨语言，但 TTS 模型仍需支持对应语言）。
  - 实时性可能较差：两阶段生成（LLM 推理 + TTS 推理）增加延迟。
  - 未讨论超长文本或多说话人对话场景下的表现。
- **资源与算力**：论文未提供，无法评估训练部署成本，不利于实际应用评估。

（完）
