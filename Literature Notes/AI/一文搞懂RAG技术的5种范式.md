[最全梳理：一文搞懂RAG技术的5种范式！](https://mp.weixin.qq.com/s/PBN3zr4fazGJs7yy9CkuuQ)
# 最全梳理：一文搞懂RAG技术的5种范式！

机器学习算法与自然语言处理 _2025年02月24日 09:01_ _吉林_

The following article is from Datawhale Author 张龙斐

[

![](http://wx.qlogo.cn/mmhead/Q3auHgzwzM54elYMTg5ONEmPUibdDTx3qsDvunJcoTXF1OEte6lOxGw/0)

**Datawhale**.

一个专注于AI领域的开源组织，汇聚了众多优秀学习者，使命-for the learner，和学习者一起成长。

](https://mp.weixin.qq.com/s/PBN3zr4fazGJs7yy9CkuuQ#)

![Image](https://mmbiz.qpic.cn/mmbiz_png/XQIcm2zHCNktgyhicnl86bnSDZIx7NUyseN9rGMKrOITqiacGeLhQBOVaIatj7iaenjWSrfWZlhL5bF4d9WYBAqhw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp)  

**MLNLP**社区是国内外知名的机器学习与自然语言处理社区，受众覆盖国内外NLP硕博生、高校老师以及企业研究人员。  

**社区的愿景**是促进国内外自然语言处理，机器学习学术界、产业界和广大爱好者之间的交流和进步，特别是初学者同学们的进步。

来源 | Datawhale

作者 | 张龙斐，Datawhale鲸英助教

本文主要回顾 RAG 技术的发展，第一部分梳理了综述和关键论文，第二部分梳理了工程实践工具。　

RAG检索增强生成技术自从出现以来经过了多轮范式迭代进展，尤其是随着近几年LLMs的广泛应用，2024年RAG技术呈现爆发态势，全年共计产生了一千篇以上的相关论文。

在经过多轮范式迭代和技术进展后，RAG系统已经从最初的简单形态变得越来越复杂越来越完善，范式从 NaiveRAG 到 AdvancedRAG 到 ModularRAG 再到 GraphRAG，最新的 AgenticRAG 范式融合了数据库，模型微调，逻辑推理，智能体等多种技术，使得其可以适应于各种复杂灵活的任务场景中。

本文梳理了 RAG 领域的关键进展和五大范式，并总结了工程应用中常见的RAG系统构建工具。希望能帮助读者快速了解RAG的基础概念，梳理发展脉络。

# 三篇关键综述

[1] ZHAO P, ZHANG H, YU Q, 等. Retrieval-Augmented Generation for AI-Generated Content: A Survey[A/OL]. arXiv, 2024[2024-06-21]. http://arxiv.org/abs/2402.19473.　

[2] GAO Y, XIONG Y, GAO X, 等. Retrieval-Augmented Generation for Large Language Models: A Survey[A/OL]. arXiv, 2024[2024-03-27]. http://arxiv.org/abs/2312.10997.（best)　

[3] FAN W, DING Y, NING L, 等. A Survey on RAG Meeting LLMs: Towards Retrieval-Augmented Large Language Models[A/OL]. arXiv, 2024[2024-06-17]. http://arxiv.org/abs/2405.06211. 　

这三篇综述把RAG的三个基本范式，朴素RAG、高级RAG、模块化RAG介绍的非常清楚明了。　

# 发展历程

自2021年RAG技术出现之后，RAG首先被用于LLMs的预训练阶段来增强语言模型，随后被用于微调与推理任务中。自ChatGPT发布以来，用于推理阶段的RAG方法如雨后春笋般大量出现，并且迅速演化出了三种范式，分别是NaiveRAG,AdvancedRAG与ModularRAG；2024年微软开源的GraphRAG开启了RAG的第四种范式，融合了知识图谱；在2024年下半年AgenticRAG出现，是前四种范式的集大成者，且具有自适应性。　

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4owvtfXtmxRTPkVjaiaQk8JrvfoyRdVv4BQdxA3Alqc1OZYHkNibrKFr4w/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

图1：按照主要设计重点、提出时间及影响力（以引用量体现）梳理的检索增强生成（RAG）和检索增强大语言模型（RA - LLMs）方法。请注意，图中所示的第一作者、年份以及模型名称可用于查找相应参考文献。[3]　

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4odRnjtTlicj5Y8vPkpJksjVSPz5SVdnYYzFF8vQbYGC15jlI6QY1icKXQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

图2：RAG研究技术树。涉及RAG的阶段主要包括预训练、微调和推理。随着LLMs的出现，对RAG的研究最初侧重于利用LLMs强大的上下文学习能力，主要集中在推理阶段。随后的研究更加深入，逐渐与LLMs的微调相结合。研究人员也一直在探索通过检索增强技术在预训练阶段增强语言模型的方法。[3]　

# RAG基本概念

#### **3.1 为什么需要RAG？**

大型语言模型（LLMs）已经取得了显著的成就，尽管它们仍然面临着很大的局限性，尤其是在特定领域或知识密集型任务中，特别是在处理超出其训练数据或需要当前信息的查询时，会产生 "幻觉"。为了克服这些挑战，检索增强生成（RAG）通过语义相似性计算从外部知识库中检索相关文档块，从而增强了 LLM。通过引用外部知识，RAG 可有效减少生成与事实不符内容的问题。将 RAG 集成到 LLM 中已被广泛采用，RAG 已成为推动聊天机器人发展的一项关键技术，并提高了 LLM 在现实世界应用中的适用性。　

#### **3.2 RAG的起源**

[4]LEWIS P, PEREZ E, PIKTUS A, 等. Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks[A/OL]. arXiv, 2021[2025-01-27]. http://arxiv.org/abs/2005.11401. DOI:10.48550/arXiv.2005.11401.　

摘要：大型预训练语言模型已被证明可以将事实知识存储在其参数中，并在下游 NLP 任务中进行微调后获得最先进的结果。然而，它们访问和精确操作知识的能力仍然有限，因此在知识密集型任务上，它们的性能落后于特定任务架构。此外，为它们的决策提供出处和更新它们的世界知识仍然是有待解决的研究课题。迄今为止，具有显式非参数内存可变访问机制的预训练模型只针对下游提取任务进行过研究。我们为检索增强生成（RAG）探索了一种通用的微调方法--将预先训练的参数记忆和非参数记忆结合起来用于语言生成的模型。我们引入的 RAG 模型中，参数记忆是预先训练的 seq2seq 模型，非参数记忆是维基百科的密集向量索引，通过预先训练的神经检索器访问。我们比较了两种 RAG 方案，一种是在整个生成序列中使用相同的检索段落，另一种是每个标记使用不同的段落。我们在广泛的知识密集型 NLP 任务中对我们的模型进行了微调和评估，并在三个开放领域的质量保证任务中确定了技术水平，其性能优于参数 seq2seq 模型和特定任务的检索和提取架构。在语言生成任务中，我们发现 RAG 模型生成的语言比最先进的纯参数 seq2seq 基线模型生成的语言更具体、更多样、更真实。　

创新： 这篇论文试图解决的问题是如何在知识密集型的自然语言处理（NLP）任务中，有效地结合预训练的语言模型（具有参数化记忆）和非参数化记忆（通过检索机制访问的外部知识库），以提高模型的性能。具体来说，论文提出了一种名为检索增强生成（Retrieval-Augmented Generation, RAG）的模型，旨在通过以下方式解决现有模型的局限性：　

1. 1. 知识访问和操作的精确性：尽管大型预训练语言模型能够存储大量事实知识，但它们在访问和精确操作这些知识方面的能力有限。这导致在知识密集型任务上，这些模型的性能通常不如特定任务架构。
    
2. 2. 决策的可解释性：预训练模型很难提供其决策过程的解释，这在需要透明度的应用场景中是一个挑战。
    
3. 3. 世界知识的更新：预训练模型在更新其知识库方面存在困难，这限制了它们适应新信息的能力。
    

为了解决这些问题，论文提出了RAG模型，它结合了预训练的序列到序列（seq2seq）模型（作为参数化记忆）和通过预训练神经检索器访问的维基百科密集向量索引（作为非参数化记忆）。RAG模型通过端到端训练，能够在多种知识密集型任务上实现最先进的性能，同时生成更具体、多样和事实性的语言。　

#### **3.3 RAG简单流程与总览**

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4oX0GoCyN6yBoFYfTjWHbH4uxlunIuHtA9CK5JAkVWJrpkdDyOBP458g/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

图3：应用于问答的RAG过程的代表性实例。它主要包括3个步骤。1)索引。文档被分割成块，编码成向量，存储在向量数据库中。2)检索。根据语义相似度检索与问题最相关的Top k块。3)生成。将原始问题和检索到的块一起输入LLM，生成最终答案。[2]　

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4oTNU0sjibJ32CSswXeBxKRBQpe0Tks0WvqfBr7wWYcEu2cWHteCQBN2g/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

图4：RAG三种范式的比较。(左)朴素RAG主要由三部分组成:索引、检索和生成。(中)高级RAG围绕检索前和检索后提出了多种优化策略，其过程与朴素RAG相似，仍然遵循链状结构。(右)模块化RAG继承和发展了以前的范式，整体上展示了更大的灵活性。这在引入多个特定功能模块和替换现有模块方面表现得很明显。整个过程并不局限于顺序检索和生成;它包括迭代和自适应检索等方法。[2]　

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4oUtLyRic41rD7ceF3RQILRDDFE83NBthxeia9JRu6bC0x7XcvOWHm1niag/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

图5：RAG技术生态系统总览[2]

# 高级RAG

#### **定义：**

高级 RAG 引入了具体的改进措施，以克服 Naive RAG 的局限性。为了提高检索质量，它采用了检索前和检索后策略。为了解决索引问题，高级 RAG 通过使用滑动窗口方法、细粒度分割和元数据的整合，改进了索引技术。此外，它还采用了多种优化方法来简化检索过程。　

#### **关键论文：**

[5] JIN J, ZHU Y, YANG X, 等. FlashRAG: A Modular Toolkit for Efficient Retrieval-Augmented Generation Research[A/OL]. arXiv, 2024[2024-11-03]. http://arxiv.org/abs/2405.13576. DOI:10.48550/arXiv.2405.13576.　

**摘要：**随着大型语言模型（LLMs）的出现，检索增强生成（RAG）技术的潜力引起了相当多的研究关注。为了增强 RAG 系统的各个方面，人们引入了许多新颖的算法和模型。然而，由于缺乏标准化的实施框架，再加上 RAG 过程本身错综复杂，研究人员在一致的环境中比较和评估这些方法既具有挑战性，又耗费时间。现有的 RAG 工具包（如 LangChain 和 LlamaIndex）虽然可用，但往往笨重臃肿，无法满足研究人员的个性化需求。为了应对这一挑战，我们提出了 FlashRAG，这是一个高效、模块化的开源工具包，旨在帮助研究人员在统一的框架内复制现有的 RAG 方法和开发自己的 RAG 算法。我们的工具包实现了 12 种先进的 RAG 方法，并收集和整理了 32 个基准数据集。我们的工具包具有多种功能，包括可定制的模块化框架、丰富的预实现 RAG 作品集、全面的数据集、高效的辅助预处理脚本以及广泛而标准的评估指标。我们的工具包和资源可在 https://github.com/RUC-NLPIR/FlashRAG 上获取。　

**创新：**　

A：论文提出了FlashRAG，一个模块化的开源工具包，来解决在RAG研究中遇到的问题。以下是FlashRAG解决这些问题的关键特性和方法：　

1. 1. 模块化RAG框架：FlashRAG实现了一个易于扩展的RAG过程，提供了13个组件，涵盖四个主要类别：裁判器（judger）、检索器（retriever）、精炼器（refiner）和生成器（generator）。这些组件可以单独使用或组合成一致的流程。
    
2. 2. 预实现的先进RAG算法：FlashRAG提供了12种先进的RAG算法的实现，如Self-RAG和FLARE，覆盖了顺序RAG、条件RAG、分支RAG和循环RAG类别。这些方法已在统一设置下进行了评估，提供了基准报告。
    
3. 3. 全面的基准数据集：为了提高RAG研究中数据集的一致性和可重用性，作者编译了32个常用的RAG基准数据集，并将其预处理成统一格式。
    
4. 4. 高效的辅助脚本：为了最小化RAG实验的设置时间，FlashRAG提供了一套全面的辅助脚本，包括下载和切片Wikipedia以创建语料库、构建检索索引以及预先准备检索结果。
    
5. 5. 支持多种评估指标：FlashRAG支持多种评估指标来衡量RAG过程的质量，包括检索方面的指标（如recall@k、precision@k、F1@k和MAP）和生成方面的指标（如token级别的F1分数、精确匹配、准确率、BLEU和ROUGE-L）。
    
6. 6. 实验结果和讨论：论文通过一系列实验展示了FlashRAG的能力，包括提供可复现的基准和探索性研究。这些实验使用了不同的数据集和评估指标，展示了FlashRAG在不同设置下的性能。
    
7. 7. 工具包结构：FlashRAG的结构包括环境模块、组件模块和管道模块，这种分层模块化设计使得研究人员可以轻松地组装和执行完整的RAG过程。
    

通过这些特性，FlashRAG旨在帮助研究人员更容易地复制现有的RAG方法，开发新的算法，并专注于优化他们的研究。　

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4osPxflP1XLrAookibCyM2Kic8SBmgR1Z6ZSOUyGVU5NtIN64sV3xbh13Q/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

图6：FlashRAG工具箱总览　

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4odibwX58X1xFyEDBmGLlJP752mK3rP9ZfWQib30qpfQ1iaw8AT3DicJF1CQ/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

图7：高级RAG链 来源：https://github.com/gomate-community/TrustRAG　

[6] SARMAH B, HALL B, RAO R, 等. HybridRAG: Integrating Knowledge Graphs and Vector Retrieval Augmented Generation for Efficient Information Extraction[A/OL]. arXiv, 2024[2024-08-24]. http://arxiv.org/abs/2408.04948. DOI:10.48550/arXiv.2408.04948.　

**摘要：**从金融应用中产生的非结构化文本数据（如财报电话会议记录）中提取和解读复杂信息，即便采用当前运用检索增强生成（RAG，即利用向量数据库进行信息检索的 VectorRAG 技术）的最佳实践，对大语言模型（LLMs）来说仍是巨大挑战，这是由于特定领域术语以及文档格式复杂等难题所致。我们引入一种全新的组合方法 ——HybridRAG，它融合了基于知识图谱（KGs）的 RAG 技术（即 GraphRAG）与 VectorRAG 技术，以增强从金融文档中提取信息的问答（Q&A）系统，该系统能够生成准确且与上下文相关的答案。我们对一组以问答格式呈现的财报电话会议记录文档进行实验，这些文档自然地提供了一系列真实的问答对。实验表明，在检索和生成阶段，从向量数据库和知识图谱中同时检索上下文的 HybridRAG，在检索准确率和答案生成方面，均优于单独使用的传统 VectorRAG 和 GraphRAG。所提出的技术应用范围不仅限于金融领域。　

**创新：**　

相关研究主要集中在信息检索（IR）领域，包括以下几个方面：　

1. 1. BM25算法：Robertson和Zaragoza (2009) 探讨了使用基于相似性搜索的BM25算法，该算法根据词频（Term Frequency, TF）、逆文档频率（Inverse Document Frequency, IDF）和文档长度来计算文档的相关性得分。
    
2. 2. 密集向量模型：Johnson等人 (2019) 研究了使用k近邻（k Nearest Neighbours, KNN）算法的密集向量模型，这些模型能够捕捉数据中的深层语义关系。通过计算向量之间的相似性（如余弦相似性），模型能够返回与查询向量最相似的k个向量对应的数据实体。
    
3. 3. 稀疏编码器模型：Zaharia等人 (2010) 探索了基于稀疏编码器的向量模型，这些模型在处理高维数据时保持了解释性，这是密集向量表示中常面临的挑战。这些模型通过将文档和用户查询映射到从大量训练数据中派生的关联术语的广泛数组中，来编码文档和查询的扩展术语。
    
4. 4. RAG系统的局限性：当前在RAG系统中使用的大多数检索方法依赖于关键词和基于相似性的搜索，这可能限制了RAG系统的整体准确性。论文中提到，尽管之前的努力主要集中在通过调整LLM提示、微调等来提高G部分的准确性，但这些方法对RAG系统的整体准确性影响有限，因为如果R部分提供的上下文不相关，答案也将不准确。
    
5. 5. 检索增强型生成（RAG）模型：Siriwardhana等人 (2023) 研究了如何改进RAG模型在开放领域问答中的领域适应性。
    
6. 6. 混合专家模型（Mixture-of-Experts, MoE）：Du等人 (2022) 提出了GLaM模型，这是一种通过混合专家模型来高效扩展语言模型的方法。
    
7. 7. 路径缩放语言模型（Pathways Language Model, PaLM）：Chowdhery等人 (2023) 提出了PaLM模型，这是一种通过路径缩放来扩展语言模型的方法。
    

这些相关研究为论文提出的“Blended RAG”方法提供了理论和技术基础，特别是在语义搜索和混合查询策略方面。　

### **1.3 模块化RAG**

#### **定义：**

模块化 RAG 架构超越了前两种 RAG 范式，具有更强的适应性和多功能性。它采用了多种策略来改进其组件，例如为相似性搜索添加搜索模块，以及通过微调完善检索器。为应对特定挑战，还引入了重组 RAG 模块和重排 RAG 管道等创新方法。向模块化 RAG 方法的转变正变得越来越普遍，它既支持顺序处理，也支持跨组件的集成端到端训练。尽管模块化 RAG 与众不同，但它建立在高级 RAG 和朴素 RAG 的基本原则之上，表明了 RAG 系列的进步和完善。　

#### **关键论文：**

[7] GAO Y, XIONG Y, WANG M, 等. Modular RAG: Transforming RAG Systems into LEGO-like Reconfigurable Frameworks[A/OL]. arXiv, 2024[2024-08-24]. http://arxiv.org/abs/2407.21059. DOI:10.48550/arXiv.2407.21059.　

**摘要：**检索增强生成（RAG）显著增强了大型语言模型（LLM）处理知识密集型任务的能力。应用场景日益增长的需求推动了 RAG 的发展，导致高级检索器、大型语言模型和其他互补技术的集成，反过来又扩大了 RAG 系统的复杂性。然而，快速的进步正在超越基本的 RAG 范式，许多方法在 "先检索后生成 "的过程中难以统一。在此背景下，本文探讨了现有 RAG 范式的局限性，并介绍了模块化 RAG 框架。通过将复杂的 RAG 系统分解为独立的模块和专门的运算符，它为高度可重构的框架提供了便利。模块化 RAG 超越了传统的线性架构，采用了更先进的设计，集成了路由、调度和融合机制。本文在广泛研究的基础上，进一步确定了流行的 RAG 模式--线性、条件、分支和循环，并全面分析了它们各自在实现上的细微差别。模块化 RAG 为 RAG 系统的概念化和部署提供了创新机会。最后，本文探讨了新运算符和新范例的潜在出现，为 RAG 技术的持续发展和实际部署奠定了坚实的理论基础和实践路线图。 

**创新:** 　

论文通过提出模块化RAG（Modular RAG）框架来解决现有RAG系统的局限性和挑战。具体的解决策略包括：　

1. 1. 模块化架构：将复杂的RAG系统分解为独立的模块和专门的操作符，形成一个高度可重配置的框架。
    
2. 2. 三层架构设计：
    

- L1 Module：关注RAG系统的核心过程，每个阶段被视为一个独立模块。
    
- L2 Sub-module：在每个模块内部进一步细化和优化功能。
    
- L3 Operator：模块或子模块中具体的功能实现。
    

1. 3. RAG Flow：模块和操作符的组合形成RAG流程，可以灵活地表示当前的RAG方法。
    
5. 4. 索引（Indexing）：优化文档分块和元数据附加，以及结构化组织，提高检索效率。
    
6. 5. 预检索（Pre-retrieval）：通过查询扩展、查询转换和查询构造来改善基于原始用户查询的检索效果。
    
7. 6. 检索（Retrieval）：选择合适的检索器，并通过检索器微调来提高检索的质量和效率。
    
8. 7. 后检索（Post-retrieval）：对检索到的文本块进行重排、压缩和选择，以优化上下文信息的利用。
    
9. 8. 生成（Generation）：使用LLM生成答案，并通过生成器微调、验证等方法提高答案的可靠性。
    
10. 9. 协同（Orchestration）：通过路由、调度和融合机制控制RAG流程，使系统能够适应不同的查询和场景。
    
11. 10. 灵活性和扩展性：模块化RAG提供了在不同应用场景中适应和扩展新方法的灵活性。
    
12. 11. 理论和实践指导：论文不仅提出了理论框架，还探讨了模块化RAG在实际部署中的潜力，为未来的研究方向和实践探索提供了指导。
    

通过这些策略，模块化RAG框架旨在提高RAG系统的灵活性、可扩展性和可维护性，同时满足不断增长和多样化的应用需求和期望。　

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4oZ0ntyK4E7KYZymtVbtRCkSicSG92kJwLPGJPBbtGDfY3nib1BASv2SMg/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

图8：三种 RAG 范式之间的比较。　

# GraphRAG

#### **定义：**

检索增强生成（RAG）是一种强大的技术，它通过从外部来源检索知识、技能和工具等附加信息来增强下游任务的执行。图本身具有 "节点由边连接 "的特性，可以编码大量的异构和关系信息，这使其成为 RAG 在大量实际应用中的黄金资源。　

#### 综述：

[8] PENG B, ZHU Y, LIU Y, 等. Graph Retrieval-Augmented Generation: A Survey[A/OL]. arXiv, 2024[2024-08-21]. http://arxiv.org/abs/2408.08921.　

**摘要：**最近，检索增强生成技术（RAG）在应对大型语言模型（LLM）的挑战方面取得了显著的成功，而无需重新训练。通过参考外部知识库，RAG 完善了 LLM 的输出，有效缓解了 "幻觉"、特定领域知识缺乏和信息过时等问题。然而，数据库中不同实体之间复杂的关系结构给 RAG 系统带来了挑战。为此，GraphRAG 利用实体间的结构信息，实现更精确、更全面的检索，捕捉关系知识，促进更准确、更能感知上下文的响应。鉴于 GraphRAG 的新颖性和潜力，对当前技术进行系统回顾势在必行。本文首次全面概述了 GraphRAG 方法。我们将 GraphRAG 工作流程正规化，包括基于图形的索引、图形引导的检索和图形增强的生成。此外，我们还研究了 GraphRAG 的下游任务、应用领域、评估方法和工业用例。最后，我们探讨了未来的研究方向，以激发进一步的探索并推动该领域的进步。　

**贡献：**　

这篇论文提供了对Graph Retrieval-Augmented Generation (GraphRAG) 方法论的全面概述。以下是论文的主要内容总结：　

1. 1. 背景介绍：论文首先介绍了大型语言模型（LLMs）的发展以及它们在自然语言处理（NLP）中的重要性。同时指出了LLMs在缺乏特定领域知识、实时更新信息和专有知识时可能遇到的问题。
    
2. 2. GraphRAG概念：提出了GraphRAG作为一种解决上述问题的框架，通过结合图数据库中的结构化信息来增强LLMs的输出。
    
3. 3. 工作流程：详细介绍了GraphRAG的三个主要阶段：图基础索引（G-Indexing）、图引导检索（G-Retrieval）和图增强生成（G-Generation）。
    
4. 4. 核心技术：探讨了GraphRAG系统中使用的核心技术，包括图神经网络（GNNs）和语言模型（LMs）。
    
5. 5. 训练方法：讨论了检索器和生成器的独立训练方法，以及它们的联合训练策略。
    
6. 6. 下游任务和应用领域：分析了GraphRAG在多种下游任务中的应用，如问答、信息提取等，并探讨了其在不同应用领域（医疗、金融、教育等）的潜在影响。
    
7. 7. 评估方法和工业用例：提供了评估GraphRAG系统性能的方法，包括基准测试和工业应用案例。
    
8. 8. 未来研究方向：论文最后提出了GraphRAG领域的未来研究方向，包括动态和自适应图、多模态信息整合、可扩展和高效的检索机制等。
    
9. 9. 贡献总结：论文总结了对现有GraphRAG方法论的系统化回顾，提供了对GraphRAG技术、应用和未来研究方向的全面理解。
    

整体而言，这篇论文为理解和应用GraphRAG提供了一个全面的视角，并为未来的研究和应用指明了方向。　

#### **关键论文：**

[9] EDGE D, TRINH H, CHENG N, 等. From Local to Global: A Graph RAG Approach to Query-Focused Summarization[A/OL]. arXiv, 2024[2024-08-03]. http://arxiv.org/abs/2404.16130. DOI:10.48550/arXiv.2404.16130.　

**摘要：**通过检索增强生成（RAG），大型语言模型（LLM）能够从外部知识源检索信息，从而回答涉及私有或未见文档的问题。然而，RAG 在处理全局问题（如“数据集的主要主题是什么？”）时表现不佳，因为这类问题本质上是查询聚焦的摘要任务，而非直接检索。现有的 QFS 方法也难以处理大规模文本。为此，我们提出图 RAG 方法，该方法结合了两种方法的优势，能够随着问题普遍性和文本量的增加而扩展。图 RAG 通过 LLM 构建图索引，先从文档中提取实体图，再预生成相关实体的摘要。在回答问题时，每个摘要生成部分答案，最终汇总为完整回答。实验表明，图 RAG 在处理大规模数据集的全局问题时，能显著提升答案的全面性和多样性。全球和本地图 RAG 的开源 Python 实现即将发布。　

**创新：**　

这篇论文提出了一种名为 Graph RAG（Graph Retrieval-Augmented Generation）的方法，旨在解决以下问题：　

1. 1. 检索增强生成（RAG）的局限性：传统的 RAG 方法在处理针对整个文本语料库的全局性问题时存在不足，例如“数据集中的主要主题是什么？”这类问题。这是因为这类问题本质上是查询聚焦的摘要（Query-Focused Summarization, QFS）任务，而不是传统的显式检索任务。
    
2. 2. 大规模文本的摘要生成：现有的 QFS 方法难以扩展到 RAG 系统所索引的大规模文本。由于大型语言模型（LLMs）的上下文窗口限制，直接检索文本块可能无法满足全局摘要的需求。
    
3. 3. 信息丢失问题：在处理大量文本时，信息可能会在较长上下文中丢失，这要求在设计摘要方法时考虑到信息的完整性和连贯性。
    
4. 4. 全局性问题的回答：为了支持人类对整个文本语料库的全局性理解，需要一种能够通过提问来应用和细化用户对数据的心理模型的方法。
    

Graph RAG 方法通过以下步骤来解决这些问题：　

- 使用 LLM 构建基于图的文本索引，包括从源文档派生出的实体知识图谱。
    
- 为所有紧密相关的实体组预生成社区摘要。
    
- 给定一个问题时，使用每个社区摘要生成部分响应，然后将所有部分响应再次汇总以生成最终的响应。
    

该方法的目标是在用户问题的一般性和要索引的源文本数量方面实现扩展，同时提高生成答案的全面性和多样性。论文还提供了一个开源的 Python 实现，用于全局和本地 Graph RAG 方法。　

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4oGHTx1cxIerCRnprzezOibbNk4hs3n2VXblLNnRUZ6YHh85QlDlQD7wA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

图9：GraphRAG流程。如上图所示，GraphRAG包括两个处理阶段，分别是：索引阶段和查询阶段。索引阶段利用LLM来自动化构建知识图谱，提取出对应的节点（如实体）、边（如关系）和协变量（如主张，claim），然后利用社区发现技术（如Leiden算法）对整个知识图谱进行子图划分，然后自底而上对子图利用LLM进行摘要、总结。针对特定查询，“全局答案（Global Search）”汇总所有与之相关的社区摘要最后汇总生成答案。与传统RAG一样，GraphRAG也需要将源文档转化为文本片段（TextUnits），这个片段既会被用于图谱抽取，也会作为知识的引用源，以便追溯回最初的原始文本内容。　

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4o3J8PmS8ce3s23KurfROHjpS6PGZQiabznQFm0ogCiauVzYMRYkA5myoA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

图10：GraphRAG数据流　

[9] GUO Z, XIA L, YU Y, 等. LightRAG: Simple and Fast Retrieval-Augmented Generation[A/OL]. arXiv, 2024[2025-01-27]. http://arxiv.org/abs/2410.05779. DOI:10.48550/arXiv.2410.05779.　

**摘要：**检索增强生成（RAG）系统通过整合外部知识源来增强大型语言模型（LLM），从而根据用户需求提供更准确、更贴近语境的回答。然而，现有的 RAG 系统有很大的局限性，包括依赖于平面数据表示和对上下文的认识不足，这可能导致无法捕捉复杂的相互依存关系的零散答案。为了应对这些挑战，我们提出了 LightRAG，将图结构纳入文本索引和检索过程。这一创新框架采用了双层检索系统，从低层次和高层次知识发现两方面加强了综合信息检索。此外，图结构与矢量表示法的整合有助于高效检索相关实体及其关系，从而在保持上下文相关性的同时显著缩短响应时间。增量更新算法进一步增强了这一能力，确保了新数据的及时整合，使系统能够在快速变化的数据环境中保持有效性和响应速度。广泛的实验验证表明，与现有方法相比，LightRAG 在检索准确性和效率方面都有显著提高。我们已将 LightRAG 开源，可通过以下链接获取：https://github.com/HKUDS/LightRAG。　

**创新：**　

论文提出了一个名为LightRAG的检索增强型生成（RAG）系统，旨在通过整合图结构改善大型语言模型（LLMs）的信息检索和生成能力。以下是论文的主要内容总结：　

1. 1. 问题陈述：
    

- 现有RAG系统在处理需要复杂实体关系理解的查询时存在限制，如依赖于平面数据表示和缺乏上下文感知能力。
    

1. 2. LightRAG框架：
    

- 提出了一个图结构化文本索引和双级检索系统的框架，以增强从文档中检索全面信息的能力。
    
- 引入了增量更新算法，使系统能够快速适应新数据，保持在动态数据环境中的有效性。
    

1. 3. 方法论：
    

- 使用LLMs提取实体和关系，构建知识图谱，并通过图结构优化信息检索过程。
    
- 实现了双级检索策略，分别关注于低层次的具体信息和高层次的广泛话题检索。
    
- 结合图结构和向量表示，提高检索效率和结果的全面性。
    

1. 4. 实验评估：
    

- 通过大量实验，验证了LightRAG在检索准确性、模型消融、响应效率和新信息适应性方面相较现有方法的显著改进。
    
- 使用了四个不同领域的数据集进行评估，并与多个基线方法进行了比较。
    

1. 5. 主要贡献：
    

- 提出了一个图增强的RAG系统，通过图结构化索引有效地表示实体间的复杂相互依赖关系。
    
- 开发了LightRAG模型，该模型结合了双级检索和图增强文本索引，以实现全面且成本效益的检索。
    
- 进行了广泛的实验，证明了LightRAG相比基线方法在多个评估维度上的有效性。
    

1. 6. 开源实现：
    

- 作者提供了LightRAG的开源实现，可通过GitHub访问。
    

总体而言，论文的创新之处在于将图结构应用于文本索引和检索过程，提出了一个能够处理复杂查询并快速适应新数据的高效RAG系统。通过这种方法，LightRAG能够生成更准确、更具上下文相关性的回答，极大地提高了RAG系统在实际应用中的有效性和实用性。　

[6] LIANG L, SUN M, GUI Z, 等. KAG: Boosting LLMs in Professional Domains via Knowledge Augmented Generation[A/OL]. arXiv, 2024[2024-11-12]. https://arxiv.org/abs/2409.13731. DOI:10.48550/ARXIV.2409.13731.　

**摘要：**最近发展起来的检索增强生成（RAG）技术能够高效地构建特定领域的应用程序。然而，它也有局限性，包括向量相似性与知识推理相关性之间的差距，以及对数值、时间关系、专家规则等知识逻辑的不敏感性，这些都阻碍了专业领域知识服务的有效性。在这项工作中，我们引入了一个专业领域知识服务框架，称为知识增强生成（KAG）。KAG的设计初衷是为了应对上述挑战，充分发挥知识图谱（KG）和向量检索的优势，通过五个关键方面双向增强大型语言模型（LLM）和知识图谱（KG），从而提高生成和推理性能：（1）LLM友好的知识表示；（2）知识图谱和原始块之间的相互索引；（3）逻辑形式引导的混合推理引擎；（4）知识与语义推理的对齐；（5）KAG的模型能力增强。我们将 KAG 与多跳问题解答中现有的 RAG 方法进行了比较，发现它的性能明显优于最先进的方法，在 F1 分数方面，KAG 在 hotpotQA 上取得了 19.6% 的相对改进，在 2wiki 上取得了 33.5% 的相对改进。我们已将 KAG 成功应用于蚂蚁金服集团的两个专业知识问答任务，包括电子政务问答和电子健康问答，与 RAG 方法相比，在专业性方面取得了显著提高。此外，我们即将在开源KG引擎OpenSPG上原生支持KAG，让开发者可以更轻松地构建严谨的知识决策或便捷的信息检索服务。这将促进 KAG 的本地化开发，使开发人员能够以更高的准确性和效率构建领域知识服务。　

**创新：**　

这篇论文提出了一个名为知识增强生成（KAG）的专业领域知识服务框架，旨在解决以下问题：　

1. 1. 检索过程中的模糊性：传统的检索增强生成（RAG）技术在检索过程中存在模糊性，这影响了知识服务的专业性和准确性。
    
2. 2. 通用语言模型的“幻觉”问题：通用语言模型在理解和推理方面存在局限性，这可能导致生成的答案不准确或不完整。
    
3. 3. 复杂系统中的级联损失：在复杂的知识服务系统中，不同组件之间的错误传递可能导致整体性能下降。
    
4. 4. 专业知识的准确性、信息的完整性和逻辑的严格性：在科学计算、医学和法律等专业领域中，对知识的准确性、信息的完整性以及规则、时间和价值的逻辑严格性有特别高的要求。
    
5. 5. 知识图谱（KG）的整合不足：尽管一些现有工作尝试将知识图谱整合到RAG框架中，但它们并没有充分利用知识图谱在专业领域知识管理方面的能力。
    

为了解决这些问题，KAG框架通过双向增强大型语言模型（LLM）和知识图谱（KG），提出了**五个关键改进**：　

1. 1. LLM友好的知识语义表示：提出了一种适合LLM的知识表示框架，以支持与LLM的兼容。
    
2. 2. 知识图谱和原始文本块之间的相互索引：通过建立图结构和原始文本块之间的索引，提高了检索的准确性。
    
3. 3. 基于逻辑形式的混合推理和求解：提出了一种结合了语言和符号的问题解决过程。
    
4. 4. 基于语义推理的知识对齐：通过定义领域知识的各种语义关系，提高了知识表示和检索的准确性。
    
5. 5. KAG模型：针对KAG框架所需的能力，如索引构建、检索、问题理解、语义推理和摘要生成，增强了通用LLM的特定能力。
    

通过这些改进，KAG框架在多跳问答任务上的表现显著优于现有的RAG方法，并在蚂蚁集团的电子政务和电子健康问答任务中实现了专业水平的显著提升。　

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4oWN0jy4EiaEPyJOfa8kVMa1wa5OOb4KkUgJBFyaS5iasZMT94VzoV8Acw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

图11：KAG 框架。左侧显示的是 KAG-Builder，右侧显示的是 KAG-Solver。图片底部的灰色区域代表 KAG-模型。　

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4oS8TMG5pds0gJ7ep1vkqol5BCZyc7EjicjPVYxMgXTurnsPeyQO8ohYw/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

图12：一个专为大型语言模型（LLM）设计的友好型知识表示框架。LLMFriSPG将实例与概念区分开来，通过概念实现与 LLMs 的对接。SPG 的属性被划分为知识区和信息区，也就是静态区和动态区，分别与具有严格模式约束的决策专业知识以及具有开放信息表示的文档检索索引知识相兼容。图中的红色虚线描绘了从信息提炼为知识的融合与挖掘过程。增强的文档块表示方法为 LLMs 提供了可追溯且易于解读的文本上下文。　

# AgenticRAG

**概念辨析：agent与agentic**　

在AI领域中，AI Agent（智能体）与Agentic AI（能动AI）虽密切相关却各有侧重。AI Agent是具体的智能实体，能在特定环境中感知、决策并执行动作以完成任务，通常基于机器学习和人工智能技术，具备一定的自主性和自适应性，主要关注单一功能或任务，如AI客服系统。而Agentic AI是一个更广泛的术语，强调AI系统在更高层面上的自主决策和问题解决能力，不仅能够感知和执行任务，还能主动思考、规划和适应环境的变化，涵盖设计和改进AI Agent的方法和框架，探索其更广泛和通用的潜力，目标是实现更广泛、更复杂的任务，能够在动态环境中自主地进行学习和优化，应用范围更广，可在不同领域和场景发挥作用，智能程度更高，不仅能处理数据、决策，还能从互动中学习并优化自身行为，使用更复杂的算法，如强化学习、元学习、大模型结合自监督学习，适用于复杂系统，如自动驾驶系统、智能金融分析、火星探测机器人等，由于其高度自主性和广泛的应用范围，伦理和风险问题更为复杂，需要更多的关注和研究。　

#### **定义：**

Agentic RAG 将 ReACT 的推理能力与 Agent 的任务执行能力相结合，创建一个动态和自适应的系统。与遵循固定管道的传统 RAG 不同，Agentic RAG 通过使用 ReACT 根据用户查询的上下文动态协调 Agent，引入了灵活性。这使得系统不仅能够检索和生成信息，还能够根据上下文、不断变化的目标和与之互动的数据采取明智的行动。这些进步使 Agentic RAG 成为一个更强大和灵活的框架。模型不再仅限于被动响应用户查询；相反，它可以主动规划、执行并调整其方法以独立解决问题。这使得系统能够处理更复杂的任务，动态适应新挑战，并提供更具上下文相关性的响应。　

#### **综述：**

[10] SINGH A, EHTESHAM A, KUMAR S, 等. Agentic Retrieval-Augmented Generation: A Survey on Agentic RAG[A/OL]. arXiv, 2025[2025-01-26]. http://arxiv.org/abs/2501.09136. DOI:10.48550/arXiv.2501.09136.　

**摘要：**大型语言模型（LLM）通过实现类似人类的文本生成和自然语言理解，给人工智能（AI）带来了革命性的变化。然而，对静态训练数据的依赖限制了它们响应动态实时查询的能力，导致输出结果过时或不准确。检索增强生成（RAG）作为一种解决方案应运而生，它通过整合实时数据检索来增强 LLM，从而提供与上下文相关的最新响应。尽管前景看好，但传统的 RAG 系统受到静态工作流程的限制，缺乏多步骤推理和复杂任务管理所需的适应性。Agentic Retrieval-Augmented Generation（Agentic RAG）通过将自主人工智能代理嵌入 RAG 管道，超越了这些限制。这些代理利用代理设计模式--反射、规划、工具使用和多代理协作--动态管理检索策略，迭代完善上下文理解，并调整工作流程以满足复杂的任务要求。这种集成使 Agentic RAG 系统能够在各种应用中提供无与伦比的灵活性、可扩展性和上下文感知能力。本调查报告从代理式 RAG 的基本原理和 RAG 范例的演变开始，对代理式 RAG 进行了全面探讨。它对代理 RAG 架构进行了详细分类，重点介绍了在医疗保健、金融和教育等行业中的关键应用，并探讨了实用的实施策略。此外，该书还探讨了在扩展这些系统、确保道德决策和优化实际应用性能方面的挑战，同时详细介绍了实施 Agentic RAG 的框架和工具。　

**贡献：**　

这篇论文提供了对Agentic Retrieval-Augmented Generation（Agentic RAG）的全面探索，主要内容可以总结如下：　

1. 1. 问题阐述：
    

- 大型语言模型（LLMs）在依赖静态训练数据时存在局限性，特别是在动态、实时查询响应方面的挑战。
    

1. 2. Agentic RAG介绍：
    

- 介绍了Agentic RAG的概念，它通过将自主AI代理集成到RAG流程中来克服LLMs的局限性，利用代理设计模式实现动态管理检索策略、迭代细化上下文理解，并适应性地调整工作流程。
    

1. 3. RAG的演变：
    

- 论文概述了从Naïve RAG到Advanced RAG、Modular RAG、Graph RAG，最终到Agentic RAG的演变过程，并讨论了每种范式的关键特征、优势和局限。
    

1. 4. Agentic RAG架构分类：
    

- 提供了一个详细的Agentic RAG架构分类，包括单代理、多代理和基于图的框架，并探讨了每种架构的特点和适用场景。
    

1. 5. Agentic RAG的应用案例：
    

- 论文探讨了Agentic RAG在医疗保健、金融、教育等多个行业中的关键应用，并提供了具体的用例分析。
    

1. 6. 工具和框架：
    

- 讨论了支持Agentic RAG系统开发的工具和框架，如LangChain、LlamaIndex、Hugging Face Transformers和Qdrant等。
    

1. 7. 基准测试和数据集：
    

- 论文讨论了评估RAG系统性能的基准测试和数据集，强调了标准化评估的重要性。
    

1. 8. 挑战和未来方向：
    

- 论文总结了Agentic RAG系统面临的挑战，包括多代理架构的协调复杂性、可扩展性和延迟问题，以及伦理考虑，并提出了未来研究的方向。
    

1. 9. 结论：
    

- 强调Agentic RAG在动态和复杂环境中的潜力，呼吁进一步的研究和创新以解决现有挑战，并探索Agentic RAG的未来方向。
    

整体而言，这篇论文为理解和应用Agentic RAG提供了一个全面的框架，并强调了其在解决传统LLMs局限性和推动AI技术发展中的重要性。　

#### **关键论文：**

[11] ASAI A, WU Z, WANG Y, 等. Self-RAG: Learning to Retrieve, Generate, and Critique through Self-Reflection[A/OL]. arXiv, 2023[2025-01-27]. http://arxiv.org/abs/2310.11511. DOI:10.48550/arXiv.2310.11511.　

**摘要：**尽管大型语言模型（LLM）具有卓越的能力，但由于它们仅依赖于它们封装的参数知识，因此它们经常产生包含事实不准确性的响应。检索增强生成（RAG），一个特设的方法，增强与检索相关的知识LM，减少这样的问题。然而，不加区别地检索和纳入固定数量的检索通道，无论检索是否必要，或者通道是否相关，都会减少LM的多功能性，或者可能导致无用的响应生成。我们引入了一个新的框架，称为自反射检索增强生成（SELF-RAG），提高LM的质量和真实性，通过检索和自我反思。我们的框架训练了一个任意的LM，它可以根据需要自适应地检索段落，并使用特殊的令牌（称为反射令牌）生成和反射检索到的段落及其自己的世代。生成反射令牌使LM在推理阶段可控，使其能够根据不同的任务需求调整其行为。实验表明，SELFRAG（7 B和13 B参数）显着优于国家的最先进的LLM和检索增强模型在一组不同的任务。具体来说，SELF-RAG在开放域QA、推理和事实验证任务上优于ChatGPT和检索增强的Llama 2-chat，并且相对于这些模型，它在提高长格式生成的真实性和引用准确性方面表现出显着的收益。　

**创新：**　

这篇论文提到了多个与SELF-RAG相关的研究领域和具体工作，主要包括以下几个方面：　

1. 1. 检索增强生成（Retrieval-Augmented Generation, RAG）：RAG方法通过在LLMs的输入中加入检索到的相关文本段落来减少知识密集型任务中的事实错误。SELF-RAG在RAG的基础上进行了改进，通过自我反思机制来更智能地决定何时进行检索以及如何利用检索到的信息。
    
2. 2. 并行RAG工作（Concurrent RAG work）：一些并行工作提出了新的训练或提示策略来改进RAG方法。例如，Lin等人（2023）通过两步微调策略来改进RAG，而Yoran等人（2023）和Xu等人（2023）则使用自然语言推理模型和摘要模型来过滤或压缩检索到的段落。
    
3. 3. 训练和生成与批评者（Training and generating with critics）：一些研究使用强化学习（如PPO）从人类反馈中训练LLMs，以使模型与人类偏好对齐。SELF-RAG则通过在训练阶段使用批评者模型来生成反思标记，从而在推理阶段实现可控生成。
    
4. 4. LLM精炼（LLM refinement）：一些工作通过迭代提示模型生成任务输出、自然语言反馈和精炼任务输出来提高模型性能，但这种方法可能会牺牲推理效率。
    
5. 5. 检索增强的LLMs：论文还比较了SELF-RAG与使用检索增强的LLMs（如ChatGPT和Llama2-chat）的性能，展示了SELF-RAG在多个任务上的优势。
    
6. 6. 自我评估引导的解码框架（Self-evaluation-guided decoding framework）：Xie等人（2023）提出了一个自我评估引导的解码框架，但主要集中在推理任务上，而SELF-RAG则在更广泛的任务上应用了自我反思机制。
    

这些相关工作为SELF-RAG提供了理论基础和实践背景，SELF-RAG在此基础上通过引入自我反思和按需检索的概念，提出了一种新的提高LLMs生成质量的方法。　

#### **AgenticRAG工作流程**

Agentic RAG 的关键创新在于其能够自主使用工具、做出决策并规划下一步，并且具有推理的能力。管道遵循以下核心阶段：　

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4odDQ5Mvy7zE6O3iaKSWpoSoVQia0xe0YMMcvldWN8eGLOepRyr0577GEA/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

图13：AgenticRAG工作流程，用户查询提交，之后一个 Agent 在 向量数据库 中搜索，文档以嵌入的形式存储，确保高效快速地检索相关信息，如果检索到的数据不足，Agentic会细化查询并进行额外的检索尝试，以提取更好的结果。使用功能工具进行外部数据获取：如果 向量数据库 缺乏必要的信息，Agent 使用 功能工具 从外部来源（如 API、网络搜索引擎或专有数据流）收集实时数据。这确保系统提供最新和上下文相关的信息。大型语言模型 (LLM) 响应生成：检索到的数据传递给 LLM，它综合这些数据生成针对查询的详细、上下文感知的响应。Agent 驱动的改进：在 LLM 生成响应后，Agentic进一步细化以确保准确性、相关性和连贯性，然后将其交付给用户。　

#### **各RAG范式比较**

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4ofUVxPteH539NziaJ8JgjtOItmDeSvrgVtQBicDYgKYVJ0Y5JsoUzu9uw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4ojGjrhvpAIH81sjHQ4puSCANDLANvf7K8ZmhiaslGXmOia4q2QVe47uhw/640?wx_fmt=png&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

# 相关的重要论文

#### **多模态RAG**

[12] YU S, TANG C, XU B, 等. VisRAG: Vision-based Retrieval-augmented Generation on Multi-modality Documents[A/OL]. arXiv, 2024[2024-10-30]. http://arxiv.org/abs/2410.10594.　

这篇论文介绍了一个名为VisRAG（Vision-based Retrieval-augmented Generation）的系统，旨在解决现有检索增强生成（RAG）系统在处理多模态文档时面临的问题。具体来说，VisRAG试图解决以下几个关键问题：　

1. 1. 利用视觉信息：传统的RAG系统仅基于文本，无法利用布局和图像等视觉信息，而这些信息在现实世界中的多模态文档中起着至关重要的作用。
    
2. 2. 消除信息丢失：在从多模态文档中获取文本信息的过程中，通常需要一个解析阶段，包括版面识别、光学字符识别（OCR）和文本合并等步骤。这个解析过程不可避免地引入了错误和信息丢失，从而可能对检索和生成阶段产生负面影响。
    
3. 3. 直接处理文档图像：VisRAG通过直接将文档作为图像嵌入到视觉-语言模型（VLM）中，而不是首先解析文档以获取文本，从而绕过了解析阶段，保留了文档中的所有信息。
    
4. 4. 提高保留和利用原始文档数据信息的能力：与基于文本的传统RAG相比，VisRAG最大化了原始文档中数据信息的保留和利用，消除了解析过程中引入的信息丢失。
    
5. 5. 多模态文档的RAG处理：在现实世界的应用中，知识通常以多模态文档的形式呈现，如教科书和手册，这些文档可能包含交错的文本和图形。VisRAG旨在通过直接处理这些文档的图像，而不是依赖于提取的文本内容，来改进RAG在多模态文档上的应用。
    

总的来说，VisRAG试图通过建立一个基于VLM的RAG流程，来解决传统RAG系统在处理包含文本和图像的多模态文档时的信息丢失和利用不足的问题。　

[13] FAYSSE M, SIBILLE H, WU T, 等. ColPali: Efficient Document Retrieval with Vision Language Models[A/OL]. arXiv, 2024[2024-10-27]. http://arxiv.org/abs/2407.01449. DOI:10.48550/arXiv.2407.01449.　

这篇论文主要解决的问题是如何提高文档检索系统在处理视觉丰富文档时的效率和性能。具体来说，论文指出现代文档检索系统虽然在文本匹配方面表现出色，但在有效利用视觉线索（如表格、图形、页面布局或字体等）方面存在不足，这限制了它们在实际文档检索应用中的性能，例如增强型检索（Retrieval Augmented Generation, RAG）。　

为了解决这个问题，论文提出了两个主要贡献：　

1. 1. ViDoRe（Visual Document Retrieval Benchmark）：这是一个新的基准测试，用于评估文档检索系统在页面级别检索视觉丰富文档的能力。它涵盖了多个领域、语言和设置。
    
2. 2. ColPali：这是一个新的检索模型架构，它利用最新的视觉-语言模型（Vision Language Models, VLMs）来从文档页面的图像中生成高质量的上下文嵌入，并通过后期交互匹配机制（late interaction matching mechanism）实现快速的查询匹配。ColPali在性能上大幅超越了现有的文档检索管道，同时具有更快的处理速度和端到端可训练性。
    

#### **逻辑推理RAG**

[14] FENG W, HAO C, ZHANG Y, 等. AirRAG: Activating Intrinsic Reasoning for Retrieval Augmented Generation via Tree-based Search[A/OL]. arXiv, 2025[2025-01-27]. http://arxiv.org/abs/2501.10053. DOI:10.48550/arXiv.2501.10053.　

这篇论文提出了一个名为AirRAG（Activating Intrinsic Reasoning for Retrieval Augmented Generation via Tree-based Search）的新方法，旨在解决以下问题：　

1. 1. 复杂任务中的推理能力：传统的检索增强生成（RAG）模型在处理复杂任务时，往往难以有效地检索到足够的知识，并且难以理解问题的复杂推理逻辑。
    
2. 2. 单一解空间的限制：现有的迭代或递归RAG方法在面对复杂问题时，常常陷入单一解空间，无法充分激活大型语言模型（LLMs）的决策能力。
    
3. 3. 推理过程中的解决方案空间探索：现有的方法在推理过程中难以有效探索解决方案空间，导致生成的推理步骤质量低下，无法有效指导自我探索。
    

为了解决这些问题，AirRAG通过以下方式进行改进：　

- 设计了五种基本推理动作（系统分析、直接回答、检索回答、查询转换和摘要回答），并通过蒙特卡洛树搜索（MCTS）扩展到广泛的树基推理空间。
    
- 引入自一致性验证来探索潜在的推理路径，并实现推理扩展。
    
- 使用计算最优策略将更多的推理计算应用于关键动作，以实现性能提升。
    

总的来说，AirRAG旨在通过结合系统分析和有效的推理动作，显著激活LLMs的内在推理能力，并扩展特定任务的解决方案空间。　

#### **个性化记忆扩展**

https://github.com/mem0ai/mem0?tab=readme-ov-file　

![Image](https://mmbiz.qpic.cn/sz_mmbiz_png/vI9nYe94fsFia5jpJ03Cjoia7k6VoWNU4oXoR51oAdXlbT4Yg1OIA8TRy2axNEdSgjHsO2Aia9CuRlE2IcIvU531g/640?wx_fmt=png&from=appmsg&tp=webp&wxfrom=5&wx_lazy=1&wx_co=1)

Mem0是一个为AI助手和代理提供智能记忆层的开源项目，旨在通过智能记忆层增强AI助手和代理的能力，实现个性化的AI交互。Mem0的核心功能包括：　

1. 1. 多层次记忆：支持用户级、会话级和AI代理级的记忆保留，确保不同层次的交互信息都能被有效处理。
    
2. 2. 自适应个性化：根据用户交互不断改进，提供精准的个性化记忆，通过分析用户的使用模式，自动调整其行为以更好地满足用户需求。
    
3. 3. 开发者友好API：提供简单易用的API接口，方便开发者集成到现有的应用程序中。
    
4. 4. 跨平台一致性：确保在不同平台和设备上保持统一的行为和数据一致。
    
5. 5. 托管服务：提供无忧的托管解决方案，便于部署和维护。
    

Mem0的工作流程主要包括以下几个步骤：　

1. 1. 记忆提取：处理新数据，如用户的聊天历史或最近的交互，提取相关的事实和偏好，并将其存储在数据存储中。
    
2. 2. 记忆搜索：将提取的记忆转换为嵌入向量，并在向量数据库中搜索类似的现有记忆。
    
3. 3. 记忆更新：根据新记忆和现有记忆的相似度，决定如何将新信息与现有知识库整合，包括添加新记忆、修改现有记忆、合并相关记忆或删除过时信息。
    
4. 4. 基于记忆的响应：当用户提出问题或请求信息时，Mem0首先在其向量数据库中搜索相关记忆，并使用这些记忆生成个性化的响应。
    

#### **RAG系统性能优化**

[15] FAN T, WANG J, REN X, 等. MiniRAG: Towards Extremely Simple Retrieval-Augmented Generation[A/OL]. arXiv, 2025[2025-01-26]. http://arxiv.org/abs/2501.06713. DOI:10.48550/arXiv.2501.06713.　

这篇论文试图解决的主要问题是在资源受限的环境中部署高效的检索增强型生成（Retrieval-Augmented Generation, RAG）系统时面临的挑战。具体来说，论文指出了以下几个关键问题：　

1. 1. 小语言模型（Small Language Models, SLMs）在现有RAG框架中的性能退化问题：当在资源受限场景（如边缘设备、隐私敏感应用和实时处理系统）中部署小语言模型时，现有的RAG系统由于SLMs的语义理解和文本处理能力有限，导致性能严重下降。
    
2. 2. 对大型语言模型（Large Language Models, LLMs）的过度依赖：目前的RAG系统在构建索引、知识检索和最终回答生成的整个流程中，主要依赖于LLMs，这导致了巨大的计算开销和资源需求，限制了它们在资源受限场景中的部署。
    
3. 3. 现有RAG系统与SLMs的架构不匹配：原本为利用LLMs高级能力而设计的RAG架构，在多个关键功能上无法适应SLMs的固有限制，如复杂的查询解释、多步推理、查询与文档之间的语义匹配和细微信息合成。
    

为了解决这些问题，论文提出了一个名为MiniRAG的新型RAG系统，该系统通过两个关键技术创新来实现极端简单和高效的设计：语义感知的异构图索引机制和轻量级拓扑增强检索方法。这些创新使得MiniRAG即使在使用SLMs时也能实现与基于LLMs的方法相当的性能，并且只需要25%的存储空间。此外，论文还提供了一个全面的基准数据集，用于在实际的设备上评估轻量级RAG系统在处理复杂查询时的表现。　

#### **其他相关综述**

[16] HAN H, WANG Y, SHOMER H, 等. Retrieval-Augmented Generation with Graphs (GraphRAG)[A/OL]. arXiv, 2025[2025-01-26]. http://arxiv.org/abs/2501.00309. DOI:10.48550/arXiv.2501.00309.　

检索增强生成（RAG）是一种强大的技术，它通过从外部来源检索诸如知识、技能和工具等额外信息，来提升下游任务的执行效果。图因其内在的 “由边连接节点” 的特性，编码了大量异构且具有关联性的信息，这使其在众多实际应用中成为RAG的宝贵资源。因此，我们最近看到越来越多的关注聚焦于为RAG配备图结构，即图检索增强生成（GraphRAG）。然而，与传统RAG不同，在传统RAG中检索器、生成器和外部数据源可以在神经嵌入空间中统一设计，而图结构数据的独特性，例如格式多样和特定领域的关系知识，在为不同领域设计GraphRAG时带来了独特且重大的挑战。鉴于GraphRAG广泛的适用性、相关的设计挑战以及其近期的迅速发展，迫切需要对其关键概念和技术进行系统且最新的综述。基于这一动机，我们对GraphRAG进行了全面且最新的综述。　

我们的综述首先通过定义其关键组件，包括查询处理器、检索器、组织者、生成器和数据源，提出了一个整体的GraphRAG框架。此外，认识到不同领域的图呈现出不同的关系模式且需要专门的设计，我们回顾了为每个领域量身定制的GraphRAG技术。最后，我们讨论了研究挑战并集思广益提出方向，以激发跨学科的机遇。我们的综述资源库在https://github.com/Graph - RAG/GraphRAG/ 上公开维护。 　

[17] GUPTA S, RANJAN R, SINGH S N. A Comprehensive Survey of Retrieval-Augmented Generation (RAG): Evolution, Current Landscape and Future Directions[A/OL]. arXiv, 2024[2024-11-08]. http://arxiv.org/abs/2410.12837. DOI:10.48550/arXiv.2410.12837.　

本文对检索增强生成（RAG）进行了全面研究，追溯其从基础概念到当前前沿水平的发展历程。RAG 将检索机制与生成式语言模型相结合，以提高输出的准确性，解决大语言模型（LLMs）的关键局限性。该研究探索了 RAG 的基本架构，重点关注检索与生成如何整合，以处理知识密集型任务。　

文中详细回顾了 RAG 的重大技术进展，包括检索增强语言模型中的关键创新，以及在问答、摘要和基于知识的任务等各个领域的应用。讨论了近期的研究突破，提出了提高检索效率的新方法。此外，本文还审视了诸如可扩展性、偏差以及部署中的伦理问题等当前面临的挑战。提出了未来的研究方向，重点在于提升 RAG 模型的稳健性、扩大 RAG 模型的应用范围，以及解决其社会影响问题。　

本综述旨在为研究人员和从业者提供基础资源，帮助他们理解 RAG 在自然语言处理中的潜力及其发展轨迹。　

# 总结

RAG发展的越来越不像“RAG”了，倒是很像工程实践的框架而且与agent连接越来越紧密，但主要还是依据以下几条思路的研究和创新：　

1. 1. 数据库层面，从最开始的简单词嵌入，到向量数据库，到知识图谱，再到混合的多种类型数据库。
    
2. 2. 数据方面，从单纯的文本扩展到多模态数据，包括文本、音频、图片、视频。获取结构化良好，高质量，干净，冗余小的数据。
    
3. 3. 数据处理方面，从需要大量的预处理步骤到一些端到端的RAG方案，例如用VLM直接处理非结构化文档。
    
4. 4. 知识层面，由于本质还是要让模型在短时间内理解领域知识，所以用各种手段（常见的有微调）优化各种环节中的各种模块，chunk，rerank，embedding，router，检索器，生成器，索引构建，查询优化。以及各个模块之间的超参数要匹配，例如embedding模型的窗口和chunk的大小匹配。
    
5. 5. workflow方面，设计编排一个高效准确的RAG pipeline。
    
6. 6. 推理运行层面，加速RAG响应时间，降低延迟和开销。
    
7. 7. 动态自动化层面，由于RAG涉及的流程和组件越来越复杂，让RAG系统作为一个agentic主动去自适应不同的复杂查询，并自我完善。
    

### 实践中如何选择合适的工具来构建RAG系统

[18] WANG X, WANG Z, GAO X, 等. Searching for Best Practices in Retrieval-Augmented Generation[A/OL]. arXiv, 2024[2025-01-26]. http://arxiv.org/abs/2407.01219. DOI:10.48550/arXiv.2407.01219.　

这篇论文探讨了检索增强型生成（Retrieval-Augmented Generation, RAG）技术在提升大型语言模型（Large Language Models, LLMs）性能方面的应用。RAG技术通过结合预训练模型和基于检索的模型的优势，提供了一个增强模型性能的稳健框架。然而，尽管RAG技术在整合最新信息、减少幻觉（hallucinations）和提高响应质量方面已被证明是有效的，特别是在专业领域，但现有的RAG方法仍然存在实施复杂和响应时间过长的问题。　

论文的主要目标是通过广泛的实验来识别RAG的最佳实践，以平衡性能和效率。具体来说，论文试图解决的问题包括：　

1. 1. RAG方法的复杂性：RAG工作流程涉及多个处理步骤，每个步骤都可以以不同的方式执行，这增加了实施的复杂性。
    
2. 2. 响应时间的延长：在执行RAG时，需要在多个步骤中进行选择，这可能影响系统的效率和响应时间。
    
3. 3. 系统性能的优化：如何系统地优化RAG流程中的每个组件，以实现整体性能的提升。
    
4. 4. 多模态检索技术的整合：探索如何将多模态检索技术整合到RAG中，以增强对视觉输入的问题回答能力，并加速多模态内容的生成。
    

论文通过实验研究了现有的RAG方法及其潜在的组合，并提出了一些策略，以便于在不同的应用场景中部署RAG，同时平衡性能和效率。此外，论文还展示了如何通过“检索即生成”策略，利用多模态检索技术显著提升对视觉输入的问题回答能力，并加速多模态内容的生成。　

# **工程实践**

## **RAG框架（强推RAGFlow）**

这里langchain，llama_index等python包当然也是可以的，但是开发难度比较高。　

## **文档解析（强推MinerU）**

另一种是用多模态大模型方案构建端到端的RAG流程　

## **RAG的12个痛点**

检索增强生成（RAG）技术虽然在提升内容准确性和相关性方面具有显著优势，但在实际应用中也存在一些痛点。根据参考资料，我们可以大致总结下存在的共性痛点以及解决方案：　

1. 1. 内容缺失：当知识库中缺少上下文时，RAG系统可能会提供一个看似合理但不正确的答案，而不是表示不知道。解决方案包括清理数据和精心设计提示词。
    
2. 2. 错过排名靠前的文档：重要文档可能未出现在系统检索组件返回的顶部结果中，导致系统无法提供准确的响应。解决方案包括调整检索策略和嵌入模型调优。
    
3. 3. 不在上下文中 — 整合策略限制：文档整合长度限制超过LLM窗口大小，导致整合策略受限。解决方案是调整检索策略和嵌入模型调优。
    
4. 4. 文件信息未提取：文档中的关键信息未被提取出来。解决方案包括数据清洗、提示词压缩和长内容优先排序。
    
5. 5. 格式错误：输出格式与预期不符。解决方案是改进提示词、格式化输出和使用大模型的Json模式。
    
6. 6. 答案不正确：缺乏具体细节，导致特需求的答案不正确。解决方案是采用先进的检索策略。
    
7. 7. 回答不完整：回答不全面。解决方案包括查询转换和细分问题。
    
8. 8. 数据提取可扩展性：数据摄取的可扩展性问题。解决方案是并行处理和提升处理速度。
    
9. 9. 结构化数据QA：结构化数据问答问题。解决方案是链式思维表格包和混合自洽查询引擎包。
    
10. 10. 从复杂PDF中提取数据：从复杂PDF中提取数据困难。解决方案是嵌入式表格检索技术。
    
11. 11. 后备模型：需要一个后备模型策略。解决方案是Neutrino路由器或OpenRouter。
    
12. 12. LLM安全性：大语言模型的安全性问题。这是一个需要持续关注和解决的问题。
    

## **RAG落地时需要考虑的若干问题**

- 检索效率低下:
    

- 痛点描述: 在庞大的数据集中进行有效检索是一个挑战，尤其是当需要实时响应时。
    
- 相关问题: 如何优化检索算法以减少查询延迟?
    

- 信息融合困难:
    

- 痛点描述: 将检索到的信息与生成的内容无缝融合是一项复杂任务，需要精确的算法来确保信息的准确性和连贯性。
    
- 相关问题: 如何设计有效的信息融合策略?
    

- 上下文理解的局限性:
    

- 痛点描述: 模型可能难以准确理解查询的上下文，特别是在复杂或模糊的情境中。
    
- 相关问题: 如何提高模型对上下文的理解能力?
    

- 数据偏差和噪声:
    

- 痛点描述: 检索到的数据可能包含偏差和噪声，这会影响模型的输出质量。
    
- 相关问题: 如何识别并减少数据中的偏差和噪声?
    

- 答案准确性和可靠性问题:
    

- 痛点描述: 生成的答案可能不够准确或可靠，尤其是在需要精确事实性回答的情况下。
    
- 相关问题: 如何验证和提高生成答案的准确性?
    

- 可扩展性问题:
    

- 痛点描述: 随着数据量的增加，模型可能难以保持高性能和可扩展性。
    
- 相关问题: 如何确保模型能够处理大规模数据?
    

- 资源消耗:
    

- 痛点描述: RAG技术通常需要大量的计算资源，这在资源受限的环境中是一个挑战。
    
- 相关问题: 如何优化模型以减少资源消耗?
    

- 隐私和安全问题:
    

- 痛点描述: 处理敏感数据时，需要确保用户隐私和数据安全。
    
- 相关问题: 如何实现隐私保护的数据处理?
    

# 参考文献

[1] ZHAO P, ZHANG H, YU Q, 等. Retrieval-Augmented Generation for AI-Generated Content: A Survey[A/OL]. arXiv, 2024[2024-06-21]. http://arxiv.org/abs/2402.19473.　

[2] GAO Y, XIONG Y, GAO X, 等. Retrieval-Augmented Generation for Large Language Models: A Survey[A/OL]. arXiv, 2024[2024-03-27]. http://arxiv.org/abs/2312.10997.（best)　

[3] FAN W, DING Y, NING L, 等. A Survey on RAG Meeting LLMs: Towards Retrieval-Augmented Large Language Models[A/OL]. arXiv, 2024[2024-06-17]. http://arxiv.org/abs/2405.06211. 　

[4]LEWIS P, PEREZ E, PIKTUS A, 等. Retrieval-Augmented Generation for Knowledge-Intensive NLP Tasks[A/OL]. arXiv, 2021[2025-01-27]. http://arxiv.org/abs/2005.11401. DOI:10.48550/arXiv.2005.11401.　

[5] JIN J, ZHU Y, YANG X, 等. FlashRAG: A Modular Toolkit for Efficient Retrieval-Augmented Generation Research[A/OL]. arXiv, 2024[2024-11-03]. http://arxiv.org/abs/2405.13576. DOI:10.48550/arXiv.2405.13576.　

[6] SARMAH B, HALL B, RAO R, 等. HybridRAG: Integrating Knowledge Graphs and Vector Retrieval Augmented Generation for Efficient Information Extraction[A/OL]. arXiv, 2024[2024-08-24]. http://arxiv.org/abs/2408.04948. DOI:10.48550/arXiv.2408.04948.　

[7] GAO Y, XIONG Y, WANG M, 等. Modular RAG: Transforming RAG Systems into LEGO-like Reconfigurable Frameworks[A/OL]. arXiv, 2024[2024-08-24]. http://arxiv.org/abs/2407.21059. DOI:10.48550/arXiv.2407.21059.　

[8] PENG B, ZHU Y, LIU Y, 等. Graph Retrieval-Augmented Generation: A Survey[A/OL]. arXiv, 2024[2024-08-21]. http://arxiv.org/abs/2408.08921.　

[9] EDGE D, TRINH H, CHENG N, 等. From Local to Global: A Graph RAG Approach to Query-Focused Summarization[A/OL]. arXiv, 2024[2024-08-03]. http://arxiv.org/abs/2404.16130. DOI:10.48550/arXiv.2404.16130.　

[10] SINGH A, EHTESHAM A, KUMAR S, 等. Agentic Retrieval-Augmented Generation: A Survey on Agentic RAG[A/OL]. arXiv, 2025[2025-01-26]. http://arxiv.org/abs/2501.09136. DOI:10.48550/arXiv.2501.09136.　

[11] ASAI A, WU Z, WANG Y, 等. Self-RAG: Learning to Retrieve, Generate, and Critique through Self-Reflection[A/OL]. arXiv, 2023[2025-01-27]. http://arxiv.org/abs/2310.11511. DOI:10.48550/arXiv.2310.11511.　

[12] YU S, TANG C, XU B, 等. VisRAG: Vision-based Retrieval-augmented Generation on Multi-modality Documents[A/OL]. arXiv, 2024[2024-10-30]. http://arxiv.org/abs/2410.10594.　

[13] FAYSSE M, SIBILLE H, WU T, 等. ColPali: Efficient Document Retrieval with Vision Language Models[A/OL]. arXiv, 2024[2024-10-27]. http://arxiv.org/abs/2407.01449. DOI:10.48550/arXiv.2407.01449.　

[14] FENG W, HAO C, ZHANG Y, 等. AirRAG: Activating Intrinsic Reasoning for Retrieval Augmented Generation via Tree-based Search[A/OL]. arXiv, 2025[2025-01-27]. http://arxiv.org/abs/2501.10053. DOI:10.48550/arXiv.2501.10053.　

[15] FAN T, WANG J, REN X, 等. MiniRAG: Towards Extremely Simple Retrieval-Augmented Generation[A/OL]. arXiv, 2025[2025-01-26]. http://arxiv.org/abs/2501.06713. DOI:10.48550/arXiv.2501.06713.　

[16] HAN H, WANG Y, SHOMER H, 等. Retrieval-Augmented Generation with Graphs (GraphRAG)[A/OL]. arXiv, 2025[2025-01-26]. http://arxiv.org/abs/2501.00309. DOI:10.48550/arXiv.2501.00309.　

[17] GUPTA S, RANJAN R, SINGH S N. A Comprehensive Survey of Retrieval-Augmented Generation (RAG): Evolution, Current Landscape and Future Directions[A/OL]. arXiv, 2024[2024-11-08]. http://arxiv.org/abs/2410.12837. DOI:10.48550/arXiv.2410.12837.　

[18] WANG X, WANG Z, GAO X, 等. Searching for Best Practices in Retrieval-Augmented Generation[A/OL]. arXiv, 2024[2025-01-26]. http://arxiv.org/abs/2407.01219. DOI:10.48550/arXiv.2407.01219.　

[19] Papers with Code - RAG[EB/OL]. [2025-01-28]. https://paperswithcode.com/task/rag.　

[20] Graph Memory[EB/OL]. [2025-01-28]. https://docs.mem0.ai/open-source/graph-memory.　

[21] OROZ T. Comparative Analysis of Retrieval Augmented Generator and Traditional Large Language Models[J]. Data Science.　

[22] INFINIFLOW. 万字长文梳理 2024 年的 RAG[EB/OL]. [2025-01-28]. http://mp.weixin.qq.com/s?__biz=MzkyMTU5MDM2MQ==&mid=2247484133&idx=1&sn=196c5c05baa8896555c8f2cab895c681&chksm=c039ee778047e58bc96c44caafb88d17168076736c090a08da93a6f44704a67634c09063da15#rd.　

  

**技术交流群邀请函**

![Image](https://mmbiz.qpic.cn/mmbiz_jpg/nJZZib3qIQW63KWRJKH7ltGE4lYffe4h93yRiaT0mqcwpK9tgx5bYsO4fOLOBmWzt6NYKmWiayojAwUFTDYfrY3yg/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp)

△长按添加小助手

扫描二维码添加小助手微信

请备注：**姓名-学校/公司-研究方向**

（如：小张-哈工大-对话系统）

即可申请加入**自然语言处理/Pytorch**等技术交流群

## 关于我们

****MLNLP** 社区**是由国内外机器学习与自然语言处理学者联合构建的民间学术社区，目前已经发展为国内外知名的机器学习与自然语言处理社区，旨在促进机器学习，自然语言处理学术界、产业界和广大爱好者之间的进步。

社区可以为相关从业者的深造、就业及研究等方面提供开放交流平台。欢迎大家关注和加入我们。

![Image](https://mmbiz.qpic.cn/mmbiz_png/nJZZib3qIQW7C30ENdIvwgnBmylorcfa4XfgvyichrjJDqGxqDkm7nUQibQrxaVJhXHiaXXbFf7x23t5P2S4PrQwFA/640?wx_fmt=other&wxfrom=5&wx_lazy=1&wx_co=1&tp=webp)

![赞赏二维码](https://mp.weixin.qq.com/s/PBN3zr4fazGJs7yy9CkuuQ)

​

![](https://mp.weixin.qq.com/mp/qrcode?scene=10000004&size=102&__biz=MzI4MDYzNzg4Mw==&mid=2247570047&idx=2&sn=e3a230a1b5ff5ed56f5be213fc09aa8c&send_time=)

Scan to Follow

[](javacript:;)

![](https://gips2.baidu.com/it/u=4003037145,3545308168&fm=3028&app=3028&f=PNG&fmt=auto&q=100&size=f108_108)