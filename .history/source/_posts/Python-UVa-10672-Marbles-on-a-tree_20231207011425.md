---
title: Python UVa 10672 - Marbles on a tree
date: 2023-12-07 00:18:59
tags:
  - UVa
  - C++
  - Python
categories:
  - [解題心得, UVa]
  - [CPE歷屆, CPE 2023/10/17]
excerpt: 這題是CPE 2023/10/17 的第六題，是咱們最愛的DFS和Tree啦！ ;-; - Python/C++ UVa 10672 - Marbles on a tree 解題心得
description: 這題是CPE 2023/05/23 的第六題，是咱們最愛的DFS和Tree啦！ ;-; - Python/C++ UVa 10672 - Marbles on a tree 解題心得
---
# Python/C++ UVa 10672 - Marbles on a tree

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&problem=1613) - UVa 10672 - Marbles on a tree 

### 這題是2023/3/21 CPE 的第六題
這題是關於DFS的題目，事後分析過後發現其實沒有很複雜 ;-;

## 題意
這題是關於樹狀結構數據處理的問題。在這題中，需要處理一棵樹，樹的每個節點包含一定數量的彈珠。目標是通過將彈珠在節點間移動，使得最終每個節點恰好只有一個彈珠。最後算出總共最少要移動幾次。

**具體要求**
給定一棵樹，樹的每個節點有一定數量的彈珠。
可以將彈珠從一個節點移動到它的任意子節點或父節點。
最終目標是讓每個節點都只有一個彈珠。
需要計算出達成這個目標所需的最小彈珠移動次數。

**Input 格式**
* 第一行：一個整數 N，表示樹中的節點數量。當 N 為 0 時，表示輸入結束。

* 接下來的 `N` 行：每行代表一個節點的資訊，格式如下：
    第一個數字是節點編號 `v`（1 到 N 之間的整數）。
    第二個數字是該節點擁有的彈珠數量。
    第三個數字是該節點的子節點數量 `d`。
    接下來的 `d` 個數字分別是該節點的子節點編號。


#### Sample Input 
`9`
`1 2 3 2 3 4`
`2 1 0`
`3 0 2 5 6`
`4 1 3 7 8 9`
`5 3 0`
`6 0 0`
`7 0 0`
`8 2 0`
`9 0 0`
`9`
`1 0 3 2 3 4`
`2 0 0`
`3 0 2 5 6`
`4 9 3 7 8 9`
`5 0 0`
`6 0 0`
`7 0 0`
`8 0 0`
`9 0 0`
`9`
`1 0 3 2 3 4`
`2 9 0`
`3 0 2 5 6`
`4 0 3 7 8 9`
`5 0 0`
`6 0 0`
`7 0 0`
`8 0 0`
`9 0 0`
`0`

#### Sample Output 
`7`
`14`
`20`

---
## 解題思路
這題需要使用 `DFS` 來解，他的 DFS 邏輯差不多就是遍歷每個子節點，計算從該子節點到當前節點所需的移動次數和彈珠數。
最後回傳的值就是答案。

我用一個二維陣列來儲存樹的資料，注意我有在每個節點上紀錄他的父節點這樣會方便遍歷所有樹的節點，因為題目並沒有給講哪個是根節點。
這使得從任何節點開始 DFS 都能夠遍歷整棵樹，即使題目沒有指定哪個是根節點。
```python
for child in temp[3:]: # 在子節點中紀錄他的父節點(方便遍歷所有樹的節點)
    trees[child].append(temp[0])
```

在一開始呼叫的時候假設1是根節點，由於先前有記錄每個節點的父節點，因此用 DFS 還是可以遍歷所有的節點
```python
ans, _ = dfs(1, -1) # 先假設節點1是跟節點，他沒有父節點(-1)
```

我寫的遞迴主要有兩個參數：`node`, `parent`，分別代表目前的節點以及他的父節點。
首先會先計算該節點還差多少才能滿足只有一個珠珠，可正可負，負數代表要拿走多少個，正數代表還需要幾個。
```python
needed = 1 - marbles[node]
```

接下來依序查找他的子節點，算出子節點總共還需要多少珠珠以及需要移動幾次。

遞迴中記得避免重複遍歷
```python
if child != parent: # 避免重複遍歷，導致遞迴出不去
```

最後遞迴會回傳`總共移動次數`, `需要移動才能變成剛好一個珠珠的次數`
因為拿出或是拿入都要算盡總共移動的次數，所以記得加上絕對值
```python
moves + abs(needed)
```

## Python程式碼
```python
# UVa 10672 - Marbles on a tree
def dfs(node, parent):
    needed = 1 - marbles[node] # 這個節點還差多少才能滿足只有一個珠珠(可正可負)
    moves = 0

    for child in trees[node]:
        if child != parent: # 避免重複遍歷，導致遞迴出不去
            childMoves, childNeeded = dfs(child, node)
            needed += childNeeded # 累加子節點需要從父節點接收或給予的珠珠數（因為珠珠要移動到子節點時一定會經過父節點）
            moves += childMoves # 紀錄總共的移動次數（這是因為無論是給予還是接收彈珠，都算作一次移動）

    return moves + abs(needed) , needed 

while True:
    try:
        n = int(input())
        if n == 0: break

        trees = [[] for _ in range(n + 1)] # 用二維陣列來存取樹
        marbles = [0] * (n + 1)

        for _ in range(n):
            temp = list(map(int, input().split()))
            trees[temp[0]].extend(temp[3:])
            for child in temp[3:]: # 在子節點中紀錄他的父節點(方便遍歷所有樹的節點)
                trees[child].append(temp[0])
            marbles[temp[0]] = temp[1]

        ans, _ = dfs(1, -1) # 先假設節點1是跟節點，他沒有父節點(-1)
        print(ans)

    except EOFError:
        break
# Accepted	PYTH3	0.270
```

{% note primary %}
這是由 ChatGPT4 從我的 Python 程式碼所轉換而成的，可以跑得過 UVa Online Judge。可能不是最好的C++寫法，僅供參考。
{% endnote %}

## C++程式碼
```c++
/*
 * C++ Solution for "Marbles on a Tree" Problem (UVa 10672)
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
 * Date: 2023/12/07
 */

#include <iostream>
#include <vector>
#include <cmath>
using namespace std;

vector<int> trees[10001]; // 用來存儲樹的結構
int marbles[10001]; // 用來存儲每個節點的彈珠數量

// DFS 函數，返回一對整數，分別代表移動次數和所需的彈珠數
pair<int, int> dfs(int node, int parent) {
    int needed = 1 - marbles[node]; // 計算當前節點還需要多少彈珠
    int moves = 0;

    for (int child : trees[node]) {
        if (child != parent) {
            auto [childMoves, childNeeded] = dfs(child, node);
            needed += childNeeded;
            moves += childMoves;
        }
    }

    return {moves + abs(needed), needed};
}

int main() {
    int n;
    while (cin >> n && n != 0) {
        // 初始化樹結構和彈珠陣列
        for (int i = 0; i <= n; ++i) trees[i].clear();
        fill(marbles, marbles + n + 1, 0);

        int node, marblesCount, childrenCount;
        for (int i = 0; i < n; ++i) {
            cin >> node >> marblesCount >> childrenCount;
            marbles[node] = marblesCount;
            while (childrenCount--) {
                int child;
                cin >> child;
                trees[node].push_back(child);
                trees[child].push_back(node);
            }
        }

        auto [ans, _] = dfs(1, -1); // 從節點 1 開始 DFS
        cout << ans << endl;
    }

    return 0;
}
// Accepted	C++	0.000
```