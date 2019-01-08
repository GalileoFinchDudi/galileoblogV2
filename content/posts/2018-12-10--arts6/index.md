---
title: ARTS 6
author: Galileo Finch
category: "arts"
cover: sharon-mccutcheon-532782-unsplash.jpg
---

# Algorithm

[680. Valid Palindrome II (Easy)](https://leetcode.com/problems/valid-palindrome-ii/submissions/)

```java
Input: "abca"
Output: True
Explanation: You could delete the character 'c'.
```

**题目描述：** 可以删除一个字符，判断是否能构成回文字符串。

```java
public boolean validPalindrome(String s) {
    int i = -1, j = s.length();
    while (++i < --j) {
        if (s.charAt(i) != s.charAt(j)) {
            return isPalindrome(s, i, j - 1) || isPalindrome(s, i + 1, j);
        }
    }
    return true;
}

private boolean isPalindrome(String s, int i, int j) {
    while (i < j) {
        if (s.charAt(i++) != s.charAt(j--)) {
            return false;
        }
    }
    return true;
}
```

# Review

这篇关于WebKit和Gecko内部操作的综合性入门是以色列开发人员Tali Garsiel进行的大量研究的结果。几年后，她回顾了有关浏览器内部的所有已发布数据，并花了很多时间阅读Web浏览器源代码。

她写道：在IE的统治时期，除了将浏览器视为“黑匣子”之外没什么可做的，但是现在，随着开源浏览器拥有超过一半的使用份额，这是一个好时机。拆开引擎盖，看看网页浏览器里面有什么。

[原文](http://taligarsiel.com/Projects/howbrowserswork1.htm)

Paul Irish 和 Tali Garsiel再之后重新整理后发布的[新文章](https://www.html5rocks.com/en/tutorials/internals/howbrowserswork/)。

# Technique/Tips

数据压缩是一门通信原理和计算机科学都会涉及到的学科，在通信原理中，一般称为信源编码，在计算机科学里，一般称为数据压缩，两者本质上没啥区别，在数学家看来，都是映射。

[这篇文章](https://www.cnblogs.com/esingchan/p/3958962.html)使用通俗的语言介绍 ZIP 算法。

# Share

论文近来受到了很多关注。有Twitter账户，世界各地的聚会，GitHub存储库，还有很多关于阅读论文的讨论。所有这些关于论文的讨论，人们一定会问：

“我应该看论文吗？”

so [should I read papers](http://michaelrbernste.in/2014/10/21/should-i-read-papers.html)?

除此之外再贴个github的仓库，[papers-we-love](https://github.com/papers-we-love/papers-we-love).
