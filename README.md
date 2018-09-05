# Pose Estimation tutorials

这个repo的内容包含了2d和3d pose estimation的一些代码和我的一些读过的觉得比较有用的文章、简单的paper reading和理解内容。代码部分包含了一些pose estimation常用的工具，其中一些需要用到的比如关键点gaussian heatmap生成的代码，性能评估，一些数据集的不同框架下的dataloader，比如mpii下的pytorch dataloader等等。以2d相关工作为主。长期更新，总结和分享学到的pose estimation和deep learning，计算机视觉相关的内容。

- 在tutorials部分整理了一些pose estimation的简单介绍和tutorial，一些文章的paper reading，代码分析之类的内容。目前包含了简单的姿态估计介绍，后续会继续更新
- 在eval部分整理了一些数据集上的eval工具，我会翻译和介绍一些使用方法之类的东西。
- 在tools部分整理了一些pose相关实验常用的工具代码。如上述提到的heatmap生成等代码
- deep_learning部分分享了一些我关于框架使用或是代码片段的分享，还有一些学习过程中的心得

## 关于pose estimation
目前的2D Pose estimation主要分为两类：第一类是单人pose estimation，目前主流方法以stacked hourglass或者convulution pose machine结构为基础为主，后续多次的state-of-the-art主要基于这两种结构的基础上再进行改动，多数以堆积一些新的trick提高性能为主。

到底什么是pose estimation？可以这么理解：对人体的姿态信息进行估计，通过人体骨架的形式表现出来。如下面这张图：

<div style="align: center">
<img src="https://camo.githubusercontent.com/fe6a3425203fd42bb3c21feb54ed6117e8449fcb/68747470733a2f2f6769746875622e636f6d2f434d552d5065726365707475616c2d436f6d707574696e672d4c61622f6f70656e706f73652f7261772f6d61737465722f646f632f6d656469612f6b6579706f696e74735f706f73655f31382e706e67"/>
</div>

这张图表明了姿态估计的任务要求:获取关键点位置，最终表达成骨架的形式。目前常见的姿态估计的任务细分主要是单人的姿态估计和多人姿态估计，单人姿态估计较成熟，以stacked hourglass为基础结构的一系列网络已经基本摸到了天花板。多人上目前还有提升空间，主要以自顶向下方法：先检测人再对每个人分别做姿态估计，和自底向上方法：先检测所有关键点，再分配的人构建多人姿态估计结果为主。自顶向上依赖检测器和后续的冗余信息处理，但是随着目前检测器性能的上升，将各种精度高的模块组合之后性能好。自底向下性能不如自顶向下好，但是由于省去了检测人的步骤，速度快，可以实现实时的多人pose estimation。具体的一些介绍和tutorial可参见tutorials部分，传送门：https://github.com/IcewineChen/Pose-estimation_tutorials/blob/master/tutorials/Posetutorial.ipynb

关于自顶向下和自底向上方法的区别，请见https://github.com/IcewineChen/Pose-estimation_tutorials/blob/master/tutorials/top-down.ipynb

## 2018 pose estimation相关文章整理

### 2D pose estimation
#### 多人姿态估计
Cascaded Pyramid Network for Multi-Person Pose Estimation, 2018 CVPR
- CPN是目前的top-down方法的state-of-the-art，17年的coco冠军，整体结构由globalNet+refineNet精修得到，代码已开源，pytorch版本传送门：https://github.com/GengDavid/pytorch-cpn

MultiPoseNet: Fast Multi-Person Pose Estimation using Pose Residual Network, 2018 ECCV 
- 这篇文章有我复现的代码，可以参见我的github代码：https://github.com/IcewineChen/pytorch-MultiPoseNet

DensePose: Dense Human Pose Estimation In The Wild, 2018 CVPR
- FAIR 2018年年初发表的文章。demo效果很有意思，文章方法综合了语义信息，结合采样点得到了一个密集的pose估计。project的传送门：https://github.com/facebookresearch/DensePose

LSTM Pose Machines, CVPR 2018
- 以Convolutional pose machine作为baseline

Generative Partition Networks for Multi-Person Pose Estimation，2018 ECCV
- 还没细看，看完更新补上这篇文章的简单介绍

