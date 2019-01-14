---
title: ARTS 9
author: Galileo Finch
category: "arts"
cover: joshua-whysall-1288045-unsplash.jpg
---

# Algorithm

题目描述：反转树。

```java
public TreeNode invertTree(TreeNode root) {

    if (root == null) {
        return null;
    }

    final Deque<TreeNode> stack = new LinkedList<>();
    stack.push(root);

    while(!stack.isEmpty()) {
        final TreeNode node = stack.pop();
        final TreeNode left = node.left;
        node.left = node.right;
        node.right = left;

        if(node.left != null) {
            stack.push(node.left);
        }
        if(node.right != null) {
            stack.push(node.right);
        }
    }
    return root;
  }
```

# Review

[脑电波控制蓝牙鼠标和电脑](https://journals.plos.org/plosone/article?id=10.1371/journal.pone.0204566)

斯坦福大学的科学家将电极植入瘫痪病人的大脑皮层，接受脑电波，转为数字信号，控制无线蓝牙鼠标，操作平板电脑。参与实验的患者，可以使用常见程序（网页浏览、电子邮件、聊天、发送短信等）。
这项发明不仅对瘫痪者有用，长期来看，可能会为意念操作电脑创造可能性。

# Technique/Tips

[详解 Object.create(null)](https://juejin.im/post/5acd8ced6fb9a028d444ee4e)

在Vue和Vuex的源码中，作者都使用了Object.create(null)来初始化一个新对象。为什么不用更简洁的{}呢？
在SegmentFault和Stack Overflow等开发者社区中也有很多人展开了讨论，本文做了相关整理。

# Share

在古希腊神话中，宗教里神叫做“almighty”，意思是万能的、无所不能的。但在古希腊神话中的神有时候连人也打不过，即便是万神之神的宙斯，他也不是想做什么就做什么，在他背后有三个命运女神在控制他。其实很多时候你得认命，人怎么能过得比较幸福，很重要的就是“你认这个命”。
