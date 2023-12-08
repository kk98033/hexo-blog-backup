---
title: Python UVa 10101 - Bangla Numbers
date: 2023-12-08 21:46:54
  - UVa
  - Python
categories:
  - [解題心得, UVa]
excerpt: 這題是是一道關於數字格式化的程式設計題目。這題的主要挑戰在於將一個給定的數字轉換成孟加拉的數字表示方式，這題是一個很好的練習，用於加強對數字格式化和遞迴方法的理解。 - Python UVa 10101 - Bangla Numbers 解題心得
description: 這題是是一道關於數字格式化的程式設計題目。這題的主要挑戰在於將一個給定的數字轉換成孟加拉的數字表示方式，這題是一個很好的練習，用於加強對數字格式化和遞迴方法的理解。 - Python UVa 10101 - Bangla Numbers 解題心得
---
# Python UVa 10101 - Bangla Numbers

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&category=13&page=show_problem&problem=1042) - UVa 10101 - Bangla Numbers


## 題意
UVa 10101 要求將一系列數字轉換成孟加拉格式。孟加拉數字系統包括四個主要單位：
| Bangla Unit | Value    |
|-------------|----------|
| Kuti        | 10000000 |
| Lakh        | 100000   |
| Hajar       | 1000     |
| Shata       | 100      |

例如：
999999999 => 99 kuti 99 lakh 99 hajar 9 shata 99
1000000000 => 1 shata kuti

{% note info %}
* 0 轉換成孟加拉數字還是為 0
* 每個數字由一個空格隔開
* 每次輸出都要輸出目前是第幾次， *他的格式是四個字元，如果長度不到四個字元，就填空格，向右對齊*
{% endnote %}

#### Sample Input 
{% codeblock line_number:false %}
23764
45897458973958
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
   1. 23 hajar 7 shata 64
   2. 45 lakh 89 hajar 7 shata 45 kuti 89 lakh 73 hajar 9 shata 58
{% endcodeblock %}

---

## 解題思路
這題主要的思路是透過`遞迴`方法來處理不同的數字單位（Kuti, Lakh, Hajar, Shata），從 **最大** 單位開始將數字分割並加上相應的單位名稱，**同時特別注意Kuti單位可能在數字中多次出現的情況**，並且對於數字為0的情況進行特殊處理，以確保格式正確且符合孟加拉數字的表示方式。

遞迴的邏輯主要是要先從最**大**的數開始計算，然後再逐步地往小的數字算，當數字小於 100 時且不等於 0 ，就直接加上去。
可以參考下方程式碼的遞迴：
```python
def solve(n):
    # 'kuti' (10000000), 'lakh' (100000), 'hajar' (1000), 'shata' (100) 
    result = ""
    if n >= 10000000:
        result += f'{solve(n // 10000000)} kuti '
        n = n % 10000000
    if n >= 100000:
        result += f'{solve(n // 100000)} lakh '
        n = n % 100000
    if n >= 1000:
        result += f'{solve(n // 1000)} hajar '
        n = n % 1000
    if n >= 100:
        result += f'{solve(n // 100)} shata '
        n = n % 100
    if n != 0:
        result += str(n)

    return result
```

關於他的輸出格式，我是這樣寫的：
```python
f'{count:>4}. {ans}'
```
我使用python的 `f-string` 來達成格式化輸出：
**{count:>4} 的含義：**
* {}：這是格式化字符串的標記，用於指定變量（在這裡是 count）的位置。
* count：這是要格式化顯示的變數名稱。
* :：這個符號之後的部分是格式化規範。
* >：這表示對齊方式。在這裡，> 表示**右**對齊。
* 4：這表示寬度。在這裡，它**表示 count 應該在一個至少4個字符寬的空間內顯示。**

假設 count = 1，那麼 {count:>4} 將會把 1 顯示在一個4個字符寬的空間內，並且右對齊。所以結果看起來會像這樣：
{% codeblock line_number:false %}
···1 (我以 “·” 符號代表空格)
{% endcodeblock %}

關於程式碼有一部分我認為我沒有寫得很好，那就是最後得到的字串有些情況下中間會有兩個空格，我是偷懶直接在最後的時候把它重新用空格做 split ，然後再把空格加入到 split 後的陣列，避免中間出現兩個空格。
```python
ans = ' '.join(solve(n).split()) # 避免中間出現兩個空格
```

## Python程式碼
```python
# UVa 10101 - Bangla Numbers
def solve(n):
    # 'kuti' (10000000), 'lakh' (100000), 'hajar' (1000), 'shata' (100) 
    result = ""
    if n >= 10000000:
        result += f'{solve(n // 10000000)} kuti '
        n = n % 10000000
    if n >= 100000:
        result += f'{solve(n // 100000)} lakh '
        n = n % 100000
    if n >= 1000:
        result += f'{solve(n // 1000)} hajar '
        n = n % 1000
    if n >= 100:
        result += f'{solve(n // 100)} shata '
        n = n % 100
    if n != 0:
        result += str(n)

    return result

count = 1
while True:
    try:
        n = int(input())
        if n == 0: 
            if n == 0: print(f'{count:>4}. 0')
        else:
            ans = ' '.join(solve(n).split()) # 避免中間出現兩個空格
            print(f'{count:>4}. {ans}')
        count += 1
    except EOFError:
        break
# Accepted	PYTH3	0.440
```