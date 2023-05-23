---
title: Python UVa 10189 - Minesweeper
date: 2023-05-23 09:29:27
tags:
  - UVa
  - UVa 一星
  - CPE 49題必考題
categories:
  - 解題心得
  - UVa
excerpt: 這題挺簡單的，只需算出各字元ASCII出現的次數即可，對於Python來說可以輕鬆秒殺，不過要記得注意他的輸出格式。 - Python UVa 10062 - Tell me the frequencies. 解題心得
description: 這題挺簡單的，只需算出各字元ASCII出現的次數即可，對於Python來說可以輕鬆秒殺，不過要記得注意他的輸出格式。 - Python UVa 10062 - Tell me the frequencies. 解題心得
---
# UVa 10189 - Minesweeper

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=1130) - UVa 10189 - Minesweeper


## 題意
給予一個 **M * N** 的地圖，找出每個位置周圍 **3 * 3** 的區域有多少地雷


#### Sample Input 
{% codeblock line_number:false %}
4 4
*...
....
.*..
....
3 5
**...
.....
.*...
0 0
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
Field #1:
*100
2210
1*10
1110

Field #2:
**100
33200
1*100
{% endcodeblock %}

{% note warning %}
* 每個輸出之間都要間隔一行
* 遇到地雷直接輸出地雷
{% endnote %}

---

## 解題思路
這題只需要

## Python程式碼
```python
# UVa 10189 - Minesweeper
def solve(row, col, field):
    checks = [
        # (row, col)
        (-1, -1), # upper left
        (-1, 0), # up
        (-1, 1), # upper right

        (0, -1), # left
        (0, 1), # right

        (1, -1), # down left
        (1, 0), # down
        (1, 1), # down right 
    ]

    ans = [[0 for c in range(col)] for r in range(row)]

    for r in range(row):
        for c in range(col):
            if field[r][c] == '*':
                ans[r][c] = '*'
                continue

            count = 0
            for check in checks:
                if 0 <= r + check[0] < row and 0 <= c + check[1] < col:
                    if field[r+check[0]][c+check[1]] == '*':
                        count += 1
            ans[r][c] = str(count)

    return '\n'.join([''.join(i) for i in ans])

t = 1
while True:
    try:
        row, col = list(map(int, input().split()))

        if row == 0 and col == 0: break

        field = []
        for i in range(row):
            field.append(list(input()))

        if t != 1: print()
        print(f'Field #{t}:\n{solve(row, col, field)}')

        t += 1
    except EOFError:
        break
# Accepted	PYTH3	0.050
```