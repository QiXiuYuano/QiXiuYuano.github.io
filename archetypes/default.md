---
title: "{{ replace .Name "-" " " | title }}" # 标题
date: {{ .Date }} # 创建时间
lastmod: {{ .Date }} # 更新时间
summary: "" # 文章简单描述，会展示在主页
# author: 
categories: 
- 分类1
- 分类2
tags: ["", ""]
series: ""
description: "" # 文章描述，与搜索优化相关
weight: # 输入1可以顶置文章，用来给文章展示排序，不填就默认按时间排序
draft: false # 是否为草稿
showComments: true # 是否展示评论
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
showbreadcrumbs: true # 顶部显示当前路径
showSummary: true
---
<!-- more --> 



