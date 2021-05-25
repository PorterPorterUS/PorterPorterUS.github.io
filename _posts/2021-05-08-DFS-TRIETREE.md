---
layout: post
author: Xusheng Ji
title: "DFS trie tree"
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

DFS 字典树问题

### 208.mplement Trie (Prefix Tree)
### 1. 构建字典树,完成以下函数
#### 1.Trie() Initializes the trie object.
#### 2.void insert(String word) Inserts the string word into the trie
#### 3.boolean search(String word) Returns true if the string word is in the trie (i.e., was inserted before), and false otherwise.
#### 4.boolean startsWith(String prefix) Returns true if there is a previously inserted string word that has the prefix prefix, and false otherwise

### 2. 复杂度分析：
### 2.1 构建的复杂度： 构建TrieTree的时间复杂度为O(M), M是单词的长度。在遍历输入字符串的每一步，我们要不就是创建一个新节点，要不就是访问这个节点，所以时间复杂度为O(M)。构建TrieTree的空间复杂度为O(M), M是单词的长度。 在最坏情况下，我们每一步都要创建一个新的node，所以空间复杂度最坏为O(M).
### 2.2 对于搜索的复杂度： M是单词的长度,时间复杂度为O(M),空间复杂度为O(1).
 

```java
class Trie {
    class TrieNode{
      TrieNode[] children ;
      boolean word;
      public TrieNode(){
        children = new TrieNode[26];
        word = false;
      }
    }
  
    private TrieNode root; 
    /** Initialize your data structure here. */
    public Trie() {
        root = new TrieNode();
    }
    
    /** Inserts a word into the trie. */
    public void insert(String word) {
        TrieNode cur = root;
        for(char c: word.toCharArray()){
          if(cur.children[c-'a']==null){
            cur.children[c-'a'] = new TrieNode();
          }
          cur = cur.children[c-'a'];
        }
      cur.word = true;
        //return root;
    }
    
    /** Returns if the word is in the trie. */
    public boolean search(String word) {
        TrieNode cur = root;
        for(char c: word.toCharArray()){
            if(cur.children[c-'a'] == null){
              return false;
            }
          cur = cur.children[c-'a'];
        }
      return cur.word;
    }
    
    /** Returns if there is any word in the trie that starts with the given prefix. */
    public boolean startsWith(String prefix) {
        TrieNode cur = root;
        for(char c: prefix.toCharArray()){
            if(cur.children[c-'a'] == null){
              return false;
            }
          cur = cur.children[c-'a'];
        }
      return true;
    }
}

/**
 * Your Trie object will be instantiated and called as such:
 * Trie obj = new Trie();
 * obj.insert(word);
 * boolean param_2 = obj.search(word);
 * boolean param_3 = obj.startsWith(prefix);
 */

```



### 212 word search II
### 给一个字母板，和一堆字符串，找出那些出现在字母板中的字符串。

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

### 211. Design Add and Search Words Data Structure
### 1. 构建字典树，完成以下函数
#### WordDictionary() Initializes the object.
#### void addWord(word) Adds word to the data structure, it can be matched later.
#### bool search(word) Returns true if there is any string in the data structure that matches word or false otherwise. word may contain dots '.' where dots can be matched with any letter

### 2. 复杂度分析：
### 2.1 构建的复杂度： 构建TrieTree的时间复杂度为O(M), M是单词的长度。在遍历输入字符串的每一步，我们要不就是创建一个新节点，要不就是访问这个节点，所以时间复杂度为O(M)。构建TrieTree的空间复杂度为O(M), M是单词的长度。 在最坏情况下，我们每一步都要创建一个新的node，所以空间复杂度最坏为O(M).
### 2.2 对于不存在dot的String的搜索的复杂度： M是单词的长度,时间复杂度为O(M),空间复杂度为O(1).
### 2.3 对于存在dot的String的搜索的复杂度： M是单词的长度,假如最坏的情况是输入的字符串全部为"...........", 这样的话时间复杂度为O(26^M),因为针对每一个小点都要搜索26个孩子，空间复杂度为O(M), 因为每次遍历到一个字符，就要调用一次DFS，所以为O(M).


