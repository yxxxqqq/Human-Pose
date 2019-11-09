#### Generalizing Monocular 3D Human Pose Estimation in the Wild





##### contribution

- 建立了一个40万张图像的3D室外数据集，单人数据集，320,000 training images and the rest for testing.
- 解决深度歧义的问题，设计了双目启发式3D标签生成器，利用多视角下的2D姿态生成3D姿态。
- 提出一种几何搜索方案，进一步优化3D姿态估计结果
- 训练了一个baseline network





##### Methodology

- 3D label generator，包含三个部分，stereoscopic view synthesis subnetwork，3D pose reconstruction subnetwork， geometric search scheme
- stereoscopic view synthesis subnetwork，给出左边视角的2D姿态，合成右边视角的2D姿态。难点是如何获得右边视角的gt，文中方法：先将相机坐标系中的3D位置稍微向右移动，同时保持y和z不变，然后重映射到2D位置。linear-ReLU layers, residual connections, batch normalization layers, dropout layers with max-norm constraint.
- 3D pose reconstruction subnetwork, 利用输入的2D左视角+合成的2D右视角(concatenation operation)，直接回归得到3D坐标。结构和上一个子网络相同，得到粗糙的相对盆骨的3D姿态
- geometric search scheme，预测的3D人体姿态通过相机内参矩阵可以以零像素误差重新映射到2D输入，使用L2损失。
- 使用unity toolbox生成部分图像





##### Details

- 训练： pytorch, Adam， SGD

- 合成图像由于和真实图像之间存在域误差，因此模型预测有稍微错误

- 对室外的估计效果提升明显

- 模型的泛化能力提升，因为使用了unity toolbox生成2D/3D的关键点对

- 

  

















##### References

- H. Rhodin, J. Sp¨orri, I. Katircioglu, V. Constantin, F. Meyer, E. M¨uller, M. Salzmann, and P. Fua. Learning monocu- lar 3d human pose estimation from multi-view images. In IEEE Conference on Computer Vision and Pattern Recognition, 2018. 
- G. Rogez and C. Schmid. Mocap-guided data augmentation for 3d pose estimation in the wild. In Neural Information Processing Systems, 2016.
- J. Wu, H. Zheng, B. Zhao, Y. Li, B. Yan, R. Liang, W. Wang, S. Zhou, G. Lin, Y. Fu, et al. Ai challenger: A large-scale dataset for going deeper in image understanding. arXiv, 2017.
- Y. Luo, J. Ren, M. Lin, J. Pang, W. Sun, H. Li, and L. Lin. Single view stereo matching. In IEEE Conference on Com- puter Vision and Pattern Recognition, 2018. 灵感来源
- X. Zhou, Q. Huang, X. Sun, X. Xue, and Y. Wei. Towards 3d human pose estimation in the wild: a weakly-supervised approach. In IEEE International Conference on Computer Vision, 2017. baseline 网络模型。
- D. Mehta, H. Rhodin, D. Casas, P. Fua, O. Sotnychenko, W. Xu, and C. Theobalt. Monocular 3d human pose esti- mation in the wild using improved cnn supervision. In Inter- national Conference on 3D Vision, 2017. 数据集 MPI-INF-3DPH
- J. Martinez, R. Hossain, J. Romero, and J. J. Little. A simple yet effective baseline for 3d human pose estimation. In IEEE International Conference on Computer Vision, 2017. 效果主要的对比论文