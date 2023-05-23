---
title: Python UVa 100 - The 3n + 1 problem
date: 2023-03-19 12:32:19
tags:
  - UVa
  - UVa 一星
categories:
  - 解題心得
  - UVa
excerpt: Python UVa 100 - The 3n + 1 problem 解題心得
description: Python UVa 100 - The 3n + 1 problem 解題心得
urlname: Python UVa 100 - The 3n + 1 problem
---
# Python UVa 10190 - Divide, But Not Quite Conquer

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&category=0&problem=36&mosmsg=Submission+received+with+ID+28318303) - UVa 100 - The 3n + 1 problem



## 題意
給予兩數`i`, `j`，使用以下的演算法，算出從`i ~ j` **(包含`i`和`j`)** 每數最多會進入迴圈幾次
```
1. input n
2. print n
3. if n = 1 then STOP
4. if n is odd then n ←− 3n + 1
5. else n ←− n/2
6. GOTO 2
```

{% note primary %}
 - 注意！小心Python會超時！！
 - 注意i, j的大小，並不是每一組的i, j都是j > i
 - 輸出格式是: `i` `j` `最大迴圈數`
{% endnote %}

#### Sample Input 
`1 10`
`100 200`
`201 210`
`900 1000`

#### Sample Output 
`1 10 20`
`100 200 125`
`201 210 89`
`900 1000 174`

---
## 解題思路
這題我猜用C++可以直接套題目給的演算法再加個迴圈跑i ~ j找最大的就可以過了，***但是這題Python直接做一定會超時D:!!***
我們可以把先前算過的數字都先記錄下來，之後如果算到那數字，就直接加上那數字的值即可，我是用字典來做儲存，這樣不管在儲存還是搜尋速度都比較快。
*註：要注意i, j的大小判斷*



## Python程式碼
```python
# UVa 100 - The 3n + 1 problem
appeared = {} # 記錄計算過的數字

def solve(i, j):
    count = ans = 0
    for cur in range(min(i, j), max(i, j)+1): # 注意i, j大小
        count = 0
        n = cur
        while True:
            if n in appeared: # 如果n之前有計算過，就直接取之前紀錄的值加上去，然後跳出迴圈
                count += appeared[n]
                break

            count += 1
            if n == 1: break
            if n % 2 != 0: n = 3 * n + 1
            else: n = n // 2
        
        appeared[cur] = count # 記錄目前數字會進入迴圈幾次
        ans = max(count, ans)

    return f'{i} {j} {ans}'

while True:
    try:
        i, j = list(map(int, input().split()))
        print(solve(i, j))
    except EOFError:
        break
# Accepted	PYTH3	0.590
```