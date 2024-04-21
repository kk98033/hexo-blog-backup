---
title: Python UVa 10922 - 2 the 9s
date: 2024-04-21 11:26:41
tags:
  - UVa
  - Python
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 這題跟 UVa 11332 很像，主要目的是要判斷給定的數字是否為 9 的倍數，並且計算這個數字需要多少次的 “數根運算” 才能變為一個單位數。 - Python UVa 10922 - 2 the 9s 解題心得
description: 這題跟 UVa 11332 很像，主要目的是要判斷給定的數字是否為 9 的倍數，並且計算這個數字需要多少次的 “數根運算” 才能變為一個單位數。 - Python UVa 10922 - 2 the 9s 解題心得
---
# Python UVa 10922 - 2 the 9s 解法

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1863) - UVa 10922 - 2 the 9s


## 題意
這題目的主要目的是要判斷給定的數字是否為 **9 的倍數**，並且計算這個數字需要多少次的 “[數根運算](https://zh.wikipedia.org/zh-tw/%E6%95%B8%E6%A0%B9)” 才能變為一個單位數。所謂的 “數根運算” 指的是***將一個數的所有位數相加，如果結果超過一位數，則繼續進行相加，直到結果為一位數為止***。

以第一個測資：999999999999999999999，為例
999999999999999999999: 9 + 9 + ... + 9 = 189 -> **是九的倍數**
189: 1 + 8 + 9 = 19 -> **是九的倍數**
18: 1 + 8 = 9 -> **是九的倍數**
9: **只剩下一位數，停止！**
因此，他的 9-degree 為：3

{% note primary %}
當輸入 0 時就停止！
注意輸出的格式！
{% endnote %}

#### Sample Input 
{% codeblock line_number:false %}
999999999999999999999
9
9999999999999999999999999999998
0
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
999999999999999999999 is a multiple of 9 and has 9-degree 3.
9 is a multiple of 9 and has 9-degree 1.
9999999999999999999999999999998 is not a multiple of 9.
{% endcodeblock %}

---

## 解題思路
這題跟 [UVa 11332 - Summing Digits](https://blog.iddle.dev/public/2024/04/21/Python-UVa-11332-Summing-Digits/) 做法差不多，直接照著題目做就好了。

先用 ***字串*** 的形式將字串轉成陣列，再用 `sum` 函數來總和（取得整數）。重複此動作一直做下去直到只剩下一位數（記得要檢查新的數字可不可以整除 9）。

至於如何計算數字總和可以參考 [UVa 11332 - Summing Digits](https://blog.iddle.dev/public/2024/04/21/Python-UVa-11332-Summing-Digits/) 我介紹的方式 :>

## Python程式碼
```python
# UVa 10922 - 2 the 9s
def solve(n):
    degree = 0
    current = n
    while len(current) > 1:
        digitSum = sum(map(int, current))  
        current = str(digitSum)  
        degree += 1
        if digitSum % 9 != 0:
            return f'{n} is not a multiple of 9.'

    if int(current) == 9:
        return f'{n} is a multiple of 9 and has 9-degree {degree}.'
    else:
        return f'{n} is not a multiple of 9.'

while True:
    try:
        n = input()
        if n == '0': break
        print(solve(n))
    except EOFError:
        break
# Accepted	PYTH3	0.390
```