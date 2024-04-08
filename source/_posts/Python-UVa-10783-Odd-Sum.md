---
layout: blog
title: Python UVa 10783 - Odd Sum
date: 2024-04-08 20:16:53
tags:
  - UVa
  - Python
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 這題就只是計算範圍內的奇數合而已，看似簡單，實則真的很簡單，我還以為會有陷阱。 - Python UVa 10783 - Odd Sum 解題心得
description: 這題就只是計算範圍內的奇數合而已，看似簡單，實則真的很簡單，我還以為會有陷阱。 - Python UVa 10783 - Odd Sum 解題心得
---
# UVa 10783 - Odd Sum Python 解法

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1724) - UVa 10783 - Odd Sum


## 題意
這題的目標是給定兩個整數 `a` 和 `b`，要求計算從 a 到 b **（包含 a 和 b）** 之間所有 **奇數** 的和。

#### Sample Input 
{% codeblock line_number:false %}
2
1
5
3
5
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
Case 1: 9
Case 2: 8
{% endcodeblock %}

---

## 解題思路
這題的 a, b 大小都小於 100，所以應該是不用考慮超時的大小。
可以直接用迴圈從 `a` ~ `b+1` （因為要包括 a, b，所以 range 要到 **b+1**）一一計算。`(可以參考solve 1)`

除此之外，由於只是找奇數和，所以我們可以在**確保 a 是奇數的情況下**，用迴圈一直加 2，直到 b（range一樣要到 **b+1**）為止，應該會快一點。`(可以參考solve 2)`

最後，如果 a 和 b 的數值很大可能會超時，我們可以用數學公式解，雖然這題用不上，但我還是大概講一下：`(可以參考solve 3)`
首先，先找出 **首項** 和 **末項**：
```python
if a % 2 == 0: a += 1
if b % 2 == 0: b -= 1
```

接下來，找出 **首項** 到 **末項** 所有的奇數數量 `n`：
```python
n = (b - a) // 2 + 1
```

最後使用 **等差數列的求和公式** 就可以快速的求出答案了！
```python
total = n * (a + b) // 2
```

## Python程式碼
```python
# UVa 10783 - Odd Sum
def solve(a, b):
    ans = 0
    for i in range(a, b+1):
        if i % 2 != 0:
            ans += i
    return ans

def solve2(a, b):
    # 確保 a 是奇數
    if a % 2 == 0:
        a += 1
    
    # 計算從 a 到 b 之間所有奇數的和
    ans = 0
    for i in range(a, b+1, 2):
        ans += i

    return ans

def solve3(a, b):
    # 確保首項和末項都是奇數
    if a % 2 == 0: a += 1
    if b % 2 == 0: b -= 1
    
    # 計算 a 到 b 之間奇數的數量
    n = (b - a) // 2 + 1
    
    # 使用等差數列的求和公式
    total = n * (a + b) // 2
    return total

T = int(input())
for t in range(T):
    n1 = int(input())
    n2 = int(input())
    
    print(f'Case {t+1}: {solve(n1, n2)}')
# Accepted	PYTH3	0.020
```