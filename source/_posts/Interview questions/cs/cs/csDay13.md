---
title: 面试题汇总CShapeDay13
tags:
  - 面试题汇总
categories:
  - [面试题，C#]
author:
  - nightstardawn
---

# C# Day13

## 1.C#中属性（property）和字段（Field）的区别是什么？

**答案：**
属性一般可以用来封装字段

属性相对字段来说，属性具有封装性，允许对字段进行封装，提供更多的控制和逻辑
相比直接访问字段来说，属性允许我们在字段访问的过程汇总添加验证、计算等逻辑

属性还可以在其中对 set、get 设置不同的访问级别，使得字段的读取和写入可以收到更精细的控制

## 2.请解释一下 C#中异步编程模型（Async/Await），它是用来做什么的？

**答案：**

C#中的异步编程模拟式一种用于处理并发任务的技术
允许在执行异步操作时，让程序继续执行其他任务而且不会阻塞主线程
这对于处理需求、文件读写、长时间计算等耗时操作非常有用

## 3.请问七大排序算法一般指哪七种排序算法？你一般用的排序算法是哪一种？简单描述一下

**答案：**

- **冒泡排序**
  通过对待排序序列从前向后（从下标较小的元素开始）,依次对相邻两个元素的值进行两两比较，若发现逆序则交换，使值较大的元素逐渐从前移向后部，就如果水底下的气泡一样逐渐向上冒。
- **选择排序**
  每一趟从待排序的数据元素中选出最小（或最大）的一个元素，顺序放在已排好序的数列的最后，直到全部待排序的数据元素排完。
- **插入排序**
  一种最简单的排序方法，其基本操作是将一条记录插入到已排好的有序表中，从而得到一个新的、记录数量增 1 的有序表
- **希尔排序**
  对直接插入排序的改进算法，在使用直接排序算法时，如果序列很长，且无序程度较高，向前插入一次可能导致移动的操作很大。如果原序列相对有序（小的数大致在前，大的数大致在后），则能缓解这一问题。希尔排序就是在插入排序的之前进行了粗略的排序，使原序列相对有序。
- **归并排序**
  建立在归并操作上的一种有效的排序算法，该算法是采用分治法（Divide and Conquer）的一个非常典型的应用。将已有序的子序列合并，得到完全有序的序列；即先使每个子序列有序，再使子序列段间有序。
- **快速排序**
  采用“分治”的思想，对于一组数据，选择一个基准元素（base），通常选择第一个或最后一个元素，通过第一轮扫描，比 base 小的元素都在 base 左边，比 base 大的元素都在 base 右边，再有同样的方法递归排序这两部分，直到序列中所有数据均有序为止。
- **堆排序**
  堆是一种叫做完全二叉树的数据结构，可以分为大根堆，小根堆，而堆排序就是基于这种结构而产生的一种程序算法。

  1. 首先将待排序的数组构造成一个大根堆，此时，整个数组的最大值就是堆结构的顶端
  2. 将顶端的数与末尾的数交换，此时，末尾的数为最大值，剩余待排序数组个数为 n-1
  3. 将剩余的 n-1 个数再构造成大根堆，再将顶端数与 n-1 位置的数交换，如此反复执行，便能得到有序数组
     //父-->子:i--->左孩子:2\*i+1, 右孩子:2\*i+2;
     //子-->父:i--->(i-1)/2; (i 为下标元素)

![ 2024-09-18 141429.png](https://s2.loli.net/2024/09/18/mz76TQF8SAC2Naj.png)

## 4.请简单描述斐波那契数列的基本规则是什么？

**答案：**
假设数列从索引从零开始
斐波那契数列的基本规则就是从数列的第 2 项开始，每一项的值都是前两项的和
即：F(n) = F(n-1) + F(n-2) n>=2
1,1,2,3,5,8,13,21···

## 5.简单描述 A 星寻路算法的基本原理

**答案：**

- 关键点
  1. 寻路消耗公式：f(寻路消耗) = g(离起点距离) + h(离重点距离)
  2. 开启列表
  3. 关闭列表
- 基本原理
  每一次寻路，计算周围点的寻路消耗，并放入开启列表，对开启列表进行排序，得到寻路消耗最小的点放入关闭列表
