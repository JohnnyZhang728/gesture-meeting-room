

# **基于手势识别的会议控制系统需求文档**

# 一、前言

## 1.1本文档面对的读者对象

客户方管理人员：本文档根据客户方管理人员所提出的需求进行编写，使客户方管理人员能够知悉系统可以提供的服务以及系统运行时必须满足的约束从而检查系统是否满足其需要。

系统最终用户：了解本产品的用途及更新脉络，使用户理解软件可以做什么并对其进行确认，并能较为直观清晰的发现在系统使用中的一些困惑的解决方法。

软件开发者：本文档是对于所有软件开发者应当实现的所有东西的正式陈述，通过对本文档所编写的需求进行了解实现，软件开发者可以确定使用什么样的编程语言来开发系统，并理解要开发什么样的系统。

系统测试工程师：使用需求来开发系统的确认测试。

系统维护工程师：使用需求来理解系统以及系统各个部分的关系。

## 1.2 版本变更信息

初始版本1.0：用户可以登入系统，创建会议，在会议进行过程中对会议过程中需要进行的一些操作通过手势识别来控制，代替传统的接触式交互。手势识别控制的功能部分包括对已上传文件的选择、旋转、缩放与截图功能。同时控制手势识别的响应时间在200ms以内。


# 二、引言

本系统是一个非接触式的手势识别会议系统，主要完成了以下4个功能：①会议功能：用户可通过此系统实现线上会议，系统内创建会议室，与会用户输入会议id即可进入会议，会议中有音频功能用于语音交流、视频功能用于实时采集手势；②视频采集功能：用户对着摄像头作出动态手势动作，软件通过摄像头实时获取视频数据；③动态手势识别功能：软件通过实时分析获取的视频数据，识别出用户作的动态手势类型；④操作控制功能：软件根据识别出的动态手势类型做出不同的针对图片的操作，包括切换、选中、缩放、旋转、保存。

本系统使用python语言进行编程，在一般的windows与mac平台均可运行，运行前，需要简单配置相应的环境。系统实现的两大要点是手势识别与线上会议系统。为实现用户能感知到的&quot;实时手势处理&quot;，在构建过程中首要考虑的是对手势的处理效率；其次要考虑用户体验，使得线上的效率可以接近或者说达到线下的效率，为此我们对此系统的界面与功能将进行许多细节处理。系统完成后，会由测试人员进行进一步测试以完善系统。

# 三、术语表

### 3.1 目标检测

目标检测，也叫目标提取，是一种基于目标几何和统计特征的图像分割。

### 3.2 LSTM

长短期记忆网络（LSTM，Long Short-Term Memory）是一种时间循环神经网络，是为了解决一般的RNN（循环神经网络）存在的长期依赖问题而专门设计出来的，所有的RNN都具有一种重复神经网络模块的链式形式。

### 3.3 R-CNN

R-CNN的全称是Region-CNN，是第一个成功将深度学习应用到目标检测上的算法。R-CNN基于卷积神经网络(CNN)，线性回归，和支持向量机(SVM)等算法，实现目标检测技术。

### 3.4 特征提取

在机器学习、模式识别和图像处理中，特征提取从初始的一组测量数据开始，并建立旨在提供信息和非冗余的派生值（特征），从而促进后续的学习和泛化步骤，并且在某些情况下带来更好的可解释性。特征提取与降维有关。特征的好坏对泛化能力有至关重要的影响。

### 3.5 过拟合

过拟合是指为了得到一致假设而使假设变得过度严格。避免过拟合是分类器设计中的一个核心任务。通常采用增大数据量和测试样本集的方法对分类器性能进行评价。

### 3.6 欠拟合

机器学习中一个重要的话题便是模型的泛化能力，泛化能力强的模型才是好模型，对于训练好的模型，若在训练集表现差，在测试集表现同样会很差，这可能是欠拟合导致。欠拟合是指模型拟合程度不高，数据距离拟合曲线较远，或指模型没有很好地捕捉到数据特征，不能够很好地拟合数据。

### 3.7 置信度

在统计学中，一个概率样本的置信区间（Confidence interval）是对这个样本的某个总体参数的区间估计。置信区间展现的是这个参数的真实值有一定概率落在测量结果的周围的程度。置信区间给出的是被测量参数测量值的可信程度范围，即前面所要求的&quot;一定概率&quot;。这个概率被称为置信水平。

### 3.8 召回率

召回率又叫查全率(Recall Ratio)是指从数据库内检出的相关的信息量与总量的比率。查全率绝对值很难计算，只能根据数据库内容、数量来估算。

