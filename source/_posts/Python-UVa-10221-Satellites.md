---
title: Python/C++ UVa 10221 - Satellites
date: 2023-12-10 20:01:48
tags:
  - UVa
  - Python
categories:
  - [解題心得, UVa]
excerpt: 這題要求計算從地球上一點到一個衛星的最短距離，主要就是考你數學的余弦定理以及角度、弧度而已。 - Python/C++ UVa 10221 - Satellites 解題心得
description: 這題要求計算從地球上一點到一個衛星的最短距離，主要就是考你數學的余弦定理以及角度、弧度而已。 - Python/C++ UVa 10221 - Satellites 解題心得
---
# 

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=24&page=show_problem&problem=1162) - UVa 10221 - Satellites 


## 題意
這道題目要求我們計算兩個在相同軌道上的衛星之間的距離。題目提供了衛星與地球表面的距離和它們與地球中心形成的角度。需要計算的是這兩個衛星之間的`弧長 (arc distance)`距離和`弦長 (chord distance)`距離。

輸入三個數： `s(衛星到地球距離)` `a (角度)` `min or deg`
輸出`弧長` `弦長`

**deg**：指的是度。這是測量角度最常用的單位，一個完整圓圈是360度。

**min**：指的是分鐘，它是角度的更小的度量單位。1度等於60分鐘。

{% note info %}
* 要注意是角度或是弧度
* 最後的結果要取小數點後六位
* 地球半徑取 6440
{% endnote %}

#### Sample Input 
{% codeblock line_number:false %}
500 30 deg
700 60 min
200 45 deg
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
3633.775503 3592.408346
124.616509 124.614927
5215.043805 5082.035982
{% endcodeblock %}

---

## 解題思路
這題就是完完全全的數學了，如果忘記公式應該就解不出來了。（希望CPE不要考這題;-;）
**我們這題還需要用到python 的 `math` 模組**

一開始要先判斷是否是 `min`，把它轉成 `deg`
1 deg = 60 min

處理角度超過180度的情況，使用補角
```python
if a > 180: a = abs(360 - a) # 處理角度超過180度的情況，使用補角
```

**這題我們一率使用`弧度`來計算！！！**
轉換公式：
將度轉換為弧度：
$$ \text{弧度} = \text{度} \times \frac{\pi}{180} $$

最後用公式算出 `弧長`, `弦長` 即可。
弧長公式：
$$ L = r \times \theta $$

弦長公式：
$$ C = 2 \times r \times \sin\left(\frac{\theta}{2}\right) $$

**最後記得要四捨五入！！！**
```python
return f'{round(arc, 6):.6f} {round(chord, 6):.6f}'
```
在 Python 中，`round(arc, 6)` 用於將變數 `arc` 四捨五入到小數點後六位。其中，
- `round` 是 Python 的內置函數，
- 第一個參數 `arc` 是需要四捨五入的數字，
- 第二個參數 `6` 指定小數點後保留的位數。

`{round(arc, 6):.6f}` 中的 `:.6f` 是一個格式化字符串，用於確定輸出的格式：
- `:` 表示這之後是格式描述符。
- `.6` 表示保留六位小數。
- `f` 表示以浮點數格式輸出。

## Python程式碼
```python
# UVa 10221 - Satellites
'''
chord formula: https://flexbooks.ck12.org/cbook/ck-12-trigonometry-concepts/section/2.7/primary/lesson/length-of-a-chord-trig/
arc formula: https://byjus.com/arc-length-formula/
wiki: https://en.wikipedia.org/wiki/Circular_segment#:~:text=,shown%20above%20the%20green%20area
'''
import math
def solve(s, a, d):
    r = 6440 # earth radius

    if d == 'min': a /= 60 # 如果角度單位是分鐘，轉換成度
    if a > 180: a = abs(360 - a) # 處理角度超過180度的情況，使用補角

    rad = math.pi / 180 * a # 將角度從度轉換成弧度

    # 計算弧長，使用公式：弧長 = 半徑 * 弧度
    arc = (r + s) * rad

    # 計算弦長，使用公式：弦長 = 2 * 半徑 * sin(弧度 / 2)
    chord = 2 * (r + s) * math.sin(rad / 2)

    # 返回結果，並保留六位小數。使用round函數來實現，參考：https://stackoverflow.com/questions/19986662/rounding-a-number-in-python-but-keeping-ending-zeros
    return f'{round(arc, 6):.6f} {round(chord, 6):.6f}'

while True:
    try:
        s, a, d = input().split()

        print(solve(int(s), int(a), d))
    except EOFError:
        break
# Accepted	PYTH3	0.090
```

## C++程式碼

{% note primary %}
這是由 ChatGPT 從我的 Python 程式碼所轉換而成的，可以跑得過 UVa Online Judge。可能不是最好的C++寫法，僅供參考。
{% endnote %}
```cpp
/*
 * C++ Solution for "Satellites" Problem (UVa 10221)
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
 * Date: 2023/12/10
 */

#include <iostream>
#include <cmath>
#include <iomanip>
#include <sstream> // 包含sstream庫以使用ostringstream

using namespace std;

string solve(int s, double a, string d) {
    int r = 6440; // 地球半徑

    if (d == "min") a /= 60; // 如果角度單位是分鐘，轉換成度
    if (a > 180) a = abs(360 - a); // 處理角度超過180度的情況，使用補角

    double rad = M_PI / 180 * a; // 將角度從度轉換成弧度

    // 計算弧長，使用公式：弧長 = 半徑 * 弧度
    double arc = (r + s) * rad;

    // 計算弦長，使用公式：弦長 = 2 * 半徑 * sin(弧度 / 2)
    double chord = 2 * (r + s) * sin(rad / 2);

    // 使用ostringstream來格式化輸出
    ostringstream oss;
    oss << fixed << setprecision(6) << arc << " " << chord;
    return oss.str();
}

int main() {
    int s;
    double a;
    string d;

    while (cin >> s >> a >> d) {
        cout << solve(s, a, d) << endl;
    }

    return 0;
}
// Accepted	C++11	0.000
```