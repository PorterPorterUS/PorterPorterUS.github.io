---
layout: post
author: Xusheng Ji
title: "DFS island"
tags: algorithm leetcode
categories: DFS
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

### DFS的格式
### DFS函数的参数：1.图,也就是grid 2. 当前图所在的位置 3.字，也就是word 4.当前word所在的位置
### DFS函数内部：
### 1.如果word出界，则返回true 
### 2.如果i,j在grid中出界 或者 当前字符不等于word的第n个字符 或者 当前字符为* 
### 3.保存临时的字符，并把当前字符设置为* 
### 4.上下左右四个方向遍历 
### 5.遍历完毕后再把临时字符设置回来
`end`
`end`
`end`
`end`
`end`
`end`
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
### 复杂度分析: 时间复杂度：O（M×N）O（M×N），其中MM是行数，NN是列数。
### 复杂度分析: 空间复杂度：最糟糕的情况是O（M×N）O（M×N），如果网格图填充了DFS深达M×N的陆地。
`end`
`end`


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
### 复杂度分析: 时间复杂度和200题完全一样
`end`
`end`

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
### 复杂度分析: 时间复杂度和200题完全一样
`end`
`end`

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
### 复杂度分析: 和200题完全一样


#### 130: 给定一个二维的矩阵，包含 'X' 和 'O'（字母 O）。  把所有不在边界上的O并且没有与边界O相连接的那些O 全部替换为X

思路：这道题我们拿到基本就可以确定是图的 dfs、bfs 遍历的题目了。
题目中解释说被包围的区间不会存在于边界上，所以我们会想到边界上的 OO 要特殊处理，只要把边界上的 OO 特殊处理了，
那么剩下的 OO 替换成 XX 就可以了。问题转化为，如何寻找和边界联通的 OO，我们需要考虑如下情况。

```c
X X X X
X O O X
X X O X
X O O X

```
这时候的 OO 是不做替换的。因为和边界是连通的。为了记录这种状态，我们把这种情况下的 OO 换成 # 作为占位符，待搜索结束之后，遇到 OO 替换为 XX（和边界不连通的 OO）；遇到 #，替换回 $O(和边界连通的 OO


```c
class Solution {
    public void solve(char[][] board) {
        for(int i = 0 ; i< board.length; i++){
          for(int j = 0; j < board[0].length; j++){
            if( (i == board.length-1 || j==board[0].length-1 || i==0 || j==0) && board[i][j] == 'O'){
              dfs(i,j,board);
            }
          }
        }
      
        for(int i = 0; i < board.length; i++){
          for(int j = 0; j < board[0].length; j++){
              if(board[i][j] == 'O'){
                board[i][j] = 'X';
              }else if(board[i][j] == '#'){
                 board[i][j] = 'O';
              }
          }
        }
      
    }
  
    private void dfs(int i, int j, char[][] board){
      if(i<0 || j<0 || i>= board.length || j>=board[0].length ||board[i][j] =='X'||board[i][j]=='#') return;
      board[i][j] = '#';
      dfs(i+1,j,board);
      dfs(i-1,j,board);
      dfs(i,j+1,board);
      dfs(i,j-1,board);
    }
}


```

### 复杂度分析: 和200题完全一样
`end` `end` `end` `end` `end` `end`


