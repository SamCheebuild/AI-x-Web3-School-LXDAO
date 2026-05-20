---
title: 大语言模型（LLM）
source: https://aiweb3.school/zh/handbook/ai/llm/#%E4%B8%BA%E4%BB%80%E4%B9%88%E8%A6%81%E5%AD%A6%E8%BF%99%E4%B8%AA
author:
  - "[[AI x Web3 School]]"
published:
created: 2026-05-15
description: LLM 不是一个会聊天的黑盒，而是把大量文本、代码和多模态信号压进参数里的概率模型。先理解它如何处理 token、上下文、模式和不确定性，后面才知道什么时候该信它、什么时候必须验证它。 为什么要学这个 很多 AI 应用一开始都会说“我们用大模型”。这句话还不够。你需要继续追问：模型负责理解用户意图，还是生成内容？它只是在回答问题，还是能调用工具？它的错...
tags:
  - clippings
cssclasses:
---
2026-05-12

> LLM 不是一个会聊天的黑盒，而是把大量文本、代码和多模态信号压进参数里的概率模型。先理解它如何处理 token、上下文、模式和不确定性，后面才知道什么时候该信它、什么时候必须验证它。

## 为什么要学这个

很多 AI 应用一开始都会说“我们用大模型”。这句话还不够。你需要继续追问：模型负责理解用户意图，还是生成内容？它只是在回答问题，还是能调用工具？它的错误会停留在文本里，还是会进入真实工作流？

LLM 是今天大多数 AI 应用的能力底座。它能总结文档、写代码、生成计划、抽取结构化信息，也能把自然语言转成更适合程序处理的格式。但它本身不是数据库，不会自动知道外部世界的最新状态，也不能替系统承担事实校验。

学 LLM 的目标不是背模型参数，而是建立一个判断： **模型输出是候选结果，不是事实本身；模型能力是推理入口，不是最终验证。**

## 第一性原理

> **LLM 生成的是概率上合理的输出，而不是天然可信的事实。**

这决定了 LLM 在任何严肃系统里的位置：它可以帮你理解、归纳、生成和规划，但不能单独承担事实来源、权限判断和最终执行。越靠近真实动作，越需要外部数据、确定性规则和人工或系统校验。

- **把模型当推理层，不当真相源** ：关键事实必须来自数据库、API、日志、文档源或其他可信系统。
- **把输出变成可检查对象** ：摘要、分类、计划和操作建议都应该尽量落成 schema、参数、引用或日志，而不是只停留在自然语言。
- **把不确定性前移暴露** ：模型不知道、资料过期、上下文不足时，要让系统显式降级，而不是编一个顺滑答案。

## 知识节点

### Token

Token 是模型处理文本的基本单位。它不一定等于一个汉字、一个英文单词或一个符号，而是 tokenizer 切分后的片段。

Token 直接影响三件事：上下文能放多少、调用成本是多少、模型能不能完整看见关键信息。长文档、代码、JSON、日志和多轮对话都很容易把上下文塞满。你需要决定哪些信息原样放入，哪些先压缩，哪些交给检索系统按需取回。

**不要把“页面很短”误认为“token 很少”** 。代码、JSON、长标识符、表格和混合语言文本经常比普通段落更吃 token。

### Embedding

Embedding 是把文本、代码或其他对象映射成向量，用来衡量“语义上是否接近”。它常用于搜索、聚类、推荐、异常检测和 RAG。

Embedding 适合帮你从文档、知识库、代码仓库、讨论记录和产品日志中找相关材料。但它不适合单独判断“这个结论是否正确”。向量相似度只能说明内容接近，不能替代来源校验、规则检查和人工判断。

相关 topic

