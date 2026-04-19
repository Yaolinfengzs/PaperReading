![[代码笔记-VLA-ADAPTER AN EFFECTIVE PARADIGM FOR TINY-SCALE VISION-LANGUAGE-ACTION MODEL－2026-01-24－17-24-14.png]]
*************
VLM sequential token prediction 序列式令牌预测
********************
==注意数据的输入输出形式==
模型训练的大致步骤：
1. 数据准备
	- 数据收集
	- 数据清洗：提高数据质量
	- 数据处理：分词，图像裁剪等
	- 数据分组：70到80的训练数据集，10的验证数据集，15的测试数据集
2. 训练配置：
	- 损失函数：常用的有L1 loss，均方误差，使误差越小越好
	- 优化器：决定如何根据Loss更新参数
	- 超参数：设置Epochs,Batch Size,Learning Rate(学习率，每一次更新参数的幅度有多大，就像蒙着眼睛下山，如果步子迈着太大，可能错过下山点直接上山)
3. 核心训练循环
	- 前向传播：预测结果y
	- 计算损失：理想目标和预测结果y计算误差
	- 反向传播：计算各个参数的梯度，即参数对于总误差的影响程度
	- 参数校正：根据梯度修改模型的参数

VLA模型的训练步骤：
1. 模型架构
	- Backbone（骨干网络）：使用预训练的视觉模型提取图像的视觉特征
	- LLM（大语言模型），即Language backbone：推理核心（大脑）
	- Projector（注入器）：将Vision Backbone提取的视觉特征融合到LLM大脑中
2.  数据准备和联合微调（Co-Fine-tuning）
	联合微调的含义是混合多种任务类型的数据，避免出现灾难性遗忘，不仅要学会动作，还要学会语义
3. Efficient Fine-tuning- Strategy（高效微调策略）和Full Fine-tuning（全量微调）
	![[代码笔记-VLA-ADAPTER AN EFFECTIVE PARADIGM FOR TINY-SCALE VISION-LANGUAGE-ACTION MODEL－2026-01-28－15-12-00.png]]
	高效微调冻结99%的骨干网络参数，只训练其中很小的一部分参数
	FFT和PEFT比较以及应用场景：
		**优点**：
		1. 性能的上限高，所有参数都更新意味着模型最大程度的想下游任务目标调整
		2. 彻底的领域适应
		**缺点**：
		- 训练成本高昂，由于更新所有的参数，训练时需要大量的显存
		- 部署条件高，存储时需要较大的内存，可能导致推理速度慢
		- 容易导致灾难性遗忘，更新所有的参数使得预训练好的模型失去了原有的功能
		- 数据需求量大
		**应用场景**
		- PEFT效果不好
		- 硬件资源和数据资源充足
### 1. 数据准备
文件夹primastic（中文的意思是“棱镜”）是核心代码库：给其他模块提供功能块
/primastic
- /training 
	- 功能：提供训练相关的工具和模块，主要是为训练过程提供支持
	- 职责：
		- init.py（提供统一的文件夹级别的接口，避免使用复杂具体文件级别的路径）
		- metrics.py（评估）
		- train_utils.py（训练相关的工具函数，如掩码生成，损失计算）
		- strategies文件夹（训练策略）
			- base_strategies.py（定义基类TrainingStrategy）
			- ddp.py（分布式数据并行）和fsdp.py（完全分片数据并行）（继承基类，实现两种策略）
		- materialize.py（工厂模式，实例化ddp.py和fsdp.py训练逻辑）
	- 实际上通过以方法实现了
- /models
	- projectors.py：==实现Brige Attention==的文件
	- 

Question1
4前向传播步骤4中拼接视觉和文本是否为brige attention的部分？
Q2
6推理生成阶段，为什么和openvla有关？

![[代码笔记-VLA-ADAPTER AN EFFECTIVE PARADIGM FOR TINY-SCALE VISION-LANGUAGE-ACTION MODEL－2026-01-28－22-50-23.png]]