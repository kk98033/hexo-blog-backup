---
title: Python UVa 10038 - Jolly Jumpers
date: 2023-03-20 18:50:34
tags:
  - UVa
  - UVa 一星
categories:
  - 解題心得
  - UVa
excerpt: Python UVa 10038 - Jolly Jumpers 解題心得
description: Python UVa 10038 - Jolly Jumpers 解題心得
---
# Python UVa 10038 - Jolly Jumpers

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=979) - UVa 10038 - Jolly Jumpers



## 題意
給予一個陣列，判斷他是否為jolly jumper

jolly jumper的定義是：如果一個n個整數的序列其相鄰兩個的值的差的絕對值恰好為`1 ~ n-1`

例如：`1 4 2 3`就是jolly jumper
他相鄰兩項的差是：`3, 2, 1`滿足`1 ~ n-1`

{% note primary %}
 - 注意！！題目沒講，相鄰兩項的差不用連續ex: 某序列相鄰兩項的差為：`1, 3, 2, 4`，他是**jolly jumper**
{% endnote %}

#### Sample Input 
`4 1 4 2 3`
`5 1 4 2 -1 6`

#### Sample Output 
`Jolly`
`Not jolly`

---
## 解題思路
先把相鄰兩數的差的絕對值算出來，再進行由小到大排序，之後用個迴圈跑`1 ~ n`(因為包前不包後，所以取n就好了)，對照陣列中的每個元素是否剛好是`i`(就是判斷陣列是否為1 ~ n-1)

***由於陣列index是從0開始，所以在跑迴圈時要用`jollies[i-1]`來判斷(i一開始是1)***

## Python程式碼
```python
# UVa 10038 - Jolly Jumpers
def solve(n, nums):
    jollies = sorted([abs(nums[i] - nums[i-1]) for i in range(1, n)])
    for i in range(1, n):
        if jollies[i-1] != i: # 因為index是從0開始算，所以要用jollies[i-1]
            return 'Not jolly'
    return 'Jolly'

while True:
    try:
        nums = list(map(int, input().split()))
        print(solve(nums[0], nums[1:]))
    except EOFError:
        break
# Accepted	PYTH3	0.010
```