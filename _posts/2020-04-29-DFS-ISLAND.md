---
layout: post
author: Xusheng Ji
title: "dfs island"
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

DFS 岛屿问题 

#### 200: 返回岛屿的数量
(一个二维矩阵, 有1和0, 找出有多少个连续的1) 
 碰到一个1就开始DFS，内部将此位置设置为0，然后上下左右四个方向深度搜索, 不使用BackTracking。
 ```c
 
 class Solution {
    public int numIslands(char[][] grid) {
        int rows = grid.length;
        int cols = grid[0].length;
        int res = 0;
        for(int i = 0 ; i < rows; i++){
          for(int j =0 ; j < cols; j++){
              if(grid[i][j] == '1'){
                dfs(i,j,grid);
                res++;
              }
              
              
          }
        }
        return res;
    }
    private void dfs(int i, int j, char[][] grid){
      if(i<0 || i>= grid.length || j<0 || j>=grid[0].length || grid[i][j]=='0') return;
      grid[i][j] = '0';
      dfs(i+1,j,grid);
      dfs(i,j+1,grid);
      dfs(i-1,j,grid);
      dfs(i,j-1,grid);
      
    }
}
 
 
 ```






