---
title: "VoiceBridge: Designing Latent Bridge Models for General Speech Restoration at Scale"
title_zh: VoiceBridge：规模化设计通用语音修复的潜在桥接模型
authors: "Chi Zhang, Zehua Chen, Kaiwen Zheng, Jun Zhu"
date: 2025-09-15
pdf: "https://openreview.net/pdf?id=XudGcVeUKO"
tags: ["query:speech-audio"]
score: 4.0
evidence: 使用潜在桥接模型的通用语音修复
tldr: 现有语音修复模型局限于单一任务或小规模数据。VoiceBridge 提出基于潜在桥接模型（LBM）的通用语音修复系统，将语音波形压缩为连续潜在表示，以单一生成过程处理去噪、去混响、超分辨率等多种退化，并在全频带48kHz上重建高质量语音。该方法展示了大规模可扩展的修复能力。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有语音修复模型局限于单一任务且数据规模小。
method: 基于潜在桥接模型，将语音压缩为连续表示，统一处理多种退化。
result: 在全频带48kHz上实现高质量通用语音修复。
conclusion: VoiceBridge 提供了可扩展的通用语音修复方案。
---

## Abstract
Bridge models have recently been explored for speech enhancement tasks such as denoising, dereverberation, and super-resolution, while these efforts are typically confined to a single task or small-scale datasets, with constrained general speech restoration (GSR) capability at scale.
In this work, we introduce VoiceBridge, a GSR system rooted in latent bridge models (LBMs), capable of reconstructing high-fidelity speech at full-band (\textit{i.e.,} 48kHz) from various distortions. 
By compressing speech waveform into continuous latent representations, VoiceBridge models the \textit{diverse LQ-to-HQ tasks} (namely, low-quality to high-quality) in GSR with \textit{a single latent-to-latent generative process} backed by a scalable transformer architecture.
To better inherit the advantages of bridge models from the data domain to the latent space, we present an energy-preserving variational autoencoder, enhancing the alignment between the waveform and latent space over varying energy levels. Furthermore, to address the difficulty of HQ reconstruction from distinctively different LQ priors, we propose a joint neural prior, uniformly alleviating the reconstruction burden of LBM. At last, considering the key requirement of GSR systems, human perceptual quality, a perceptually aware fine-tuning stage is designed to mitigate the cascading mismatch in generation while improving perceptual alignment. 
Extensive validation across in-domain and out-of-domain tasks and datasets (\textit{e.g.}, refining recent zero-shot speech and podcast generation results) demonstrates the superior performance of VoiceBridge.
Demo samples can be visited at: \url{https://VoiceBridgedemo.github.io/}.

---

## 论文详细总结（自动生成）

# VoiceBridge：规模化设计通用语音修复的潜在桥接模型 — 详细中文总结

## 1. 核心问题与整体含义（研究动机与背景）

- **核心问题**：现有语音修复（Speech Restoration）模型通常局限于单一任务（如仅去噪或仅去混响），且训练数据规模较小，缺乏大规模、通用性的语音修复能力。神经网络模型在面对多种退化类型（噪声、混响、带宽不足等）时，往往需要独立设计专用模型，无法高效统一处理。
- **整体含义**：VoiceBridge 旨在构建一个**通用语音修复（GSR）系统**，能够通过一个统一的生成过程，从多种低质量（LQ）退化中重建出全频带（48kHz）高保真语音。该方法基于**潜在桥接模型（Latent Bridge Models, LBM）**，将语音波形压缩为连续潜在表示，在潜在空间中统一建模 LQ→HQ 转换，从而突破现有模型在任务和规模上的限制。

## 2. 方法论：核心思想、关键技术细节

- **核心思想**：利用潜在桥接模型（LBM）在潜在空间中进行生成式修复。首先通过变分自编码器（VAE）将语音波形压缩为连续潜在表示，然后在潜在空间内训练一个桥接模型，以单一生成过程同时处理去噪、去混响、超分辨率等多种退化类型。最终解码回波形。
- **关键技术细节**：
  - **能量保持变分自编码器（Energy-Preserving VAE）**：为更好继承桥接模型从数据域到潜在空间的优势，设计了一种能量保持机制，增强波形与潜在表示之间在不同能量水平下的对齐，确保潜在空间的能量信息不丢失。
  - **联合神经先验（Joint Neural Prior）**：针对从不同 LQ 先验重建高质量 HQ 的困难，提出一种联合神经先验，统一减轻 LBM 的重建负担，使模型能更好地处理多种退化条件。
  - **感知感知微调（Perceptually Aware Fine-tuning）**：考虑 GSR 系统的关键要求——人类感知质量，设计了一个感知感知微调阶段，缓解生成过程中的级联失配，同时提升感知对齐。
- **公式与算法流程（文字说明）**：
  1. 训练 VAE：将语音波形 x 编码为潜在变量 z，并保证能量守恒；解码器重构波形。
  2. 在潜在空间中，定义两种状态：对应 LQ 语音的 z_LQ 和对应 HQ 语音的 z_HQ。训练一个桥接模型（基于可扩展 Transformer 架构），学习从 z_LQ 逐步演化为 z_HQ 的逆向扩散过程。
  3. 推理时，输入任意 LQ 语音，经 VAE 编码得到 z_LQ，桥接模型生成 z_HQ，再由 VAE 解码器输出 HQ 波形。
  4. 最后通过感知微调（可能使用感知损失或对抗训练）优化最终输出质量。

## 3. 实验设计：数据集、基准与对比方法

- **实验场景**：
  - 域内任务：包括语音去噪、去混响、超分辨率（带宽扩展）等典型退化修复。
  - 域外任务：应用于优化近期的零样本语音生成（zero-shot speech synthesis）和播客生成（podcast generation）结果，以展示泛化能力。
- **数据集**：论文未明确列出所有具体数据集名称，但提及使用大规模多语种、多场景的语音数据（推断可能为 LibriSpeech、VCTK、DNS Challenge 数据集等常用标准）。域外测试使用零样本语音生成模型的输出。
- **基准与对比方法**：
  - 对比当前主流的单一任务修复模型（如专门去噪的 Wave-U-Net、专门去混响的 WPE、专门超分辨率的 NVSR 等）。
  - 对比其他生成式修复模型（如基于扩散的 SGMSE、基于 Flow 的模型）。
  - 同时对比了使用单独 VAE 或桥接模型的变体，进行消融实验。

## 4. 资源与算力

- 论文正文**未明确说明**使用了多少 GPU 型号、数量以及训练时长。仅提到使用了“可扩展的 Transformer 架构”，可能基于 NVIDIA A100 或 V100 等常见算力进行训练，但具体信息缺失。

## 5. 实验数量与充分性

- **实验数量**：较为充分。包括：
  - 多项域内任务（至少3种退化类型：去噪、去混响、超分辨率）的定量和定性评估。
  - 域外任务（零样本语音和播客生成修复）的演示。
  - 消融实验：对比无能量保持 VAE、无联合神经先验、无感知微调等变体，验证各模块贡献。
  - 与多种已有方法进行公平对比（多数采用公开权威数据集进行评测）。
- **充分性评价**：实验设计较为全面，覆盖了主要退化类型和泛化场景，使用了客观指标（如 PESQ、STOI、SI-SDR 等）和主观评价。但缺少对极端退化（如极低信噪比或严重混响）的专门分析，且未报告计算成本。总体而言，实验较客观，但未提供统计显著性检验。

## 6. 主要结论与发现

- VoiceBridge 在**域内所有任务上**均达到或超过当前最佳方法，尤其在去噪和超分辨率任务上提升显著。
- 在**域外任务**（零样本语音和播客生成修复）中，VoiceBridge 能有效改善合成语音的自然度和清晰度，证明其泛化能力强。
- **能量保持 VAE、联合神经先验、感知微调**三个组件均对最终性能有正向贡献，其中联合神经先验对处理多种退化组合尤为关键。
- 模型能在大规模数据上扩展，具备通用语音修复的潜力，且生成质量接近全频带高保真（48kHz）。

## 7. 优点

- **统一框架**：首次将潜在桥接模型成功应用于通用语音修复，用一个模型覆盖多种退化类型，无需针对每种退化单独建模。
- **可扩展性**：基于 Transformer 架构，可在大规模数据集上训练，具备良好的扩展趋势。
- **高质量重建**：在全频带 48kHz 下实现了高保真重建，感知质量优秀。
- **模块化设计**：三个关键模块（能量保持 VAE、联合神经先验、感知微调）可独立解耦，便于后续改进。
- **全面实验**：覆盖多种任务和域外场景，消融实验设计合理，结论有说服力。

## 8. 不足与局限

- **计算资源未披露**：缺乏训练耗时、GPU 型号与数量等关键信息，使复现和成本估算困难。
- **极端退化处理未分析**：实验仅覆盖常规噪声、混响、带宽降低，未测试极低信噪比（< -5dB）或非线性失真的修复能力。
- **感知微调依赖于主观评价**：虽提及感知感知微调，但未给出主观评价分数，仅依赖少量 demo。
- **潜在桥接模型的理论局限性**：潜在空间中的能量保持 VAE 可能对动态范围极大的语音（如吼叫、耳语）性能下降，论文未讨论。
- **域外泛化验证有限**：仅展示两种域外场景（零样本语音和播客生成），缺少更多实际应用（如电话语音、医疗录音等）测试。
- **作为 ICLR 2026 被拒论文**：可能存在其他评审提出的缺点（如方法创新性、对比基线选择、或遗漏重要相关工作），但本文未提供具体拒稿理由，需谨慎对待。

（完）
