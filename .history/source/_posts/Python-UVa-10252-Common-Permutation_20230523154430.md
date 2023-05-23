---
title: Python UVa 10252 - Common Permutation
date: 2023-05-23
tags:
  - UVa
  - UVa 一星
  - CPE 49題必考題
categories:
  - 解題心得
  - UVa
excerpt: Python UVa 10252 - Common Permutation 解題心得
description: Python UVa 10252 - Common Permutation 解題心得
---
# UVa 10252 Python

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=1193) - UVa 10252 - Common Permutation


## 題意
給予兩個字串 **a**, **b** 印出最長的小寫字串 **x** ，使得 **x** 經過重新排列後為 **a** 的子序列，且 **x** 經過重新排列後為 **b** 的子序列，印出由大排到小的 **x**。
也就是找出 `a`, `b`, 兩個字串 **所有共同出現** 的字元

#### Sample Input 
{% codeblock line_number:false %}
pretty
women
walking
down
the
street
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
e
nw
et
{% endcodeblock %}

{% note warning %}
注意：是印出`“所有”`共同出現的字元，**重複的也要算進去**
例如：`hello` 和 `lol` 要印出： `llo`
{% endnote %}

---

## 解題思路
因為這題是要找所有兩個字串的共同字元， **包括重複的** 也要算進去
我這裡是先用兩個字典來分別計算兩個字串中每個字元的出現次數<br>

接下來去出兩個字典中共同有的字元，再把它加到陣列裡
```python
for key, item in n1Dic.items():
        if key in n1 and key in n2:
            ans.extend([key] * min(n1Dic[key], n2Dic[key]))
```
<br>

由於 **重複的也要算進去** ，我這裡直接用`[key] * min(n1Dic[key], n2Dic[key])`
<br>



## Python程式碼
```python
# UVa 10252 - Common Permutation
def solve(n1, n2):
    ans = ''
    n1Dic, n2Dic = {}, {}
    for i in n1:
        n1Dic[i] = n1Dic.get(i, 0) + 1
    for i in n2:
        n2Dic[i] = n2Dic.get(i, 0) + 1

    for key, item in n1Dic.items():
        if key in n1 and key in n2:
            ans += (f'{key}' * min(n1Dic[key], n2Dic[key]))
    return ''.join(sorted(ans))

while True:
    try:
        n1 = input()
        n2 = input()

        print(solve(n1, n2))
    except EOFError:
        break
# Accepted	PYTH3	0.020
```