### 3.9 ResNet

ResNet(Residual Network)，残差网络。

### 3.10 手部21关键点

定位手部的21个主要骨节点，包括指尖、各节指骨连接处等，返回每个骨节点的坐标信息.

### 3.11 全连接层

全连接层的每一个结点都与上一层的所有结点相连，用来把前边提取到的特征综合起来。由于其全相连的特性，一般全连接层的参数也是最多的。

### 3.12 KNN算法

邻近算法，或者说K最近邻（KNN，K-NearestNeighbor）分类算法是数据挖掘分类技术中最简单的方法之一。所谓K最近邻，就是K个最近的邻居的意思，说的是每个样本都可以用它最接近的K个邻近值来代表。近邻算法就是将数据集合中每一个记录进行分类的方法。

### 3.13 EMA指标

EMA（Exponential Moving Average）是指数移动平均值。也叫 EXPMA 指标，它也是一种趋向类指标，指数移动平均值是以指数式递减加权的移动平均。

### 3.14 OpenCV

OpenCV是一个基于BSD许可（开源）发行的跨平台计算机视觉和机器学习软件库，可以运行在Linux、Windows、Android和Mac OS操作系统上。它轻量级而且高效——由一系列 C 函数和少量 C++ 类构成，同时提供了Python、Ruby、MATLAB等语言的接口，实现了图像处理和计算机视觉方面的很多通用算法。

### 3.15 PyQt

PyQt是一个创建GUI应用程序的工具包。它是Python编程语言和Qt库的成功融合。Qt库是目前最强大的库之一。PyQt是由Phil Thompson 开发。

### 3.16 TensorFlow

TensorFlow是一个基于数据流编程（dataflow programming）的符号数学系统，被广泛应用于各类机器学习（machine learning）算法的编程实现。

### 3.17 回归模型

回归模型（regression model）对统计关系进行定量描述的一种数学模型。如多元线性回归的数学模型可以表示为$y=β0+β1\*x+εi$，式中，β0，β1，…，βp是p+1个待估计的参数，εi是相互独立且服从同一正态分布N(0,σ2)的随机变量，`y`是随机变量；`x`可以是随机变量，也可以是非随机变量,`βi`称为回归系数，表征自变量对因变量影响的程度。

### 3.18 特征向量

矩阵的特征向量是矩阵理论上的重要概念之一，它有着广泛的应用。数学上，线性变换的特征向量（本征向量）是一个非简并的向量，其方向在该变换下不变。该向量在此变换下缩放的比例称为其特征值（本征值）。

### 3.19 梯度

梯度的本意是一个向量（矢量），表示某一函数在该点处的方向导数沿着该方向取得最大值，即函数在该点处沿着该方向（此梯度的方向）变化最快，变化率最大（为该梯度的模）。

### 3.20 超参数

在机器学习的上下文中，超参数是在开始学习过程之前设置值的参数，而不是通过训练得到的参数数据。通常情况下，需要对超参数进行优化，给学习机选择一组最优超参数，以提高学习的性能和效果。

### 3.21 分类器

分类是数据挖掘的一种非常重要的方法。分类的概念是在已有数据的基础上学会一个分类函数或构造出一个分类模型（即我们通常所说的分类器(Classifier)）。该函数或模型能够把数据库中的数据纪录映射到给定类别中的某一个，从而可以应用于数据预测。总之，分类器是数据挖掘中对样本进行分类的方法的统称，包含决策树、逻辑回归、朴素贝叶斯、神经网络等算法。

# 四、用户需求定义

用户需求

随着人工智能技术的发展，高水平人机协同正在成为主流的生产和服务方式，提高着各行各业的生产效率，形成新的产业形态。本项目便聚焦于变革传统会议控制系统的交互方式，引入一种全新的非接触式人机交互方式——动态手势操控，从而提升会议过程中各种操作的效率同时方便多人协同。

本项目目前拟定实现以下需求功能：

 1. 视频采集功能：用户对着摄像头作出动态手势动作，软件通过摄像头实时获取视频数据；

 2. 动态手势识别功能：软件通过实时分析获取的视频数据，识别出用户作的动态手势类型；

 3. 操作控制功能：软件根据识别出的动态手势类型做出不同的针对图片的操作，包括切换、选中、缩放、旋转、保存。

通过对上述需求描述的分析，我们识别出会议参会者这一参与者，以及五个用例，用例描述如下：

| **用例名**   | 切换图片                                                 |
| ------------ | -------------------------------------------------------- |
| **用例编号** | 1                                                        |
| **参与者**   | 会议参会者                                               |
| **用例描述** | 会议参会者对着摄像头作出平移手势来切换软件中的候选图片。 |

