---
title: "Brain2voice 2.0: High-performance voice synthesis brain-computer interface"
title_zh: 脑声2.0：高性能语音合成脑机接口
authors: "Wairagkar, M., Srinivasan, A., Card, N. S., Singer-Clark, T., Hou, X., Iacobacci, C., Miller, L. M., Hochberg, L. R., Brandman, D. M., Stavisky, S. D."
date: 2026-07-06
pdf: "https://www.biorxiv.org/content/10.64898/2026.06.30.735633v1.full.pdf"
tags: ["query:speech-audio"]
score: 8.0
evidence: 从脑信号合成语音
tldr: "脑机接口（BCI）有望恢复因神经损伤导致的言语功能，但现有脑到声音BCI合成的语音清晰度不足，无法实际应用。本文提出Brain2voice 2.0多模态Transformer解码器，联合连续声学特征和音素目标进行训练，并结合自监督与对抗学习提升语音质量。系统在10毫秒步长内因果输出声学特征和音素预测，实现实时合成。在颅内脑信号基准数据集上，人类听写词错误率从先前最优的43.75%降至5.24%，清晰度提升8倍，首次达到临床可用阈值。"
source: biorxiv
selection_source: fresh_fetch
motivation: 现有脑到声音BCI合成语音清晰度不足，无法满足临床实际应用需求。
method: 提出多模态Transformer解码器，联合连续声学特征和音素目标训练，并采用自监督与对抗学习提升语音质量。
result: "在颅内脑信号基准上，人类听写词错误率从43.75%降至5.24%，清晰度提升8倍。"
conclusion: 首次实现临床可行的实时高清晰度语音合成，为瘫痪患者提供有效的脑机接口语音恢复方案。
---

## 摘要
脑机接口（BCI）通过直接从大脑活动中解码预期语音，为因神经损伤导致的言语丧失提供了一种有前景的解决方案。尽管近期的BCI已经恢复了高精度的基于文本的通信，但它们无法提供自然对话流程所必需的即时语音输出。脑到语音BCI通过从神经信号中直接解码语音来弥补这一差距。然而，即便是最先进的BCI合成语音，其可理解性也尚未达到实际应用的水平。我们提出了脑声2.0，一种新的基于多模态Transformer的BCI解码器架构，能够从皮层内神经信号中实时合成高度可理解的语音。脑声2.0在连续且自定义分词化的声学目标以及音素目标上进行训练，利用它们互补的语音信息。我们采用了自监督和对抗训练目标，以增强声学特征质量并提高合成可理解性。在每个10毫秒的时间步，模型因果地输出连续和分词化的声学特征以进行实时语音合成，以及时间对齐的音素预测（原始音素错误率：7%，与最新的脑到文本模型相当）。我们在先前的皮层内脑到语音基准数据集（Wairagkar等人，2025）上评估了这种新方法。天真的人类听写者转录脑声2.0合成语音的单词错误率为5.24%——与之前最先进的结果（43.75%）相比，可理解性提高了8倍。脑声2.0首次证明了从神经信号中实现高度可理解的实时语音合成是可行的，跨越了临床可行的脑到语音BCI对于瘫痪患者所需的可理解性阈值。

## Abstract
Brain-computer interfaces (BCIs) offer a promising solution to speech loss due to neurological injury by decoding intended speech directly from brain activity. While recent BCIs have restored high-accuracy text-based communication, they fail to provide instantaneous voice output essential for the natural flow of conversation. Brain-to-voice BCIs address this gap by decoding voice directly from neural signals. However, even the state-of-the-art (SOTA) BCI-synthesized voice is not yet intelligible enough for real-world adoption. We introduce brain2voice 2.0, a new multimodal Transformer-based BCI decoder architecture capable of synthesizing highly intelligible voice from intracortical neural signals in real-time. Brain2voice 2.0 is trained on continuous and custom-tokenized acoustic targets and phoneme targets, leveraging their complementary speech information. We use self-supervised and adversarial training objectives that enhance acoustic feature quality and improve synthesis intelligibility. At each 10 ms timestep, the model causally outputs continuous and tokenized acoustic features for real-time voice synthesis as well as time-aligned phoneme predictions (raw phoneme error rate: 7%, comparable to the latest brain-to-text models). We evaluated this new approach on our prior intracortical brain-to-voice benchmark dataset (Wairagkar et al. 2025). Naive human listeners transcribed brain2voice 2.0 synthesized voice with a word error rate of 5.24%--an 8x improvement in intelligibility over previous SOTA results (43.75%). Brain2voice 2.0 demonstrates that highly intelligible real-time voice synthesis from neural signals is achievable, for the first time crossing the intelligibility threshold necessary for clinically viable brain-to-voice BCIs for people with paralysis.

