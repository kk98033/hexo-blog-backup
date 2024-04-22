---
title: Python UVa 11063 - B2-Sequence
date: 2024-04-21 13:54:58
tags:
  - UVa
  - Python
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 這個問題要求我們檢查一組數列是否為 B2-Sequence。這題不複雜，正常做不會超時，只不過要小心 B2-Sequence 的定義！ - Python UVa 11063 - B2-Sequence 解題心得
description: 這個問題要求我們檢查一組數列是否為 B2-Sequence。這題不複雜，正常做不會超時，只不過要小心 B2-Sequence 的定義！ - Python UVa 11063 - B2-Sequence 解題心得
---
# UVa 11063 - B2-Sequence 解法

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=2004) - UVa 11063 - B2-Sequence


## 題意
這個問題要求我們檢查一組數列是否為 B2-Sequence。
根據題目的定義，一個數列 B（1 ≤ b1 < b2 < b3 ...）被稱為 B2-Sequence 的條件是：

1. 所有 B[i]+B[j]（其中 i≤j）的和都是 ***唯一*** 的。
2. 數列 B 必須是 ***嚴格遞增的***。
3. 數列的每個數字都要 ***大於等於一***

{% note primary %}
這題的 input/output 很討厭，要小心格式：
每比測資的 input 之間有個空格！
每筆輸出的答案 output 之間有個空格!
{% endnote %}

#### Sample Input 
{% codeblock line_number:false %}
4
1 2 4 8

4
3 7 10 14
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
Case #1: It is a B2-Sequence.

Case #2: It is not a B2-Sequence.
{% endcodeblock %}

---

## 解題思路
這題就直接照著題目意思做就好了，不會超時 :D。

首先，先檢查 b1, b2 ... bn 是否是嚴格遞增的 **（嚴格遞增就是每項皆不相同，並且遞增）**。
***很重要的一點是，如果有任何數字小於一皆是為不是 B2-Sequence!***
```python
for i in range(1, n):
    if nums[i] < 1 or nums[i-1] < 1: return 'It is not a B2-Sequence.'
    if nums[i-1] >= nums[i]: return 'It is not a B2-Sequence.'
```

接下來可以直接**窮舉**來計算每個 **B[i]+B[j]（其中 i≤j）的和**，我這裡用了一個陣列 `seen`，來記錄出現過的數字，如果 B[i]+B[j] 的值出現在 seen 裡面代表有重複的和，代表不是 B2-Sequence。
### 用 list 的方法
```python
seen = []
for i in range(n):
    for j in range(i, n):
        if nums[i] + nums[j] in seen: return 'It is not a B2-Sequence.'
        seen.append(nums[i] + nums[j])
# 0.140
```

### 用 set 的方法
```python
seen = set()
for i in range(n):
    for j in range(i, n):
        sum_val = nums[i] + nums[j]
        if sum_val in seen:
            return 'It is not a B2-Sequence.'
        seen.add(sum_val)
# 0.050
```

### 用 dict 的方法
```python
seen = {}
for i in range(n):
    for j in range(i, n):
        sum_val = nums[i] + nums[j]
        if sum_val in seen:
            return 'It is not a B2-Sequence.'
        seen[sum_val] = True
# 0.020
```

{% note info %}
在 python 用 `set` 或 `dict` 的**搜尋**和**插入**速度會**比陣列快**，不過這題應該是不會有超時的問題。
我試過了，從原本的 0.140(用 lsit) -> 0.050(用 set)
{% endnote %}

## Python程式碼
```python
# UVa 11063 - B2-Sequence
def solve(n, nums):
    for i in range(1, n):
        if nums[i] < 1 or nums[i-1] < 1: return 'It is not a B2-Sequence.'
        if nums[i-1] >= nums[i]: return 'It is not a B2-Sequence.'

    seen = []
    for i in range(n):
        for j in range(i, n):
            if nums[i] + nums[j] in seen: return 'It is not a B2-Sequence.'
            seen.append(nums[i] + nums[j])
    return 'It is a B2-Sequence.'

count = 1
while True:
    try:
        n = int(input())
        nums = list(map(int, input().split()))
        print(f'Case #{count}: {solve(n, nums)}')
        print()
        count += 1
        input()
    except EOFError:
        break
#	Accepted	PYTH3	0.140
```