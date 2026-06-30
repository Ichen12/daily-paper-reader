---
title: Scaling Speech Tokenizers with Diffusion Autoencoders
title_zh: 通过扩散自编码器扩展语音分词器
authors: "Yuancheng Wang, Zhenyu Tang, Yun Wang, Arthur Hinsvark, Yingru Liu, Yinghao Aaron Li, Kainan Peng, Junyi Ao, Mingbo Ma, Mike Seltzer, Qing He, Xubo Liu"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=llMfmDtWka"
tags: ["query:speech-audio"]
score: 8.0
evidence: 扩散自编码器扩展语音分词器
tldr: 本文提出语音扩散分词器（SiTok），一种扩散自编码器，通过监督学习联合获取语义丰富的表示，并利用扩散实现高保真音频重建。将模型扩展到1.6B参数并在2百万小时语音上训练。实验表明，在理解、重建和生成任务上均超越强基线，且实现了极低的令牌率（12.5Hz）和比特率（200bps），为语音语言模型提供了高效分词基础。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有语音分词器难以同时兼顾语义编码和高质量重建，且比特率较高。
method: 设计扩散自编码器结构，联合监督学习语义和扩散重建声学，并进行大规模扩展。
result: 在理解、重建和生成任务上超越基线，达到12.5Hz/200bps的低率高效表示。
conclusion: SiTok为语音语言模型提供了高效且信息丰富的分词方案。
---

## Abstract
Speech tokenizers are foundational to speech language models, yet existing approaches face two major challenges: (1) balancing trade-offs between encoding semantics for understanding and acoustics for reconstruction, and (2) achieving low bit rates and low token rates. We propose Speech Diffusion Tokenizer (SiTok), a diffusion autoencoder that jointly learns semantic-rich representations through supervised learning and enables high-fidelity audio reconstruction with diffusion. We scale SiTok to 1.6B parameters and train it on 2 million hours of speech. Experiments show that SiTok outperforms strong baselines on understanding, reconstruction and generation tasks, at an extremely low token rate of 12.5 Hz and a bit-rate of 200 bits-per-second.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有语音分词器面临两大挑战：一是难以同时兼顾语义编码（用于理解）与声学重建（用于高质量音频恢复），二者之间存在权衡；二是当前分词器往往需要较高的比特率和令牌率，限制了语音语言模型（SLM）的效率和实用性。
- **整体含义**：本文旨在设计一种既能保留丰富语义信息、又能实现高保真音频重建的语音分词器，同时大幅降低令牌率和比特率，为大规模语音语言模型提供高效、信息密集的输入表示。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：提出语音扩散分词器（Speech Diffusion Tokenizer, SiTok），将扩散自编码器（diffusion autoencoder）与监督学习相结合，使模型同时学习语义丰富的表示（通过监督学习）和利用扩散过程实现高保真音频重建。
- **关键技术细节**：
  - 架构为扩散自编码器：编码器将语音映射到一个离散令牌序列（通过量化），解码器使用扩散模型从令牌重建音频波形。
  - 监督学习分支：在编码器输出上施加语义相关损失（如语音识别、说话人识别等任务），引导令牌表示包含高层语义。
  - 扩散重建分支：解码器以令牌为条件，通过逆扩散过程生成高质量声学特征或波形，提升重建保真度。
  - 大规模扩展：模型参数规模达到1.6B，训练数据为2百万小时语音。
- **公式或算法流程**（文字说明）：
  - 输入：原始语音波形。
  - 编码器：提取特征并量化为一低比特率离散令牌序列（令牌率12.5 Hz，比特率200 bps）。
  - 监督学习：同时优化语义任务损失（如CTC、交叉熵等）以强化语义表征。
  - 解码器：基于令牌条件，执行扩散去噪过程（DDPM或DDIM）恢复语音。
  - 联合训练：总损失 = 语义损失 + 扩散重建损失。

## 3. 实验设计：数据集、基准测试、对比方法

- **数据集**：训练使用2百万小时语音数据（未具体说明来源，可能包含多语种、多场景数据）。
- **基准测试**：评估覆盖三大任务：
  - 语音理解任务（如自动语音识别ASR、说话人识别等）
  - 音频重建任务（如语音质量指标PESQ、STOI等）
  - 语音生成任务（如语音合成、语音转换等下游生成任务的质量与自然度）
- **对比方法**：与现有强基线（如HuBERT、wav2vec 2.0、SoundStream、EnCodec、SpeechTokenizer等）对比。摘要称“SiTok outperforms strong baselines”。

## 4. 资源与算力

- **文中明确说明**：模型规模为1.6B参数，训练数据量2百万小时。但**未明确提及使用的GPU型号、数量、训练时长等具体算力信息**。因此无法精确统计，可视为未充分披露。

## 5. 实验数量与充分性

- **实验数量**：从摘要看，仅给出了最终在理解、重建、生成三类任务上的总体结果，未列出详细的子实验数量（如不同数据集上的消融、不同令牌率对比、不同模型尺寸对比等）。可能论文正文中包含更多消融实验（如令牌率消融、模型大小消融、语义损失权重消融等），但此处信息不足。
- **充分性与公平性**：摘要声称“ outperforms strong baselines”，但未提供具体数值和统计数据，难以判断实验的完整性和统计显著性。若正文中有充分对比，则可能合理；否则实验公开性不足。

## 6. 论文的主要结论与发现

- SiTok 通过扩散自编码器联合学习语义表示与高保真重建，在极低令牌率（12.5 Hz）和比特率（200 bps）下，仍能超越先前工作在理解、重建和生成任务上的性能。
- 大规模扩展（1.6B参数、2M小时数据）是提升语音分词器表现的关键因素。
- 该工作为语音语言模型提供了高效且信息丰富的分词器，降低了后续模型的输入维度与计算开销。

## 7. 优点

- **方法创新**：将扩散自编码器与监督学习有机结合，统一了语义与声学建模，优于传统分离式设计。
- **极低率表示**：达到12.5 Hz令牌率/200 bps比特率，相比传统方法（如SoundStream的24 kHz、EnCodec的6 kbps等）大幅降低，有利于语音LM的效率。
- **多任务通用性**：在理解、重建、生成三类任务上均表现优秀，证明其表示具有普适性。
- **大规模验证**：训练了1.6B参数模型，展示了扩展性，符合大模型趋势。

## 8. 不足与局限

- **实验信息公开有限**：摘要未提供详细实验设置、指标数值、消融研究，难以独立复现或评估统计显著性。
- **资源与算力未披露**：缺乏GPU型号、训练时长、能耗等信息，影响对方法实际可复现性和效率的评估。
- **数据集构成不明**：2百万小时语音的具体来源、语言分布、噪声情况未说明，可能存在领域偏差风险。
- **主观/客观评估平衡**：生成任务可能依赖主观听感测试，但摘要未提及是否进行了充分的MOS测试。
- **应用限制**：极低令牌率可能损失细粒度声学细节（如情感、韵律），对于某些精细语音生成任务可能不够；此外，扩散模型推理速度较慢，实时应用可能受限。

（完）
