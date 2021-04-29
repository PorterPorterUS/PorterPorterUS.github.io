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


#### 695: 最大岛屿的面积
(一个二维矩阵, 有1和0, 找出最多连续的1的个数) 碰到一个1就开始DFS，内部将此位置设置为0，然后上下左右四个方向深度搜索, 不用使用BackTracking,  

#在200题的基础之上加入了res+=的概念
 ```c
 
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int res = 0;
        for(int i = 0; i < grid.length; i++){
          for(int j = 0; j < grid[0].length; j++){
              if(grid[i][j] == 1){
                res = Math.max(res,dfs(i,j,grid));
              }
              
          }
        }
        return res;
    }
  
  
    private int dfs(int i, int j, int[][] grid){
      if(i < 0 || i>=grid.length || j<0 || j>=grid[0].length || grid[i][j] == 0) return 0;
      int res = 1;
      grid[i][j] = 0;
      res+= dfs(i+1,j,grid);
      res+= dfs(i,j+1,grid);
      res+= dfs(i-1,j,grid);
      res+= dfs(i,j-1,grid);
      return res;
    }
}
 ```

#### 694: 不同岛屿的面积
一个二维矩阵, 有1和0, 找出最多连续的1的个数) 碰到一个1就开始DFS，内部将此位置设置为0，然后上下左右四个方向深度搜索,不用使用BackTracking,      
#### 在200题基础上加了字符的参数
最后一步，要加一个sb.append(b), 因为

```c
1 1              1  1   1
  1 1               1


```
会相同

不加sb.append(b)的话, 两者的结果都为RRDR,但是如果加了sb.append(b)的话，在返回的路上加了B, 第一个结果为RRDR, 第二个结果为RRDBR


```c
 class Solution {
    public int numDistinctIslands(int[][] grid) {
        int res = 0;
        HashSet<String> set = new HashSet<>();
        for(int i =0 ; i < grid.length; i++){
          for(int j = 0; j < grid[0].length; j++){
              if(grid[i][j] == 1){
                StringBuffer sb = new StringBuffer();
                dfs(i,j,grid,sb,"S");
                set.add(sb.toString());
              }
          }
        }
        return set.size();
    }
  
    private void dfs(int i, int j, int[][] grid, StringBuffer sb, String s){
        if(i<0 || j<0 || i>=grid.length || j>=grid[0].length || grid[i][j] == 0) return;
        grid[i][j] = 0;
        sb.append(s);
        dfs(i-1,j,grid,sb,"U");
        dfs(i+1,j,grid,sb,"D");
        dfs(i,j-1,grid,sb,"L");
        dfs(i,j+1,grid,sb,"R");
        sb.append("B");
    }
}
 ```

#### 1254: 孤立岛屿的个数
0是陆地，1是水, 求封闭岛屿的个数，封闭的岛屿是一个堆0连接起来，并且周围都是1，上下左右都是1

思路: 如果当前位置是0就进行DFS遍历，如果当前位置已经是1了,那么就返回1，不要进行DFS遍历，如果不是一个封闭的岛屿,那么通过某个0进行DFS遍历,一定能遍历到一个位于边界位置的0


```c
class Solution {
    public int closedIsland(int[][] grid) {
        int res = 0;
        for(int i =0; i < grid.length;i++){
          for(int j = 0; j < grid[0].length; j++){
              if(grid[i][j] == 0){
                 if(dfs(i,j,grid) == true){
                res++;
              }
              }
             
          }
        }
        return res;
    }
  
    private boolean dfs(int i, int j,int[][] grid){
        if(i<0 || j<0 || i>= grid.length || j>=grid[0].length) return false;
        if(grid[i][j] == 1) return true;
        grid[i][j] = 1;
        boolean s1 = dfs(i+1,j,grid);
         boolean s2 = dfs(i-1,j,grid);
       boolean s3 = dfs(i,j+1,grid);
       boolean s4 = dfs(i,j-1,grid);
      return s1 && s2 && s3 && s4;
    }
}
 ```









