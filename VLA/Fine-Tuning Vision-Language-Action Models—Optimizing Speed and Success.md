### 1.Summary
这是一篇基于OpenVLA的微调策略文章.文章使用了并行解码和动作分块，创新了FiLM层将语言指令嵌入到视觉编码器中,提高特征的对指令的专注性,得益于并行解码的性能冗余,OpenVLA-OFT(Optimized Fine-Tuning)能够处理更高维度的数据并且提高了响应能力,最终其性能大幅超越OPenVLA和Π0.
### 2.Key Novelty
1. 并行解码和动作分块
	1. 并行解码是基于**双向注意力机制Bidirectional Attention**,区别于自回归的逐一生成token的思路,Bidirectional Attention从正反两个方向执行注意力机制,每一个token的生成不依赖前面或者后面的token,从而**提高了推理速度**,能够一次生成一个动作块.==允许模型一次性利用所有上下文信息==
	2. **有利于多模态融合**:有更多的推理余量用于处理更多维度的信息
	3. Ａction Chunk动作分块,在自回归机制Autoregressive中是不可能实现的,因为Autoregressive具有因果关系,延迟会在每一个token按序生成的过程中累计,最终导致动作分块失败
	4. PS:理论上,Bidirectional Attention的性能不如Cause Self-attention，因为Bidirectional Attention不是严谨的基于上一步推理得来的.
ps: Autoregressive 是时间上的生成顺序，Masked Self-Attention 负责在这个顺序中维护自身的一致性，而 Cross-Attention 负责在这个顺序中引入外部信息的指导
2. 创新了**FiLM**的使用方法,破除复杂图像丢失对指令的聚焦问题(Language Grounding失效).
	1. 双骨干全层嵌入
		![[Fine-Tuning Vision-Language-Action Models——Optimizing Speed and Success－2026-01-06－20-06-36.png]]
		在**每一个**ViT（视觉解码器）中增加一个FiLM层,注入从文字指令提取的向量,确保视觉特征向量聚焦于指令,**使得每一个Block**都能独立学习图像的特征
		==简述指令注入ViT的流程:==
		1. 从任务描述中获取词向量(Open-VLA基于Llama-2,所以是Lama-2的词嵌入向量)
		2. 获取句向量,对词向量去平均值,得到单一句子的平均向量
		3. ==FiLM的核心定义：==
		   通过Simple Affine Transformations（仿射变换），即![[Fine-Tuning Vision-Language-Action Models—Optimizing Speed and Success－2026-01-08－20-45-31.png]]
			参数介绍：
			W：缩放系数，由神经网络基于向量X生成的缩放“向量”
			b：平移系数，由神经网络基于向量X生成的平移“向量”
			这篇文章对于FiLM的使用进行了初始参数优化，即第三点无损初始化
			（Linear Layer）线性层输出仿射变换的参数，**注意每一个ViT块都有自己的仿射参数,即独立的FiML注入**
			==FiLM的发挥作用的位置：==
			对于全图的所有Feature Channel独立进行调制，可以进行这样的理解，不同的Feature Channel侧重了图像不同的特征，for excepler, channel 1 代表“金属”，channel 2代表了“苹果”，根据**目的**对于**苹果**的Feature进行放大
			==FiLM从数字维度的角度解释：==
			对于不同的指令输入z,神经网络生成对应的向量W，缩放系数W的每一个维度是对于一个特征的处理
			同理：平移系数β的每一个维度是对于一个特征的处理
		4. FiLM调制,即用第三步获得的两个参数,对FiLM自身进行一次仿射变换，再注入MLP层（前馈网络）进行训练（**注意图上的位置就行**）
	2. 空间无关调制（即指令向量和图像分成的Patch进行融合）
	   PS:这个思路也是从文章上**迁移**过来的,并不是独创的一个思路.多看文章还是有利于当一个拼接侠.
		在图像分为Patch后，使用同一组参数对多个Patch统一调制,使得语义调制在全局方面生效
	3. 无损初始化
		通过设置(1+Y)降低损失,因为Y的初始值常常为0,导致等式两边相等．
### 3.Key Results
性能超过OpenVLA　20%
### 4.Thoughts
可以借鉴FiLM层的思路对transformer的结构进行改进,对于要聚焦的东西进行融合.

其次是Bidirectional Attention

在并行解码和动作分块实现中，用**Empty Additional Quries**,这个方法在[[VLA-ADAPTER AN EFFECTIVE PARADIGM FOR TINY-SCALE VISION-LANGUAGE-ACTION MODEL]]论文中被借鉴.
具体来说,就是在生成动作的时候,在输入序列的末尾插入K个空嵌入(对应预测未来的K步),每个空嵌入的维度是1* Dmodel(模型处理向量的宽度),那么K个空嵌入的维度是K* Dmodel.经过双向注意力机制后,前提到的[[#2.Key Novelty]]中一次性看清全局信息,借由空嵌入实现.
输出的**空嵌入向量**融合了全局的视觉,语义,动作信息,,每一个空嵌入向量经过MLP层(**用于压缩**)投影维动作,最后输出是K* Daction
![[Fine-Tuning Vision-Language-Action Models—Optimizing Speed and Success－2026-01-08－17-07-56.png]]