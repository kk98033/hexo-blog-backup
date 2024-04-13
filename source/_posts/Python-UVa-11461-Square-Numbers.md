---
title: Python UVa 11461 - Square Numbers
date: 2024-04-13 08:59:26
tags:
  - UVa
  - Python
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 這題要小心超時的問題！ - Python UVa 11461 - Square Numbers 解題心得
description: 這題要小心超時的問題！ - Python UVa 11461 - Square Numbers 解題心得
---
# Python UVa 11461 - Square Numbers 解法

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=2456) - UVa 11461 - Square Numbers


## 題意
這題主要是讓我們計算兩個整數之間 **（包含這兩個數字本身）** 有多少個完全平方數。

一個完全平方數，是指 **可以表示成某個整數乘以自身的數字**，比如 1 (1x1), 4 (2x2), 9 (3x3), 16 (4x4)，以此類推。

假設給定的數字是 3 和 17，完全平方數分別有 4 (2x2), 9 (3x3), 16 (4x4)，所以在這個範圍內有三個完全平方數。所以，答案應該是3。

{% note primary %}
當輸入 `0 0` 時就結束
{% endnote %}

#### Sample Input 
{% codeblock line_number:false %}
1 4
1 10
0 0
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
2
3
{% endcodeblock %}

---

## 解題思路
這題不能直接做！！會超時（可參考下方的solve）。

我這裡使用了數學函式 `math`，的 `sqrt` 來計算他的平方根 **（你也可以直接將他開 0.5 次方: n ** 0.5）**，當他的 ***平方根取整數後乘以平方根等於自己***，就代表他是完全平方數。

#### 不超時解法（參考 solve2）
當你計算 n1 和 n2 的平方根時，並取整（sqrt_n1 和 sqrt_n2），***你實際上是在找出哪些整數的平方會落在這個範圍內***。sqrt_n1 是最小的整數，其平方大於等於 n1，而 sqrt_n2 是最大的整數，其平方小於等於 n2。

所以我們先將 n1, n2 取平方根整數（下高斯），接下來直接將兩個平方根相減就可以得到答案了。
*最後如果 **n1 是完全平方數**，答案要再加一！！*

如果想要知道為何當 n1 是完全平方數時，答案要再加一，我簡單解釋一下：
計算範圍的起點： 當計算 sqrt_n1（即 n1 的平方根的整數部分），如果 sqrt_n1 * sqrt_n1 == n1 成立，意味著 **n1 正好是一個完全平方數**。在這種情況下，sqrt_n1 的平方（即 n1）應被計入範圍內的完全平方數。

計算範圍的終點： 另一方面，sqrt_n2（即 n2 的平方根的整數部分）計算出的結果，其平方始終小於或等於 n2。這意味著**即使 n2 自身是一個完全平方數，它也會自動被包含在內，因為 sqrt_n2 的計算已經考慮了這點**。

## Python程式碼
```python
# UVa 11461 - Square Numbers
import math
def solve(n1, n2):
    # Time limit exceeded	PYTH3	1.000
    ans = 0
    for i in range(n1, n2+1):
        # sqrt = int(i ** (0.5))
        sqrt = math.sqrt(i)
        if i == sqrt * sqrt: 
            ans += 1
    return ans

def solve2(n1, n2):
    sqrt_n1, sqrt_n2 = int(math.sqrt(n1)), int(math.sqrt(n2))
    if sqrt_n1 * sqrt_n1 == n1:
        return sqrt_n2 - sqrt_n1 + 1
    else:
        return sqrt_n2 - sqrt_n1

while True:
    try:
        n1, n2 = list(map(int, input().split()))
        if n1 == 0 and n2 == 0: break
        # print(solve(n1, n2))
        print(solve2(n1, n2))
    except EOFError:
        break
# Accepted	PYTH3	0.010
```