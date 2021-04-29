---
layout: post
author: Xusheng Ji
title: "Bfs single source"
tags: algorithm leetcode
---

{% include lib/mathjax.html %}


<script type="text/javascript" async
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.5/MathJax.js?config=TeX-MML-AM_CHTML">
</script>

<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    extensions: [
      "MathMenu.js",
      "MathZoom.js",
      "AssistiveMML.js",
      "a11y/accessibility-menu.js"
    ],
    jax: ["input/TeX", "output/CommonHTML"],
    TeX: {
      extensions: [
        "AMSmath.js",
        "AMSsymbols.js",
        "noErrors.js",
        "noUndefined.js",
      ]
    }
  });
</script>

BFS 单源最短路径问题

934.有两个岛屿, 在给定的二维二进制数组 A 中，存在两座岛。（岛是由四面相连的 1 形成的一个最大组。） 现在，我们可以将 0 变为 1，以使两座岛连接起来，变成一座岛。 返回必须翻转的 0 的最小数目。（可以保证答案至少是 1。）





另外，本道题提供了一种满足条件，可以直接退出双层循环的办法, 可以使用一个flag，一旦条件满足，flag设置为false，同时在外面两层循环设置这个flag为true的时候才执行循环代码


