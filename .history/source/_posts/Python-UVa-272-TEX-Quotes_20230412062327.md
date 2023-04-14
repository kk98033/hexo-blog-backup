---
title: Python UVa 272 - TEX Quotes
date: 2023-03-19 20:47:25
tags:
  - UVa
  - UVa 一星
categories:
  - 解題心得
  - UVa
excerpt: Python UVa 272 - TEX Quotes 解題心得
description: Python UVa 272 - TEX Quotes 解題心得
---
# Python UVa 10190 - Divide, But Not Quite Conquer

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=0&problem=208&mosmsg=Submission+received+with+ID+28319792) - UVa 272 - TEX Quotes



## 題意
給予一連串的字串，依照下列的模式來修改字串
如果字串是：
```text
"To be or not to be," quoth the bard, "that is the question."
```
必須修改成：
```text
``To be or not to be,'' quoth the bard, ``that is the question.''
```
簡而言之，就是把第一個"修改成``，第二個"改成'' ...

{% note primary %}
 - 文章是一行一行輸入的，但每行字串視為同一個文章，也就是說count要繼續算下去
{% endnote %}

#### Sample Input 
```text
"To be or not to be," quoth the Bard, "that
is the question".
The programming contestant replied: "I must disagree.
To `C' or not to `C', that is The Question!"
```

#### Sample Output 
```text
``To be or not to be,'' quoth the Bard, ``that
is the question''.
The programming contestant replied: ``I must disagree.
To `C' or not to `C', that is The Question!''
```

---
## 解題思路
這題很容易，只需要用迴圈去找"的位置，再判斷他是奇數或是偶數，奇數就改成``，偶數就改成''(依照你count初始的值來訂)
這題要注意的點是他每次的輸入count都要接續下去



## Python程式碼
```python
# UVa 272 - TEX Quotes
count = 0 # 記得放在最外圍，不要歸0
while True:
    try:
        s = list(input())
        for i in range(len(s)):
            if s[i] == '"':
                if count % 2 == 0: 
                    s[i] = '``'
                else:
                    s[i] = "''"
                count += 1
        print(''.join(s))
    except EOFError:
        break
# Accepted	PYTH3	0.020
```