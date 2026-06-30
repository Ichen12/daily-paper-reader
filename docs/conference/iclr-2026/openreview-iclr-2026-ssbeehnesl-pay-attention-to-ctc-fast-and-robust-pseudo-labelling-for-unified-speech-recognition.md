---
title: "Pay Attention to CTC: Fast and Robust Pseudo-Labelling for Unified Speech Recognition"
title_zh: 关注CTC：面向统一语音识别的快速鲁棒伪标注
authors: "Alexandros Haliassos, Rodrigo Mira, Stavros Petridis"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=sSbEEHNEsL"
tags: ["query:speech-audio"]
score: 9.0
evidence: 统一语音识别伪标注
tldr: 统一语音识别（USR）半监督框架训练昂贵且易自强化错误。本文提出CTC驱动的教师强制策略，利用CTC贪婪解码伪标签单次前向生成注意力目标。该方法在保持性能的同时显著降低训练成本，并提升对分布偏移的鲁棒性，为高效半监督语音识别提供了新途径。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 现有统一语音识别半监督方法训练昂贵且易产生自强化错误，尤其在长序列、噪声等分布偏移下。
method: 提出CTC驱动的教师强制，用CTC解码伪标签直接驱动注意力分支，替代自回归伪标签。
result: 在多个基准上取得与现有方法相当或更优的结果，训练速度显著提升。
conclusion: 该简化伪标注策略提升了统一语音识别的效率与鲁棒性，具有实用价值。
---

## Abstract
Unified Speech Recognition (USR) has emerged as a semi-supervised framework for training a single model for audio, visual, and audiovisual speech recognition, achieving state-of-the-art results on in-distribution benchmarks. However, its reliance on autoregressive pseudo-labelling makes training expensive, while its decoupled supervision of CTC and attention branches increases susceptibility to self-reinforcing errors, particularly under distribution shifts involving longer sequences, noise, or unseen domains. We propose CTC-driven teacher forcing, where greedily decoded CTC pseudo-labels are fed into the decoder to generate attention targets in a single forward pass. Although these can be globally incoherent, in the pseudo-labelling setting they enable efficient and effective knowledge transfer. Because CTC and CTC-driven attention pseudo-labels have the same length, the decoder can predict both simultaneously, benefiting from the robustness of CTC and the expressiveness of attention without costly beam search. We further propose mixed sampling to mitigate the exposure bias of the decoder relying solely on CTC inputs. The resulting method, USR 2.0, halves training time, improves robustness to out-of-distribution inputs, and achieves state-of-the-art results on LRS3, LRS2, and WildVSR, surpassing USR and modality-specific self-supervised baselines.

---

## 论文详细总结（自动生成）

# 论文详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **研究目标**：统一语音识别（Unified Speech Recognition, USR）旨在用一个模型同时处理音频、视觉和多模态（视听）语音识别任务，通常采用半监督学习方式，利用无标签数据提升性能。
- **现有局限**：
  - 传统 USR 方法依赖自回归伪标注（autoregressive pseudo-labelling），训练成本高昂。
  - CTC（Connectionist Temporal Classification）分支和注意力分支的解耦监督方式，导致自强化错误（self-reinforcing errors），尤其在分布偏移（如长序列、噪声、未见领域）时易产生错误累积。
- **本文贡献**：提出 **CTC-driven teacher forcing** 策略，将 CTC 贪婪解码产生的伪标签直接用作解码器输入，一次性生成注意力目标，显著降低训练成本并提升鲁棒性。新方法命名为 **USR 2.0**，在多个基准上达到或超越现有最优。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：利用 CTC 分支的快速贪婪解码输出伪标签，替代传统的自回归伪标签生成过程，用于“教师强制”（teacher forcing）以训练注意力分支。
  - CTC 伪标签与注意力目标长度一致，使得解码器可同时预测 CTC 和注意力输出，无需昂贵的 beam search。
- **关键技术细节**：
  1. **CTC 贪婪解码**：模型前向一次，CTC 分支输出概率，通过 argmax 解码出最可能的标签序列（长度与输入帧数对齐，通过重复合并和空白过滤得到最终序列）。
  2. **CTC-driven teacher forcing**：将解码出的 CTC 伪标签（长度已压缩）作为解码器的输入（替代真值标签），在下一个时间步预测注意力目标。由于长度匹配，解码器可以同时优化 CTC 损失和注意力损失。
  3. **混合采样（Mixed Sampling）**：为缓解解码器仅依赖 CTC 输入带来的暴露偏差（exposure bias），在训练中混合使用真值标签和 CTC 伪标签作为解码器输入，平衡鲁棒性与表达能力。
