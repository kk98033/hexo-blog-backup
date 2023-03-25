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
輸入一個`N`，
{% note primary %}
 - 兩數長度不一定一樣
 - **注意！當只有一個進位時carry operation不需加上's'**
 - 當輸入'0 0'時結束
{% endnote %}

#### Sample Input 
`123 456`
`555 555`
`123 594`
`0 0`

#### Sample Output 
`No carry operation.`
`3 carry operations.`
`1 carry operation.`

---
## 解題思路
一開始我是先使用陣列來計算，但之後我發現這題直接用數字容易許多。
雖然題目是要兩數相加，但其實可以不用真的做加法計算，只需一個變數**carry**來計算目前位置加總的進位值，算式是**n1 + n2 + carry**，如果大於等於10，代表有進位，將**carry**設為**1**並且count + 1，反之將**carry**為**0**，重複此步驟直到兩數都等於0
*註：取出一個數字 'n' 最後一位: **n % 10*** ，將一個數字 'n' 右移一位: **n // 10***



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