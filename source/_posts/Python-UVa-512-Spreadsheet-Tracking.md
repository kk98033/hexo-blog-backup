---
title: Python UVa 512 - Spreadsheet Tracking
date: 2023-05-28 09:20:26
tags:
  - UVa
categories:
  - [解題心得, UVa]
  - [CPE歷屆, CPE 2023/05/23]
excerpt: 這題是CPE 2023/05/23 的第五題，屬於模擬題，照著題目說明做就可以了 - Python UVa 512 - Spreadsheet Tracking 解題心得
description: 這題是CPE 2023/05/23 的第五題，屬於模擬題，照著題目說明做就可以了 - Python UVa 512 - Spreadsheet Tracking 解題心得
---
# UVa 512 Python

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&category=0&problem=453&mosmsg=Submission+received+with+ID+28504732) - UVa 512 - Spreadsheet Tracking


## 題意
給予一串指令，依照指令內容修改表格，接下來給予多個位置座標，輸出最後他的位置。<br>

有以下五種指令：
| 指令 |             動作             |
|:----:|:----------------------------:|
|  IR  |    在目標**row前**插入一個row    |
|  IC  | 在目標**column前**插入一個column |
|  DR  |          刪除目標row         |
|  DC  |        刪除目標column        |
|  EX  |     交換(r1, c1) (r2, c2)    |
{% note primary %}
**每行的指令動作都是同一時間完成的！！**
例如有一個指令是刪除第 row 1, 2, 3 ，會同時把1, 2, 3給刪除
{% endnote %}

## 題目測資解釋
**我在這裡大概的把第一個測資的過程做一遍**
表格大小：**7 * 9**
指令：
{% codeblock line_number:false %}
DR 2 1 5
DC 4 3 6 7 9
IC 1 3
IR 2 2 4
EX 1 2 6 5
{% endcodeblock %}

需要輸出的位置：
{% codeblock line_number:false %}
(4, 8)
(5, 5)
(7, 8)
(6, 5)
{% endcodeblock %}

**在開始之前要先注意他的輸入格式**<br>

DR, DC, IC, IR 指令格式為：
{% codeblock line_number:false %}
DR n r1 r2 ... rn
{% endcodeblock %}
**n 為後面要刪除n個row**<br>

{% codeblock line_number:false %}
EX r1 c1 r2 c2
{% endcodeblock %}
**代表交換 (r1, c1) (r2, c2)**<br>

我把每個位置座標都加上去方邊觀看
|   | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 1, 1 | 1, 2 | 1, 3 | 1, 4 | 1, 5 | 1, 6 | 1, 7 | 1, 8 | 1, 9 |
| 2 | 2, 1 | 2, 2 | 2, 3 | 2, 4 | 2, 5 | 2, 6 | 2, 7 | 2, 8 | 2, 9 |
| 3 | 3, 1 | 3, 2 | 3, 3 | 3, 4 | 3, 5 | 3, 6 | 3, 7 | 3, 8 | 3, 9 |
| 4 | 4, 1 | 4, 2 | 4, 3 | 4, 4 | 4, 5 | 4, 6 | 4, 7 | 4, 8 | 4, 9 |
| 5 | 5, 1 | 5, 2 | 5, 3 | 5, 4 | 5, 5 | 5, 6 | 5, 7 | 5, 8 | 5, 9 |
| 6 | 6, 1 | 6, 2 | 6, 3 | 6, 4 | 6, 5 | 6, 6 | 6, 7 | 6, 8 | 6, 9 |
| 7 | 7, 1 | 7, 2 | 7, 3 | 7, 4 | 7, 5 | 7, 6 | 7, 7 | 7, 8 | 7, 9 |

第一個指令：
`DR 2 1 5`
刪除row 1, 5
注意，需要 **同時** 刪除 row 1, 5 !!!!
|   | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |
|---|---|---|---|---|---|---|---|---|---|
| 1 | 2, 1 | 2, 2 | 2, 3 | 2, 4 | 2, 5 | 2, 6 | 2, 7 | 2, 8 | 2, 9 |
| 2 | 3, 1 | 3, 2 | 3, 3 | 3, 4 | 3, 5 | 3, 6 | 3, 7 | 3, 8 | 3, 9 |
| 3 | 4, 1 | 4, 2 | 4, 3 | 4, 4 | 4, 5 | 4, 6 | 4, 7 | 4, 8 | 4, 9 |
| 4 | 6, 1 | 6, 2 | 6, 3 | 6, 4 | 6, 5 | 6, 6 | 6, 7 | 6, 8 | 6, 9 |
| 5 | 7, 1 | 7, 2 | 7, 3 | 7, 4 | 7, 5 | 7, 6 | 7, 7 | 7, 8 | 7, 9 |

