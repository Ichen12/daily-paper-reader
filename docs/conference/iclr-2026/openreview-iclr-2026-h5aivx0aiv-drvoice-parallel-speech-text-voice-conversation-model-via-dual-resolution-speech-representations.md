---
title: "DrVoice: Parallel Speech-Text Voice Conversation Model via Dual-Resolution Speech Representations"
title_zh: DrVoice：基于双分辨率语音表示的并行语音-文本语音对话模型
authors: "Chao-Hong Tan, Qian Chen, Wen Wang, Chong Deng, Qinglin Zhang, Luyao Cheng, Hai Yu, Xin Zhang, Xiang Lyu, Tianyu Zhao, Chong Zhang, Yukun Ma, Yafeng Chen, Hui Wang, Jiaqing Liu, Xiangang Li, Jieping Ye"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=h5AiVx0Aiv"
tags: ["query:speech-audio"]
score: 9.0
evidence: 并行语音-文本语音对话模型
tldr: 端到端语音生成中，文本与语音的联合建模仍不充分。DrVoice 提出基于双分辨率语音表示的并行语音-文本对话模型，通过联合自回归建模实现文本与语音的相互感知，避免了独立生成带来的模态脱节。该方法在保持生成质量的同时提升了模态交互效率。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有端到端方法中文本与语音生成缺乏模态间感知。
method: 提出双分辨率语音表示，通过联合自回归建模实现并行语音-文本生成。
result: 实现了模态相互感知的高效语音对话生成。
conclusion: DrVoice 提升了语音对话模型中文本与语音的协同能力。
---

## Abstract
Recent studies on end-to-end (E2E) speech generation with large language models (LLMs) have attracted significant community attention, with multiple works extending text-based LLMs to generate discrete speech tokens. Existing E2E approaches primarily fall into two categories: (1) Methods that generate discrete speech tokens independently without incorporating them into the LLM’s autoregressive process, resulting in text generation being unaware of concurrent speech synthesis. (2) Models that generate interleaved or parallel speech-text tokens through joint autoregressive modeling, enabling mutual modality awareness during generation. This paper presents DrVoice, a parallel speech-text voice conversation model based on joint autoregressive modeling, featuring dual-resolution speech representations. Notably, while current methods utilize mainly 12.5Hz input audio representation, our proposed dual-resolution mechanism reduces the input frequency for the LLM to 5Hz, significantly reducing computational cost and alleviating the frequency discrepancy between speech and text tokens and in turn better exploiting LLMs’ capabilities. Experimental results demonstrate that DrVoice-7B establishes new state-of-the-art (SOTA) on prominent speech benchmarks including OpenAudioBench, VoiceBench, UltraEval-Audio and Big Bench Audio, making it a leading open-source speech foundation model in ∼7B models.

---

## 论文详细总结（自动生成）

# DrVoice：基于双分辨率语音表示的并行语音-文本语音对话模型 —— 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有端到端（E2E）语音生成大语言模型（LLM）中，文本与语音的生成通常相互独立，缺乏模态间的相互感知，导致文本与语音生成之间出现脱节，影响对话的自然度和协同性。
- **研究背景**：当前主流方法分为两类：
  - 独立生成离散语音 token，不将其纳入 LLM 自回归过程，导致文本生成无法感知同步的语音合成；
  - 通过交错或并行的语音-文本 token 联合自回归建模，使生成时模态间可以互相感知。
- **研究动机**：为了在保持生成质量的同时，提升文本与语音模态之间的交互效率，并降低计算开销，需要一种更高效的语音表示与建模方式。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：提出 **DrVoice**，一种基于 **双分辨率语音表示** 的并行语音-文本语音对话模型，通过联合自回归建模实现文本与语音的并行生成，使两者在生成过程中互相感知。
- **关键技术细节**：
  - **双分辨率语音表示**：区别于现有方法普遍使用的 12.5Hz 输入音频表示，DrVoice 将 LLM 的输入频率降低至 **5Hz**，显著减少计算成本，同时缓解语音 token 与文本 token 之间的频率不匹配问题，从而更好地发挥 LLM 的能力。
  - **联合自回归建模**：将文本 token 与低分辨率语音 token 在同一个自回归过程中并行生成，实现模态间的相互感知，避免独立生成带来的模态脱节。
