---
title: Python UVa 12455 - Bars
date: 2023-05-28 15:41:06
tags:
  - UVa
categories:
  - [解題心得, UVa]
  - [CPE歷屆, CPE 2023/05/23]
excerpt: 這題是CPE 2023/05/23 的第四題，動態規劃的背包問題，也可以直接暴力硬做 - Python UVa 12455 - Bars 解題心得
description: 這題是CPE 2023/05/23 的第四題，動態規劃的背包問題，也可以直接暴力硬做 - Python UVa 12455 - Bars 解題心得
---
# UVa 12455 Python

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&page=show_problem&category=24&problem=3886) - UVa 12455 - Bars


## 題意
給予許多不同長度的金屬棒，找出是否可以用其中特定的金屬棒拼成所需要的長度，金屬棒不可切割，可以的話輸出 **'YES'** 不能輸出 **'NO'**

#### Sample Input 
{% codeblock line_number:false %}
4
25
4
10 12 5 7
925
10
45 15 120 500 235 58 6 12 175 70
120
5
25 25 25 25 25
0
2
13 567
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
NO
YES
NO
YES
{% endcodeblock %}

---

## 解題思路
一開始我使用暴力法直接硬做，我才也許是遞迴的關係，超時了(可以參考下方的`solve()`)<br>

後面把他改成 **動態規劃** 就過了<br>

**具體動態規劃作法如下：**
可參考 `solve2()`
先宣告一個 `target+1` 大小的陣列 **dp** (target為目標長度)，陣列內容為 `False` ，index `i` 代表 bar 是否可以組成 i 長度的，**dp的第 0 個元素要先設成 `True`**<br>

接下來我們依序的檢查每個 bar，<br>

**dp 邏輯為:**
如果 **dp[i] 為 True** 以及 **bar + i < target**，那麼代表目前的bar可以組成 `bar + i` 的長度，因此 `dp[bar + i] = True`
```python
if dp[i] and bar + i <= target:
    dp[bar + i] = True
```

**記得 dp 要從後面開始做!!**(避免多算)
如果從前面開始做，假設 bar 是 7
那麼到 `dp[7]` 的時候因為 dp[7] 為 True(**因為算`dp[0]` 的時候會讓 `dp[7]` 為 True!**) 以及 `7 + 7 < target` 所以就會執行 `dp[7 + 7] = True` 也就是 `dp[14]` 會變成 True <br>

但如果我們從後面開始做，遇到 `dp[7]` 時，由於 `dp[7]` 為 False，就不會繼續執行下`dp[14] = True`這串指令<br>

**完整DP程式碼：**
```python
for bar in nums:
    for i in range(target, -1, -1): # 記得從後面開始
        if dp[i] and bar + i <= target:
            dp[bar + i] = True
```

**使用Python內建套件的暴力法**
最後我順便介紹一個 python 的內部套件，叫做`itertools.combinations`，這個套件可以比較快速的取得所有陣列的排列組合。記得要`import itertools`<br>

我做法是先取得所有的排列組合，接下來再用sum算出長度，看是否為要找的長度，一樣就輸出 'YES' 否則輸出 'NO' (可以參考下方的`solve3()`)<br>



## Python程式碼
```python
# UVa 12455 - Bars
import itertools
def solve(target, n, nums):
    ''' 暴力法 '''
    def dfs(n, nums, cur, result):
        result.append(cur)

        if n == 0: 
            return
        
        for i in range(len(nums)):
            dfs(n-1, nums[:i], cur + nums[i], result)
        return
        
    result = []
    dfs(n, nums, 0, result)

    return 'YES' if target in result else 'NO'
    # Time limit exceeded	PYTH3	1.000
    
def solve3(target, n, nums):
    ''' itertools.combinations '''
    for i in range(n+1):
        for subset in itertools.combinations(nums, i):
            if sum(subset) == target:
                return 'YES'
    return 'NO'
    # Accepted	PYTH3	0.010


def solve2(target, n, nums):
    ''' DP '''
    dp = [False] * (target + 1)
    dp[0] = True
    for bar in nums:
        for i in range(target, -1, -1): # 記得從後面開始
            if dp[i] and bar + i <= target:
                dp[bar + i] = True
    return 'YES' if dp[-1] else 'NO'
    # Accepted	PYTH3	0.020

T = int(input())
for t in range(T):
    target = int(input())
    n = int(input())
    nums = list(map(int, input().split()))

    # print(solve(target, n, nums))
    # print(solve2(target, n, nums))
    print(solve3(target, n, nums))
# Accepted	PYTH3	0.020
```