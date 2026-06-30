---
title: "Whisfusion: Parallel ASR Decoding via a Diffusion Transformer"
title_zh: Whisfusion：通过扩散变换器实现并行ASR解码
authors: "Taeyoun Kwon, Junhyuk Ahn, Taegeun Yun, Heeju Jwa, Yoonchae Choi, Siwon Park, Nam-Joon Kim, Jongchan Kim, Hyun Gon Ryu, Hyuk-Jae Lee"
date: 2025-09-20
pdf: "https://openreview.net/pdf?id=JCujsFnDS7"
tags: ["query:speech-audio"]
score: 9.0
evidence: 使用扩散变换器的并行ASR解码
tldr: 自回归ASR解码延迟随长度线性增长。Whisfusion 提出非自回归框架，冻结Whisper编码器并融合掩码扩散文本解码器，所有token并行更新。通过轻量交叉注意力适配器参数高效微调，在保持准确性的同时显著降低解码延迟，为实时ASR提供了高效方案。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 自回归解码延迟随语音长度线性增长。
method: 融合冻结Whisper编码器与掩码扩散文本解码器，实现并行解码。
result: 显著降低解码延迟且保持准确性。
conclusion: Whisfusion 为快速ASR提供了有效的非自回归解决方案。
---

## Abstract
Fast automatic speech recognition (ASR) is crucial for applications such as captioning and transcription. Although modern ASR encoders can process up to ~30 seconds of audio in a single pass, Whisper-style autoregressive (AR) decoders still generate tokens sequentially, making decoding latency grow linearly with utterance length. We propose Whisfusion, a non-autoregressive (NAR) ASR framework that fuses a frozen pre-trained Whisper encoder with a masked-diffusion text decoder. At each diffusion step, the decoder conditions on the full acoustic context and updates all tokens in parallel, mitigating the AR latency bottleneck while preserving Whisper-compatible generative structure. A lightweight cross-attention adapter trained via parameter-efficient fine-tuning bridges audio and text, and we introduce Parallel Diffusion Decoding (PDD), an ASR-tailored batch-parallel sampling scheme that improves the accuracy–latency trade-off in low-to-mid batch regimes. With 6.5k hours of training data, Whisfusion reaches 4.9\% WER on LibriSpeech test-clean, comparable to similarly sized Whisper model (Whisper-small at 5.0\%), while enabling much faster decoding. In particular, on 20–30s segments within Whisper’s 30s window, Whisfusion reduces decoding time from 674.7 ms to 80.7 ms (8.4× faster) at similar accuracy, demonstrating an efficient NAR operating point for Whisper-compatible ASR.

---

## 论文详细总结（自动生成）

# 论文详细总结：Whisfusion：通过扩散变换器实现并行ASR解码

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现代ASR编码器（如Whisper）可以一次性处理约30秒音频，但其解码器仍采用自回归（AR）方式逐个生成token，导致解码延迟随语音长度线性增长，成为实时应用的瓶颈。
- **研究动机**：为了在保持Whisper兼容的生成结构和高准确性的同时，大幅降低解码延迟，需要一种非自回归（NAR）解码方案。
- **整体含义**：本文提出Whisfusion，通过融合冻结的Whisper编码器与掩码扩散文本解码器，实现所有token并行更新，在保持与Whisper-small相当WER（4.9% vs 5.0%）的前提下，将20-30秒片段解码时间从674.7ms降至80.7ms（8.4倍加速），为实时ASR提供了高效的非自回归解决方案。

## 2. 论文提出的方法论

- **核心思想**：使用扩散模型（masked-diffusion text decoder）替代自回归解码器，在每一步扩散中，解码器以完整声学上下文为条件，并行更新所有token，从而消除AR延迟瓶颈。
- **关键技术细节**：
  - **冻结Whisper编码器**：保留预训练的Whisper编码器，不进行微调，以保持其强大的声学表示能力。
  - **掩码扩散文本解码器**：采用基于掩码的扩散过程，从完全掩码的序列开始，逐步去噪生成文本。
  - **轻量交叉注意力适配器**：通过参数高效微调（PEFT）训练一个轻量级交叉注意力模块，用于桥接音频和文本模态，实现音频条件注入。
  - **Parallel Diffusion Decoding (PDD)**：针对ASR定制的批量并行采样方案，在低到中等批量范围内改善了准确率与延迟之间的权衡。
