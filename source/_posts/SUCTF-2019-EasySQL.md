---
title: SUCTF-2019-EasySQL
cover: 'https://ww1.sinaimg.cn/large/005ZbjyVly1gqonghh3u8j30xc0hgabp.jpg'
feature: false
tags:
  - Sql Injection
  - Web
categories:
  - CTF
keywords: 'SqlInjection'
description: 一道简单的 SQl 注入题目，说实话，我不直为啥。
date: 2021-05-22 11:44:42
---
<meta name="referrer" content="no-referrer"/>

## 题目页面

![005ZbjyVly1gqr29dcadzj30qc0swq3g](https://ww1.sinaimg.cn/large/005ZbjyVly1gqr29dcadzj30qc0swq3g.jpg)

## 访问页面

![005ZbjyVly1gqr2a5dz52j30q406ut8m](https://ww1.sinaimg.cn/large/005ZbjyVly1gqr2a5dz52j30q406ut8m.jpg)

## 分析解题

打开页面之后，没有什么思路，然后查看源代码也没有什么思路

然后我就只能百度了

发现了连个 payload

- `*,1`
- `1;set sql_mode=PIPES_AS_CONCAT;SELECT 1`

分别的显示结果为

- ![005ZbjyVly1gqr2r0ox96j31n20ha0ta](https://ww1.sinaimg.cn/large/005ZbjyVly1gqr2r0ox96j31n20ha0ta.jpg)

- ![005ZbjyVly1gqr2s1pkhgj31n60gwmxo](https://ww1.sinaimg.cn/large/005ZbjyVly1gqr2s1pkhgj31n60gwmxo.jpg)

## 题目源码

![005ZbjyVly1gqr2tu3hssj30sz0fvac5](https://ww1.sinaimg.cn/large/005ZbjyVly1gqr2tu3hssj30sz0fvac5.jpg)

其实主要就是 SQL 查询语句：`select ".$post['query']."||flag from Flag";`

由于题目没有过滤`*`，因此可以查询所有内容就可以拿到 Flag 了

Payload:`*,1`

拼接后就变成了`SELECT * ,1 || flag FROM Flag`

官方的 WriteUp 发现这不是预期的解法（可能是出题人忘记过滤`*`号了`==`)，这道题的考点其实是一个 sql_mode 参数
Oracle 在缺省情况下支持使用 `||`连接字符串，但是在 MySQL 中缺省不支持 ，MySQL 缺省使用 CONCAT 系列函数来连接字符串。

可以通过修改`sql_mode`模式：`PIPES_AS_CONCAT`来实现将`||`视为**字符串连接符**而非**或**运算符 .

因此这里预期的 Payload 是通过修改`sql_mode`来拿到 Flag，如下

Payload:`1;set sql_mode=PIPES_AS_CONCAT;SELECT 1`

拼接后就变成了`SELECT 1;set sql_mode=PIPES_AS_CONCAT;SELECT 1 || flag FROM Flag`

## 成功页面

![005ZbjyVly1gqr3c539y8j30vm0680ss](https://ww1.sinaimg.cn/large/005ZbjyVly1gqr3c539y8j30vm0680ss.jpg)