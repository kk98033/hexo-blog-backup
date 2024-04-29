---
title: Python UVa 10908 - Largest Square
date: 2024-04-28 21:24:36
tags:
  - UVa
  - Python
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 這題是找出矩陣中最大的同字符正方形的邊長，直接一個一個邊長試就好了。 - Python UVa 10908 - Largest Square 解題心得
description: 這題是找出矩陣中最大的同字符正方形的邊長，直接一個一個邊長試就好了。 - Python UVa 10908 - Largest Square 解題心得
---
# UVa 10908 - Largest Square Python 解法

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1849) - UVa 10908 - Largest Square


## 題意
給定一個 `M x N` 的字符矩陣和`數個查詢`，每個查詢指定矩陣中的一個點，任務是確定以該點為**中心**的 ***最大同字符正方形的邊長***。這需要判斷從該點向四周擴展時，是否形成邊長相等的正方形，且這些擴展的區域內的字符必須全部相同。

輸入包括一個整數 `T`，代表有 `T` 個測試案例。每個案例開始於包含三個整數 `M`、`N` 和 `Q`，這裡 `M` 和 `N` 分別表示網格的**行數**和**列數**；`Q` 表示**查詢的數量**。接著是 `M` 行，每行有 `N` 個字符組成的矩陣。最後是 `Q` 行，每行包含兩個整數 `r`和 `c`，代表要查詢的矩陣中心點的座標。這些座標是從零開始的索引，用於確定查詢的起始點。

#### Sample Input 
{% codeblock line_number:false %}
1
7 10 4
abbbaaaaaa
abbbaaaaaa
abbbaaaaaa
aaaaaaaaaa
aaaaaaaaaa
aaccaaaaaa
aaccaaaaaa
1 2
2 4
4 6
5 2
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
7 10 4
3
1
5
1
{% endcodeblock %}

---

## 解題思路
這題的邏輯很簡單，我們只需要檢查的中心座標周圍一圈，如果都是一樣的字符，就繼續檢查下一圈，不一樣就停止，然後輸出答案，具體來說如下：
1. 檢查以 (r, c) 為中心的正方形是否所有元素都相同。
2. 如果相同，則增加正方形的邊長並繼續擴展。
3. 如果不同，則停止擴展並記錄當前的最大邊長。

簡單介紹一下我的程式碼：
一開始我是用一個 **while** 迴圈從邊長 **1** 開始檢查，我這裡用 `length` 來記錄邊長，`index` 來記錄目前正方形擴展的圈數，`newLength` 為下一個檢查的邊長。

首先我先將 `length` + 2，為 `newLength`（因為每擴展一圈，邊長都會多二）
找出新的正方形最左上角的座標：
```python
topLeftRow, topLeftCol = row - index, col - index # 新的正方形左上角座標
```

接下來要檢查一下這個正方形是否超出 m, n 的範圍（如果超出就回傳上一個正方形的 length）：
```python
# 檢查邊界
if not (0 <= topLeftRow < m and 0 <= topLeftCol < n and 
        topLeftRow + newLength - 1 < m and topLeftCol + newLength - 1 < n):
        return length # 超出範圍，回傳前一個邊長
```
* **0 <= topLeftRow < m** 和 **0 <= topLeftCol < n**： 檢查正方形的**左上角**位於矩陣內。

* **topLeftRow + new_length - 1 < m** 和 **topLeftCol + new_length - 1 < n**： 檢查正方形的**底部**和**右邊**位於矩陣內。


接下來檢查正方形新的外圍一圈是否跟中心點相同（如果不符合就回傳上一個的 length）
```python
# 檢查邊框上的所有字符是否與中心點字符相同
for i in range(newLength):
    # 檢查上下邊框和左右邊框            
    if not (matrix[topLeftRow][topLeftCol + i] == mid and
            matrix[topLeftRow + newLength - 1][topLeftCol + i] == mid and
            matrix[topLeftRow + i][topLeftCol] == mid and
            matrix[topLeftRow + i][topLeftCol + newLength - 1] == mid):
        return length # 不相同，回傳前一個邊長
```
* **matrix[topLeftRow][topLeftCol + i]**: 檢查正方形頂部邊框的字符。這裡從正方形的左上角開始，向右遍歷到右上角。

* **matrix[topLeftRow + new_length - 1][topLeftCol + i]**: 檢查正方形底部邊框的字符。同樣從左下角到右下角。

* **matrix[topLeftRow + i][topLeftCol]**: 檢查正方形左邊邊框的字符。這從左上角向下遍歷到左下角。

* **matrix[topLeftRow + i][topLeftCol + new_length - 1]**: 檢查正方形右邊邊框的字符。從右上角向下到右下角。

## Python程式碼
```python
# UVa 10908 - Largest Square
def check(m, n, row, col):
    mid = matrix[row][col]
    length = 1
    index = 1
    while True:
        newLength = length + 2
        topLeftRow, topLeftCol = row - index, col - index # 新的正方形左上角座標

        # 檢查邊界
        if not (0 <= topLeftRow < m and 0 <= topLeftCol < n and 
                topLeftRow + newLength - 1 < m and topLeftCol + newLength - 1 < n):
            return length
        
        # 檢查邊框上的所有字符是否與中心點字符相同
        for i in range(newLength):
            # 檢查上下邊框和左右邊框            
            if not (matrix[topLeftRow][topLeftCol + i] == mid and
                    matrix[topLeftRow + newLength - 1][topLeftCol + i] == mid and
                    matrix[topLeftRow + i][topLeftCol] == mid and
                    matrix[topLeftRow + i][topLeftCol + newLength - 1] == mid):
                return length # 不相同，回傳前一個邊長

        length = newLength
        index += 1
        
T = int(input())
for t in range(T):
    m, n, q = list(map(int, input().split()))
    matrix = []
    for i in range(m):
        matrix.append(list(input()))

    centers = []
    for i in range(q):
        centers.append(list(map(int, input().split())))

    print(f'{ m } { n } { q }')
    for row, col in centers:
        print(check(m, n, row, col))    
# Accepted	PYTH3	0.050
```