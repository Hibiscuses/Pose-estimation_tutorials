# Pose Estimation tutorials

这个repo的内容包含了2d和3d pose estimation的一些代码和我的一些读过的觉得比较有用的文章、简单的paper reading和理解内容。代码部分包含了一些pose estimation常用的工具，其中一些需要用到的比如关键点gaussian heatmap生成的代码，性能评估，一些数据集的不同框架下的dataloader，比如mpii下的pytorch dataloader等等。以2d相关工作为主。长期更新，总结和分享学到的pose estimation和deep learning，计算机视觉相关的内容。

- 在tutorials部分整理了一些pose estimation的简单介绍和tutorial，一些文章的paper reading，代码分析之类的内容。目前包含了简单的姿态估计介绍，后续会继续更新
- 在eval部分整理了一些数据集上的eval工具，我会翻译和介绍一些使用方法之类的东西。
- 在tools部分整理了一些pose相关实验常用的工具代码。如上述提到的heatmap生成等代码
 
## 关于pose estimation
目前的2D Pose estimation主要分为两类：第一类是单人pose estimation，目前主流方法以stacked hourglass或者convulution pose machine结构为基础为主，后续多次的state-of-the-art主要基于这两种结构的基础上再进行改动，多数以堆积一些新的trick提高性能为主。

到底什么是pose estimation？可以这么理解：对人体的姿态信息进行估计，通过人体骨架的形式表现出来。如下面这张图：

![avatar](https://camo.githubusercontent.com/fe6a3425203fd42bb3c21feb54ed6117e8449fcb/68747470733a2f2f6769746875622e636f6d2f434d552d5065726365707475616c2d436f6d707574696e672d4c61622f6f70656e706f73652f7261772f6d61737465722f646f632f6d656469612f6b6579706f696e74735f706f73655f31382e706e67)

这张图表明了姿态估计的任务要求:获取关键点位置，最终表达成骨架的形式。目前常见的姿态估计的任务细分主要是单人的姿态估计和多人姿态估计，单人姿态估计较成熟，以stacked hourglass为基础结构的一系列网络已经基本摸到了天花板。多人上目前还有提升空间，主要以自顶向下方法：先检测人再对每个人分别做姿态估计，和自底向上方法：先检测所有关键点，再分配的人构建多人姿态估计结果为主。自顶向上依赖检测器和后续的冗余信息处理，但是随着目前检测器性能的上升，将各种精度高的模块组合之后性能好。自底向下性能不如自顶向下好，但是由于省去了检测人的步骤，速度快，可以实现实时的多人pose estimation。具体的一些介绍和tutorial可参见tutorials部分，传送门：https://github.com/IcewineChen/Pose-estimation_tutorials/blob/master/tutorials/Posetutorial.ipynb

## 2018 pose estimation相关文章整理

### 2D pose estimation
Cascaded Pyramid Network for Multi-Person Pose Estimation, 2018 CVPR
- CPN是目前的top-down方法的state-of-the-art，17年的coco冠军，整体结构由globalnet+refinenet精修得到，代码已开源，pytorch版本传送门：https://github.com/GengDavid/pytorch-cpn

MultiPoseNet: Fast Multi-Person Pose Estimation using Pose Residual Network, 2018 ECCV 
- 这篇文章我有复现的代码，可以参见我的github代码：https://github.com/IcewineChen/pytorch-MultiPoseNet

DensePose: Dense Human Pose Estimation In The Wild

## pose相关的经典文章整理
Stacked Hourglass Networks for Human Pose Estimation, 2016 ECCV
- 必须要看的一片文章，非常经典，融合了多个尺度信息的沙漏网络。不管是做单人还是多人的top-down，stacked hourglass是大多数姿态估计网络的主要结构。代码可以参见我的github，有一版mxnet的复现：https://github.com/IcewineChen/mxnet-stacked_hourglass, 一版pytorch的代码：https://github.com/IcewineChen/pytorch-pose-hg-3d的2d branch,fork自xingyizhou并调整了一些bug

Convolutional pose machines, 2016 CVPR
- 另一篇单人姿态估计的经典方法，多个stage响应结合特征传递，引入中继监督和扩大感受野等各种trick提高性能，对遮挡条件下的估计也有很强的鲁棒性。代码作者在文章中给了开源地址

Learning Feature Pyramids for Human Pose Estimation, 2017 ICCV
- 这篇文章写得很好，读起来非常顺应思路。结构上改动不大，基于沙漏结构，修改了残差连接的结构，在输入上用了多个scale，根据方差的推导提出了新的初始化方法。值得一读，学习写作。






