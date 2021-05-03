---
layout: post
author: Xusheng Ji
title: "Backtracking"
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








