---
title: VLA-ADAPTER-AN EFFECTIVE PARADIGM FOR TINY-SCALE VISION-LANGUAGE-ACTION MODEL
authors: "[object Object]"
from:
year: 2025-12-24
tags:
  - 顶会
  - "#VLA-EFFICENT"
descibe: 通过探究最合适的conditions，使用自动化注入conditions方法，将Additional Query和RawFeature结合充分利用，达到了large scale模型的性能表现，缓解了训练速度慢问题，可以在消费级GPU上训练和部署
date created: 2025-12-09T11:33:00
date end:
date modified:
"paper link:": https://arxiv.org/abs/2509.09372
参考文献阅读: "[[Inference-VLA-ADAPTER-AN EFFECTIVE PARADIGM FOR TINY-SCALE VISION-LANGUAGE-ACTION MODEL]]"
代码分析: "[[代码笔记-VLA-ADAPTER AN EFFECTIVE PARADIGM FOR TINY-SCALE VISION-LANGUAGE-ACTION MODEL]]"
---

阅读目的
- [ ] 写文献综述、查资料  
- [ ] 了解新领域  
- [ ] 训练提升学术能力  
- [x] 借鉴文章具体方法  
- [ ] 回答下列问题：

作者的写作目的
- [ ] 总结其他研究  
- [ ] 建议未来方向  
- [x] 报告新发现  
- [x] 发展理论/算法  
- [ ] 表达特别的观点

阅读状态: 
- [ ] 泛读
- [ ] 略读
- [x] 精读
- [ ] 深读  
**************************
1. discretize 离散化
2. exclusively 仅仅，单独；exclusively 仅仅，单独；exclusively 仅仅，单独；exclusively 仅仅，单独
3. proprioceptive 本体感知的 proprioceptive state 本体感知状态
4. from scratch 从零开始
5. prominent 重要的，显著的
6. mitigate 减轻,缓解

文章通过**“中间层性能优于深层”的实验数据**，反推得出“深层特征过于偏向语义而丢失了动作所需的细节”这一结论，并据此设计了门控机制
*****************
### Abstracta
VLA模型通常通过预训练一个基于机器人数据的大规模（大规模指的是模型参数量和预训练的数据量）VLM模型，来填补感知和动作空间之间的鸿沟。尽管这种方法显著的增强了表现，但是它产生了显著的训练成本。
- 在这篇文章中，我们提出了一个高效率的桥梁来连接视觉语言表征和动作（VL to A ）。
- 我们提出了VLA—Adapter，这是一个新范式用来减少VLA模型对大规模VLM的依赖和自身昂贵的训练成本。
- 最后，我们首次系统的分析了多种视觉语言条件的有效性，并展示了哪一个条件是最有利于填补感知和动作空间的关键发现。基于这些见解,我们发布了一个轻量化政策的基于桥接注意力的模块,它自动的将最有利的条件注入到动作空间中.
通过这种方法,我们的方法成功实现了高性能表现而仅仅只使用了0.5B参数量的骨干网络,并且没有任何的机器人数据的预训练.在仿真和真实世界机器人一般标准的昂贵实验展示了VLA-Adapter不仅仅实现了最高水准的表现,也展示了最快的推理速度截至发布的日期.同时,先进的桥接范式的提出,让VLA-Adapter能够训练一个有能力的VLA模型仅仅8个小时在单张消费级的CPU上,显著的降低了部署VLA模型的障碍.
### Introduction
*****
==介绍了文章解决这个问题的背景==
在过去的两年时间里,随着在多模态大语言模型的领域的显著突破,发展机器人系统的普通感知,理解,行为能力已经成为一个在人工智能领域关键的研究方向.特别来说,VLA模型的出现提供了是一个使机器人行动能够被指令所驱使的解决方案.
VLA的研究首先聚焦于提取多模态信息和在动作空间对齐多模态信息从而生成高质量的动作.当前的VLA模型通常需要大规模的[[具身智能数据(embodied data)]],并且需要为了任务的适应性来预训练一个多模态大语言模型(特别是指VLM),然后去设计一个策略网络去解码或者生成动作来处理多种环境之下的任务.
然而,当面临[[高维度控制的环境]]时,VLA模型仍然面临多种困境,包括过于依赖大规模训练的VLMs,缓慢的微调速度,高昂的GPU内存开销,以及缓慢的推理速度.
为此,有必要去探索在VLA领域最重要但很少被讨论的一个问题:怎样更高效的去填补VL空间到动作空间的间隙?
***********
==解决问题的思路,以及创新==
为了解决这个问题,我们提出了VLA-Adapter,一个全新的VLA桥接范式．我们系统的探索了不同的条件（==原文中并没有指出这个conditions是什么,写论文的时候需要注意==）怎样去影响动作生成，并且给予一些VLA设计的关键发现.在这个基础上,我们创建了一个带桥接注意力机制的策略网络来自动的注入最佳条件到动作空间中.
实验表明VLA-Adapter已经有了优秀的性能,很快的推理速度,以及用小规模的骨架网络实现快速的吞吐量.这有效的降低了VLA部署的障碍.主要的贡献被总结为以下几点:
- 据我们所知,这个工作首次系统的分析了桥接范式在动作生成方面的效率.并且我们也提供了以下VLA模型设计的关键发现.
- VLA-Adapter将充分的多模态信息传输到提出的策略网络从而进行动作生成,有效的弥补了VL到A的模态差异.
- 充分的实验表明VLA-Adapter已经有了更好的成功率,更小的规模,更低的调整成本,以及在多种仿真和真实世界下机器人任务的更快推理速度.
**********************
FInerly:解决两个问题
- VLA模型开销大
- VLA模型推理速度慢
- 核心问题：怎样弥补VL to A
*****************
## 文章总结

