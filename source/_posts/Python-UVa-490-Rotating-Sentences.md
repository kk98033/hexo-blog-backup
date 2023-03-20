---
title: Python UVa 490 - Rotating Sentences
date: 2023-03-20 06:24:44
tags:
  - UVa
  - UVa 一星
categories:
  - 解題報告
  - UVa
excerpt: Python UVa 490 - Rotating Sentences 解題報告
description: Python UVa 490 - Rotating Sentences 解題報告
---
# Python UVa 299 - Train Swapping

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&category=94&page=show_problem&problem=431) - UVa 490 - Rotating Sentences



## 題意
給予多串的文字，最後把他們 ***向右旋轉90度***

{% note primary %}
 - 需一直讀入字串，直到EOF再把他們向右翻轉
 - *每行字串長度不一定一樣，所以長度不夠時需要用一個空格來代替*
{% endnote %}

#### Sample Input 
```text
Rene Decartes once said,
"I think, therefore I am."
```

#### Sample Output 
```text
"R
Ie
 n
te
h 
iD
ne
kc
,a
 r
tt
he
es
r 
eo
fn
oc
re
e 
 s
Ia
 i
ad
m,
. 
"
```

---
## 解題思路
把輸入的的字串存入陣列，最後再直接翻轉陣列就好。
翻轉陣列只需要把他for迴圈反過來輸出即可：
```python
for i in range(col):
    for j in range(row-1, -1, -1): # 因為題目是向右轉90度，所以要從最後一個row開始到第一個
        print(text[j][i])
```

至於`col`和`row`可以用`len(text)`和`max([len(s) for s in text]) # 找到最長的row`來得到
***記得處理row長度不夠的狀況***

## 程式碼
```python
# UVa 490 - Rotating Sentences
def solve(text):
    row, col = len(text), max([len(s) for s in text])
    ans = ''
    for i in range(col):
        temp = ''
        for j in range(row-1, -1, -1):
            if i >= len(text[j]): # 當此row長度不足時補一個' '給他
                temp += ' '
            else:
                temp += text[j][i]
        ans += temp + '\n'
    return ans[:-1] # 因為ans最後會多一個'\n'，所以要把他給去掉，如果是直接print(text[j][i])的話就可以不用做

text = []
while True:
    try:
        text.append(list(input()))

    except EOFError:
        print(solve(text))
        break
# Accepted	PYTH3	0.010
```