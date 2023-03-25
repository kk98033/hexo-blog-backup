---
title: Python UVa - 11240 - Antimonotonicity
date: 2023-03-22 10:21:10
tags:
  - UVa
categories:
  - [解題報告, UVa]
  - [CPE歷屆, CPE 2023/03/21]
excerpt: 這題是2023/3/21 CPE 的第四題，我覺得這題類似買股票的題目，用Greedy是找高點和低點。直接用迴圈跑一次就可以解出來了 - Python UVa - 11240 - Antimonotonicity 解題報告，
description: 這題是2023/3/21 CPE 的第四題，我覺得這題類似買股票的題目，用Greedy是找高點和低點。直接用迴圈跑一次就可以解出來了 - Python UVa - 11240 - Antimonotonicity 解題報告
---
# Python UVa - 11240 - Antimonotonicity

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&category=0&problem=2181&mosmsg=Submission+received+with+ID+28325133) - UVa - 11240 - Antimonotonicity 

### 這題是2023/3/21 CPE 的第四題
我覺得這題類似買股票的題目，用Greedy是找高點和低點。直接用迴圈跑一次就可以解出來了

## 題意
輸入一個陣列，代表`Fred`，算出在`Fred`中，符合`Mary`最長的子序列(subsequence)，[subsequence定義](https://web.ntnu.edu.tw/~algo/Subsequence.html)

Mary需要符合以下條件
`Mary[0] > Mary[1] < Mary[2] > Mary[3] < ...`

{% note primary %}
- `子序列(subsequence)`定義最好看一下
{% endnote %}

#### Sample Input 
`4`
`5 1 2 3 4 5`
`5 5 4 3 2 1`
`5 5 1 4 2 3`
`5 2 4 1 3 5`

#### Sample Output 
`1`
`2`
`5`
`3`

---
## 解題思路
這題是要在`Fred`裡找出`Mary`最長的長度。

由於`Mary`的pattern是`大、小、大、小 ...`，所以只需在`Fred`中找出這樣的pattern，由於是找[subsequence](https://web.ntnu.edu.tw/~algo/Subsequence.html)，所以直接用一個迴圈判斷符合`大、小、大、小 ...`順序的元素有幾個就可以了

我是先設一個變數`current`，代表目前是`大`(current = 1)還是`小`(current = -1)，因為是從`大`開始，所以`current`先設為`1`，之後依序找`Fred`的值，用`nums[i-1] - nums[i]`代表`nums[i]`是屬於大還是小(> 0 為大, < 0 為小>)，如果`current`與目前大小一樣，就讓`current *= -1`(切換目前是要找大還是找小)，並且`ans + 1`，最後`ans`就是答案了

*註：`ans`一開始記得設為1，因為無論如何Mary長度一定會大於等於一*

## 程式碼
```python
# UVa - 11240 - Antimonotonicity
# 2023/03/21/ CPE - 4
def solve(n, nums):
    current = 1 
    ans = 1
    for i in range(1, n):
        if current == 1:
            if nums[i-1] - nums[i] > 0: 
                ans += 1
                current *= -1
        else:
            if nums[i-1] - nums[i] < 0: 
                ans += 1
                current *= -1
    return ans

T = int(input())
for t in range(T):
    nums = list(map(int, input().split()))

    print(solve(nums[0], nums[1:]))
# Accepted	PYTH3	0.460
```