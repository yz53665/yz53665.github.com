---
layout: post
title: Winodws环境下Cuda和Cudnn安装
categories:
- 深度学习
---


# Cuda和Cudnn安装

如果想在深度学习计算过程中使用英伟达显卡进行矩阵运算加速，需要安装Cuda和Cudnn（AMD显卡请出门右转）。Cuda是英伟达提供的基于C/C++语言的gpu矩阵运算操作函数集，Cudnn是基于Cuda封装好的机器学习、深度学习库，诸如tensorflow和pytorch在使用gpu运算时会直接调用Cudnn来实现。

## 安装最新驱动

到这里[官方 GeForce 驱动程序 | NVIDIA](https://www.nvidia.cn/geforce/drivers/)选择自己的显卡型号，然后安装最新的驱动（驱动越新越好，新驱动有很多显卡加速技术，不光是深度学习、游戏性能也会有提升）：

![image-20220126143318347](/img/image-20220126143318347.png)

下载后安装时选择精简安装即可。

## Cuda安装

在安装Cuda前先到电脑的应用程序->卸载里把 **Nvidia frameview**卸载了。

Cuda安装时需要与驱动版本匹配。到这里[Release Notes :: CUDA Toolkit Documentation (nvidia.com)](https://docs.nvidia.com/cuda/cuda-toolkit-release-notes/index.html)查看支持该驱动的Cuda版本，像我的驱动版本更新后为511.23，最高支持到CUDA 11.6 GA：

![image-20220126123740165](/img/image-20220126123740165.png)

但是请注意，这并不意味着你就可以直接安装11.6版本的Cuda了，在tensorflow官网[GPU 支持  | TensorFlow (google.cn)](https://tensorflow.google.cn/install/gpu)可以看到，目前tensorflow最高支持Cuda 11.2版本：

![image-20220126124041117](/img/image-20220126124041117.png)

然后到这里[CUDA Toolkit 11.2 Downloads | NVIDIA Developer](https://developer.nvidia.com/cuda-11.2.0-download-archive?target_os=Windows&target_arch=x86_64&target_version=10&target_type=exelocal)下载Cuda 11.2版本：

![image-20220126124645117](/img/image-20220126124645117.png)

安装时选择精简安装即可，然后一路点击下一步就行。



## Cudnn安装

下载地址[cuDNN Archive | NVIDIA Developer](https://developer.nvidia.com/rdp/cudnn-archive)，他会要求你先创建一个账号，然后回答一些问题，之后就可以下载了。需要注意的是cudnn需要和Cuda的版本相匹配，这里我们选择和11.2匹配的8.1.0版本：

![image-20220126145827287](/img/image-20220126145827287.png)

下载后会得到一个压缩包，解压到任意位置即可。

## 系统环境变量添加

安装完Cuda和Cudnn后，需要把路径添加到系统PATH中。

我的电脑右键->属性->关于->高级系统设置->环境变量->PATH->编辑

![image-20220126150901850](/img/image-20220126150901850.png)

然后将以下四个变量添加到PATH变量中（前面三个在CUDA默认安装路径下，最后一个是cudnn）：

![image-20220126150727668](/img/image-20220126150727668.png)

配置完成后重新运行项目的test.py文件，如果下面的available device显示为非0的数，就说明cuda已经成功安装并启用：

![image-20220126151721861](/img/image-20220126151721861.png)

最后一步，将**\CUDA\v11.2\extras\CUPTI\lib64**中的cupti64_2020.3.0.dll复制到**\CUDA\v11.2\bin**文件夹下并重命名为**cupti64_112.dll**。