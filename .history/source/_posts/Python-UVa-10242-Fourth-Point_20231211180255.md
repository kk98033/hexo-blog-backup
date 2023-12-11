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
這題簡單來說就是在已知平行四邊形三個點的座標，找出第四點座標。
公式如下：
{% codeblock line_number:false %}
   B _________ D
   /          /
  /          /
 A _________ C
{% endcodeblock %}

在這個平行四邊形中:
- A, B, C, 和 D 是平行四邊形的四個頂點。
- 如果我們知道 A, B, 和 C 的座標，我們可以用公式 `D = B + C - A` 來計算點 D 的座標。

這題的輸入是輸入平行四邊形兩個邊的起點、終點座標，分別為： ** x1, y1, x2, y2, x3, y3, x4, y4 **

* 注意輸入的數字是浮點數，輸入時要記得用 **float** 轉換
```python
points = list(map(float, input().split()))
```

另外我在呼叫函式的時候，使用了
```python
solve(*points)
```
`*` 符號（稱為「星號運算符」或「解包運算符」）用於函數呼叫時有特別的意義，特別是當你將列表或元組作為參數傳遞給函數時。
以這題的程式碼來看：
points 是一個包含多個值的陣列。
當 * 放在 points 之前時，它會「解包」這個列表，意味著它會將列表中的每個元素作為獨立的參數傳遞給 solve 函數。

舉例來說，假設 points 是這樣的一個列表 [1, 2, 3, 4, 5, 6, 7, 8]。當呼叫 solve(*points) 時，它等效於 solve(1, 2, 3, 4, 5, 6, 7, 8)。這就是說，列表中的每個元素都被當作單獨的參數傳遞給了 solve 函數。

{% note info %}
你可以不用使用跟我依樣的呼叫，這裡只是順便介紹 python 可以這樣做 :D
{% endnote %}

我們這題需要先 **決定哪個座標是兩個邊的交點(也就是重複出現的座標，參考上方平行四邊形的 A 點)**，
這裡我們可以直接用 if 來判斷哪個點是重複的點，再來套用公式就好了。

注意輸出的時候需要**四捨五入到後三位**，我這裡是使用`f-string`
`:3f` 格式代表的是：
這部分指定了輸出格式，其中 .3f 表示結果將格式化為固定點數表示，並保留三位小數。

## Python程式碼
```python
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

## C++程式碼

{% note primary %}
這是由 ChatGPT 從我的 Python 程式碼所轉換而成的，可以跑得過 UVa Online Judge。可能不是最好的C++寫法，僅供參考。
{% endnote %}
```cpp
/*
 * C++ Solution for "Fourth Point !!" Problem (UVa 10242)
 * 
 * Description:
 * This code is a C++ translation of a Python solution for the "Marbles on a Tree" problem,
 * originally developed and converted using ChatGPT4. The solution has been verified to
 * pass on UVa Online Judge.
 *
 * Note:
 * This implementation serves as a reference and may not represent the most optimized approach.
 * It demonstrates a basic use of depth-first search (DFS) in tree data structures.
 *
 * Author: Chia Yu Yang
 * Translated by: ChatGPT4
 * Date: 2023/12/11
 */

#include <iostream>
#include <iomanip>  // 用於設置輸出格式
using namespace std;

// 解決問題的函數
void solve(double x1, double y1, double x2, double y2, double x3, double y3, double x4, double y4) {
    if (x1 == x3 && y1 == y3) {
        cout << fixed << setprecision(3) << (x2 + x4) - x1 << " " << (y2 + y4) - y1 << endl;
    } else if (x1 == x4 && y1 == y4) {
        cout << fixed << setprecision(3) << (x2 + x3) - x1 << " " << (y2 + y3) - y1 << endl;
    } else if (x2 == x3 && y2 == y3) {
        cout << fixed << setprecision(3) << (x1 + x4) - x2 << " " << (y1 + y4) - y2 << endl;
    } else {
        cout << fixed << setprecision(3) << (x1 + x3) - x2 << " " << (y1 + y3) - y2 << endl;
    }
}

int main() {
    double x1, y1, x2, y2, x3, y3, x4, y4;
    while (cin >> x1 >> y1 >> x2 >> y2 >> x3 >> y3 >> x4 >> y4) {
        solve(x1, y1, x2, y2, x3, y3, x4, y4);
    }
    return 0;
}
// Accepted	C++11	0.000
```