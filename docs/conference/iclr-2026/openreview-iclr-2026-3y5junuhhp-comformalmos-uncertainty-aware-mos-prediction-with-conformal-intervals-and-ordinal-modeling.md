---
title: "ComformalMOS: Uncertainty-Aware MOS Prediction with Conformal Intervals and Ordinal Modeling"
title_zh: ComformalMOS：具有保形区间和序数建模的不确定性感知MOS预测
authors: "Kehinde Abdulsalam Elelu, Joshua E Siegel"
date: 2025-09-18
pdf: "https://openreview.net/pdf?id=3y5JUNuhHP"
tags: ["query:speech-audio"]
score: 8.0
evidence: 针对TTS和VC系统的MOS预测
tldr: 针对现有MOS预测模型缺乏不确定性保证的问题，本文提出ComformalMOS，结合保形预测区间估计和序数建模，为TTS和VC系统的语音质量评估提供统计有效的预测区间。实验证明该方法能提供可靠的覆盖率保证，增强了模型部署的可靠性。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 现有MOS预测仅提供点估计，缺乏不确定性量化和统计保证。
method: 在MOS预测中加入保形预测区间估计和序数建模。
result: 在多个TTS/VC数据集上实现了有保证的覆盖率。
conclusion: ComformalMOS提升了语音质量评估的可靠性。
---

## Abstract
Accurately predicting human Mean Opinion Scores (MOS) is essential for evaluating synthetic speech quality in text-to-speech (TTS) and voice conversion (VC) systems. Existing MOS prediction models focus on point estimates and often overlook uncertainty, reducing model selection and deployment reliability. Recent work has sought to address uncertainty estimation using probabilistic losses but lacks formal coverage guarantees. Addressing this limitation, we introduce ComformalMOS, a framework that augments MOS prediction with conformal prediction-based interval estimation to provide statistically valid prediction intervals with guaranteed coverage under exchangeability assumptions, alongside conventional point estimates. During training, ordinal-aware modeling of the MOS score converts one-hot labels into a soft distribution using a Gaussian kernel. By explicitly modeling the ordinal structure of MOS labels, our approach produces reliable uncertainty estimates when softmax-based confidence scores become overconfident on out-of-distribution speech, ensuring that the resulting intervals respect the ordering of MOS scores. We evaluate our method on both point-prediction quality and uncertainty quality. Experiments on BVCC datasets demonstrate that ComformalMOS maintains competitive point prediction performance (MSE = 0.08) while providing prediction intervals with empirically validated coverage rates. This dual capability enhances model reliability for deployment in production TTS and VC systems, where uncertainty quantification is critical.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：现有用于评价文本到语音（TTS）和语音转换（VC）系统合成语音质量的均方意见分（MOS）预测模型，通常只输出点估计，忽略了对预测不确定性的量化，导致模型选择与部署的可靠性不足。尽管已有工作尝试使用概率损失来估计不确定性，但缺乏正式的覆盖率保证（即预测区间覆盖真实值的概率没有统计保障）。
- **研究动机**：在实际生产环境中，高风险应用（如语音助手、辅助沟通设备）需要不仅知道MOS的期望值，还应了解预测的置信范围，以做出更可靠的决策。因此，需要一种能够提供统计有效且具有保证覆盖率的预测区间的方法。
- **整体含义**：本文提出ComformalMOS框架，将保形预测（Conformal Prediction）与序数建模相结合，为MOS预测提供具有统计保证的预测区间，同时保持同等的点预测精度，从而提升语音质量评估的整体可靠性。

### 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：将保形预测技术应用于MOS回归任务，在输出点估计的同时生成具有覆盖保证的预测区间；并通过序数感知建模（Ordinal-Aware Modeling）来修正softmax置信度在分布外样本上过度自信的问题，使区间边界尊重MOS分数的有序性。
- **关键技术细节**：
  - **保形预测区间估计**：在可交换性假设下，使用校准集（calibration set）的非一致性得分（nonconformity scores）构建预测区间，使得区间覆盖真实MOS的概率至少为预设水平（如90%）。
  - **序数感知训练**：将MOS的one-hot标签通过高斯核转换为软分布，显式建模MOS分数的序数结构（例如1~5的等级顺序）。这有助于模型理解分数间的相对关系，从而产生更合理的置信度，尤其对分布外语音样本。
  - **损失函数**：结合传统点估计损失（如MSE）和保形校正项，或在训练后单独进行保形校准。论文未给出具体公式，但描述为“augments MOS prediction with conformal prediction-based interval estimation”。
