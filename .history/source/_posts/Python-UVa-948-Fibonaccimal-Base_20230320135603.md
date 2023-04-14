---
title: Python UVa 948 - Fibonaccimal Base
date: 2023-03-20 13:29:19
tags:
  - UVa
  - UVa 一星
categories:
  - 解題報告
  - UVa
excerpt: Python UVa 948 - Fibonaccimal Base 解題報告
description: Python UVa 948 - Fibonaccimal Base 解題報告
---
# Python UVa 948 - Fibonaccimal Base

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=889) - UVa 948 - Fibonaccimal Base



## 題意
給予一個十進位的數字，以[費氏數列](https://zh-yue.wikipedia.org/wiki/%E8%B2%BB%E6%B0%8F%E6%95%B8%E5%88%97)當作進位輸出。
**例如：**
費氏數列前15個數：`1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610`
以17為例： 17 = 100101 (fib)
| fib | 1 | 2 | 3 | 5 | 8 | 13 | 21 | 34 |
|:---:|:-:|:-:|:-:|:-:|:-:|:--:|:--:|:--:|
|  17 | 1 | 0 | 1 | 0 | 0 |  1 |  - |  - | 

*記得把表格倒過來*

{% note primary %}
 - 費氏數列前兩個數都是1，我們把它當作一個1
{% endnote %}

#### Sample Input 
`10`
`1`
`2`
`3`
`4`
`5`
`6`
`7`
`8`
`9`
`10`

#### Sample Output 
`1 = 1 (fib)`
`2 = 10 (fib)`
`3 = 100 (fib)`
`4 = 101 (fib)`
`5 = 1000 (fib)`
`6 = 1001 (fib)`
`7 = 1010 (fib)`
`8 = 10000 (fib)`
`9 = 10001 (fib)`
`10 = 10010 (fib)`

---
## 解題思路
先建個費氏數列的表，題目說輸入的值會小於`100,000,000`，大概找40個就夠了，我這裡是找50個。
建完表後再從費氏數列後面找到前面，當目前的數`i`小於`n`時就讓`n - i`，輸出1，其餘補0，繼續重複直到結束，
*註：答案的開頭不能為0*



## 程式碼
```python
# UVa 948 - Fibonaccimal Base
def getFib():
    fibs = [0] * 50
    fibs[0], fibs[1] = 1, 1 
    for i in range(2, len(fibs)):
        fibs[i] = fibs[i-1] + fibs[i-2] # 費氏數列，第i位是(i-1) + (i-2)加起來的值
    return fibs

def solve(n):
    ans = ''
    temp = n
    for i in fibs[::-1]: # 從費氏數列最後開始找，找到最前面
        if temp >= i:
            ans += '1'
            temp -= i
        elif ans: # 當ans有值時才會補0，也就是說當ans是'1...'時才會補零
            ans += '0'
    return f'{n} = {ans[:-1]} (fib)' # 因為我的費氏數列前兩項都會為一，所以要去掉一個（也就是答案的最後一個元素）

fibs = getFib()

T = int(input())
for t in range(T):
    n = int(input())
    print(solve(n))
# Accepted	PYTH3	0.020
```