第二個指令：
`DC 4 3 6 7 9`
刪除column 3, 6, 7, 9
|   | 1 | 2 | 3 | 4 | 5 |
|---|---|---|---|---|---|
| 1 | 2, 1 | 2, 2 | 2, 4 | 2, 5 | 2, 8 |
| 2 | 3, 1 | 3, 2 | 3, 4 | 3, 5 | 3, 8 |
| 3 | 4, 1 | 4, 2 | 4, 4 | 4, 5 | 4, 8 |
| 4 | 6, 1 | 6, 2 | 6, 4 | 6, 5 | 6, 8 |
| 5 | 7, 1 | 7, 2 | 7, 4 | 7, 5 | 7, 8 |

第三個指令：
`IC 1 3`
插入column 3
也就是在第 **三** 個column前插入一個column，我這裡用0, 0 表示插入的column
**注意：插入是插入在column前方!!**
|   | 1 | 2 | 3 | 4 | 5 | 6 |
|---|---|---|---|---|---|---|
| 1 | 2, 1 | 2, 2 | 0, 0 | 2, 4 | 2, 5 | 2, 8 |
| 2 | 3, 1 | 3, 2 | 0, 0 | 3, 4 | 3, 5 | 3, 8 |
| 3 | 4, 1 | 4, 2 | 0, 0 | 4, 4 | 4, 5 | 4, 8 |
| 4 | 6, 1 | 6, 2 | 0, 0 | 6, 4 | 6, 5 | 6, 8 |
| 5 | 7, 1 | 7, 2 | 0, 0 | 7, 4 | 7, 5 | 7, 8 |


第四個指令：
`IR 2 2 4`
插入row 2, 4
**注意：插入是插入在row前方!!**
|   | 1      | 2      | 3      | 4      | 5      | 6      |
|---|--------|--------|--------|--------|--------|--------|
| 1 | 2, 1   | 2, 2   | 0, 0   | 2, 4   | 2, 5   | 2, 8   |
| 2 | 0, 0   | 0, 0   | 0, 0   | 0, 0   | 0, 0   | 0, 0   |
| 3 | 3, 1   | 3, 2   | 0, 0   | 3, 4   | 3, 5   | 3, 8   |
| 4 | 4, 1   | 4, 2   | 0, 0   | 4, 4   | 4, 5   | 4, 8   |
| 5 | 0, 0   | 0, 0   | 0, 0   | 0, 0   | 0, 0   | 0, 0   |
| 6 | 6, 1   | 6, 2   | 0, 0   | 6, 4   | 6, 5   | 6, 8   |
| 7 | 7, 1   | 7, 2   | 0, 0   | 7, 4   | 7, 5   | 7, 8   |


第五個指令：
`EX 1 2 6 5`
交換(1, 2), (6, 5)
|   | 1      | 2      | 3      | 4      | 5      | 6      |
|---|--------|--------|--------|--------|--------|--------|
| 1 | 2, 1   | **6, 5**   | 0, 0   | 2, 4   | 2, 5   | 2, 8   |
| 2 | 0, 0   | 0, 0   | 0, 0   | 0, 0   | 0, 0   | 0, 0   |
| 3 | 3, 1   | 3, 2   | 0, 0   | 3, 4   | 3, 5   | 3, 8   |
| 4 | 4, 1   | 4, 2   | 0, 0   | 4, 4   | 4, 5   | 4, 8   |
| 5 | 0, 0   | 0, 0   | 0, 0   | 0, 0   | 0, 0   | 0, 0   |
| 6 | 6, 1   | 6, 2   | 0, 0   | 6, 4   | **2, 2**   | 6, 8   |
| 7 | 7, 1   | 7, 2   | 0, 0   | 7, 4   | 7, 5   | 7, 8   |


#### Sample Input 
{% codeblock line_number:false %}
7 9
5
DR 2 1 5
DC 4 3 6 7 9
IC 1 3
IR 2 2 4
EX 1 2 6 5
4
4 8
5 5
7 8
6 5
0 0
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
Spreadsheet #1
Cell data in (4,8) moved to (4,6)
Cell data in (5,5) GONE
Cell data in (7,8) moved to (7,6)
Cell data in (6,5) moved to (1,2)
{% endcodeblock %}

輸入第一行有兩個數 row, column，代表表格大小
第二行會輸入一個數 **n** ，接下來會輸入 **n行指令**
再來會輸入一個數 **t** ， 接下來會輸入 **t行位置座標**

**最後輸入一個 0 0 代表結束**

{% note warning %}
每個測資輸出前都要輸出 **Spreadsheet #**
每個測資輸出間都要間隔一個空白
最後輸入一個 0 0 代表結束
{% endnote %}

---

## 解題思路
這題主要是模擬題，直接照著題目模擬就可以了。<br>

