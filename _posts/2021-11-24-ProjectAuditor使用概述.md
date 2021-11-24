---
layout: post
title: >
    Unity插件-ProjectAuditor使用概述
tags: [Unity, 插件]
color: gray
---
ProjectAuditor是一个实验性的unity项目静态分析工具。
这个工具会将unity项目的资产脚本和项目设置统一做分析，并报告有可能影响性能、内存等地方的问题列表。

# **使用**

**可视窗口：**
Summary概述

**Code Issues：** 代码相关

**Setting Issues：** 设置

**Asset in Resources folders：** Resource目录下资源

**Shaders in the project:** shader相关

**Build Report available:** 分析LastBuild.buildreport文件（但是一直显示no）

#### 2.Code代码
**Filter过滤器：**

- Assembly，过滤程序集
- Area：指定区域，可选择cpu，memory等指定区域筛选
- Search：字符串搜索（不支持正则）
- show：一些特定的选项。
- only critical issues：只展示重要的条目
- Muted Issues：展示忽略的条目。
- Actions行为（可扩展）：
selected：可以给issue设置ignore（Mute和Unmute）


### 3.Resource资源

 展示所有resource资源。
 shader可以展示是否启用了srp batcher和gpu instancing。
（个人感觉用处不大，所有的recommand都是建议用assetbundle）

### 4.Settings设置
过滤unity里的各种setting。列出会影响性能的指标。

### 5.Assemblies程序集
展示程序集。

### 6.BuildReport
展示打包报告。

# 总结
**优点：**

1. 对代码检查十分有用。能检查一些可能导致堆分配过高的代码逻辑。
1. 检查一些耗费性能的代码使用。
1. 扩展性强，可以针对项目一些特定的逻辑，资源做一些自定义的recommandation。
1. 对检查unity设置也比较管用。能查看一些可能遗漏、影响性能的unity设置选项。

**缺点：**

1. 工具本身一些设置并不能实时刷新。
1. 搜索不能用正则过滤（可能处于性能考虑）
1. 在资源检查上并不是很有用。建议配合其他插件做资源检查。