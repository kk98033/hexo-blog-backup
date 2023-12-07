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
excerpt: 這題是CPE 2023/10/17 的第二題，這題主要考的是進制轉換和數學餘數的概念。題目要求從一個包含數字和字母的字串中找出一個最小的進制N，使得該字串在N進制下轉換成的十進制數能整除N-1。這需要理解如何將不同字符轉換成特定進制下的數值，以及如何檢查數值與進制之間的除法關係。 - Python/C++ UVa 10672 - Marbles on a tree 解題心得
description:這題是CPE 2023/10/17 的第二題，這題主要考的是進制轉換和數學餘數的概念。題目要求從一個包含數字和字母的字串中找出一個最小的進制N，使得該字串在N進制下轉換成的十進制數能整除N-1。這需要理解如何將不同字符轉換成特定進制下的數值，以及如何檢查數值與進制之間的除法關係。 - Python/C++ UVa 10672 - Marbles on a tree 解題心得
---
# Python/C++ UVa 10093 - An Easy Problem

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1034) - UVa 10093 - An Easy Problem

### 這題是2023/3/21 CPE 的第二題
題目需求要看清楚！！

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
`maxNum`代表了最大的值，我們要取的 `N` ，也就是最小可能的進位。
至於為什麼要加一呢？因為在任何進制系統中，最大的單一數位數字總是該進制減一。
```python
maxNum = max([num[i] for i in s if i in num]) + 1 # 找出字串中最大的數值
sumS = sum([num[i] for i in s if i in num])  # 計算字串中所有字符對應數值的總和 
```

接下來一一的用迴圈從 `N` 開始，找出多少的 `N` 可以使 `sumS` 整除 `N-1`，如果 `N` 超出範圍就輸出 **'such number is impossible!'**

**重要概念!!**
你可能會好奇，為什麼我並沒有照題目講的那樣把字串轉成 `N` 進位再來判斷是否可以整除 `N-1`，只使用了 `sumS` 判斷是否可以整除 `N-1`。
當然你想把字串轉乘 `N` 進位再判斷也是 ok 的，不過為何可以只算出他的 `sumS` 就可以判斷了呢？

當我們要檢查一個字串在特定進制下轉換成的數字是否能夠整除該進制減一時，其實並不需要將字串完全轉換成該進制下的數字。這是因為在任何進制中，一個數字的值可以表示為其各位數字與進制冪次的乘積的和。
假設我們有一個數字 \( X \) 在進制 \( N \) 下的表示，其每位的數值分別為 \( a_0, a_1, a_2, \ldots, a_k \)（從最低位到最高位），那麼這個數字在十進制下的值就是：

\[ X = a_0 \times N^0 + a_1 \times N^1 + a_2 \times N^2 + \ldots + a_k \times N^k \]

對於檢查 \( X \) 是否能被進制的減一（\( N-1 \)）整除的問題，根據數學上的性質，一個數字能被其進制的減一整除的條件是，其所有位的數字之和能被進制的減一整除。這是因為除了最低位 \( N^0 \) 以外，每一位 \( N^k \) 在除以 \( N-1 \) 時都會留下1的餘數。

因此，如果我們計算了字串中所有字符對應數值的總和（表示為 \( \text{sumS} \)），我們只需要檢查：

\[ \text{sumS} \mod (N - 1) = 0 \]

來判斷 \( X \) 是否能被 \( N-1 \) 整除。

哈哈數學好好玩...

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