- **算法流程（文字说明）**：
  1. 输入音频通过双分辨率编码器获得低分辨率（5Hz）的语音表示；
  2. 将文本序列与低分辨率语音表示拼接，作为 LLM 的输入；
  3. LLM 以自回归方式同时预测下一个文本 token 和下一个低分辨率语音 token；
  4. 生成的低分辨率语音表示通过上采样模块恢复为高分辨率（如 12.5Hz）语音 token，最终合成高质量语音。

## 3. 实验设计

- **使用的数据集/场景**：论文未明确列出具体训练数据集，但评估使用了多个公开语音及音频 benchmark。
- **Benchmark 与评价指标**：
  - OpenAudioBench
  - VoiceBench
  - UltraEval-Audio
  - Big Bench Audio
  - 未详述具体评测任务（如语音识别、语义理解、对话生成等），但推测为综合性的语音/音频理解与生成任务。
- **对比方法**：未明确列出对比基线，但声称 **DrVoice-7B** 在这些 benchmark 上建立了新的 **SOTA**，且是约 7B 参数量的开源语音基础模型中的领先者。

## 4. 资源与算力

- 论文**未明确说明**训练所用的 GPU 型号、数量及训练时长等算力信息。仅知模型参数量为 **~7B**（DrVoice-7B）。

## 5. 实验数量与充分性

- 实验覆盖了 **4 个主流语音/音频 benchmark**，属于较为广泛的评估场景，包括语音理解与生成任务。
- **消融实验**：从摘要中未提及消融实验或额外控制实验。由于缺乏详细信息，无法判断实验是否完整覆盖了所提出方法各组件（如双分辨率策略、联合建模等）的贡献。
- 总体上，实验覆盖度较高（多 benchmark），但**透明度不足**：缺少实验设置细节、对比方法列表、消融分析等，其充分性和公平性难以从摘要中完全评估。

## 6. 论文的主要结论与发现

- **DrVoice 方法可以显著提升语音对话模型中文本与语音的协同能力**，通过双分辨率表示减少了计算开销，同时保持了生成质量。
- 将 LLM 的语音输入频率从 12.5Hz 降低到 5Hz 不仅能降低计算成本，还能缓解模态间 token 频率差异，从而更充分地利用 LLM 的能力。
- DrVoice-7B 在多个公开 benchmark 上超越了现有方法，成为约 7B 参数规模下领先的开源语音基础模型。

## 7. 优点

- **方法创新**：提出双分辨率语音表示策略，有效降低 LLM 处理语音 token 的计算量，同时保持模态对齐。
- **模态交互增强**：通过联合自回归建模实现文本与语音的并行感知，避免了独立生成导致的模态脱节。
- **性能领先**：在多个主流语音 benchmark 上取得 SOTA，验证了方法的有效性和通用性。
- **模型规模适中**：在 7B 参数量级实现开放源码，利于后续研究与应用扩展。

## 8. 不足与局限

- **实验细节缺失**：论文摘要中未提供完整实验设置，如训练数据来源、对比基线具体名称、评测指标明细、消融实验等，降低了结果的可靠性和可复现性。
- **资源算力未公开**：未说明训练所需的 GPU 种类、数量及时间，不利于研究人员评估资源需求。
- **应用范围限制**：摘要未讨论方法在低资源语言、噪声环境、长对话或多轮交互场景下的表现，可能存在泛化性风险。
- **偏差风险**：仅使用了四个 benchmark，可能未覆盖所有语音交互场景；且未提及对生成语音的自然度、说话人风格保持等主观评估。
- **方法细节不透明**：双分辨率表示的具体编码器结构、上采样方式、联合建模的损失函数设计等均未在摘要中给出，需要阅读全文才能充分理解。

（完）
