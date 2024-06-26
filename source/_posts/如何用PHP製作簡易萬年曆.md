---
title: 如何用PHP製作簡易萬年曆
date: 2024-04-05 15:20:21
tags:
  - PHP
  - 網站開發
categories:
  - 學習筆記
toc: true
---
## 如何用PHP製作簡易萬年曆

最近因緣際會需要在網頁上製作一個萬年曆，最好是要可以超級簡單且非常好背的版本，最後決定直接用 PHP 做，想說順便分享一下我的作法。接下來本文將帶領大家使用PHP，來製作一個簡易的萬年曆！

## 效果圖
因為是“簡易”萬年曆，又要可以很簡單又容易記住，所以我就沒有太注重美觀細節，雖然長得很陽春，但該有的功能都有，重點是他很簡單很好記！ :3

[如果想要直接看完整程式碼可以點我](https://blog.iddle.dev/public/2024/04/05/%E5%A6%82%E4%BD%95%E7%94%A8PHP%E8%A3%BD%E4%BD%9C%E7%B0%A1%E6%98%93%E8%90%AC%E5%B9%B4%E6%9B%86/#%E5%AE%8C%E6%95%B4%E7%A8%8B%E5%BC%8F%E7%A2%BC)

{% img [class names] https://blog.iddle.dev/public/2024/04/05/%E5%A6%82%E4%BD%95%E7%94%A8PHP%E8%A3%BD%E4%BD%9C%E7%B0%A1%E6%98%93%E8%90%AC%E5%B9%B4%E6%9B%86/calendar.png 512  '"萬年曆" "萬年曆"' %}

## 環境準備
首先，你需要有一個能運行 PHP 的環境。這可以是任何安裝了 PHP 的伺服器，或者是像 XAMPP、 MAMP 這樣的本地開發環境。另外要確保你的PHP版本至少是 5.6 以上（當然，最好使用新版本）。

如果你不打算使用 XAMPP 之類的方式來架設 PHP 伺服器，你可以參考我之前的文章——[使用VSCode實現PHP server和Live Server的即時開發](https://blog.iddle.dev/public/2024/04/04/%E4%BD%BF%E7%94%A8VSCode%E5%AF%A6%E7%8F%BEPHP-server%E5%92%8CLive-Server%E7%9A%84%E5%8D%B3%E6%99%82%E9%96%8B%E7%99%BC/)，在本地端 VScode 架設一個可以即時刷新的 PHP 開發環境 :D

## 步驟一：設置時區
為了確保時間的準確性，首先要設置正確的時區。在這裏，我們將時區設定為'Asia/Taipei'，這樣可以確保萬年曆中顯示的時間是準確的。

```php
<?php
date_default_timezone_set('Asia/Taipei');
?>
```

## 步驟二：處理年份和月份
接下來，我們需要獲取當前的年份和月份，使用它來展示當前月份的日歷。這裡我使用 `GET` 的方式，如果 URL 中有提供，使用 URL 中的年份和月份；否則，默認使用當前的年份和月份。

```php
$year = isset($_GET['year']) ? $_GET['year'] : date('Y');
$month = isset($_GET['month']) ? $_GET['month'] : date('m');
```

## 步驟三：計算前後月份的年份和月份
為了能夠瀏覽前一個月和下一個月的萬年曆，我們需要計算前後月份的年份和月份。

這裡先簡單介紹一下這裡會用到的函式

### mktime 函式
`mktime()` 是 PHP 中的一個函式，用於**獲取 Unix 時間戳(timestamp)**。Unix 時間戳(timestamp)是指自1970年1月1日（即Unix紀元）以來的秒數。這個函式的一般格式如下：

```php
mktime(hour, minute, second, month, day, year)
```

### date 函式
date() 函式用於**格式化 Unix 時間戳(timestamp)**，將其轉換為可讀的日期和時間格式。其語法如下：

```php
date(format, timestamp)
```
format: 日期和時間的格式。
timestamp: **可填可不填**。指定的 Unix 時間戳(timestamp)。**如果省略，將使用當前的日期和時間**。

計算前後月份的年份和月份程式碼：
```php
$prevMonth = date('m', mktime(0, 0, 0, $month - 1, 1, $year));
$prevYear = date('Y', mktime(0, 0, 0, $month - 1, 1, $year));
$nextMonth = date('m', mktime(0, 0, 0, $month + 1, 1, $year));
$nextYear = date('Y', mktime(0, 0, 0, $month + 1, 1, $year));
```
這段程式碼首先利用 `mktime()` 函式生成特定日期的 Unix 時間戳(timestamp)。
對於前一個月（`$prevMonth` 和 `$prevYear`），它將當前月份減 1（` $month - 1`）。
對於下一個月（`$nextMonth` 和 `$nextYear`），則將當前月份加 1（` $month + 1`）。
日期（日）設定為 1，讓時間戳**指向目標月份的第一天。時、分、秒都設為 0**。

然後，`date()` 函式根據這些時間戳**格式化出具體的年份和月份**。
對於月份（`$prevMonth` 和 `$nextMonth`），格式設定為 `'m'`，這將返回格式化為兩位數的月份（01 到 12）。
對於年份（`$prevYear` 和 `$nextYear`），格式設定為 `'Y'`，這將返回四位數的年份（例如 2023）。

通過這種方式，即使當前是一月或十二月，計算也能正確跨年處理，因為 `mktime()` 會自動調整日期，讓他成為有效值。
例如，如果當前月份是 1 月，`$month - 1` 會變成 0，`mktime()` 會自動將其解讀為上一年的 12 月。同樣的，如果當前月份是 12 月，`$month + 1` 會變成 13，`mktime()` 會將其解讀為下一年的 1 月。這樣就實現了無縫的年份和月份跨越，而且非常非常的簡單好記 :D。

## 步驟四：初始化其他變數
現在，讓我們初始化萬年曆的初始變數。

```php
$firstDayOfMonth = mktime(0, 0, 0, $month, 1, $year);
$dayOfWeek = date('w', $firstDayOfMonth);
$daysInMonth = date('t', $firstDayOfMonth);
$today = date('Y-m-d');
```

簡單解釋一下：
**計算當月第一天的時間戳(timestamp)**：`$firstDayOfMonth = mktime(0, 0, 0, $month, 1, $year);`
我們先使用 `mktime()` 函式來生成 **當月第一天** 的 Unix 時間戳。這裡時、分、秒都設為了0，代表當天的開始時刻。`$month` 和 `$year` 分別代表當前的月份和年份，**日期設定為1**，即當月的第一天。

**獲取當月第一天是星期幾**：`$dayOfWeek = date('w', $firstDayOfMonth);`
使用了 `date()` 函式和 `'w'` 格式，根據 `$firstDayOfMonth` （即當月第一天的時間戳）來獲取當月第一天是星期幾。
在PHP中，`'w'` 格式返回的數字是**0（代表星期日）到6（代表星期六）之間**的一個數字，這樣你就可以知道當月第一天落在星期幾了。

**計算當月有多少天**: `$daysInMonth = date('t', $firstDayOfMonth);`
這裡同樣使用 `date()` 函式，但是格式字符串變成了 `'t'`。這會返回當月的天數，即**28到31之間**的數字。這樣，你就可以知道當月一共有多少天，進而知道如何在萬年曆中排列這些天數了。

**獲取今天的日期**: `$today = date('Y-m-d');`
最後，這行程式碼使用 `date()` 函式和 `'Y-m-d'` 格式來獲取今天的日期，格式是年-月-日（例如：2023-04-01）。取得今天日期的目的是要在萬年曆上面標註今日的日期，可以不用做。

## 步驟五：生成日歷的HTML結構
現在我們已經有了所有必要的數據，下一步就是將它們整合進HTML中，創建出日歷的外觀。

首先先做出選取上一個月以及下一個月的功能，主要是使用改變網址的方式來實現。
```html
<div class="calendar-nav">
    <a href="?year=<?php echo $prevYear; ?>&month=<?php echo $prevMonth; ?>">前一個月</a>
    <span><?php echo $year; ?>年 <?php echo $month; ?>月</span>
    <a href="?year=<?php echo $nextYear; ?>&month=<?php echo $nextMonth; ?>">下一個月</a>
</div>
```

接下來畫出萬年曆，我這裡是用 html 的表格來製作
首先要先將月份一開始填充空白，直到月分的第一天：
```php
// 填充空白
for ($i = 0; $i < $dayOfWeek; $i++) {
    echo "<td></td>";
}
```

再來根據這個月的天數，用迴圈生成每一天
我們可以使用 `if (($day + $dayOfWeek) % 7 == 0 || $day == $daysInMonth)`來判斷是否要換行
```php
// 輸出當月所有天數
for ($day = 1; $day <= $daysInMonth; $day++) {
    // 當達到一周的天數時，開始新的一行
    if (($day + $dayOfWeek) % 7 == 0 || $day == $daysInMonth) {
        $class = (date('Y-m-d', mktime(0, 0, 0, $month, $day, $year)) == $today) ? "today" : "";
        echo "<td class='{$class}'>{$day}</td></tr><tr>";
    } else {
        $class = (date('Y-m-d', mktime(0, 0, 0, $month, $day, $year)) == $today) ? "today" : "";
        echo "<td class='{$class}'>{$day}</td>";
    }
}
```

## 步驟六：加上 CSS
可以加上簡單的 CSS 讓他好看一點。
```html
<style>
table {
    width: 100%;
    border-collapse: collapse;
}
table, th, td {
    border: 1px solid black;
}
th, td {
    padding: 5px;
    text-align: center;
}
th {
    background-color: #f2f2f2;
}
.today {
    background-color: #ffff99;
}
</style>
```

## 完整程式碼
最後附上完整的程式碼
```php
<?php
date_default_timezone_set('Asia/Taipei');

// 檢查GET請求中是否有年份和月份參數，如果有則使用，否則使用當前年份和月份
$year = isset($_GET['year']) ? $_GET['year'] : date('Y');
$month = isset($_GET['month']) ? $_GET['month'] : date('m');

// 計算前一個月和下一個月的年份和月份
$prevMonth = date('m', mktime(0, 0, 0, $month - 1, 1, $year));
$prevYear = date('Y', mktime(0, 0, 0, $month - 1, 1, $year));
$nextMonth = date('m', mktime(0, 0, 0, $month + 1, 1, $year));
$nextYear = date('Y', mktime(0, 0, 0, $month + 1, 1, $year));

// 其他變數初始化...
$firstDayOfMonth = mktime(0, 0, 0, $month, 1, $year); // 取得這個月第一天的 timestamp
$dayOfWeek = date('w', $firstDayOfMonth); // 第一天是星期幾
$daysInMonth = date('t', $firstDayOfMonth); // 這個月總共有幾天
$today = date('Y-m-d'); // 今天是幾號
?>
<!DOCTYPE html>
<html>
<head>
    <title>萬年曆</title>
    <style>
        table {
            width: 100%;
            border-collapse: collapse;
        }
        table, th, td {
            border: 1px solid black;
        }
        th, td {
            padding: 5px;
            text-align: center;
        }
        th {
            background-color: #f2f2f2;
        }
        .today {
            background-color: #ffff99;
        }
    </style>
</head>
<body>
    <div style="width: 100%; text-align: center">
        <a href="?year=<?php echo $prevYear; ?>&month=<?php echo $prevMonth; ?>">前一個月</a>
        <!-- 顯示年份和月份 -->
        <span><?php echo $year; ?>年 <?php echo $month; ?>月</span>
        <a href="?year=<?php echo $nextYear; ?>&month=<?php echo $nextMonth; ?>">下一個月</a>
    </div>

    <table>
        <tr>
            <th>日</th>
            <th>一</th>
            <th>二</th>
            <th>三</th>
            <th>四</th>
            <th>五</th>
            <th>六</th>
        </tr>
        <tr>
            <?php
            // 填充空白天數
            for ($i = 0; $i < $dayOfWeek; $i++) {
                echo "<td></td>";
            }

            // 輸出當月所有天數
            for ($day = 1; $day <= $daysInMonth; $day++) {
                // 當達到一周的天數時，開始新的一行
                if (($day + $dayOfWeek) % 7 == 0 || $day == $daysInMonth) {
                    $class = (date('Y-m-d', mktime(0, 0, 0, $month, $day, $year)) == $today) ? "today" : "";
                    echo "<td class='{$class}'>{$day}</td></tr><tr>";
                } else {
                    $class = (date('Y-m-d', mktime(0, 0, 0, $month, $day, $year)) == $today) ? "today" : "";
                    echo "<td class='{$class}'>{$day}</td>";
                }
            }
            ?>
        </tr>
    </table>
</body>
</html>

```