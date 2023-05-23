---
title: Python UVa 10190 - Divide, But Not Quite Conquer
date: 2023-03-18 15:56:18
tags:
  - UVa
  - UVa 一星
categories:
  - 解題心得
  - UVa
excerpt: Python UVa 10190 - Divide, But Not Quite Conquer 解題心得
description: Python UVa 10190 - Divide, But Not Quite Conquer 解題心得
# urlname: Python UVa 10190 - Divide, But Not Quite Conquer
---
# Python UVa 10190 - Divide, But Not Quite Conquer

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=0&problem=1131&mosmsg=Submission+received+with+ID+28315580) - UVa 10190 - Divide, But Not Quite Conquer!



## 題意
給予一個`n`，將他除以`m`直到`n = 1`，如果`n`無法整除`m`，輸出`Boring!`，反之，輸出`n n/m n/m/m ... 1`

{% note primary %}
 - 注意！當n, m有一個等於0或者是1時，直接輸出Boring!
{% endnote %}

#### Sample Input 
`125 5`
`30 3`
`80 2`
`81 3`

#### Sample Output 
`125 25 5 1`
`Boring!`
`Boring!`
`81 27 9 3 1`

---
## 解題思路
直接用個while迴圈判斷n是否大於1，如果n不能整除m，就直接跳出迴圈輸出Boring!
你可以用陣列或者是字串的形式來記錄n的變化，我個人是比較喜歡用陣列
*註：我程式碼的n,m跟題目的n,m是相反的*



## Python程式碼
```python
# UVa 10190 - Divide, But Not Quite Conquer!
def solve(m, n):
    if m == 0 or n == 0 or m == 1 or n == 1: return 'Boring!' 
    ans = [m] # use list
    while m > 1:
        if m % n != 0: return 'Boring!'
        ans.append(m // n)
        m //= n
    return ' '.join([str(i) for i in ans])

def solve2(m, n):
    if m == 0 or n == 0 or m == 1 or n == 1: return 'Boring!' 
    ans = f'{m}' # use string
    while m > 1:
        if m % n != 0: return 'Boring!'
        ans += f' {m // n}'
        m //= n
    return ans

while True:
    try:
        m, n = list(map(int, input().split()))

        print(solve(m, n))
    except EOFError:
        break
# Accepted	PYTH3	0.000
```