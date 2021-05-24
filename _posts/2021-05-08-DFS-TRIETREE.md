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
### 如果字符串中带有dot, 例如“abc.defg”, 那么遍历到"."的时候, 要查看c的所有26个孩子，如果某个孩子不为空，例如字典树中有"c->a->d",则访问到a，然后把defg当作输入字符串再次传入DFS
```java
class WordDictionary {
    class TrieNode{
      boolean word ;
      TrieNode[] children;
      public TrieNode(){
        word = false;
        children = new TrieNode[26];
      }
    }
    TrieNode root;
    /** Initialize your data structure here. */
    public WordDictionary() {
        root = new TrieNode();
    }
    
    public void addWord(String word) {
        TrieNode cur = root;
        for(char c: word.toCharArray()){
            if(cur.children[c-'a'] == null){
                cur.children[c-'a'] =new TrieNode();
            }
          cur = cur.children[c-'a'];
        }
      cur.word = true;
    }
    
    public boolean search(String word) {
      return dfs(word,root);  
      
    }
  
    private boolean dfs(String word,TrieNode root){
       TrieNode cur = root;
       for(int i = 0 ; i < word.length(); i++){
         if(word.charAt(i)!='.'&&cur.children[word.charAt(i)-'a']==null) return false;
         if(word.charAt(i)=='.'){
           for(int j = 0; j < 26; j++){
             if(cur.children[j]!=null){
               if(dfs(word.substring(i+1),cur.children[j])){
                 return true;
               }
             }
           }
           return false;
         }
         cur = cur.children[word.charAt(i)-'a'];
         
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




