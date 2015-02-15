---
title: Hbase Lattice研究笔记
layout: post
categories: 架构
tags: Hbase Lattice
---

项目官网：[HBase-Lattice](https://github.com/dlyubimov/HBase-Lattice)

## 动机(关键特性)

- 是术语“cassendra是oltp，hbase是olap”的继续
- 支持海量的实时数据
- 低查询延迟
- 号称实时的 只要你做到持续更新数据立方体
- 和H生态系统良好的结合

## 关键原理

- 数据立方体是持续更新的 --> 谁更新？用什么样的机制更新？
- 编译生成pig代码
- 定制了hbase的filter这样查询效率更高？
- 四种客户端：java client，HBL，MapReduce(for 批处理)，R客户端
- 所有的查询都是基于数据立方体(cubids)，完全不看事实表了
- 数据立方体需要人工建模
- 支持定制个性化的维度和聚合需求

## 开发工作流

1. 定义立方体模型
2. 在Hbase的HBL系统表中部署/更新模型
3. 利用compiler bean生成comile脚本
4. 在事实流的下一个portion运行增量comile脚本
5. 按需重复4
6. 从2以后，任何时刻数据立方体都是可以查询的


##维度类型

- ___HexDimension___ 定长字节维。在Hbase的组合主键中这类维度将生成ASCII Hex，这样使用诸如HBase Shell之类的工具具有更好的可读性。
通常___HexDimension___适合分布式统一ID或其他Hash索引的数据。输入是byte[]类型。
任何的pic数值类型也可以作为输入，这里会用[Big Endian](http://blog.csdn.net/sunshine1314/article/details/2309655)方法进行转换。
在定义维度的时候需要声明维度长度。在数据转化的时候，会生成定长的维度值。若事实值长于维度定义长度，则截断删除；若小于，则填充。

- ___SimpleTimeHourHierarchy___ 时间层级维。这是一个将格里高利日历(GregorianCalendar)或代表从纪元开始的long值作为输入的，转化为[ALL].[YEAR- MONTH].[DATE-HOUR]的层次维度。
本维度描述时间的的最小粒度是一个小时。

- ___Utf8CharDimension___ 文本维。这种维度类型和___HexDimension___非常类似，唯一区别在于期待的输入是utf-8编码的字符串

___lattice认为所有维度是离散的。___ 即使从事实中结算出来的维度(例如时间)是连续的，lattice也会映射为离散值。
举例来说，___SimpleTimeHourHierarchy___ 将连续的自然时间处理为 年-月-日-时 的离散空间。
