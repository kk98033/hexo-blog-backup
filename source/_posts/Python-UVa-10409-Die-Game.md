---
title: Python UVa 10409 - Die Game
date: 2024-04-06 11:59:09
tags:
  - UVa
categories:
  - [解題心得, UVa]
excerpt: 這題應該算是模擬題，只要知道骰子的排列規則就可以輕鬆解決了！ - Python UVa 10409 - Die Game 解題心得
description: 這題應該算是模擬題，只要知道骰子的排列規則就可以輕鬆解決了！ - Python UVa 10409 - Die Game 解題心得
---
# Python UVa 10409 - Die Game 解法

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1350) - UVa 10409 - Die Game


## 題意
我們需要模擬一個六面的骰子在平面上滾動的情況。給定一系列的指令，包括`north`、`south`、`east`、`west`，我們需要確定最終骰子的**頂面**和**前面**的數字。

一開始骰子的一、二和三分別位於骰子的`頂部`、`北面`和`西面`，類似這樣：
{% codeblock line_number:false %}
       2
    3  1  4  6
       5
{% endcodeblock %}

{% note primary %}
 - 每行會先輸入一個數字 n，代表有 n 指令。
 - 如果輸入為 0 就結束。
{% endnote %}

#### Sample Input 
{% codeblock line_number:false %}
1
north
3
north
east
south
0
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
5
1
{% endcodeblock %}

---

## 解題思路
這題重要的部分是要知道骰子的特性：
**對面之和是否為7：**標準骰子的一個特性是任何一面和它的對面之和都應該是7。知道這個規則後，我們就可以利用它來確定其他面的點數了。

接下來只要利用他，加上一點點的想像力，模擬骰子滾動即可（始骰子位置可以參考上面題意裡的骰子展開圖）！

模擬骰子滾動：
**向東（east）**：
1. `north` 保持不變
2. `west` 成為原來頂部的相對面（`7 - top`）
3. `top` 則變成原來 `west` 的值。

**向南（south）**：
1. `north` 變成原來頂部的相對面（`7 - top`）
2. `west` 保持不變
3. `top` 變成原來 `north` 的值。

**向西（west）**：
1. `north` 保持不變
2. `west` 變成原來頂部的值
3. `top` 則變成原來 `west` 的相對面（`7 - west`）。

**向北（north）**：
1. `north` 變成原來頂部的值
2. `west` 保持不變
3. `top` 變成原來 `north` 的相對面（`7 - north`）。

以下是滾動規則表格：
| 方向         | north 變化                                       | west 變化                                       | top 變化                                                  |
| ------------ | ------------------------------------------------ | ----------------------------------------------- | --------------------------------------------------------- |
| 向東 (east)  | ***north*** 保持不變                             | ***west*** 成為原來頂部的相對面 (***7 - top***) | ***top*** 變成原來 ***west*** 的值                        |
| 向南 (south) | ***north*** 變成原來頂部的相對面 (***7 - top***) | ***west*** 保持不變                             | ***top*** 變成原來 ***north*** 的值                       |
| 向西 (west)  | ***north*** 保持不變                             | ***west*** 變成原來頂部的值                     | ***top*** 變成原來 ***west*** 的相對面 (***7 - west***)   |
| 向北 (north) | ***north*** 變成原來頂部的值                     | ***west*** 保持不變                             | ***top*** 變成原來 ***north*** 的相對面 (***7 - north***) |


最後只需要依照題目要求回傳 `top` 即可。

## Python程式碼
```python
# UVa 10409 - Die Game
def solve(codes):
    # "east"、"south"、"west"、"north"。
    '''
           2
        3  1  4  6
           5
    '''
    north, west, top = 2, 3, 1
    for code in codes:
        if code == 'east':
            north, west, top = north, 7 - top, west
        elif code == 'south':
            north, west, top = 7 - top, west, north
        elif code == 'west':
            north, west, top = north, top, 7 - west
        else: # north
            north, west, top = top, west, 7 - north
    return top

while True:
    try:
        n = int(input())
        if n == 0: break

        codes = []
        for _ in range(n):
            codes.append(input())

        print(solve(codes))
    except EOFError:
        break
# Accepted	PYTH3	0.020
```