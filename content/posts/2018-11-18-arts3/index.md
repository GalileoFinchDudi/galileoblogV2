---
title: ARTS 3
author: Galileo Finch
category: "arts"
cover: oskar-yildiz-631351-unsplash.jpg
---

# Algorithm

## 题目

### 题目大意

实现pow(x, n)，计算x的N次幂的值。
Implement pow(x, n), which calculates x raised to the power n (xn).

解法1：采用递归分治的方法

```java
public double myPow(double x, int n) {
  if(n == 0)
      return 1;
  if (n < 0)
      return 1 / x * myPow(1 / x, -(n + 1)); //注意这里再java中如果n取-2147483648，如果取反就会超过java int 最大值。所以需要先提出一位进行区别运算
  if (n % 2 == 1)
      return x * myPow(x, n -1);
  return myPow(x * x, n / 2);
}
```

解法二：下面来参考下另外一种写法，也是采用递归分治，程序思路一致，采用了绝对值简化一些运算

```java
public double myPow(double x, int n) {
    if (n == 0) return 1;
    if (n < 0) {
        x = 1 / x;
    }
    long absN = Math.abs((long)n);
    
    return absN % 2 == 0 ? myPow(x * x, (int)(absN / 2)) : myPow(x * x, (int)(absN / 2)) * x;
}
```

# Review

## [The Log: What every software engineer should know about real-time data's unifying abstraction](https://engineering.linkedin.com/distributed-systems/log-what-every-software-engineer-should-know-about-real-time-datas-unifying) [中译版](https://github.com/oldratlee/translations/tree/master/log-what-every-software-engineer-should-know-about-real-time-datas-unifying)

这篇文章上周已经阅读过，本周命题是【[关于日志的那些事儿](https://galileofinch.com/log-about/)】所以又重新拜读了一次。



# Technique/Tips

没有什么新的技术分享。按照Dzone上的教程修改后写成一篇教程。

https://galileofinch.com/category/microservice

# Share

《大学的替代方案》。作者与许多成功的企业家一样，没有读完大学，他从自己的经历出发，谈了如果不读大学，人生怎么办。

大学确实有一些好处，尤其是从事 STEM（科学，技术，工程和数学）、医学、法律相关职业的人，学位几乎是必需的。但是，对于其他职业（比如互联网开发），从经济成本、时间成本和培养能力的角度来看，大学并不是最好的选择。如果你努力工作，并且采用正确的方法学习，不读大学也不是太大的问题，而且可能比读大学的结果更好。

有些学生读大学，不是因为他想读，而是因为其他人都读大学，或者他听说大学毕业生收入比较高。这种盲目的高等教育效果很差，因为学什么、怎么学、何时学（大一微积分、大二统计学......），都听任别人为你安排，这会导致你将来要做的事情，可能跟大学教育没有一点关系。你可能白白浪费四年。

大学教育可以帮助你谋生，这是不假。但是，发财靠的都是自学。课堂教不会你如何成功和获取财富，只有真实的生活经验才能教会你。大学的替代方案，就是你设法在真实的世界，自己完成对自己的教育，设法取得成功。下面几点是作者给出的建议。

__（一）旅行。__如果你不知道想干什么，对什么有热情，那就去长途旅行一次。去那些遥远的国家，体验新的文化，结识各式各样的人，测试不同的生活方式，了解这个世界是如何运作的。看一下真实的世界，感受世界的丰富多彩，看看其他地方的人们怎么生活，你可能就会知道自己想干什么。

__（二）自学。__没有了大学课堂，你只有依靠自学。幸运的是，我们这个时代是最容易自学的时代。你要观看行业领导者的视频，从你想要学习的专家那里购买在线课程，参加由行业内主要公司举办的活动，听播客，阅读最好的商业书籍和专业书籍，聘请顾问在你所选的领域辅导你。

__（三）跟随杰出人士。__你选择一个想要追随的成功者，悉心研究他的一言一行。你不仅可以从此了解他所在领域的细节，而且还会了解帮助他们成功的习惯和思维方式，并且学着自己也采用相同的习惯和思维方式。

__（四）多结交正能量的朋友。__大学的一个好处，就是它提供了许多独特的机会，让你结实很多优秀的同学和老师。所以，如果你跳过大学，那么必须付出额外的努力来建立自己的社交网络。

__（五）多存钱。__你应该避免负债，不要把钱花在愚蠢的事情上面。尽可能多地存钱，这样才有能力投资自己。

https://www.knowledgeformen.com/alternatives-to-college/