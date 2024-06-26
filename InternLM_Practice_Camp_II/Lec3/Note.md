## 笔记三 (茴香豆：搭建你的RAG智能助理)
### RAG
**RAG (Retrieval Augmented Generation)**
>结合检索和生成, 利用外部知识库增强LLM性能.\
通过检索结合与用户输入相关的信息片段以生成更准确、丰富的回答,\
应对LLM在处理知识密集型任务时可能遇到的挑战,\
如生成幻觉、过时知识以及缺乏透明和可追溯的推理过程,\
提供更准确的回答、降低成本、实现外部记忆等\
可应用于: 问答系统、文本生成、信息检索、图片描述等

>典型的RAG由索引 (indexing), 检索 (retrieval)以及生成 (generation)三部分构成.\
索引: 将知识源 (如文档或网页) 分割成chunk, 编码成向量, 并存储在向量数据库中.\
检索: 接收到用户的问题后, 将问题也编码成向量, 并在向量数据库中找到与之最相关的文档块 (top-k chunks).\
生成: 将检索到的文档块与原始问题一起作为提示 (prompt) 输入到LLM中, 生成最终回答.

**向量数据库 (Vector-DB)**
> 将数据通过其它预训练模型转换为固定长度的向量表示, 这些向量能够捕捉文本的语义信息.\
根据用户的查询向量, 使用向量数据库快速找出最相关的向量的过程, 通常通过余弦相似度等相似性度量来完成, 检索结果根据相似度得分进行排序, 选取最相关的文档用以后续的文本生成.\
使用句子嵌入或段落嵌入等文本编码技术, 以及对数据库进行优化以支持大规模向量搜索\
https://github.com/chenzomi12/AISystem

**RAG发展进程**\
最早出自[Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks](https://arxiv.org/abs/2005.11401)一文, 目前已有三种RAG范式.\
Naive RAG, Advanced RAG, Modular RAG\
RAG常见优化方法:
> 嵌入优化 (Embedding Optimization)\
索引优化 (Indexing Optimization)\
查询优化 (Query Optimization)\
上下文管理 (Context Curation)\
> 检索优化:
>  - 迭代检索: 根据初始查询和迄今为止生成的文本进行重复搜索
>  - 递归检索: 迭代细化搜索查询; 链式推理 (Chain-of-Thought) 指导检索过程
>  - 自适应检索: Flare, Self-RAG, 使用LLM主动决定检索的最佳时机和内容
> 
> LLM微调: 检索微调、生成微调、双重微调\

**RAG vs. Fine-tuning**\
**RAG**:
> 非参数记忆, 利用外部知识库提供实时更新的信息; 能够处理知识密集型任务, 提供准确的事实性回答;
通过检索增强, 可以生成更多样化的内容.\
**使用场景**: 适用于需要结合最新信息和实时数据的任务: 开放域回答、实时新闻摘要等.\
**优势**: 动态知识更新、处理长尾知识问题.\
**局限**: 依赖于外部知识库的质量和覆盖范围, 依赖大模型能力.

**Fine-tuning**:
> 参数记忆, 通过在特定任务数据上训练, 模型可以更好地适应该任务; 通常需要大量标注数据进行有效微调; 微调后的模型可能过拟合, 导致泛化能力下降.\
**使用场景**: 适用于数据可用且需要模型高度专业化的任务, 如特定领域的文本分类、情感分析、文本生成等.\
**优势**: 模型性能针对特定任务优化.\
**局限**: 需要大量的标注数据, 且对新任务的适应性较差.

**评价RAG技术**:\
**经典评估指标:**\
准确率 (Accuracy), 召回率 (Recall), F1分数 (F1 Score), BLEU分数 (用于机器翻译和文本生成)以及ROUGE分数 (用于文本生成的评估)\
**RAG评测框架:**\
基准测试: RGB, RECALL, CRUD\
评测工具: RAGAS, ARES, TruLens

### [茴香豆](https://github.com/InternLM/HuixiangDou)
茴香豆是一个由书生·浦语团队开发的基于LLMs的领域知识助手. 专为IM工具中的群聊场景优化的工作流, 提供及时准确的技术支持和自动化问答服务. 通过检索增强生成 (RAG) 技术, 茴香豆能够理解和高效准确的回应与特定知识领域相关的复杂查询.

**应用场景**:\
智能客服: 技术支持、领域知识对话.\
IM工具中创建用户群组, 讨论、解答相关的问题.

**场景难点**:\
群聊中的信息量巨大, 且内容多样, 从技术讨论到闲聊应有尽有.\
用户问题通常与个人紧密相关, 需要准确的实时的专业知识解答.\
传统的NLP解决方案无法准确解析用户意图, 且往往无法提供满意的答案.\
需要一个能够在群聊中准确识别与回答相关问题的智能助手, 同时避免造成消息过载.

**茴香豆的核心特性**\
开源免费: BSD 3-Clause 免费商用\
高效准确: 专为群聊优化\
领域知识: 应用RAG技术, 专业知识快速获取\
部署成本低: 无需额外训练, 可利用云端模型API, 本地算力需求少\
安全: 可完全本地部署, 信息不上传, 保护数据和用户隐私.\
扩展性强: 兼容多种IM软件, 支持多种开源LLMs和云端API

**茴香豆构建**\
本地大模型支持书生·浦语和通义千问\
云端模型支持Kimi, ChatGPT, DeepSeek, ChatGLM

[零编程玩转大模型，学习茴香豆部署群聊助手](https://www.bilibili.com/video/BV1S2421N7mn/)