---
layout: post
title: >
    KillAsset使用指南
tags: [Unity, 插件]
---
这是一个关于unity资源清理的插件，可快速进行资源的整理。
插件链接见文结尾。

```yaml
### KillAsset

KillAsset是一个unity的资源清理工具。能快速查询资源依赖，资源大小，引用关系，快速清理无用资源。

### 特点

- 搜索栏支持正则表达式，针对资源名称，路径快速过滤资源。
- 对资源名称，路径，类型，引用关系排序。
- 高扩展性。除了内置workFlow外，可通过重写Workflow自定义工作流，适合任何项目。
- 目前已内置的workflow：
    1. 资源清理


### 配置
文件：插件根目录下，命名为EditorConfig.asset
可自定义配置筛查路径，剔除文件，剔除资源，剔除后缀。

### 版本
支持unity2018及以上

### 链接
[Github](https://github.com/shawnyangdx/KillAsset)