---

## 论文详细总结（自动生成）

## 论文详细中文总结

### 1. 论文的核心问题与整体含义（研究动机和背景）
- **背景**：脑机接口（BCI）能够直接从大脑活动中解码预期语音，恢复因神经损伤导致的言语丧失。现有“脑到文本”BCI取得了高精度文本通信，但无法提供自然对话所需的即时语音输出；而“脑到语音”BCI虽能直接合成语音，但即便是最先进的系统（Wairagkar et al. 2025），合成语音的可理解性也严重不足——人类听写单词错误率（WER）高达43.75%，远未达到临床实际应用的门槛。
- **核心问题**：如何从高分辨率皮层内神经信号中实时合成高度可理解的语音，使WER降低到可接受水平（例如<10%）？现有方法受限于神经信号分辨率、训练数据有限、以及缺乏合适的模型架构。
- **整体含义**：本文提出Brain2voice 2.0，首次实现了临床可行的、高度可理解的实时语音合成（WER降至5.24%），为瘫痪患者提供了有效的语音恢复方案，有望显著改善其生活质量和自主性。

### 2. 论文提出的方法论：核心思想、关键技术细节
#### 核心思想
- 构建一个**多模态因果Transformer解码器**，同时学习连续声学特征、分词化声学特征和音素三种互补的语音表征，结合自监督学习（SSL）和对抗训练（GAN）增强声学特征质量，从而大幅提升合成语音的可理解性。

#### 关键技术细节
- **输入**：皮层内神经信号（256个微电极，512维神经特征，10 ms bins）。
- **架构**：线性投影 + 正弦位置编码 → 8层因果Transformer编码器（隐藏大小384，6头自注意力）→ 四个并行输出头：
  1. **连续声学头**：预测20维LPCNet特征（回归，MSE损失），用于直接语音合成。
  2. **分词声学头**：预测8个码本的离散LPCNet token（分类，交叉熵损失），通过自定义RVQ分词器将连续特征离散化。
  3. **音素头**：预测39个音素+silence+blank（CTC损失），提供语言级指导。
  4. **自监督学习（SSL）头**：预测掩蔽位置Transformer隐藏状态（余弦相似度损失），正则化改善泛化。
- **对抗训练**：多尺度鉴别器（4个时间尺度：10 ms、40 ms、160 ms、320 ms）对连续声学特征进行判别，生成器损失和特征匹配损失提升感知质量。
- **自定义RVQ分词器**：基于残差矢量量化（8个码本，每个128个质心），直接在LPCNet特征空间操作，保留说话人特性、可逆、时间对齐精确。
- **损失函数**：总损失 = 加权连续损失 + 加权分词损失 + 0.1×音素CTC损失 + 对抗损失 + 2.0×特征匹配损失 + 0.3×SSL损失。连续和分词损失使用不确定加权自动调整，其他固定。
- **实时推理**：滑动窗口800 ms，每次步进10 ms，仅取最后预测帧，总计算时间<10 ms（连续1.47 ms，分词2.11 ms，声码器1.2 ms）。

### 3. 实验设计：数据集、基准、对比方法
- **数据集**：使用先前公开发布的皮层内BCI数据集（Wairagkar et al. 2025），来自一名ALS患者（严重构音障碍），4个微电极阵列（256通道）置于腹侧中央前回，195天记录，共8,489个试验。保留128个试验作为测试集（基准集和视频集）。
- **基准**：对比对象是同一数据集上的先前最先进（SOTA）结果（Brain2voice 1.0，WER 43.75%）。
- **评估方式**：
  - **人类听写**：每个测试音频由7名天真听写者独立转录，计算中位数词错误率（WER）和音素错误率（PER）。
  - **自动语音识别（ASR）**：Whisper模型转录，用于一致性比较。
  - **客观声学质量**：Pearson相关系数（Mel频谱）和梅尔倒谱失真（MCD）。
  - **音素预测**：原始音素错误率（无语言模型）。

