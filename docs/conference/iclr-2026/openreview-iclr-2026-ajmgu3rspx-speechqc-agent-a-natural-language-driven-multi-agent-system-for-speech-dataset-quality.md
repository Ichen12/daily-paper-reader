---
title: "SpeechQC-Agent: A Natural Language Driven Multi-Agent System for Speech Dataset Quality"
title_zh: SpeechQC-Agent：自然语言驱动的多智能体语音数据集质量控制系统
authors: "Rishabh Kumar, Abhinav Painuli, Chriss Philip Saji, Devesh Soni, Ganesh Ramakrishnan"
date: 2025-09-19
pdf: "https://openreview.net/pdf?id=AJMgU3Rspx"
tags: ["query:speech-audio"]
score: 8.0
evidence: 语音数据集质量控制框架
tldr: 大规模语音数据集的质量验证依赖人工且领域固定。SpeechQC-Agent 提出首个自然语言驱动的多智能体框架，通过中央规划器将用户查询分解为 DAG 工作流，子智能体结合工具与 LLM 合成函数，实现跨模态、跨供应商、跨语言的灵活可扩展验证。该设计支持并行与依赖管理，为数据集质量评估提供了通用基准。
source: ICLR-2026-Public
selection_source: conference_retrieval
motivation: 当前数据集质量验证管道静态、领域特定且依赖人工。
method: 提出自然语言驱动的多智能体框架，中央LLM规划器将查询分解为DAG工作流，子智能体执行可复用工具。
result: 实现了跨模态、供应商和语言的通用可扩展验证。
conclusion: SpeechQC-Agent 为语音数据集质量评估提供了灵活通用的解决方案。
---

## Abstract
Ensuring the quality of large-scale datasets is a prerequisite for reliable machine learning, yet current verification pipelines are static, domain-specific, and heavily reliant on human experts. We introduce **SpeechQC-Agent**, the first natural language-driven agentic framework for dataset quality control that generalizes across modalities, vendors, and languages. A central planner LLM decomposes user queries into directed acyclic graph (DAG) workflows executed by modular sub-agents that combine reusable tools with LLM-synthesized functions, enabling flexible and scalable verification. Unlike rule-based scripts, this design supports parallelism, dependency management, and adaptive extension to novel schemas. To benchmark verification systems, we release **SpeechQC-Dataset**, a multilingual speech corpus with controlled perturbations spanning audio, transcripts, and metadata, allowing systematic evaluation of 24 verification tasks. Experiments show that SpeechQC-Agent achieves 80-90\% of expert level accuracy while operating at less than 20\% of cost and time and generalizes from synthetic perturbations to real vendor-supplied corpora. Comparative analysis across multiple planner LLMs highlights trade-offs between fidelity (GPT-4.1-mini), efficiency (LLaMA-3.3-70B), and reasoning strength (DeepSeek-R1). Beyond speech, our approach establishes a general paradigm for LLM-driven workflow generation in dataset quality assurance, with implications for the curation of multimodal and multilingual resources on scale.

---

## 论文详细总结（自动生成）

### 1. 论文的核心问题与整体含义（研究动机和背景）

- **研究动机**：大规模语音数据集的质量验证是机器学习可靠性的前提，但现有验证管道静态、领域特定且高度依赖人工专家，缺乏灵活性和可扩展性。
- **核心问题**：如何设计一个自然语言驱动的、跨模态、跨供应商、跨语言的通用数据集质量验证框架，以替代传统规则脚本和人工检查。
- **整体含义**：提出首个此类框架，为数据集质量保证提供可泛化的LLM驱动工作流生成范式，对多模态、多语言资源的大规模管理具有重要价值。

### 2. 论文提出的方法论

- **核心思想**：采用自然语言驱动的多智能体系统（SpeechQC-Agent），中央规划器LLM将用户查询分解为有向无环图（DAG）工作流，由模块化子智能体执行，子智能体结合可复用工具与LLM合成函数，实现灵活、可扩展的验证。
- **关键技术细节**：
  - **中央规划器**：使用LLM（如GPT-4.1-mini、LLaMA-3.3-70B、DeepSeek-R1）解析用户自然语言查询，生成DAG工作流，支持并行与依赖管理。
  - **子智能体**：每个子智能体负责特定验证任务（如音频、文本、元数据检查），可调用预定义工具或由LLM动态合成新函数。
  - **自适应扩展**：支持新模式的即插即用，无需重新设计规则脚本。
