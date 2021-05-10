---
layout: post
author: Xusheng Ji
title: "PriorityQueue"
tags: algorithm leetcode
categories: TOOLS
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



#### 特定排序, 按照entry.price从小到大排序

```java
private static class Entry implements Comparable<Entry> {
	int city; // 为了判断是否达到终点,
	int price; // 为了根据price在pq中排序
	int stops; // 为了判断是否达到K步 
	Entry(int city, int price, int stops) {
		this.city = city;
		this.price = price;
		this.stops = stops;
	}
    // 该对象存储的话，是按照price进行排序的
	@Override
	public int compareTo(Entry that) {
		return this.price - that.price;
	}
}

```


#### 特定排序, 小顶堆, Key是字符串, Value是频率, 首先按照频率从小到大排序，如果频率相同则按照字符串的字母顺序从小到大排序
#### 所以PQ.PEEK 处的元素首先是频率小的，然后如果频率一样，那么字母顺序的排在队列顶端. 

```java
PriorityQueue<Map.Entry<String, Integer>> pq = new PriorityQueue<>(
                 (a,b) -> a.getValue()==b.getValue() ? b.getKey().compareTo(a.getKey()) : a.getValue()-b.getValue()
        )

```
