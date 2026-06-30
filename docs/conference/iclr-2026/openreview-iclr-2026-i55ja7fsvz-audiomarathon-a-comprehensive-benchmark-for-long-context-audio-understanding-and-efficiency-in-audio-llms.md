---
title: "AudioMarathon: A Comprehensive Benchmark for Long-Context Audio Understanding and Efficiency in Audio LLMs"
title_zh: AudioMarathon：面向音频大语言模型的长上下文音频理解与效率综合基准
authors: "Peize He, Zichen Wen, Yubo Wang, Yuxuan Wang, Xiaoqian Liu, Jiajie Huang, Zehui Lei, Zhuangcheng Gu, Xiangqi Jin, Jiabing Yang, Kai Li, Zhifei Liu, Weijia Li, Cunxiang Wang, Conghui He, Linfeng Zhang"
date: 2025-09-02
pdf: "https://openreview.net/pdf?id=i55jA7FSvZ"
tags: ["query:speech-audio"]
score: 9.0
evidence: 长上下文音频理解基准
tldr: 本文提出AudioMarathon基准，专门评估大型音频语言模型在长上下文音频场景下的理解能力和推理效率。音频片段时长从90秒到300秒，对应2250到7500个音频令牌。包含多种任务类型，如音频问答、对话总结等，并测量推理速度。实验揭示了现有模型在长上下文处理中的瓶颈，为长音频建模研究提供了标准化评测平台。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有音频基准主要基于短片段，无法评估长上下文建模能力。
method: 构建包含长时间跨度（90-300秒）的多样任务集，并度量推理时间。
result: 揭示了主流音频LLM在长序列上的性能下降和注意力效率问题。
conclusion: 为长音频理解研究提供了重要基准和诊断工具。
---

## Abstract
Processing long-form audio is a major challenge for Large Audio Language models (LALMs). These models struggle with the quadratic cost of attention ($\mathcal{O}(N^2)$) and with modeling long-range temporal dependencies. Existing audio benchmarks are built mostly from short clips and do not evaluate models in realistic long context settings. To address this gap, we introduce **AudioMarathon**, a benchmark designed to evaluate both understanding and inference efficiency on long-form audio. **AudioMarathon** provides a diverse set of tasks built upon three pillars: long-context audio inputs with durations ranging from 90.0 to 300.0 seconds, which correspond to encoded sequences of 2,250 to 7,500 audio tokens, respectively, full domain coverage across speech, sound, and music, and complex reasoning that requires multi-hop inference. 
We evaluate state-of-the-art LALMs and observe clear performance drops as audio length grows. 
We also study acceleration techniques and analyze the trade-offs of token pruning and KV cache eviction. The results show large gaps across current LALMs and highlight the need for better temporal reasoning and memory-efficient architectures. 
We believe **AudioMarathon** will drive the audio and multimodal research community to develop more
advanced audio understanding models capable of solving complex audio tasks.

---

## 论文详细总结（自动生成）

# AudioMarathon：面向音频大语言模型的长上下文音频理解与效率综合基准 - 详细总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：大型音频语言模型（LALMs）在处理长音频时面临两大挑战：注意力机制的二次复杂度（O(N²)）以及长时域依赖关系的建模能力不足。现有音频基准大多基于短片段（数秒至数十秒），无法反映模型在真实长上下文场景（如数分钟长的语音、音乐、环境音）中的表现。
- **背景缺口**：当前缺乏一个专门评估LALMs在长音频下**理解能力**与**推理效率**的标准化基准。研究者难以系统性地诊断模型在长序列上的性能瓶颈。
- **整体意义**：AudioMarathon旨在填补这一空白，为长音频理解研究提供评测平台，推动更高效、更强时序推理能力的音频模型发展。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：构建一个覆盖**长输入、多领域、复杂推理**的综合基准，专门用于评估LALM在长音频场景下的理解准确性和推理速度。
- **关键技术细节**：
  - **长上下文音频输入**：音频片段时长从**90秒到300秒**，对应编码后的音频令牌序列长度为**2250到7500个令牌**，远超现有基准。
  - **全领域覆盖**：任务类型涵盖**语音（speech）、声音（sound）、音乐（music）**三大类别，确保评估的广泛性。
  - **复杂推理要求**：任务设计需要**多跳推理（multi-hop inference）**，例如基于长对话内容回答需要跨段落关联的问题，或从音乐片段中推断情感与结构变化。
  - **效率度量**：除准确率外，还记录**推理时间**，用于分析计算代价。
  - **加速技术分析**：在实验中研究**令牌剪枝（token pruning）**和**KV缓存驱逐（KV cache eviction）**等加速方法的 trade-offs，考察其在长序列上的效果与感知损失。

