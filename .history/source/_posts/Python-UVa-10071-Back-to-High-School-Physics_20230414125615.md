---
title: Python UVa 10071 - Back to High School Physics
date: 2023-04-14 12:39:44
tags:
  - UVa
  - UVa 一星
categories:
  - 解題心得
  - UVa
excerpt: Python UVa 10071 - Back to High School Physics 解題心得
description: Python UVa 10071 - Back to High School Physics 解題心得
---
# Python UVa 10071 - Back to High School Physics

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1012) - UVa 10071 - Back to High School Physics
<br/>

## 題意
給予兩數，`v`, `t`，假設在 t 秒後此粒子的速度為 v ，請問這個粒子在 **2t** 秒後所經過的位移是多少。


#### Sample Input
{% codeblock line_number:false %}
0 0
5 12
{% endcodeblock %}

#### Sample Output
{% codeblock line_number:false %}
0
120
{% endcodeblock %}

---

## 解題思路
因為是等速移動，所以在2t時間的位移就是 `v * t * 2`


## Python程式碼
```python
# 10071 - Back to High School Physics
while True:
    try:
        v, t = list(map(int, input().split()))
        print(v * 2 * t)
    except EOFError:
        break
# Accepted	PYTH3	1.190
```