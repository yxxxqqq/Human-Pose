#### Camera Distance-aware Top-down Approach for 3D Multi-person Pose Estimation from a Single RGB Image, ICCV2019

##### 模型结构有三部分

- DetectNet: 人体检测网络，使用Mask RCNN
- RootNet: 裁剪bounding box，3D人体的根网络，估计出以相机中心为坐标的人体根位置。首先backbone使用ResNet捕获全局特征；然后特征矩阵上采样，使用的是三个连续的解卷积层，BN层和ReLU激活函数，1-by-1的卷积得到基于根的2D heatmap，softmax根据heatmap得到坐标位置；最后，估计深度值，使用上述类似的步骤，但是输出是一个标量值，标量倒数乘以k得到最终的绝对深度值。使用L1 loss训练RootNet。
- PoseNet: 基于根位置估计出完整的单人姿态，其中x,y是关节的像素位置，z是相对相机坐标系的深度值，通过一些变换以及反向传播将相对深度转换到绝对深度，也就是真实输出。模型设计和RootNet的结构一样。ResNet + upsamples(3 deconvolutional layers, batch normalization layers, ReLU activation function) + 1-by-1 convolution+ softmax. 使用L1 loss训练PoseNet。



##### 贡献

- 自上而下的方法估计多人3D姿态，单RGB图像
- 甚至在推理阶段没有真值信息，算法性能也超越了之前有真值的3D单人姿态估计的效果
- 模型输出绝对相机中心坐标系下的多人3D姿态的位置
- 容易将单人的3D姿态估计推广到多人的3D姿态估计



##### 具体实现

- 通过矩形框面积与图像的比例关系获得人体于相机的深度关系，但是一些情况中，尽管与相机的距离相同，但是矩形框的面积大小不同；近处的小孩和远处的大人，有相同的矩形框大小，但是于相机的距离不同。解决这一个问题，文中使用了RootNet，使用了图像面积，图像特征能够提供图像面积的变化。
- Mask R-CNN pre-trained on COCO dataset, backbone use ResNet-50 pre-trained on ImageNet dataset, weights initialized by Gaussian distribution, Adam optimizer



##### datasets

- Human3.6M
- 训练策略：训练Human3.6M时加上MPII 2D





##### 相关阅读

- D. Mehta, O. Sotnychenko, F. Mueller, W. Xu, S. Sridhar, G. Pons-Moll, and C. Theobalt. Single-shot multi-person 3d pose estimation from monocular rgb. In 3DV, 2018.  数据集
- G. Rogez, P. Weinzaepfel, and C. Schmid. Lcr-net: Localization-classification-regression for human pose. In CVPR, 2017.  
- X. Sun, B. Xiao, F. Wei, S. Liang, and Y. Wei. Integral human pose regression. In ECCV, 2018.  文中的PoseNet原型。
- C. Ionescu, D. Papava, V. Olaru, and C. Sminchisescu. Hu- man3.6m: Large scale datasets and predictive methods for 3d human sensing in natural environments. TPAMI, 2014. 数据集
- D. Mehta, H. Rhodin, D. Casas, P. Fua, O. Sotnychenko, W. Xu, and C. Theobalt. Monocular 3d human pose esti- mation in the wild using improved cnn supervision. In 3DV, 2017. 数据集
- X. Sun, B. Xiao, F. Wei, S. Liang, and Y. Wei. Integral hu- man pose regression. In ECCV, 2018. 
- T.-Y. Lin, M. Maire, S. Belongie, J. Hays, P. Perona, D. Ra- manan, P. Doll´ar, and C. L. Zitnick. Microsoft coco: Com- mon objects in context. In ECCV, 2014. 数据集

