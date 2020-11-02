# 一篇博客　https://blog.csdn.net/sinat_37145472/article/details/98869328
# github:https://github.com/donggong1/memae-anomaly-detection

# Memorizing Normality to Detect Anomaly(ICCV 2019)
## Abstract
	认为model仅仅在正常样本上训练，学到的完全是正常样本的分布．model训练完成后，如果输入的为abnormaly样本，则对异常样本的重构能力较差．重构误差大于一个值，则表示这个样本为异常．由于一些情况下，autoencoder训练的非常好，对异常样本的重构也较好，重构误差低，难以判定为异常．因此本文中作者提出了利用注意力机制的模型来扩展该模型．
## 主要贡献
	提出在autoencoder中加入注意力机制．
## 整体思路
	先利用正常样本训练，memory中会保存正常样本的信息(normal pattern), 训练时候memory会随着encoder和decoder更新．在test, memory固定，通过比较输入的query(也就是z), 计算momory中的各项权重信息，将得到的组合信息输入给decoder进行解码，来完成对异常图像的重构．

	上述的处理过程实际上是使用了基于注意力机制的记忆处理（attention based memory addressing）。同时作者还提出了一种可微的难压缩操作（hard shrinkage operator） ，使得记忆处理权重有一定的稀疏性，从而记忆项接近特征空间中的query。
	测试阶段：所学习的记忆内容是固定的，通过使用少量的正常记忆项（normal memory items）来进行重建，这些正常记忆项被选择为输入编码的邻域。由于重建是在内存中获得的正常模式，所以它往往接近于正常数据。

## memory process
###1.memory representatino
	用一个大型矩阵M,维度为(NXC).其中N表示the maximum capacity of memory, C:表示latent dimension.
###2.attention for memory addressing(也就是对memory中的N个维度向量分别计算权重)
	计算公式请参考论文
###3.Hard Shrinkage for Sparse Addressing
	由于一些异常有可能通过(memory items)的复杂组合(cobination)很好的重建．为了缓解这个问题，作者采用了hard shrinkage operator来提高w ww的稀疏性。进行了类似Relu的Hard Shrinkage约束，其实就是超过阈值才保留，不超过阈值就是0。



## experiment
	dataset:
	1)MNIST:0.9751
	2)CIFAR-10:0.6088.相较于skip-gan的0.73(自己跑的效果为0.67)，这个性能并不好．

### 疑问点：
对memoery的更新这一部分没看懂．


	