| **用例名**   | 选中图片                                                     |
| ------------ | ------------------------------------------------------------ |
| **用例编号** | 2                                                            |
| **参与者**   | 会议参会者                                                   |
| **用例描述** | 会议参会者对着摄像头作出单击手势来选中软件中的需要进一步修改的图。 |

| **用例名**   | 缩小图片                                                     |
| ------------ | ------------------------------------------------------------ |
| **用例编号** | 3                                                            |
| **参与者**   | 会议参会者                                                   |
| **用例描述** | 会议参会者对着摄像头作出缩放手势（双手趋内）来缩小选中的图片。 |

| **用例名**   | 放大图片                                                     |
| ------------ | ------------------------------------------------------------ |
| **用例编号** | 4                                                            |
| **参与者**   | 会议参会者                                                   |
| **用例描述** | 会议参会者对着摄像头作出缩放手势（双手趋外）来放大选中的图片。 |

| **用例名**   | 旋转图片                                           |
| ------------ | -------------------------------------------------- |
| **用例编号** | 5                                                  |
| **参与者**   | 会议参会者                                         |
| **用例描述** | 会议参会者对着摄像头作出旋转手势来旋转选中的图片。 |

| **用例名**   | 保存图片                                               |
| ------------ | ------------------------------------------------------ |
| **用例编号** | 6                                                      |
| **参与者**   | 会议参会者                                             |
| **用例描述** | 会议参会者对着摄像头作出抓取手势来保存修改完毕的图片。 |

用例图如下所示：

