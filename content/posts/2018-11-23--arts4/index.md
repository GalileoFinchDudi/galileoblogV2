---
title: ARTS 4
author: Galileo Finch
category: "arts"
cover: florencia-viadana-744471-unsplash.jpg
---

# Algorithm

## Two Sum 2 - Input array is sorted

### 题目描述

在一个排序好的数组中找出两个数，使他们的和等于`目标数`。

#### 解法1，通过双层遍历，对比每个数值的累计。

#### 解法2：使用双指针

一个指针元素最左，一个指向最右。从左往右开始移动指针进行比较。

- 如果两个元素的和等于目标数，成功匹配
- 如果和大于目标数，又指针往左移动
- 否者，左指针往右移动

```java
public int[] twoSum(int[] numbers, int target) {
  int i = 0, j = numbers.length - 1;
  while (i < j) {
      int sum = numbers[i] + numbers[j];
      if (sum == target) {
          return new int[]{i + 1, j + 1};
      } else if (sum < target) {
          i++;
      } else {
          j--;
      }
  }
  return null;
}
```

# Review

[neat-algorithms-paxos](http://harry.me/blog/2014/12/27/neat-algorithms-paxos/)
Paxos 算法，是莱斯利·兰伯特（Lesile Lamport）于 1990 年提出来的一种基于消息传递且具有高度容错特性的一致性算法。但是这个算法太过于晦涩，所以，一直以来都处于理论上的论文性质的东西。

# Technique/Tips

暂留。

# Share

笛以无腔为适，琴以无弦为高，会以不期为真率，客以不迎送为坦夷。幽人清事总在自适。但识琴中趣，何劳弦上？
