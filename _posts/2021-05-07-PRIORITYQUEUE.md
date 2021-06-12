---
layout: post
author: Xusheng Ji
title: "PriorityQueue以及排序"
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
### 1. 对于primative类型的数组, 只能使用Arrays.sort进行正序排序，
### 用Arrays.sort()进行倒序排序 或者 用Arrays.asList()进行把array转为ArrayList都是不合理的,想这么操作只能使用java8中的Arrays.stream(arr).boxed();
### Arrays.sort(OK),Arrays.sort(Collections.reverseOrder())(NO), Arrays.asList(NO)
### 记住，Arrays中的方法都是作用于array而不是 ArrayList

### 1.2 给primative类型的array数组排序升序
```java
int[] a = new int[]{3,6,1,2,8,7};
Arrays.sort(a);
```

### 1.3 给primative类型的array数组排序降序
```java
int[] arr = new int[]{3,6,1,2,8,7};
arr= Arrays.stream(arr).boxed().sorted(Collections.reverseOrder()).mapToInt(Integer::intValue).toArray();
```

### 1.4 int array 转为ArrayList
```java
List<Integer> arrayList = Arrays.stream(arr).boxed().collect(Collectors.toList());
```
### 1.5 ArrayList 转为 int Array
```java
int[] array = res.stream().mapToInt(Integer::valueOf).toArray();
```



### 2.而对于非primative类型的array数组进行倒序排序 或者 转为ArrayList 可以直接使用Arrays.sort() 或者Arrays.asList();
### Arrays.sort(OK),Arrays.sort(Collections.reverseOrder())(OK), Arrays.asList(OK)
### 同时，请记住Arrays.sort()中传入的排序方式，可以是Collections.reverseOrder(),也可以是自己定义的comparator.
```java
String[]  strings  = new String[]{"123","456","789"};
Arrays.sort(strings);
Arrays.sort(strings,Collections.reverseOrder());
List<String> list=Arrays.asList(strings);

```

### 3.对于二维数组来说, Arrays.sort(OK),Arrays.sort(Collections.reverseOrder())(OK), Arrays.asList(OK)
### 3.1 对二维数组的排序,可以直接使用Arrays.sort()
```java
int[][] intervals = new int[ ][ ] { };
 Arrays.sort(intervals,(a,b)->a[0] - b[0]);
```
### 3.2 对二维数组的排序,转为ArrayList
```java
int[][] a = new int[][]{{1,2}};
List<int[]> rrr=Arrays.asList(a);
```

### 4.对于ArrayList,请使用Collections.sort()
### 同时，请记住Collections.sort()中传入的排序方式，可以是Collections.reverseOrder(),也可以是二维数组的箭头方式,也可以是自己定义的comparator.

### 4.1 给ArrayList升序排序
```java
Collections.sort(arraylist);
```

### 4.2 给ArrayList降序排序
```java
Collections.sort(arrr,Collections.reverseOrder());
```
 
### 5.自己定义的Comparator,可以传入Arrays.sort()以及Collections.sort()中或者PQ定义中
### 每次创建comparator的时候, 都需要重写compare 方法, 返回值为正数则代表需要调整顺序，返回值为负数代表不需要调整顺序。
```java
Comparator<String> myComp = new Comparator<String>() {
         @Override
         public int compare(String s1, String s2) {
           //
	   String[] split1 = s1.split(" ", 2);
           String[] split2 = s2.split(" ", 2);
           boolean isDigit1 = Character.isDigit(split1[1].charAt(0));
           boolean isDigit2 = Character.isDigit(split2[1].charAt(0));
           if(!isDigit1 && !isDigit2) {             
             int comp = split1[1].compareTo(split2[1]);
             if(comp != 0)
              return comp;
              return split1[0].compareTo(split2[0]);
           }
             return isDigit1 ? (isDigit2 ? 0 : 1) : -1;
         }
       };
```



### 6.对于PriorityQueue来说, 当创建PQ的时候，传入的参数和Arrays.sort()和Collections.sort()的参数一致可以是Collections.reverseOrder(),也可以是二维数组的箭头方式,也可以是自己定义的comparator.
#### 6.1 构造小顶堆

```c
PriorityQueue small=new PriorityQueue<>();
```


#### 6.2 构造大顶堆

```c
PriorityQueue small=new PriorityQueue<>(Collections.reverseOrder());

PriorityQueue<int[]> pq= new PriorityQueue<>((a,b)->b[0]-a[0]);

```



#### 使得PriorityQueue中的对象(node.value从小到大排序)能够自动排序，
#### --> 先让class实现Comparable接口,然后修改class的compareTo方法
 
 

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

#### 使得PriorityQueue中的对象(按照entry.price从小到大排序)能够自动排序，
#### --> 先让class实现Comparable接口,然后修改class的compareTo方法


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
