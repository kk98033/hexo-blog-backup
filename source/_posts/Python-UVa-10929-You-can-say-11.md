---
title: Python UVa 10929 - You can say 11
date: 2024-04-16 10:00:07
tags:
  - UVa
  - Python
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 這題是 python 的福音，因為 Python 可以直接處理大數，所以可以直接 % 11 :D (不過還是要注意他的測資)，如果是 C++ 可能就比較麻煩了。 - Python UVa 10929 - You can say 11 解題心得
description: 這題是 python 的福音，因為 Python 可以直接處理大數，所以可以直接 % 11 :D (不過還是要注意他的測資)，如果是 C++ 可能就比較麻煩了。 - Python UVa 10929 - You can say 11 解題心得
---
# Python UVa 10929 - You can say 11 Python 解法

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&category=0&problem=1870&mosmsg=Submission+received+with+ID+29384240) - UVa 10929 - You can say 11


## 題意
判斷輸入的數字是否為 11 的倍數，如果是輸出：`is a multiple of 11.`，不是的話輸出 `is not a multiple of 11.`
當輸入為 0 就結束

{% note primary %}
要注意他的測資！！！
有些測資長這樣："    00000000030800 "(沒錯他輸入的數字前後就是故意有空格 = =)
正確的輸出格式要是 "00000000030800 is a multiple of 11." <- 前面的 0 也要！！！！
{% endnote %}

#### Sample Input 
{% codeblock line_number:false %}
112233
30800
2937
323455693
5038297
112234
0
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
112233 is a multiple of 11.
30800 is a multiple of 11.
2937 is a multiple of 11.
323455693 is a multiple of 11.
5038297 is a multiple of 11.
112234 is not a multiple of 11.
{% endcodeblock %}

---

## 解題思路
**這題我們讀入測資的時候先不要將他轉成 int !!!**
讀入測資後我們用：
```python
int(n) % 11 == 0
```
來判斷是否為 11 的倍數

接下來回傳的事後我們要：
1. 保留完整的數字（包括如果數字開頭有 0 也要輸出 0）。
2. 去除所有的空格。
剛好 python 有個很好用的函式叫做：`strip()`，可以移除**字串開頭和結尾的空白字符**。

可以參考下方程式碼的用法：

## Python程式碼
```python
# UVa 10929 - You can say 11
while True:
    try:
        n = input()
        if n == '0': break
        if int(n) % 11 == 0:
            print(f'{n.strip()} is a multiple of 11.')
        else:
            print(f'{n.strip()} is not a multiple of 11.')
    except EOFError:
        break
# Accepted	PYTH3	0.060
```