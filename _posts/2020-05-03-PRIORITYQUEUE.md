---
layout: post
author: Xusheng Ji
title: "PriorityQueue"
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



PriorityQueue   

#### 构造小顶堆

```c
PriorityQueue small=new PriorityQueue<>();
```


#### 构造大顶堆

```c
PriorityQueue small=new PriorityQueue<>(Collections.reverseOrder());

PriorityQueue<int[]> pq= new PriorityQueue<>((a,b)->b[0]-a[0]);

```



#### 特定排序, 按照node.value从小到大排序

```c
static class Node implements Comparable<Node>{

        int index;
        int value;
        public Node(int index,int value){this.index = index; this.value= value;}

        @Override
        public int compareTo(Node o) {
            if(o.value<this.value){
                return 1;
            }else if(o.value>this.value){
                return -1;
            }else{
                return 0;
            }
        }
    }


```








