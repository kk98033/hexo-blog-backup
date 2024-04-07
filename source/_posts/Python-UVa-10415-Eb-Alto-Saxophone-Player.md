---
title: Python UVa 10415 - Eb Alto Saxophone Player
date: 2024-04-06 12:47:39
tags:
  - UVa
categories:
  - [解題心得, UVa]
excerpt: 這題是模擬題，只要讀懂題目意思就可以輕鬆解決了！ - Python UVa 10409 - Die Game 解題心得
description: 這題是模擬題，只要讀懂題目意思就可以輕鬆解決了！ - Python UVa 10409 - Die Game 解題心得
---
# UVa 10415 - Eb Alto Saxophone Player 解法

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1356) - UVa 10415 - Eb Alto Saxophone Player

## 題意
題目會給予一連串的音符，我們要計算出演奏這些音符時各個手指需要壓下的次數。每個音符對應一種特定的手指組合。

#### Sample Input 
{% codeblock line_number:false %}
3
cdefgab
BAGFEDC
CbCaDCbCbCCbCbabCCbCbabae
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
0 1 1 1 0 0 1 1 1 1
1 1 1 1 0 0 1 1 1 0
1 8 10 2 0 0 2 2 1 0
{% endcodeblock %}

---

## 解題思路
首先先創建每個音符對應的手指組合，我這裡是用 python 的`字典` 的方式來實現的，字典的 key 是 **音符的值** ，value 是一串 1 和 0 組成的字串，其中 **1 代表按下**， **0 代表沒有按下**。最累的部分已經完成了！

接下來，我們需要一個變數來記錄當前的手指狀態，我使用了 current 這個變數，初始化為 **'0000000000'**，即所有手指**都未按下**的狀態。

另外，我們還需要一個 list 叫 ans 來存儲最終的結果，即每個手指按下的次數。初始化 ans 為一個長度為 10，所有元素都是 0 的 list（ans = [0] * 10 我這樣寫是因為我懶得創一個裡面有10個0的 list）。

最後，我們遍歷每個音符，使用字典查找對應的手指組合。
要注意的是，並不是計算有多少個 1而已，是要計算**之前未按下，且當前按下的情況**！！！！
也就是說，**對於每個手指位置，如果在當前狀態下這個手指是未按下的（current[i] == '0'），但在新的手指組合中需要被按下（hold[i] == '1'），那麼我們就將對應的 ans 中的元素加1，表示這個手指被按下了一次。**
接下來更新 current 為新的手指組合。

最後的最後，將 ans list 轉為字串的形式回傳，可以參考我的方法（因為 ans 裡面的元素是 **數字**，所以用 `join` 函式之前要先把 ans 裡面的所有元素轉為 **字串**才能用，我這裡是用 `map` 的方式）：
```python
' '.join(map(str, ans))
```

## Python程式碼
```python
# UVa 10415 - Eb Alto Saxophone Player
def solve(codes):
    dict = {
        'c': '0111001111',
        'd': '0111001110',
        'e': '0111001100',
        'f': '0111001000',
        'g': '0111000000',
        'a': '0110000000',
        'b': '0100000000',
        'C': '0010000000',
        'D': '1111001110',
        'E': '1111001100',
        'F': '1111001000',
        'G': '1111000000',
        'A': '1110000000',
        'B': '1100000000',
    }
    current = '0000000000'
    ans = [0] * 10
    for code in codes:
        hold = dict[code]
        for i in range(10):
            if current[i] == '0' and hold[i] == '1':
                ans[i] += 1
        current = hold

    return ' '.join(map(str, ans))

T = int(input())
for t in range(T):
    codes = input()
    print(solve(codes))
# Accepted	PYTH3	0.340
```