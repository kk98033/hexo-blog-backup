---
title: Python UVa 10193 - All You Need Is Love
date: 2023-05-23 12:38:42
tags:
  - UVa
  - UVa 一星
  - CPE 49題必考題
categories:
  - 解題心得
  - UVa
excerpt: 這題有點需要用到數學，主要是用輾轉相除法找公因數，不過我們可以使用python內建的math模組的gcd來解 :) Python UVa 10193 - All You Need Is Love 解題心得
description: 這題有點需要用到數學，主要是用輾轉相除法找公因數，不過我們可以使用python內建的math模組的gcd來解 :) Python UVa 10193 - All You Need Is Love 解題心得
---
# 

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1134) - UVa 10193 - All You Need Is Love


## 題意
題目寫了一大堆，簡單來說就是給予兩個二進位數字字串`s1`, `s2`，找出s1, s2是否互質，如果互質輸出 **Love is not all you need!** ，反之則輸出 **All you need is love!**

#### Sample Input 
輸入的第一個數`N`代表有幾個測資，
接下來會輸入兩行，代表`s1` `s2`，總共 **N** 次
{% codeblock line_number:false %}
5
11011
11000
11011
11001
111111
100
1000000000
110
1010
100
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
Pair #1: All you need is love!
Pair #2: Love is not all you need!
Pair #3: Love is not all you need!
Pair #4: All you need is love!
Pair #5: All you need is love!
{% endcodeblock %}

---

## 解題思路
因為輸入是 **二進位** 字串，我們可以用 `int(s, 2)` 把它轉成十進位數字，接下來直接使用 `math` 模組的 `gcd` 來快速的找到最大公因數，如果最大公因數大於一，代表他們沒有互質，輸出 **All you need is love!**

{% note info %}
如果你不要用math模組找最大公因數，建議使用 `輾轉相除法` 來做，避免超時
{% endnote %}

## Python程式碼
```python
# UVa 10193 - All You Need Is Love
import math
def solve(s, l):
    s, l = int(s, 2), int(l, 2)
    if math.gcd(s, l) > 1:
        return 'All you need is love!'
    return 'Love is not all you need!'

T = int(input())
for t in range(T):
    s = input()
    l = input()
    print(f'Pair #{t+1}: {solve(s, l)}')
# Accepted	PYTH3	0.050
```