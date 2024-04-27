---
title: Python UVa 11321 - Sort! Sort!! and Sort!!!
date: 2024-04-27 16:35:11
tags:
  - UVa
  - Python
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 這題是比較服雜的排續題，最簡單的解法是寫一個自訂排序的 key，不過需要注意 Python 跟 C 語言 MOD 的處理方式不一樣，這題是要根據 C 語言的 MOD 來做！！！ - Python UVa 11321 - Sort! Sort!! and Sort!!! 解題心得
description: 這題是比較服雜的排續題，最簡單的解法是寫一個自訂排序的 key，不過需要注意 Python 跟 C 語言 MOD 的處理方式不一樣，這題是要根據 C 語言的 MOD 來做！！！ - Python UVa 11321 - Sort! Sort!! and Sort!!! 解題心得
---
# UVa 11321 - Sort! Sort!! and Sort!!! Python 解法

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=99999999&page=show_problem&category=0&problem=2296&mosmsg=Submission+received+with+ID+29418729) - UVa 11321 - Sort! Sort!! and Sort!!!


## 題意
給定 `N` 和 `M`，接下來會給予 `N` 個數字，我們需要根據一個特定的排序規則來排序這些數字，排序規則如下：
1. 數字按照它們除以 M 的 ***餘數*** 升序排列。
2. 如果兩個數的餘數相同，奇數要排在偶數前面。
3. 如果兩個數的餘數相同且都是奇數，則數值較大的排在前面。
4. 如果兩個數的餘數相同且都是偶數，則數值較小的排在前面。

很重要的一點！
**對於負數的餘數值，遵循 C 程序設計語言的規則！**
**對於負數的餘數值，遵循 C 程序設計語言的規則！**
**對於負數的餘數值，遵循 C 程序設計語言的規則！**

也就是說：
負數的模數永遠不會大於零。例如，-100 MOD 3 = -1, -100 MOD 4 = 0 等等。

{% note info %}
當輸入 0 0 時就結束!
輸出時也要一併輸出 N M!
最後的 0 0 也要輸出!
{% endnote %}

#### Sample Input 
{% codeblock line_number:false %}
15 3
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
0 0
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
15 3
15
9
3
6
12
13
7
1
4
10
11
5
2
8
14
0 0
{% endcodeblock %}

---

## 解題思路
這題的核心是自定義排序規則。我們可以利用 Python 的 `sorted()` 函數或 `sort()` 方法來實現，這兩者都允許通過 key 參數指定一個函數，來控制排序的行為。

在這個問題中，我們需要根據元素對某個 **數字 M 的 MOD** 進行排序，並且需要考慮元素的奇偶性。因為比較複雜，我是寫一個額外的 sort_key 函數，此函數會返回一個 tuple ，這個 tuple 是根據題目要求的多重條件來定義每個元素的排序優先級，具體實現方式如下（因為我一開始以為這題規則會很麻煩要寫很多判斷，所以才把它寫在外部的函式，但其實這裡也可以直接用匿名函式 lambda 寫進去 sort key 裡面）：

另外，因為題目要求要 ***遵循 C 程序設計語言的規則!!!***
所以這裡要自訂 MOD 函式：
我是參考[這人的解法](https://stackoverflow.com/a/46969300)
```python
def mod(a, b):  
    return abs(a) % abs(b) * (1,-1)[a<0]
```
他寫的很簡潔，其中這行我解釋一下：
`(1, -1)[a < 0]`：
這是一個條件表達式，使用了 Python 的 tuple 索引方式來決定最終結果的符號。這裡 (1, -1) 是一個包含兩個元素的 tuple ，分別是 1 和 -1。
表達式 [a < 0] 會根據 a 是否小於零來評估為 True 或 False。在 Python 中，True 相當於整數 1，False 相當於整數 0。
如果 a 是負的（a < 0 為 True），則選擇 tuple 中的 -1（-1 是 tuple 的第二個元素，索引為 1）；如果 a 是非負的，則選擇 1（ tuple 的第一個元素，索引為 0）。

直觀一點，其實可以寫成這樣：
```python
def mod(a, b):
    result = abs(a) % abs(b)  # 先計算絕對值的 mod
    if a < 0:
        result *= -1  # 如果 a 是負數，使結果為負
    return result
```

接下來介紹一下自訂的 sort key（用 lambda 也可以過）:
```python
def sort_key(x):
    return (mod(x, m), x % 2 == 0, -x if x % 2 != 0 else x)
# ans = sorted(nums, key=lambda x: (mod(x, m), x % 2 == 0, -x if x % 2 != 0 else x)) # 直接用 lambda 也可以
ans = sorted(nums, key=sort_key)
```

接下來我來簡單解釋這裡的判斷：
1. **數字按照它們除以 M 的餘數升序排列。**
這裡記得要用自己定義的 mod，因為 python 的 mod 規則跟 C 語言規則不一樣！
```python
mod(x, m)
```


2. **如果兩個數的餘數相同，奇數要排在偶數前面。**
```python
x % 2 == 0
```
這部分檢查 x 是否為偶數。如果是偶數，x % 2 == 0 為真（即 True，在 Python 中等於 1），這使得偶數在排序時排在奇數後面，因為 False（0）在 True（1）之前。


3. **如果兩個數的餘數相同且都是奇數，則數值較大的排在前面。**
4. **如果兩個數的餘數相同且都是偶數，則數值較小的排在前面。**
```python
-x if x % 2 != 0 else x
```
當 x 為奇數時（x % 2 != 0），我們使用 -x。這代表在比較時，較大的數會有更小的負值，因此會**排在前面。**

相反地，當 x 為偶數時（x % 2 == 0），我們直接使用 x。這保證了偶數在其自身比較時，較小的數排在前面。


## Python程式碼
```python
# UVa 11321 - Sort! Sort!! and Sort!!!
def solve(n, m, nums):
    '''
        1. 數字按照它們除以 M 的餘數升序排列。
        2. 如果兩個數的餘數相同，奇數要排在偶數前面。
        3. 如果兩個數的餘數相同且都是奇數，則數值較大的排在前面。
        4. 如果兩個數的餘數相同且都是偶數，則數值較小的排在前面。
    '''
    # https://stackoverflow.com/a/46969300
    # def mod(a, b):  
    #     return abs(a)%abs(b)*(1,-1)[a<0]
    def mod(a, b):
        result = abs(a) % abs(b)  # 先計算絕對值的模
        if a < 0:
            result *= -1  # 如果 a 是負數，使結果為正
        return result
    def sort_key(x):
        return (mod(x, m), x % 2 == 0, -x if x % 2 != 0 else x)

    ans = sorted(nums, key=sort_key)
    # ans = sorted(nums, key=lambda x: (mod(x, m), x % 2 == 0, -x if x % 2 != 0 else x)) # 用 lambda 也可以
    return '\n'.join(map(str, ans)) 

while True:
    n, m = list(map(int, input().split()))
    print(f'{n} {m}')
    if n == 0 and m == 0: break
    nums = []
    for i in range(n):
        nums.append(int(input()))

    print(solve(n, m, nums))
# Accepted	PYTH3	1.160
```