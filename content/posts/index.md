---
title: ARTS 5
author: Galileo Finch
category: "arts"
cover: eli-francis-100644-unsplash.jpg
---

# Algorithm

Merge Sorted Array

归并两个有序数组，并把结果放在第一个数组上。

```java
Input:
nums1 = [1,2,3,0,0,0], m = 3
nums2 = [2,5,6],       n = 3

Output: [1,2,2,3,5,6]
```

考虑到最后需要把结果放在第一个数组上，就不能创建新的数组。所以从尾开始遍历，否则在 nums1 上归并得到的值会覆盖掉还未进行归并比较的值。

```java
public void merge(int[] nums1, int m, int[] nums2, int n) {
    int i = m - 1, j = n - 1;
    int k = m + n - 1;
    while (i >= 0 || j >= 0) {
        if (i < 0) {
            nums1[k--] = nums2[j--];
        } else if (j < 0) {
            nums1[k--] = nums1[i--];
        } else if (nums1[i] > nums2[j]) {
            nums1[k--] = nums1[i--];
        } else {
            nums1[k--] = nums2[j--];
        }
    }
}
```

# Review

[为什么静态资源应该使用`CDN`？](https://forestry.io/blog/for-static-sites-theres-no-excuse-not-to-use-a-cdn/)

本文用一个简单的例子解释什么是 CDN，以及它的好处。

`CDN`或`Content Delivery Networks`通常用于分发静态资源，如图像，视频和客户端代码（CSS和JavaScript）。由于静态网站完全由静态资源（包括HTML页面）组成，因此可能通过CDN服务来构建整个网站！

在本文中，我们将探讨为什么您应该使用CDN来托管您的静态站点，以及如何使用Netlify来实现它。Netlify使用CDN轻松部署和托管您的静态站点。

我博客的架子就是采用netlify + reactjs + github搭建。由于是静态页面，就不考虑到服务器的问题，只需要购买一个域名即可。

# Technique/Tips

本周主要在研究监控系统，linux向还是比较薄弱，要做此类事情还是要先对linux有个整体的印象，以及一些基本概念的了解。

[初探监控系统](https://galileofinch.com/monitor-system/)
[监控系统如何选型](https://galileofinch.com/monitor-system-choose/)

# Share

前几天看到一篇文章[解谜英语语法](http://www.yinwang.org/blog-cn/2018/11/23/grammar)，其实是以计算机编程的思维来讲述英语语法，一个很独特的思考。很喜欢最后一段话。

> 中文的世界里有真知的资源实在太少，所以我基本不看中文书籍或者网站。无论什么知识，只要你用英文一搜，往往能找到更准确，更简单，更深入本质的解释。你会惊讶地发现中文的世界有多少的谬论，误传，照本宣科和一知半解。这就是为什么我认为学好英语是如此重要，因为它将为你开启一扇获取更可靠知识的窗户。
>
> 每一次讲述自己的真实思想，都是一次沉重而疑虑的付出，因为我为了获得这些思想，付出了无数的艰辛。很多时候我从没见过任何人达到如此的认识。所以当我写书的时候，我也会疑惑，我的读者们到底值不值得我如此的付出，他们获得这些知识是好事还是坏事？我不希望他们是自私自利，毫无感恩之心，甚至抄袭盗窃的小人。

emmmm，刚刚上去核实地址的时候发现地址变了。。然后那段文字也不见了。。