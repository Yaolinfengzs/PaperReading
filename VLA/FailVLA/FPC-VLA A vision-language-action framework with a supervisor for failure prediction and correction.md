作者团队：
- Tianjin Key Laboratory of Intelligent Robotics, and TBI Center（天津市智能机器人技术重点实验室）
- Xiaomi EV
- Faculty of Robot Science and Engineering, Northeastern University
- 澳门大学
发表状态：2025.12
Project & Coed：None
技术特性：自动生成失败轨迹，离散动作预测，外接模块，failure correction dataset generation，dual-stream action fusion
关键词：Failure recover,vla，failure correction dataset generation
模型骨干：Qwen

### 1.Summary（解决的问题，输入输出）
当前VLA模型，对于失败，大多是失败后处理，缺少失败前预警和失败时纠偏，一旦偏离成功轨迹，往往不会自救。本文创建了一个Spuervisor模块和检测，中断，纠偏的结构，使得VLA在关键时刻进行检测，发现失败立刻接管。
输入：当前的图像，文本prompt
输出：Yes/No; if No,输出粗粒度控制
### 2.Motivation(第一性原理)
1. 当前VLA模型能看懂，能输出动作，但是不知道动作对不对
2. 重新训练一个VLA模型很麻烦，考虑外接监督模块
3. 外接模块每一个时刻都要检查吗？只需要在高风险时刻检查，本文在夹爪开取时刻判断
4. 直接输出7维数据，会不会太难了？输入是当前时刻图像和文本prompt，输出是失败与否和指导动作
5. 纠偏连续corrention难学，成本高昂，考虑离散的粗粒度的纠偏
6. 数据集从哪里来？可以从成功的样本中自动生成失败数据集（failure correction dataset generation）
7. 回到动作本身，历史数据有没有用？历史动作预测存在正确的信息，把模式相同的pose混合，把grisper和pose分开处理（dual-stream action fusion）
### 3.Motheds
1. 从成功的轨迹中自动构建失败数据集
2. 对于pose和grisper数据分别处理；利用历史动作预测中匹配的pose
3. 只在关键时刻启动supervisor
4. 利用LoRa微调Qwen模型，只输出粗粒度动作，纠正动作离散化
### 4.Key Novelty
##### N1：VLA关键时刻容易出错
VLA+VLM-supervisor+历史动作融合架构
- 主VLA生成精细动作
- VLM-supervisor关键时刻监督
- 输出中断判断和粗粒度动作
- 清空历史队列
##### N2：输出精细动作成本高
让 supervisor 用 VLM 做离散决策；纠偏连续corrention难学，成本高昂，考虑离散的粗粒度的纠偏
##### N3：历史数据的使用+双流动作融合
历史动作预测存在正确的信息，把模式相同的pose混合；
把grisper和pose分开处理（dual-stream action fusion）
#### N4：数据集怎么构建？
从现有 RLDS 成功轨迹自动生成 failure prediction/correction 数据
### 5.Key Results
- **SIMPLER / Google Robot**：Visual Matching 平均 **78.0**，Variant Aggregation 平均 **65.8 ± 0.3**
- **LIBERO 平均**：**86.9%**
- **LIBERO-Long**：**82.2%**，高于第二名 **70.9%**
- **真实机器人平均**：grasp **92.8%**，task success **86.0%**
- Action fusion的消融实验
跨平台泛化强；长时序任务强；实机强。
### 6.Limitations
1. 延迟高，VLM推理时，机械臂可能处在静止状态
2. 只在grisper时刻判断风险，但是任务失败还有抓起来后物体滑落，长时序中途偏航等问题
3. 纠偏空间太粗糙
4. 问题设定仍偏静态环境、短到中等时长任务
### 7.启发和借鉴：
启发：
- ==Aciton fusion历史动作预测的使用==
- 如果使用机械臂状态，可以借鉴pose和gripper的分离处理
- 只在高风险时介入
- 数据集的自动构建
- 离散动作生成，粗粒度控制
借鉴：
- 作者在消融实验中已经证明了Action fusion的有效性：单步动作抖动；action chunk 信息浪费；多模态动作平均失真。==使用哪种方法，可以去文章中探索一下==
- 借鉴高风险时介入，但是这样会导致创新点不够，因此需要模块足够小，强调延迟低，online判断
- 历史动作有效性。对于中断触发的判断，通过加入历史的patch_embedding避免延迟触发
- 更见坚定动态阈值；预测图像；局部patch_embedding(top-k)作为一种创新点。

