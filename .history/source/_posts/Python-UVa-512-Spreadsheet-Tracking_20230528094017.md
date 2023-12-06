---
title: Python UVa 512 - Spreadsheet Tracking
date: 2023-05-28 09:20:26
tags:
  - UVa
categories:
  - [解題心得, UVa]
  - [CPE歷屆, CPE 2023/05/23]
excerpt: 這題是CPE 2023/05/23 的第五題，屬於模擬題，照著題目說明做就可以了 - Python UVa 512 - Spreadsheet Tracking 解題心得
description: 這題是CPE 2023/05/23 的第五題，屬於模擬題，照著題目說明做就可以了 - Python UVa 512 - Spreadsheet Tracking 解題心得
---
# UVa 512 Python

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&page=show_problem&category=0&problem=453&mosmsg=Submission+received+with+ID+28504732) - UVa 512 - Spreadsheet Tracking


## 題意
給予一串指令，依照指令內容修改表格，接下來給予多個位置座標，輸出最後他的位置。<br>

有以下五種指令：
| 指令 |             動作             |
|:----:|:----------------------------:|
|  IR  |    在目標**row前**插入一個row    |
|  IC  | 在目標**column前**插入一個column |
|  DR  |          刪除目標row         |
|  DC  |        刪除目標column        |
|  EX  |     交換(r1, c1) (r2, c2)    |
{% note primary %}
**每行的指令動作都是同一時間完成的！！**
例如有一個指令是刪除第 row 1, 2, 3 ，會同時把1, 2, 3給刪除
{% endnote %}




#### Sample Input 
{% codeblock line_number:false %}
7 9
5
DR 2 1 5
DC 4 3 6 7 9
IC 1 3
IR 2 2 4
EX 1 2 6 5
4
4 8
5 5
7 8
6 5
0 0
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
Spreadsheet #1
Cell data in (4,8) moved to (4,6)
Cell data in (5,5) GONE
Cell data in (7,8) moved to (7,6)
Cell data in (6,5) moved to (1,2)
{% endcodeblock %}

---

## 解題思路


## Python程式碼
```python

```