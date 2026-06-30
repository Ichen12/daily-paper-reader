---
title: "MambaVoiceCloning: Efficient and Expressive Text-to-Speech via State-Space Modeling and Diffusion Control"
title_zh: MambaVoiceCloning：通过状态空间建模和扩散控制实现高效表达的文本到语音
authors: "Sahil Kumar, Namrataben Patel, Honggang Wang, Youshan Zhang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=0oXyMbPMtP"
tags: ["query:speech-audio"]
score: 9.0
evidence: 基于状态空间建模和扩散控制的TTS
tldr: 现有扩散TTS依赖注意力机制，计算成本高。本文提出MambaVoiceCloning，在推理时完全去除注意力，仅用状态空间模型进行文本、节奏和韵律条件控制。结合门控双向Mamba文本编码器和扩散控制，实现了线性时间推理，同时保持或提升合成质量。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有扩散TTS依赖注意力机制，计算效率低且不便于流式处理。
method: 使用完全基于状态空间模型的条件路径，包括门控双向Mamba文本编码器和可丢弃对齐教师的时域Bi-Mamba。
result: 在多个评估指标上达到或超越基于注意力的混合系统，且推理更快。
conclusion: 展示了全SSM条件路径在TTS中的可行性和优势。
---

## Abstract
MambaVoiceCloning (MVC) asks whether the conditioning path of diffusion-based TTS can be made fully SSM-only at inference—removing all attention and explicit RNN-style recurrence layers across text, rhythm, and prosody—while preserving or improving quality under controlled conditions. MVC combines a gated bidirectional Mamba text encoder, a Temporal Bi-Mamba supervised by a lightweight alignment teacher discarded after training, and an Expressive Mamba with AdaLN modulation, yielding linear-time $\mathcal{O}(T)$ conditioning with bounded activation memory and practical finite look-ahead streaming. Unlike prior Mamba--TTS systems that remain hybrid at inference, MVC removes attention-based duration and style modules under a fixed StyleTTS2 mel--diffusion--vocoder backbone. Trained on LJSpeech/LibriTTS and evaluated on VCTK, CSS10 (ES/DE/FR), and long-form Gutenberg passages, MVC achieves modest but statistically reliable gains over StyleTTS2, VITS, and Mamba--attention hybrids in MOS/CMOS, F$_0$ RMSE, MCD, and WER, while reducing encoder parameters to 21M and improving throughput by $1.6\times$. Diffusion remains the dominant latency source, but SSM-only conditioning improves memory footprint, stability, and deployability. 
Code available at: \url{https://github.com/sahilkumar15/MVC}.

---

## 论文详细总结（自动生成）

# 论文详细中文总结：MambaVoiceCloning

## 1. 核心问题与整体含义（研究动机和背景）

- **问题**：现有基于扩散的文本到语音（TTS）系统（如StyleTTS2）严重依赖注意力机制进行文本、节奏和韵律的条件控制，导致计算复杂度高（通常为二次复杂度 \(O(T^2)\)），推理效率低，且不便于流式处理。同时，已有的将状态空间模型（SSM）引入TTS的尝试（如Mamba-TTS）在推理时仍保留混合架构（SSM + attention），未能完全消除注意力。
- **动机**：探索能否在扩散TTS的条件路径中完全使用状态空间模型（SSM），在推理时移除所有注意力层和显式RNN风格循环层，同时保持或提升合成质量，实现线性时间复杂度的条件控制。

## 2. 论文提出的方法论

- **核心思想**：提出 **MambaVoiceCloning (MVC)**，一个完全基于SSM的条件路径架构，在推理阶段不包含任何注意力机制。通过门控双向Mamba文本编码器、可丢弃的时序对齐教师模型、以及带AdaLN调制的表达性Mamba模块，实现文本、节奏和韵律的高效条件控制。
- **关键技术细节**：
  - **门控双向Mamba文本编码器**：对文本序列进行前向和后向的Mamba处理，并通过门控机制融合信息，提取上下文感知的文本表示。
  - **时序Bi-Mamba + 可丢弃对齐教师**：在训练时，一个轻量级的对齐教师模型（基于attention）监督时序Bi-Mamba学习音素级别的时长对齐；在推理时，该教师模型被完全丢弃，仅用Bi-Mamba处理节奏信息，保持线性时间推理。
  - **表达性Mamba与AdaLN调制**：将韵律/风格控制通过自适应层归一化（AdaLN）调制到扩散过程的去噪网络中，全部使用SSM实现。
  - **整体流程**：文本 → 门控Bi-Mamba编码 → 时序Bi-Mamba（时长预测） → 扩散条件路径（含AdaLN） → 梅尔谱生成（基于StyleTTS2的mel-diffusion-vocoder骨干）。所有条件模块在推理时均不含注意力，实现 \(O(T)\) 时间复杂度。
- **公式/算法流程**（文字说明）：模型首先将输入文本经过门控双向Mamba得到文本隐状态；然后通过时序Bi-Mamba预测每帧对应的时长（由对齐教师蒸馏）；该时长用于扩展文本隐状态到帧级别；再将该帧级别表示与噪声输入一起送入扩散UNet（其中AdaLN模块通过Mamba实现条件调制），逐步去噪得到梅尔谱；最后通过神经声码器合成波形。

## 3. 实验设计

- **数据集**：训练使用 LJSpeech（单人英语）和 LibriTTS（多人英语）；零样本/跨语言评估使用 VCTK（英语多说话人）、CSS10 的三种语言（西班牙语 ES、德语 DE、法语 FR）、以及长文本Gutenberg段落。
- **基准方法**：主要对比 StyleTTS2、VITS、以及混合Mamba+attention的TTS系统。
- **评估指标**：
  - 主观：MOS（平均意见分）、CMOS（比较MOS）
  - 客观：F₀ RMSE（基频误差）、MCD（梅尔倒谱距离）、WER（词错误率）
  - 效率：编码器参数量（21M）、推理吞吐量（提升1.6倍）

## 4. 资源与算力

- 论文中未明确说明使用的GPU型号、数量及训练时长。仅提到编码器参数量缩减至21M，并指出扩散过程仍然是延迟的主要来源（即SSM条件路径本身占比较小）。具体算力信息缺失，无法进一步总结。

## 5. 实验数量与充分性

- **实验数量**：涉及多个数据集（3个英语多说话人、3个非英语单语言、1个长文本）、多项指标（主观+客观），并进行了消融（文中提到“modest but statistically reliable gains”暗示有统计检验）。但未明确列出所有消融实验组数。
- **充分性与公平性**：
  - 优点：覆盖单说话人、多说话人、跨语言、长文本场景，评估维度较全面。
  - 缺点：未在更大规模模型（如FastSpeech等）上对比；消融实验细节不足；仅与少量基线比较（缺少如Tacotron、Tacotron2等经典模型）。

## 6. 主要结论与发现

- MVC展示了完全基于SSM的条件路径在扩散TTS中的可行性和优势：在多个评估指标上达到或超越基于注意力的混合系统（StyleTTS2、VITS、Mamba-attention hybrid），编码器参数量降至21M，推理吞吐量提升1.6倍，且内存占用更稳定。
- 扩散过程本身仍是延迟主要来源，但SSM-only条件路径改善了整体内存占用、稳定性和部署友好性。

## 7. 优点（方法或实验设计亮点）

- **架构创新**：首次在扩散TTS推理阶段完全去除注意力，实现线性时间条件路径，便于流式处理和低延迟部署。
- **高效轻量**：编码器仅21M参数，吞吐量提升显著。
- **设计巧妙**：利用可丢弃对齐教师实现训练时的监督，推理时无额外开销；门控双向Mamba有效捕捉文本双向上下文。
- **评估全面**：覆盖多语言、多说话人、长文本，包含主观与客观指标，且进行了统计显著性检验。

## 8. 不足与局限

- **实验覆盖有限**：
  - 缺少与更多先进TTS系统（如NaturalSpeech系列、VALL-E）的对比。
  - 消融实验不够详细（未单独分析门控、双向、AdaLN各模块贡献）。
  - 仅在LJSpeech/LibriTTS训练，未在更大规模多说话人数据库（如VCTK本身作为训练集）上验证零样本能力。
- **扩散模块未优化**：论文承认扩散过程仍是主要延迟瓶颈，未尝试用SSM替换扩散UNet，因此整体延迟改善有限。
- **算力信息缺失**：无法评估训练成本。
- **潜在偏差**：代码开源但未提及是否采用固定随机种子、是否进行多次实验统计。CMOS等主观评价的参与者数量和方法未说明。
- **应用限制**：仅报告英文、西班牙语、德语、法语结果，缺乏对中文等声调语言的测试。

（完）
