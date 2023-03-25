---
title: Python UVa 11824 - A Minimum Land Price
date: 2023-03-23 15:20:13
tags:
  - UVa
  - UVa 一星
categories:
  - [解題報告, UVa]
  - [CPE歷屆, CPE 2023/03/21]
excerpt: 這題是2023/3/21 CPE 的第二題, UVa 11824 - A Minimum Land Price 解題報告
description: 這題是2023/3/21 CPE 的第二題, UVa 11824 - A Minimum Land Price 解題報告
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
這題用[Stack](https://www.geeksforgeeks.org/stack-in-python/)做比較容易
`stack.append()`：新增元素
`stack.pop()`：拿出元素

當遇到**Sleep**時，就把X加入stack，遇到**Kick**，就pop掉，遇到**Test**就print出stack最上面的元素(stack[-1])
***在pop時要注意stack是否有元素，如果沒有就print('Not in a dream')***

## 程式碼
```python
# UVa 13055 - Inception
# 2023/03/21/ CPE - 3
def solve(n, codes):
    dream = []
    for code in codes:
        if code[0] == 'Sleep':
            dream.append(code[1])
        elif code[0] == 'Test':
            if dream:
                print(dream[-1])
            else:
                print('Not in a dream')
        elif code[0] == 'Kick':
            if dream:
                dream.pop()

n = int(input())
while True:
    try:
        codes = []
        for i in range(n):
            codes.append(input().split())
        solve(n, codes)
    except EOFError:
        break
# Accepted	PYTH3	0.160
```