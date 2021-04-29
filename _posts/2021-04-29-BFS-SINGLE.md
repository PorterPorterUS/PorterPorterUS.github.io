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

####  BFS的格式

1. 定义queue+visited+4个方向

2.处理当前层: While loop 中循环size次, 每次poll出一个元素,  如果这个元素抵达终点直接返回


3.处理下一层:然后遍历4次，往上下左右四个方向进行遍历, check这4个方向是否合法,   r如果合法就添加到queue和visited中，不合法就continue。


4.合法与否：一般来说在边界之内，并且没有visited过，则为合法





#### 934.有两个岛屿, 在给定的二维二进制数组 A 中，存在两座岛。（岛是由四面相连的 1 形成的一个最大组。） 现在，我们可以将 0 变为 1，以使两座岛连接起来，变成一座岛。 返回必须翻转的 0 的最小数目。（可以保证答案至少是 1。）


解法：先用LC200的办法找到一个岛屿, 并且把这个岛屿设置为2，次遍历到一个1的时候，就把这个1对应的坐标加入到queue中。
第二步，对Queue进行BFS，每次pop出一个坐标，就对这个坐标的上下左右进行check, 如果某一个方向为1，那么直接return，如果没有1，那么把上下左右也设置为2，同时添加到queue中。做完一层BFS之后，step++



另外，本道题提供了一种满足条件，可以直接退出双层循环的办法, 可以使用一个flag，一旦条件满足，flag设置为false，同时在外面两层循环设置这个flag为true的时候才执行循环代码


```c
class Solution {
    public int shortestBridge(int[][] A) {
        paint(A); //paint one island with int 2
        Queue<int[]> q = new LinkedList<>(); //queue contains coordinates to do bfs
        boolean[][] visited = new boolean[A.length][A[0].length];
        
        for(int i = 0; i < A.length; i ++){//initialize queue with all coordinates with number 2
            for(int j = 0; j < A[0].length; j ++){
                if(A[i][j] == 2){
                    q.add(new int[]{i,j});
                    visited[i][j] = true;
                }
            }
        }
        
        int level = 0;
        while(!q.isEmpty()){//level order bfs
            int size = q.size();
            for(int i = 0; i < size; i ++){
                int[] cur = q.poll();
                int x = cur[0];
                int y = cur[1];
                if(A[x][y] == 1){//found, then return
                    return level - 1;
                }
                if(x - 1 >= 0 && !visited[x - 1][y]){
                    q.add(new int[]{x - 1, y});
                    visited[x - 1][y] = true;
                }
                if(x + 1 < A.length && !visited[x + 1][y]){
                    q.add(new int[]{x + 1, y});
                    visited[x + 1][y] = true;
                }
                if(y - 1 >= 0 && !visited[x][y - 1]){
                    q.add(new int[]{x, y - 1});
                    visited[x][y - 1] = true;
                }
                if(y + 1 < A[0].length && !visited[x][y + 1]){
                    q.add(new int[]{x, y + 1});
                    visited[x][y + 1] = true;
                }
            }
            level ++; //next level
        }
        return -1;
    }
    
    public void paint(int[][] A){//paint one island with int 2
        for(int i = 0; i < A.length; i ++){
            for(int j = 0; j < A[0].length; j ++){
                if(A[i][j] == 1){
                    dfs(i, j, A);
                    return;
                }
            }
        }
    }
    
    public void dfs(int x, int y, int[][] A){//helper function for paint function
        if(x < 0 || x > A.length - 1 || y < 0 || y > A[0].length - 1 || A[x][y] != 1){
            return;
        }
        A[x][y] = 2;
        dfs(x - 1, y, A);
        dfs(x + 1, y, A);
        dfs(x, y - 1, A);
        dfs(x, y + 1, A);
    }
}

```


#### 994.给一个m*n的矩阵,  1代表新鲜水果,  2代表腐烂, 每分钟腐烂水果的四周的新鲜水果会变的腐烂, 问水果全部腐烂的最快时间是多少，或者如果不能全部腐烂返回-1.


