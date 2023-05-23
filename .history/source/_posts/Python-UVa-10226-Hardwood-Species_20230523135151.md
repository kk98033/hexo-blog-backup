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
輸入一個數 **n** 代表有多測資， **接下來輸入空一行** 最後，輸入多個字串，代表樹種，算出每個樹種出現了幾次，以及所佔的比例。
輸出依照樹種 **字典序** 排序

#### Sample Input 
{% codeblock line_number:false %}
1
Red Alder
Ash
Aspen
Basswood
Ash
Beech
Yellow Birch
Ash
Cherry
Cottonwood
Ash
Cypress
Red Elm
Gum
Hackberry
White Oak
Hickory
Pecan
Hard Maple
White Oak
Soft Maple
Red Oak
Red Oak
White Oak
Poplan
Sassafras
Sycamore
Black Walnut
Willow
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
Ash 13.7931
Aspen 3.4483
Basswood 3.4483
Beech 3.4483
Black Walnut 3.4483
Cherry 3.4483
Cottonwood 3.4483
Cypress 3.4483
Gum 3.4483
Hackberry 3.4483
Hard Maple 3.4483
Hickory 3.4483
Pecan 3.4483
Poplan 3.4483
Red Alder 3.4483
Red Elm 3.4483
Red Oak 6.8966
Sassafras 3.4483
Soft Maple 3.4483
Sycamore 3.4483
White Oak 10.3448
Willow 3.4483
Yellow Birch 3.4483
{% endcodeblock %}

{% note warning %}
這題的輸入非常非常的奇怪、麻煩以及討厭，記得小心處理！！
{% endnote %}
---

## 解題思路
先把每個樹種存進陣列裡，接下來我們可以使用字典來算出每個樹種出現多少次，最後用`sorted`來對字典的`key`做排序就可以了<br>

非常遺憾，我嘗試使用Python解但始終都過不了(TLE)

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