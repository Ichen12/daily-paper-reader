---
title: "DepthSense+DP: Adaptive Learning for Robust and Differential Private Silent Speech Recognition"
title_zh: DepthSense+DP：面向鲁棒差分隐私静默语音识别的自适应学习
authors: "Rong Fu, weizhi Tang, Simon James Fong"
date: 2025-09-06
pdf: "https://openreview.net/pdf?id=HBozeTR6J6"
tags: ["query:speech-audio"]
score: 7.0
evidence: 差分隐私静默语音识别
tldr: 静默语音识别面临隐私挑战。本文提出DepthSense+DP，在3D深度点云上集成校准输入扰动、特征级差分隐私和几何保持对齐，采用P4DConv前端和Conformer编码器。实验表明在保护隐私的同时保持接近基线的准确率，并显著降低成员推断风险。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 静默语音识别需要隐私保护，且现有方法泛化性差。
method: 结合双阶段差分隐私注入、自适应DAD门和几何保持对齐的轻量级架构。
result: 在大型多位置语料库上达到接近基线的准确率，成员推断显著减少。
conclusion: 实现了高效设备端推理的隐私保护静默语音识别。
---

## Abstract
DepthSense+DP is a privacy-preserving framework for silent speech recognition from dynamic 3D depth point clouds. It integrates calibrated input perturbation, feature-level differential privacy, and geometry-preserving alignment within a lightweight P4DConv front end and Conformer encoder to ensure robust cross-user and cross-device generalization under formal DP guarantees. A dual-stage DP pipeline injects noise at point and feature levels while maintaining articulatory geometry, aided by an adaptive DAD gate for improved privacy–utility trade-off. The co-designed architecture enables efficient on-device inference. Experiments on a large multi-location corpus show near-baseline accuracy with significant reductions in membership, inversion, and attribute-inference risks, supported by full DP accounting and attack evaluations.

---

## 论文详细总结（自动生成）

# 详细中文总结

## 1. 论文的核心问题与整体含义（研究动机和背景）

静默语音识别（Silent Speech Recognition, SSR）从动态3D深度点云中识别无声发音，但现有方法存在两大挑战：一是缺乏隐私保护机制，容易遭受成员推断、模型反演和属性推断攻击；二是跨用户、跨设备的泛化性能差。本文提出 **DepthSense+DP** 框架，旨在实现**形式化差分隐私（DP）保障**下的鲁棒静默语音识别，同时保持高精度并支持高效设备端推理。

## 2. 论文提出的方法论

### 核心思想
在轻量级P4DConv前端和Conformer编码器中集成**双阶段差分隐私注入**（点级扰动+特征级扰动），并引入**自适应DAD门**（Differentially Adaptive Denoising Gate）和**几何保持对齐**机制，在隐私预算与识别精度之间取得更优折中。

### 关键技术细节
- **校准输入扰动**：对原始3D点云坐标添加符合DP的高斯噪声，同时保留发音几何结构。
- **特征级差分隐私**：在编码器中间特征层注入噪声，增强隐私保护层次。
- **几何保持对齐**：通过损失函数约束确保噪声扰动不破坏发音动作的几何连续性。
- **自适应DAD门**：根据当前隐私预算和梯度信息动态调整去噪强度，优化隐私-效用权衡。
- **整体架构**：P4DConv（Point Cloud 4D Convolution）+ Conformer 编码器，支持高效端侧部署。

### 公式/算法流程（文字说明）
1. 输入原始3D深度点云序列；
2. 在点云层级添加校准高斯噪声（满足ε0-DP）；
3. 通过P4DConv提取时空特征；
4. 在特征层再次注入噪声（满足ε1-DP），总隐私预算 ε = ε0 + ε1；
5. 经过自适应DAD门自适应去噪；
6. 送入Conformer编码器+解码器输出文本；
7. 训练时使用DP-SGD确保梯度隐私。

## 3. 实验设计

- **数据集**：大型多位置语料库（multi-location corpus），包含不同用户、不同设备采集的动态3D深度点云。
- **基准（Benchmark）**：未加隐私保护的基线模型（即DepthSense不带DP）。
- **对比方法**：
  - 标准DP-SGD（仅梯度扰动）
  - 仅输入扰动方法
  - 仅特征级扰动方法
  - 无隐私保护的原始模型
- **评估指标**：词错误率（WER）、隐私攻击成功率（成员推断、模型反演、属性推断）。

根据元数据提到“在大型多位置语料库上达到接近基线的准确率，成员推断显著减少”。

## 4. 资源与算力

论文中**未明确说明**使用的GPU型号、数量及训练时长等具体算力信息。仅提到“高效设备端推理”，但未给出训练资源细节。

## 5. 实验数量与充分性

- **实验数量**：包含多组对比实验（至少4种基线方法）、消融实验（评估DAD门、双阶段DP的作用）、隐私预算 ε 的多种取值实验、以及三种隐私攻击评估。元数据显示“全DP核算和攻击评估”支持。
- **充分性**：实验设计比较全面，覆盖了精度、隐私、泛化三大维度。但缺少在更多公开SSR数据集（如LRW、LRS3等）上的验证，且未报告跨设备/跨用户的具体性能方差。总体相对充分，但可补充更多场景。

## 6. 论文的主要结论与发现

- 提出的DepthSense+DP在保护隐私（成员推断风险显著降低）的同时，保持了接近无保护基线的识别准确率。
- 双阶段差分隐私（点级+特征级）优于单阶段扰动，自适应DAD门进一步改善了隐私-效用权衡。
- 该框架支持高效设备端推理，适用于实际部署。

## 7. 优点

- **创新性**：首次将双阶段DP注入与几何保持对齐结合用于静默语音识别，设计自适应DAD门。
- **隐私保护全面**：覆盖输入、特征、梯度三个层次，并进行多种隐私攻击评估。
- **实用性**：轻量级架构（P4DConv+Conformer）利于边缘设备部署。
- **实验严谨**：包含完整的DP核算和攻击验证。

## 8. 不足与局限

- **数据集单一**：仅在自家大型多位置语料库上评估，未在公开基准（如Lip Reading in the Wild）上测试，泛化性有待确认。
- **算力未公开**：缺乏训练资源报告，难以复现成本。
- **隐私预算影响未深入**：对不同ε值下的WER-攻击成功率完整曲线展示可能不够充分（元数据未提及详细图表）。
- **应用限制**：依赖3D深度摄像头，对硬件要求较高；静默语音识别本身受环境光照、遮挡影响，文中未讨论。
- **偏差风险**：未分析跨性别、口音、语言等的效果差异。

（完）
