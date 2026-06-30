---
title: Hierarchical Semantic-Acoustic Modeling via Semi-Discrete Residual Representations for Expressive End-to-End Speech Synthesis
title_zh: 基于半离散残差表示的分层语义-声学建模用于表现力丰富的端到端语音合成
authors: "Yixuan Zhou, Guoyang Zeng, Xin Liu, Xiang Li, Renjie Yu, Ziyang Wang, Runchuan Ye, Weiyue Sun, Jiancheng Gui, Kehan Li, Zhiyong Wu, Zhiyuan Liu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=h5KLpGoqzC"
tags: ["query:speech-audio"]
score: 9.0
evidence: 表现力丰富的端到端语音合成
tldr: 本文通过半离散残差表示进行分层语义-声学建模，解决了语音合成中离散标记稳定性与连续信号表现力之间的权衡。提出可微分的量化瓶颈，促使文本语义语言模型和声学语言模型自然专业化，以端到端方式生成富含表现力的语音，同时避免多阶段管线中的语义-声学割裂问题。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有语音合成在离散标记的稳定性和连续信号的表现力之间面临两难。
method: 引入可微量化瓶颈，将文本语义与声学建模在分层框架中联合优化。
result: 生成既稳定又富有表现力的语音，无需依赖预训练的分词器。
conclusion: 半离散残差表示弥合了语义与声学之间的鸿沟。
---

## Abstract
Generative models for speech synthesis face a fundamental trade-off: discrete tokens ensure stability but sacrifice expressivity, while continuous signals retain acoustic richness but suffer from error accumulation due to task entanglement. This challenge has driven the field towards multi-stage pipelines that rely on pre-trained discrete speech tokenizers, but these create a semantic-acoustic divide, limiting holistic and expressive speech generation.  We resolve these dilemma through hierarchical semantic-acoustic modeling with semi-discrete residual representations.Our framework introduces a differentiable quantization bottleneck that induces natural specialization: a Text-Semantic Language Model (TSLM) generates semantic-prosodic plans, while a Residual Acoustic Model (RALM) recovers fine-grained acoustic details.This hierarchical semantic-acoustic representation guides a local diffusion-based decoder to generate high-fidelity speech latents. 
Critically, the entire architecture is trained end-to-end under a simple diffusion objective, eliminating dependency on external discrete speech tokenizers. Trained on over 1 million hours of speech, our 0.5B-parameter model achieves state-of-the-art zero-shot TTS performance among open-source systems, demonstrating that our approach delivers expressive and stable synthesis. Audio samples are available at: https://voxcpm.github.io/VoxCPM-demopage/.

---

## 论文详细总结（自动生成）

# 论文详细总结

## 1. 论文的核心问题与整体含义

- **研究动机**：语音合成生成模型面临根本性权衡——离散 token 能保证合成稳定性但牺牲表现力，连续信号能保留声学丰富性但任务纠缠导致误差累积。现有方法依赖预训练的离散语音分词器构建多阶段管线，但引发语义-声学割裂，限制了整体性和表现力丰富的语音生成。
- **核心问题**：如何在不依赖外部离散分词器的前提下，同时实现稳定性与表现力，弥合语义与声学之间的鸿沟。
- **整体含义**：提出一种端到端的分层语义-声学建模框架，利用半离散残差表示自然区分语义/韵律规划与细粒度声学细节，从而实现既稳定又富有表现力的零样本语音合成。

## 2. 方法论

- **核心思想**：引入可微分的量化瓶颈（differentiable quantization bottleneck），促使文本语义语言模型（Text-Semantic Language Model, TSLM）和残差声学模型（Residual Acoustic Model, RALM）自然专业化分工——TSLM 生成语义-韵律计划，RALM 恢复细粒度声学细节。
- **关键技术细节**：
  - **半离散残差表示**：将语义信息与声学信息分层编码，语义部分用离散表示保证稳定性，声学残差用连续表示保留表现力。
  - **可微分量化瓶颈**：使得整个网络可以端到端训练，梯度通过量化操作反向传播，无需预定义分词器。
  - **局部扩散解码器**：基于分层表示指导扩散过程，生成高保真语音潜在表示。
