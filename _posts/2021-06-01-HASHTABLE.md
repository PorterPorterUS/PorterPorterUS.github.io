---
layout: post
author: Xusheng Ji
title: "Hash table"
tags: algorithm leetcode
categories: HashTable
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


HashTable 题目


### 347. Top K Frequent Elements

### 思路: 首先创建HashMap, 对应的关系为 number => freq 
### 然后创建一个list,其中每个元素都是一个ArrayList, list[freq] = {1,2,3,4},意思为{1,2,3,4}出现的频率都为freq
### 最后按照ArrayList的倒叙, 向结果集中存放元素，当元素个数等于K的时候，退出


```java
class Solution {
    public int[] topKFrequent(int[] nums, int k) {
        //1. hashmap store num->freq
        //2. 2d array list[freq]=ArrayList
        //3. back for loop the arraylist 
        HashMap<Integer,Integer> map=new HashMap<>();
        for(int num:nums){
            map.put(num,map.getOrDefault(num,0)+1);
        }
        List<Integer>[] list=new List[nums.length+1];
        for(int num:map.keySet()){
            int freq=map.get(num);
            if(list[freq]==null){
                list[freq]=new ArrayList<>();
            }
            list[freq].add(num);
        }
        ArrayList<Integer> res=new ArrayList<>();
        for(int i=list.length-1;i>=0 && res.size()<k;i--){
            if(list[i]!=null){
                res.addAll(list[i]);
            }
        }
        return res.stream().mapToInt(Integer::valueOf).toArray();
        
        
    }
}

```


### 692. Top K Frequent Words

### 和347题一样，但是元素改为了字符串, 并且当出现了两个字符串频率相同的情况，因为先按照字母顺序从大到小排列

### 思路：首先创建HashMap, 对应的关系为 String => freq

### 然后创建一个PriorityQueue, 是一个minHeap, 首先按照频率从小到大排序，然后相同频率的字符串按照字母顺序从小到大排序。

### 为了保证时间复杂度为 O(NlogK),N是需要遍历多少个元素,K是PriorityQueue的大小, 只有实现了minheap,等到其大小大于K然后poll的话，才能保证O(NlogK)的时间复杂度。

### 把PQ中的元素, 倒序插入到结果集合中，出现的结果就是，首先是频率大的，然后频率相同的话按照从小到大排列

```java
class Solution {
    public List<String> topKFrequent(String[] words, int k) {
        
        List<String> result = new LinkedList<>();
        Map<String, Integer> map = new HashMap<>();
        for(int i=0; i<words.length; i++)
        {
          map.put(words[i],map.getOrDefault(words[i],0)+1);
        }
        
        PriorityQueue<Map.Entry<String, Integer>> pq = new PriorityQueue<>(
                 (a,b) -> a.getValue()==b.getValue() ? b.getKey().compareTo(a.getKey()) : a.getValue()-b.getValue()
        );
        for(Map.Entry<String,Integer> e:map.entrySet()){
             pq.offer(e);
             if(pq.size()>k){
               pq.poll();
             }
        }

      
        while(!pq.isEmpty())
            result.add(0, pq.poll().getKey());
        
        return result;
    }
}

```
