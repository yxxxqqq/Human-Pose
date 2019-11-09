#### LightTrack: A Generic Framework for Online Top-Down Human Pose Tracking, CVPR2019

##### contribution

- 第一个自上而下的实时人体姿态跟踪框架，在线估计；human pose estimator和Re-ID module都可以替换。

- 提出Siamese Graph Convolution Network (SGCN), as a Re-ID module for human pose matching，不同现有的Re-ID模型，人体关键点匹配使用图的表示方法。因为基于骨架的表示可以捕获人体姿态的相似性，可以有效防止相机突然移动造成的人体漂移。

- 进行了多种实验，线上线下视频都适用。

  



##### Details

- 追踪不像以往的使用卷积特征，而是使用pose特征，比如关键点。

- 最近的一些目标检测也使用HPE中heatmap的方法来推断检测结果。

- 追踪目标只在关键帧生成，单个目标跟踪实际上是姿态估计器。

- 学习ST-GCN中的时空骨架图结构，构造GCN表示人体关键点之间的空间关系，用来表示人体姿态，这样表示具有很强的鲁棒性，与人体位置和视角无关。

- Single-person Pose Tracking (SPT) 的策略是：反复估计从另外一个（Human location会影响单人姿态估计效果，人体的关键点位置同样可以大致得到人体的location，因此两者之间可以相互联系）

- Multi-target Pose Tracking (MPT) 策略，SPT的方法不适用于MPT，更好的策略是：同时跟踪多个人，并使用Re-ID模块保留/更新其身份。

- 文中处理多目标跟踪的具体措施：首先，分开处理每个人物，保证他们在的ID能够在所有帧中保留，减少时间消耗；如果由于遮挡或相机移动而丢失跟踪对象，则调用检测模块以恢复候选对象，并通过姿态匹配将他们与之前的跟踪对象关联起来。实现思路其实就是SPT+pose matching module.

- 下一帧的bounding box通过当前帧的姿态模型的joint推断出，找到最小和最大坐标，将此ROI区域每边扩大20%，这个区域作为下一帧中此人的位置，如果关键点的平均置信度低于标准值，则表明目标丢失，因为关节不太可能出现在这个bounding box区域中。

- 目标丢失时候的策略：（1）固定的关键帧间隔 Fixed Keyframe Interval (FKI)  （2）自适应的关键帧间隔 Adaptive Keyframe Interval (AKI),  文中措施：目标存在是使用KFI，一旦目标丢失，就使用AKI

- identity association: spatial consistency,  pose consistency. 首先使用spatial consistency，使用IOU阈值比较，这样的实现依赖于前后帧对象存在重叠，但是相机移动的时候，可能例外，因此需要重新检测，Re-ID模块。

  



##### Some Problems

- Multi-Object Tracking (MOT), 线下的方法会使用过去和未来的帧特征以生成轨迹，但是线上只能使用到目前为止的帧特征。



##### Pose-Track Datasets

- U. Iqbal, A. Milan, and J. Gall. Posetrack: Joint multi-person pose estimation and tracking. In CVPR, 2017.
- M. Andriluka, U. Iqbal, E. Insafutdinov, L. Pishchulin, A. Milan, J. Gall, and B. Schiele. Posetrack: A benchmark for human pose estimation and tracking. In CVPR, pages 5167–5176, 2018.
- E. Insafutdinov, M. Andriluka, L. Pishchulin, S. Tang, E. Levinkov, B. Andres, and B. Schiele. Arttrack:articulated multiperson tracking in the wild. In CVPR, 2017. 数据集  'MPII Video Pose dataset' 和一个经典的自上而下的线下追踪方法。
- J. Zhu, H. Yang, N. Liu, M. Kim, W. Zhang, and M.-H. Yang. Online multi-object tracking with dual matching at- tention networks. In ECCV, pages 379–396, 2018. 线上追踪方法
- S. Yan, Y. Xiong, and D. Lin. Spatial temporal graph convo- lutional networks for skeleton-based action recognition. In AAAI, 2018. 图网络结构ST-GCN

