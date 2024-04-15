---
title: Python UVa 11349 - Symmetric Matrix
date: 2024-04-15 09:29:07
tags:
  - UVa
  - Python
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 這題題目是叫你判斷是不是對稱矩陣，看起來很複雜，但其實可以直接把矩陣攤開檢查他是不是迴文就可以了，小心矩陣內如果有負數就不是對稱矩陣 - Python UVa 11349 - Symmetric Matrix 解題心得
description: 這題題目是叫你判斷是不是對稱矩陣，看起來很複雜，但其實可以直接把矩陣攤開檢查他是不是迴文就可以了，小心矩陣內如果有負數就不是對稱矩陣 - Python UVa 11349 - Symmetric Matrix 解題心得
---
# UVa 11349 - Symmetric Matrix 解法

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=2324) - UVa 11349 - Symmetric Matrix


## 題意
給定一個矩陣，檢查該矩陣是否為 ***對稱矩陣***。
這題裡面 ***對稱矩陣*** 的定義如下：
這題要求的對稱性是指矩陣需要關於其中心對稱。這意味著矩陣中的每個元素都應該與其相對於矩陣中心的元素相等。

具體來說，對於一個矩陣來說：

* 矩陣的每一行都應該是回文的，即第一行應該與最後一行相同，第二行應該與倒數第二行相同，依此類推。
* 每一列也應該是回文的，即第一列應該與最後一列相同，第二列應該與倒數第二列相同，等等。
* 矩陣中的所有元素必須是非負的。

這種對稱性可以視為矩陣在水平和垂直方向上都應該是對稱的，就像一個完美對稱的圖形一樣。


#### Sample Input 
{% codeblock line_number:false %}
2
N = 3
5 1 3
2 0 2
3 1 5
N = 3
5 1 3
2 0 2
0 1 5
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
Test #1: Symmetric.
Test #2: Non-symmetric.
{% endcodeblock %}

---

## 解題思路
這題可以先 ***把矩陣攤開來成一為陣列***（可以在讀取的時候用 `extend` 來將它加到 list 裡面），然後檢查兩件事：
1. 檢查裡面是否有負數，如果有的話直接回傳 **'Non-symmetric.'** *（題目有說不能包括負數）*
2. 檢查他是否是迴文，如果是迴文就回傳 **'Symmetric.'**

Python 判斷迴文非常非常簡單，可以輕鬆秒殺，我們可以用 ***切片(slice)*** 的方式： 
```python
if matrix == matrix[::-1]
```
這代表判斷是否 matrix 等於相反過後的 matrix，也就是判斷是否是迴文。

{% note primary %}
**迴文(palindrome)** 是指由左念到右或由右念到左，字母排列順序都一樣的單字、片語、句子、甚至是數字
{% endnote %}

另外有一點要注意的是，他的輸入很鳥，有一行測資是：`N = 3`。
我們可以用以下方式來取得我們要的 `3`:
```python
n = int(input().split()[-1])
```
我們先將 input 的值做 split，得到陣列: ['N', '=', '3']
接下來取最後一位(用 list[-1] 可以得到 list 最後一位的值。因為 split 是**回傳陣列**，所以可以直接用 input().split()[-1] 來取得最後一位的值)，最後取 int 即可。

## Python程式碼
```python
# UVa 11349 - Symmetric Matrix
def solve(n, matrix):
    for i in matrix:
        if i < 0: return 'Non-symmetric.'
    return 'Symmetric.' if matrix == matrix[::-1] else 'Non-symmetric.'

T = int(input())
for t in range(T):
    n = int(input().split()[-1])
    matrix = []
    for i in range(n):
        matrix.extend(list(map(int, input().split())))

    print(f'Test #{t+1}: {solve(n, matrix)}')
# Accepted	PYTH3	0.280
```