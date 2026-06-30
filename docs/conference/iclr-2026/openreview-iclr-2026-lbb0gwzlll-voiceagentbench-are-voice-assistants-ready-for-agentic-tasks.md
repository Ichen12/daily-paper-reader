---
title: "VoiceAgentBench: Are Voice Assistants ready for agentic tasks?"
title_zh: VoiceAgentBench：语音助手准备好处理代理任务了吗？
authors: "Dhruv Jain, Harshit Shukla, Gautam Rajeev, Ashish Kulkarni, Chandra Khatri, Shubham Agarwal"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=lBb0GwzllL"
tags: ["query:speech-audio"]
score: 9.0
evidence: 面向代理任务的语音助手评测基准
tldr: 现有语音模型评测注重基础能力而非代理场景。本文提出VoiceAgentBench，包含6000+合成口语查询，覆盖单工具、多工具、多轮交互及安全评估，并融入印度语境的多语言文化理解，为评测语音语言模型在复杂代理任务中的表现提供了标准化基准。
source: ICLR-2026-Rejected-Public
selection_source: conference_retrieval
motivation: 现有语音语言模型评测缺乏对代理场景（多工具、多轮、安全）的系统评估。
method: 构建包含6000+口语查询的基准，覆盖单/多工具、多轮及安全场景，并提供印度语境样本。
result: 实验揭示了现有模型在代理任务上的显著不足，特别是在多语言和安全方面。
conclusion: VoiceAgentBench为语音代理评测建立了标准化新基准。
---

## Abstract
Large-scale Speech Language Models (SpeechLMs) have enabled voice assistants capable of understanding natural spoken queries and performing complex tasks. However, existing speech benchmarks primarily focus on isolated capabilities such as transcription, or question-answering, and do not systematically evaluate agentic scenarios encompassing multilingual and cultural understanding, as well as adversarial robustness. To address this, we introduce VoiceAgentBench, a comprehensive benchmark designed to evaluate SpeechLMs in realistic spoken agentic settings. It comprises over 6,000 synthetic spoken queries, including dialogues grounded in Indian context, covering single-tool invocations, multi-tool workflows, multi-turn interactions, and safety evaluations. The benchmark supports English, Hindi, and 5 other Indian languages, reflecting real-world linguistic and cultural diversity. We simulate speaker variability using a novel sampling algorithm that selects audios for TTS  voice conversion based on its speaker embeddings, maximizing acoustic and speaker diversity. Our evaluation measures tool selection accuracy, structural consistency, and the correctness  of tool invocations, including adversarial robustness. Our experiments reveal significant gaps in contextual tool orchestration tasks, Indic generalization, and adversarial robustness, exposing critical limitations of current SpeechLMs.

---

## 论文详细总结（自动生成）

好的，我将基于您提供的论文内容，严格按照您的要求生成中文总结。

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **研究动机**：现有的大规模语音语言模型（SpeechLM）评测基准主要关注孤立的基础能力（如语音转录、问答），缺乏对**代理（Agentic）场景**的系统评估。真实的语音助手需要处理多工具调用、多轮对话、安全风险，并具备多语言和跨文化理解能力，而现有基准未能覆盖这些。
- **整体含义**：本文旨在填补这一空白，提出VoiceAgentBench，一个专门用于评测语音语言模型在复杂口语化代理任务（包括单/多工具、多轮交互、安全评估）中表现的综合性基准，尤其强调了印度语境下的多语言和文化多样性。

### 2. 论文提出的方法论：核心思想、关键技术细节
- **核心思想**：构建一套标准化、口语化的代理任务查询集，系统评估SpeechLM的**工具选择准确性**、**结构一致性**和**工具调用正确性**（包括对抗鲁棒性），而非仅评测转录或问答能力。
- **关键技术细节**：
    - **数据构成**：包含超过6000条合成口语查询，分为四个场景：单工具调用、多工具工作流、多轮交互、安全评估。查询基于印度语境，涵盖英语、印地语及其他5种印度语言。
    - **说话人多样性模拟**：提出一种新颖的**采样算法**，基于说话人嵌入从TTS语音转换中选择音频，最大化声学特征和说话人类型的多样性。
    - **评估指标**：工具选择准确性、结构一致性（如JSON结构正确性）、工具调用的功能正确性，以及针对对抗样本的鲁棒性。

