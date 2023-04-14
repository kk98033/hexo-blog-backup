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

這題跟[UVa 10041 - Vito's Family](https://blog.iddle.dev/public/2023/03/18/Python-UVa-10041-Vito-s-Family/)很類似，也都是要找 ***中位數*** ，只不過這題需要把所有符合的中位數給找出來。

## 題意
輸入多組數字，`X1, X2, ... Xn`，找到一個`A`使得：
|X1 − A| + |X2 − A| + . . . + |Xn − A|
為最小值

**輸出三個整數**
{% note info %}
 - 第一個數字是能得到該算式最小值的A。(中位數取最小)
 - 第二個數字是|Xi − A|為最小值的數量。(也就是中位數數量)
 - 第三行數字是可能有幾種最小值。(當有偶數個的時候才需要算，中位數1 - 中位數2)
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