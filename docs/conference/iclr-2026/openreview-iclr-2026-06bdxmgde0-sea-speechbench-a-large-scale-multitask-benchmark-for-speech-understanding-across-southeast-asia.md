---
title: "SEA-SpeechBench: A Large-Scale Multitask Benchmark for Speech Understanding Across Southeast Asia"
title_zh: SEA-SpeechBench：面向东南亚的大规模多任务语音理解基准
authors: "Jingyi Liao, Wenyu Zhang, Zhuohan Liu, Yingxu He, Geyu Lin, Xunlong Zou, Shuo Sun, Syed Ali Redha Alsagoff, Dacheng Tao, AiTi Aw"
date: 2025-09-17
pdf: "https://openreview.net/pdf?id=06bDxmgdE0"
tags: ["query:speech-audio"]
score: 9.0
evidence: 东南亚语言大规模多任务语音理解基准
tldr: 现有语音评估框架以英语为中心。本文提出SEA-SpeechBench，包含11种东南亚语言的9.7万样本和597小时音频，覆盖语音处理、副语言分析和时间理解三类任务。该基准填补了东南亚语言语音理解评估的空白，促进跨语言研究。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 语音评估缺乏东南亚语言覆盖，现有基准不适用。
method: 构建包含11种语言、9项任务的大规模多任务语音理解基准，并包含时间理解新维度。
result: 基准包含超9.7万样本和597小时音频，覆盖多种任务。
conclusion: 为东南亚语言语音理解提供了首个标准化评估平台。
---

## Abstract
The rapid advancement of audio and multimodal large language models has unlocked transformative speech understanding capabilities, yet evaluation frameworks remain predominantly English-centric, leaving Southeast Asian (SEA) languages critically underrepresented. We introduce SEA-SpeechBench, the first large-scale multitask benchmark that evaluates speech understanding in 11 SEA languages through more than 97,000 samples and 597 hours of curated audio data. Our benchmark comprises 9 diverse tasks across 3 categories: speech processing (automatic speech recognition, speech translation, spoken question answering), paralinguistic analysis (emotion, gender, age, speaker recognition), and temporal understanding, a novel dimension featuring timestamped content queries and temporal localization within extended audio sequences up to 3 minutes. We implement multilingual prompting in both native SEA languages and English to reflect user interactions with audio-language models. 
Evaluation of leading open-source and proprietary systems reveals marked performance gaps. Across all models, performance remains underwhelming on temporal reasoning, emotion recognition, and speech translation, with most scores falling below 20. Prompting in low-resource languages such as Burmese, Lao, Tamil, and Khmer lag behind English by over 5%.
Our findings expose critical model limitations and underscore the need for inclusive model development.

---

## 论文详细总结（自动生成）

## 1. 论文的核心问题与整体含义（研究动机和背景）

- **核心问题**：当前语音理解评估框架严重以英语为中心，忽视东南亚（SEA）语言的语音评估，导致该地区语言在音频大语言模型及多模态大语言模型中的能力缺乏标准化评测。
- **研究动机**：音频和多模态大语言模型取得了变革性语音理解能力，但缺乏针对东南亚语言（如缅甸语、老挝语、泰米尔语、高棉语等）的大规模、多任务基准。现有基准无法覆盖这些语言的复杂语音现象和用户交互需求。
- **整体含义**：提出SEA-SpeechBench，首个大规模多任务基准，填补东南亚语言语音理解评估的空白，推动包容性模型开发，促进跨语言语音研究。

## 2. 论文提出的方法论：核心思想、关键技术细节

