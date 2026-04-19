---
title: Efficient Vision-Language-Action Models for Embodied Manipulation- A Systematic Survey
authors: Weifan Guan
year: 2025-10-23
from: Institute of Automation, Chinese Academy of Sciences / University of Chinese Academy of Sciences
tags:
  - "#顶会"
descibe: Systematic Survey
date created: 2025-12-13T16:53:00
date modified: First 2025.12.13/
"paper link:": https://www.arxiv.org/abs/2510.17111
github: https://github.com/guanweifan/awesome-efficient-vla.git
---
阅读目的
- [ ] 文献综述、查资料  
- [x] 了解新领域  
- [ ] 训练提升学术能力  
- [ ] 借鉴文章具体方法  
- [ ] 回答下列问题：

作者的写作目的
- [x] 总结其他研究  
- [ ] 建议未来方向  
- [ ] 报告新发现  
- [ ] 发展理论/算法  
- [ ] 表达特别的观点

阅读状态: 
- [ ] 泛读
- [x] 略读
- [ ] 精读
- [ ] 深读  
************************
1. constraint 限制
2. backone 脊柱，骨干
3. sematic 语义的，语义学的
4. latent 潜在的
5. Token pruning 令牌修剪

动作块机制大幅提升了单次推理的动作令牌长度，在双臂任务中序列长度近乎翻倍，增加了自回归架构的负载。为解决该问题，FAST[71]在推理前对离散化动作序列进行压缩：先通过离散余弦变换（DCT）将序列转换至频域，保留主导低频系数，再经字节对编码（BPE）缩短令牌长度，既降低推理阶段开销，又能无损重建原始动作，实现显著计算节约。类似地，OmniSAT[72]提出统一动作令牌器，将连续轨迹转换为紧凑离散令牌：先通过B样条[73]时间对齐得到定长控制点表征，再经残差向量量化（RVQ）[74]离散为多层码本索引，并按位置、旋转、夹爪维度分组，最终生成短且语义结构化的令牌序列，在保留轨迹精度的同时，支持多构型机器人的高效自回归建模。

***********************************
[[Mamba SSM structure]]
## 文章总结

> 写完笔记之后最后填，概述文章的内容，以后查阅笔记的时候先看这一段。注：写文章summary切记需要通过自己的思考，用自己的语言描述。忌讳直接Ctrl + c原文。
### 主要创新点在哪里？

### Abstract
- VLA模型通过映射自然语言指令和视觉感知到机器人动作的方式，将视觉语言模型拓展到具身控制领域。尽管VLA的能力很强，但是VLA系统面领着巨大的挑战，因为他们需要大量的计算能力和存储需求，这些与边缘平台需要高实时性表现的限制相冲突，比如在线移动控制平台。解决这个冲突成为最近研究的热点。鉴于高效和可适VLA的进展，本研究提供了提高VLA效率的系统研究方法综述，这些高效VLA聚焦于减少延迟，存储优化，以及训练和推理开销。我们总结解决方法的4个维度，列出了代表性的技术列表：
	- model architecture模型架构
	- perception feature感知特征
	- action generation动作生成
	- training/inference strategies 训练和推理策略
- 最后，我们讨论了未来趋势和开放性的挑战，强调了提升具身智能效率的方向。文章涉及的论文位置在笔记属性中给出。
### Introduction
![[Finerly/多模态大模型/论文阅读/VLA/论文阅读/Inference Artical/image.png]]

******************
## Research Qbjectives

> 作者的研究目标是什么？  
> 关于这个问题，其它学者提出了哪几类的解决方案，有何缺陷?

## Background

> 作者需要解决的问题是什么？

## Method(s)
作者解决问题的方法/算法是什么？是否基于前人的方法？基于了哪些？
## Evaluation

> 作者如何评估自己的方法？  
> 实验的setup是什么样的？  
> 感兴趣实验数据和结果有哪些？  
> 有没有问题或者可以借鉴的地方？  
> 从结果（含曲线)上看，作者是如何有力地证明他解决了问题?

## Conclusion
作者给出了哪些结论？哪些是strong conclusions, 哪些又是weak的conclusions（即作者并没有通过实验提供evidence，只在discussion中提到；或实验的数据并没有给出充分的evidence）?
## Structure

> 本文的结构怎样？对你写文章有什么参考作用？

## 阅读笔记
1. 这篇文章解决了什么问题
2. 关于这个问题，其他学者提出了哪几类的解决方案，有何缺陷？
3. 作者围绕该问题，是如何构建解决思路的？
4. 从结果上看，作者是如何有力的证明他解决了问题？
5. 缺陷在哪？能否提出一个解决思路？
6. 尝试一下，在你所阅读的论文与现实生活之间建立物理与逻辑联系，找出一个可以拓展的点?
7. 对于作者提出的解决方法，你有何看法和建议
8. 你知道怎么撰写联系论文作者的邮件吗？先写下来，不过暂时不要发出去。

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