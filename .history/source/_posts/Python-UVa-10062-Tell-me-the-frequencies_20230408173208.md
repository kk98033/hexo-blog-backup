---
title: Python UVa 10062 - Tell me the frequencies
date: 2023-04-08 17:24:12
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
# Python UVa 10057 - A mid-summer night's dream.

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1003) - UVa 10062 - Tell me the frequencies

這題挺簡單的，只需算出各字元ASCII出現的次數即可，對於Python來說可以輕鬆秒殺，不過要記得注意他的輸出格式。

## 題意
給你一列文字，算出各字元ASCII出現的次數。

輸出要以各字元出現的次數由小到大輸出，**如果有2個以上的字元有相同的次數，則ASCII值較大的先輸出！！**
每筆測資輸出中間要空一格！

{% note info %}
 - 要注意輸出的規則！
{% endnote %}


#### Sample Input 
`2`
`10`
`10`
`4`
`1`
`2`
`2`
`4`

#### Sample Output 
`10 2 1`
`2 2 1`

---

## 解題思路
這題很討厭，我覺得題目沒有把輸出說的很清楚>:(
這題是找中位數，一樣先把他排序，再找最中間的數
**但這題需要把偶數個數中間的兩個中位數給找出來分開討論**

**假設兩個中位數是mid1, mid2（奇數個只會有一個mid1）**
**對於輸出的第一個數 *‘第一個數字是能得到該算式最小值的A’***
=> 找最小的那個中位數就可以了(**就是`mid1`**)

**對於輸出的第二個數 *‘|Xi − A|為最小值的數量’***
=> 也就是找最小中位數的數量，對於**奇數個**直接`count(mid1)`就可以了
**但偶數個需要找他兩個中位數的數量!**，而且要記得考慮兩個中位數相等的情況，寫個if來判斷是要`count(mid1)`還是`count(mid1) + count(mid2)`

**對於輸出的第三個數 *‘可能有幾種最小值’***
=> 對於奇數個直接輸出`1`，
偶數個要計算`mid1 - mid2 + 1`(就是算兩個中位數之間有幾個數字)


## 程式碼
```python
# UVa 10062 - Tell me the frequencies!
def solve(s):
    dic = {}
    for i in s:
        dic[i] = dic.get(i, 0) + 1
    ans = sorted([[ord(key), val] for key, val in dic.items()], key=lambda x: (int(x[1]), -x[0]))
    return '\n'.join([f'{key} {val}'for key, val in ans])

count = 0
while True:
    try:
        s = input()
        if count != 0: print()
        if s:
            print(solve(s))
        count += 1
    except EOFError:
        break
# Accepted	PYTH3	0.040
```