---
layout: post
author: Xusheng Ji
title: "Sliding Window"
tags: algorithm leetcode
categories: Sliding Window
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


Sliding Window 类的题目

### 340. Longest Substring with At Most K Distinct Characters
### 解法思路：Sliding Window + HashMap 解决
### HashMap,存储的key是每个字符, value存储的是每个字符对应的rightMost的索引
### Sliding Window, 设置左右指针，右侧指针恒定向前移动, 并且恒定更新map的每个字符对应的index,同时恒定的去更新res = Math.max(res,right-left+1);
### 左侧指针在map.size>K的时候进行移动，当map.size>K的时候把map中rightMost最小对应的Character从map中删除。 然后把left指针更新为最小的rightMost_index + 1.
```java

class Solution {
    public int lengthOfLongestSubstringKDistinct(String s, int k) {
        if(k == 0) return 0;
        int maxLen = 0;
        HashMap<Character,Integer> map = new HashMap<>();
        int left = 0;
        int right = 0;
        while(right < s.length()){
          map.put(s.charAt(right),right);
          if(map.size()>k){
            int oldIndex = Collections.min(map.values());
            map.remove(s.charAt(oldIndex));
            left = oldIndex+1;
          }
          maxLen = Math.max(maxLen,right-left+1);
          right ++ ;
        }
      return maxLen;
    }
}
```




