---
title: Python UVa 10035 - Primary Arithmetic
date: 2023-03-17 8:18:21
tags:
 - UVa

categories:
 - 解題報告
 - UVa

excerpt: Python UVa 10035 - Primary Arithmetic
---
# Python UVa 10035 - Primary Arithmetic

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&category=0&problem=976&mosmsg=Submission%20received%20with%20ID%2028310725) - UVa 10035 - Primary Arithmetic 



## 題意
輸入兩個整數，將兩數相加，計算總共會進位幾次
{% note primary %}
 - 兩數長度不一定一樣
 - **注意！當只有一個進位時carry operation不需加上's'**
 - 當輸入'0 0'時結束
{% endnote %}

#### Sample Input 
{% note primary %}
123 456
555 555
123 594
0 0
{% endnote %}

#### Sample Output 
{% note primary %}
No carry operation.
3 carry operations. 
1 carry operation.
{% endnote %}

---
## 解題思路
一開始我是先使用陣列來計算，但之後我發現這題直接用數字容易許多。
雖然題目是要兩數相加，但其實可以不用真的做加法計算，只需一個變數**carry**來計算目前位置加總的進位值，算式是**n1 + n2 + carry**，如果大於等於10，代表有進位，將**carry**設為**1**並且count + 1，反之將**carry**為**0**，重複此步驟直到兩數都等於0
*註：取出一個數字 'n' 最後一位: **n % 10*** ，將一個數字 'n' 右移一位: **n // 10***

## 程式碼
```python
def solve(n1, n2):
    count = carry = 0
    while n1 > 0 or n2 > 0:
        carry = 1 if n1 % 10 + n2 % 10 + carry >= 10 else 0
        count += carry
        n1 //= 10
        n2 //= 10
    # 處理題目輸出，因為我懶得寫那麼多，可以自己寫3個if來依據count的個數來print
    end = 's' if count > 1 else ''
    return 'No carry operation.' if count == 0 else f'{ count } carry operation{ end }.'
    
while True:
    try:
        n1, n2 = list(map(int, input().split()))
        if n1 == 0 and n2 == 0: break
        print(solve(n1, n2))
    except EOFError:
        break
```