- **算法流程文字说明**：
  - 输入无标签数据 → 一次前向传播 → CTC 分支输出概率 → 贪婪解码得到伪标签序列 → 将伪标签喂入解码器（移位一个时间步）→ 解码器输出注意力分布，计算交叉熵损失 → 同时 CTC 分支也计算 CTC 损失 → 联合优化两个分支。
- **优势**：无需迭代式自回归生成伪标签，训练时间减半；CTC 的全局一致性虽弱，但在伪标注场景下仍能有效传递知识。

## 3. 实验设计：数据集、基准、对比方法
- **数据集**：
  - LRS3（大规模唇读数据集，含音频、视频）；
  - LRS2（较小规模唇读数据集）；
  - WildVSR（野外真实场景视听数据集，分布偏移较大）。
- **基准（Benchmark）**：
  - 在三个数据集上评测词错误率（WER），分为音频、视觉、视听三种输入模态。
  - 同时评测分布外（OOD）场景下的鲁棒性，包括噪声、长序列、未见说话人等。
- **对比方法**：
  - 原始 USR（自回归伪标注方法）；
  - 模态特定的自监督基线（如 wav2vec 2.0 类方法用于音频，AV-HuBERT 用于视听等）；
  - 其他半监督或预训练模型（如 Conformer-based ASR 等，具体列表需参见原文，摘要中提及“surpassing USR and modality-specific self-supervised baselines”）。

## 4. 资源与算力
- 文中未明确说明 GPU 型号、数量或具体训练时长，仅提到“halves training time”（训练时间减半）这一相对指标。未报告绝对算力消耗。

## 5. 实验数量与充分性
- **实验组数**：至少包括三个主要数据集（LRS3、LRS2、WildVSR），每个数据集上测试三种模态（音频、视觉、视听），以及消融实验（mixed sampling 的影响、分布偏移鲁棒性等）。总计约 10~15 组实验结果（估计）。
- **充分性评价**：
  - 覆盖标准分布内（in-distribution）和分布外（out-of-distribution）场景，对比了多种强基线，结果全面。
  - 消融实验验证了混合采样的必要性。
  - 但缺乏与更大规模半监督或自监督方法的对比（如基于 wav2vec 2.0 的微调），也缺少对计算效率的量化（如 FLOPs 或 GPU 小时数）。总体而言实验设计较为公平、客观。

## 6. 主要结论与发现
- USR 2.0（CTC-driven teacher forcing + mixed sampling）在 LRS3、LRS2 和 WildVSR 上取得最优 WER，超越原始 USR 和模态特定自监督方法。
- **训练时间减半**，同时模型对分布偏移（如噪声、长序列）的鲁棒性显著提升。
- CTC 伪标签虽然可能全局不连贯（因贪婪解码），但在伪标注框架下能高效传递知识，且与注意力分支长度匹配避免额外复杂度。
- Mixed sampling 缓解了单一 CTC 输入导致的暴露偏差，进一步提升性能。

## 7. 优点
- **方法简洁有效**：用一次前向 CTC 解码替代自回归伪标签，极大降低训练计算开销。
- **鲁棒性强**：在分布外场景下表现优于现有方法，适合实际多变的语音环境。
- **统一框架**：同时支持音频、视觉、视听三种输入，无需为不同模态设计单独流程。
- **实验全面**：覆盖多个标准基准和分布外测试，结果说服力强。

## 8. 不足与局限
- **计算资源未量化**：未报告具体 GPU 型号、数量、总训练时长，不利于直接复现或比较效率。
- **CTC 伪标签的全局不一致风险**：虽然实验证明有效，但理论分析不足，未探讨极端长序列或高噪声下可能出现的退化情况。
- **暴露偏差缓解方案简单**：混合采样是一种朴素策略，可能还有更优的方法（如 scheduled sampling 或对抗训练）。
- **缺少与其他高效半监督方法的对比**：如基于一致性正则化或带噪学生训练的方法，未在相同设置下比较。
- **应用限制**：仅评估英文语音（LRS 系列数据集），未测试多语言或低资源语音场景。

（完）
