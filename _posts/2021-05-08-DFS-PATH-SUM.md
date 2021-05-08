---
layout: post
author: Xusheng Ji
title: "dfs PathSum"
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

DFS pathSum 

### 112: Path Sum 
是否存在一条root-path的路径, 使得这条路径上的之和为TargetSum. 

#### 利用 DFS 找出从根节点到叶子节点的所有路径，只要有任意一条路径的 和 等于 sum，就返回 True。

#### 从根节点开始，每当遇到一个节点的时候，从目标值里扣除节点值，一直到叶子节点判断目标值是不是被扣完。

#### 其实，做为树的递归题目是非常有套路可循的，因为树有两个分支，所以在递归里也有两个分支，一般是通过 递归 A（||，&&）递归 B 来实现分支的。只要明白了这一点，递归函数就不会很难设计。


```java
class Solution {
    public boolean hasPathSum(TreeNode root, int targetSum) {
        if(root == null) return false;
        if(root.left==null && root.right == null){return targetSum == root.val;}
        return hasPathSum(root.left,targetSum-root.val) || hasPathSum(root.right,targetSum-root.val);
    }
}

```



### 113: Path Sum 2
返回所有的root-path 路径, 使得这条路径上的之和为TargetSum.

#### 1. 搜索所有路径问题, 需要使用Backtracking算法


####  2. 首先DFS函数的输入为(节点, targetSum, tmp, Res)


####  3. DFS内部逻辑，如果是个空的节点，或者是叶子结点但是value并不是等于targetSum,那么 直接return


####  4. 如果是叶子结点并且value等于targetSum,那么把当前叶子节点加入到tmp,然后res.add(tmp),然后再remove这个节点。


####  5. 先把当前节点加入到tmp, 进入两层DFS, 然后remove tmp node.


```java
class Solution {
    public List<List<Integer>> pathSum(TreeNode root, int targetSum) {
        List<List<Integer>> res = new ArrayList<>();
        List<Integer> tmp = new ArrayList<>();
        dfs(root,targetSum,tmp,res);
        return res;
    }
    private void dfs(TreeNode root,int targetSum,List<Integer> tmp, List<List<Integer>> res){
        if(root == null || (root.left==null && root.right== null && targetSum !=root.val)){
          return;
        }
        if(root.left==null && root.right== null && targetSum ==root.val){
            tmp.add(root.val);
            res.add(new ArrayList<>(tmp));
            tmp.remove(tmp.size()-1);
            return;
        }
      
        tmp.add(root.val);
        dfs(root.left,targetSum-root.val,tmp,res);
        dfs(root.right,targetSum-root.val,tmp,res);
        tmp.remove(tmp.size()-1);
    }
    
}

```

### 560 Subarray Sum Equals K

给定一个整数nums和一个整数k的数组，返回总和等于k的连续子数组的总数。

### 解法: 如果Subarray的总和为target, 有两种情况，一种是从第一个元素加到当前元素总和为target, 第二种情况是从某个位置开始加到当前元素总和为target。
### 建立一个hashmap, key is presum, value是这个preSum出现了多少次，每次遍历一个元素，产生一个preSum，就更新到hashmap中。
### 对于情况1 ： 所以在遍历数组的时候，如果preSum == targer, 则count++
### 对于情况2 ： hashmap中存放的是所有之前出现过的preSum, 如果当前位置的preSum减去某一个之前出现过的preSum的差值为targer, 则证明这段区间内targer出现了，则count+=hashMap[preSum-target];

```java
class Solution {
    public int subarraySum(int[] nums, int k) {
        HashMap<Integer,Integer> map = new HashMap<>();
        int pre = 0;
        int count =0 ;
        map.put(0,1);
        for(int i = 0 ; i < nums.length; i++){
            pre+=nums[i];
            if(map.containsKey(pre-k)){
              count+= map.get(pre-k);
            }
           map.put(pre,map.getOrDefault(pre,0)+1);
        }
      return count;
    }
}

```




### 113: Path Sum 3
返回所有的non - root-path 路径, 使得这条路径上的之和为TargetSum.
#### 和563题一样的解法, 只不过遍历数组变为了遍历Tree

```java
/**
 * Definition for a binary tree node.
 * public class TreeNode {
 *     int val;
 *     TreeNode left;
 *     TreeNode right;
 *     TreeNode() {}
 *     TreeNode(int val) { this.val = val; }
 *     TreeNode(int val, TreeNode left, TreeNode right) {
 *         this.val = val;
 *         this.left = left;
 *         this.right = right;
 *     }
 * }
 */
class Solution {
    HashMap<Integer,Integer> map = new HashMap<>();
    int k;
    int count = 0;
    public int pathSum(TreeNode root, int targetSum) {
        k = targetSum;
        dfs(root,0);
        return count;
    }
    private void dfs(TreeNode root, int preSum){
        if(root == null) return;
        preSum+=root.val;
        if(preSum == k) count++;
        if(map.containsKey(preSum-k)){
          count+=map.get(preSum-k);
        }
        map.put(preSum,map.getOrDefault(preSum,0)+1);
        dfs(root.left,preSum);
        dfs(root.right,preSum);
        map.put(preSum,map.get(preSum)-1);
    }
}

```







