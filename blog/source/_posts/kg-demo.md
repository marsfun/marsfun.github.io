---
title: kownledge graph demo
date: 2018-08-07 20:23:40
categories: 
- 技术
- 研究
tag: [知识图谱 ]
---
 基于elasticsearch ， kibana 知识图谱查询的例子。

1）实现思路是将知识内容，以知识图谱的概念梳理成 subject predicate object   ，将数据索引在es中。
2）通过kibana graph 进行数据相关性查询以及展示。
3）可以对实体进行下钻查看。

【demo 地址】：https://github.com/marsfun/elk-graphsearch

基于golang 的开源图数据库Cayley , Cayley is an open-source graph inspired by the graph database behind Freebase and Google's Knowledge Graph.

主要是基于标准图数据库方式对数据进行存储和查询。相比于Neo4j 部分功能需要授权使用来说，Cayley是完全开源。

后端支持es存储等多种存储方式，具体feature如下：


【demo地址】：https://github.com/marsfun/cayley-graph

Lambda³ - 基于自然语言分析模型 与 ES 实现的 knowledge graph 系统 
  
官方描述 ： StarGraph (aka *graph) is a graph database to query large Knowledge Graphs. Playing with Knowledge Graphs can be useful if you are developing AI applications or doing data analysis over complex domains.

  主要有以下部分构成：
  Indra： 模型训练及服务组件
               提供的模型有  w2v ，dep ，esa ，lsa

  stargraph： 负责图数据存储及相关查询转换逻辑
                      底层使用jena 存储

  corenlp：自然语言处理

  es：进行数据的索引
  

【官方docker-compose  地址】：https://github.com/Lambda-3/Stargraph    
