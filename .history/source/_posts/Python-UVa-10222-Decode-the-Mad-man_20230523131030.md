---
title: Python UVa 10222 - Decode the Mad man
date: 2023-05-23 12:56:37
tags:
  - UVa
  - UVa 一星
  - CPE 49題必考題
categories:
  - 解題心得
  - UVa
excerpt: 這題就是建表解密而已，只不過建表很麻煩，最好祈禱考CPE不要遇到他 - Python UVa 10222 - Decode the Mad man 解題心得
description: 這題就是建表解密而已，只不過建表很麻煩，最好祈禱考CPE不要遇到他 - Python UVa 10222 - Decode the Mad man 解題心得
---
# 

>[題目連結](https://onlinejudge.org/index.php?option=onlinejudge&Itemid=8&category=14&page=show_problem&problem=1163) - UVa 10222 - Decode the Mad man


## 題意
給予一個字串，輸出解密過後的字串


#### Sample Input 
{% codeblock line_number:false %}
k[r dyt I[o
{% endcodeblock %}

#### Sample Output 
{% codeblock line_number:false %}
how are you
{% endcodeblock %}

---

## 解題思路
觀察範例輸入輸出後，可以發現他的規則是： **原文 = 輸出的字元在 *鍵盤* 上位置左移兩格**
這題我是直接建一個字典`dict`([感謝他的字典:D]([https://](https://zerojudge.tw/ShowThread?postid=33728&reply=0)))

## Python程式碼
```python
# UVa 10222 - Decode the Mad man
# Thanks for you dictionary bro :D https://zerojudge.tw/ShowThread?postid=33728&reply=0
def solve(s):
    return ''.join([dict[i.lower()] for i in s])

# 反斜線要用: '\\'表示
dict = {
        'e':'q','d':'a','c':'z','r':'w','f':'s','v':'x','t':'e','g':'d','b':'c','y':'r','h':'f','n':'v','u':'t','j':'g','m':'b','i':'y','k':'h',',':'n','o':'u',
        'l':'j','.':'m','p':'i',';':'k','/':',','[':'o',"'":'l',']':'p','2':'`','3':'1','4':'2','5':'3','6':'4','7':'5','8':'6','9':'7','0':'8','-':'9','=':'0', '\\': '',
        ' ': ' '
    } 

while True:
    try:
        s = input()
        print(solve(s))
    except EOFError:
        break
# Accepted	PYTH3	0.050
```