![图4.1 系统用例图](https://github.com/JohnnyZhang728/gesture-meeting-room/blob/main/imgs/4.1.png)


# 五、系统规格需求说明

## 5.1 功能性需求

1. 系统应该能够通过识别用户手势给出反馈

2. 用户通过手势动作完成对于照片的调整

## 5.2 非功能性需求

#### 5.2.1 用户界面

本系统采用C/S架构，操作界面在系统界面模块介绍，将为用户提供美观，大方，直观，操作简单的用户界面。

#### 5.2.2 硬件需求

PC和移动终端硬件配置应具有高的可靠性，可用性和安全性，无其他特殊需求。

#### 5.2.3 运行环境

Python环境和TensorFlow深度学习框架，对操作系统无限制。

#### 5.2.4 性能需求

精度需求：基于KNN实现动态手势分类器，通过摄像头采集并识别控制者连续的手势动作，完成点击、平移、缩放、抓取、旋转这5 种基本交互功能。在这五种动态手势识别上需达到80%以上的识别准确率。

处理能力：本产品涉及到大量图片和视频资源的上传，会占据较大的存储空间，其处理能力主要考虑系统的视频存储格式和云数据库能承载的最大数据容量，可以通过格式转换将视频所占空间降至最低，以增强系统的处理能力，降低购买云服务的成本。

相应时间：要求实现的动态手势识别算法能够在100ms内输出结果。

#### 5.2.5 可用性需求

1. 方便操作，操作流程合理。尽量从用户角度出发，以方便使用本产品。

2. 控制必录入项。本系统能够对必须录入的项目进行控制，使用户能够确保手势信息录入的完整。

3. 操作完成时有统一规范的提示信息。例如使用抓取手势保存当前图片时，系统可弹出提示框&quot;是否保存图片？&quot;，用户点击确认后，系统才执行保存操作，保存后可直接刷新相关页面。

#### 5.2.6 安全性需求

权限控制：根据不同用户角色，设置相应权限，用户的重要操作都做相应的日志记录以备查看。会议管理员可以查看所有用户的日志记录。没有授权登录的用户只可查看会议信息，无法通过手势识别操作控制会议。

记录日志：本系统应该能够记录系统运行时所发生的所有错误，包括本机错误和网络错误。这些错误记录便于查找错误的原因。日志同时记录用户的关键性操作信息。

# 六、系统模型

## 6.1 数据流图模型

![图6.1 系统数据流图](https://github.com/JohnnyZhang728/gesture-meeting-room/blob/main/imgs/7.1.png)

## 6.2 手势识别会议系统的分层体系结构

![图6.2 系统分层结构图](https://github.com/JohnnyZhang728/gesture-meeting-room/blob/main/imgs/7.2.png)

# 七、系统演化

系统基于KNN、EMA的动态手势分类器, 由系统界面模块、动态手势识别模块，图片操作模块三大模块构成，实现单击、抓取、缩放、平移、旋转的功能。预期变化包含但不限于：需求增加硬件设备数量及种类、增加手势识别范围、系统功能调整、部署使用软硬件环境变化、增强运行性能效率要求、运行时错误问题解决、操作界面调整。

# 八、附录

关键技术介绍：

## 8.1 基于YOLOV3的手部目标检测模型实现

### 8.1.1概述

目标检测，也叫目标提取，是一种基于目标几何和统计特征的图像分割，它的主要目的是让计算机可以自动识别图片或者视频帧中所有目标的类别，并在该目标周围绘制边界框，标示出每个目标的位置。

目标检测这些年来已经取得了很多进展，目前主流的算法主要分为两个类型：

（1）基于 Region Proposal 的 R-CNN 系算法，它们的主要思路是先通过启发式方法或者 CNN 网络产生一系列稀疏的候选框，然后对这些候选框进行分类与回归，属于 two-stage 方法；

（2）Yolo 和 SSD 算法，它们的主要思路是均匀地在图片的不同位置进行密集抽样，抽样时可以采用不同尺度和长宽比，然后利用 CNN 提取特征后直接进行分类与回归，整个过程只需要一步，属于 one-stage 方法。

针对我们的需求——手部目标检测，我们选用了识别速度快、对小目标检测效果好的YOLOV3算法进行实现。

### 8.1.2 YOLOV3网络模型介绍

YOLOV3是由Joseph Redmon和Ali Farhadi提出的单阶段检测器, 该检测器与达到同样精度的传统目标检测方法相比，推断速度能达到接近两倍。

YOLOV3将输入图像分成S\*S个格子，每个格子预测B个bounding box，每个bounding box预测内容包括: Location(x, y, w, h)、Confidence Score和C个类别的概率，因此YOLOV3输出层的channel数为B\*(5 + C)。YOLOv3的loss函数也有三部分组成：Location误差，Confidence误差和分类误差。

YOLOV3的网络结构如下图所示：

![图8.1 YOLOV3网络结构](https://github.com/JohnnyZhang728/gesture-meeting-room/blob/main/imgs/9.1.png)

YOLOv3 的网络结构由基础特征提取网络、multi-scale特征融合层和输出层组成。

基础特征提取网络：YOLOv3使用 DarkNet53作为特征提取网络，DarkNet53 基本采用了全卷积网络，用步长为2的卷积操作替代了池化层，同时添加了 Residual 单元，避免在网络层数过深时发生梯度弥散。

特征融合层：为了解决之前YOLO版本对小目标不敏感的问题，YOLOv3采用了3个不同尺度的特征图来进行目标检测，分别为13\*13,26\*26,52\*52,用来检测大、中、小三种目标。特征融合层选取 DarkNet 产出的三种尺度特征图作为输入，借鉴了FPN(feature pyramid networks)的思想，通过一系列的卷积层和上采样对各尺度的特征图进行融合。

输出层：同样使用了全卷积结构，其中最后一个卷积层的卷积核个数是255：3\*(80+4+1)=255，3表示一个gridcell包含3个bounding box，4表示框的4个坐标信息，1表示Confidence Score，80表示COCO数据集中80个类别的概率。如果换用别的数据集，80可以更改为实际类别数量。

## 8.2 基于ResNet的手部21关键点检测回归模型实现

### 8.2.1概述

在上一步通过YOLOV3训练出手部目标检测模型成功获取图像中的手部位置后，我们将bbox中的手部目标图片裁剪下来作为这一部分手部关键点检测模型的输入。

这一部分的基于ResNet的手部21关键点检测模型我们使用了开源项目进行实现。

### 8.2.2 ResNet网络模型介绍

ResNet(Residual Network)是2015年ImageNet图像分类、图像物体定位和图像物体检测比赛的冠军。针对随着网络训练加深导致准确度下降的问题，ResNet提出了残差学习方法来减轻训练深层网络的困难。在已有设计思路(BN, 小卷积核，全卷积网络)的基础上，引入了残差模块。每个残差模块包含两条路径，其中一条路径是输入特征的直连通路，另一条路径对该特征做两到三次卷积操作得到该特征的残差，最后再将两条路径上的特征相加。

残差模块如图所示，左边是基本模块连接方式，由两个输出通道数相同的3x3卷积组成。右边是瓶颈模块(Bottleneck)连接方式，之所以称为瓶颈，是因为上面的1x1卷积用来降维，下面的1x1卷积用来升维，这样中间3x3卷积的输入和输出通道数都较小。

![图8.2 Resnet残差模块](https://github.com/JohnnyZhang728/gesture-meeting-room/blob/main/imgs/9.2.png)
