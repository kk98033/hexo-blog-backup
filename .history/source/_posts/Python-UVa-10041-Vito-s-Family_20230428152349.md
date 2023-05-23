---
title: Python UVa 10041 - Vito's Family
date: 2023-03-18 15:14:59
tags:
  - UVa
  - UVa 一星
categories:
  - 解題心得
  - UVa
excerpt: Python UVa 10041 - Vito's Family 解題心得
description: Python UVa 10041 - Vito's Family 解題心得
# urlname: Python UVa 10041 - Vito's Family
---

# Python UVa 10041 - Vito's Family

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=982) - UVa 10041 - Vito's Family



## 題意
給予一個陣列，找出Vito家應該要設在哪個位置，使得他與陣列中的所有元素距離和為最小

{% note primary %}
 - 注意輸入的測資，每行的第一個數字代表後面陣列大小
{% endnote %}

#### Sample Input 
`2`
`2 2 4`
`3 2 4 6`

#### Sample Output 
`2`
`4`

---
## 解題思路
這題其實只需要找**中位數**，再將中位數與其他元素的差加起來即可。
要找出中位數只需要將陣列排序，再找出`n//2`位置的值，即是中位數。
一開始我是把奇數長度和偶數長度的陣列分開算，但我發現由於最後要把中位數跟其他的元素相減，其實就算陣列是偶數個也只需要找出第n//2的值作為中位數，有興趣可以自己算算看。
*註：記得相減時要套上絕對值*



## Python程式碼
```python
# UVa 10041 - Vito's Family
def solve2(n, nums):
    nums.sort()
    mid = nums[n//2]
    ans = 0
    for i in nums:
        ans += abs(i - mid)
    return ans

T = int(input())
for t in range(T):
    n = list(map(int, input().split()))
    print(solve2(n[0], n[1:]))
# Accepted	PYTH3	0.040
```