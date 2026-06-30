---
title: Revisiting Audio-language Pretraining for Learning General-purpose Audio Representation
title_zh: 重新审视音频-语言预训练以学习通用音频表示
authors: "Wei-Cheng Tseng, Xuanru Zhou, Mingyue Huo, Yiwen Shao, Hao Zhang, Dong Yu"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=2YSqaj725G"
tags: ["query:speech-audio"]
score: 7.0
evidence: CaptionStew音频-语言预训练数据集
tldr: 音频-语言预训练在通用音频表示方面仍不成熟。本文首次进行系统实证研究，识别三大障碍并构建大规模图文数据集CaptionStew（1070万条）。实验表明，精心设计的预训练目标能获得通用音频编码器，为后续研究奠定基础。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 音频-语言预训练缺乏系统研究，数据规模小、字幕多样性不足。
method: 构建大规模音频-文本数据集CaptionStew，并进行系统的预训练目标对比实验。
result: 揭示了不同预训练目标在多种音频任务上的表现规律，并证明了有效通用音频表征的可行性。
conclusion: 提供了首个系统性基准，推动音频-语言预训练领域发展。
---

## Abstract
Audio-language pretraining holds promise for leraning general-purpose audio representation, yet remains underexplored compared to its vision counterpart. 
Crucially, there is no consensus on whether audio–language models can build effective general-purpose audio encoders, nor a systematic understanding of how pretraining objectives behave across diverse audio processing tasks and scales.
We identify three key barriers: limited large-scale audio-text corpora, insufficient caption diversity, and lack of systematic exploration and evaluation.
To fill this gap, we present the first principled empirical study of audio–language pretraining.
To this end, we introduce CaptionStew, a 10.7M caption dataset aggregating diverse open-source audio-text corpora across multiple domains and captioning styles.
Using this resource, we conduct the first comprehensive evaluation comparing contrastive and captioning objectives for audio representation learning across speech, music, and environmental sound tasks.
Our results not only demonstrate that audio-language pretraining yields competitive, transferable representations, but also reveal critical trade-offs: contrastive learning offers superior data efficiency, while captioning exhibits better scalability.
Furthermore, we find that supervised initialization provides diminishing returns at scale, challenging common practices.
By grounding these claims in empirical evidence, we establish a viable pathway toward general-purpose audio representation learning, guiding future research. To accelerate progress, we will release data preparation recipes, training protocols, and pretrained models.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：音频-语言预训练在通用音频表示学习方面仍不成熟，远落后于视觉领域的同类技术。学界缺乏共识——即音频-语言模型能否构建有效的通用音频编码器，也缺乏对预训练目标在不同音频任务和规模下行为的系统理解。
- **研究动机**：作者识别出三大关键障碍：
  - 缺乏大规模音频-文本语料库；
  - 字幕多样性不足；
  - 缺乏系统的探索和评估。
- **整体含义**：本文旨在填补这一空白，通过首次系统的实证研究，为通用音频表示学习建立可行路径，推动该领域发展。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程

- **核心思想**：通过构建大规模、多样化的音频-文本数据集（CaptionStew），并系统比较对比学习（contrastive）与字幕生成（captioning）两类预训练目标在音频表示学习中的表现，揭示其规律与权衡。
- **关键技术细节**：
  - **数据集构建**：CaptionStew 包含 1070 万条字幕，聚合了多种开源音频-文本语料，覆盖多个领域（语音、音乐、环境声）和多种字幕风格。
  - **预训练目标**：
    - 对比学习：如 CLIP 式对比损失，强调不同模态间的对齐；
    - 字幕生成：如基于 Transformer 的自回归生成损失，强调文本重建。
  - **训练流程**：使用统一的预训练框架，在不同目标下训练音频编码器，并在下游任务中评估迁移性能。
- **公式/算法流程**（文字说明）：
  - 输入音频经过编码器得到表征，文本经过编码器得到表征；
  - 对比学习：计算音频-文本对之间的相似度，最大化正对相似度，最小化负对相似度；
  - 字幕生成：将音频表征作为条件，解码为文本序列，最小化交叉熵损失；
  - 最终将预训练好的音频编码器固定或微调，在下游任务中评估。

## 3. 实验设计

- **使用的数据集/场景**：
  - 预训练数据集：自建的 CaptionStew（1070 万条字幕）。
  - 下游评估任务：覆盖语音、音乐、环境声三大领域，具体包括：音频分类、检索、字幕生成等 benchmark（文中未列出具体名称，但提及“各种音频处理任务”）。
- **Benchmark**：未明确指定单一基准，但通过对比多种任务的性能来综合评价。
- **对比方法**：
  - 主要比较两类预训练目标：对比学习 vs. 字幕生成；
  - 同时对比了有监督初始化（如使用预训练任务标签）与无监督初始化；
  - 可能还包括不同数据规模下的表现。

## 4. 资源与算力

- 文中**未明确说明**使用的 GPU 型号、数量、训练时长等具体算力信息。仅在最后提到“将发布训练协议”，但未在摘要中展示细节。

## 5. 实验数量与充分性

- **实验数量**：摘要提到进行了“首次全面评估”，但未列出具体实验组数。推测至少包括：
  - 不同预训练目标（对比 vs. 字幕生成）在多个下游任务上的对比；
  - 不同数据规模（如从较小到1070万）的消融实验；
  - 有监督初始化与无监督初始化的对比。
- **充分性**：从“系统实证研究”、“揭示关键权衡”等表述看，实验设计较为全面，覆盖了主要维度。但缺少具体任务数量、重复次数、统计显著性等细节，公平性难以完全确认。

## 6. 论文的主要结论与发现

- **竞争性迁移能力**：音频-语言预训练能够产生有竞争力且可迁移的表示。
- **关键权衡**：
  - 对比学习在数据效率上更优（小数据时表现好）；
  - 字幕生成在可扩展性上更好（大数据时提升明显）。
- **有监督初始化收益递减**：随着预训练规模增大，有监督初始化的额外收益逐渐减少，挑战了常见做法。
- **可行性证明**：为通用音频表示学习建立了可行路径。

## 7. 优点

- **首次系统性研究**：填补了音频-语言预训练领域缺乏系统实证的空白。
- **大规模高质量数据集**：CaptionStew 聚合了多领域、多风格的字幕，解决了数据规模小、多样性不足的问题。
- **全面的评估维度**：覆盖语音、音乐、环境声三大领域，对比了两种主流预训练目标，实验结果揭示了重要规律。
- **开源承诺**：将发布数据制备脚本、训练协议和预训练模型，有利于后续研究复现和推进。

## 8. 不足与局限

- **实验细节缺失**：摘要中未提供具体的下游任务列表、评估指标、模型架构（如编码器尺寸、Transformer层数等），读者无法直接评估实验的彻底性。
- **算力信息不透明**：未说明训练所需资源，影响可复现性和成本评估。
- **局限性可能包括**：CaptionStew 虽然大规模，但可能存在领域分布不均（如语音、音乐、环境声的比例未提及），且字幕多样性可能仍受限于开源语料的风格。另外，仅对比了对比学习和字幕生成，未探索混合目标（如两者联合）或其他变体。
- **泛化风险**：结论基于特定数据集和任务，在实际工业场景中的表现仍需验证。

（完）
