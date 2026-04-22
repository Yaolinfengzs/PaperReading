作者团队：Physical Intelligence
发表状态： October 31, 2024
Project & Coed：[pi0.pdf](https://www.pi.website/download/pi0.pdf)
技术特性：FlowMatching,autogressive,diffudion model,VLA foundation model
关键词：FlowMatching
模型骨干：

### 1.Summary（解决的问题，输入输出）

如何构建一个通用的机器人模型，具备在多机器人平台，多指令，多任务场景下，经过一定策略的预训练和后训练后，能执行高效，精准，长序列任务。其瞄准的是RT-1的自回归动作生成，离散化的动作天然的不具备高效，精确的特性。同时，认为机器人通用模型需要解决多机械平台多任务的问题。
总结：
- 泛化性（文章的解决方案是合理配置training recipe=高质量小规模数据和低质量大规模数的合理配置）
- 动作的高效，精确，action chunk

输入：多模态
输出：动作块
### 2.Motivation(第一性原理)
当前VLA模型存在什么问题？泛化性不足，长任务序列能力不足，精确性和鲁棒性不足
那么理想的通用机器人模型应该是什么样子的？多机械平台多任务精确工作，鲁棒性强
怎样确保泛化性？
- 从数据上来说，训练时使用多平台机械臂数据，提高泛化性；预训练使用**多平台、多任务、多来源** 的数据混合，提高鲁棒性，语义理解；后训练使用自己创建的高质量小规模数据集，提高动作产生的高效性，精确性；
那么动作的精确性怎样提升呢？
- 使用高质量小规模数据集
- 从架构上来说，VLM backbone + action expert + FlowMatching generation生成连续动作（ACT）
### 3.Motheds

### 4.Key Novelty

总结一句话：机器人基础模型的关键，不是怎样VLM放到具身智能载体上，而是将感知，理解，动作生成放到同一个框架下，采取合适的train pipline，使其具有多平台多任务的高精度执行能力。

###### 架构上的创新：VLM backbone + action expert + FlowMatching generation生成连续动作（ACT）

- VLM backbone使用互联网级别的大规模数据预训练，语义能力很强
- action expert是从transfusion+MOE的思路参考的，对于action token和task token(patch_embeddings+Prompt_embeddings)天然不适配的情况，使用两套权重分别处理
- FlowMatching是一种动作生成范式
  - 区别于autogressive action generation,其生成action chunk，思路与动作是连续的天然相符合
  - 区别于DDPM,flow matching学习的是当前点到目标点的路径，通常是直线，比DDPM更块
- vertical hierarchy.使用大小脑架构，大脑负责长任务规划，小脑负责生成动作。

###### 训练上的创新：基础模型需要借鉴LLM的