> 写完笔记之后最后填，概述文章的内容，以后查阅笔记的时候先看这一段。注：写文章summary切记需要通过自己的思考，用自己的语言描述。忌讳直接Ctrl + c原文。
### 主要创新点在哪里？

### RelateQuestion　　　　　　　　　　　　　　　　　　　　　　　
- These methods typically introduce an intermediate latent token to connect the VLMs and the Policy, using an asynchronous mechanism to enhance coordination between the two systems (Zhang et al., 2024). This design mitigates latency issues during action generation
  双系统架构中，system1和system2之间存在时序问题
  1. 时序不匹配。S2推理速度远远慢于S1，那么指导S1的S2结果往往是由当前状态之前的几个状态来推理得到的，对应的是**过时**的策略
  2. 训练和推理的分布偏移。在训练时，模型输入的数据是同步的，导致在推理时，模型认为输入也是实时的，信息滞后导致推理失败
- 分层神经符号规划（Hierarchical Neuro-Symbolic Planning）：高层 LLM 规划器分解复杂指令（如 “泡茶”→“取水壶→加水→加热→放茶包”），==中层模块生成参数化运动计划==（parameterized motion plans），低层扩散 / Transformer 控制器生成平滑轨迹（smooth trajectories）。
- An open foundation model for generalist humanoid robots
  1. 数据策略：
	  - **仿真轨迹：** 使用 **DexMimicGen** 在仿真环境（RoboCasa 等）中将少量人类演示通过重排和变换扩展为大规模数据。
	  - **神经轨迹 (Neural Trajectories)：** 利用微调后的视频生成模型（Image-to-Video），生成大量的“反事实”视频轨迹（例如改变物体颜色、位置），从而将有限的遥操作数据扩充了约 10 倍。
	  - [[无动作视频中学习（Latent Actions）]]
