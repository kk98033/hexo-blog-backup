---
title: Python UVa 10812 - Beat the Spread
date: 2024-04-10 18:39:58
tags:
  - UVa
  - Python
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 這題是簡單的多項式數學題，搞清楚題目在講什麼就可以解出來了。 - Python UVa 10812 - Beat the Spread 解題心得
description: 這題是簡單的多項式數學題，搞清楚題目在講什麼就可以解出來了。 - Python UVa 10812 - Beat the Spread 解題心得
---
# Python UVa 10812 - Beat the Spread Python 解法

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=1753) - UVa 10812 - Beat the Spread


## 題意
這題是一個簡單的數學問題，給定兩個數字 `s`, `d`：分別兩個隊伍的 **分數之和(s)** 和 **分數之差(d)**。我們的任務是找到兩個**非負整數**分數 `x` 和 `y`（x >= y），使得它們的和等於 s 且它們的差等於 d。如果這樣的分數不存在或者不唯一，則輸出 `"impossible"`。

#### Sample Input 
{% codeblock line_number:false %}
2
40 20
20 40
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
30 10
impossible
{% endcodeblock %}

---

## 解題思路
首先，先檢查是否 d > s，如果 d > s 就代表 **'impossible'**（兩數相減不可能大於兩數相加）。

這個問題可以直接用簡單的代數方法解決。假設給定的兩個方程式是：
x + y = s
x - y = d

我們可以算出：
2x = s + d
x = (s + d) / 2
y = s - x = s - (s + d) / 2 = (s - d) / 2
直接照著做就好了。

最後，我們檢查 y 是否是整數，我們可以用：
```python
y == int(y)
```
來判斷一個數是否為整數。

如果 y 不為整數，也代表 **'impossible'**！！（只需檢查 y 或 x 其中一個使否為整數，因為當 y 不為整數時，x 一定也不為整數） 

## Python程式碼
```python
# UVa 10812 - Beat the Spread!
def solve(sums, different):
    if different > sums: return 'impossible'

    '''
        x + y = 40
        x - y = 20

        2y = 20
        y = 10
        x = 30
    '''
    y = (sums - different) / 2
    x = (sums + different) / 2 

    if not (y == int(y)): # [or use isinstance(y, float)]
        return 'impossible' # float
    return f'{int(x)} {int(y)}'

T = int(input())
for t in range(T):
    sums, different = list(map(int, input().split()))
    print(solve(sums, different))
# Accepted	PYTH3	0.020
```