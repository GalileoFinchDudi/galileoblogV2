---
title: ARTS 10
author: Galileo Finch
category: "arts"
cover: blur-blurred-book-46274.jpg
---

# Algorithm

题目描述：反转树。

```java
public String addBinary(String a, String b) {
    int i = a.length() - 1, j = b.length() - 1, carry = 0;
    StringBuilder str = new StringBuilder();
    while (carry == 1 || i >= 0 || j >= 0) {
        if (i >= 0 && a.charAt(i--) == '1') {
            carry++;
        }
        if (j >= 0 && b.charAt(j--) == '1') {
            carry++;
        }
        str.append(carry % 2);
        carry /= 2;
    }
    return str.reverse().toString();
}
```

# Review

[微服务简介 by Nginx](https://www.nginx.com/blog/introduction-to-microservices/)

该文章是一系列的微服务教程。

# Technique/Tips

[浏览器往返缓存（Back/Forward cache）问题的分析与解决](https://efe.baidu.com/blog/bfcache-analysis-and-fix/)

往返缓存（Back/Forward cache，下文中简称bfcache）是浏览器为了在用户页面间执行前进后退操作时拥有更加流畅体验的一种策略。该策略具体表现为，当用户前往新页面时，将当前页面的浏览器DOM状态保存到bfcache中；当用户点击后退按钮的时候，将页面直接从bfcache中加载，节省了网络请求的时间。但是bfcache的引入，导致了很多问题。

# Share

[很少人知道自己在愚昧之巅](https://mp.weixin.qq.com/s/h16rUrKHXUNyH8u-ND4Mvw)

美团，相信很多人都在用，但这家公司身上有哪些你平常看不到的更深层的东西？这些东西又能给我们带来哪些不一样的启发呢？极客公园创始人张鹏和美团联合创始人、高级副总裁王慧文展开了一次深刻的对话，对这些问题进行了回答。透过这次对话，让我们看懂美团到底是一个什么样的组织，又正在创造什么样的价值。
