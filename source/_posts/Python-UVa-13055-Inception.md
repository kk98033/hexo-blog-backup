---
title: Python UVa 13055 - Inception
date: 2023-03-23 13:07:58
tags:
  - UVa
  - UVa 一星
categories:
  - [解題心得, UVa]
  - [CPE歷屆, CPE 2023/03/21]
excerpt: 這題是2023/3/21 CPE 的第三題，屬於模擬題，直接照題目意思做就可以了, UVa 13055 - Inception 解題心得
description: 這題是2023/3/21 CPE 的第三題，屬於模擬題，直接照題目意思做就可以了, UVa 13055 - Inception 解題心得
# urlname: Python UVa 13055 - Inception
---
# UVa 13055 - Inception

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=4953) - UVa 13055 - Inception

這題是2023/3/21 CPE 的第三題，屬於模擬題，直接照題目意思做就可以了

## 題意
輸入`N`個指令，Dom會照著以下規則操作

* ‘Sleep X’ - Dom進入 X 的夢
* ‘Kick’    - 把Dom所在的夢的人叫醒
* ‘Test’    - 輸出Dom在誰的夢裡，如果Dom沒有在任何人的夢裡就輸出‘Not in a dream’

#### Sample Input 
```text
20
Sleep Dom
Sleep Sakin
Test
Sleep Asif
Sleep Mushfiq
Test
Kick
Test
Sleep Shafi
Test
Kick
Test
Kick
Test
Kick
Test
Kick
Test
Kick
Test
```

#### Sample Output 
```text
Sakin
Mushfiq
Asif
Shafi
Asif
Sakin
Dom
Not in a dream
Not in a dream
```

---
## 解題思路
這題用[Stack](https://www.geeksforgeeks.org/stack-in-python/)做比較容易
`stack.append()`：新增元素
`stack.pop()`：拿出元素

當遇到**Sleep**時，就把X加入stack，遇到**Kick**，就pop掉，遇到**Test**就print出stack最上面的元素(stack[-1])
***在pop時要注意stack是否有元素，如果沒有就print('Not in a dream')***

## Python程式碼
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