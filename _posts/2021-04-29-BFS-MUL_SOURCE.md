---
layout: post
author: Xusheng Ji
title: "Bfs multiple sources"
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


BFS 多源最短路径问题

####  BFS的格式

#### 1197.给定一个无限大的棋盘, 骑士开始的位置在(0,0),  有8个方向可以移动，类似于中国象棋中的相的移动方式，然后求从(0,0)到目标地点最少需要移动多少步




思路:  首先因为棋盘是对称的, 所以把目标位置统一转为正数, 然后创建visited, queue. 
开始BFS，获取size, 遍历size次, 每一次更新8个位置，如果这个位置没有visited并且newX>=-1&&newY>=-1,(这里没用0作判断条件是因为特殊case(1,1).
如果判断条件为0则输出为4，但是判断条件为-1则输出为2), 就加入到队列中，同时加入到visited中. 
每次一个BFS结束后，res++,如果中途碰到了目标位置return res,否则return -1.


```java
class Solution {
    int[][] dd = {{2, 1}, {1, 2}, {-1, 2}, {-2, 1}, {-2, -1}, {-1, -2}, {1, -2}, {2, -1}}; 
    public int minKnightMoves(int x, int y) {
        Set<String> set = new HashSet<>();
        Queue<int[]> q = new LinkedList<>();
        x = Math.abs(x);
        y = Math.abs(y);
        q.add(new int[2]);
        set.add("0" + " " + "0");
        int step = 0;
        while (!q.isEmpty()) {
            int size = q.size();
            for (int i = 0; i < size; i++) {
                int[] temp = q.poll();
                if (temp[0] == x && temp[1] == y) {
                    return step;
                }
                for (int j = 0; j < 8; j++) {
                    int nx = temp[0] + dd[j][0];
                    int ny = temp[1] + dd[j][1];
                    String key = nx + " " + ny;
                    if (nx < 0 || ny < 0 ||set.contains(key)) {
                        continue;
                    }
                    set.add(key);
                    q.add(new int[]{nx, ny});
                }
            }
            step++;
        }
        return -1;
    }
}
```