- For example, [[some studies use features from a middle layer (NVIDIA et al., 2025)]], [[the first-half layers (Shukor et al., 2025)]], or all intermediate-layer features (Black et al., 2025b).
- 3.3 Policy With Bridge Attention Pipe line undwestanding
  1. 总览。Policy network的输入包括四个变量：![[Finerly/多模态大模型/论文阅读/VLA/论文阅读/02_assets/VLA-ADAPTER-AN EFFECTIVE PARADIGM FOR TINY-SCALE VISION-LANGUAGE-ACTION MODEL/image.png|200x25]],
     以下从左到右解释：
     - 来自VLＭ的原始隐特征（更关注视觉语义信息，全局的细节的像素级别信息）。t表示策略网络和VLM的层级。
     - 来自VLM的额外查询向量（更关注动作生成信息相关的语义信息，例如打开一个抽屉，抽屉是木头的，ActionQuery关注抽屉,而不是"抽屉是木头的"）
	    *为什么说ActionQuery更关注Action generate，而Raw feature更加关注语义信息呢？*
	    这是两者在训练过程中的差异导致的。
	    - ActionQuery,计算预期和实际目标的差异,通过反向传播对ActionQuery的参数进行修正,于是ActionQuery的生成过程中只关注了于动作的差异,去除了与动作无关的图像信息
	    - Raw Feature,是基于骨干网络的VLM的权重对图片进行提取产生的，它什么都看，是全局性的．（涉及到冻结骨干网络往往更有效,具体参考:[[Prismatic VLMs-Investigating the Design Space of Visually-Conditioned Language Models]],本文的骨干网络也是基于这篇文章的结果）
     - Initial Aciton。Policy network的初始**动作向量序列**，**序列长度**为H（H为动作块中包含的步数），首先进行**升维**：经过LN和MLP升维。（维度与VLM的HideSize相同，文后指出为896）
     - 指机器人的感知状态，包括关节角度，第三人称视角，第一人称视角，三维坐标等（*This VLM follows the Prismatic-VLMs architecture (Karamcheti et al., 2024)）*。同样要进行升维操作：通过双层MLP转化为Feature Dimension（为了方便训练应该与VLA中的特征维度相匹配）。
 3. 文章中结构图如下
    ![[VLA-ADAPTER-AN EFFECTIVE PARADIGM FOR TINY-SCALE VISION-LANGUAGE-ACTION MODEL－2026-01-09－10-21-08.png]]
    1. 右边Brige Attention.
		第i层的Action Latent输出作为第i+1层的输入,==作为Q去询问Cross Attention和Self Attention==
		1. [1]表明Q = Q' + f(Q')
	    2. [5]ActionQuery,在训练过程中提取动作信息,用于指导The Policy中动作的生成.在图中[3]->[5]过程中,ActionQuery与P(机器对状态的感知)结合,作为K和V
	    3. [4]Raw Features,在训练过程中提取细节,像素级信息(具体坐标),通过==自学习的参数g==调控Raw Features注入程度.在[4]作为KV
	    4. [6]Action Latent作为QKV,去学习全局的动作信息
	2. Cross Attention 和Self Attention作用分别是什么?
		- Cross Attention Q为Action Latent,K和V为Raw Features,==信息融合==
			这表明Cross Attention是为了学习与动作相关的视觉信息,"这个物体在哪里"
		- Self Attention Q,K,V都是自己,==动作一致性==
			动作自己去查询自己,处理动作序列内部的时间依赖关系,保证动作轨迹平滑
		- 在标准的 Transformer Decoder 中，通常是先做 Self Attention 再做 Cross Attention。但在 VLA-Adapter 的图 中，你会发现它们是**并行 (Parallel)** 处理然后 **拼接 (Concat)** 的
******************
## Research Qbjectives
> 作者的研究目标是什么？  
> 	作者希望用更小的计算成本,来缩小视觉语言空间到动作空间的鸿沟,尽可能提高成功率，推理效率，训练效率.
> 关于这个问题，其它学者提出了哪几类的解决方案，有何缺陷?
> 1. ActionQuery,use or unuse ,Last Layer ActionQuery or to use all layer Query,the auther conducted a ablation experiment.
> 2. Raw Feature使用那一层,或者使用全层
> 3. 对于提高推理效率,OpenVLA-OFT使用了FiLM嵌入每一个Block的用法,对于实时性要求高,特定任务的推理比较适合.
> 4. primismatic VLA使用两种编码器提取视觉信息,一个擅长语义理解,一个擅长图像理解

## Background
> 作者需要解决的问题是什么？
> 	问题基调:
> 	当前的VLA模型对于VL to A 的桥接通常是对VLM进行**基于机器人数据的微调**，这种方法需要大量的硬件资源，时间成本，且推理效率低，很难满足实时性要求以及边缘性部署
> 	- 大模型的训练和部署门槛高
> 	- 特征的利用不充分(基于基准问题发现的细分问题)
> 		- 单一层级
> 		- 单一特征类型
> 	－Raw Features和ActionQuery融合产生语义偏见
> 作者围绕该问题，是如何构建解决思路的？
## Method(s)
作者解决问题的方法/算法是什么？是否基于前人的方法？基于了哪些？
	1. condition的选择.作者使用全层的ActionQuery和Raw Feature最合适,并且通过一个可学习的参数g来调控Rew Features的注入程度(通过实验比较了ActionQuery和Raw Feature之间的性能)
	   在后面的消融实验中,比较了last layer ActionQuery和全层ActionQuery以及ActionQuery数量对性能性能,最后选择了全层ActionQuery以及64size的ActionQuery.
	==基于前人的方法:==
		1. OpenVLA-OFT:**Action Query**和FiLM并行生成tion chunk,提高了推理效率.借鉴了ActionQuery的用法.
		2. Primismatic VLA:使用了Primismatic的框架.
	2. 创新了Brige Attention模块.使用两次Cross attention和一次Self attention提取视觉信息,平滑动作生成.
		基于Transformer
	3. 创新使用可学习的阈值来调节原始特征的注入程度
	
