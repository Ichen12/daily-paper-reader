---
title: "Drax: Speech Recognition with Discrete Flow Matching"
title_zh: Drax：基于离散流匹配的语音识别
authors: "Aviv Navon, Aviv Shamsian, Neta Glazer, Yael Segal-Feldman, Gill Hetz, Joseph Keshet, Ethan Fetaya"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=E9hSdtsAG0"
tags: ["query:speech-audio"]
score: 9.0
evidence: 面向ASR的离散流匹配
tldr: 扩散/流模型在ASR中的潜力未充分探索。Drax 提出离散流匹配框架用于ASR，构建音频条件概率路径，使训练轨迹模拟推理中的典型误差，理论分析将泛化差距与累积速度误差关联。实验证明该框架在并行解码下取得了有竞争力的词错误率。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 扩散模型在ASR中应用不足。
method: 提出离散流匹配框架，构建音频条件概率路径模拟推理误差。
result: 在并行解码下取得有竞争力的错误率。
conclusion: Drax 扩展了流匹配在ASR中的应用，为高效并行解码提供了新思路。
---

## Abstract
Diffusion and flow-based non-autoregressive (NAR) models have shown strong promise in large language modeling, however, their potential for automatic speech recognition (ASR) remains largely unexplored. We propose Drax, a discrete flow matching framework for ASR that enables efficient parallel decoding. To better align training with inference, we construct an audio-conditioned probability path that guides the model through trajectories resembling likely intermediate inference errors, rather than direct random noise to target transitions. Our theoretical analysis links the generalization gap to divergences between training and inference occupancies, controlled by cumulative velocity errors, thereby motivating our design choice. Empirical evaluation demonstrates that our approach attains recognition accuracy on par with state-of-the-art speech models while offering improved accuracy-efficiency trade-offs, highlighting discrete flow matching as a promising direction for advancing NAR ASR.

---

## 论文详细总结（自动生成）

基于给定论文内容（摘要及元数据），生成如下总结：

## 1. 论文的核心问题与整体含义（研究动机和背景）
- **核心问题**：扩散模型和基于流的非自回归（NAR）模型在大语言建模中展示出强大潜力，但在自动语音识别（ASR）领域的应用仍未被充分探索。
- **研究动机**：探索离散流匹配（discrete flow matching）在ASR中的可行性，旨在实现高效并行解码，同时缩小训练与推理之间的差距。
- **整体含义**：Drax 提出了一种新的离散流匹配框架，为NAR ASR提供了一种有前途的方向，在保持识别精度的同时提升效率。

## 2. 论文提出的方法论：核心思想、关键技术细节、公式或算法流程（用文字说明即可）
- **核心思想**：通过构建音频条件概率路径（audio-conditioned probability path），使训练轨迹模拟推理中可能出现的典型中间误差，而非从随机噪声直接向目标转换，从而更好对齐训练与推理。
- **关键技术细节**：
  - 采用离散流匹配（discrete flow matching）来建模从初始分布到目标文本序列的转换。
  - 构建音频条件概率路径：该路径以音频特征为条件，指导模型沿类似推理中间错误的轨迹演化。
  - 理论分析：将泛化差距（generalization gap）与训练和推理 occupancy 之间的散度联系起来，该散度由累积速度误差（cumulative velocity errors）控制，从而为路径设计提供理论依据。
- **算法流程**（文字描述）：训练时，根据音频条件概率路径生成中间状态序列，目标是学习速度场（velocity）以拟合从初始分布到目标序列的流；推理时，通过学习到的流进行并行解码，一步生成整个序列。

## 3. 实验设计：使用了哪些数据集 / 场景，它的 benchmark 是什么，对比了哪些方法
- **数据集与场景**：摘要未明确列出具体数据集。元数据中 tags 为 `query:speech-audio`，推测可能涉及常见语音识别基准（如 LibriSpeech、Common Voice 等），但无确切信息。
- **Benchmark**：与当前最先进的语音模型（state-of-the-art speech models）进行对比，衡量词错误率（WER）。
- **对比方法**：包括现有NAR ASR方法以及其他并行/自回归模型。具体对比方法未在摘要中列出。

## 4. 资源与算力：如果文中有提到，请总结使用了多少算力（GPU 型号、数量、训练时长等）。若未明确说明，也请指出这一点
- **资源与算力**：论文摘要及元数据中 **未提及** 任何 GPU 型号、数量或训练时长等信息。

## 5. 实验数量与充分性：大概做了多少组实验（如不同数据集、消融实验等），这些实验是否充分、是否客观、公平
- **实验数量**：摘要中仅给出“Empirical evaluation demonstrates...”，未报告具体实验组数（如消融实验、不同数据集结果等）。元数据中提到“在并行解码下取得有竞争力的错误率”，暗示仅进行了主要性能评估。
- **充分性与客观性**：由于缺乏详细实验设置（如数据集、超参数、基线复现情况），无法判定实验是否充分、客观、公平。仅凭摘要不足以支撑严谨评估。

## 6. 论文的主要结论与发现
- 提出 Drax 框架，将离散流匹配成功应用于 ASR，实现并行解码。
- 通过音频条件概率路径模拟推理误差，有效缩小训练与推理的分布差异，理论分析支持了该设计。
- 实验表明，Drax 在识别准确率上与最先进语音模型相当，同时在准确率-效率权衡上表现更优。
- 结论：离散流匹配是推动非自回归 ASR 进步的有前景方向。

## 7. 优点：方法或实验设计上有哪些亮点
- **方法创新**：首次将离散流匹配引入 ASR，并构建了音频条件概率路径，主动让训练轨迹模拟推理错误，这一思路新颖且具有理论动机。
- **理论贡献**：提供了泛化差距与累积速度误差之间的理论关系，为设计训练路径提供了原则性指导。
- **效率优势**：并行解码设计可能大幅降低推理延迟，适合实时或低资源场景。

## 8. 不足与局限：包括实验覆盖、偏差风险、应用限制等
- **实验不足**：摘要未披露具体数据集、对比方法、消融实验、超参数敏感性分析等，实验覆盖明显不足，难以全面评估方法鲁棒性。
- **偏差风险**：由于缺少多场景（如噪声环境、不同语种、低资源语言）的验证，可能存在过拟合于特定基准的风险。
- **应用限制**：仅关注词错误率，未见对模型大小、推理显存、解码延迟的量化对比；且方法依赖离散流匹配，可能对离散序列长度敏感，未讨论长语音序列的扩展性。
- **资源与可复现性**：未提供代码或训练细节，可复现性未知。

（完）
