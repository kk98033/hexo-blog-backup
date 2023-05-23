---
title: Python UVa 10235 - Simply Emirp
date: 2023-05-23
tags:
  - UVa
  - UVa 一星
  - CPE 49題必考題
categories:
  - 解題心得
  - UVa
excerpt: Python UVa 10235 - Simply Emirp 解題心得
description: Python UVa 10235 - Simply Emirp 解題心得
---
# UVa 10235 Python

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1176) - UVa 10235 - Simply Emirp


## 題意
輸入一個 **N** 
輸出：
1. "N is not prime."，如果 N 不是一個質數
2. "N is prime."，如果 N 是一個質數，但是不是一個 Emirp
3. "N is emirp."，如果 N 是一個 emirp
   
如果把一個 **質數** 反過來也是一個 **質數** ，則此數就為： **Emirp**

#### Sample Input 
{% codeblock line_number:false %}
17
18
19
179
199
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
17 is emirp.
18 is not prime.
19 is prime.
179 is emirp.
199 is emirp.
{% endcodeblock %}

---

## 解題思路
我們只需要依照題目意思，判斷此數是 prime 、 emirp 還是 not prime就可以了
判斷質數的方法可以參考下方的方法
*我們只需判斷到第`int(n ** 0.5) + 1`就好了!*
```python
def isPrime(n):
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0: return False
    return True
```
<br>

至於把一個數顛倒可以這樣做：
`int(str(n)[::-1])`

## Python程式碼
```python
# UVa 10235 - Simply Emirp
def isPrime(n):
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0: return False
    return True

def solve(n):
    if isPrime(n) and isPrime(int(str(n)[::-1])) and n != int(str(n)[::-1]):
        return f'{n} is emirp.'
    elif isPrime(n):
        return f'{n} is prime.'
    else:
        return f'{n} is not prime.'

while True:
    try:
        n = int(input())

        print(solve(n))
    except EOFError:
        break
# Accepted	PYTH3	0.410
```