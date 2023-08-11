---
title: "Markdown语法测试" #标题
date: 2023-08-11T21:19:05+08:00 #创建时间
lastmod: 2023-08-11T21:19:05+08:00 #更新时间
summary: "测试博客主题对markdown语法的适配度" 
# author: 
categories: 
- Markdown
tags: 
- 测试
series: ""
description: "" # 文章描述，与搜索优化相关
weight: # 输入1可以顶置文章，用来给文章展示排序，不填就默认按时间排序
draft: false # 是否为草稿
showComments: true #是否展示评论
hidemeta: false # 是否隐藏文章的元信息，如发布日期、作者等
showbreadcrumbs: true #顶部显示当前路径
showSummary: true
---

# Typora设置自动上传图片至GitHub图床

## 1 字体等设置

> **字体改为微软雅黑+Consolas**

### 1.1 高手操作

对于高手一句话总结，只需修改如下两个文件：

```c++
C:\Users\sheng7k\AppData\Roaming\Typora\conf\conf.user.json
C:\Users\sheng7k\AppData\Roaming\Typora\themes\github.css
```

### 1.2 小白步骤

1. 打开Typora软件，选择需要修改主题，比如github主题。文件 >> 偏好设置 >> 打开主题文件夹

2. 接下来只需要修改这个关键的`github.css`文件即可
3. 为确保安全，强烈建议把`github.css`文件先备份一份到别处存放。
4. 首先忽略所有`@`符号开头的，然后把所有带`font-family`这一行，修改为`font-family: Consolas,"Microsoft Yahei",Arial,sans-serif;`
5. 解释下代码，`font-family`代表此处控制的是字体，一种逗号代表一个字体，这里我们写了4个字体，其中最后两种字体不重要，删去也可。我们这句话的意思就是遇到一个字体，如果是英文选用`Consolas`，因为它在前面，如果是中文就选用`Microsoft Yahei`（微软雅黑）字体，`Consolas`是不包含中文字体的。



## 2 Typora+PicGo+GitHub自动上传图片

使用三个工具：**`Typora`** +　**`PicGo`** ＋　**`GitHub`**

需要先下载安装好 `Typora` 和 `PicGo`

### 2.1 Typora设置：

1. 打开Typora工具，打开`菜单栏`—`文件`—`偏好设置`—`图像`
2. 上传服务选择：`PicGo`;PicGo路径选择PicGo安装路径下PicGo.exe

<img src="https://cdn.jsdelivr.net/gh/zwyzhy/TyporaImageBed/img/202304261250429.png" alt="image-20230426125027382" style="zoom:80%;" />

### 2.2 Github配置：

1. 进入官网`https://github.com/`注册/登录账号，新建仓库

![image-20230426125939794](https://cdn.jsdelivr.net/gh/zwyzhy/TyporaImageBed/img/202304261259840.png)

2. 生成个人令牌（**Personal access token**）GitHub主页右上角进入`Setting` —> `Developer settings` 

<img src="https://cdn.jsdelivr.net/gh/zwyzhy/TyporaImageBed/img/202304261304699.png" alt="image-20230426130429655" style="zoom:75%;" />

![image-20230426130533632](https://cdn.jsdelivr.net/gh/zwyzhy/TyporaImageBed/img/202304261305678.png)

![image-20230426130939959](https://cdn.jsdelivr.net/gh/zwyzhy/TyporaImageBed/img/202304261309993.png)

![image-20230426130639996](https://cdn.jsdelivr.net/gh/zwyzhy/TyporaImageBed/img/202304261306037.png)

> **注意：生成的私人令牌仅在生成时显示一次，往后GitHub不会再显示，因此要自己保存好！**



### 2.3 PicGo设置：

1. 打开PicGo工具，选择`图床设置`，点击`GitHub图床`

<img src="https://cdn.jsdelivr.net/gh/zwyzhy/TyporaImageBed/img/202304261314515.png" alt="image-20230426131449468" style="zoom:80%;" />

2. 各选项设置如下：

- 设定仓库名：GitHub用户名/仓库名
- 设定分支名：main  （GitHub从2020年起主分支名从master改为main）
- 设定Token：`2.2`中GitHub配置生成的 `Personal access token`
- 设定存储路径：即图片在仓库保存图片的文件夹名（可不设置）
- 设定自定义域名：`https://cdn.jsdelivr.net/gh/GitHub用户名/仓库名`

设置完成后确定即可！



### 2.4 测试：

1. `打开Typora` —> `偏好设置` —> `图像` —> `设置如下` —> `点击验证图片上传选项`：

<img src="https://cdn.jsdelivr.net/gh/zwyzhy/TyporaImageBed/img/202304261324224.png" alt="image-20230426132452164" style="zoom: 67%;" />

2. 出现如下验证成功表明Typora配置成功!往后在Typora中插入图片即会自动上传图片至GitHub图床

![image-20230426132609630](https://cdn.jsdelivr.net/gh/zwyzhy/TyporaImageBed/img/202304261326664.png)

