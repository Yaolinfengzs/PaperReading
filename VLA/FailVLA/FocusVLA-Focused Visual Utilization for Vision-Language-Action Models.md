paper link:https://arxiv.org/abs/2603.28740
哈工深；**Daimon Robotics（戴盟机器人）**；南京大学；中国人民大学
### 1.Summary
这是基于[[VLA-ADAPTER AN EFFECTIVE PARADIGM FOR TINY-SCALE VISION-LANGUAGE-ACTION MODEL]]工作的一篇论文。它延续了VLA-Adapter的基础架构，专注于”不是视觉表征质量，种类不够好（多），还有可能是没有充分利用视觉表征“（not reprensent but not utilization visual features）。基于这个核心问题，文章做了一组消融实验证明使用同一组数据不同的实验方法，包括pooling，1-gate，cascaded attention,证明了以下的发现：
- 仅仅使用pooling和1-gate就能提高baseline，说明没有充分使用visual，视觉patch没有聚焦，噪声大
- 使用cascaded attention，强行利用视觉特征，提升最大，说明VLA-adapter的police存在问题
作者通过”充分利用视觉特征“切入，保留VLM基座，修改policy，使得FocusVLA在训练侧和推理侧效率大幅提升，多个任务成功率提高，主要提升了机械臂对于细节的处理能力。
### 2.Key Novelty
- cascaded attention
	VLA-adapter的输出是动作，对于动作生成来说，最容易获取的信息是action query(基于反向传播Loss的理论)，所以模型天然的更加关注action query反而忽略视觉信息，这从gate容易趋向于0可以辅证。这导致VLA忽视视觉细节，对于高精度的任务能力不足。
	作者提出使用cascaded attention,设置feature的处理顺序，从而使VLA无法忽视视觉特征。
- Patch-level Focus：视觉特征的数量
	通过对==视觉的patch统计attention分数==，建立热力图:
	![[image.png|428|945x548]]
	发现VLA-adapter关注了背景，而机械臂需要关注局部细节，因此作者提出Top-K patch，只使用排名前256的图像patch作为视觉特征
- Channel-level Focus：视觉特征的质量
	图像的噪声通道很多，VLA-adapter只使用一个gate，无法动态关闭噪声通道，因此提出动态多gate机制，提高视觉特征的质量
-  方法创新![[image-3.png|814|814x483]]
### 3.Key Results
成功率：
![[image-1.png|563]]
==训练侧和推理侧==：
![[image-2.png|548]]
### 4.Thoughts
全文围绕”充分利用视觉特征“出发，对于policy架构进行改进，不仅提高了训练效率，还大幅提高了LIBERO各项任务的成功率
文章没有解决的问题：
- 长时序记忆
- 多阶段子目标切换
- 失败恢复
- 接触状态变化下的动态重聚焦
- VLM侧怎样生成更合适的视觉特征


##### 方向 1：时间维度更多
把当前的空间 focus 扩展成时空 focus：
- 不只选当前 patch
- 还选历史若干关键帧
- 甚至选“哪个时间段的哪个 patch 最关键”
这会更适合长时序操作、失败恢复、遮挡前后的状态跟


论文只定义了三大类 failure：

- translation
- rotation
- no-ops

这确实覆盖很多常见问题，但仍遗漏了大量更难的真实失败：

- 接触滑移
- 力控失败
- 夹持不稳
- 多物体相互干扰
- 遮挡导致目标切换错误
- 语义层面的子任务误判