### 79 Word Search 
### 题目意思, 给一个字符串，给一个字母板, 求问字符串是否出现在字母板中
### 解法：利用岛屿问题的DFS模版，如果某个字母板位置的字符等于字符串中的第一个字符，并且DFS返回true，则证明找到这个字符串了
### DFS的参数:1.图,也就是grid 2. 当前图所在的位置 3.字，也就是word 4.当前word所在的位置
### DFS的内部逻辑：首先判断index>=String.length则证明已经搜索过每个字符串了于是就可以return true ++++++ 然后判断出界, 然后存储临时变量+++然后四个方向DFS
### 复杂度分析：空间复杂度为O(L),L是word的长度
### 复杂度分析：时间复杂度为O(M * N * 4^L) where M*N is the size of the board and we have 4^L for each cell because of the recursion. 
```java
class Solution {
    public boolean exist(char[][] board, String word) {
        int rows = board.length;
        int cols = board[0].length;
        for(int i = 0; i < rows; i++){
          for(int j = 0; j < cols; j++){
             if(board[i][j] == word.charAt(0) && dfs(board,i,j,word,0)){
                return true;
             }
          }
        }
        return false;
    }
  
    private boolean dfs(char[][] grid,int i, int j, String word, int index){
      if(index>=word.length()) return true;
      if(i<0 || i== grid.length || j<0 || j==grid[0].length || word.charAt(index)!=grid[i][j] || grid[i][j] == '*') return false;
      char tmp = grid[i][j];
      grid[i][j] = '*';
      if(dfs(grid,i+1,j,word,index+1) ||dfs(grid,i,j+1,word,index+1)||dfs(grid,i-1,j,word,index+1)||dfs(grid,i,j-1,word,index+1) ){
        return true;
      }
      
      grid[i][j] = tmp;
      return false;
    }
}

```


### 212 Word Search II 
### 题目意思, 给N个字符串，给一个字母板, 求问N个字符串有哪些出现在字母板中，如果出现了请把他们都打印出来
### 解法: 凡是查看一个字符串是否出现在一堆字符串集群中的问题，请使用字典树TrieTree. 可以把着N个字符串建立为TrieTree,然后在字母板中的每次DFS遍历，都把当前字符串放入到TrieTree中查看是否存在。
### 建立字典树的办法: 首先创立一个空的头节点, 再设置一个指针，每次遍历一个新的字符串的时候，都先把指针指向头节点，然后每次遍历一个字符串的字符，都查看指针.children[c-'a']是否存在，如果存在就继续下一个字符，如果不存在就建立一个空节点，然后继续下一个字符，最终遍历完整个字符串的话，就把这个节点的node.StringValue = String.
### DFS内部逻辑：首先判断出界++然后判断字典树中的元素是否存在++然后存储临时变量+++然后DFS四个方向
### DFS参数：1.grid 2.grid的位置 3.res结果集合 4.TrieNode root
### 时间复杂度分析：m*n是grid的大小，WL是所有单词的平均长度。For naive approach, runtime is O(m * n * num_words * min(4^wl, m * n)). For trie approach, runtime is O(m * n * min(4^wl, m * n) + wl * num_words). WL*num_words是建立TrieTree的时间复杂度， min(4^wl, m * n)是因为有可能M*N会比4^WL要大，而DFS最多是遍历M*N次

```java
class Solution {
    class TrieNode{
      String word;
      TrieNode[] children;
      public TrieNode(){
        children = new TrieNode[26];
        word = null;
      }
    }
    private TrieNode root = new TrieNode();
     
  
    private TrieNode buildTree(String[] words){
      //TrieNode root = new TrieNode();
      for(String word:words){
          TrieNode cur =  root;
          for(char c: word.toCharArray()){
            if(cur.children[c-'a']==null){
              cur.children[c-'a'] = new TrieNode();
            }
            cur = cur.children[c-'a'];
          }
        cur.word = word;
      }
        return root;
    }
    public List<String> findWords(char[][] board, String[] words) {
        int rows = board.length;
        int cols = board[0].length;
        List<String> res = new ArrayList<>();
        TrieNode root = buildTree(words);
        for(int i = 0 ; i < rows; i++){
          for(int j = 0; j < cols; j++){
              dfs(i,j,board,root,res);
          }
        }
        return res;
    }
    private void dfs(int i, int j, char[][] board, TrieNode root,List<String> res){
        if(i<0 || i==board.length || j<0 || j==board[0].length || board[i][j] == '*'){
          return;
        }
        char tmp = board[i][j];
        if(root.children[tmp-'a']==null) return;
        root = root.children[tmp-'a'];
        if(root.word!=null){
          res.add(root.word);
          root.word = null;
        }
       board[i][j] = '*';
      dfs(i+1,j,board,root,res);
      dfs(i-1,j,board,root,res);
      dfs(i,j+1,board,root,res);
      dfs(i,j-1,board,root,res);
        
       board[i][j] = tmp; 
    }
    
}
```

 

