### 大模型的发展历史
### 常见的大模型解释
#### [[Transformer架构]]
#### [[BERT（Bidirectional Encoder Representations from Transformers）]]
#### GPT

The encoder of the Transformer, when used independently and after undergoing pre-training and fine-tuning, becomes BERT; 
similarly, the decoder, after similar pre-training and fine-tuning, becomes GPT. BERT and GPT are applied to different natural language processing tasks: BERT is primarily used for understanding tasks, while GPT is mainly used for generation tasks.



The encoder of the Transformer, when used independently and after undergoing pre-training and fine-tuning, becomes BERT; similarly, the decoder, after similar pre-training and fine-tuning, becomes GPT. BERT and GPT are applied to different natural language processing tasks: BERT is primarily used for understanding tasks, while GPT is mainly used for generation tasks. BERT (Bidirectional Encoder Representations from Transformers) is a pre-training model proposed by Devlin et al.[17]. The core idea of BERT is to generate contextual representations of words using a bidirectional Transformer encoder. It achieves performance improvements across various NLP tasks by performing unsupervised pre-training on large-scale corpora, followed by fine-tuning on specific tasks. BERT’s pre-training involves two tasks: Masked Language Model (MLM) and Next Sentence Prediction (NSP). In the MLM task, BERT randomly masks some words in the input text and predicts these masked words using the model, thereby learning the relationships between words. In the NSP task, BERT learns the relationships between sentences by determining whether two sentences are continuous. GPT (Generative Pre-trained Transformer) is a series of generative pre-training models proposed by OpenAI[18, 19, 20]. Unlike BERT, GPT mainly uses a unidirectional Transformer decoder to generate text. GPT’s training consists of two stages: first, pre-training on large-scale unlabelled data, and then fine-tuning on specific tasks. During the pre-training stage, the model uses an autoregressive method to predict the next word, thereby learning the sequence information of words. GPT performs exceptionally well in generation tasks because it can generate coherent and meaningful text based on the given context. With the development of the GPT series, GPT-2 and GPT-3 introduced more parameters and larger model scales, further improving generation quality and task generalization capabilities.


白等人。[65]对数据在开发多通道大语言模型(MLLMS)中的作用进行了全面的调查。文章**强调了文本、图像、音频和视频等多模式数据的质量、多样性和数据量在有效训练MLLMS中的重要性**。它确定了多模式数据收集中的重大挑战，如数据稀疏性和噪声，并探索了潜在的解决方案，如**合成数据生成和主动学习**，以缓解这些问题。作者主张更多地以**数据为中心的方法，其中数据的提炼和管理优先，最终提高模型性能并在日益复杂的环境中推进MLLM功能**。Yang等人。[66]研究大型语言模型(LLM)和代码的交集，将**代码**作为增强LLMS作为智能代理运行的能力的重要工具。本文讨论了代码如何在自动化、代码生成和软件开发等任务中支持LLM。它还解决了有关代码正确性、效率和安全性的关键挑战，这些都是在实际应用中部署LLM时必不可少的。通过分析代码与LLMS的集成，作者展示了这些模型演变为能够管理跨不同领域的复杂任务的自治代理的潜力，从而扩大了LLMS在人工智能驱动的创新中的范围和影响。


4.6 Contunual Learning

4.9 MLLMs in Graph Learning

4.10 Retrieval-Augmented Generation (RAG) in MLLM(检索-增强技术)

5、Bias: Hallucinations and Data Bias 多模型中的幻觉现象
Hanchao Liu, Wenyuan Xue, Yifei Chen, Dapeng Chen, Xiutian Zhao, Ke Wang, Liping Hou, Rongjun Li, and Wei Peng. A survey on hallucination in large vision-language models, 2024.

Multimodal large language models (MLLMs) have been increasingly applied to graph learning tasks,==outperforming traditional graph neural networks (GNNs)==. By incorporating textual attributes and other modalities, MLLMs enhance the representational power of GNNs, enabling improved performance in classification, prediction, and reasoning tasks. Jin et al. proposed a taxonomy categorizing the integration of MLLMs with graphs into enhancers, predictors, and alignment components, highlighting their utility in various graph tasks. Similarly, Chen et al. and Li et al. discuss the integration of LLMs with knowledge graphs, illustrating the benefits of combining graph structures with multimodal data for broader applications [74, 75, 76].
[74]Bowen Jin, Gang Liu, Chi Han, Meng Jiang, Heng Ji, and Jiawei Han. Large language models on graphs: A comprehensive survey. arXiv preprint arXiv:2312.02783, 2023. 
[75] Zhuo Chen, Yichi Zhang, Yin Fang, Yuxia Geng, Lingbing Guo, Xiang Chen, Qian Li, Wen Zhang, Jiaoyan Chen, Yushan Zhu, et al. Knowledge graphs meet multi-modal learning: A comprehensive survey. arXiv preprint arXiv:2402.05391, 2024. 
[76] Yuhan Li, Zhixun Li, Peisong Wang, Jia Li, Xiangguo Sun, Hong Cheng, and Jeffrey Xu Yu. A survey of graph meets large language model: Progress and future directions. arXiv preprint arXiv:2311.12399, 2023.

Cross-Domain Applications and Transfer Learning
[81] Xiao Wang, Guangyao Chen, Guangwu Qian, Pengcheng Gao, Xiao-Yong Wei, Yaowei Wang, Yonghong Tian, and Wen Gao. Large-scale multi-modal pre-trained models: A comprehensive survey, 2024.

Cross-Domain Applications and Transfer Learning