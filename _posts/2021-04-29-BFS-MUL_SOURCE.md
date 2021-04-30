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


```c++

 class Solution {
    private final int[][] dirs= new int[][]{ {2,1},{-2,-1},{2,-1},{-2,1},{1,2},{-1,-2},{1,-2},{-1,2} };
    public int minKnightMoves(int x, int y) {
       x = Math.abs(x); y = Math.abs(y);
       int res = 0;
       Queue<int[]> queue = new LinkedList<>();
       queue.offer(new int[]{0,0});
                   
       HashSet<String> visited = new HashSet<>();
       visited.add("0,0");
       while(!queue.isEmpty()){
           int size = queue.size();
           for(int i = 0; i < size; i++){
              int[] tmp = queue.remove();
              int xOld = tmp[0];
              int yOld = tmp[1];
             // final dst 
             if(xOld == x && yOld ==y){
               return res;
             }
              
              for(int[] d:dirs){
                 //int xNew = xOld+dirs[d][0];
                 //int yNew = xOld+dirs[d][1];
                int newX = xOld + d[0];
                int newY = yOld + d[1];
                 if(!visited.contains(newX+","+newY) && newX>=-1 && newY>=-1){
                   queue.add(new int[]{newX,newY} );
                   visited.add(newX+","+newY);
                 }
              }  

           }
          res++;
       }
      return -1;
      
    }
}
          
      
```



#### 542.给定一个由 0 和 1 组成的矩阵，找出每个元素到最近的 0 的距离。 两个相邻元素间的距离为 1 




思路:  求多源的最短路径，通常用BFS来解决。 
这道题不可以使用DFS岛屿问题来解决， 因为存在找到某些点，这些点在上一轮的DFS中已经搜索过了，所以如果用岛屿DFS来解决，会有重复计算。 
因此要用BFS解决。 但是不可以在每个1上面做BFS，那样的话，BFS的次数等于1的个数，我们需要只做一次DFS就可以了 
所以要一次性的把所有的0全部加入到queue中，然后visited 设置为true。 然后进行一次BFS，每次拿出一个点，都对他上下左右的check，如果这4个上下左右的点在范围内并且之前没有visited，那么update 这个新点的距离
`=res[newi][newj]=res[i][j]+1; `
然后加入到queue中，然后设置一下visited

```java
class Solution {
    public int[][] dirs = new int[][]{ {0,1},{1,0},{0,-1},{-1,0} };
    public int[][] updateMatrix(int[][] matrix) {
        int rows = matrix.length;
        int cols = matrix[0].length;
        int[][] res = new int[rows][cols];
        Queue<int[]> queue = new LinkedList<>();
        boolean[][] visited = new boolean[rows][cols];  
      
        for(int i = 0; i < rows ; i++){
          for(int j = 0; j < cols ; j++){
            if(matrix[i][j]==0){
              queue.offer(new int[]{i,j});
              visited[i][j]=true;
            }
          }
        }
      
        while(!queue.isEmpty()){
          int[] cell = queue.poll();
          int i = cell[0]; int j = cell[1];
          for(int d =0;d<4;d++){
            int newi = i+dirs[d][0];
            int newj = j+dirs[d][1];
            if(newi>=0 && newi<rows && newj>=0 && newj<cols && !visited[newi][newj]){
              res[newi][newj]=res[i][j]+1;
              queue.offer(new int[]{newi,newj} );
              visited[newi][newj]= true;
            }
          }
        }
        return res;
    }
}

```




#### 286. -1是障碍物, 0是gate, INF是空白位置。求的每个空白处的最近的gate的距离。


思路： 此类问题是多源的最短路径问题,  和1091不同, 1091是单源最短路径. 单源路径每次都是加入起点，因为已经知道起点了，然后while loop内遍历size次，每一次之内又重新遍历8个方向 
但是多源路径，一开始需要把每个gate全部加入到queue中，然后while loop之内没有for size, 直接pop出一个，然后遍历4个方向，每次check一个方向是否合法，如果合法，更新当前位置(当前位置的old value + 1 =>new value, 这里不用担心同一个位置被多个gate所更新，因为永远是离着它最近的gate先把这个empty先更新掉)，然后加入到队列中。


```java
class Solution {
    private static final int emp = Integer.MAX_VALUE;
    private static final int gate = 0;
    private static final int wall = -1;
    private static final int[][] dirs = new int[][]{{1,0},{0,1},{-1,0},{0,-1}};
    public void wallsAndGates(int[][] rooms) {
        //corner case
        if(rooms==null || rooms.length ==0 || rooms[0].length == 0) return;
        int rows = rooms.length;
        int cols = rooms[0].length;
        Queue<int[]> queue = new LinkedList<>();
        for(int i = 0; i < rows; i++){
          for(int j = 0; j < cols; j++){
            if(rooms[i][j] == gate){
              queue.add(new int[]{i,j});
            }
          }
        }
      while(!queue.isEmpty()){
        int[] cur = queue.poll();
        int x = cur[0]; int y = cur[1];
        for(int d = 0 ; d<4;d++){
          int newX = x+dirs[d][0];
          int newY = y+dirs[d][1];
          if(newX<0 || newX==rows || newY<0 || newY==cols || rooms[newX][newY] !=emp) continue;
          rooms[newX][newY] = rooms[x][y]+1;
          queue.offer(new int[]{newX,newY});
        }
        
      } 
    }
}

```