#### 单人姿态估计
18年目前没看到很好的相关文章。大概stacked hourglass融入的各种trick已经导致性能很难大幅上升，基于stacked hourglass相对最好的工作大概是17 ICCV的PRM？目前18年ECCV的MSSNet已经超过PRM-B（2018.9.5更新），在下面的经典文章中有提到。一些经典的单人姿态估计文章和方法在下面会有提到。

18年文章列表：

Multi-Scale Structure-Aware Network for Human Pose Estimation，2018 ECCV
- 击坠了PRM-B之前的92.0，在MPII上提升到了92.1……其实区别不大，文章17年在arxiv就已经看得到了，中了18年的ECCV。总体思路还是基于stacked hourglass，主要是在hourglass的每个尺度上都把信息融合在loss上，做了一个multi-scale supervision。

## pose相关的代表文章整理
Stacked Hourglass Networks for Human Pose Estimation, 2016 ECCV
- 必须要看的一片文章，非常经典的单人姿态估计方法，融合了多个尺度信息的沙漏网络。不管是做单人还是多人的top-down，stacked hourglass是大多数姿态估计网络的主要base的结构。代码可以参见我的github，有一版mxnet的复现：https://github.com/IcewineChen/mxnet-stacked_hourglass, 一版pytorch的代码：https://github.com/IcewineChen/pytorch-pose-hg-3d 的2d branch,fork自xingyizhou并调整了一些bug

Convolutional pose machines, 2016 CVPR
- 另一篇单人姿态估计的经典方法，多个stage响应结合特征传递，引入中继监督和扩大感受野等各种trick提高性能，对遮挡条件下的估计也有很强的鲁棒性。代码作者在文章中给了开源地址。

Learning Feature Pyramids for Human Pose Estimation, 2017 ICCV
- 这篇文章写得很好，读起来非常顺应思路。结构上改动不大，基于沙漏结构，修改了残差连接的结构，在输入上用了多个scale，根据方差的推导提出了新的初始化方法。值得一读，学习写作。我复现了这篇文章的pytorch版本，传送门：https://github.com/IcewineChen/pytorch-PyraNet .PRM-B在MPII上跑出了

Realtime Multi-Person 2D Pose Estimation using Part Affinity Fields, 2017 CVPR
- 经典的多人姿态估计bottom-up方法，通过PAF编码相对位置的空间信息，巧妙在多人重叠条件下优化了连接的np-hard问题。代码开源在github上，工作组发布了一个openpose库，基本可以实现实时姿态估计。

Associative Embedding: End-to-End Learning for Joint Detection and Grouping, 2017 NIPS
- 将回归得到的heatmap映射到embedding特征，bottom-up思路的多人姿态估计工作。

RMPE: Regional Multi-person Pose Estimation, 2017 ICCV
- 堆积了当时大量的state-of-the-art方法作为模块组成，自顶向下的多人姿态估计。重点放在最后单人姿态估计后的处理，解决定位误差和bounding box冗余的问题。引入了一种姿态参数的nms和一个对称空间变幻网络，效果很不错，在github开源了alphapose的系统

Adversarial PoseNet: A Structure-aware Convolutional Network for Human Pose Estimation，arxiv
- 这篇文章我似乎见过发表的版本，不过我最早看的时候还是挂在arxiv上。引入GAN, 两个Discriminator轮流训练，引用confidence去做鉴别器的motivation是解决遮挡问题，思路上还是蛮站的住脚的，mpii上也刷出了92.1。

Self Adversarial Training for Human Pose Estimation
- 同样是引入GAN的方法，效果上跟上面一篇差不多。D沿用了stacked hourglass对heatmap再次重构，计算ground truth和生成heatmap的MSELoss。

#### 单人姿态估计中基于stacked hourglass的方法
stacked hourglass频繁作为baseline，被后续很多文章借鉴和改进。这几天对ECCV的pose文章进行了调研，把accepted list中挂在arxiv上的文章简单读了一下，发现2016年的stacked hourglass直到18年还是很多方法在借鉴和修改。很多文章在上面已经提到了，在这里总结一下，方便大家沿着这条线研究，也整理一下自己的思路。

- [x] Stacked Hourglass Networks for Human Pose Estimation
- [x] Learning Feature Pyramids for Human Pose Estimation
- [x] Multi-Scale Structure-Aware Network for Human Pose Estimation
- [x] Self Adversarial Training for Human Pose Estimation

and so on……还有不少，但是我一时间印象深刻的就这几篇。想起一点更新一点。

很多用stacked hourglass的多人检测暂时先不归类。