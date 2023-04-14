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
# Python UVa 10062 - Tell me the frequencies

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
```text
AAABBC
122333
```

#### Sample Output 
```text
67 1
66 2
65 3

49 1
50 2
51 3
```

---

## 解題思路
我們可以使用Python的`字典`來儲存字元出現的次數，要使用字典儲存次數很簡單，使用一行就可以解決：

```python
dic[i] = dic.get(i, 0) + 1
```
dic.get(i, 0)代表：
如果字典內有`i`，就將它加一，
如果沒有`i`，就先把`i`加入字典裡，value設為0，再加一

可以直接將這行程式碼格式背起來，不過還是建議先大概了解[Python字典中的get如何使用](https://www.w3schools.com/python/ref_dictionary_get.asp)

也可以用以下的方法代替：
```python
if i in dic:
    dic[i] += 1
else:
    dic[i] = 1
```

把所有字元出現次數算出來後我使用Python陣列生程式把字典的`key` `value`分別拿出來存成二維陣列([可參考Python 字典 items使用方法](https://www.runoob.com/python/att-dictionary-items.html))，再用`sorted`來依照題目的敘述來排序

`key`是字元的ASCII碼，所以要使用[ord](https://www.runoob.com/python/python-func-ord.html)函數來把字元轉成ASCII

`value`是出現的次數

我用lambda來幫忙做排序，我大概講解一下程式碼中的`key=lambda x: (int(x[1]), -x[0])`是什麼意思：

在二維陣列中依照每個陣列的第二個元素`x[1]`(**出現次數**)來排序，如果順序一樣，就依照第一個元素`x[0]`(**ASCII碼**)來排序

*加上負號代表 **由大到小排序***

最後再依照題目要求print答案就可以了

## Python程式碼
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