## Evaluation
> *作者如何评估自己的方法?*
> 	1. "仿真实验为主,真机实验为辅助"，主要关注了三个指标:success rate , inference speed , model parament number.
> 	2. 作者将可以复现的模型，分为大型，中型，小型模型，执行各项任务，泛化任务进行比较．
> 	3. 进行消融实验对比三个组成部分对性能的影响（The number of ActionQuery,conditions,the inject degree of Raw Features）
> 
> *实验的setup是什么样的？*  
> 	[[#RelateQuestion]]
> 
> 感兴趣实验数据和结果有哪些？  
> 	1. 小参数量达到大参数量模型的性能
> 	2. 训练速度快,推理速度快,即使在freeze backbone情况下,性能依旧强劲.
> 	3. 验证了Raw Features 和 ActionQuery之间的互补性
> 	4. 在VLM->Policy中,全层连接的性能由于最后一层连接的性能
> #spirate 
> 在实验设计上有没有问题或者可以借鉴的地方？  
> 	- 可以借鉴的地方:
> 		1. 对于阈值g的使用,动态控制Raw features的注入程度
> 		2. 全层特则会给你融合
> 	- 潜在问题:
> 		1. 真机实验有限
> 从结果（含曲线)上看，作者是如何有力地证明他解决了问题?

## Conclusion
作者给出了哪些结论？哪些是strong conclusions, 哪些又是weak的conclusions（即作者并没有通过实验提供evidence，只在discussion中提到；或实验的数据并没有给出充分的evidence）?
	1. strong conclusion:
		1. 本桥接范式是有效的:本文提出的桥接范式仿真基准测试中，多项任务能达到large-scale的模型性能
		2. 多层特征比单层有用
		3. 参数高效微调可行性
		4. 混合特征最优
	2. weak conclusion
		1. 目前仅进行了仿真实验,针对LIBERO-Long数据集,但是对于**长时序任务**的有效性仍未得到验证
## Structure
> 本文的结构怎样？对你写文章有什么参考作用？
> ![[Finerly/多模态大模型/论文阅读/VLA/论文阅读/02_assets/VLA-ADAPTER-AN EFFECTIVE PARADIGM FOR TINY-SCALE VISION-LANGUAGE-ACTION MODEL/image-1.png]]
> 这篇文章聚焦重点,一些非重点摒弃不讲.
> 
> 在Abstruct和Introduction中,例如Conditions,brige Paradigms这些并没有解释其概念,目的是:
> 	1.Introduction的关键是WHY,而不是How,具体概念的解释应该在Mothods中提出
> 	2.遵循"抽象阶梯策略",前到后逐渐解释
> 	3.设置悬念,暗示读者这个概念会在后面进行解释
> 	==这个观点还是要问一问师兄师姐是否合理==
## 启发
#spirate 
> 1. 有没有问题或者可以借鉴的地方？  
> 	- 可以借鉴的地方:
> 		1. 对于阈值g的使用,动态控制Raw features的注入程度
> 		2. 全层特则会给你融合
> 2. 缺陷在哪？能否提出一个解决思路？
> 	- 潜在问题: 
> 	1. 真机实验有限
> 	2. 在长时序任务的性能验证不够充分
> 	- 作者提出的后续发展的方向:
> 	1. 在真实世界的性能验证
> 	2. 动作生成的质量依赖于VLM提供的conditions的质量,需要进一步开发conditions的质量(往VLM的方向发展)
> 	3. 设计更加丰富的训练过程（从结构的角度）
>
> 3. 尝试一下，在你所阅读的论文与现实生活之间建立物理与逻辑联系，找出一个可以拓展的点?
>	远程医疗机器人
>	针对**远程**这个通点,需要解决网络延迟和手抖的问题,而ActionQuery提取语义,RawFeatures负责提取具体细节,这样能够针对目标生成平滑的动作,避免两个问题的侵害.而操作者,只需要提供指令,具体的动作由机器人执行/.> 4. 对于作者提出的解决方法，你有何看法和建议

## 对我有什么用

1. 回答了我为什么要读的问题了吗？
2. 同意/反对
3. 准备正面/负面的引用吗？
4. 准备深度的讨论吗？

## 改进思路

## 全文翻译

## Reference

> (optional) 列出相关性高的文献，以便之后可以继续track下去。























![[Pasted image 20250918151051.png]]

![[Pasted image 20250918151107.png]]