- [RAG](https://aiweb3.school/zh/handbook/ai/rag/) ：继续看 embedding 如何进入检索增强生成系统。
- [OpenAI Embeddings Guide](https://platform.openai.com/docs/guides/embeddings) ：理解 embedding 的用途、向量距离和 API 形态。

### Transformer

Transformer 是现代 LLM 的核心架构之一。它的关键能力来自 attention：模型可以在生成时关注输入中的不同位置，学习词、代码、事实和上下文之间的关系。

你不需要一开始就推公式，但要理解一个工程事实：Transformer 擅长在上下文里找模式，不等于它拥有稳定数据库。它能把文档、代码和用户目标组合起来生成解释，也可能因为上下文缺失或相似模式误导而给出错误归纳。

**Transformer 给了模型强大的模式组合能力，但没有给它事实最终裁决权。**

### Hallucination

Hallucination 指模型生成了看起来合理、但并不真实或无法验证的内容。它可能编造 API、错误解释代码、引用不存在的资料，或者把旧版本文档当成当前状态。

在普通问答里，幻觉可能只是答案质量问题。在任何带执行能力的系统里，幻觉都会变成流程风险：错误参数、错误权限解释、错误操作建议，都可能进入后续自动化链路。

处理幻觉不要只靠“写更好的 prompt”。更可靠的方式是把模型输出接到外部校验：来源引用、schema 校验、规则检查、人工确认和审计日志。

相关 topic

- [Evaluation](https://aiweb3.school/zh/handbook/ai/evaluation/) ：用测试集和回归评估持续捕捉模型错误。
- [AI Security](https://aiweb3.school/zh/handbook/bridge/ai-security/) ：看模型错误和攻击输入进入工具执行后的风险。

### Multimodal

Multimodal 模型可以处理文本、图片、音频、视频或屏幕截图。对 builders 来说，它的价值不是“更炫”，而是让模型读懂更多真实工作界面：图表、控制台、设计稿、应用页面、错误截图和确认弹窗。

但多模态输入同样需要边界。截图里的文字可能被遮挡，图表可能缺少坐标，页面可能被伪造。模型可以辅助识别和解释，但关键判断仍要回到结构化数据和可信来源。

## 在 AI x Web3 中的位置

LLM 位在 AI x Web3 系统的理解和生成层。它负责把用户目标转成可讨论的计划，把复杂链上数据解释成人能读的语言，把文档和代码串成可执行思路。

真正的产品通常还需要这些层配合：

- 数据层：RPC、索引器、预言机、向量库、项目文档。
- 编排层：Prompt、Context、RAG、Agent workflow。
- 执行层：工具调用、钱包、Smart Account、合约交互。
- 安全层：Guard、simulation、权限策略、人工确认、日志。

LLM 越靠近执行层，系统越要把它的自然语言输出变成可验证对象。

## 最小实践

做一个“交易解释器”的最小版本。

输入一笔交易哈希，让系统读取交易详情、事件日志和相关合约 ABI，然后让 LLM 生成一段解释。要求输出包含：

- 用户发起了什么动作
- 涉及哪些资产和地址
- 哪些信息来自链上数据，哪些是模型推断
- 模型不确定的地方
- 如果要签类似交易，用户应该检查什么

练习重点不是让解释很漂亮，而是把 **模型生成、链上事实、来源边界、不确定性** 分开。

下面我用“给 PM/业务写产品文档”的视角，帮你把 Embedding、Multimodal、Evaluation、AI Security 各写一版“一句话定义 + 典型用法 + 推荐中文说法”，方便你直接拿去用或微调。

***

## Embedding（嵌入向量）

**给 PM/业务的一句话定义**

- Embedding 是“把文本、图片等内容转换成数字向量，让系统能按语义理解和匹配内容”的那一步。 [cloud.tencent](https://cloud.tencent.com/developer/article/2402242)

**常见产品用法（怎么写在文档里）**

- 我们会说：  
  - “系统会为用户的问题和知识库内容生成嵌入向量，用向量相似度做语义检索。”  
  - “通过文本嵌入向量，产品可以支持按含义搜索，而不仅仅是关键字匹配。” [nvidia](https://www.nvidia.cn/glossary/vector-database/)

**中文 AI 圈常见叫法（推荐写法）**

- 推荐：**嵌入向量（Embedding）** / **向量表示（Embedding）**  
- 首次出现可以写：“我们使用文本嵌入向量（Text Embedding），将用户输入编码为高维向量，用于语义检索和推荐。”

***

## Multimodal（多模态）

**给 PM/业务的一句话定义**

- Multimodal 指“模型可以同时理解和处理多种类型的数据，比如文本、图片、音频、视频，而不是只看文字”。 [version-2.com](https://version-2.com.tw/2026/01/%E7%90%86%E8%A7%A3%E5%A4%9A%E6%A8%A1%E6%85%8B-ai-multimodal-ai/)

**常见产品用法**

- 文档里可以这样描述：  
  - “本产品支持多模态输入，用户可以同时提供文本、图片等信息，由同一个模型统一理解。” [ibm](https://www.ibm.com/cn-zh/think/topics/multimodal-ai)
  - “未来会逐步升级为多模态大模型，支持截图问答、文档 + 图片混合分析。”

**中文 AI 圈常见叫法（推荐写法）**

- 标准：**多模态（Multimodal）**、**多模态大模型**  
- 首次出现可写：“我们计划引入多模态能力（Multimodal），让模型同时理解文本、图片等多种输入。”
在 AI 里，“多模态（multimodal）”本质上是“一个模型/系统同时处理多种模态的数据（文本、图像、音频、视频、传感器等）”。ibm+2

如果你问“Multimodal 的同级概念有什么”，可以从两个维度看：  
1）按“模态数量/关系”这条线上的“同级/邻级”概念；  
2）按“整体 AI 系统架构/任务类型”中与多模态并列的概念。

---

## 一、按模态数量/关系的“同级概念”

这条线就是在回答：“模型在模态上的设计，是哪一种？”

- **Uni-modal（单模态）**：  
    只处理一种模态的数据，比如只处理文本（传统 LLM），或只处理图像（ResNet、ViT）。superannotate+1
    
- **Multimodal（多模态）**：  
    同时处理多种模态，并在内部进行融合和联合推理，比如同时理解图片和文字、语音和文字等。tiledb+2
    
- **Cross-modal / Crossmodal（跨模态）**：  
    关注不同模态之间的对齐、映射和检索关系，比如“以文搜图”、“以图生成文”，本质上是模态间的映射与交互机制，而不是简单地各玩各的。aclanthology+1
    
- **Any-modal / Anymodal（任意模态）**：  
    新一点的说法，强调模型可以在“模态子集缺失或组合变化”时仍然工作，比如某些传感器数据缺失也能鲁棒推理，这在多传感器场景被称为 “anymodal segmentor/learner”。[arxiv](https://arxiv.org/html/2411.17141v1)
    
- 有时也会提到 **multi-sensor**（多传感器）：  
    更偏工程语境，强调来自不同传感器（雷达、摄像头、红外等）的模态组合，通常也属于多模态 AI 的一个子集。[tiledb](https://www.tiledb.com/blog/multimodal-ai-guide)
    

可以粗略理解为一条线：  
Uni-modal → Multi-sensor/Multimodal → Cross-modal → Any-modal（鲁棒到任意组合）

---

## 二、在 AI 系统层面的并列概念

在“AI overall / AI in general” 这个视角下，多模态是描述“输入/表示形式”的一个维度，除此以外还有很多“同级维度”的分类方式，会一起出现：

## 1）按任务/能力维度

这些是和“是否多模态”正交、但经常一起讨论的标签：

- **Perception vs Reasoning**  
    感知（识别物体、听懂语音） vs 推理（规划、逻辑、数学、工具使用）。多模态更多解决感知输入的丰富性，但可以和强推理的 LLM 结合。tutai+1
    
- **Generative vs Discriminative**  
    生成式（text-to-image、image captioning 等） vs 判别式（分类、检测）。多模态可以出现在两边，比如多模态分类器、多模态生成模型。superannotate+1
    
- **Foundation model / LLM / VLM / Audio model**
    
    - LLM：只看文本的基础大模型（单模态）。[tutai](https://tutai.ai/en/blog/multimodal-ai-vision-audio-text-convergence)
        
    - VLM（Vision-Language Model）：典型多模态（图像+文本）。kdnuggets+1
        
    - Audio(-language) model：语音 + 文本。  
        这些都是基础模型类型，多模态是其中一种演化方向。
        

## 2）按交互/系统架构维度

多模态经常和这些架构概念一起出现，是“平行的系统设计维度”：

- **Tool-using / Tool-augmented AI（会调用工具的大模型）**  
    例如 LLM + API + 数据库，侧重“能否操作外部系统”，与是否多模态是两个维度：你可以有“单模态 + 工具”或“多模态 + 工具”。kdnuggets+1
    
- **RAG / Retrieval-augmented generation**  
    专注“是否检索外部知识库”，与多模态是正交维度：有文本 RAG、多模态 RAG（检索文本+图像+视频等）。tutai+1
    
- **Embodied AI / 具身智能**  
    强调“和真实世界实体/机器人绑定，能感知和行动”，通常天然是多模态（视觉、触觉、语音等），但“具身”这个标签在意的是“能不能行动/交互”，而不是模态数目。[moea](https://www.moea.gov.tw/mns/doit/industrytech/IndustryTech.aspx?menu_id=13545&it_id=573)
    

---

## 三、在“模态设计内部”的配套概念

如果你是在读架构论文或做产品设计，跟 Multimodal 紧挨着会出现这些词（不是并列分类，而是配套术语）：

- **Modality / 模态**：  
    某一种数据形式本身，比如 text / image / audio / video / sensor signal。superannotate+1
    
- **Modality gap / 模态差异**：  
    不同模态在分布、信息密度、学习速度上的差异，会导致多模态模型对某一模态“偏见/过度依赖”（unimodal bias）。aclanthology+1
    
- **Fusion / 对齐（alignment）**：  
    多模态模型的关键过程：
    
    - early fusion（早期在特征层拼接）
        
    - late fusion（各自做完再拼结果）
        
    - cross-attention 等更复杂对齐方式。mckinsey+2
        
- **Unimodal representation / Crossmodal representation**：  
    分别指在“每个模态内”的表示、以及“模态间交互之后”的联合表示。[aclanthology](https://aclanthology.org/2021.emnlp-main.720.pdf)
    

这些都不算“同级类别”，但如果你写文档、做产品需求，通常会围绕 multimodal 一起出现。

---

## 四、如果你想在文档/产品里用“同级标签”怎么写

站在“AI overall”产品或文档角度，你可以把多模态当作“一个轴”的取值，然后和其他轴并列：

例子（伪需求文档风格）：

- 输入与感知维度：
    
    - 单模态文本（LLM）
        
    - 多模态（文本 + 图像 + 音频）
        
- 推理与行为维度：
    
    - 仅对话问答
        
    - 工具使用（API/DB/Agent 调用）
        
    - 具身交互（机器人/AR 设备）
        
- 知识来源维度：
    
    - 封闭模型（仅参数内知识）
        
    - 检索增强（RAG / 多模态 RAG）
        

在这个视角下，“Multimodal” 的“同级概念”就是：

- 单模态（unimodal）、跨模态（cross-modal）、任意模态（anymodal）这一条线上邻居；
    
- 与之正交但并列描述系统能力的标签：tool-using、RAG、embodied、generative、perception vs reasoning 等。
    

***

## Evaluation（模型评估 / 模型评测）

> 对应你看到的那句：“用测试集和回归评估持续捕捉模型错误”。

**给 PM/业务的一句话定义**

- Evaluation 是“用一套测试集和指标，持续测量模型在不同场景下的表现，发现和跟踪错误”的过程。 [docs.bigmodel](https://docs.bigmodel.cn/cn/guide/tools/evaluation)

**常见产品用法**

- 在产品说明/方案文档中可以写：  
  - “我们为核心场景设计了专门的测试集，对模型进行持续评估（Evaluation），监控准确率、稳健性和回归问题。” [help.aliyun](https://help.aliyun.com/zh/model-studio/model-evaluation-overview)
  - “每次模型或提示词（Prompt）更新后，都会跑一遍回归评测，确保关键路径不被破坏。”

**中文 AI 圈常见叫法（推荐写法）**

- 推荐：**模型评估（Evaluation）** 或 **模型评测（Evaluation）**，二选一全篇统一。  
- 首次出现可写：“我们通过模型评估（Model Evaluation）体系，用自动测试 + 人工评审，持续发现和修复模型错误。”

***

## AI Security（AI 安全）

> 对应你看到的那句：“看模型错误和攻击输入进入工具执行后的风险”。

**给 PM/业务的一句话定义**

- AI Security 是“围绕 AI 系统的安全防护体系，用来防止提示词攻击、数据泄露、误用工具等风险”。 [yeasy.gitbook](https://yeasy.gitbook.io/ai_security_guide)

**常见产品用法**

- 在业务/安全章节中可以写：  
  - “我们按照 AI 安全（AI Security）最佳实践，对模型输入输出、外部工具调用等环节进行防护，例如 Prompt 注入检测、敏感信息屏蔽和权限控制。” [ai.google](https://ai.google.dev/responsible/docs/evaluation?hl=zh-cn)
  - “对可能触发高风险工具（如下单、转账）的请求，会通过策略检查和人机协同审批来降低误用风险。” [ai.google](https://ai.google.dev/responsible/docs/evaluation?hl=zh-cn)

**中文 AI 圈常见叫法（推荐写法）**

- 推荐：**AI 安全** / **大模型安全** / **人工智能安全**  
- 更工程一点的写法：  
  - “我们构建了覆盖模型、数据、工具链的 AI 安全体系（AI Security），包括输入过滤、输出审核、权限隔离和安全监控等机制。” [feifei](https://feifei.tw/ai-security-101-common-attacks-defense-strategies-guide/)

***

V



## 扩展阅读

- [OpenAI Text Generation Guide](https://platform.openai.com/docs/guides/text-generation) ：了解 LLM 在 API 中如何接收输入、生成文本和返回结构化内容。
- [OpenAI Tokenizer](https://platform.openai.com/tokenizer) ：直观看不同文本、代码、地址和 JSON 会被切成多少 token。
- [OpenAI Embeddings Guide](https://platform.openai.com/docs/guides/embeddings) ：理解 embedding 在搜索、聚类和 RAG 中的基础作用。
- [Attention Is All You Need](https://arxiv.org/abs/1706.03762) ：Transformer 的原始论文，适合建立 attention 和现代大模型架构的技术背景。
- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/) ：从安全角度理解 Prompt Injection、敏感信息泄露、过度代理等常见风险。