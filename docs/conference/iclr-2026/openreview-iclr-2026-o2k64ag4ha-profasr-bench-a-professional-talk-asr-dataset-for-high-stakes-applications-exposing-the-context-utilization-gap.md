---
title: "ProfASR-Bench: A Professional-talk ASR Dataset for High-Stakes Applications Exposing the Context-Utilization Gap"
title_zh: ProfASR-Bench：面向高风险应用的专业谈话ASR数据集，揭示上下文利用差距
authors: Deepak Piskala
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=o2k64ag4HA"
tags: ["query:speech-audio"]
score: 9.0
evidence: 专业领域ASR基准数据集
tldr: 本文提出ProfASR-Bench，一个面向金融、医疗、法律和技术等高风险场景的专业谈话ASR评测套件。每个样本包含自然语言提示和富含实体的目标语句，支持上下文依赖识别测试。除了常规指标外，还提供实体感知得分以及按口音和性别的细分报告。该基准揭示了现有ASR系统在上下文利用方面的差距，为提升专业场景识别性能提供了标准测试床。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有ASR基准缺乏专业术语和上下文条件评估。
method: 构建包含领域提示和实体丰富目标语句的数据集，设计实体感知和切片准确性指标。
result: 揭示了主流ASR在专业场景上下文利用上的明显不足。
conclusion: 为专业ASR性能提升提供了基准和诊断工具。
---

## Abstract
Automatic Speech Recognition (ASR) in professional settings faces challenges that existing benchmarks underplay: dense domain terminology, formal register variation, and near-zero tolerance for critical entity errors. We present \textsc{ProfASR-Bench}, a \emph{professional-talk} evaluation suite for high-stakes applications across finance, medicine, legal, and technology. Each example pairs a natural-language \emph{prompt} (domain cue and/or speaker profile) with an entity-rich target utterance, enabling controlled measurement of \emph{context-conditioned} recognition. The corpus supports conventional metrics alongside \emph{entity-aware} scores and slice-wise reporting by accent and gender. Using representative families \emph{Whisper} (encoder–decoder ASR) and \emph{Qwen-Omni} (audio LM) under matched \emph{no-context}, \emph{profile}, \emph{domain+profile}, \emph{oracle}, and \emph{adversarial} conditions, we uncover a consistent pattern: lightweight textual context produces little to no change in average WER, even when providing the gold transcript as an oracle prompt, and adversarial prompts do not reliably degrade WER. We term this the \textbf{\emph{context-utilization gap(CUG)}}: current systems are nominally promptable yet underuse readily available side information. Entity-centric analyses reveal only modest, model-dependent gains on information-bearing tokens, underscoring the need for stronger fusion mechanisms and calibrated trust in prompts.\textsc{ProfASR-Bench} contributes (i) a standardized \emph{context ladder} with paired, within-utterance estimation; (ii) entity-aware and slice-aware reporting with confidence intervals; and (iii) a reproducible testbed to compare fusion strategies across model families. We release data and code to foster comparable, context-aware evaluation in high-stakes domains.

---

## 论文详细总结（自动生成）

# ProfASR-Bench 论文总结

## 1. 核心问题与整体含义（研究动机和背景）
- **问题**：现有自动语音识别（ASR）基准主要关注通用场景，忽略了高风险专业领域（金融、医疗、法律、技术）中的三大挑战：密集的领域术语、正式语域变化、以及对关键实体错误的近乎零容忍。
- **动机**：当前缺乏专门评估专业谈话（professional-talk）的标准化评测套件，导致ASR系统在高风险应用中上下文利用能力不足的问题被掩盖。
- **含义**：本文首次揭示了ASR系统在专业场景中存在的系统性“上下文利用差距”（Context-Utilization Gap, CUG），即系统名义上支持提示词，但实际上几乎不利用给定的侧信息（如领域线索、说话人画像）来改善识别。