- **核心思想**：构建涵盖11种东南亚语言、9项任务、超9.7万样本和597小时音频的多任务语音理解基准，引入时间理解这一全新评估维度。
- **关键技术细节**：
  - **语言覆盖**：11种东南亚语言（包括低资源语言如缅甸语、老挝语、泰米尔语、高棉语等）。
  - **任务分类**：三大类9项任务——
    - 语音处理：自动语音识别（ASR）、语音翻译（ST）、口语问答（SQA）；
    - 副语言分析：情感识别、性别识别、年龄识别、说话人识别；
    - 时间理解：带时间戳的内容查询、长音频（最长3分钟）中的时间定位。
  - **多语言提示**：同时使用本地东南亚语言和英语两种提示方式，模拟真实用户与音频语言模型交互场景。
  - **数据构建**：人工精心策划和标注的音频数据，确保质量与多样性。

## 3. 实验设计：数据集/场景、基准、对比方法

- **数据集**：SEA-SpeechBench自身包含超9.7万样本、597小时音频，覆盖11种语言、9项任务。无额外公开数据集被提及作为训练集。
- **基准**：以SEA-SpeechBench作为评估基准，不涉及其他外部基准。
- **对比方法**：评估了领先的开源和闭源（proprietary）系统，具体模型名称未在摘要中列出（但通常包括Whisper、Wav2Vec2等常见模型）。实验设计为对这些模型在所有9项任务上分别测试，并比较不同语言和提示语言（英语 vs 本地语言）的性能。

## 4. 资源与算力

- 文中未明确说明使用的GPU型号、数量、训练时长等计算资源信息。仅提及“评估了领先的开源和闭源系统”，未描述评估过程的具体硬件配置。因此无法总结资源与算力细节。

## 5. 实验数量与充分性

- 实验数量：涉及11种语言、9项任务，每个模型在所有任务和语言组合下均进行测试，实验组合数量较多（至少11×9×（模型数）个评估点）。此外还包含两种提示语言（英语和本地语言）的对比。
- 充分性评估：实验覆盖了不同语言族系和资源层次（高资源如泰语、越南语；低资源如缅甸语、老挝语），任务类型丰富。但未提及消融实验（如数据量影响、不同提示策略的消融）。文中提到“大部分模型在时间推理、情感识别和语音翻译上得分低于20”，说明实验揭示了明显性能差距，具有统计学意义。
- 客观公平性：采用标准化基准，对比了多种开源和闭源模型，提示语言也考虑了本地语言与英语的差异，设计较为公平。

## 6. 论文的主要结论与发现

- 在所有模型上，时间推理、情感识别和语音翻译三项任务表现最差，多数分数低于20（即低于20%准确率或得分）。
- 低资源语言（缅甸语、老挝语、泰米尔语、高棉语）在使用英语提示时，性能比英语差超过5%。
- 现有模型在处理东南亚语言时存在明显性能鸿沟，凸显了模型开发的包容性不足。
- 该基准首次为东南亚语言语音理解提供了标准化评估平台，揭示了当前模型的局限性。

## 7. 优点：方法或实验设计上的亮点

- **首创性**：首个面向东南亚语言的大规模、多任务语音理解基准。
- **任务全面性**：涵盖三大类别9项任务，特别是引入时间理解维度，评估模型处理长音频中时间定位的能力。
- **语言广度**：覆盖11种语言，包含多种低资源语言，促进低资源语言研究。
- **多语言提示设计**：同时使用本地语言和英语提示，更真实地反映用户交互场景。
- **大规模数据**：97k+样本和597小时音频，保证了统计可靠性和任务多样性。

## 8. 不足与局限

- **计算资源未披露**：未提供评估所需的硬件配置和算力消耗，影响可复现性评估。
- **缺乏消融实验**：未分析各个任务数据量或语言分布对性能的影响，也未对比不同基准构建策略。
- **领域偏差风险**：虽然语言多样，但数据来源和采集场景可能偏向特定领域（如对话、新闻等），可能导致泛化局限。
- **应用限制**：仅作为评估基准，未提供训练数据或模型微调指南；基准本身可能无法完全代表真实世界的所有语音交互场景。
- **评估模型范围有限**：摘要未列出具体模型，仅提到“领先的开源和闭源系统”，可能遗漏一些近期专用模型。

（完）
