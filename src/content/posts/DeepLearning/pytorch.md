---
title: pytorch机器学习笔记
date: 2026-03-26T10:54:27.000Z
tags: [深度学习,pytorch]
category: 深度学习(pytorch)
comments: true
summary: 基础的深度学习概念知识，和pytorch
---

# tensor
numpy库的ndarray和tensor可以互相转化，可以直接修改，共享内存
## scalar
	0 dimension
	 scalar = torch.tensor(7)
## vector
	vector = torch.tensor([7,7])
	 1 dimension

## MATRIX
	MATRIX = torch.tensor([[7,7],[7,8]])
	 2 dimension(above)
usually upper
## TENSOR
	 TENSOR = torch.tensor([[[1,2,3],[1,2,3],[1,2,3]]])
	 3 dimension
	 shape = (1,3,3)
usually upper

## reshaping
	x=torch.arange(1.,10.) --> [1,2,3,4,5,6,7,8,9]
	 reshape要与原张量对应
	 可以 x_reshaped =  x.reshaped(9,1) or x.reshaped(3,3)
	
## view
	shares a same memory of the tensor
	 offer a different view
	similar to 指针  
	z = x.view(1,9) 如果 修改z————> z[:,0] = 5 则x第一个元素也都换了

	可以像数组一样索引 “0”代表第一个 “-1”代表最后一个
## stacking
### vstack
	
### hstack

## squeeze
	去掉单一维度 如 x = ([[1,2,3,4,5]])
	x.squeeze = ([1,2,3,4,5])
## unsqueeze
	for example x = ([1,2,3,4,5])
	x.unsqueeze = ([[1,2,3,4,5]])
## permute
	按照指定顺序重新排列目标张量的维度
	x_original = torch.rand(size=(224,224,3))
	x_permuted = x_original.permute(2,0,1)  #2，0，1分别指的是列！！

## 计算
	 ·torch.dot(x,y) 点积 向量乘向量
	 ·torch.mv(A,x)  矩阵乘向量  （左侧矩阵列数要等于右侧竖向量的行数
	 ·torch.mm(A,B)  矩阵乘以矩阵 （左列=右行

# NumPy
和数组有关的一个库
	Pytorch-->NumPy ` tensor.numpy() `
	NumPy-->Pytorch ` torch.from_numpy(ndarray) ` dtype = float64
    don't share memory

	x = np.array([1,2,3]) //生成数组
	x.dtype() //查看数据类型
矩阵计算用

	np.dot(A,B) //内积

# Reproducibility
flavour random seed
	   
	  RANDOM_SEED = 42
	  
	  torch.manual_seed(RANDOM_SEED)
	  random_tensor_c = torch.rand(3,4)


	                                       分开设置random seed
	   torch.manual_seed(RANDOM_SEED)
	  random_tensor_4 = torch.rand(3,4)
然后三四就会一样


# Matplotlib
绘制图形的库
（基础版）
	
	import numpy as np
	import matplotlib.pyplot as plt

	x = np.arange(0,6,0.1)
	y = np.sin(x)

	plt.plot(x,y)
	plt.show()
（加强版）
	
	import numpy as np
	import matplotlib.pyplot as plt

	x = np.arange(0,6,0.1)
	y1 = np.sin(x)
	y2 = np.cos(x)

	plt.plot(x,y1,label="sin")
	plt.plot(x,y2,linestyle="- -",label = "cos")
	plt.xlabel("x") //x轴标题
	plt.ylabel("y") //y轴标题
	plt.title('sin&cos') //标题
	plt.legend()
	plt.show()


显示图像
	
	import matplotlib.pyplot as plt
	from matplotlib.image import imread

	img = imread('path.png')
	plt.imshow(img)

	plt.show

# 感知机

**在感知机部分才会用到激活函数**

感知机运行原理
w1 * x1 + w2 * x2 >= 阈值 输出1
w1 * x1 + w2 * x2 <= 阈值 输出0

## 单层感知机
用函数实现与门、或门、非门
实现过程中 仅 *b（负阈值，bias）w(权重)* 的数值不一样


## 叠加层感知机
实现异或门

	def XOR(x1.x2)  //异或门
		s1 = NAND(x1,x2)  //与非门
		s2 = OR(x1,x2)   //或门
		y = AND(s1,s2)   //与门
		return y




# 神经网络

### 激活函数
必须使用非线性函数！！！
      1 (x>0)
   h(x) 
      0 (x<=0)

 转换 : 
    
    a = b+w1x1+w2x
      y = h(a)
 
  具体的激活函数

sigmoid函数
 h(x) = 1/(1+exp(-x))
 
 阶跃函数

ReLU函数
      x (x>0)
  h(x)
     0 (x<=0)

	def relu(x)
		return np.maximum(0,x)
softmax函数 概率分类

	yk=exp(ak)/sigma(1-n)(exp(ai))
  
  softmax防溢出

	np.exp(a-c)/np.sum(np.exp(a-c))

最小二乘法(LS)  
  均方误差
  用来求 **代价函数 （e与w关系）

梯度下降算法  (最大梯度)
   梯度——代价函数的导数
 **寻找代价最小时的*w
 
  *学习率 —— 新w = 旧w - 斜率 *学习率


### 线性回归
**梯度下降** —— 沿反梯度方向更新参数
**小批量随机梯度下降**  —— 随机采样b个样本近似损失
 批量大小——重要超参数 不能太大不能太小

# 卷积神经网络

卷积运算 = 滤波器运算

# ResNet


# WORKFLOW 
1、data prepare and load
2、build model
3、fit the model to the data
4、make prediction
5、save and load the model
6、put it all together

## nn
neural networks
layers!!

	import torch
	from torch import nn
	import matplotlib.pyplot as plt
## Data
1、get data into  a numberical representation
2、build a model to learn pattern

### linear regression formula

**weight权重**  **bias偏差**

	x
	y = weight*x + bias

### split into **training** and **validation** and **test** sets
not always need a validation set
for example 
	
	len(x)=50
	
	train_split = 50 * 0.8
	
	X_train = X[:train_split]   //分割



## Building a pytorch model

	class linear_regression(nn.Module)
		def __init__(self):
			super().__init__()
			self.weights = nn.Parameter(torch.randn(1,requires_grad=True,dtype=torch.float))

			self.bias = nn.Parameter(torch.randn(1,requires_grad=True,dtype=torch.float))
                 //为权重和偏差创建两个变量 随机

			//Forward method
			
			def forward(self,x:torch.Tensor) -> torch.Tensor:
				
				return self.weights*x + self.bias


# transfromer搭建

步骤：
1、构建小编码器
2、构建小解码器
3、还有linear softmax等等 最后通过小的框架 把它构建起来


## 残差和标准化（add&norm）
- **​残差连接（Add）​**​：解决深层网络的梯度消失问题，保留原始信息。
- ​**​层归一化（Norm）​**​：对输出进行标准化，使数据分布更稳定，加速训练。
- **dropout**  在训练过程中随机 **“关闭”** （丢弃）一部分神经元​​，强制网络不依赖任何单个神经元，从而提升泛化能力。
- **size(-1)** 是用于获取张量（Tensor）​**​最后一个维度的大小​**​的操作
- ***key.transpose(-2, -1)**   交换张量的最后两个维度



 