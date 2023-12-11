---
title: Python/C++ UVa 10242 - Fourth Point
date: 2023-12-10 20:54:45
tags:
  - UVa
  - Python
categories:
  - [解題心得, UVa]
excerpt: 給定平行四邊形的三個點，求第四點的座標。又双叒叕是數學題。 - Python/C++ UVa 10242 - Fourth Point 解題心得
description: 給定平行四邊形的三個點，求第四點的座標。又双叒叕是數學題。 - Python/C++ UVa 10242 - Fourth Point 解題心得
---
# 

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1183) - UVa 10242 - Fourth Point


## 題意
給定平行四邊形的三個點，求第四點的座標。

#### Sample Input 
{% codeblock line_number:false %}
0.000 0.000 0.000 1.000 0.000 1.000 1.000 1.000
1.000 0.000 3.500 3.500 3.500 3.500 0.000 1.000
1.866 0.000 3.127 3.543 3.127 3.543 1.412 3.145
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
1.000 0.000
-2.500 -2.500
0.151 -0.398
{% endcodeblock %}

---

## 解題思路
利用平行四邊形的性質，對邊平行且等長，我們可以用向量的方式來找出第四個點。如果已知點是 A(x1, y1), B(x2, y2), 和 C(x3, y3)，
則第四點 D(x4, y4) 可以用以下公式計算：
$$ \[ D = B + C - A \] $$
```python
'''
    B ____ C
     /___/
    A     D
'''
# UVa 10242 - Fourth Point !!
def solve(x1, y1, x2, y2, x3, y3, x4, y4):
    if x1 == x3 and y1 == y3: # 檢查點1和點3是否相同（即它們是重複的點）
        return f"{(x2 + x4) - x1:.3f} {(y2 + y4) - y1:.3f}"
    
    elif x1 == x4 and y1 == y4: # 檢查點1和點4是否相同
        return f"{(x2 + x3) - x1:.3f} {(y2 + y3) - y1:.3f}"
    
    elif x2 == x3 and y2 == y3: # 檢查點2和點3是否相同
        return f"{(x1 + x4) - x2:.3f} {(y1 + y4) - y2:.3f}"
    
    else: # 如果上述條件都不符合，則點2和點4是中點
        return f"{(x1 + x3) - x2:.3f} {(y1 + y3) - y2:.3f}"

while True:
    try:
        points = list(map(float, input().split()))
        print(solve(*points))
    except EOFError:
        break
# ???
```

## Python程式碼
```python

```