思路: 	 首先遍历一遍矩阵，把所有腐烂的水果加入到queue中，然后统计有多少个新鲜水果， 
开始BFS, BFS每轮对分钟数量+1，计算size，循环size次，
每次对当前位置计算上下左右四个位置，如果位置合法四个位置设置为腐烂，然后添加到queue中，同时新鲜水果数量-1. 
最后  return count_fresh == 0 ? count-1 : -1; 
一定注意：check不合法位置的逻辑是 出界+空水果+腐烂水果 另外注意: 如果初始的freshNum的数量为0，那么返回0


```c
class Solution {
    private final int[][] dirs = new int[][]{{0,1},{1,0},{-1,0},{0,-1}};
    public int orangesRotting(int[][] grid) {
    if(grid == null || grid.length == 0 || grid[0].length ==0) return -1;
   
    int rows = grid.length; int cols = grid[0].length;
    int freshNum = 0;
    int minutes = 0;
    Queue<int[]> queue = new LinkedList<>();
    for(int i = 0; i < rows; i++){
      for(int j = 0; j < cols; j++){
          if(grid[i][j] == 2){
             queue.offer(new int[]{i,j});
          }else if (grid[i][j]==1){
            freshNum++;
          }
      }
    }
      if(freshNum == 0) return 0;

     
      while(!queue.isEmpty()){
        minutes++;
        int size = queue.size();
        for(int i = 0; i < size; i++){
            int[] p = queue.poll();
            int x = p[0]; int y = p[1];
            for(int d=0;d<4;d++){
              int newX= x+dirs[d][0];
              int newY= y+dirs[d][1];
              if(!check(grid,newX,newY)){
                continue;
              }
              grid[newX][newY]=2;
              queue.offer(new int[]{newX,newY});
              freshNum--;
            }
        }
      }
      return freshNum==0?minutes-1:-1;
      
    }
  private boolean check(int[][] grid, int x,int y){
    if(x<0 || x==grid.length || y==grid[0].length || y<0 || grid[x][y]==2 || grid[x][y]==0) return false;
    return true;
  }
  
}
```




#### 752. 你有一个带有四个圆形拨轮的转盘锁。每个拨轮都有10个数字： '0', '1', '2', '3', '4', '5', '6', '7', '8', '9' 。每个拨轮可以自由旋转：例如把 '9' 变为  '0'，'0' 变为 '9' 。每#### 次旋转都只能旋转一个拨轮的一位数字。 锁的初始数字为 '0000' ，一个代表四个拨轮的数字的字符串。列表 deadends 包含了一组死亡数字，一旦拨轮的数字和列表里的任何一个元素相同，这个锁将会被永久锁#### 定，无法再被旋转。 字符串 target 代表可以解锁的数字，你需要给出最小的旋转次数，如果无论如何不能解锁，返回 -1。  



思路：属于单源最短路径问题, 
解题思路 定义queue+visited 
处理当前层： While loop 中循环size次, 每次poll一个String,  如果这个String等于终点直接返回，如果这个String是deadends则continue， 
处理下一层： 然后遍历4次，每次确定两个新的String(UP,DOWN), 对于每个String都check一下是否在visited中，如果符合法律，就加入到queue和visited中 
如果不在，则加入到visited和queue中


```c
class Solution {
    
    private static final String START = "0000";
    
    public int openLock(String[] deadends, String target) {
        if (target == null || target.length() == 0) return -1;
        Set<String> visited = new HashSet<>(Arrays.asList(deadends));
        Queue<String> queue = new LinkedList<>();
        int level = 0;
        queue.offer(START);
        
        while (!queue.isEmpty()) {
            int size = queue.size();
            for (int i = 0; i < size; i++) {
                String currentLock = queue.poll();
                if (!visited.add(currentLock)) continue;
                if (currentLock.equals(target)) return level;
                
                for (String nextLock : getNextStates(currentLock)) {
                    if (!visited.contains(nextLock)) queue.offer(nextLock);
                }
            }
            level++;
        }
        
        return -1;
    }
    
    private List<String> getNextStates(String lock) {
        List<String> locks = new LinkedList<>();
        char[] arr = lock.toCharArray();
        for (int i = 0; i < arr.length; i++) {
            char c = arr[i];
            arr[i] = c == '9' ? '0' : (char) (c + ((char) 1));
            locks.add(String.valueOf(arr));
            arr[i] = c == '0' ? '9' : (char) (c - ((char) 1));
            locks.add(String.valueOf(arr));
            arr[i] = c;
        }
        return locks;
    }
}
```




