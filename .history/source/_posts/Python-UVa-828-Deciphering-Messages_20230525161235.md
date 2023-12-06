---
title: Python UVa 828 - Deciphering Messages
date: 2023-05-25 15:12:24
tags:
  - UVa
categories:
  - [解題心得, UVa]
  - [CPE歷屆, CPE 2023/05/23]
excerpt: 這題是CPE 2023/05/23 的第六題，只需依照題目的規則進行凱薩解密就可以了 - Python UVa 828 - Deciphering Messages 解題心得
description: 這題是CPE 2023/05/23 的第六題，只需依照題目的規則進行凱薩解密就可以了 - Python UVa 10222 - Decode the Mad man 解題心得
---
# UVa 828 Python

CPE 2023/05/23 第六題

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=0&problem=769&mosmsg=Submission+received+with+ID+28497412) - UVa 828 - Deciphering Messages


## 題意
給予一段加密後的文字，將他解密，如果此密文不合法，輸出'error in encryption'
加密的規則如下：<br>

給予一個 `alphabetical key` (L) 以及 `numerical key` (N)
* 規則一： 每個文字會由 **左到右** 依序加密， **空格保存不加密**。
* 規則二： 如果字元(p) **沒有出現在 L** 中，就加密成 `c = ciph(p)` (ciph下方會解釋)
* 規則三： 如果字元(p) **出現在 L** 中，就加密成 `第 m 個 L`, `c = ciph(p)`, `第 m + 1 個 L` 這三個字元， **m** 是 L 的 index<br>
  
**要注意！ L 是循環的！！**<br>

函式 `ciph(p)` 方法如下：
對 p 向右位移 **N** 個(numerical key)
**要注意！ 位移是循環的！！**<br>

假設 N = 2： ciph(A) = C, ciph(B) = D, . . . , ciph(X) = Z; ciph(Y) = A; ciph(Z) = B. <br>

對於第一個測資：
L: RSAEIO
N: 2
原文： RICE<br>

加密過程：

1. R
    m = 0
    R 有出現在 L 裡 -> 套用規則三
    加密成： 第 m 個 L, ciph(R), 第 m+1 個 L
        =>    R,        T,         S

2. I
    m = 1
    I 有出現在 L 裡 -> 套用規則三
    加密成： 第 m 個 L, ciph(I), 第 m+1 個 L
        =>    S,        K,         A
    m + 1

3. C
    C 沒有出現在 L 裡 -> 套用規則二
    加密成： ciph(C)
        =>    E

4. E
    m = 2
    E 有出現在 L 裡 -> 套用規則三
    加密成： 第 m 個 L, ciph(E), 第 m+1 個 L
        =>    A,        G,         E

所以加密後的文字: RTS/SKA/E/AGE (斜線只是分割方邊觀看)

|   | m |  加密後 |
|:-:|:-:|:-------:|
| R | 0 | R, T, S |
| I | 1 | S, K, A |
| C | 1 |    E    |
| E | 2 | A, G, E |

#### Sample Input 
{% codeblock line_number:false %}
1

RSAEIO
2
5
RTSSKAEAGE
GRSCAV
RGSSCAV
RUSIQO
RUSSGAACEV JEGIITOOGR
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
RICE
error in encryption
EAT
error in encryption
SEAT HERE
{% endcodeblock %}

{% note warning %}
注意：輸入t後會輸入一個空格
注意：每輪測資輸出後都需要空一個空格
{% endnote %}

---

## 解題思路
這題麻煩的是要我們 **解密** 而不是加密，這題重點是在於我們可不可以判斷加密後的字元是使用 **第二或是第三的規則或是無效** <br>

我這裡是主要判斷 **是否屬於第三個規則** ，如果是使用第三個規則，那麼加密後的文字第 **n** 以及第 **n+2** 個字元一定是屬於 **L中的 m 以及 m+1** ，如果不符合這樣的形式，那麼代表他是用第二個規則加密的。<br>

如果是套用第二個規則 => 對 **n**（也就是三個字元中間的字元）做解密
如果是套用第三個規則 => 對 **n+1** 做解密

## Python程式碼
```python
# UVa 828 - Deciphering Messages
def solve(key, numKey, texts):
    def decrypt(char):
        # A: 65, Z: 90
        if char == ' ': return char
        if ord(char) - numKey < 65:
            return chr(ord(char) - numKey + 26)
        return chr(ord(char) - numKey)
    
    for text in texts:
        index = cur = 0
        ans = ''
        # RTS SKA E AGE
        #  R   I  C  E
        error = False
        while index < len(text):
            if index + 2 < len(text) and (text[index] == key[cur%len(key)] and text[index+2] == key[(cur+1)%len(key)]):
                temp = decrypt(text[index+1])
                if temp in key:
                    ans += temp
                else:
                    error = True
                    break

                index += 3
                cur += 1
            else:
                temp = decrypt(text[index])
                if temp not in key:
                    ans += temp
                else:
                    error = True
                    break

                index += 1

        if error:
            print('error in encryption')
        else:
            print(ans)

T = int(input())
for t in range(T):
    input() # blank
    key = input()
    numKey = int(input())
    n = int(input())
    texts = []
    for i in range(n):
        texts.append(input())

    if t != 0: print()

    solve(key, numKey, texts)
# Accepted	PYTH3	0.040
```