---
title: Python UVa 10226 - Hardwood Species
date: 2023-05-23 13:23:18
tags:
  - UVa
  - UVa 一星
  - CPE 49題必考題
categories:
  - 解題心得
  - UVa
excerpt: 這題就是建表解密而已，只不過建表很麻煩，最好祈禱考CPE不要遇到他 - Python UVa 10226 - Hardwood Species 解題心得
description: 這題就是建表解密而已，只不過建表很麻煩，最好祈禱考CPE不要遇到他 - Python UVa 10226 - Hardwood Species 解題心得
---
# UVa 10226 Python 

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1167) - 10226 - Hardwood Species


## 題意


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
# 10226 - Hardwood Species
def solve(trees):
    if t != 0: print()

    count = {}
    for tree in trees:
        count[tree] = count.get(tree, 0) + 1
    n = len(trees)

    ''' attempt 2 TLE '''
    ans = sorted(count.keys())
    ans = [f'{key} {count[key] / n * 100:.4f}' for key in ans]
    return '\n'.join(ans)

    ''' attempt 1 TLE '''
    ans = sorted([[tree, f'{round(amount / n * 100, 4):.4f}'] for tree, amount in count.items()], key=lambda x: x[0])
    return '\n'.join([f'{tree} {amount}' for tree, amount in ans])

T = int(input())
for t in range(T):
    trees = []
    if t == 0: input()
    while True:
        try:
            tree = input()
            if tree: 
                trees.append(tree)
            else: 
                print(solve(trees))
                break
        except EOFError:
            print(solve(trees))
            break
# OnlineJudge: Time limit exceeded	PYTH3	3.000
# ZeroJudge: AC (5.2s, 76.1MB)
```