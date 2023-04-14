---
title: Python UVa 11824 - A Minimum Land Price
date: 2023-03-23 15:20:13
tags:
  - UVa
  - UVa 一星
categories:
  - [解題心得, UVa]
  - [CPE歷屆, CPE 2023/03/21]
excerpt: 這題是2023/3/21 CPE 的第二題, UVa 11824 - A Minimum Land Price 解題心得
description: 這題是2023/3/21 CPE 的第二題, UVa 11824 - A Minimum Land Price 解題心得
---
# UVa 160 - Factors and Factorials

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=2924) - UVa 11824 - A Minimum Land Price

這題是2023/3/21 CPE 的第二題

## 題意
輸入`T`，代表接下有`T`組測資，接下來輸入多個數字，每組測資以`0`分割，**找出最小的價格**。

**價格的計算方法如下：**
假設是[7, 2, 3]
價格是：(2 × 7) + (2 × 2^2) + (2 × 10^3) = 2022 millions baht

如果最低的價格超出了**5,000,000**，輸出`Too expensive`

{% note primary %}
 - **注意輸入的格式**
{% endnote %}

#### Sample Input 
```text
3
7
2
10
0
20
29
31
0
42
41
40
37
20
0
```

#### Sample Output 
```text
134
17744
Too expensive
```

---
## 解題思路
這題只需要先由大到小排序，再帶入公式就會是價格的最小值了，記得處理 **>5,000,000**的狀況

## 程式碼
```python
# UVa 11824 - A Minimum Land Price
# 2023/03/21/ CPE - 2
def solve(nums):
    nums = sorted(nums)[::-1]
    ans = 0
    for i, value in enumerate(nums):
        ans += 2 * (value ** (i + 1))
    return ans if ans < 5000000 else 'Too expensive'

T = int(input())
nums = []
for _ in range(T):
    nums = []
    while True:
        temp = int(input())
        if temp == 0: 
            print(solve(nums))
            break
        nums.append(temp)
# Accepted	PYTH3	0.040
```