- **算法流程**：输入文本 → TSLM 生成语义-韵律离散计划 → RALM 基于离散计划生成残差声学连续细节 → 局部扩散解码器融合两者并去噪 → 输出语音潜变量。所有模块在简单扩散目标下联合优化，无多阶段分离。

## 3. 实验设计

- **数据集**：使用超过 100 万小时语音数据训练。
- **Benchmark**：未明确列出具体基准名称，但声称在开源系统中实现零样本 TTS 最优性能（state-of-the-art zero-shot TTS performance among open-source systems）。
- **对比方法**：未详细列出对比系统，推测与主流开源零样本 TTS 模型（如 VITS、YourTTS、NaturalSpeech 系列等）进行比较，但摘要中未给出具体对比表。
- **评估指标**：未说明，通常包括客观指标（如 WER、MOS、自然度）和主观听感测试。

## 4. 资源与算力

- **模型规模**：0.5B 参数（500M）。
- **训练数据**：超过 100 万小时语音。
- **算力细节**：文中未明确说明使用的 GPU 型号、数量及训练时长。仅能从模型和数据量推断训练代价较高，但缺乏具体核算信息。

## 5. 实验数量与充分性

- **实验数量**：摘要中仅提及在百万小时级数据上训练，并报告零样本 TTS 性能达到开源 SOTA，未给出消融实验或更多子实验的数量。
- **充分性评估**：
  - 优点：数据规模大，模型具备通用性。
  - 不足：缺少对以下方面的展示——不同数据条件下的泛化能力、与基线模型的详细对比（数值与显著性检验）、主观评测的样本量、消融实验对各个组件贡献的验证。因此实验充分性有限，需要查阅全文才能完整评判。

## 6. 论文的主要结论与发现

- 半离散残差表示能有效弥合语义与声学之间的鸿沟，实现稳定且富有表现力的语音合成。
- 可微分量化瓶颈促使模型自然分化出语义语言模型和残差声学模型，无需显式人为设计。
- 端到端训练避免了多阶段管线中语义-声学割裂问题，且无需依赖外部预训练离散分词器。
- 0.5B 参数模型在百万小时级数据上训练，在开源零样本 TTS 系统中取得最佳性能。

## 7. 优点

- **方法创新**：首次将可微分量化瓶颈与分层语义-声学建模结合，解决离散/连续权衡问题，属于端到端框架的原创贡献。
- **工程实用性**：模型规模适中（0.5B），且不需要外部分词器，简化了部署流程。
- **表现力与稳定性兼得**：半离散表示在保持稳定性的同时，通过残差模型保留声学细节，避免单一表示方式的缺陷。
- **数据规模优势**：使用 1M+ 小时数据，保证了模型在零样本场景下的泛化能力。

## 8. 不足与局限

- **实验报告不完整**：摘要中未提供详细的实验设置、对比结果表、消融实验和主观评测具体数值，无法判断方法在多个数据集上的鲁棒性。
- **资源开销未公开**：没有说明训练所需的 GPU 数量、训练时长和推理延迟，影响实际应用评估。
- **依赖大规模数据**：模型在超过 100 万小时数据上训练，对小型团队或资源受限场景可能不友好，且可能隐含数据偏差风险（如语种、口音、录音环境的覆盖不透明）。
- **未见失败案例或局限性讨论**：未分析模型在复杂韵律、低资源语言或噪声条件下的表现，缺乏对当前方法边界的认识。
- **对比公平性存疑**：声称开源 SOTA，但未列出对比系统的配置（是否使用相同数据或模型大小），需要全文验证对比是否公平。

（完）