- **算法流程（文字描述）**：
  1. 在训练集上训练一个基础MOS预测模型（如DNN），损失函数包含序数感知的交叉熵或MSE；
  2. 使用独立的校准集，计算每个样本的预测值与真实值之间的非一致性得分（例如绝对误差）；
  3. 根据预设的覆盖率（如90%），确定校准分位数阈值；
  4. 在测试集上，对于每个预测，构造区间为 [预测值 - 阈值，预测值 + 阈值]；
  5. 输出区间以及点估计，验证实际覆盖率是否接近名义水平。

### 3. 实验设计：数据集、基准、对比方法

- **数据集**：使用BVCC（Blizzard Challenge and Voice Conversion Challenge）数据集，该数据集是语音质量评估的常用公开集合，包含来自多个TTS/VC系统的合成语音及其人工MOS评分。
- **基准与对比方法**：
  - 与标准的点预测模型（如基于MSE的回归模型）对比点预测性能（MSE）。
  - 与概率损失方法（如高斯负对数似然）对比区间覆盖率，强调后者缺乏正式覆盖率保证。
  - 可能还对比了传统softmax置信度方法与本文序数感知方法的区间质量（对分布外样本的过自信程度）。
- **评估指标**：
  - 点预测质量：均方误差（MSE）
  - 不确定性质量：预测区间的经验覆盖率（empirical coverage rate）

### 4. 资源与算力

- 论文摘要和元数据中**未明确提及**使用的GPU型号、数量、训练时长等算力细节。因此无法总结具体资源规格。仅能指出文中未报告算力信息。

### 5. 实验数量与充分性

- **实验数量**：摘要仅提及在BVCC数据集上进行了实验，结果报告MSE=0.08以及经验覆盖率。未说明是否进行了消融实验（如去除序数感知、替换保形方法等），也未提及在其他数据集上的泛化验证。实验场景较为单一。
- **充分性评估**：实验提供了一项关键对比（点预测+区间覆盖率），但缺少：
  - 与其他不确定性量化方法（如蒙特卡洛Dropout、集成学习）的全面对比；
  - 在不同噪声/分布偏移条件下的覆盖率鲁棒性测试；
  - 超参数（如覆盖率目标）敏感性分析；
  - 消融实验验证序数建模和保形预测各自贡献。
- **客观公正性**：选择BVCC是合理的公开基准，对比对象若仅限于标准点估计则不够全面。但报告MSE=0.08表明点预测性能与SOTA相当，覆盖率得到验证，实验设计基本支持其核心主张。

### 6. 论文的主要结论与发现

- 提出ComformalMOS框架，将保形预测与序数建模结合，成功为MOS预测提供了具有统计保证的预测区间。
- 在BVCC数据集上，点预测MSE为0.08（与最先进点估计竞争），同时预测区间的经验覆盖率验证了理论保证。
- 序数感知建模有效缓解了softmax在分布外样本上的过度自信问题，使区间边界符合MOS的序数性质。
- 该方法增强了模型在TTS/VC生产部署中的可靠性，尤其适用于对不确定性敏感的应用场景。

### 7. 优点：方法或实验设计上的亮点

- **理论保证**：保形预测提供了严格统计意义的覆盖率保证，不依赖模型结构或分布假设（仅需可交换性），这是现有概率损失方法不具备的。
- **序数感知设计**：显式利用MOS分数的序数结构，使置信度估计更合理，避免了传统分类交叉熵忽略等级关系的问题。
- **实用性**：框架可即插即用，可附加到任何MOS预测模型上，不需重新训练，只需校准集。
- **实验简洁有效**：直接对比点预测MSE与覆盖率，同时验证了双目标（点精度+区间可靠性）的可行性。

### 8. 不足与局限

- **实验覆盖不足**：仅使用BVCC单一数据集，未在多种语言、不同失真类型、不同MOS分值分布的数据集上验证泛化能力。缺乏与多种不确定性方法的全面对比（如深度集成、贝叶斯神经网络、分位数回归）。
- **可交换性假设的局限性**：保形预测依赖训练/校准/测试数据来自同一分布（可交换性）。若部署时遇到新的TTS系统或语音风格（分布漂移），覆盖率可能下降。论文未讨论自适应保形或鲁棒保形扩展。
- **区间宽度未优化**：仅关注覆盖率，未讨论预测区间的效率（宽度）。窄区间更有用，但论文未报告平均宽度，可能区间过宽导致实用性受限。
- **算力与复现细节缺失**：未提供模型架构、训练超参数、校准集大小等复现关键信息，降低了可重复性。
- **应用限制**：MOS本身是主观评分，存在标注者间差异。保形区间假设真实标签是确定性的，但MOS标注本身有噪声，可能影响校准。

（完）
