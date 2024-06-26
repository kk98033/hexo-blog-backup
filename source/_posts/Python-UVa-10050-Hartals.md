---
title: Python UVa 10050 - Hartals
date: 2023-03-20 19:29:06
tags:
  - UVa
  - UVa 一星
categories:
  - 解題心得
  - UVa
excerpt: Python UVa 10050 - Hartals 解題心得
description: Python UVa 10050 - Hartals 解題心得
# urlname: Python UVa 10050 - Hartals
---
# Python UVa 10050 - Hartals

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=991) - UVa 10050 - Hartals



## 題意
有`p`個政黨，每個政黨都會每隔幾天罷工一次，**假日放假不罷工**，給予`N`天，求出在`N`天裡面有幾天會因為罷工無法工作

範例：
有三個政黨`p1 = 3`, `p2 = 4`, `p3 = 8`, `N = 14`，答案為:`5`，圖表如下
| Days  |   1   |   2   |   3   |   4   |   5   | 6<br>Sat | 7<br>Sun |   8   |   9   |  10   |  11   |  12   | 13<br>Sat | 14<br>Sun |
| :---: | :---: | :---: | :---: | :---: | :---: | :------: | :------: | :---: | :---: | :---: | :---: | :---: | :-------: | --------- |
|  P1   |       |       |   X   |       |       |    X     |          |       |   X   |       |       |   X   |           |           |
|  P2   |       |       |       |   X   |       |          |          |   X   |       |       |       |   X   |           |           |
|  P3   |       |       |       |       |       |          |          |   X   |       |       |       |       |           |           |
|       |       |       |   1   |   2   |       |          |          |   3   |   4   |       |       |   5   |           |           |

**要注意的是第6天，因為是假日，所以不罷工，不能把它算進去**

{% note primary %}
 - 要記得處理假日不罷工的情況
 - 當一天有好幾個政黨罷工一樣視為一
{% endnote %}

#### Sample Input 
`2`
`14`
`3`
`3`
`4`
`8`
`100`
`4`
`12`
`15`
`25`
`40`

#### Sample Output 
`5`
`15`

---
## 解題思路
這題是模擬題，我的解法是把每個政黨的罷工日期用while迴圈模擬跑一遍並給記下來，存在`set`(set裡的值不會重複)裡，最後`set`的長度就是答案了

至於要判斷是否為假日可以用`temp % 7 != 6`判斷禮拜六，`temp % 7 != 0`判斷禮拜日

## Python程式碼
```python
# UVa 10050 - Hartals
def solve(days, party):
    strike = set()
    for i in party:
        temp = i # 模擬從第i天罷工直到第N天
        while temp <= days:
            if temp % 7 != 6 and temp % 7 != 0:
                strike.add(temp)
            temp += i

    return len(strike)

T = int(input())
for t in range(T):
    days = int(input())
    party = []
    for i in range(int(input())):
        party.append(int(input()))

    print(solve(days, party))
# Accepted	PYTH3	0.010
```