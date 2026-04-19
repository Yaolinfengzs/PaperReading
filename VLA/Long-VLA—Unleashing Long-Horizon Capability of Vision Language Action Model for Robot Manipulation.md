### 1.Summary
本文章致力于解决机器人控制中长时序任务的问题．其将长时序任务分成两个阶段，移动阶段和交互阶段，创新了Input-level Adaptation via Masking（基于掩码的输入级自适应策略），阻断了长时序任务的==**误差累计问题**==
[[Fine-Tuning Vision-Language-Action Models—Optimizing Speed and Success]]不同的处理**误差累积**的思路
### 2.Key Novelty
1. 长时序任务分成移动阶段和交互阶段．本文通过在向量中增加一个值为0/1的维度,区分两个阶段.在训练数据中,通过启发式规则给出了这个维度的答案,而这个维度在推理数据中是需要进行预测的.
2. Input-level Adaptation via Masking(掩码输入级自适应策略)
	设置一个掩码矩阵,只有Q和K对应的向量掩码值**均为1(激活状态)**,才会计算注意力权重.由此可以在不同的阶段选择需要关注的Token信息
	优势:过滤掉了干扰信息,**减少了误差累计问题**
	- 在移动阶段,关注第三人称视角(注重全局的信息)
	- 在交互阶段,关注第一人称视角(注重操作的精细度)
3. 聚焦目标任务:
	- 使用Grounding DINO 集成预测像素边界获得细粒度的空间信息
	- 将获得的**边界信息编码为特征向量Ed**,使用**FiLM**(特征级线性调试)([[Fine-Tuning Vision-Language-Action Models—Optimizing Speed and Success]])嵌入专注目标物体,减小干扰
### 3.Key Results
![[Long-VLA——Unleashing Long-Horizon Capability of Vision Language Action Model for Robot Manipulation－2026-01-06－21-14-23.png]]
### 4.Thoughts
可以借鉴这种动态掩码选择参与的实现思路
**FiLM聚焦机制**:[[Fine-Tuning Vision-Language-Action Models—Optimizing Speed and Success]]中也使用了FiLM方法