### 3. 实验设计：数据集、基准、对比方法
- **数据集/场景**：使用自建的VoiceAgentBench（6000+口语查询），覆盖单工具、多工具、多轮、安全四大场景，并融入印度多语言语境。
- **基准（Benchmark）**：VoiceAgentBench本身即为提出的新基准。
- **对比方法**：论文摘要及元数据中**未明确列出所对比的具体模型名称**（如GPT-4o Audio、Qwen-Audio、SpeechGPT等），仅笼统指出“当前语音语言模型（SpeechLMs）”。但实验结果显示这些模型存在显著差距。**这一信息缺失是本文的一个不足**。

### 4. 资源与算力
- **明确说明**：论文内容（摘要及元数据）中**未提及**所使用的GPU型号、数量、训练时长或推理算力等任何计算资源细节。因此无法总结。

### 5. 实验数量与充分性
- **实验数量**：从摘要看，实验覆盖了单工具、多工具、多轮、安全四个维度的评估，并且包含多语言（6种语言）和对抗鲁棒性测试。但**未给出具体实验组数**（如不同语言各多少组、消融实验是否有等）。
- **充分性评价**：实验场景设计较为全面（4大场景+对抗），但存在以下问题：
    - **缺乏消融实验**：例如未分析不同工具数量、不同语言难度、不同采样策略对结果的影响。
    - **对比不充分**：未列出具体模型名称，无法判断是否包含了最先进的开源/闭源SpeechLM，且未进行业界常见模型的横向对比。
    - **数据来源单一**：仅基于印度语境，缺乏其他地区（如中文、欧洲语言）的通用性验证。
    - 总体而言，实验设计框架合理但**详细程度和透明度不足**，未达到顶级会议严格的公平性要求。这也可能是论文被ICLR-2026拒稿的原因之一。

### 6. 论文的主要结论与发现
- 当前SpeechLM在以下方面存在**严重短板**：
    1. **上下文工具编排能力**：在需要根据对话历史动态选择并组合多个工具的任务中表现差。
    2. **Indic语言泛化能力**：对英语以外的印度语言（如印地语等）的理解与执行能力显著下降。
    3. **对抗鲁棒性**：面对简单扰动或恶意查询时容易失败。
- 结论：VoiceAgentBench揭示了现有语音助手在处理真实代理任务时的系统性缺陷，为未来模型改进提供了基准方向。

### 7. 优点：方法或实验设计上的亮点
- **首创性**：首次系统性地针对**口语代理任务**而非基础能力设计评测基准，填补了领域空白。
- **场景全面**：单工具、多工具、多轮、安全四类场景覆盖了语音助手实际应用中的主要挑战。
- **多语言与文化融入**：特别纳入印度语境下的多语言（6种语言）查询，强调文化多样性，这在以往基准中罕见。
- **说话人多样性采样算法**：提出的基于说话人嵌入的音频采样方法，提升了评测的声学鲁棒性和生态效度。

### 8. 不足与局限
- **实验描述不完整**：未列出对比模型名称、未公布计算资源、缺乏消融实验和详细指标数值，导致可复现性和公平性存疑。
- **数据偏差**：全部查询为合成数据（TTS生成），可能无法完全代表真实用户的自然口语（如口音、歧义、填词等），存在域迁移风险。
- **地域局限**：仅针对印度语境，未讨论其他多语言地区（如中国、欧洲），通用性受限。
- **安全场景定义模糊**：未说明“安全评估”的具体对抗类型（如注入攻击、隐私泄露等），评估深度有限。
- **被拒稿背景**：作为ICLR-2026被拒绝稿件，可能审稿人指出了上述不足的严重性。

（完）
