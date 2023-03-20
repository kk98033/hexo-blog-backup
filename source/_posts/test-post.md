---
title: test post
date: 2023-03-13 14:18:56
updated: 2023-3-15 12:41:00
description: Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

banner_img: https://blog.iddle.dev/public/2023/03/13/test-post/test_img.png
excerpt: this is jsut a test post, it will delete in 69 hours

Last updated: 2023-03-10
hidden: true
# tags: 
# - test
# - test tag
# categories:
# - cat1
# - test
---
## TEST PAGE
Lorem ipsum dolor sit amet, consectetur adipiscing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.

> test page

###
![](./test-post/test_img.png)
![](./test-post/test_img2.png)

{% note primary %}
...markdown content
{% endnote %}

## 功能介紹
---
- 每10秒隨機生成3種道具的其中一種
    1.讓球變大(思考表情emoji) 2. 讓平台變大(讚emoji) 3. 反射(神聖彗星反射力量卡片)
    - 平台會變大三秒，並且會有提示，當平台顏色變成紅色代表即將變回原來的長度
    - 接到反射道具時可以獲得一次的反射機會，按下滑鼠的左鍵即可把所有的球往上彈
- 每10秒會到達下一關 
- 每五關會多生成一顆球
- 關卡越後面球速會越快
- 每到下一關平台會縮小(他會先變紅再變回原來的顏色來提示你)，最小到150px
- 作弊模式
    - 無敵模式   
    - 自動遊玩模式
        - 按下'a'鍵電腦會自行操控平台接球，當電腦接不到球時(球低於平台)，此時如果之前有接到反射道具，電腦會自己使用反射道具的功能
## How to Play
---
移動滑鼠來移動你的平台來接球
如果沒接到就輸了
吃到除反射道具以外的道具會自動使用
| 按鍵 | 說明 |
| ------ | ------ |
| 滑鼠 | 移動平台 |
| 左鍵 | 使用反射道具 |
| ESC | 離開遊戲 |
| P | 暫停 |
| ENTER | 開始遊戲/繼續遊戲 |
| C | 開啟作弊無敵模式 |
| A | 讓電腦操控自動遊玩 |

###### test
{% codeblock _.compact http://underscorejs.org/#compact Underscore.js %}
_.compact([0, 1, false, 2, '', 3]);
=> [1, 2]
{% endcodeblock %}

{% img [class names] https://blog.iddle.dev/public/2023/03/13/test-post/test_img.png [256] [256] '"ai generated image" "alt text"' %}

{% youtube lJIrF4YjHfQ %}

{% post_link hello-world %}

## 1793A - codeforces
```python
def solve(a, b, n, m):
    ans = min(a * (m * (n // (m + 1))) + min(a, b) * (n % (m + 1)), b * n)
    return ans

T = int(input())
for t in range(T):
    a, b = list(map(int, input().split()))
    n, m = list(map(int, input().split()))

    print(solve(a, b, n, m))
```

```python
>>> import mypackage
```

```ruby
require 'redcarpet'
markdown = Redcarpet.new("Hello World!")
puts markdown.to_html
```