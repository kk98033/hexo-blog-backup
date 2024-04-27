---
title: Python UVa 10931 - Parity
date: 2024-04-27 08:31:37
tags:
  - UVa
  - Python
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 這題是二進制轉換的題目，python 可以很簡單的解決 :D。 - Python UVa 10931 - Parity 解題心得
description: 這題是二進制轉換的題目，python 可以很簡單的解決 :D。 - Python UVa 10931 - Parity 解題心得
---
# UVa 10931 - Parity Python 解法

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=24&problem=1872#google_vignette) - UVa 10931 - Parity


## 題意
給定一個整數`i`，轉換為二進制後，輸出其二進制表示以及其中`1`的數量。

**輸出格式為：** The parity of `i 的二進制` is `1 的數量` (mod 2).

{% note info %}
輸入為 0 時結束！
{% endnote %}

#### Sample Input 
{% codeblock line_number:false %}
1
2
10
21
0
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
The parity of 1 is 1 (mod 2).
The parity of 10 is 1 (mod 2).
The parity of 1010 is 2 (mod 2).
The parity of 10101 is 3 (mod 2).
{% endcodeblock %}

---

## 解題思路
這題很簡單，不要被他的輸出 (mod 2) 給騙了！這題就只是需要算出 `i` 轉為二進位 ***有多少個一*** 而已！

在 Python 中，將數字轉換成二進制可以使用內建的 `bin()` 函數。不過，如果你想要一種更現代且簡潔的方式，可以考慮使用 **Python 3.6** 以上版本的 `f-string` 格式化方法。(我比較傾向於用 `f-string`)

例如，要將數字 `n` 轉換為二進制，你可以寫成：
```python
n = 10
print(f'{n:b}')
```

因為 `f-string` 是回傳 `string` 的形式，所以接下來可以使用 `.count()` 函式來計算有多少個 `1`：
```python
f"{n:b}".count("1")
```

因此這題可以一行解出來:D ：
```python
print(f'The parity of {n:b} is {f"{n:b}".count("1")} (mod 2).')
```

## Python程式碼
```python
# UVa 10931 - Parity
def solve(n):
    return f'The parity of {n:b} is {f"{n:b}".count("1")} (mod 2).'

while True:
    try:
        n = int(input())
        if n == 0: break
        print(solve(n))
    except EOFError:
        break
# Accepted	PYTH3	0.040
```