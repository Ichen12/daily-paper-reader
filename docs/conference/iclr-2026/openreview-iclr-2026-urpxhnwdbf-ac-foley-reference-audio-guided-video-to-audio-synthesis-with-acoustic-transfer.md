---
title: "AC-Foley: Reference-Audio-Guided Video-to-Audio Synthesis with Acoustic Transfer"
title_zh: AC-Foley：基于参考音频引导和声学迁移的视频到音频合成
authors: "Pengjun Fang, Yingqing He, Yazhou Xing, Qifeng Chen, Ser-Nam Lim, Harry Yang"
date: 2026-01-26
pdf: "https://openreview.net/pdf?id=URPXhnWdBF"
tags: ["query:speech-audio"]
score: 6.0
evidence: 基于参考音频引导的声学迁移视频到音频合成
tldr: 文本条件驱动的视频到音频(V2A)合成面临语义粒度粗、描述模糊等问题。本文提出AC-Foley，直接利用参考音频实现细粒度高精度V2A生成，通过声学迁移传递输入音频的微观声学特征，在保持视觉对齐的同时产生更可控且真实的声学细节，适用于专业拟音场景。
source: ICLR-2026-Accepted
selection_source: conference_retrieval
motivation: 文本条件V2A合成无法精确控制微观声学特征，且训练数据标签粒度不足。
method: 提出音频条件V2A模型，通过参考音频提供细粒度声学控制，并设计声学迁移机制。
result: 相比文本条件方法，生成的音频在声学细节和可控性上显著提升。
conclusion: AC-Foley为V2A合成提供了参考音频控制的新范式，有助于专业级音效制作。
---

## Abstract
Existing video-to-audio (V2A) generation methods predominantly rely on text prompts alongside visual information to synthesize audio. However, two critical bottlenecks persist: semantic granularity gaps in training data (e.g., conflating acoustically distinct sounds like different dog barks under coarse labels), and textual ambiguity in describing microacoustic features (e.g., "metallic clang" failing to distinguish impact transients and resonance decay). These bottlenecks make it difficult to perform fine-grained sound synthesis using text-controlled modes. To address these limitations, we propose **AC-Foley**, an audio-conditioned V2A model that directly leverages reference audio to achieve precise and fine-grained control over generated sounds. This approach enables: fine-grained sound synthesis (e.g., footsteps with distinct timbres on wood, marble, or gravel), timbre transfer (e.g., transforming a violin’s melody into the bright, piercing tone of a suona), zero-shot generation of sounds (e.g., creating unique weapon sound effects without training on firearm datasets) and better audio quality. By directly conditioning on audio signals, our approach bypasses the semantic ambiguities of text descriptions while enabling precise manipulation of acoustic attributes. Empirically, AC-Foley achieves state-of-the-art performance for Foley generation when conditioned on reference audio, while remaining competitive with SOTA video-to-audio methods even without audio conditioning.

---

## 论文详细总结（自动生成）

# AC-Foley: 参考音频引导的声学迁移视频到音频合成 — 详细中文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：现有的视频到音频（V2A）合成方法主要依赖文本提示（text prompts）与视觉信息生成音频。但存在两个关键瓶颈：
  - 训练数据中语义粒度不足：例如不同狗叫声在标签中可能被笼统归为“狗叫”，丢失了声学差异。
  - 文本描述的模糊性：微观声学特征（如“金属碰撞声”难以区分冲击瞬态和共振衰减）无法精确表达。
