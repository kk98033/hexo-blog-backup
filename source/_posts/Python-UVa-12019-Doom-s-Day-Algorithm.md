---
title: Python UVa 12019 - Doom's Day Algorithm
date: 2024-05-02 08:44:45
tags:
  - UVa
  - Python
  - CPE 49題必考題
categories:
  - [解題心得, UVa]
excerpt: 這題目的是計算某個 2011 年的日期在一週是星期幾，可以直接計算，也可以用 Python 的內建函式 datetime - Python UVa 12019 - Doom's Day Algorithm 解題心得
description: 這題目的是計算某個 2011 年的日期在一週是星期幾，可以直接計算，也可以用 Python 的內建函式 datetime - Python UVa 12019 - Doom's Day Algorithm 解題心得
---
# UVa 12019 - Doom's Day Algorithm Python 解法

>[題目連結](https://onlinejudge.org/index.php?option=com_onlinejudge&Itemid=8&category=242&page=show_problem&problem=3170) - UVa 12019 - Doom's Day Algorithm


## 題意
這道題目介紹了一種名為 "Doom's Day" 算法。簡單來說，這題就是**給定2011年的一個日期，計算該日期是星期幾。**

翻譯一下 "Doom's Day" 算法：
這個算法建立在一個概念上，稱為"doomsday"（末日），這是每年固定出現在某些日期的星期幾。例如，4月4日、6月6日、8月8日、10月10日和12月12日這些日期，每年的這些天都是doomsday。每一年都有自己的doomsday。例如2011年的doomsday是星期一，因此2011年的4/4、6/6、8/8、10/10和12/12都是星期一。有了這個信息，就可以輕易計算其他任何日期的星期幾。例如，2011年12月13日是星期二，12月14日是星期三等等。

其他在doomsday發生的日期還包括5月9日、9月5日、7月11日和11月7日。在閏年，doomsday還包括1月11日和2月22日；非閏年則是1月10日和2月21日。

#### Sample Input 
{% codeblock line_number:false %}
8
1 6
2 28
4 5
5 26
8 1
11 1
12 25
12 31
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
Thursday
Monday
Tuesday
Thursday
Monday
Tuesday
Sunday
Saturday
{% endcodeblock %}

---

## 解題思路
這題題目上介紹的 "Doom's Day" 算法其實不重要，這題我們可以直接解，不用理那個算法。

### 解法一
這裡我先介紹 python 內建的函式：`datetime` 來解。

```python
date = datetime.date(2011, month, day)
```
這一行創建了一個 datetime.date 對象，它代表了2011年的特定月份和日期。這裡固定年份為2011。

接下來用 date 來得到是星期幾：
```python
dayOfWeek = date.weekday()  # 返回 0-6，其中0是星期一，6是星期日
```
這裡使用了 `date.weekday()` 方法，它從 datetime.date 對象中計算出日期對應的星期數。這個方法返回一個整數值，其中**0代表星期一，6代表星期日。**

最後依照回傳的數值查表輸出星期就可以了，非常簡單！

### 解法二
如果不知道 datetime 函式的用法，也可以直接解，不會太難。
我先創了兩個陣列，一個紀錄每個月有幾天，另一個紀錄星期的名稱表。
名稱表我們從 `Saturday` 開始，因為該年 1/1 是 Saturday。
{% note info %}
我們可以從題目的描述： *'其他在doomsday發生的日期還包括5月9日、9月5日、7月11日和11月7日。在閏年，doomsday還包括1月11日和2月22日；非閏年則是1月10日和2月21日'* 得知以下結果：

根據描述，因為 2011 年不是閏年，所以 1/10 號是 doomsday，也就是 `Monday`，由此可以依序推敲 1/1 是 `Saturday`
{% endnote %}

要計算某一天是星期幾，我們可以**計算該天數距離該年某一天的天數**，然後將其 `% 7`，將其結果對照陣列的日期就可以得出該天是星期幾（陣列的排序要依據你選定的那一天星期開始排，例如 1/1 是 `Saturday`，所以陣列要是：`Saturday`, `Sunday` ... 的順序）。

最簡單計算距離該年某一天的天數很簡單，可以直接用 for 迴圈：
```python
# 從1月1日到給定日期的總天數
days = date # 已經包括當月到目前日期的天數
for i in range(month - 1): # 只累加到前一個月
    days += months[i]
```

接下來將`(結果 -1) % 7 ` 對照陣列即可：
```python
weekOfDay[(days-1) % 7]
```
**這裡減1是因為1月1日是年的第一天，也是列表weekOfDay中的第0個元素（Saturday），不然每天的日期都會偏離一天！**

## Python程式碼
```python
# UVa 12019 - Doom's Day Algorithm
def solve(month, day):
    # Monday, Tuesday, Wednesday, Thursday, Friday, Saturday, Sunday
    # 1/10 - Monday
    # 1/9 - Sunday
    # 1/8 - Saturday
    # 1/7 - Friday
    # 1/6 - Thursday
    # 1/5 - Wednesday
    # 1/4 - Tuesday
    # 1/3 - Monday
    # 1/2 - Sunday
    # 1/1 - Saturday

    # 月份天數
    months = [31, 28, 31, 30, 31, 30, 31, 31, 30, 31, 30, 31]

    # 星期名稱
    weekOfDay = ['Saturday', 'Sunday', 'Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday']
    
    # 從1月1日到給定日期的總天數
    days = day # 已經包括當月到目前日期的天數
    for i in range(month - 1): # 只累加到前一個月
        days += months[i]

    # 計算星期幾
    return weekOfDay[(days-1) % 7]
 
T = int(input())
for t in range(T):
    month, day = list(map(int, input().split()))
    print(solve(month, day))
# 	Accepted	PYTH3	0.050
```

## Python程式碼(使用內建 datetime 函式)
```python
import datetime
def solve2(month, day):
    # Accepted	PYTH3	0.020
    date = datetime.date(2011, month, day)
    weekdays = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
    dayOfWeek = date.weekday()  # 返回 0-6，其中0是星期一，6是星期日
    return weekdays[dayOfWeek]
```