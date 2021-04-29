---
layout: post
author: Xusheng Ji
title: "BFS single search"
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



另外，本道题提供了一种满足条件，可以直接退出双层循环的办法, 可以使用一个flag，一
旦条件满足，flag设置为false，同时在外面两层循环设置这个flag为true的时候才执行循环代码



