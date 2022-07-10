



# 流程文档说明

[TOC]

## 1. 介绍

本项目是基于《[Convolutional LSTM Network: A Machine Learning Approach for Precipitation Nowcasting](https://arxiv.org/pdf/1506.04214.pdf)》这篇文章所提出的ConvLstm网络，对数据集提供的预报数据进行订正。

## 2. 运行程序

### 2.1 项目文件结构如下

请按照一下结构存放数据集

```bash
AI_rain_shandong
 |--sample_x
 |--sample_y
 |--latDeg.npy
trainz.py
requirements.txt
AiRain_predict.py
```

### 2.2  创建conda环境（如果已安装请跳过）

为什么安装conda环境？

- conda能管理不同的Python环境，不同的环境之间是互相隔离，互不影响的。以避免原本环境乱七八糟而导致错误。

如何创建和激活conda环境？

（1）首先先创建一个虚拟环境，名为 **torch**，python版本为3.8

```bash
conda create -n torch python=3.8
```

之后，弹出提示，输入 y，即可安装。

（2）查看环境是否安装成功

```bash
conda info --envs
```

（3）激活虚拟环境 torch

```bash
 conda activate torch
```

（4）安装pytorch

进入pytorch安装官网界面：https://pytorch.org/get-started/locally/，根据自身电脑配置选择对应要求，复制下面的命令行：

![image-20220709203409036](C:\Users\Jingyi_Wang\AppData\Roaming\Typora\typora-user-images\image-20220709203409036.png)

```bash
conda install pytorch torchvision torchaudio cudatoolkit=11.3 -c pytorch
```

完成以上操作，即可完成conda虚拟环境的创建和pytorch的安装

### 2.3 激活conda环境

```bash
conda activate torch 
```

### 2.4 安装依赖包

需要的包有：

```
data==0.4
numpy==1.21.2
tensorboardX==2.5.1
torch==1.9.0
torchvision==0.10.0
```

下载所有依赖包：

```
pip install -r requirements.txt
```

### 2.5 Training

path_x传入集合预报数据(YYYYMMDDHH_x.npy)路径；

path_y传入基准数据(YYYYMMDDHH_y.npy)路径

lr：学习率

epochs：训练回合数

```bash
python trainz.py --path_x AI_rain_shandong/sample_x --path_y AI_rain_shandong/sample_y -lr 1e-4 --epochs 100 --batch_size 2
```

模型参数(.pth.tar)将保存在 ./save_model 路径下, 用于后面的predict预测

### 2.6 Predicting

path_x是待订正的预报数据的x.npy路径，weight传入训练好的模型（.pth.tar）的路径

```
python AiRain_predict.py --path_x AI_rain_shandong/AI_rain_shandong/sample_x/2021050100_x.npy --weight checkpoint_49_0.107832.pth.tar
```

input size : 4\*120*100\*100 	output size : 1\*120\*100\*100
