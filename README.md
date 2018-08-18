# Posetools && tutorials

这个repo的内容包含了2d和3d pose estimation的一些代码和我的一些读过的觉得比较有用的文章、简单的paper reading和理解内容。其中一些需要用到的比如关键点gaussian heatmap生成的代码，一些数据集的不同框架下的dataloader，比如mpii下的pytorch dataloader等等。以2d相关工作为主。长期更新，总结和分享学到的pose estimation和计算机视觉相关的内容。

- 在tutorials部分整理了一些pose estimation的简单介绍，一些文章的paper reading，代码分析之类的内容。目前包含了简单的姿态估计介绍，后续会继续更新
- 在eval部分整理了一些数据集上的eval工具。会翻译一些使用方法之类的东西。
- 在tools部分整理了一些pose相关实验常用的工具代码。如上述提到的heatmap生成等代码

## 2018 pose estimation相关文章整理

### 2D pose estimation
Cascaded Pyramid Network for Multi-Person Pose Estimation, 2018 CVPR

MultiPoseNet: Fast Multi-Person Pose Estimation using Pose Residual Network, 2018 ECCV (这篇文章我有复现的代码，可以参见我的github代码：(https://github.com/IcewineChen/pytorch-MultiPoseNet))
