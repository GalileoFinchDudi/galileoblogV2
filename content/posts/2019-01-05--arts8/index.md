---
title: ARTS 8
author: Galileo Finch
category: "arts"
cover: rasmus-smedstrup-mortensen-1268814-unsplash.jpg
---

# Algorithm

题目描述：判断一个数组能不能只修改一个数就成为非递减数组。

在出现 nums[i] < nums[i - 1] 时，需要考虑的是应该修改数组的哪个数，使得本次修改能使 i 之前的数组成为非递减数组，并且**不影响后续的操作**。优先考虑令 nums[i - 1] = nums[i]，因为如果修改 nums[i] = nums[i - 1] 的话，那么 nums[i] 这个数会变大，就有可能比 nums[i + 1] 大，从而影响了后续操作。还有一个比较特别的情况就是 nums[i] < nums[i - 2]，只修改 nums[i - 1] = nums[i] 不能使数组成为非递减数组，只能修改 nums[i] = nums[i - 1]。

```java
Input: [4,2,3]
Output: True
Explanation: You could modify the first 4 to 1 to get a non-decreasing array.
```

```java
public boolean checkPossibility(int[] nums) {
    int cnt = 0;
    for (int i = 1; i < nums.length && cnt < 2; i++) {
        if (nums[i] >= nums[i - 1]) {
            continue;
        }
        cnt++;
        if (i - 2 >= 0 && nums[i - 2] > nums[i]) {
            nums[i] = nums[i - 1];
        } else {
            nums[i - 1] = nums[i];
        }
    }
    return cnt <= 1;
}
```

# Review

## [优质平庸](https://www.businessoffashion.com/articles/opinion/op-ed-how-premium-mediocre-conquered-fashion)

2017年，有人发明了“优质平庸”（Premium Mediocre）这个词。它指的是一种营销手段，让消费者认为他们正在消费奢侈品，而实际上只是在消费普通商品，比如“精酿”啤酒、“手工”比萨饼、“烘焙师签名”汉堡等等都是“优质平庸”的例子。

这种做法很简单，就是用一些额外的手段，让平庸的东西看上去更加优质，让消费者产生一种幻想，认为自己正在购买高级产品。营销人员通常采用的手段是，为商品名加上“首选”、“手工”、“收藏级”等词语。

许多公司希望消费者愿意付出较高的价格，就用“负担得起的奢侈品”的定位来推销自己的产品。当然，他们销售的并不是奢侈品，而是把奢侈品的光环像面包粉一样，洒一点在平庸商品上面。这里的重点是，必须让消费者觉得，平庸的商品一点不平庸。

“优质平庸”也延伸到了真正的奢侈品品牌。普通消费者买不起这些奢侈品，但是奢侈品公司仍然想赚他们的钱，就设法提供一些入门级的产品系列，将一些低成本的产品贴上自家的奢侈品品牌，比如 Prada 尼龙背包、路易威登帆布包、Gucci的塑料凉鞋等等。这个策略很成功，优质平庸市场的利润非常高，据统计，2015年小型皮具奢侈品的销售额达到57亿美元，预计到2020年将达到75亿美元。对于消费者来说，只要几百美元，就能买到奢侈品牌，很多人愿意尝试。

这件事情的启示就是，商品的徽标比产品本身更重要，普通鸡蛋只能卖一块钱，但是标上“天然土鸡蛋”就能卖一块五。“优质平庸”的关键，就是商品有一个独一无二、消费者认同的徽标。

# Technique/Tips

## [json编译器教程](https://github.com/miloyip/json-tutorial)

json-tutorial：由Milo Yip发起的用 C 从零开始编写 JSON 库教程。

- 启程：编译环境、JSON 简介、测试驱动、解析器主要函数及各数据结构。
- 解析数字：JSON number 的语法
- 解析字符串：使用 union 存储 variant、自动扩展的堆栈、JSON string 的语法、valgrind
- Unicode：Unicode 和 UTF-8 的基本知识、JSON string 的 unicode 处理
- 解析数组：JSON array 的语法
- 解析对象：JSON object 的语法、重构 string 解析函数
- 生成器：JSON 生成过程、注意事项。练习完成 JSON 生成器
- 访问与其他功能：JSON array／object 的访问及修改

# Share

## [王兴：20年to C，20年to B](https://mp.weixin.qq.com/s?__biz=MzIxNTAzNzU0Ng==&mid=2654615671&idx=1&sn=b8d8819ad02ce10d579a4cd33ec40c16)

“下半场”这个词是去年上半年提出来的，而且这个词提出来之后，在整个行业，包括国家产生了比较大的反响。下半场到底是什么意思，为什么会有下半场这个说法？这个下半场是几个层面：一个是我们美团点评这家公司要进入下半场；一个是互联网和移动互联网这个行业我们认为要进入下半场；一个是中国的产业、中国的经济要进入下半场；全球的经济和政治，我们认为也要进入下半场。

核心观点就是，以前的野蛮时代个个抢资源，抢流量的红利期过去了。借用耗叔的话就是：

- 第一个阶段：野蛮开采。这个阶段的主要特点是资源过多，只需要开采就好了。
- 第二个阶段：资源整合。在这个阶段，资源已经被不同的人给占有了，但是需要对资源整合优化，提高利用率。这时通过管理手段就能实现。
- 第三个阶段：精耕细作。这个阶段基本上是对第二阶段的精细化运作，并且通过科学的手段来达到。
- 第四个阶段：发明创造。 在这个阶段，人们利用已有不足的资源来创造更好的资源，并替代已有的马上要枯竭的资源。这就需要采用高科技来达到了。

so, to B or not to B.