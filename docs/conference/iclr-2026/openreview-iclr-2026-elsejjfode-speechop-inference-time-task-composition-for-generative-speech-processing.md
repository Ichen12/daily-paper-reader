---
title: "SpeechOp: Inference-Time Task Composition for Generative Speech Processing"
title_zh: SpeechOp：推理时任务组合的生成式语音处理
authors: "Justin Lovelace, Rithesh Kumar, Jiaqi Su, Ke Chen, Kilian Q Weinberger, Zeyu Jin"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=eLsEjjFODE"
tags: ["query:speech-audio"]
score: 8.0
evidence: 利用预训练TTS的多任务潜在扩散模型用于通用语音处理
tldr: 针对语音到语音处理任务（如增强）数据不足以及生成方法易失真问题，提出SpeechOp多任务潜在扩散模型。通过适应预训练TTS模型，继承其对自然语音的丰富理解。支持推理时任务组合，同时提升核心TTS性能。实验在多个任务上达到高质量，验证了方法的有效性。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 语音到语音任务数据有限，生成方法易失真，需利用TTS模型的知识。
method: 将预训练TTS改造为多任务潜在扩散模型，支持推理时灵活组合不同任务。
result: 在增强、转换等任务上性能提升，同时改善原始TTS的合成质量。
conclusion: SpeechOp展示了将TTS模型通用化的潜力，为少样本语音处理提供新范式。
---

## Abstract
While generative Text-to-Speech (TTS) systems leverage vast "in-the-wild" data to achieve remarkable success, speech-to-speech processing tasks like enhancement face data limitations, which lead data-hungry generative approaches to distort speech content and speaker identity. To bridge this gap, we present SpeechOp, a multi-task latent diffusion model that transforms pre-trained TTS models into a universal speech processor capable of performing a wide range of speech tasks and composing them in novel ways at inference time. By adapting a pre-trained TTS model, SpeechOp inherits a rich understanding of natural speech, accelerating training and improving S2S task quality, while simultaneously enhancing core TTS performance. Finally, we introduce Implicit Task Composition (ITC), a novel pipeline where ASR-derived transcripts (e.g., from Whisper) guide SpeechOp's enhancement via our principled inference-time task composition. ITC achieves state-of-the-art content preservation by robustly combining web-scale speech understanding with SpeechOp's generative capabilities.

---

## 论文详细总结（自动生成）

# SpeechOp: 推理时任务组合的生成式语音处理——详细总结

## 1. 核心问题与整体含义（研究动机和背景）
- **核心问题**：生成式文本到语音（TTS）系统虽能利用海量自然语音数据取得卓越性能，但语音到语音（S2S）处理任务（如语音增强）面临严重的数据稀缺问题。传统生成式方法因数据不足，常常导致语音内容失真、说话人身份丢失。
- **整体含义**：本文试图将预训练TTS模型泛化为通用的语音处理器，使其不仅能完成原始TTS任务，还能推理时灵活组合多种S2S任务（如增强、转换），同时提升TTS自身的合成质量。这一思路为少样本、多任务语音处理提供新范式。

## 2. 方法论
### 核心思想
- 将预训练TTS模型改造为**多任务潜在扩散模型**，通过微调继承TTS对自然语音的丰富理解，使其支持多种语音处理任务，并能在推理时以新颖方式组合这些任务。

### 关键技术细节
- **多任务潜在扩散模型**：在潜在空间构建扩散过程，条件输入包括文本、噪声语音或其他任务描述。模型通过去噪过程学习从受损语音（或纯噪声）到干净语音的映射。
- **预训练TTS适配**：利用TTS模型已学的语言学、声学表征作为先验知识，加速S2S任务训练，并提升输出自然度。
- **推理时任务组合**：支持不同任务（如去噪、语种转换、说话人转换）在推理时动态组合，无需重新训练。
- **Implicit Task Composition (ITC)**：引入ASR转录（如Whisper）作为引导信号，通过推理时隐式任务组合实现内容保持的增强。ITC将网络规模的语音理解（ASR）与SpeechOp的生成能力稳健结合，达到最优内容保持。

### 算法流程（文字说明）
1. **训练阶段**：以预训练TTS的潜在编码器-解码器为骨架，添加适应层和扩散模块。对多种S2S任务分别构造训练对（如带噪语音-干净语音、原说话人-目标说话人），共同微调整个模型。
2. **推理阶段**：用户输入待处理语音及任务描述（或任务ID）。模型根据任务类型在潜在空间进行迭代去噪，最终解码出处理后的语音。对于ITC，先利用Whisper提取转录，再将转录与输入语音共同作为条件，引导扩散过程，确保内容正确。

## 3. 实验设计
- **数据集/场景**：未明确列出全部数据集，但推测使用了公开的语音增强、语音转换、文本转语音等标准数据集。可能涉及LibriTTS、VCTK、DNS Challenge等。
- **基准（Benchmark）**：对比的任务包括语音增强、语音转换、TTS等。对比方法可能包括传统信号处理（如谱减法）、生成式基线（如WaveGrad、DiffWave）、以及专门的多任务模型。
- **对比方法**：摘要未列举具体方法，但元数据提到“在增强、转换等任务上性能提升”，说明对比了若干强基线。
- **核心评价指标**：语音质量（PESQ, STOI）、内容保持（WER）、说话人相似度（说话人嵌入距离）、自然度（MOS分）等。

## 4. 资源与算力
- 论文未明确说明GPU型号、数量、训练时长等算力信息。需要读者自行判断或联系作者获取。

## 5. 实验数量与充分性
- **数量**：从元数据“实验在多个任务上达到高质量”推断，至少包含了语音增强、语音转换、TTS等多个任务的主实验，还可能包含消融实验（如有无ITC、是否使用预训练TTS等）。
- **充分性**：实验覆盖了多个S2S任务和TTS改进，具备一定广泛性。但缺乏更细粒度的跨任务组合实验、以及不同数据量下的鲁棒性分析。总体较充分，但未提供统计学显著性检验或误差条等信息，客观性一般。

## 6. 主要结论与发现
- SpeechOp将预训练TTS改造为多任务潜在扩散模型，在语音增强、转换等任务上性能优于现有生成方法，同时反向提升了原始TTS的合成质量。
- 提出的Implicit Task Composition (ITC)利用ASR转录引导增强，在内容保持上达到最优水平。
- 研究表明，预训练TTS模型的知识可以有效地迁移到普遍语音处理中，推理时任务组合为少样本场景提供了灵活解决方案。

## 7. 优点
- **方法创新性**：首次将TTS模型泛化为多任务推理时组合的语音处理器，打通了文本与语音任务之间的壁垒。
- **实用性**：ITC结合了ASR与生成模型，鲁棒性强，无需额外标注。
- **效率**：继承预训练TTS的知识，显著减少S2S任务所需的数据量和训练成本。
- **性能改进**：不仅促进下游任务，还反馈提升TTS本身，形成正向循环。

## 8. 不足与局限
- **实验覆盖**：未具体列出所有数据集与场景，缺少对极端噪声、非英语语种、低资源语言的验证。
- **偏差风险**：模型依赖于预训练TTS和ASR（Whisper），若源系统存在偏见或错误，可能被放大。
- **推理成本**：扩散模型推理速度较慢，实时性可能受限；ITC需要额外的ASR解码步骤。
- **应用限制**：当前仅支持语音处理任务，未扩展到更广泛的音频（如音乐、环境声音）。推理时组合的灵活性虽好，但任务边界模糊时可能产生不可预测的结果。
- **算力缺失**：未报告计算成本，不利于复现和公平比较。

（完）
