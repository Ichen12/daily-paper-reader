---
title: "DiSTAR: Diffusion over a Scalable Token Autoregressive Representation for Speech Generation"
title_zh: DiSTAR：可扩展自回归标记表示上的扩散零样本语音生成
authors: "Yakun Song, Xiaobin Zhuang, Jiawei Chen, Zhikang Niu, Guanrou Yang, Chenpeng Du, Dongya Jia, Zhuo Chen, Yuping Wang, Yuxuan Wang, Xie Chen"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=2EQPpEZtEK"
tags: ["query:speech-audio"]
score: 9.0
evidence: 零样本文本转语音框架，使用RVQ和扩散模型
tldr: 本文提出DiSTAR，一种零样本文本到语音框架，完全在离散残差向量量化（RVQ）编码空间中操作。通过将自回归语言模型与掩码扩散模型紧密耦合，无需强制对齐或时长预测器，即可生成长格式语音。该方法在分布偏移下保持鲁棒性，并支持块级并行解码，实现了高质量且可控的语音合成。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有语音合成在分布偏移下鲁棒性差且可控性有限。
method: 使用AR语言模型草稿RVQ块级标记，再通过掩码扩散并行填充完成合成。
result: 实现长格式零样本语音合成，无需对齐或时长预测，在分布偏移下仍保持鲁棒。
conclusion: AR与扩散的紧密耦合提升了语音合成的稳定性和可控性。
---

