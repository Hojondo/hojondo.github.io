---
title: GIS-几个地图可视化工具-对比试用
top: false
cover: false
toc: true
mathjax: false
date: 2020-06-28 11:09:57
tags: ['webGL图形学']
password:
summary:
categories:
---
我们常常听说的数据可视化大多指狭义的数据可视化以及部分信息可视化。根据数据类型和性质的差异，经常分为以下几种类型：
- 统计数据可视化：用于对统计数据进行展示、分析，一般都是以数据库表的形式提供，常见的有 **HighCharts**、**ECharts**、**G2**、**Chart.js** 、**FineBI**等等；
- 关系数据可视化：主要表现为节点和边的关系，比如流程图、网络图、UML 图、力导图等。常见的关系可视化类库有 mxGraph、JointJS、GoJS、G6 等；
- 地理空间数据可视化：常见类库如 Leaflet、Turf、Polymaps 等等；
- 还有时间序列数据可视化（如 timeline）、文本数据可视化（如 worldcloud）等等
# GIS即 地理信息系统
国外有设计优秀的地图以及相关应用的公司，知名的如 mapbox、 cartoDB、 stamen工作室等等
- mapbox几乎是最美地图的设计公司（简而言之，他家主要生产地图底图），公司孜孜不倦地提高视觉水准
- cartoDB 是一个支持GIS的数据库，你有了数据要展现，数量可能很大，需要找个地方增删改查，cartoDB就干这个，carto的主页上长着许多有趣的应用
- stamen则是一家非常有个性的地图以及数据可视化的设计工作室，东西做的非常惊艳，但个性化有余而平台化不足
## Deck.gl
Uber开源的地理空间分析工具箱，基于 Mapbox 的底层设计，提供更加简洁易行的位置数据分析能力
## kepler.gl
基于deck.gl构建的React 组件，应用于大规模地理定位数据集的可视化探索
## WebGL Globe
an open platform for geographic data visualization

# 参考
> - GIS 有哪些酷炫的应用？ - 周宁奕的回答 - 知乎https://www.zhihu.com/question/30616181/answer/63052745
> - 一般用哪些工具做大数据可视化分析？ - 帆软的回答 - 知乎https://www.zhihu.com/question/33692103/answer/796113370
> - 大数据可视化的"轮子" - 知乎文章 - https://zhuanlan.zhihu.com/p/82174830