- **算法流程**（文字说明）：
  1. 用户输入自然语言查询（例如“检查音频采样率与元数据是否一致”）。
  2. 中央LLM规划器解析查询，分解为一系列可并行或串行执行的子任务，形成DAG结构。
  3. 子智能体接收子任务，调用对应工具（如FFmpeg检测采样率、Whisper转录比对等）或利用LLM生成临时验证函数。
  4. 子智能体执行任务并返回结果，规划器汇总形成最终质量报告。

### 3. 实验设计

- **数据集与场景**：
  - 作者释放了**SpeechQC-Dataset**，一个多语言语音语料库，包含对音频、文本、元数据的受控扰动（perturbations），覆盖24个验证任务。
  - 额外在真实供应商提供的数据集上评估泛化能力（从合成扰动到真实场景）。
- **基准（Benchmark）**：以人工专家验证结果为基线，对比SpeechQC-Agent的准确率、成本和时间。
- **对比方法**：
  - 未明确列出其他方法，但文中提到“与基于规则脚本的对比”，以及在不同规划器LLM（GPT-4.1-mini、LLaMA-3.3-70B、DeepSeek-R1）间的比较。
  - 可能还包括无多智能体架构的基线（如直接使用单一LLM验证）。

### 4. 资源与算力

- 论文中**未明确说明**使用了多少算力（GPU型号、数量、训练时长等）。仅提及使用不同LLM作为规划器（如GPT-4.1-mini闭源、LLaMA-3.3-70B开源等），但未提供硬件配置细节。

### 5. 实验数量与充分性

- **实验数量**：在SpeechQC-Dataset上系统评估了24个验证任务，覆盖音频、文本、元数据三类扰动，同时对比了多种规划器LLM，并测试了从合成扰动到真实供应商数据的泛化。
- **充分性**：实验规模较为充分，涉及多任务、多LLM变体，以及真实场景外推。但论文未提供消融实验（如去掉中央规划器或子智能体等组件的效果），也未报告统计显著性。整体客观性较好，但可进一步补充。

### 6. 论文的主要结论与发现

- SpeechQC-Agent能达到人工专家级别80-90%的准确率，同时成本和时间均低于专家验证的20%。
- 规划器LLM之间存在权衡：GPT-4.1-mini在保真度（fidelity）上最优，LLaMA-3.3-70B效率最高，DeepSeek-R1推理能力最强。
- 框架具备从合成扰动到真实供应商数据的良好泛化能力。
- 该工作建立了LLM驱动工作流生成在数据集质量保证中的通用范式。

### 7. 优点

- **创新性**：首个自然语言驱动的多智能体数据集质量控制框架，解决了传统静态管道的局限。
- **灵活性**：支持DAG并行与依赖管理，可动态扩展新验证模式，无需硬编码。
- **实用性**：在成本和效率上显著优于人工专家，且易于集成到现有流程中。
- **基准贡献**：释放了SpeechQC-Dataset，为后续研究提供了标准评估基准。

### 8. 不足与局限

- **实验覆盖**：仅评估了语音数据集，未在图像、文本等其他模态上验证泛化性，尽管论文声称范式通用。
- **偏差风险**：依赖LLM规划器的能力，不同LLM表现差异较大，且闭源模型（如GPT系列）可能存在查询成本或可重复性问题。
- **应用限制**：
  - 需要LLM API或本地部署，可能引入延迟或成本。
  - 对于高度复杂或需要领域专家知识的验证任务（如语义理解），8-90%准确率可能不满足极高可靠性要求（如医疗数据）。
  - 缺乏对恶意对抗扰动（adversarial perturbations）的鲁棒性评估。
- **方法论局限**：子智能体合成函数的正确性依赖于LLM能力，存在幻觉风险；规划器生成的DAG可能过于简单或错误。

（完）
