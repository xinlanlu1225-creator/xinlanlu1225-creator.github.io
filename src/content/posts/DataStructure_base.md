---
title: Python数据结构基础
date: 2026-03-26T10:54:27.000Z
tags: [Python,数据结构]
category: 自学记录
comments: true
---

## **Python**数据结构基础
*number*

slice切片
### *List列表*
	a=[1,2,3,4]
	增
	a.append(5) -> [1,2,3,4,5]
	删
	a.pop(index) 返回此值 有一个返回值
	查 像数组一样
	改 a[0]=2 -> [2,2,3,4]
### *Tuple元组*
	a=(1,2,3,4)
	不能增删改
### *Set集合*
无序性 不重复性

	a={1,2,"abc"}
	增 a.add(3)
	删 a.remove(1)
	查 2 in a return true/false
### *Dictionary字典*
 type key:value   无序性 查key

	e.g
	dict={"name":"LULU","age":20}
	查： dict["name"]="LULU"
	增： dict["platform"]="bilibili"
	删： dict.pop("platform") 同时删除key和value 返回value
	改： dict["age"]=19
	
	遍历：
	e.g
	dict={"name":"LULU","age":19,"platform"="bilibili"}

	判断是否在：  "name" in dict 返回T/F

	for value in dict.values():
		print(value)	

	for k,v in dict.items(): (同时打印key和value)
		print(k,v)


### *string字符串*
	s="Hello World"  本质是由一个个字符组合成array数组
	查看字符 比如 s[0]=H s[-1]=d 而s[:]="Hello World"  s[0:4]="Hell"
	一些常用函数
	len(s) 返回字符串长度
	s.count("H") 返回s里面H有多少个
	s.isupper()   是不是全是大写字母 T/F
	s.islower()   是不是全是小写字母 T/F
	s.isdigit()   是不是都是数字 T/F
	s.lower()    把字符全部变成大写
	s.upper()    把字符全部变成小写
	s.strip()   去掉两边空格
	s.lstrip()/s.rstrip() 去掉左边/右边空格
	s.swapcase()  字符串大写变小写 小写变大写
	s.replace(old,new)  把old字符变成new字符
	s.split(" ") 用什么方式分割 这里用的是空格
	max(s) 返回最大的字母
	min(s) 返回最小的字母

### *array数组*
a=1，2，3，4，5
一块内存，**连着**的几个位置，相邻的
python用list表示数组
	
	优点： 读取(access)特别快 直接读内存索引
	缺点： 查询(比如说查2在哪里) 插入 删除都很慢 (因为插入删除都要一个个挪)

### *LinkedList链表*
刷题过程中一般都会已经定义好了
一般由  **val、next指针** 两个部分组成 

插入删除非常快
查询和访问都很慢

### *Hash Table 哈希表*
用字典实现 和dictionary来实现

key:value 查询非常快 插入和删除都很快
无法access(因为没有index)

key -> 哈希算法 ->映射到某一个内存

### *Queue队列*
特点 **FIFO 先入先出**  (像管道)
主要用在BFS算法
单端队列 双端队列

在python中如何表示呢？

1、用list表示 用append和pop 但是每次要挪比较麻烦
2、用==deque== 
	from collections import deque

	deque 具体使用
	· append() 和 popleft() 搭配使用
	·appendleft() 和 pop() 搭配使用


### *stack 栈*
特点  **FILO 先入后出** (像杯子)
主要用在DFS算法

在python中如何表示

1、deque表示

	·append() 和 pop()
	·appendleft() 和 popleft()
	
2、用==List==
比如stack: [1,2,3,4]
stack.append[4]
stack.pop(4)  都在尾部操作不需要移动整个list

### *heap 堆*
（大堆，小堆）
大堆：pop时最顶端时永远是最大值
小堆：pop时最顶端永远是最小值

在python中如何表示？

python中默认是小堆       list变堆    堆加东西    堆删除顶端

	from ==heapq== import heapify,  heappush,   heappop
	a=[1,2,3]
	heapify(a)      //变成了小堆
	heappush(a,4)     //增加一个4 4加在哪不确定 但顶端一定是最小的 目前是1
	(a=[1,2,3,4])
	heappop(a)  => a=[2,3,4]     //顶端1已经pop掉了

	nlargest(2,a) => [3,4]   //a里面前两个最大的值  
	nsmallest(2,a) => [2,3]  //a里面最小的两个值
	
python里面如何写大堆  
（无原生包）  （好像也有了 a._heapify_ max()
a[1,2,3,4] * -1
然后heapify 
最后结果再全部乘-1  全部都要乘-1

### tree树
1、有父子关系的树
2、连成环的不属于树

概念 节点 根节点  叶子节点

	根节点 最最最前面的节点
	叶子节点 无孩子 在最末端

另外的概念： 高度 深度 层

	高度： 某个节点的高度 -> 这个节点到叶子节点的最长路径
	深度： 节点深度 ->从根节点开始 到此节点的边数
	层： 分层次

	树的高度：从根节点到叶子节点的最长路径

二叉树：每个节点最多有两个叉

	普通二叉树：如上
	满二叉树：除了叶子节点 每个节点都有左右两个子节点
	完全二叉树：对树中的节点，从上至下，从左至右编号，编号的节点与满二叉树节点的位置相同
	
![[Pasted image 20260211183114.png]]
![[Pasted image 20260211183230.png]]

二叉树的遍历
·前序遍历
·中序遍历
·后序遍历
![[Pasted image 20260211183350.png]]

## 算法的复杂度
### 时间复杂度
什么是时间复杂度？
![[Pasted image 20260211191028.png]]

看里面有没有for/while

O(1)   无循环

O($logn$) 也有循环 但是循环次数不是n次 循环迭代和次方相关

O(n)   有循环 for循环

O(m+n)  有循环 有两个传入的参数

O($n\cdot logn$) 循环套循环 一个次数是n 一个logn

O($n^2$)  循环套循环 俩次数都是n

### 空间复杂度
算法存储空间与**输入值**之间的关系

占空间的都是声明出来的变量

如有一个array里面数据数量不断增加改变 O(N)

一般都是O(1) 或者 O(n)，很少有$n^2$

**看有没有递归**
递归基本都是O(N)
