---
title: Python UVa 10093 - An Easy Problem
date: 2023-12-07 23:02:03
tags:
  - UVa
  - C++
  - Python
categories:
  - [解題心得, UVa]
  - [CPE歷屆, CPE 2023/10/17]
excerpt: 這題是CPE 2023/10/17 的第一題，這題主要考的是進制轉換和數學餘數的概念。題目要求從一個包含數字和字母的字串中找出一個最小的進制N，使得該字串在N進制下轉換成的十進制數能整除N-1。這需要理解如何將不同字符轉換成特定進制下的數值，以及如何檢查數值與進制之間的除法關係。 - Python/C++ UVa 10672 - Marbles on a tree 解題心得
description:這題是CPE 2023/10/17 的第一題，這題主要考的是進制轉換和數學餘數的概念。題目要求從一個包含數字和字母的字串中找出一個最小的進制N，使得該字串在N進制下轉換成的十進制數能整除N-1。這需要理解如何將不同字符轉換成特定進制下的數值，以及如何檢查數值與進制之間的除法關係。 - Python/C++ UVa 10672 - Marbles on a tree 解題心得
---
# Python/C++ UVa 10093 - An Easy Problem

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1034) - UVa 10093 - An Easy Problem


## 題意
給定一個只包含數字和字母的字串，需要找出一個最小的進制數N，使得這個字串在N進制下轉換成的十進制數能夠整除N-1。如果找不到這樣的N，則輸出"such number is impossible!"。
每個字符需要轉換成相對應的數值。例如，'0'-'9' 對應 0-9，'A'-'Z' 對應 10-35，'a'-'z' 對應 36-61。

#### Sample Input 
{% codeblock line_number:false %}
3
5
A
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
4
6
11
{% endcodeblock %}

---

## 解題思路
嗚嗚嗚這題我當初完全看不懂他要我們做什麼 ;-;，總而言之，就是給一個字串，找出他最小是幾進制數 `N`，使得這個字串在 `N` 進制下轉換成的十進制數能夠整除 `N-1`。

這題的解法是要先建表，我這裡是建一個字典，代表每個字元的值代表多少，可以參考下方程式碼的 `generate()` 函式。

接下來就算出該字串最大的字元代表的值，以及所有字串值加起來的總和。
`maxNum`代表了最大的值，我們要取的 `N`
```python
maxNum = max([num[i] for i in s if i in num]) # 找出字串中最大的數值
sumS = sum([num[i] for i in s if i in num])  # 計算字串中所有字符對應數值的總和 
```



## Python程式碼
```python
# UVa 10093 - An Easy Problem!
def generate():
    # 建立一個字典來映射字元到對應的數值
    num = {}
    index = 0

    # 將 '0' 到 '9' 映射到 0-9
    for i in range(ord('0'), ord('9')+1):
        num[chr(i)] = index
        index += 1

    # 將 'A' 到 'Z' 映射到 10-35
    for i in range(ord('A'), ord('Z')+1):
        num[chr(i)] = index
        index += 1

    # 將 'a' 到 'z' 映射到 36-61
    for i in range(ord('a'), ord('z')+1):
        num[chr(i)] = index
        index += 1
    return num

def solve(s):
    maxNum = max([num[i] for i in s if i in num]) + 1 # 找出字串中最大的數值
    sumS = sum([num[i] for i in s if i in num])  # 計算字串中所有字符對應數值的總和 
    if maxNum <= 1: return 2

    while maxNum <= 62:
        # 如果總和能被當前進制減一整除，則找到了答案
        if sumS % (maxNum - 1) == 0: 
            return maxNum
        maxNum += 1
    return 'such number is impossible!'

num = generate()
while True:
    try:
        s = input().replace(' ', '') # 讀取輸入並去除空格
        print(solve(s))
    except EOFError:
        break
# Accepted	PYTH3	0.160
```