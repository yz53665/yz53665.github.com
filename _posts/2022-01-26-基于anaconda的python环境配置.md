---
layout: post
title: 基于anaconda+pycharm/vscode的python环境配置
categories:
- Python
---


# 基于anaconda+pycharm/vscode的python开发环境配置

anaconda是python下包含环境管理、包管理功能的工具。通过它可以非常方便的创建多个独立于系统、互相平行的python环境，每个环境互不干扰。

## Windows环境下配置

从官网[Anaconda | Individual Edition](https://www.anaconda.com/products/individual)下载anaconda安装包（新手不建议用miniconda）。

安装时在这一个页面只勾选第二个即可。

第一个选项是指将anaconda3的可执行文件添加到windows的PATH环境中，以后可以直接通过windows命令行（cmd）直接执行anaconda指令，但这会覆盖掉原来的python环境，可能影响电脑其它软件和功能的正常使用；而且之后我们会使用anaconda自带的虚拟Linux命令行来代替cmd，所以这个不建议勾选。

第二个选项是运行Pycharm、vscode等编辑器时自动检测anaconda环境并将其添加为默认开发环境，如果想使用系统的python环境这个可以不选，但之后需要手动添加anaconda环境。

![image-20220124171722702](/img/image-20220124171722702.png)

安装完成后，在我们的菜单启动栏多出了一个Anaconda3的文件夹，以后我们都使用里面的anaconda Prompt来进行操作。

第一个Anaconda Navigator是带界面的python环境配置，设计的不好，卡的很，不建议使用；

第二个是PowerShell的操作方式，一般人不用；

第四个是JupyterNotebook，是个类似于matlab可交互式python编辑器，有兴趣自己了解，但是它也可以从anaconda prompt中启动；

最后两个Spyder跟编辑器有关，相当于Jupyter Notebook的加强版，有兴趣可以自己尝试。

![image-20220124125755565](/img/image-20220124125755565.png)

## anaconda基本使用

以下操作均在anaconda prompt下进行。

### 更新conda

刚安装的conda版本和基础环境版本需要更新：

```bash
conda update -n base -c defaults conda
```

### 创建新的环境

创建名字为**deepLearning**并且python版本为**3.7**的环境：

```bash
conda create --name "deepLearning" python=3.7
```

### 查看已有环境名称及路径

```bash
conda env list
```

### 切换环境

切换到**deepLearning**环境：

```bash
conda activate deepLearning
```

### 安装必备库

深度学习需要使用tensorflow（深度学习库）、scikit-learn（机器学习库）、matplotlib（画图的）、h5py（支持h5格式数据读取）、python版本的opencv（图像处理库）：

```bash
pip install matplotlib scikit-learn h5py tensorflow==2.5 opencv-contrib-python
```

如果安装速度比较慢，可能需要换源，在后面加上**-i https://pypi.tuna.tsinghua.edu.cn/simple** 改为使用清华镜像：

```
pip install matplotlib scikit-learn h5py tensorflow==2.5 opencv-contrib-python -i https://pypi.tuna.tsinghua.edu.cn/simple
```

其中tensorflow和opencv会比较大（吐槽：windows下的东西编译好比linux下大好几倍，linux下tensorflow只有50mb左右，windows有400多mb），需要多等一会。

## 编辑器开发环境配置（二选一）

### VsCode

下载安装包[Download Visual Studio Code - Mac, Linux, Windows](https://code.visualstudio.com/Download)，如图所示：

![image-20220124172138195](/img/image-20220124172138195.png)

VsCode打开安装包后选择路径、勾上创建桌面快捷方式，然后一路确认即可。

第一次打开时右下角会提示安装中文语言包，需要重启，点确认即可。

重启后选择左侧最下面的商店，并下载python插件，如下图所示：

![image-20220126100213088](/img/image-20220126100213088.png)

然后会提示你选择python解释器的位置，选择刚刚创建的deepLearning即可

![image-20220126100358061](/img/image-20220126100358061.png)

然后打开项目，选择test.py，如果输出正常，没有报错就说明安装没有问题

![image-20220126105538822](/img/image-20220126105538822.png)

上面提示没有.dll文件是正常的，之后需要安装cuda和cudnn来实现gpu加速。

### PyCharm

下载安装包[Download PyCharm: Python IDE for Professional Developers by JetBrains](https://www.jetbrains.com/pycharm/download/#section=windows)，选择community版本即可，如果需要专业版可以用自己的学校邮箱申请，一般community版本就够用了。

安装时有配置界面，左边第一个是创建桌面快捷方式，第二个是将”作为项目使用Pycharm打开当前文件夹“这一选项添加到鼠标右键菜单，第三个是默认使用Pycharm打开.py文件，右边第一个是将Pycharm的bin文件夹加入PATH（不推荐），可以根据自己喜好选择。

![image-20220124171722702](/img/image-20220124171722702-16439433506851.png)

打开PyCharm后，选择项目路径

![image-20220125162537495](/img/image-20220125162537495.png)

打开项目后，点击右上角的设置按钮，配置刚刚创建的deepLearning环境的python解释器的路径

![image-20220125220236497](/img/image-20220125220236497.png)

然后在打开的窗口左侧选择conda environment->existing environment，然后在Interpreter中选择创建的环境路径（不知道在哪可以在conda prompt中输入**conda env list**查看环境的路径）下的python可执行文件：

![image-20220125220546475](/img/image-20220125220546475.png)

确认完成后一路点击确定，等待屏幕下方Loading进度条完成后就可以正常使用了（第一次时间会比较长，可能需要几分钟时间才能加载完成），如果提示缺少什么库，就上conda prompt中使用conda指令安装包（**注意每次重新打开conda都要conda activate deepLearning来激活对应的环境，否则默认安装到base环境中，pycharm还是搜索不到**）：

```bash
conda install xxxx
```

如果查找不到这个包或提示有冲突，可以改为使用pip指令安装：

```bash
pip install xxx
```

