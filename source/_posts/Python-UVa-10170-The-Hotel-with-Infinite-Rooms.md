---
title: Python UVa 10170 - The Hotel with Infinite Rooms
date: 2023-03-18 08:27:25
tags:
  - UVa
  - UVa 一星
categories:
  - 解題報告
  - UVa
excerpt: Python UVa 10170 - The Hotel with Infinite Rooms 解題報告
description: Python UVa 10170 - The Hotel with Infinite Rooms 解題報告
---

# Python UVa 10035 - Primary Arithmetic

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=0&problem=1111&mosmsg=Submission+received+with+ID+28314511) - UVa 10170 - The Hotel with Infinite Rooms



## 題意
有一個奇怪的旅館，他裡面有無限個房間，來這個旅館的團體會遵循以下規則：
* 一次只能有一組團體住在旅館裡
* 每組都會在早上check-in，離開時在晚上check-out
* 另一組人會在上一組人離開的後一天入住
* **下一組客人一定會比上一組客人還要多一人**
* 當每組有**n**人時，他們會在旅館住**n**天

輸入兩個整數，S, D，求出第D天時旅館內目前住了幾人
(S代表第一天入住了幾人，D代表第幾天)

{% note primary %}
 - **不能暴力硬解，D最大會到10^15，python一定會超時**
{% endnote %}

#### Sample Input 
`1 6`
`3 10`
`3 14`

#### Sample Output 
`3`
`5`
`6`

---
## 解題思路
這題要考驗你的數學能力:(
假設第一天住了*S*人，第*D*天時住了*N*人

$$\overbrace{S + (S + 1) + (S + 2) + ... + N}^{總共待的天數} \leq D$$

總共有**N - S + 1個間格**
套上等差數列求和公式

$$\Rightarrow \frac{(S + N) * (N - S + 1)}{2} \leq D$$

$$\Rightarrow N^{2} + N + (S^{2} + S - 2 * D)$$

$$\Rightarrow \frac{-1 \pm \sqrt{1^{2} - 4 * 1 * (S^{2} + S - 2 * D)}}{2}$$

*最後記得無條件進位，需導入math模組的ceil*


## 程式碼
```python
# UVa 10170 - The Hotel with Infinite Rooms
import math
def solve2(s, d):
    return int(math.ceil((-1 + ((1 - 4 * (-(s ** 2) + s - 2 * d))) ** 0.5) / 2))

while True:
    try:
        s, d = list(map(int, input().split()))
        print(solve2(s, d))
    except EOFError:
        break
# Accepted	PYTH3	0.130
```