![[CoT-VLA_Visual Chain-of-Thought Reasoning for Vision-Language-Action Models－2026-03-18－22-37-11.png]]
传统VLA和COT-VLA之间的区别：
- 数据来源：
  1. vanilla-VLA使用标注后的机器人数据
  2. COT-VLA可以使用海量的未标注数据，且可以使用人类动作数据
- 流程：
  1. vanilla是黑盒操作，输入obs image and text 输出action
  2. COT-VLA先生成goal image，依据goal image token和text生成action

COT-VLA的核心训练过程分为两步：视觉预测使用==因果注意力机制==，动作生成使用==全连接注意力==
![[CoT-VLA_Visual Chain-of-Thought Reasoning for Vision-Language-Action Models－2026-03-18－23-07-50.png]]
- 视觉预测：机器人示教数据集Dr​与无动作标注视频数据集Dv​。观测图像+语言指令--->子目标图像tokens
- 动作生成：仅在机器人示教数据集Dr​上训练。观测图像+语言指令+子目标图像token--->动作块

实验设置：
![[CoT-VLA_Visual Chain-of-Thought Reasoning for Vision-Language-Action Models－2026-03-19－00-17-18.png]]
### 1.Summary
COT-VLA将思维链加入VLA过程中，致力于将图像+语言->动作的黑盒过程，通过生成子目标图像的方式显式表现出来。
优点是可以使用海量的未标注数据，在后续的实验中具有更好的泛化性能。
缺点是需要更多的计算消耗，动作生成慢，无法满足高帧率运动。
它走了另外一条路，目标是将VLA动作生成过程透明化，实际部署很难。
### 2.Key Novelty
- 将COT引入传统的VLA架构。两步走，先生成子目标图像，依据观测图像＋指令＋子目标图像生成动作序列
- 混合注意力机制，在视觉预测过程中使用因果注意力，在动作生成中，使用全注意力
- 评估了单纯VLA，混合注意力机制，COT机制的效果
### 3.Key Results
在仿真环境与真实世界中开展了全面的评估实验，证明视觉思维链推理能够提升 VLA 模型的性能，且我们的系统在多个机器人平台与任务中均取得了当前最优的性能表现

限制：
- 自回归图像生成的质量不如Diffusion model
- 延迟高
- 计算量大
### 4.Thoughts
与VLA-Adapter相比走另一条路径。VLA-Adapter放弃了显示画出未来，而选择隐式的提取未来
- 使用ActionQuery在提取指令信息和图像信息，不需要输出图片
- BrigeAttention和分层利用，提取了不同深度的信息

![[CoT-VLA_Visual Chain-of-Thought Reasoning for Vision-Language-Action Models－2026-03-19－00-38-03.png]]（这是一个好的处理方式吗？）