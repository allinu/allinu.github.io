---
title: 极客大挑战_2019_EasySQL
feature: false
cover: https://ww1.sinaimg.cn/large/005ZbjyVly1gqonghh3u8j30xc0hgabp.jpg
tags:
  - 极客大挑战 2019
  - Web
  - Sql Injection
categories:
  - CTF
keywords: 'SqlInjection'
description: 一道入门级的 Sql 注入题目，真的太初级了，我竟然也做了出来
date: 2021-05-16 19:03:56
---
<meta name="referrer" content="no-referrer"/>

# 极客大挑战 2019 EasySQL

## 题目页面

![005ZbjyVly1gqkhbqtyx4j30uc0ukmyf](https://ww1.sinaimg.cn/large/005ZbjyVly1gqkhbqtyx4j30uc0ukmyf.jpg)

没有信息，直接进行下一步

## 访问页面

![005ZbjyVly1gqkhf5uarfj32ce1f0n64](https://ww1.sinaimg.cn/large/005ZbjyVly1gqkhf5uarfj32ce1f0n64.jpg)

查看网页源代码之后并没有什么有价值的信息，那就在输入框下手

## 实施过程

输入`admin`和`password`测试

![005ZbjyVly1gqkhjqvzbzj326i110q9b](https://ww1.sinaimg.cn/large/005ZbjyVly1gqkhjqvzbzj326i110q9b.jpg)

报错并不是弱口令，那就试试 SQL 注入，万能密码：`admin' or 1=1 -- -`, 帐号密码同时输入

## 成功页面

![005ZbjyVly1gqkhle0f9gj317o0rktbp](https://ww1.sinaimg.cn/large/005ZbjyVly1gqkhle0f9gj317o0rktbp.jpg)
