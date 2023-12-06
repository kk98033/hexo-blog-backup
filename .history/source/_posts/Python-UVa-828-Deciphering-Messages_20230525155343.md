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
L: RSAEIO

1. R
    R 有出現在 L 裡 -> 套用規則三
    加密成： 第 m 個 L, ciph(R), 第 m+1 個 L
        =>    R,        T,         A
2. I
    I 有出現在 L 裡 -> 套用規則三
    加密成： 第 m 個 L, ciph(I), 第 m+1 個 L
        =>    R,        K,         A

#### Sample Input 
{% codeblock line_number:false %}
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
{% endcodeblock %}

---

## 解題思路


## Python程式碼
```python

```