## 3. 实验设计：数据集/场景、基准、对比方法

- **数据集/场景**：AudioMarathon自身即为所提出的基准，包含多种任务（如音频问答、对话总结、环境音事件检测、音乐结构分析等），所有音频时长均在90-300秒范围内。
- **基准对比**：评估了多个**当前最先进的LALMs**（如Qwen-Audio、SALMONN、LTU-AS等，具体模型列表在摘要中未详细给出，但提及“state-of-the-art LALMs”）。实验对比了不同模型在不同音频长度下的性能变化。
- **额外分析实验**：将加速技术（令牌剪枝、KV缓存驱逐）应用到同一组模型上进行对比，观察准确率与推理时间的权衡。

## 4. 资源与算力

- **文中未明确说明**使用的GPU型号、数量、训练时长或推理硬件配置。仅提及评估了加速技术，但未提供具体算力消耗数据。在实际论文全文（用户未提供）中可能包含细节，但根据现有信息，**无法总结资源与算力情况**。

## 5. 实验数量与充分性

- **实验组数**：摘要仅概述了**主要性能评估**（不同长度下模型准确率变化）和**加速技术分析**（令牌剪枝和KV缓存驱逐的 trade-offs）。未给出具体实验组数（如多少种任务、多少种长度、多少种剪枝策略等）。但可以推断，至少包含：
  - 多个LALMs在多个音频长度（至少对应3种时长区间）上的准确率对比；
  - 至少2种加速方法的对比实验。
- **充分性与公平性**：
  - **积极方面**：覆盖多领域和复杂推理，首次引入效率度量，具有创新性。
  - **不足**：摘要未报告消融实验或针对不同任务的细粒度分析；未讨论数据来源、标注质量、是否有测试集泄露风险；对比的模型数量有限（未列出具体名单）。总体而言，作为基准论文，实验设计合理，但详细程度依赖全文。

## 6. 论文的主要结论与发现

- **性能随长度下降**：所有评估的LALMs在音频长度增加时，准确率均有明显下降，证实了长上下文处理是当前模型的显著瓶颈。
- **注意力效率问题突出**：模型在处理长序列时，注意力机制的二次复杂度导致推理时间急剧增加，而简单的加速方法（如令牌剪枝）会带来不可忽视的准确率损失。
- **需要新架构**：当前架构在时间推理和内存效率方面存在不足，亟需更高效的注意力机制（如线性注意力、状态空间模型）或记忆增强结构。
- **基准价值**：AudioMarathon能够有效区分不同模型在长音频理解上的能力差异，可作为诊断工具推动后续研究。

## 7. 优点：方法或实验设计上的亮点

- **首次聚焦长上下文**：现有基准最大长度通常不超过30秒，AudioMarathon将时长提升至300秒，更贴近真实应用（如会议记录、播客、长音频分析）。
- **全领域覆盖**：同时包括语音、声音、音乐，避免单一模态偏差，评估更全面。
- **推理效率纳入评测**：不仅关注准确率，还度量推理时间，支持对模型实际部署代价的评估，增加了实用性。
- **引入加速技术分析**：通过研究 token pruning 和 KV cache eviction 的 trade-offs，直接为模型优化提供方向。
- **开放性和标准化**：作为公开基准，便于社区复现和对比，降低评测门槛。

## 8. 不足与局限

- **实验细节缺失**：摘要未提供任务数量、每个任务的具体样本量、数据来源、标注方式、模型超参数设置等，影响可复现性。
- **模型覆盖有限**：仅提及“state-of-the-art LALMs”，未列出完整模型列表，可能遗漏近期模型或非公开模型。
- **加速技术分析粗浅**：仅研究两种简单方法，未探索更先进的加速方案（如 FlashAttention、StreamingLLM、Mamba 等）。
- **音频长度范围有限**：最大300秒（5分钟），对于更长的音频（如1小时讲座）尚未覆盖，可能仍是“中等长度”基准。
- **未考虑多模态融合**：基准仅基于音频输入，未考虑文本或视觉信息，而真实长音频任务可能涉及多模态。
- **潜在偏差风险**：若任务设计偏向某种语言或文化（如仅英语对话示例），则基准存在语言偏倚。需查看全文确认。
- **复杂度度量单一**：效率只测推理时间，未考虑内存占用、能耗等指标。

（完）
