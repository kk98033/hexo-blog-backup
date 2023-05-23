---
title: Python UVa 10019 - Funny Encryption Method
date: 2023-03-20 14:27:36
tags:
  - UVa
  - UVa 一星
categories:
  - 解題心得
  - UVa
excerpt: Python UVa 10019 - Funny Encryption Method 解題心得
description: Python UVa 10019 - Funny Encryption Method 解題心得
urlname: Python UVa 10019 - Funny Encryption Method
---
# Python UVa 10019 - Funny Encryption Method

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=960) - UVa 10019 - Funny Encryption Method



## 題意
輸入一個數字`N`找出`b1`, `b2`
b1 = N(當成十進位) 轉為2進位，`1`有幾個
b2 = N(當成**十六進位**) 轉為2進位，`1`有幾個

#### Sample Input 
`3`
`265`
`111`
`1234`

#### Sample Output 
`3 5`
`6 3`
`5 5`

---
## 解題思路
使用[`f-string`](https://docs.python.org/zh-tw/3/tutorial/inputoutput.html)(f-string非常方便，建議學起來)，來把10進位數字轉為2進位字串：`f'{decimal:b}'`(decimal為10進位的數字)

對於`b2`，可以先[用int(n, 16)把16進位轉成十進位](https://stackoverflow.com/questions/9210525/how-do-i-convert-hex-to-decimal-in-python)，再丟進f-string轉成二進位字串就好了

至於算有幾個1可以用`.count('1')`

## Python程式碼
```python
# UVa 10019 - Funny Encryption Method
def solve(n):
    # https://stackoverflow.com/questions/9210525/how-do-i-convert-hex-to-decimal-in-python
    n_2 = f'{n:b}'.count('1')
    n_16 = f'{int(str(n), 16):b}'.count('1')

    return f'{n_2} {n_16}'

T = int(input())
for t in range(T):
    n = int(input())
    print(solve(n))
# Accepted	PYTH3	0.010
```