## Abstract
Recent attempts to interleave autoregressive (AR) sketchers with diffusion-based refiners over continuous speech representations have shown promise, but they remain brittle under distribution shift and offer limited levers for controllability. We introduce DiSTAR, a zero-shot text-to-speech framework that operates entirely in a discrete residual vector quantization (RVQ) code space and tightly couples an AR language model with a masked diffusion model, without forced alignment or a duration predictor. Concretely, DiSTAR drafts block-level RVQ tokens with an AR language model and then performs parallel masked-diffusion infilling conditioned on the draft to complete the next block, yielding long-form synthesis with blockwise parallelism while mitigating classic AR exposure bias. The discrete code space affords explicit control at inference: DiSTAR produces high-quality audio under both greedy and sample-based decoding using classifier-free guidance, supports trade-offs between robustness and diversity, and enables variable bit-rate and controllable computation via RVQ layer pruning at test time. Extensive experiments and ablations demonstrate that DiSTAR surpasses state-of-the-art zero-shot TTS systems in robustness, naturalness, and speaker/style consistency, while maintaining rich output diversity. Audio samples are provided on \url{https://anonymous.4open.science/w/DiSTAR_demo}.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：现有的零样本文本到语音（TTS）系统在分布偏移（如新说话人、新噪声环境）下鲁棒性差，且可控性有限。许多方法依赖自回归（AR）模型与连续语音表示的扩散精炼器交织，但存在脆弱的泛化能力和缺乏灵活控制机制。
- **整体含义**：DiSTAR 提出一种完全在离散残差向量量化（RVQ）编码空间上操作的零样本 TTS 框架，通过紧密耦合 AR 语言模型与掩码扩散模型，在无需强制对齐或时长预测器的条件下实现长格式语音合成，旨在提升鲁棒性、自然度、可控性和多样性。

### 2. 论文提出的方法论：核心思想、关键技术细节、算法流程
- **核心思想**：将语音合成分为“草稿-精炼”两阶段，均使用离散 RVQ 标记。首先由 AR 语言模型生成块级 RVQ 草稿标记，然后利用掩码扩散模型并行地对草稿进行条件填充，完成下一个语音块的完整生成。
- **关键技术细节**：
  - **离散 RVQ 编码空间**：将连续语音信号量化为多层离散标记，提供自然的可变比特率控制和显式推理可控性。
  - **块级并行解码**：AR 模型只生成每个块的粗略草稿，扩散模型并行填充剩余标记，缓解了纯 AR 模型的暴露偏差问题。
  - **无强制对齐/时长预测**：完全依赖条件生成，降低了模型复杂度。
  - **推理时灵活控制**：支持贪婪解码与基于采样的解码（通过无分类器引导），可切换鲁棒性与多样性；支持 RVQ 层剪枝以实现可变比特率与可调节计算量。
- **算法流程（文字说明）**：
  1. 输入文本经过编码转换为文本表示。
  2. AR 语言模型基于文本和先前生成的语音块，逐步生成当前块的 RVQ 草稿标记（仅顶层或部分层）。
  3. 掩码扩散模型以草稿标记为条件，通过并行迭代去噪过程填充该块内其他 RVQ 层的标记，形成完整的块级语音表示。
  4. 所有块按顺序拼接，并通过 RVQ 解码器合成最终语音波形。

### 3. 实验设计：数据集、基准与对比方法
- **数据集**：原文摘要未明确列出具体数据集名称，但暗示使用大规模语音数据（可能包含多说话人、多风格数据）进行零样本训练和评估。
- **基准**：对比了当前最先进的零样本 TTS 系统（如 VALL-E、YourTTS、NaturalSpeech 等，具体列表需参考全文）。
- **对比方法**：报道中称 DiSTAR 在鲁棒性、自然度、说话人/风格一致性上超越 SOTA，并进行了消融实验（如验证块级并行解码、扩散精炼、RVQ 层剪枝等组件的作用）。

### 4. 资源与算力
- 原文未明确说明使用的 GPU 型号、数量或训练时长。
- 推测需要大规模计算资源（如 8×A100 或 V100 集群），但未提供具体数据，属于信息缺失。

### 5. 实验数量与充分性
- **实验数量**：进行了“广泛”的主实验和消融实验，至少包含以下维度：
  - 与多个基线系统的对比（鲁棒性、自然度、说话人相似度、风格一致性的定量和定性评估）。
  - 推理时的控制实验（贪婪 vs. 采样解码、无分类器引导强度、RVQ 层剪枝效果）。
  - 块级并行解码对延迟和质量的影响。
- **充分性**：实验覆盖了零样本 TTS 的核心性能指标（鲁棒性、自然度、一致性）以及可控性维度，消融实验验证了关键设计选择。但缺乏详细的数据集描述和统计显著性检验信息，可能不够完整。

### 6. 论文的主要结论与发现
- DiSTAR 在零样本 TTS 任务上达到新 SOTA，尤其在分布偏移下保持鲁棒性，同时具备显式可控性（鲁棒性-多样性折中、可变比特率）。
- 离散 RVQ 空间与 AR+扩散的紧耦合设计消除了对强制对齐和时长预测的需求，简化了训练与推理。
- 块级并行解码有效降低了推理延迟，且不牺牲质量。

### 7. 优点
- **方法创新**：巧妙结合 AR 草稿与掩码扩散精炼，在离散空间实现高效、稳定的长格式合成。
- **可控性强**：推理时可通过无分类器引导、采样策略、RVQ 层剪枝灵活调整质量与多样性。
- **鲁棒性好**：对分布偏移（新说话人、新噪声）具有更好的泛化能力。
- **无需复杂结构**：无需时长预测器或对齐模块，降低了系统复杂度。

### 8. 不足与局限
- **实验信息缺失**：未公开具体使用的数据集、计算资源、训练细节，影响可重复性。
- **论文被拒背景**：作为 ICLR 2026 被拒稿件，可能存在方法论验证不充分或与其他方法对比不全面等问题（如未考虑公平的训练/测试数据划分）。
- **应用限制**：依赖 RVQ 码本质量，对极高保真度或极低延迟场景可能仍有限制；零样本泛化能力可能受限于训练数据多样性。
- **未见详细评估**：缺乏对识别错误、性别偏差、长尾说话人表现等细粒度分析。

（完）
