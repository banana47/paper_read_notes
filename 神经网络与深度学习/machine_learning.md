# 机器学习

##1.机器学习就是让计算机从数据进行自动学习，得到某种知识或规律．
##2.机器学习三要素：
1.模型：线性模型/非线性模型;
2.学习准则：也是损失函数
	(1) 风险最小化准则：也就是让损失函数最小

	(2) 过拟合：
		描述：模型在训练时候很好，但在test stage错误率较高；
		原因：less training dataset;噪声；模型能力强．
		解决方案：
			(1)noamalization; (2)dropout;(3)data augmentation
	(3) 欠拟合：
		描述：模型在train stage不能很好的拟合，test stage也不能很好的拟合；
		原因：一般是模型能力不足造成的．

3.优化算法
	(1)在机器学习中，优化可以分为参数优化和超参数优化．
		参数优化：可以通过优化算法学习的　模型的参数；
		超参数优化：定义模型结构或优化策略参数．超参数的选取一般都是组合优化的问题．

		1)梯度下降法
		2)提前停止：如果模型在验证集上的错误率不再下降，就停止迭代．
		3)随机梯度下降法
			1.批量梯度下降法(BGD)：目标函数是整个训练集上的风险函数．每次迭代需要计算每个样本上损失函数的梯度并求和．
				缺点：当数据量N很大时，每次迭代的计算开销也很大．
			2.为了减少每次迭代的计算复杂度，在每次迭代时候只采集一个样本，计算这个样本损失函数的梯度并更新参数．
				缺点：无法充分利用计算机的并行计算能力．因此提出了小批量梯度下降法．



	