- **意义**：这些瓶颈导致基于文本的控制模式难以实现细粒度声学合成，限制了专业拟音（Foley）场景中真实、可控的音效生成。AC-Foley通过直接引入参考音频作为条件，绕开语义歧义，实现更精细、可迁移的声学控制，为V2A合成提供了新范式。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：将参考音频（reference audio）作为条件信号，与视觉特征一起输入模型，通过声学迁移（Acoustic Transfer）机制传递参考音频的微观声学特征（如音色、瞬态、衰减），同时保持与视频内容的视觉对齐。
- **关键技术细节**：
  - 模型架构：推测为基于编码器-解码器的端到端生成框架，视觉特征（来自视频帧）和音频特征（来自参考音频）在某种融合模块（如交叉注意力）中结合，最终生成目标音频。
  - 声学迁移机制：可能通过学习将参考音频的声学属性（如梅尔频谱细节）映射到与视频同步的输出中，支持零样本生成（如从未训练过的武器音效）。
  - 条件方式：直接以音频信号（非文本）为控制源，允许用户提供任意参考声音（如木地板上脚步声）来引导生成声音（如不同材质上的脚步声）。
- **具体场景**：
  - 细粒度声音合成：区分木材、大理石、砾石等材质上的脚步声。
  - 音色迁移：将小提琴旋律转换为唢呐的明亮尖锐音色。
  - 零样本生成：无需专门训练即可生成新颖武器音效。

## 3. 实验设计
- **数据集**：从摘要和元数据未明确提及具体数据集名称。推测可能使用了通用V2A基准数据集（如VGGSound、AudioSet等）进行训练与评估，但需结合完整论文确认。
- **Benchmark**：与现有的 SOTA 视频到音频方法（如文本条件的V2A模型）进行比较；同时比较在条件为参考音频时的生成质量。
- **对比方法**：包括无音频条件（纯视觉条件）的SOTA V2A方法，以及可能存在的其他参考音频引导方法（若有）。

## 4. 资源与算力
- **未明确说明**：在提供的摘要和元数据中，没有提及所使用的GPU型号、数量、训练时长或计算资源。需要查看完整论文才能获知。

## 5. 实验数量与充分性
- **预估**：从方法论描述来看，至少包含了：
  - 对细粒度合成（不同材质脚步声）的性能评估。
  - 对音色迁移效果的定量或定性评估。
  - 零样本生成能力的测试。
  - 与SOTA非音频条件方法的对比实验。
  - 可能还有消融实验（如去掉参考音频条件、替换融合方式等）。
- **充分性评价**：目前信息有限，但从动机逻辑判断，实验设计覆盖了核心能力（细粒度、迁移、零样本）和与现有方法的对比，合理充分。但缺乏公开数据集、指标细节（如FAD、IS、MOS分数）和统计显著性检验等，需要完整论文确认其严谨性。

## 6. 主要结论与发现
- AC-Foley在参考音频条件下实现了SOTA的Foley生成性能。
- 即使在没有音频条件时，其性能依然与纯视觉条件的SOTA V2A方法相当（即模型在没有参考音频时依然有效）。
- 直接以音频作为条件能够有效绕过文本的语义歧义，实现对微观声学特征的精准操控，生成音频质量更高、可控性更强。

## 7. 优点
- **方法创新**：提出基于参考音频的声学迁移方案，填补了现有V2A方法在细粒度控制上的空白。
- **实际应用价值**：适用于专业拟音制作（如电影、游戏），用户只需提供一段参考音频即可生成所需声音，降低专业门槛。
- **零样本泛化**：能生成训练集中未见过的音效（如新颖武器声），表现出良好的迁移能力。
- **多场景覆盖**：同时支持细粒度合成、音色迁移、零样本生成。

## 8. 不足与局限
- **实验细节缺失**：从提供材料中无法获知具体数据集、评价指标、对比方法的完整列表，限制了对方法有效性的全面判断。
- **计算资源未提及**：无法评估方法的可复现性和实际部署成本。
- **潜在偏差风险**：参考音频的质量和相似性可能对生成结果敏感，若参考音频与视频场景不匹配，可能产生音画不同步或声学不一致。
- **应用限制**：仅依赖参考音频可能仍无法处理某些非声学抽象的语义控制（如情感、风格），且要求用户具备可用的参考音频。
- **未讨论失败案例**：缺少对边界情况（如极端噪声参考、视觉模糊场景）的分析。

（完）
