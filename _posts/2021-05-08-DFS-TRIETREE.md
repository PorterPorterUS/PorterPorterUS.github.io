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
### 构建字典树,完成以下函数
#### 1.Trie() Initializes the trie object.
#### 2.void insert(String word) Inserts the string word into the trie
#### 3.boolean search(String word) Returns true if the string word is in the trie (i.e., was inserted before), and false otherwise.
#### 4.boolean startsWith(String prefix) Returns true if there is a previously inserted string word that has the prefix prefix, and false otherwise


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
### 构建字典树，完成以下函数
#### WordDictionary() Initializes the object.
#### void addWord(word) Adds word to the data structure, it can be matched later.
#### bool search(word) Returns true if there is any string in the data structure that matches word or false otherwise. word may contain dots '.' where dots can be matched with any letter










