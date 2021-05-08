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

#### 搜索所有路径问题, 需要使用Backtracking算法


####  首先DFS函数的输入为(节点, targetSum, tmp, Res)


####  DFS内部逻辑，如果是个空的节点，或者是叶子结点但是value并不是等于targetSum,那么 直接return


####  如果是叶子结点并且value等于targetSum,那么把当前叶子节点加入到tmp,然后res.add(tmp),然后再remove这个节点。


####  先把当前节点加入到tmp, 进入两层DFS, 然后remove tmp node.





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











