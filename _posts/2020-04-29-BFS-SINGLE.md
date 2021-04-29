---
layout: post
author: Xusheng Ji
title: "BFS single"
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



BFS 单源最短路径 

#### 934: 有两个岛屿, 在给定的二维二进制数组 A 中，存在两座岛。（岛是由四面相连的 1 形成的一个最大组。） 

现在，我们可以将 0 变为 1，以使两座岛连接起来，变成一座岛。 返回必须翻转的 0 的最小数目。（可以保证答案至少是 1。）


解法：先用LC200的办法找到一个岛屿, 并且把这个岛屿设置为2，次遍历到一个1的时候，就把这个1对应的坐标加入到queue中。
第二步，对Queue进行BFS，每次pop出一个坐标，就对这个坐标的上下左右进行check, 如果某一个方向为1，那么直接return，
如果没有1，那么把上下左右也设置为2，同时添加到queue中。做完一层BFS之后，step++.


```c
class Solution {
    private Queue<int[]> q = new LinkedList<>();
    public int shortestBridge(int[][] A) {
        int[][] dirs = new int[][]{{0,1},{0,-1},{1,0},{-1,0}};
        int rows = A.length;
        int cols = A[0].length;
        int res = 0;
        boolean found = false;
        
        for(int i =0; i < rows && !found;i++){
          for(int j = 0; j < cols&&!found;j++){
            if(A[i][j]==1){
              dfs(i,j,rows,cols,A);
              found = true;
            }
          }
        }
      while(!q.isEmpty()){
        int size = q.size();
        for(int i = 0 ; i < size; i++){
          int[] cur = q.poll();
          for(int d=0; d<4;d++){
            int newX = dirs[d][0]+cur[0];
            int newY = dirs[d][1]+cur[1];
            if(newX<0||newX==rows||newY<0||newY==cols||A[newX][newY]==2) continue;
            if(A[newX][newY] == 1) return res;
            A[newX][newY] =2;
            q.offer(new int[]{newX,newY});
          }
          
        }
        res++;
      }
      return -1;
      
    }
  
    private void dfs(int i, int j,int rows,int cols,int[][] A){
      if(i==rows || i<0 || j<0 || j==cols || A[i][j]!=1) return;
      A[i][j] = 2;
      q.offer(new int[]{i,j});
      dfs(i+1,j,rows,cols,A);
      dfs(i-1,j,rows,cols,A);
      dfs(i,j+1,rows,cols,A);
      dfs(i,j-1,rows,cols,A);
      
    }
}
```






