---
title: Python UVa 11005 - Cheapest Base
date: 2024-04-19 13:22:10
tags:
  - UVa
  - Python
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 這題題目看起有點複雜，但簡單來說，就是進位制轉換，然後算出多少進位制的成本最便宜。 - Python UVa 11005 - Cheapest Base 解題心得
description: 這題題目看起有點複雜，但簡單來說，就是進位制轉換，然後算出多少進位制的成本最便宜。 - Python UVa 11005 - Cheapest Base 解題心得
---
# UVa 11005 - Cheapest Base Python 解法

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1946) - UVa 11005 - Cheapest Base


## 題意
在這題中，我們首先會得到一個數字，代表有幾的測資。
接下來會給我們一組每個數字（0-35）的成本，***共輸入四行！*** 
再來輸入一個數 n，代表接下來要計算 n 個數字的成本
最後我們需要針對這些 n 個數字，計算出它們在每個可能的基數（從2到36）的成本，然後找出哪個基數可以使得成本最低。

{% note info %}
* 成本最低的基數可能不只一個！
* 要注意 output 格式！
* 輸出的每個測資之間都要有個空格！
{% endnote %}

#### Sample Input 
{% codeblock line_number:false %}
2
10 8 12 13 15 13 13 16 9
11 18 24 21 23 23 23 13 15
17 33 21 23 27 26 27 19 4
22 18 30 30 24 16 26 21 21
5
98329921
12345
800348
14
873645
1 1 1 1 1 1 1 1 1
1 1 1 1 1 1 1 1 1
1 1 1 1 1 1 1 1 1
1 1 1 1 1 1 1 1 1
4
0
1
10
100
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
Case 1:
Cheapest base(s) for number 98329921: 24
Cheapest base(s) for number 12345: 13 31
Cheapest base(s) for number 800348: 31
Cheapest base(s) for number 14: 13
Cheapest base(s) for number 873645: 22
Case 2:
Cheapest base(s) for number 0: 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19
20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36
Cheapest base(s) for number 1: 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 18 19
20 21 22 23 24 25 26 27 28 29 30 31 32 33 34 35 36
Cheapest base(s) for number 10: 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
26 27 28 29 30 31 32 33 34 35 36
Cheapest base(s) for number 100: 11 12 13 14 15 16 17 18 19 20 21 22 23 24 25
26 27 28 29 30 31 32 33 34 35 36
{% endcodeblock %}

---

## 解題思路
這題簡單來說就是把數字轉成 n 進位，並且計算轉成 n 進位所需要花的成本。
這題可以直接計算每個數字 ***從 2 到 37*** 進位的所有成本，然後找出最少成本的進位（不會超時 :D）

可以用 python 的**字典**建立數字的成本表（用字典搜尋速度會比較快，如果要直接用陣列應該也沒問題）：

```python
tableDict = { i: table[i] for i in range(36) }
```
我這裡用了 Python 的字典生成式（dictionary comprehension），它是一種快速的建立字典的方法。具體來說：
* {} 表示創建一個字典。
* i: table[i] 表示字典的鍵和值的對應關係，其中 `i` 是 key，`table[i]` 是 value。
* for i in range(36) 是一個迴圈，它遍歷從0到35的數字，這正好對應於數字的成本表的索引。

我是用一個陣列來存每個 base 的成本 `costs = [0] * 37`。
接下來我們就可以一一的計算每個數字從 2 ~ 37 的成本：
```python
tempNum = n
cost = 0
while tempNum:
    cost += tableDict[tempNum % base]
    tempNum //= base
costs[base] = cost
```
這段程式碼首先將要計算的數字 n 複製到臨時變數 tempNum（這樣才不會改到 n），並用cost為來累計成本。
接著就跟**計算十進位轉二進位**一樣，透過一個 `while 迴圈`，持續將 tempNum 除以基數 base，並將 ***餘數*** 對應的成本（也就是轉成 xx 進位的數字）加到 cost 上，***直到 tempNum 為0***。最後，將計算出的成本 cost 儲存至 costs 列表的對應位置。這樣我們就得到了數字在該基數下的總成本。

最後，使用 `min(costs)` 來取得全部成本的最小值，使用 `enumerate(costs)` 取得 costs 陣列的 `index` 以及 `值`，並且檢查他是否符合最小的成本，如果符合，將他 append 到 `ans` 陣列中，最終 ans 裡面的值就是答案了（要小心處理輸出格式，我這裡就不解釋我的作法了）！

## Python程式碼
```python
# UVa 11005 - Cheapest Base
def solve(table, nums):
    tableDict = { i: table[i] for i in range(36) }

    for n in nums:
        costs = [0] * 37
        ans = []
        for base in range(2, 37):
            tempNum = n
            cost = 0
            while tempNum:
                cost += tableDict[tempNum % base]
                tempNum //= base
            costs[base] = cost

        minCost = min(costs[2:])
        for base, cost in enumerate(costs):
            if base == 0 or base == 1: continue
            if cost == minCost:
                ans.append(base)
        print(f'Cheapest base(s) for number { n }: { " ".join(map(str, ans)) }')

T = int(input())
for t in range(T):
    table = []
    for i in range(4):
        table.extend(list(map(int, input().split())))

    nums = []
    for i in range(int(input())):
        nums.append(int(input()))

    if t != 0: print()
    print(f'Case { t+1 }:')
    solve(table, nums)
# Accepted	PYTH3	0.070
```