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

給予一個 `alphabetical key` 以及 `numerical key` 
* 規則一： 每段文字會由－依序加密，空格保存不加密。
* 規則二： 如果這個字
Rule 1: Each word is encrypted separately; the space between words is preserved. Each word is
encrypted from the leftmost to the rightmost letters.
Rule 2: A plaintext message letter p that does not belong to L is enciphered as a ciphertext letter
c = ciph(p) where ciph is a function defined below.
Rule 3: A plaintext message letter p that belongs to L is enciphered as a string of three letters: the
m-th letter of L, followed by a ciphertext letter c = ciph(p), followed by the (m + 1)-th letter of
L. The value of m is incremented by one each time this rule is applied. Arithmetic is performed
as if L were circular.
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