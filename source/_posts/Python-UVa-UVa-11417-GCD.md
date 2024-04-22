---
title: Python UVa UVa 11417 - GCD
date: 2024-04-22 09:03:08
tags:
  - UVa
  - Python
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 這道題目要求我們計算一個稱為 G(N) 的值，G(N) 是基於所有小於 N 的正整數 i 和 j 組成的配對的最大公因數 (GCD) 之和。Python 有函式可以直接算出最大公因數！ - Python UVa UVa 11417 - GCD 解題心得
description: 這道題目要求我們計算一個稱為 G(N) 的值，G(N) 是基於所有小於 N 的正整數 i 和 j 組成的配對的最大公因數 (GCD) 之和。Python 有函式可以直接算出最大公因數！ - Python UVa UVa 11417 - GCD 解題心得
---
# UVa UVa 11417 - GCD 解法

>[題目連結](https://onlinejudge.org/external/114/11417.pdf) - UVa UVa 11417 - GCD（online judge 上面的題目連結有點怪怪的，所以我只給 pdf）

## 題意
這道題目要求我們計算一個稱為 G(N) 的值，G(N) 是基於所有小於 N 的正整數 i 和 j 組成的配對的最大公因數 (GCD) 之和。給定一個 `N`，我們需要找出所有 i, j（其中 **1≤i<j≤N** ）的 ***GCD*** 並計算它們的總和。題目持續讀取 N 的值，直到 N 為 0 停止。

以下為 G(N) 的公式（題目提供的）：
$$G = \sum_{i=1}^{i < N} \sum_{j=i+1}^{j \leq N} \text{GCD}(i, j)$$

以下為 G(N) 的程式碼邏輯（題目提供的）：
```c
G=0;
for(i=1;i<N;i++)
for(j=i+1;j<=N;j++)
{
    G+=GCD(i,j);
}
/*Here GCD() is a function that finds
the greatest common divisor of the two
input numbers*/
```

{% note primary %}
當輸入 0 時停止！
{% endnote %}

#### Sample Input 
{% codeblock line_number:false %}
10
100
500
0
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
67
13015
442011
{% endcodeblock %}

---

## 解題思路
這題題目第一次看到時挺嚇人的（因為有數學符號💀💀），但其實題目已經把大概的程式碼架構都給你了（題目上面附的程式碼），這題要完成的其實就只有 ***GCD*** 這個函式而已，其他的程式碼都跟題目一樣。

要求 **最大公因數（GCD）** 很簡單，用一個 **while** 迴圈一行就可以做出來了：
```python
# Accepted	PYTH3	1.410
def gcd(a, b):
    while b:
        a, b = b, a % b
    return a
```

不過 python 本身內建的 math 函式庫也有 **GCD** 的函式，速度比用 while 迴圈寫的還要快上許多，可以背一下：
```python
import math
math.gcd(a, b) # 回傳最大公因數
```

## Python程式碼
```python
# UVa 11417 - GCD
import math
def solve(n):
    g = 0
    for i in range(1, n):
        for j in range(i+1, n+1):
            g += math.gcd(i, j)
            # g += gcd(i, j)
    return g

def gcd(a, b):
    # Accepted	PYTH3	1.410
    while b:
        a, b = b, a % b
    return a

while True:
    try:
        n = int(input())
        if n == 0: break

        print(solve(n))
    except EOFError:
        break
# Accepted	PYTH3	0.820
```