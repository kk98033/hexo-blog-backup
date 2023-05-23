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
excerpt: 我被這題搞了好久，一直TLE，最後發現是input的數量太多了導致超時，如果你用python做也超時，可以參考我下方的方法 - Python UVa 10226 - Hardwood Species 解題心得
description: 我被這題搞了好久，一直TLE，最後發現是input的數量太多了導致超時，如果你用python做也超時，可以參考我下方的方法 - Python UVa 10226 - Hardwood Species 解題心得
---
# UVa 10226 Python 

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1167) - 10226 - Hardwood Species


## 題意
輸入一個數 **n** 代表有多測資， **接下來輸入空一行** 最後，輸入多個字串，代表樹種，算出每個樹種出現了幾次，以及所佔的比例 （**取到小數點第四位**）。
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

使用字典來算出出現的次數可以這樣做：
```python
count = {}
for tree in trees:
    count[tree] = count.get(tree, 0) + 1
n = len(trees)
```
<br>

或者使用`Counter`模組   
```python
from collections import Counter
count = Counter(trees)
```
<br>

取到小數點後四位我用`f-string`，非常好用，建議學一下
```python
f'{round(amount / n * 100, 4):.4f}'
```

~~非常遺憾，我嘗試使用Python解但始終都過不了(TLE ;-; )~~<br>

---<br>

## 5/23更新
感謝 [ChatGPT](https://chat.openai.com/chat) 的建議:
{% codeblock line_number:false %}
很抱歉您仍然遇到时间限制超过的问题。在这种情况下，您可以尝试使用更高效的输入和输出方法，以减少IO操作的开销。
{% endcodeblock %}

{% codeblock line_number:false %}
在这个版本中，我们使用sys.stdin.readline()来读取输入，以及使用sys.stdout.write()来输出结果。这些方法比标准输入输出更快，并且可以减少不必要的IO操作。
{% endcodeblock %}
<br>

照著他的說法，我把
```python
tree = input()
```
替換成
```python
tree = sys.stdin.readline().strip()
```
記得 `import sys`

就過了 :|
`OnlineJudge: Accepted	PYTH3	1.840`

## Python程式碼
```python
# 10226 - Hardwood Species
from collections import Counter
import sys
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

def solve2(trees):
    count = Counter(trees)
    n = len(trees)
    
    ans = {k: count[k] for k in sorted(count)}
    ans = [f'{key} {count[key] / n * 100:.4f}' for key in ans]
    return '\n'.join(ans)

T = int(input())
for t in range(T):
    trees = []
    if t == 0: input()
    if t != 0: print()
    while True:
        try:
            # tree = input()
            tree = sys.stdin.readline().strip() # bruh
            if tree: 
                trees.append(tree)
            else: 
                print(solve2(trees))
                break
        except EOFError:
            # print(solve(trees))
            print(solve2(trees))
            break
# Attempt ???: OnlineJudge: Accepted	PYTH3	1.840
# Attempt 1:   OnlineJudge: Time limit exceeded	PYTH3	3.000
# ZeroJudge: AC (5.2s, 76.1MB)
```