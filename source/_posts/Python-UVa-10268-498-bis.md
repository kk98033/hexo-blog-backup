---
title: Python UVa 10268 - 498-bis
date: 2024-04-05 14:10:42
tags:
  - UVa
  - Python
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 這題又是數學題，基本上跟著他題目的描述做就可以了（但是 python 這樣做在 uva online judge 上面會超時，文章內會提到如何解決） - Python UVa 10268 - 498-bis 解題心得
description: 這題又是數學題，基本上跟著他題目的描述做就可以了（但是 python 這樣做在 uva online judge 上面會超時，文章內會提到如何解決） - Python UVa 10268 - 498-bis 解題心得
---
# UVa 10268 - 498-bis Python 解法

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&category=14&page=show_problem&problem=1209) - Python UVa 10268 - 498-bis


## 題意
輸入一個 `X`，以及一連串的數字代表 `a0, a1, ... an`，算出：
$$ \ a_0nx^{n-1} + a_1(n-1)x^{n-2} + \ldots + a_{n-2}2x + a_{n-1} \ $$

解釋一下第一個測資：
x = 7
a0 = 1, a1 = -1
此時 `n = 1`

照著題目的算式做答案會等於： *1 * 1 * 7^(1 - 1) + (-1) * (1 - 1) * 7^(1 - 2) = 1 + 0 = 1*

#### Sample Input 
{% codeblock line_number:false %}
7
1 -1
2
1 1 1
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
1
5
{% endcodeblock %}

---

## 解題思路
直接照著題目意思做就好了，用 `enumerate` 依序取得 `index` 以及 `a0, a1 ... an-1`，注意，只需要取到 **an-1**，因為 **an 算出來一定會為 0**，另外 n 是多項式最大的幕，直接用 `len(nums) - 1` 就可以得到 n 的值。

很可惜的是，python 在 UVa online judge 會超時 :( ，不過在 Zerojudge 可以通過，此時有兩個選擇：
1. 換 C++ 寫
2. 用演算法解他 :[ (參考下方)

## Python程式碼
```python
def solve(x, nums):
    ans = 0
    n = len(nums) - 1
    for i, ai in enumerate(nums[0:-1]): # 最後一項不用做，因為一定為 0
        ans += ai * (n - i) * x ** (n - i - 1)
    return ans

while True:
    try:
        x = int(input())
        nums = list(map(int, input().split()))

        print(solve(x, nums))
    except EOFError:
        break
# TLE in UVa Online Judge
```

## 用演算法解
因為程式碼採用了直接計算的方式，對於每一項都計算了 x 的冪，這在 x 或者多項式的次數很大時會非常耗時!!!

為了解決 TLE 問題，我們可以嘗試優化計算過程。一種方法是使用 [霍納法則（Horner's Rule）(也稱為秦九韶算法)](https://wikimedia.org/api/rest_v1/media/math/render/svg/fae76b2d1f6c192ce13b893ef49233cfc03b7dd6)  來計算多項式及其導數的值。

先來看一下程式碼：
```python
def solve(x, nums):
    ans = 0
    n = len(nums) - 1
    for i in range(n, 0, -1):  # 從 n-1 開始遞減到 1
        ans = ans * x + i * nums[n - i]
    return ans
```
這樣，我們就將每一項的計算從 O(n) 降低到了 O(1)，整體算法從 O(n^2) 降低到了 O(n)，應該能有效避免 TLE

這行
```python
ans = ans * x + i * nums[n - i]
```
是根據霍納法則 (Horner's Rule) 來計算多項式導數的一種快速方式。

如果我們直接用傳統方式計算這個導數在某點 x 的值，我們需要對每一項分別計算 x 的冪然後乘以相應的系數和次數，這在計算量上是非常大的。
霍納法則讓我們可以更有效地計算這個導數值。核心思想是從最高次項開始，每一步計算都利用上一步的結果。每次回圈我們都執行如下操作：

1. 將目前的答案乘以 x **（這相當於將所有項的次數降低一級）**。
2. 加上當前項的系數乘以它的次數，即 `i * nums[n−i]`。其中，`nums[n−i]` 是當前項的系數，`i` 是這個系數對應的原始多項式中的次數 **（因為我們是從高次向低次計算，所以用 n−i）**。

有興趣理解他的原理可以參考 [維基百科](https://zh.wikipedia.org/zh-tw/%E7%A7%A6%E4%B9%9D%E9%9F%B6%E7%AE%97%E6%B3%95) 上面的解釋：

{% img [class names] https://blog.iddle.dev/public/2024/04/05/Python-UVa-10268-498-bis/wiki.png 512  '"wiki" "wiki"' %}