### 4. 资源与算力
- **训练硬件**：单个NVIDIA RTX 5090 GPU。
- **训练时长**：约1.5小时。
- **推理速度**：每10 ms步长，连续特征预测1.47±0.11 ms，分词特征预测2.11±0.21 ms，声码器帧合成1.2 ms，总计<10 ms，满足实时要求。
- 未明确说明使用的CPU、内存等其他资源。

### 5. 实验数量与充分性
- **主要实验**：
  - 人类听写评估（基准集和视频集）报告了WER和PER，置信区间（95% CI）。
  - ASR转录评估。
  - 客观声学质量指标（Pearson r、MCD）。
  - 音素预测PER。
- **消融实验**（附录A.3）：8种模型配置，逐步添加组件（原始目标、分词头、音素头、GAN、SSL、改进目标、新自身声音目标），每个配置在基准集上由5-7名听写者评估。
- **电极实验**（附录A.4）：随机选择64/128/192/256电极训练模型，以及单阵列模型，评估WER。
- **充分性**：实验覆盖了多个组件贡献、不同电极数量、不同阵列位置，对比了主要指标，且统计上提供了置信区间。消融实验系统逐步揭示每个组件的增量效果，设计较为全面、客观。
- **潜在不足**：所有测试数据均来自同一参与者，且为离线模拟实时推理（非实际闭环BCI），但论文指出这是与已有工作的标准比较方式。

### 6. 论文的主要结论与发现
- **核心成果**：Brain2voice 2.0合成语音的人类听写WER为5.24%（连续输出）、5.65%（分词输出），相比先前SOTA（43.75%）提升约8倍，首次跨越临床可用可理解性阈值。
- **其他发现**：
  - 连续和分词输出达到相似可理解性，表明两种表示均成功捕获语音模式。
  - 音素预测原始PER为7%（基准集），与最新脑到文本BCI相当，且自动时间对齐到语音。
  - 多尺度鉴别器改善了感知质量；SSL稳定训练；高质量自身声音目标（克隆自参与者发病前语音）显著提升效果。
  - 所有四个电极阵列对最高性能均必要，其中v6v阵列贡献最大。
- **结论**：高度可理解的实时神经语音合成是可行的，为下一代语音神经假体提供了坚实的架构基础。

### 7. 优点：方法或实验设计上的亮点
- **创新性架构**：多模态联合学习连续、离散和语言级表征，利用互补信息。
- **自定义RVQ分词器**：保留说话人特定属性（语速、声纹、构音障碍特征），可逆且时间对齐精确，优于通用tokenizer。
- **因果实时推理**：全因果设计，延迟<10 ms，可直接部署于闭环BCI。
- **全面消融实验**：逐步验证每个组件的贡献，包括目标质量、辅助头、对抗训练、SSL等。
- **客观与主观评估结合**：既有人类听写（最可靠的可理解性指标），又有ASR和客观声学指标，结果一致。
- **结果统计严谨**：报告均值、置信区间、中位数等，提供了统计可靠性。

### 8. 不足与局限
- **单一参与者**：仅来自一名ALS患者（失语但保留发声尝试），泛化性未知。未来需在多参与者（包括完全失语）中验证。
- **离线模拟**：评估是离线模拟实时推理，未在实际闭环BCI（含听觉反馈）中测试；闭环中用户可能适应模型输出，效果可能不同。
- **依赖地面真值语音时间**：需要至少一次“种子”会话的语音时间标注（如启动词时间），对于完全无法发声的患者，该初始标注方法需进一步开发。
- **结构化任务**：数据来自视觉提示的句子复述，而非自发生成语音；在自发性交流中的性能未知。
- **训练数据规模有限**：尽管有8000+试验，但均为同一人、有限句子集；扩展新词汇或风格需重新训练tokenizer。
- **算力信息不够详细**：仅提及单个RTX 5090及其训练时间，未提供超参数调优的具体搜索范围等细节。

（完）
