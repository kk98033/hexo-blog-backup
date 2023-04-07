---
title: Python UVa 10057 - A mid-summer night's dream.
date: 2023-03-25 14:04:06
tags:
  - UVa
  - UVa 一星
categories:
  - 解題報告
  - UVa
excerpt: 這題跟[UVa 10041 - Vito's Family](https://blog.iddle.dev/public/2023/03/18/Python-UVa-10041-Vito-s-Family/)很類似，也都是要找 ***中位數*** ，只不過這題需要把所有符合的中位數給找出來。 - Python UVa 10057 - A mid-summer night's dream. 解題報告
description: 這題跟[UVa 10041 - Vito's Family](https://blog.iddle.dev/public/2023/03/18/Python-UVa-10041-Vito-s-Family/)很類似，也都是要找 ***中位數*** ，只不過這題需要把所有符合的中位數給找出來。 - 解題報告
---
# Python UVa 10057 - A mid-summer night's dream.

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=998) - UVa 10057 - A mid-summer night's dream.

這題跟[UVa 10041 - Vito's Family](https://blog.iddle.dev/public/2023/03/18/Python-UVa-10041-Vito-s-Family/)很類似，也都是要找 ***中位數*** ，只不過這題需要把所有符合的中位數給找出來。

## 題意
輸入多組數字，`X1, X2, ... Xn`，找到一個`A`使得：
|X1 − A| + |X2 − A| + . . . + |Xn − A|
為最小值

**輸出三個整數**
{% note info %}
 - 第一個數字是能得到該算式最小值的A。(中位數取最小)
 - 第二個數字是|Xi − A|為最小值的數量。(也就是中位數數量)
 - 第三行數字是可能有幾種最小值。(當有偶數個的時候才需要算，中位數1 - 中位數2)
{% endnote %}

這題很討厭，我覺得題目沒有說的很清楚

#### Sample Input 
`2`
`10`
`10`
`4`
`1`
`2`
`2`
`4`

#### Sample Output 
`10 2 1`
`2 2 1`
---
## 解題思路
這題是找中位數，一樣先把他排序，在找最中間的數
**但這題需要把偶數個數中間的兩個中位數給找出來分開討論**

**對於輸出的第一個數 *‘|Xi − A|為最小值的數量’***
=> 找最小的那個中位數就可以了

**對於輸出的第二個數 *‘’***



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