### 1.Summary
这是一篇基于控制变量法来对VLM的影响因素进行分析的文章，提出了一些反直觉的发现，以及一个视觉表示上的融合创新．
### 2.Key Novelty
1. 视觉表示的融合创新:将擅长语义理解模型（CLIP）的特征与擅长低层次空间理解模型(DINOv2)的特征进行融合,可以显著提升模型在**定位**和**推理**上的能力.
2. 反直觉的发现:
	1. 单阶段的训练优于多阶段训练
	2. 冻结视觉骨干网络优于全量微调
	3. 暴力调整尺寸效果还行
	4. 基础模型表现强劲:相比于经过指令微调的语言模型,基础模型Llama-2在VLM任务中表现良好
### 3.Key Results

### 4.Thoughts
在[[#2.Key Novelty]]中,本文并没有使用FiLM(Feature-wise linear Modulation特征级线性调试)（[[Fine-Tuning Vision-Language-Action Models—Optimizing Speed and Success]]）,而使用了两种不同的VLM去理解图像,这两种方法有各有其优势.
究其原因,是本文是基于**提高准确性**,而[[Fine-Tuning Vision-Language-Action Models—Optimizing Speed and Success]]这篇文章基于环节长时间序列操作的延迟问题,是为了**提高效率**.

由此简单说明两者的区别.
准确性:Transformer > FiLM
运行速度:FiLM > Transformer
Tranformer中,指令在每一层都与视觉latent进行交互,比FiLM融合更加深入彻底.时间复杂度是O(N^2)
FilM只对指令进行简单的按元素进行乘法和加法.时间复杂度是O(N).FilM像是戴了名为指令的偏折镜片,只能看到和指令相关的;而transformer能看到全局的信息
因此,在一些强调效率,实时性,指令相关性很强的特定任务的条件下,适用于FiLM（因此，在[[Fine-Tuning Vision-Language-Action Models—Optimizing Speed and Success]]中,结合FiLM和双向交叉注意力机制降低延迟）