## 2. 方法论：核心思想、关键技术细节
- **核心思想**：构建一个包含自然语言提示（prompt）和实体丰富目标语句的配对数据集，支持对上下文条件化识别进行受控测量。
- **数据集结构**：每个样本由两部分组成：
  - **自然语言提示**：包含领域线索和/或说话人画像（例如：金融分析师、男性、美国口音）。
  - **实体丰富目标语句**：富含专业术语、数字、专有名词等实体。
- **上下文条件设计**：定义了五级“上下文阶梯”用于控制实验：
  - `no-context`：无任何提示
  - `profile`：仅提供说话人画像
  - `domain+profile`：提供领域线索和说话人画像
  - `oracle`：提供真实转录文本作为提示（上界）
  - `adversarial`：提供误导性提示
- **评估指标**：除传统词错误率（WER）外，新增：
  - **实体感知得分**：重点关注实体（如人名、数字、专业术语）的识别准确率。
  - **切片报告**：按口音和性别进行分层报告，并附置信区间。

## 3. 实验设计
- **数据集**：构建的专业谈话评测语料库（ProfASR-Bench），覆盖金融、医疗、法律、技术四个高风险领域。
- **基准模型**：测试了两类代表性模型族：
  - **Whisper**（编码器-解码器ASR模型）
  - **Qwen-Omni**（音频语言模型）
- **对比条件**：在五种上下文条件下（no-context, profile, domain+profile, oracle, adversarial）评估WER和实体感知得分，对比不同模型和不同上下文级别的性能变化。

## 4. 资源与算力
- 论文未明确说明使用的GPU型号、数量、训练时长等具体算力信息。
- 仅提及将公开数据和代码，供复现使用。

## 5. 实验数量与充分性
- **实验数量**：主要进行了两大类模型在五种上下文条件下的对比实验，以及实体中心的分析。未提及大规模消融实验或多数据集交叉验证。
- **充分性评价**：实验设计针对“上下文利用差距”这一核心问题较为充分，通过引入oracle和adversarial条件提供了强对照，但模型种类仅两种（且Qwen-Omni为音频LM，与Whisper差异较大），覆盖广度有限。缺乏与其他专业ASR系统或传统baseline（如DeepSpeech、Kaldi）的比较。

## 6. 主要结论与发现
- **上下文利用差距（CUG）**：轻量级文本上下文（profile, domain+profile）对平均WER几乎没有改变；即使提供真实转录作为oracle提示，WER也并未显著下降；对抗性提示也未可靠地使WER升高。说明当前ASR系统即使接受提示也未能有效利用上下文。
- **实体识别增益有限**：实体感知得分仅显示微小的、与模型相关的提升，表明现有模型未能从prompt中捕获关键实体信息。
- **结论**：需要更强的融合机制和对提示的可信度校准，才能提升高风险专业场景的ASR性能。

## 7. 优点
- **创新性**：首次提出专门针对专业谈话的ASR评测套件，聚焦高风险领域实体错误容忍度低这一实际痛点。
- **评估设计**：引入“上下文阶梯”和实体感知指标，能够系统性地诊断ASR系统对上下文的利用能力，超越传统WER单一指标。
- **可复现性**：承诺公开数据和代码，便于学术界进行公平比较和后续研究。
- **切片报告**：按口音和性别分组评估，关注公平性与群体偏差。

## 8. 不足与局限
- **实验覆盖有限**：仅测试了Whisper和Qwen-Omni两个模型族，缺乏更多样化的ASR系统（如大型自监督模型、端到端模型、混合模型）的对比。
- **数据集规模未披露**：语料库大小、每领域样本数、口音和性别分布等细节未知，可能影响结论的普适性。
- **资源信息缺失**：缺少算力消耗和训练成本，不利于评估方案的可部署性。
- **对抗性提示设计单一**：仅提到对抗性条件，未详细说明提示的生成策略，可能削弱结论的鲁棒性。
- **实际应用限制**：仅评估纯文本上下文提示，未考虑音频上下文或多模态上下文；实验环境为受控条件，真实场景中的噪声、多人对话等复杂性未被覆盖。
- **论文被拒**：该论文为ICLR 2026被拒稿，可能同行评审认为方法或实验存在显著不足，阅读时需批判性吸收。

（完）
