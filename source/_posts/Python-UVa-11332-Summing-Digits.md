---
title: Python UVa 11332 - Summing Digits
date: 2024-04-21 07:10:16
tags:
  - UVa
  - Python
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 給定一個整數 N，任務是不斷地將 N 的各位數字相加，直到得到一個單一的數字為止。如果要直接照題目意思做可以直接過，也很簡單就可以寫出來了，不過參考了網路上的作法後我才發現可以直接用數學解:O - Python UVa 11332 - Summing Digits 解題心得
description: 給定一個整數 N，任務是不斷地將 N 的各位數字相加，直到得到一個單一的數字為止。如果要直接照題目意思做可以直接過，也很簡單就可以寫出來了，不過參考了網路上的作法後我才發現可以直接用數學解:O - Python UVa 11332 - Summing Digits 解題心得
---
# UVa 11332 - Summing Digits 解法

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&category=25&page=show_problem&problem=2307) - UVa 11332 - Summing Digits


## 題意
給定一個整數 N，任務是不斷地將 N 的各位數字相加，直到得到一個單一的數字為止。
例如，對於 N = 9875，運算過程如下：
9＋8＋7＋5＝29
2＋9＝11
1＋1＝2
**最終結果為 2。**

#### Sample Input 
{% codeblock line_number:false %}
2
11
47
1234567892
0
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
2
2
2
2
{% endcodeblock %}

---

## 解題思路
這題有兩種解法，一種是直接計算，直到只剩下一位數字為止（不會超時），另一種是利用數學特性來解（我也是看網路上的解法才知道的）。

### 解法一：直接計算（不會超時）
程式碼可參考 solve()

這裡我是直接用 python 的字串來讀入，因為 python 可以很簡單的把字串裡面的數字相加，方法有很多種，我這裡就簡單的介紹兩種可以一行解決的方式：

#### 方法 1：使用列表推導式（List Comprehension）
```python
n = '12345'  # n 是一個字串
sum_of_digits = sum(int(char) for char in n)
print(sum_of_digits)  # 輸出結果會是 15
```

#### 方法 2：使用 map() 函數
```python
n = '12345'  # n 是一個字串
sum_of_digits = sum(map(int, n))
print(sum_of_digits)  # 輸出結果會是 15
```

接下來就可以用 while 迴圈來持續這個步驟，***直到 n 只剩一位***
```python
while len(n) > 1:
```

### 解法二：利用數學特性
程式碼可參考 solve2()

其實這個題目的計算過程就是在找數字 n 的「[數根](https://zh.wikipedia.org/zh-tw/%E6%95%B8%E6%A0%B9)」。

根據維基百科：
> 在數學中，數根（又稱位數根或數字根Digital root）是自然數的一種性質，換句話說，每個自然數都有一個數根。
數根是將一正整數的各個位數相加（即橫向相加），若加完後的值大於10的話，則繼續將各位數進行橫向相加直到其值小於10為止[1]，或是，將一數字重複做數字和，直到其值小於10為止，則所得的值為該數的數根。
例如54817的數根為7，因為5+4+8+1+7=25，25大於10則再加一次，2+5=7，7小於10，則7為54817的數根。

接下來我們需要瞭解到數字根的一個數學特性：
數字根可以透過對 ***9*** 取 **模（mod）** 來快速求得。
另外，如果 N 是 9 的倍數，則其數字根應為 9（前提是 N != 0）。

因此我們可以整理出：
1. 如果 N 為 0，則直接返回 0。
2. 否則，如果 N % 9 為 0，則返回 9。
3. 如果上述兩條都不成立，則返回 N % 9。
   
利用這個特性，我們就可以寫出：
```python
if N == 0:
    return 0
elif N % 9 == 0:
    return 9
else:
    return N % 9
```

## Python程式碼
```python
# UVa 11332 - Summing Digits
def solve(n):
    ans = n
    n = list(map(int, n))
    while len(n) > 1:
        ans = sum(n)
        n = list(map(int, str(ans)))
    return ans

def solve2(n):
    n = int(n)
    if n == 0: return 0
    if n % 9 == 0: return 9
    return n % 9

while True:
    try:
        n = input()
        if n == '0': break
        print(solve2(n))
    except EOFError:
        break
# Accepted	PYTH3	0.020
```