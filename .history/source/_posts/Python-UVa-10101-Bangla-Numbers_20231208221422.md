---
title: Python/C++ UVa 10101 - Bangla Numbers
date: 2023-12-08 21:46:54
  - UVa
  - C++
  - Python
categories:
  - [解題心得, UVa]
excerpt: 這題是是一道關於數字格式化的程式設計題目。這題的主要挑戰在於將一個給定的數字轉換成孟加拉的數字表示方式，這題是一個很好的練習，用於加強對數字格式化和遞迴方法的理解。 - Python/C++ UVa 10101 - Bangla Numbers 解題心得
description: 這題是是一道關於數字格式化的程式設計題目。這題的主要挑戰在於將一個給定的數字轉換成孟加拉的數字表示方式，這題是一個很好的練習，用於加強對數字格式化和遞迴方法的理解。 - Python/C++ UVa 10101 - Bangla Numbers 解題心得
---
# Python/C++ UVa 10101 - Bangla Numbers

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&category=13&page=show_problem&problem=1042) - UVa 10101 - Bangla Numbers


## 題意
UVa 10101 要求將一系列數字轉換成孟加拉格式。孟加拉數字系統包括四個主要單位：
| Bangla Unit | Value    |
|-------------|----------|
| Kuti        | 10000000 |
| Lakh        | 100000   |
| Hajar       | 1000     |
| Shata       | 100      |
例如：
999999999 => 99 kuti 99 lakh 99 hajar 9 shata 99
1000000000 => 1 shata kuti

{% note info %}
* 0 轉換成孟加拉數字還是為 0
* 每個數字由一個空格隔開
* 每次輸出都要輸出目前是第幾次， *他的格式是四個字元，如果長度不到四個字元，就填空格，向右對齊*
{% endnote %}

#### Sample Input 
{% codeblock line_number:false %}
23764
45897458973958
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
   1. 23 hajar 7 shata 64
   2. 45 lakh 89 hajar 7 shata 45 kuti 89 lakh 73 hajar 9 shata 58
{% endcodeblock %}

---

## 解題思路
這題主要的思路是透過方法來處理不同的數字單位（Kuti, Lakh, Hajar, Shata），從最大單位開始將數字分割並加上相應的單位名稱，同時特別注意Kuti單位可能在數字中多次出現的情況，並且對於數字為0的情況進行特殊處理，以確保格式正確且符合孟加拉數字的表示方式。

## Python程式碼
```python

```