- **算法流程**（文字描述）：
  1. 输入音频通过冻结的Whisper编码器提取声学特征。
  2. 初始化一个全掩码的文本token序列（长度预定义或动态确定）。
  3. 在多个扩散步骤中，解码器根据当前部分掩码的文本和声学特征（通过交叉注意力适配器），预测所有位置的概率分布，并逐渐替换掩码token为预测token。
  4. 经过预定的扩散步数后，得到完整文本序列。

## 3. 实验设计

- **数据集**：使用了LibriSpeech（test-clean、test-other），以及可能其他数据集（摘要未详细列出，但提到6.5k小时训练数据，可能包括多种来源）。
- **Benchmark**：主要对比Whisper-small（类似规模的自回归模型），以及可能其他基线（如标准AR Whisper变体）。
- **对比方法**：与Whisper-small在WER和解码时间上进行对比。未提及其他NAR ASR方法对比。
- **评估指标**：词错误率（WER）和解码延迟（毫秒）。特别关注20-30秒长音频片段的解码加速。

## 4. 资源与算力

- 论文中**未明确说明**使用的GPU型号、数量、训练时长等具体算力信息。仅提及训练数据量为6.5k小时，但未提供具体训练资源配置。

## 5. 实验数量与充分性

- **实验数量**：主要报告了LibriSpeech上的WER结果和不同长度片段的解码延迟对比。未提及消融实验数量（如不同扩散步数、批量大小等的影响）。
- **充分性**：实验设计能够直接展示Whisfusion在准确性与延迟上的优势，但与Whisper-small的对比较为直接。缺乏与更多NAR方法（如CTC、RNN-T等）的对比，也缺少在更多噪声环境或不同语言上的评估。消融实验不够详细（例如PDD方案的效果验证）。因此**充分性一般**，还需要更多维度的实验来验证泛化能力。
- **客观公平**：对比Whisper-small时，两者模型规模相似，但Whisfusion使用了冻结编码器+额外适配器，参数量可能略增，不过解码速度优势明显。延迟测量在同一硬件上对比，具有合理性。

## 6. 论文的主要结论与发现

- Whisfusion能够在不牺牲准确率（WER 4.9% vs 5.0%）的情况下，将长音频（20-30秒）解码延迟降低8.4倍（从674.7ms到80.7ms）。
- 非自回归扩散解码器与冻结Whisper编码器结合，通过轻量适配器实现高效跨模态融合，证明了参数高效微调在NAR ASR中的有效性。
- 提出的PDD采样方案在低到中等批量下改善了准确率-延迟权衡。
- Whisfusion为实时ASR（如字幕、转录）提供了可行的非自回归操作点。

## 7. 优点

- **方法创新**：将扩散模型引入ASR解码，替代自回归，实现并行生成，思路新颖。
- **效率显著**：在保持准确率的前提下实现8倍以上加速，对实时应用价值大。
- **参数高效**：通过PEFT仅训练轻量适配器，无需微调整个Whisper模型，节省计算资源。
- **结构兼容**：保留了Whisper的编码器，易于利用现有预训练模型和生态。
- **结果清晰**：直接展示了延迟-准确率权衡的关键指标。

## 8. 不足与局限

- **对比有限**：缺少与其他NAR ASR方法（如CTC、非自回归Transformer、Mask-CTC等）的系统对比，难以判断其在NAR流派中的相对地位。
- **数据集单一**：主要基于LibriSpeech评估，未覆盖嘈杂环境、多说话人、跨语言等挑战场景，通用性存疑。
- **训练数据量不透明**：虽然提到6.5k小时，但未说明具体来源和混合比例，实验可重复性受限。
- **未报告计算资源**：缺少GPU型号、训练时间等信息，阻碍对整体成本的理解。
- **扩散步数影响未深入**：未讨论不同扩散步数对质量与延迟的具体影响，以及是否引入额外推理延迟。
- **模型大小匹配问题**：对比的Whisper-small是AR模型，但Whisfusion额外使用了适配器，实际参数量可能不同，未明确比较模型大小。

（完）