### 3.搜索部分的思路：
### 如果是正常字符串，没有dot, 假如字符串为"abcd", 每次遍历到一个字符的时候, 就去对应的字典树节点中查询是否存在
### 如果字符串中带有dot, 例如“abc.defg”, 那么遍历到"."的时候, 要查看"."对应的level中所有的节点，例如"."在"“abc.defg"中是第4个字符，那么就要查看c的第4层的所有儿子节点, 遍历每个儿子节点，如果这个儿子不为空，那么就把这个儿子节点和字符串“(儿子)defg”传入下一层DFS中。
```java
class WordDictionary {
    class TrieNode{
      boolean word;
      TrieNode[] children;
      public TrieNode(){
        word = false;
        children = new TrieNode[26];
      }
    }
    private TrieNode root;
    /** Initialize your data structure here. */
    public WordDictionary() {
        root = new TrieNode();
    }
    
    public void addWord(String word) {
        TrieNode cur = root;
        for(char c : word.toCharArray()){
            if(cur.children[c-'a']==null){
              cur.children[c-'a'] = new TrieNode();
            }
          cur = cur.children[c-'a'];
        }
        cur.word = true;
    }
    
    public boolean search(String word) {
        return dfs(word,root);
    }
    
    public boolean dfs(String word,TrieNode cur){
       for(int i = 0; i < word.length(); i++){
          char c = word.charAt(i);
          if(c == '.' || cur.children[c-'a']==null){
            if(c == '.'){
              for(TrieNode child:cur.children){
                if(child == null) continue;
                if(dfs(word.substring(i+1),child)){
                  return true;
                }
              }
            }
            return false;
          }else{
            cur = cur.children[c-'a'];
          }
       }
      return cur.word;
    }
}

/**
 * Your WordDictionary object will be instantiated and called as such:
 * WordDictionary obj = new WordDictionary();
 * obj.addWord(word);
 * boolean param_2 = obj.search(word);
 */

```

### 421. Maximum XOR of Two Numbers in an Array

### 给定一个整数数组nums，返回nums [i] XOR nums [j]的最大结果，其中0 <= i≤j <n。
这道题目还是蛮有意思的，用到前缀树的思想来将复杂度降低到 O(N)。
我们分两步来解决这个问题：

构建二进制前缀树
具体来说就是利用数的二进制表示，从高位到低位构建一棵树（因为只有0和1 两个值，所以是一棵二叉树），每个从根节点到叶子节点的路径都表示一个数。（构建的树看下图）

搜索前缀树
然后遍历数组中的数字，将每一个二进制位，在对应的层中找到一个异或的最大值，也就是：如果是1，找0的那条路径，如果是0，找1的那条路径。
这样搜索下来的路径就是这个数字和整个数组异或的最大值，看下图

![image](https://user-images.githubusercontent.com/60555283/119420773-48c22c80-bccb-11eb-97a5-444fed708433.png)

具体步骤是：
对于2， 二进制从高到低是 0，0，1，0

第一步：二进制位是0，我们到第四层去选择，有1，我们选择1这个节点，异或计算结果是1
第二步：二进制位是0，在第三层，上一步选择的节点没有为1的子节点，所以我们只能选择0，异或计算结果是0
第三步：二进制位是1，在第二层，上一步选择的节点的子节点下有0的节点，我们选择0，异或计算结果是1
第四部：二进制位是0，在第一层，上一步选择的节点的子节点下只有一个0，所以选择0，异或计算结果是0
所以我们异或的结果是1010， 十进制表示是10.

```java
class Solution {
     class Trie {
        Trie[] children;
        public Trie() {
            children = new Trie[2];
        }
    }
    
    public int findMaximumXOR(int[] nums) {
        if(nums == null || nums.length == 0) {
            return 0;
        }
        // Init Trie.
        Trie root = new Trie();
        for(int num: nums) {
            Trie curNode = root;
            for(int i = 31; i >= 0; i --) {
                int curBit = (num >>> i) & 1;
                if(curNode.children[curBit] == null) {
                    curNode.children[curBit] = new Trie();
                }
                curNode = curNode.children[curBit];
            }
        }
        int max = Integer.MIN_VALUE;
        for(int num: nums) {
            Trie curNode = root;
            int curSum = 0;
            for(int i = 31; i >= 0; i --) {
                int curBit = (num >>> i) & 1;
                if(curNode.children[curBit ^ 1] != null) {
                    curSum += (1 << i);
                    curNode = curNode.children[curBit ^ 1];
                }else {
                    curNode = curNode.children[curBit];
                }
            }
            max = Math.max(curSum, max);
        }
        return max;
    }
}

```



