---
title: Python UVa 10008 - What's Cryptanalysis?
date: 2023-03-20 13:57:41
tags:
  - UVa
  - UVa 一星
categories:
  - 解題心得
  - UVa
excerpt: Python UVa 10008 - What's Cryptanalysis? 解題心得
description: Python UVa 10008 - What's Cryptanalysis? 解題心得
---
# Python UVa 10008 - What's Cryptanalysis?

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&category=12&page=show_problem&problem=949) - UVa 10008 - What's Cryptanalysis?



## 題意
給予一連串文字，算出裡面的`字母`總共出現幾次，所有輸入的字母均視為`大寫`!
輸出依照出現次數來排序，如果出現次數一樣，依照字母的順序排序

{% note primary %}
 - 所有輸入的字母均視為`大寫`
 - 忽略所有字母以外的字元
 - 注意input，是全部輸入完才會一起輸出
{% endnote %}

#### Sample Input 
`3`
`This is a test.`
`Count me 1 2 3 4 5.`
`Wow!!!! Is this question easy?`

#### Sample Output 
`S 7`
`T 6`
`I 5`
`E 4`
`O 3`
`A 2`
`H 2`
`N 2`
`U 2`
`W 2`
`C 1`
`M 1`
`Q 1`
`Y 1`

---
## 解題思路
這題分為兩個重點， ***算次數*** 和 ***排序***。
先講算次數：
算次數可以用python中的 ***dictionary*** 很容易的算出來，可參考下方程式碼
`ans[i] = ans.get(i, 0) + 1`代表的是: 字母`i`的value = (字典中字母i的數量 ***如果字典沒有`i`，就增加一個key: `i`, value: `0`進入字典*** ) + 1
關於python`dict.get()`用法可以參考[此文章](https://www.w3schools.com/python/ref_dictionary_get.asp)

算完字母出現幾次後，我把字典的key, value拿出來當作一個字串ex: 'A 15'(中間留一個空格，是為了符合輸出格式)，存在一個陣列 **`ansList`**，之後再依照題目要求來做排序就是答案了

再來是排序的部分：
`ansList.sort(key=lambda x: (-int(x[2:]), x[0]))` 的意思是：以陣列中的x[2:]元素由大排到小(也就是字母出現的次數)(注：加上負號是由大到小排序)，後面的那項x[0]代表如果x[2:]相同，就以x[0]來排序
詳情可以參考[python多元素的排序](https://stackoverflow.com/questions/4233476/sort-a-list-by-multiple-attributes), [python匿名函數Lambda](https://www.w3schools.com/python/python_lambda.asp)

{% note info %}
**最後有興趣的也可以參考一下我下方的一行解:D，主要就是把上方的過程和在一起，需要用到Counter函式**
{% endnote %}

## Python程式碼
```python
# UVa 10008 - What's Cryptanalysis?
from collections import Counter
def solve(text):
    ans = {}
    for i in text:
        if 'A' <= i <= 'Z':
            ans[i] = ans.get(i, 0) + 1
    ansList = [f'{key} {val}' for key, val in ans.items()]
    ansList.sort(key=lambda x: (-int(x[2:]), x[0]))
    return '\n'.join(ansList)

def solveOneLine(text):
    return '\n'.join(sorted([f'{key} {val}' for key, val in Counter([i for i in text if 'A' <= i <= 'Z']).items()], key=lambda x: (-int(x[2:]), x[0])))


T = int(input())
text = ''
for t in range(T):
    text += input()
# print(solve(text.upper()))
print(solveOneLine(text.upper()))
# Accepted	PYTH3	0.000
```