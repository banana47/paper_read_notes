#Skip-GANomaly:Skip Connected and Adversarially Trained Encoder-Decoder Anomaly Detection

##Abstrsct
	1.真实世界中，异常检测最大的挑战是极度的数据不平衡(正常样本多，异常样本少)，因此极大的限制了监督学习方法的应用．
	2.本论文提出采用带有跨层连接的编解码卷积神经网络(encoder-decoder convolutional neural network with skip connecions)在高维图像空间中捕获尽量多的正常数据分布分多尺度分布．
	3.在该框架中，使用对抗训练，可以在高维的图像空间和低纬的隐向量空间中对原图进行较好的重构．
	4.训练过程中最小化图像空间和隐空间的重构误差，可让该模型更好的学习正常样本的分布．因为仅在正常的样本上进行训练，认为模型对正常的样本有较好的重构能力，对异常的样本重构能力较差，因此大的重构误差则表示该样本是异常．

##Introduction
 	公认的假设是异常样本和正常样本在高纬和低维的图像空间中都有所差异，因此认为把image映射到低维空间是必须的，但关键的问题是学习本的分布是有困难的，直到GAN的出现该问题才得以解决．
	主要贡献：
	1.unsupervised anomaly detection:提出了一个跨层连接的编解码卷积神经网络架构，在image space和latent space都可以较好的重构；
	2.efficacy:在当前的方法中，achieve quantitatively and qualitatively superior performance
	3.reproducibility:可轻易复制．

##proposed method
A.background
	1.首先介绍了GAN, 后面又提出encoder-decoder network和Discriminator一起训练会导致训练不稳定问题，以前文献中有两种解决方法：一种是在整个网络中加入fully convolutional layer和Batch Normalization;另一种是在训练过程中使用Ｗassertein loss.
	2.AAE.AAE包含一个generator和一个discriminator, gnerator作用是编解码，discriminator作用是判断输入的latent space 是来自auto-encoder还是预先初始化的任意向量．
	3.Inference within GAN
	认为输入的noise vector和generator network的输出有很大关系，相似的latent space会产生高质量的输出．
1)一种方法是通过gardients, 将image逆映射回hidden space，来找到最优的latent vector.
2)另一种方式是通过添加一个encoder, 通过down-smapling, 将image下采样到latent sapce.
3)第三种是先将image下采样到latent space, 再从latent space返回image.
本论文的目的是利用latent vector representaion来探索inference within GAN, 从而得到正常样本的表示．

B.proposed approach
1)问题定义：
	输入的数据分成两部分：train dataset和test dataset.在train dataset中，只有正常样本，label=0;在test dataset中，包含正常和异常样本，normal_label=0;abnormal_label=1．
	训练时候，可以捕获image和latent space中数据的分布．

2)pipelien:两个网络，一个generator,一个discriminator.
	encoder:将image转换到latent space(有五个卷积层，五个BN,和LeakyReLu激活函数)
	decoder:将latent vector Z映射会image　space，编码网络中的每一个下采样层和decoder中的每一个上采样层相对应．skip connection的优点：通过在层间直接的信息转换，保留局部和全局信息，因而有更好的重构能力．
	判别器D:不仅作为分类器，还作为特征提取器．对输入的原始图片和重构图片抽取特征，在latent space进行推理，这是另一个新的部分．

C.training Objective
	假设：模型仅在正常样本上训练，不能重构异常样本，因此遇到异常样本，则有较大的重构误差．设计了三个损失函数．x:original image, x_hat:reconstruct image.
	(1)adversarial loss:该目的是min(G), max(D)
		L(adu)= E[log(D(x))] + E[log(1 - D(x_hat))]
 	(2)contextual loss:对抗损失只能让模型生成真实的图片，但不能保证该模型学习关于输入的纹理信息．为了更好的学习input image的纹理信息来更好的捕获normal image的data distribution,使用L1 normalization.
		L(con) = E|x - X_hat|1
	(3)latent loss:有了上面的对抗损失和置信度损失，该模型能够生成真实的且纹理相似的图片．为了让重构图片x_hat的latent representation 和input image X的latent representation尽可能的相近，加入隐变量损失．
		L(lat) = E|f(x) - f(x_hat)|2
	total loss: L = weight_a * L(adu) + weight_b * L(con) + weight_c * L(lat)
	weight_i: 分别代表各个损失函数的权重.

D.Inference
	A(x) = a*L(con)(x_hat) + (1 - a) * L(lat)(x_hat)(没有对抗的损失)

## experiment
1.dataset:
	CIFAR-10
	University Baggage Dataset
	Full Firearm vs Operationnal Benign


	
	
	
	
	


























	


