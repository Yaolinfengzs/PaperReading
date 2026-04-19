![[Finerly/多模态大模型/论文阅读/VLA/论文阅读/02_assets/VLA-ADAPTER-AN EFFECTIVE PARADIGM FOR TINY-SCALE VISION-LANGUAGE-ACTION MODEL/image-1.png]]
**名词解释**：
	1.
**文章的创新思路**：
1. 用作者训练的VQ-VAE对Action sequence压缩成低维的离散化表示，即Codebook Indices,就是所说的Action Tokens。优势在于，捕捉了OpenVLA原本离散化表示无法收集的**时间和空间上的连续性**
2. 将Actions Tokens作为OpenVlA的输入（OpenVLA对于动作的原输入处理：它将连续的机器人动作（如手臂移动的距离、角度）的每一个维度直接划分成 256 个离散的区间， 动作被转换成一系列 0 到 255 之间的整数。模型每次只能预测一个时间步的动作）。
   此时Actions Tokens代替0-255的离散数值
**VQ-VAE的创新点**：
	1. 数据集的扩展：远远超过其他训练器的数据量
	2. 合成数据的应用：发现并验证了模拟合成数据与真实数据的领域差距极小，可以作为高质量数据用于模型的训练