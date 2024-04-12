---
title: Python/C++ UVa 11150 - Cola
date: 2024-04-12 23:15:45
tags:
  - UVa
  - Python
  - C++
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 這題雖然被歸類為模擬題，你可以直接照題目意思模擬，但是其實可以直接用數學解出來。 - Python/C++ UVa 11150 - Cola 解題心得
description: 這題雖然被歸類為模擬題，你可以直接照題目意思模擬，但是其實可以直接用數學解出來。 - Python/C++ UVa 11150 - Cola 解題心得
---
# Python/C++ UVa 11150 - Cola 解法

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=2091) - UVa 11150 - Cola


## 題意
題目假設你每買一瓶可樂就會得到一個空瓶子，而每當你收集到三個空瓶子時，就可以免費換得一瓶新的可樂。問題是，給定一開始你可以購買的可樂數量，你需要計算出在充分利用空瓶兌換條件下，最終能喝到的最大可樂數量。

#### Sample Input 
{% codeblock line_number:false %}
8
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
12
{% endcodeblock %}

---

## 解題思路
#### 解法一：直接用數學計算
公式非常簡單：
`n * 1.5`， **取下高斯（直接取 int）** 就完成了。

公式解釋：
第一輪：你用 *n* 個空瓶換得 *n/3* 瓶可樂。
第二輪：這 *n/3* 瓶可樂再產生 *n/3* 個空瓶，再換得 *n/9* 瓶可樂。
持續這樣下去直到不能再換為止。

當你把這個無窮級數 $$n + \frac{n}{3} + \frac{n}{9} + \frac{n}{27} + \dots$$ 加總起來，其實是 $$n \times (1 + \frac{1}{3} + \frac{1}{9} + \frac{1}{27} + \dots)$$，而這個級數的和是 $$n \times \frac{1}{1 - \frac{1}{3}} = n \times 1.5$$


#### 解法二：用模擬方式計算（參考 C++ 解法）
1. 初始購買：根據初始數量 n，計算起始的可樂數量及對應的空瓶數 **（都是 n）**。
2. 重複換購：每次迴圈檢查當前的空瓶數是否足夠兌換新的可樂。如果足夠，則計算能兌換多少瓶可樂並更新空瓶數。
3. 邊界處理：當最後空瓶數**等於2**時，可考慮**借一個空瓶**來換取一瓶新的可樂，**因為後續你會有三個空瓶可以還掉一個空瓶並再次兌換。**

## Python程式碼
```python
# UVa 11150 - Cola
while True:
    try:
        n = int(input())

        print(int(n * 1.5))
    except EOFError:
        break
# Accepted	PYTH3	0.050
```

## C++ 程式碼（模擬方式解法）
```cpp
// UVa 11150 - Cola
#include <stdio.h>

int main() {
    int n;
    while(scanf("%d", &n) != EOF) {
        int total = n;  // 初始可樂數量
        int bottles = n;  // 初始空瓶數

        while (bottles >= 3) {
            int new_cola = bottles / 3;
            total += new_cola;
            bottles = bottles % 3 + new_cola;
        }

        // 如果最後剩下2個空瓶，可以借一個空瓶來換一瓶可樂
        if (bottles == 2) {
            total += 1;
        }

        printf("%d\n", total);
    }
    return 0;
}
// Accepted	C++11	0.000
```