由於最後要追蹤表格位置的變化，我的作法是先創一個表格，裡面填入他原本的座標位置，這樣做完所有的指令後只需要在修改後的表格裡**搜尋要追蹤的座標**即可<br>

新增、刪除row, column我寫了以下的函式來輔助：
**DR, DC, IR, IC** ，可參考下方的程式碼<br>

對於刪除，我用`pop(i-1)`直接把他刪掉
因為指令式 **1-index** 形式，所以要`(i-1)`<br>

**row, column做法不同，可以參考下方我寫的函式**
對於row直接pop掉就行了，對於column要把每個陣列的row取出來，把column位置pop掉
<br>

對於新增，我用`insert(i-1, (0, 0))`
因為要新增在前一個row或column，所以要`(i-1)`
(0, 0)是新增的元素，要新增一整個row可以用`[(0, 0)] * len(tempTable[0])`<br>

對於row直接insert新的一整個row就行了，對於column要把每個陣列的row取出來，把column位置insert一個值
**row, column做法不同，可以參考下方我寫的函式**<br>

**要注意的一點是：**
因為是 **同時** 新增、刪除，所以我們必須要 **從位置最後面的指令開始做!!** ，這樣才不會影響順序
所以每個指令都必須 **先由大到小排序** `sorted(rows)[::-1]`

{% note warning %}
表格index 是由 **1** 開始的
{% endnote %}

**最後有一點很重要的是關於input的形式!!**
這題測資比較大，一開始我在uva online judge上是TLE的，不過可以參考我在[Python UVa 10226 - Hardwood Species](https://blog.iddle.dev/public/2023/05/23/Python-UVa-10226-Hardwood-Species/)的作法，把所有的輸入都改成`sys.stdin.readline()`後，時間從 **>3.000** 減少到 **2.270** 秒，低空飛過 3 秒的限制 :D

## Python程式碼
```python
# UVa 512 - Spreadsheet Tracking
import sys
def solve(row, col, codeNum, operations, n, checks):
    def DR(rows):
        for i in sorted(rows)[::-1]:
            tempTable.pop(i-1)
        return
    
    def DC(c):
        for c in sorted(c)[::-1]:
            for row in tempTable:
                row.pop(c-1)
        return
    
    def IR(rows):
        for i in sorted(rows)[::-1]:
            tempTable.insert(i-1, [(0, 0)] * len(tempTable[0]))
        return
    
    def IC(c):
        for c in sorted(c)[::-1]:
            for row in tempTable:
                row.insert(c-1, (0, 0))
        return
    
    def EX(pos):
        
        r1, c1 = pos[0] - 1, pos[1] - 1
        r2, c2 = pos[2] - 1, pos[3] - 1
        tempTable[r1][c1], tempTable[r2][c2] = tempTable[r2][c2], tempTable[r1][c1]
        return
    
    # 創建表格，裡面元素為他的位置(row, column)
    tempTable = [[(i+1, j+1) for j in range(col)] for i in range(row)]

    for oper in operations:
        if oper[0] == 'DR':
            DR(oper[2:])
        elif oper[0] == 'DC':
            DC(oper[2:])
        elif oper[0] == 'IR':
            IR(oper[2:])
        elif oper[0] == 'IC':
            IC(oper[2:])
        elif oper[0] == 'EX':
            EX(oper[1:])

    # 一一的搜尋每個修改後的位置在哪裡
    for pos in checks:
        found = False
        for i in range(len(tempTable)):
            for j in range(len(tempTable[0])):
                # 找到，輸出修改後的表格位置
                if (pos[0], pos[1]) == tempTable[i][j]:
                    print(f'Cell data in ({ pos[0] },{ pos[1] }) moved to ({ i+1 },{ j+1 })')
                    found = True
                    break
            if found: break

        # 沒找到，代表被刪除了
        if not found:
            print(f'Cell data in ({ pos[0] },{ pos[1] }) GONE')

t = 1
while True:
    try:
        # row, col = list(map(int, input().split()))
        row, col = list(map(int, sys.stdin.readline().strip().split())) # input改成sys.stdin
        if (row, col) == (0, 0): break

        # codeNum = int(input())
        codeNum = int(sys.stdin.readline().strip()) # input改成sys.stdin
        operations = []
        for i in range(codeNum):
            # temp = input().split()
            temp = sys.stdin.readline().strip().split() # input改成sys.stdin
            operations.append([temp[0]] + list(map(int, temp[1:])))

        n = int(input())
        checks = []
        for i in range(n):
            checks.append(list(map(int, sys.stdin.readline().strip().split()))) # input改成sys.stdin
            # checks.append(list(map(int, input().split())))

        if t != 1: print()
        print(f'Spreadsheet #{t}')
        solve(row, col, codeNum, operations, n, checks)
        t += 1
    except EOFError:
        break
# 	Accepted	PYTH3	2.270
```