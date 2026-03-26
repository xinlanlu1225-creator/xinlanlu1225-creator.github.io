---
title: LeetcodeLearningLog
date: 2026-03-26T10:54:27.000Z
tags: [Python,数据结构]
category: 数据结构、算法自学
comments: true
summary: To Be Continued……
---

## 数组
在**连续**的内存空间中，存储一组**相同类型**的元素

**·访问**  O(1)   %% 时间复杂度 %%
**·搜索**  O(N) 
**·插入**  O(N) 
**·删除**  O(N) 

适合读 不适合写

$\underline{\text{数组常用操作}}$

	创建数组
	a = []  
	添加元素
	a.append(1)
	a.append(2)     %%  append也有可能是O(N)的，比如如果2后面内存不够了，再加要整体挪 %%

	插入 	a.insert(1,99)  %% 意思是在元素1后面加一个99 %%

	访问  a[2] (=99)

	更新元素   a[2]=88  %% 就可以把99换成88 %%

	删除元素的三种方法 a.remove(88)  %%  88指的是值 %%
		         a.pop(1)  %% 1指的是索引 %%
		         a.pop() %%  默认删除最后一个元素 %%

	获取数组长度  a=[1,2,3]
		size = len(a)

	遍历数组的三种方法
			for i in a:
				print(i)   %% i代表的是数组里面的元素 %%
		
			for index,element in enumerate(a):
				print("index at",index,"is",element)

			for i in range(0,len(a)):
				print("i:",i,"element:",a[i])		

	查找某个元素
			index = a.index(2)   %% 此时为查找"2"这个值的索引 %%

	数组排序  排序时间复杂度O(NlogN)
		a=[2,1,3]
		a.sort()    %%  a变成了[1,2,3],从小到大 %%
		a.sort)(reverse=True)  %% a变成[3,2,1] %%

**数组三个练习题**  485,283,27

## 链表
203 206  **适合读少写多

python中常用操作：

用的包 **deque**

1、创建链表
linkedlist = deque()

2、添加元素
linkedlist.append(1)  %% 此时是在尾部添加 %%
linkedlist.append(2)
linkedlist.append(3)

%% 想在中间添加 %%
linkedlist.insert(2,99) %% 分别是位置索引和值 %%
%% 此时复杂度仍然为O(N),因为指针很有可能在一开始，要找到这个元素 %%

3、访问元素
element = linkedlist[2]
print(element) %% 时间复杂度O(N) %%

4、搜索元素
index = linkedlist.index(99)  %% 查找数值为99的元素的索引 %%

5、更新元素
linkedlist[2]=88

6、删除元素
linkedlist.remove(88)  %% 这是删除88这个元素 %%

del linkedlist[2] %% 这是删除索引为2的值 %%

7、长度
length = len(linkedlist)