---
title: ARTS 7
author: Galileo Finch
category: "arts"
cover: laura-college-190105-unsplash.jpg
---

# Algorithm

`Given s = "leetcode", return "leotcede".`

使用双指针指向待反转的两个元音字符，一个指针从头向尾遍历，一个指针从尾到头遍历。

```java
private final static HashSet<Character> vowels = new HashSet<>(Arrays.asList('a', 'e', 'i', 'o', 'u', 'A', 'E', 'I', 'O', 'U'));

public String reverseVowels(String s) {
    int i = 0, j = s.length() - 1;
    char[] result = new char[s.length()];
    while (i <= j) {
        char ci = s.charAt(i);
        char cj = s.charAt(j);
        if (!vowels.contains(ci)) {
            result[i++] = ci;
        } else if (!vowels.contains(cj)) {
            result[j--] = cj;
        } else {
            result[i++] = cj;
            result[j--] = ci;
        }
    }
    return new String(result);
}
```

# Review

## [无人出租车开始运营](https://www.latimes.com/business/autos/la-fi-hy-waymo-one-20181205-story.html)

谷歌投资的无人汽车公司 Waymo，开始在亚利桑那州提供出租车服务。这标志着无人驾驶技术进入生产环境了。

现在，这项服务只向 Waymo 挑选的400个当地居民开放，用车的时候需要使用手机预约。每辆车的司机位会坐着一个 Waymo 的工程师，负责处理紧急情况，实际的驾驶由电脑完成。目前披露的价格是：15分钟3英里（4.8公里）为7.59美元。

# Technique/Tips

## [Docker 镜像中有什么？](https://cameronlonsdale.com/2018/11/26/whats-in-a-docker-image/)

Docker 的 Image 文件是分层的，本文简单介绍怎么查看每一层的内容，它们又是怎么组合成一个可以运行的 Image 文件。还有另外一篇类似的[文章](https://www.datawire.io/not-engineer-running-3-5gb-docker-images/)，通过控制分层来缩小 Image 文件尺寸。

# Share

## [解读张小龙2018年会语录](https://mp.weixin.qq.com/s?__biz=MjM5NzYxMzk4MQ==&mid=2649331265&idx=1&sn=b0bb5dd3b056f92426156a3644f781f7)

2018年12月12日是腾讯2018年的员工大会，微信之父张小龙的演讲语录刷屏全网。其中有这么一句话：
`善良比聪明更重要，AI可以比你更聪明，但是你比AI更善良。`

