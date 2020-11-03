# paper link:https://openaccess.thecvf.com/content_CVPR_2019/papers/Bergmann_MVTec_AD_--_A_Comprehensive_Real-World_Dataset_for_Unsupervised_Anomaly_CVPR_2019_paper.pdf


# 主要内容
本文提出了一个异常检测数据集合：

total images:5354

normal image:3629, used to train

abnormal image:1725, used to test, containing defect and defect-free

categories: 5 textures, 10 objects

number of defect type: 73.

image resolutions:in the range between 700*700 and 1024 * 1024 
# data augmentation

texture:随机旋转裁剪固定大小的旋转矩形块．

object:随机平移和旋转．

还增加了mirroring.

