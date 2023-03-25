---
title: Python UVa 160 - Factors and Factorials
date: 2023-03-23 11:26:14
tags:
  - UVa
categories:
  - [解題報告, UVa]
  - [CPE歷屆, CPE 2023/03/21]
excerpt: 這題是2023/3/21 CPE 的第五題，意外的挺簡單的，不過要小心他的輸出格式, UVa 160 - Factors and Factorials 解題報告
description: 這題是2023/3/21 CPE 的第五題，意外的挺簡單的，不過要小心他的輸出格式, UVa 160 - Factors and Factorials 解題報告
---
# UVa 160 - Factors and Factorials

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=96) - UVa 10035 - Primary Arithmetic 

這題是2023/3/21 CPE 的第五題，意外的挺簡單的，不過要小心他的輸出格式，當時被他的輸出搞很久>:(

## 題意
輸入一個`N`，依照題目的格式輸出`N!`的值

ex: 
`N = 5` 需輸出 `  5! =  3  1  1`
**因為：2 ^ 3 * 3 ^ 1 * 5 ^ 1 = 5! = 120**

**輸出格式**
* 先輸出`N! =`，其中`N`的長度要為**3個字元**，不夠就補空格
  假如 N = 5，那麼就要輸出`  5! =`

* 再來輸出一連串數字，n1 n2 n3 ... 每個長度需為**2個字元**，不夠就補空格

* **一行最多輸出15個數字，如果超過就換行，並且要對齊上方的數字，可以參考第二個範例測資**

{% note primary %}
 - ***注意輸出的格式***
 - 輸入0結束
{% endnote %}

#### Sample Input 
```text
5
53
0
```

#### Sample Output 
```text
  5! =  3  1  1
 53! = 49 23 12  8  4  4  3  2  2  1  1  1  1  1  1
        1
```

---
## 解題思路
**在考CPE時，我以為python算`100!`會超時，結果回家測試後發現速度其實挺快的，可以直接做...**

這題的解法很多，我這裡是提供先直接把N!算出來的解法，

先創一個質數表，型態為字典，紀錄1~100的所有質數和他們出現的次數，
之後先算出N的階層`temp`，再把質數表裡的元素一一取出，用`while`迴圈看可以整除幾次temp，直到`temp < 目前的prime`

*這裡我用一個`tail`來記錄質數最大會到多少，之後大於tail的質數就不輸出*

至於這題的輸出，可以參考我下方程式碼，我是用一個`head`變數來紀錄他開頭有多長 **('N! ='的這個部分)**


## 程式碼
```python
# UVa 160 - Factors and Factorials
# 2023/03/21/ CPE - 5
def isPrime(n):
    for i in range(2, int(n ** 0.5) + 1):
        if n % i == 0:
            return False
    return True

def getPrimeTable():
    primeTable = {i:0 for i in range(2, 100) if isPrime(i)}
    return primeTable

def solve(n):
    primeTable = getPrimeTable()

    temp = 1
    for i in range(1, n+1): temp *= i

    tail = 2
    for prime in primeTable.keys():
        if temp < prime: break
        while temp % prime == 0:
            tail = prime
            temp //= prime
            primeTable[prime] += 1

    # print answer
    count = 0
    head = f'{n:>3}! ='
    print(head, end='')
    for prime, times in primeTable.items():
        if prime > tail: break

        if count % 15 == 0 and count != 0: 
            print()
            print(len(head) * ' ', end='')
            
        print(f'{times:>3}', end='')
        count += 1
    print()

    return primeTable

while True:
    try:
        n = int(input())
        if n == 0: break

        solve(n)
    except EOFError:
        break
# Accepted	PYTH3	0.040
```