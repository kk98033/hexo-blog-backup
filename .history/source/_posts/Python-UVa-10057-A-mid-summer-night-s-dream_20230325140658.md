---
title: Python UVa 10057 - A mid-summer night's dream.
date: 2023-03-25 14:04:06
tags:
  - UVa
  - UVa 一星
categories:
  - 解題報告
  - UVa
excerpt: Python UVa 10057 - A mid-summer night's dream. 解題報告
description: Python UVa 10057 - A mid-summer night's dream. 解題報告
---
# Python UVa 10057 - A mid-summer night's dream.

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=998) - UVa 10057 - A mid-summer night's dream.

這題跟[UVa 10041 - Vito's Family](https://blog.iddle.dev/public/2023/03/18/Python-UVa-10041-Vito-s-Family/)很類似，也都是要找 ***中位數*** 

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



## 程式碼
```python
# UVa 10057 - A mid-summer night's dream.
def solve(n, nums):
    nums.sort()
    mid1, mid2 = n // 2, n // 2 - 1
    if n % 2 == 0:
        if nums[mid1] == nums[mid2]:
            return f'{min(nums[mid1], nums[mid2])} {nums.count(nums[mid1])} {nums[mid1] - nums[mid2] + 1}'
        return f'{min(nums[mid1], nums[mid2])} {nums.count(nums[mid1]) + nums.count(nums[mid2])} {nums[mid1] - nums[mid2] + 1}'
    return f'{nums[mid1]} {nums.count(nums[mid1])} 1'

while True:
    try:
        n = int(input())
        nums = []
        for i in range(n):
            nums.append(int(input()))

        print(solve(n, nums))
    except EOFError:
        break
# Accepted	PYTH3	5.410
```