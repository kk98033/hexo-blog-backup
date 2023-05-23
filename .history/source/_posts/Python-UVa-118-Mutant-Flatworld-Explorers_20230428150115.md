---
title: Python UVa 118 - Mutant Flatworld Explorers
date: 2023-03-19 13:55:35
tags:
  - UVa
  - UVa 一星
categories:
  - 解題心得
  - UVa
excerpt: Python UVa 118 - Mutant Flatworld Explorers 解題心得
description: Python UVa 118 - Mutant Flatworld Explorers 解題心得
urlname: Python UVa 118 - Mutant Flatworld Explorers
---
# Python UVa 10190 - Divide, But Not Quite Conquer

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=0&problem=54&mosmsg=Submission+received+with+ID+28318420) - UVa 118 - Mutant Flatworld Explorers



## 題意
給予一個場地的 `右上角座標(w, h)`，`機器人座標(x, y), 面向方向(W, N, E, S)`，`指令(L, R, F)`，機器人會依照指令移動。
如果機器人移動後超出了邊界，機器人就會掉下去，此時輸出 ***機器人移動前*** 的座標，**面向方向**以及 **'LOST'**
輸出機器人**最後的座標**，**面向方向**

***重點：***
在機器人掉落後會把掉落前的座標給記下來，如果下一個機器人執行指令後掉落的位置是此座標，就會直接忽略這個指令，繼續下一個指令。
比如說，上一個機器人掉落前在(3, 3)，那麼如果之後的機器人在(3, 3)執行指令後會掉落邊界，他會忽略這個指令，留在(3, 3)，然後再執行下一個指令。***(可以參考Sample Input 的第三個測資)***
* 左轉（Left）：機器人在原地往左轉 90 度。
* 右轉（Right）: 機器人在原地往右轉 90 度。
* 前進（Forward）: 機器人往其面向的方向向前走一格，且不改變其面向之方向。

{% note primary %}
 - 地圖大小是從左下角(0, 0)開始算，到右上角(w, h)`包含(w和h)!!`
 - 不用開陣列算，直接判斷位置就好了
{% endnote %}

#### Sample Input 
`5 3`
`1 1 E`
`RFRFRFRF`
`3 2 N`
`FRRFLLFFRRFLL`
`0 3 W`
`LLFFFLFLFL`

#### Sample Output 
`1 1 E`
`3 3 N LOST`
`2 3 S`

---
## 解題思路
直接讓機器人照著指令跑一遍即可。不需要開陣列，直接判位置看有沒有掉下去。這題好麻煩:(

*關於轉向和移動，可以參考我的方法，或者是寫一堆if來做都可以*
**轉向**：我的做法是設一個陣列，記錄位置的關係`dirs = ['N', 'E', 'S', 'W']`(順序要照順時針方向)右轉是`index + 1`左轉是`index - 1`記得判斷頭尾的狀況

**移動**：我是設一個move的字典，輸出位置，可以得到移動的位置，可參考下方move註解

每次機器人移動一步，我都會用`previous`來記錄移動前的座標，方便機器人掉下去後紀錄



## Python程式碼
```python
# UVa 118 - Mutant Flatworld Explorers
'''
   N
w -|- E
   S
'''

move = {
        'N': (0, 1),  # go up - N
        'S': (0, -1), # go down - S
        'E': (1, 0),  # go right - E
        'W': (-1, 0)  # go left - W
    }

falls = []

def turn(faces: str, direction: str) -> str:
    dirs = ['N', 'E', 'S', 'W']
    cur = dirs.index(faces)
    direction = 1 if direction == 'R' else -1
    
    if cur + direction < 0: return dirs[-1]
    if cur + direction > 3: return dirs[0]
    return dirs[cur + direction]

def solve(width, height, pos, faces, codes):
    # pos -> [x, y]
    for code in codes:
        if code == 'F':
            previous = [pos[0], pos[1]]
            pos[0] += move[faces][0]
            pos[1] += move[faces][1]

            if (pos[0] < 0 or pos[0] > width) or (pos[1] < 0 or pos[1] > height): # 判斷機器人是否超出邊界
                if previous in falls: # 機器人移動後超出了邊界，但是這個位置已經被紀錄過了，所以忽略此指令
                    pos[0], pos[1] = previous[0], previous[1] # 記得把機器人位置移到移動前的位置
                    continue

                falls.append(previous) # 紀錄掉下去之前的位置
                return f'{previous[0]} {previous[1]} {faces} LOST'
        else:
            faces = turn(faces, code)

    return f'{pos[0]} {pos[1]} {faces}'

width, height = list(map(int, input().split()))
while True:
    try:
        x, y, faces = input().split()
        codes = input()

        print(solve(width, height, [int(x), int(y)], faces, codes))
    except EOFError:
        break
# Accepted	PYTH3	0.010
```