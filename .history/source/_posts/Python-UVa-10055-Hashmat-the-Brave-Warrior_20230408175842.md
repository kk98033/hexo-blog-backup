---
title: Python UVa 10055 - Hashmat the Brave Warrior
date: 2023-03-20 19:56:01
tags:
  - UVa
  - UVa 一星
categories:
  - 解題心得
  - UVa
excerpt: Python UVa 10055 - Hashmat the Brave Warrior 解題心得
description: Python UVa 10055 - Hashmat the Brave Warrior 解題心得
---
# Python UVa 10055 - Hashmat the Brave Warrior

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=996) - UVa 10055 - Hashmat the Brave Warrior



## 題意
給予兩數，求出兩數的差的絕對值。

#### Sample Input 
`10 12`
`10 14`
`100 200`

#### Sample Output 
`2`
`4`
`100`

---
## 解題思路
這題太水了，希望下次CPE可以出這題...

## 程式碼
```python
# UVa 10055 - Hashmat the Brave Warrior
while True:
    try:
        n1, n2 = list(map(int, input().split()))
        print(abs(n1-n2))
    except EOFError:
        break
# Accepted	PYTH3	0.480
```