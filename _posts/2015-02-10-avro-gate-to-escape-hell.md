---
title: avro 逃离苦海的大门
layout: post
categories: 序列化
tags: avro 序列号
---
##1 现状

- 用csv作为对象序列化方法，hardcoding 序列化/反序列化
- 实时系统，升级改变schema

##2 设计

###2.1 优点

- 代码无入侵 [无须入侵的代码方案](http://avro.apache.org/docs/current/gettingstartedjava.html#Serializing+and+deserializing+without+code+generation)
- rpc和存储统一的方案
- 数据自完备
- hadoop平台的良好支持
- [java代码生产schema](http://avro.apache.org/docs/current/api/java/org/apache/avro/reflect/package-summary.html#package_description)

###2.2 缺点

- linux的运维方案不再支持了。统计统计数据啊~用awk+wc -l等

###2.3 疑问

- 序列化 循环引用？ RPC需要考虑
- schema 不一致问题？

##3 参考

- [avro 主页 spec](http://avro.apache.org/docs/current/spec.html)
