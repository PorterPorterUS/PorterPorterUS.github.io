---
layout: post
author: Xusheng Ji
title: "Backtracking"
tags: algorithm leetcode
categories: BFS
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



Backtracking   

#### Backtracking的套路:

1.首先外层函数需要定义一个二维的结果集合(res),定义一个一维的临时结果集合(tmp).


2.DFS需要是void类型的, 然后输入参数为4个(1.数组 2.起始位置 3.临时结果集合tmp 4.最终结果集合 res)


3.函数内部, 首先判断退出条件，然后把临时结果集添加到最终结果集中, 最后退出




#### 78:Subset 1: 给定一个数组,返回所有的子集.(输入数据没有重复元素)

```c

LEVEL 1: 1   2   3

LEVEL 2: 2   3   

LEVEL 3: 3   


每次循环到某一层直接加入到最终结果集合
```

首先临时结果集合加入到最终结果集没有限定条件, 然后start Index 为i+1


```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> tmp = new ArrayList<>();
        dfs(nums,0,tmp,res);
        return res;
      
    }
  
    private void dfs(int[] nums, int index, List<Integer> tmp, List<List<Integer>> res){
      res.add(new ArrayList<>(tmp));
      for(int i = index; i < nums.length;i++){
          tmp.add(nums[i]);
          dfs(nums,i+1,tmp,res);
          tmp.remove(tmp.size()-1);
      }
      
    }
}
```


#### 90:Subset 2: 给定一个数组,返回所有的子集.(输入数据存在重复元素)

```java
class Solution {
    public List<List<Integer>> subsets(int[] nums) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> tmp = new ArrayList<>();
        dfs(nums,0,tmp,res);
        return res;
      
    }
  
    private void dfs(int[] nums, int index, List<Integer> tmp, List<List<Integer>> res){
      res.add(new ArrayList<>(tmp));
      for(int i = index; i < nums.length;i++){
          if(i > index && nums[i] == nums[i-1]) continue; // skip duplicates
          tmp.add(nums[i]);
          dfs(nums,i+1,tmp,res);
          tmp.remove(tmp.size()-1);
      }
      
    }
}
```



#### 46:Permutation: 给定一个数组,返回所有的排列组合.(输入数据不存在重复元素)


```c

LEVEL 1: 1                2               3

LEVEL 2: 123             123            123   

LEVEL 3: 123123123   123123123       123123123   


每次循环到某一层直接加入到最终结果集合
```


首先tmp加入到res的条件为size == nums.length, 然后start Index 为0


```java
class Solution {
    public List<List<Integer>> permute(int[] nums) {
        List<List<Integer>> res=  new ArrayList<>();
        List<Integer> tmp = new ArrayList<>();
        dfs(nums,0,tmp,res);
        return res;
    }
  
    private void dfs(int[] nums,int index,List<Integer> tmp, List<List<Integer>> res){
      if(tmp.size()==nums.length){
        res.add(new ArrayList<>(tmp));
        return;
      }
      
      for(int i = 0 ; i < nums.length; i++){
        if(tmp.contains(nums[i])) continue;
        tmp.add(nums[i]);
        dfs(nums,0,tmp,res);
        tmp.remove(tmp.size()-1);
      }
      
    }
}
```


#### 47:Permutation 2: 给定一个数组,返回所有的排列组合.(输入数据存在重复元素)


```java

public class Solution {
    public List<List<Integer>> permuteUnique(int[] nums) {
        List<List<Integer>> res = new ArrayList<List<Integer>>();
        if(nums==null || nums.length==0) return res;
        boolean[] used = new boolean[nums.length];
        List<Integer> list = new ArrayList<Integer>();
        Arrays.sort(nums);
        dfs(nums, used, list, res);
        return res;
    }

    public void dfs(int[] nums, boolean[] used, List<Integer> list, List<List<Integer>> res){
        if(list.size()==nums.length){
            res.add(new ArrayList<Integer>(list));
            return;
        }
        for(int i=0;i<nums.length;i++){
            if(used[i]) continue;
            if(i>0 &&nums[i-1]==nums[i] && !used[i-1]) continue;
            used[i]=true;
            list.add(nums[i]);
            dfs(nums,used,list,res);
            used[i]=false;
            list.remove(list.size()-1);
        }
    }
}
```



#### 39 Combination Sum, 找出那些能凑出targetSum的子数组. 


```c

LEVEL 1: 1   2   3

LEVEL 2: 1   2   3   

LEVEL 3: 1   2   3   


每次循环到某一层直接加入到最终结果集合
```
结束条件为 target == 0, index为当前的i. 

```java 

class Solution {
    public List<List<Integer>> combinationSum(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> tmp = new ArrayList<>();
        dfs(candidates,0,tmp,res,target);
        return res;      

    }
  
    private void dfs(int[] nums, int index, List<Integer> tmp, List<List<Integer>> res,int target){
      if(target==0){
        //tmp.add(nums[index]);
        res.add(new ArrayList<>(tmp));
        return;
      }
      
      for(int i = index; i < nums.length; i++){
        if(target>=nums[i]){
          tmp.add(nums[i]);
          dfs(nums,i,tmp,res,target-nums[i]);
          tmp.remove(tmp.size()-1);
        }
        
      }
      
    }
}
```


#### 40 Combination Sum 2, 找出那些能凑出targetSum的子数组. 


```c

LEVEL 1: 1   2   3

LEVEL 2: 1   2   3   

LEVEL 3: 1   2   3   


每次循环到某一层直接加入到最终结果集合
```
结束条件为 target == 0, index为当前的i. 

```java 

class Solution {
    public List<List<Integer>> combinationSum2(int[] candidates, int target) {
        List<List<Integer>> res = new ArrayList<>();
        Arrays.sort(candidates);
        bt(candidates,res,new ArrayList<>(),0,target);
        return res;
    }
  private void bt(int[] nums,List<List<Integer>> res,List<Integer> tmp,int index,int target){
      if(target==0){
        res.add(new ArrayList<>(tmp));
        return;
      }
      
      for(int i = index; i < nums.length; i++){
          if(target<nums[i] || (i>index && nums[i] == nums[i-1])) continue;
          tmp.add(nums[i]);
          bt(nums,res,tmp,i+1,target-nums[i]);
          tmp.remove(tmp.size()-1);
      }
      
    }
}
```


### 1239.Maximum Length of a Concatenated String with Unique Characters
### 给定一个字符串数组arr。 字符串s是具有唯一字符的arr子序列的串联。返回s的最大可能长度。
### 思路类似于 subSetI

```java
class Solution {
   private int max = 0;
    public int maxLength(List<String> arr) {
        dfs(arr, 0, "");
        return max;
    }

    public void dfs(List<String> arr, int start, String str) {
        
        if (isUnique(str)){
          max = Math.max(max, str.length());
        } 
        if (start == arr.size() || !isUnique(str))  return;
        for(int i = start ; i < arr.size(); i++){
            dfs(arr,i+1,str+arr.get(i));
        }
        
        
    }
  
  
    private boolean isUnique(String s){
        int[] hash = new int[26];
        for(char c: s.toCharArray()){
          hash[c-'a']++;
        }
        for(int i = 0; i < hash.length; i++){
            if(hash[i]>1) return false;
        }
      return true;
    }
  }


```






