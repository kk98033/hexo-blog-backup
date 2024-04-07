---
title: Python UVa 10420 - List of Conquests
date: 2024-04-07 19:47:01
tags:
  - UVa
categories:
  - [解題心得, UVa]
excerpt: 這題就只需要統計字串出現的次數並且排序而已！ - Python UVa 10420 - List of Conquests 解題心得
description: 這題就只需要統計字串出現的次數並且排序而已！ - Python UVa 10420 - List of Conquests 解題心得
---
# UVa 10420 - List of Conquests 解法

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&category=16&page=show_problem&problem=1361) - UVa 10420 - List of Conquests


## 題意
給定一系列的 **國家** 和 **女子名字** ，每行包含一個國家和一名女子的名字，格式為 `國家 女子名字`。目標是統計每個國家出現的次數，並將結果**按國家名字的字母順序輸出**。


#### Sample Input 
{% codeblock line_number:false %}
3
Spain Donna Elvira
England Jane Doe
Spain Donna Anna
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
England 1
Spain 2
{% endcodeblock %}

---

## 解題思路
因為這題只需要計算國家名稱出現的次數，所以只讀國家名字就好了，其他的就不用理他。

利用 python 的 **字典**，就可以簡單的算出出現次數，字典的 key 是國家，value 是出現次數：
```python
count[country] = count.get(country, 0) + 1
```

{% note primary %}
count.get(country, 0)：這部分是在使用字典的get方法。get方法會嘗試從字典count中找到鍵名為country的值。如果找到了，就返回這個值；如果沒找到，就返回get方法的第二個參數，這裡是0。所以，如果country已經在count字典中，這會返回country當前的計數，如果不在，就返回0。
{% endnote %}

最後輸出的 `count.items()` 會返回一個包含字典中所有鍵值對的列表，列表中的每個元素都是一個 tuple： `(key, value)`，其中 **key 是國家名（country）**，**value 是該國家的出現次數（count）**。sorted() 函數將這個列表排序，預設會根據 tuple 中的第一個元素（也就是國家名）進行字母順序排序。

## Python程式碼
```python
# UVa 10420 - List of Conquests
T = int(input())
count = {}
for t in range(T):
    country = input().split()[0]
    count[country] = count.get(country, 0) + 1

for country, count in sorted(count.items()):
    print(f'{country} {count}')
# Accepted	PYTH3	0.070
```