### 1.Summary
这是一篇介绍了双系统架构的VLA论文，缓解了（并没解决）推理延迟的问题．用类似于消费者生产者的思路，使用System2和System1两个系统Asynchronous Parallelism with Buffering(带内存的异步并行推理),或者说是异步内存流水线工作.
HiRT的延迟大幅减小
### 2.Key Novelty
1. 双系统架构:慢速脑System2和快速脑System1
	- System2
		- 输入:语言指令和单张第三人称图片(用于提取**全局视觉信息**)
		- 输出:携带语言指令和视觉信息的Latent
		- 特点:使用的VLM模型参数量大(提取了**细节全局**的视觉信息),运行**缓慢**
	- System1
		- 输入:System2输出的Latent和**实时**图片序列
		- 输出:Action Control
		- 特点:模型参数量小(只提取基本框架信息),运行**快**
	- System1和System2通过内存**异步连接,并行工作**
2. 细节的创新
	1. Asynchronous Sampling（异步采样）,在训练时,随机选取过去的时间步给S2,使得System1学习指令和图像的延滞性
	2. 对于System1![[HiRT-Enhancing Robotic Control with Hierarchical Robot Transformers－2026-01-08－11-29-12.png]]
### 3.Key Results
**理想情况 (HiRT-AS, Interval 1)：** 如果 VLM 每一步都能实时更新（没有延迟），成功率是 **96.7%**
**真实延迟模拟 (HiRT, Interval 6)：** 当引入 6 步的延迟（即便用了异步采样训练），成功率下降到了 **63.4%**
**不使用**的话成功率只有 52.2%
### 4.Thoughts
只能说是缓解,在考虑到延迟时,从上面可以看到提升了10%
主要是状态延迟和指令延迟的问题.如果说状态更新太快,比如说红绿灯变化,那么在执行到S1时刻,当前状态可能已经不是S2时刻的图像,那么指令和环境是不匹配的.
指令延迟,指令更新太快
#spirate
能不能使用[[VLA-ADAPTER AN EFFECTIVE PARADIGM FOR TINY-SCALE VISION-LANGUAGE-ACTION MODEL]]中用自动选择**最好的**条件进行注入?

