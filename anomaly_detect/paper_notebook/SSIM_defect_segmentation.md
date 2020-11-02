# Improving Unsupervised Defect Segmentation by Applying Structural Similarity To Autoencoders

# Abstact
	传统的方法是基于raw image与recon image 每个像素的重构误差，距离测量大多用欧
	式距离L2，这会造成：
	1.重建包括边缘的轻微定位不准确，都会得到较大的重构误差；
	2.当亮度值基本保持不变，他也没法显示视觉上已经被改变的缺陷区域。
	3.无法检测结构输入和重构不同的结构。
	
	proposed:
	提出了一个基于结构相似的感知损失函数，它可以检查局部图像区域之间的相互依赖性，考虑亮度、对比度和结构信息。
	在纳米纤维和两种机织物数据上性能提高了20个百分点。
# 1.Introduction
	1.监督学习方法，需要大量带有注释和数据，但在真是世界中，获取大量带有注释的数
	据非常难。
	2.以前的测量方法是基于每对像素(raw and recon image)的的欧式距离来进行测量，但这种只要在位置上有轻微误差，在就会得到较大的重构误差；并且还不能检测输入与输出结构的差异性；
	3.提出采用SSIM.它是一种距离度量，能够捕捉对边缘对齐不太敏感的相似性，并重视输入和重构之间的显著差异。能够捕捉局部像素区域之间的相互依赖性。

# 2.Related work
	1.异常定义：
		1.在分类场景中，出现和已知目标类别完全不同的sample,被视为异常；
		2.变现为对已知结构或者数据的细微偏差。
	2.各种思路：
		1.训练时候，对学习到的特征进行聚类；测试时候，那些违背聚类中心的点被视为
		anomalous 结构.
		2.

# 3.Methodology
## 1.Autoencoder for unsupervised defect segmentation
	input image: x 维度：K*H*W
	laten dimension:d
	为了防止对输入图片的信息简单复制，d需要满足条件：d << K*H*W.
	x_hat = D(E(x)) = D(z)
	loss function(per-pixel error measure):
	L2(x, x_hat) = ||x -x_hat||2

## 2.Variational Autoencoder
	对latent space施加限制，假设其服从某一分布(本文单位高斯), z~P(z), 输入一张图片，经过encoder后会得到Q(z|x)
	1.计算KL(Q(z|x) || P(z)),如果与已知的分布P(z)差距太大, 则判定anomaly.
	2.从后验分布Q(z|x)中采用N个latent样本，并且计算他们的重构概率。
## 3.Feature matching Autoencoder
	L(x, x_hat) = L2(x, x_hat) + a||F(x) - F(x_hat)||2.

	
## 4.SSIM
	vae和特征匹配自动编码器，对本文所采用的两个数据，性能都没有太大的提升。
	考虑了luminous, contrast, and structure
	
# experiment
	1.train dataset:
		image size:128*128
		number:10000
		data processing:randomly crop.为了得到更多的全局信息，将图像下采样到
		256*256，然后在进行剪切.
	2.test
		采样步长为128， 但是由于同一数据在不同位置会产生不同重构，因此，将stride降低为30，平均重构的像素值。对得到的residual map进行阈值处理，来获得defect的候选区域。