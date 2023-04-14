---
title: Python UVa 10056 - What is the Probability ?
date: 2023-03-20 20:20:56
tags:
  - UVa
  - UVa 一星
categories:
  - 解題心得
  - UVa
excerpt: Python UVa 10056 - What is the Probability ? 解題心得
description: Python UVa 10056 - What is the Probability ? 解題心得
---
# Python UVa 10056 - What is the Probability ?

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&category=12&page=show_problem&problem=997) - UVa 10056 - What is the Probability ?



## 題意
輸入`N`位玩家、贏的機率`P`，找出第`i`個玩家的獲勝機率，要取到小數點後四位

{% note primary %}
 - 當`P`為`0`時，要輸出`0.0000`
{% endnote %}

#### Sample Input 
`2`
`2 0.166666 1`
`2 0.166666 2`

#### Sample Output 
`0.5455`
`0.4545`

---
## 解題思路
這題是數學題D:，要用等比級數計算。

假設贏的機率是`p`、輸的機率是`q`(也就是就是`1-p`)、總共有`n`人、要找第`t`個玩家贏的機率

那麼
第一輪遊戲第`t`個玩家贏的機率是： $$ p \times q^{(t - 1)} $$
第二輪遊戲第`t`個玩家贏的機率是： $$ p \times q^{(t - 1)} + p \times q^{(n + t - 1)} $$
第三輪遊戲第`t`個玩家贏的機率是： $$ p \times q^{(t - 1)} + p \times q^{(n + t - 1)} + p \times q^{(n + n + t - 1)} $$
整理一下後會變成 $$ \Rightarrow p(q^{t-1}+q^{n+t-1}+q^{2n+t-1}...) $$
套上[無窮等比級數求和公式](https://jimmyyao0203.pixnet.net/blog/post/68413370)後(公差: q^n) $$ \Rightarrow p(\frac{q^{t-1}}{1-q^{n}}) $$

最後再用`f-string`取小數點後四位就好了(像這樣`f'{num:.4f}'`)

{% note primary %}
 - 記得加個條件：如果p為0，就直接輸出0.0000
{% endnote %}

## Python程式碼
```python
# 10056 - What is the Probability ?
def solve(n, p, target):
    if p == 0: return f'{0:.4f}'
    q = (1 - p)
    return f'{p * (q ** (target - 1) / (1 - q ** n)):.4f}'

T = int(input())
for t in range(T):
    n, p, target = input().split()
    print(solve(int(n), float(p), int(target)))
# Accepted	PYTH3	0.010
```