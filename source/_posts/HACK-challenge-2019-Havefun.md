---
title: HACK-challenge-2019-Havefun
cover: 'https://ww1.sinaimg.cn/large/005ZbjyVly1gqonghh3u8j30xc0hgabp.jpg'
feature: false
tags:
  - 极客大挑战 2019
  - Web
  - Sql Injection
categories:
  - CTF
keywords: 'PHP'
description: 极客大挑战 2019 Havefun
date: 2021-05-22 10:59:37
---
<meta name="referrer" content="no-referrer"/>

# 极客大挑战 2019 Havefun

## 题目界面

![005ZbjyVly1gqr0xhuj7pj30qm0ridge](https://ww1.sinaimg.cn/large/005ZbjyVly1gqr0xhuj7pj30qm0ridge.jpg)

## 访问界面

![005ZbjyVly1gqr0x0tr0ij31160hymx1](https://ww1.sinaimg.cn/large/005ZbjyVly1gqr0x0tr0ij31160hymx1.jpg)

## 分析解题

页面没有什么信息，**F12 **查看源代码，发现注释信息

![005ZbjyVly1gqr0zvvyvlj30r406eglj](https://ww1.sinaimg.cn/large/005ZbjyVly1gqr0zvvyvlj30r406eglj.jpg)

然后，这是一个传参问题，直接构造 payload:`?cat=dog`就成功获取了 fla

## 成功页面

![005ZbjyVly1gqr18jibtfj314a0jit8p](https://ww1.sinaimg.cn/large/005ZbjyVly1gqr18jibtfj314a0jit8p.jpg)