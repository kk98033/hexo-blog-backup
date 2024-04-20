---
title: 解決CORS策略封鎖問題:No 'Access-Control-Allow-Origin' header
date: 2024-04-07 18:45:16
tags:
  - PHP
  - 網站開發
categories:
  - 學習筆記
excerpt: 簡單紀錄如何解決 "No 'Access-Control-Allow-Origin' header" 的方法以及 apache .htaccess 設定。
description: 簡單紀錄如何解決 "No 'Access-Control-Allow-Origin' header" 的方法以及 apache .htaccess 設定。
toc: true
---

最近在開發 API 服務的時候，遇到了一個很討厭的問題：

```text
Access to XMLHttpRequest at 'https://xxx' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.
```

雖然在 php 開頭加一行就可以解決，不過這也讓我好奇 CORS policy 到底是什麼，所以稍微查了一下，想說在這裡順便記錄一下，以便未來忘記如何解決的我以及其他人參考 :D

{% note primary %}
這裡主要是介紹 php 以及 apache server 的解決方式！
{% endnote %}

## CORS 是什麼？
跨來源資源共享（CORS）是一種基於 **HTTP 標頭**的機制，它允許一個網域下的伺服器明確指出哪些其他網域的請求是被允許的。CORS 機制讓 Web 開發者有能力控制他們的網站或Web應用如何與其他網站的資源互動，這是通過一系列 HTTP 標頭來實現的。

## CORS 的工作原理
當發生跨來源請求時，瀏覽器會自動在 HTTP 請求中加上一些特殊的標頭。最常見的是 `Origin` 標頭，它指出該請求來自哪個源（即協議、域名和端口）。如果伺服器允許該跨來源請求，它會在響應中包含一個 `Access-Control-Allow-Origin` 標頭，指明哪些源是被允許的。

## 為什麼需要 CORS？
在 Web 的早期，由於安全考慮，瀏覽器實施了同源政策（Same-Origin Policy）。這項政策防止了一個來源（origin）的文檔或腳本與另一個來源的資源進行互動。雖然這項政策在很大程度上提高了Web安全性，但隨著Web應用變得越來越互動化和複雜，開發人員越來越需要從不同的來源請求資源，這時同源政策就成為了一個限制。

CORS 正是為了解決這個問題而被引入。它允許服務器指明哪些請求是被允許的，從而在不犧牲安全性的前提下，讓跨來源請求成為可能。這對於現代Web開發來說至關重要，因為它使得各種 Web 服務和 API 能夠安全地集成和互操作。

## CORS標頭的作用
**Access-Control-Allow-Origin**：這是最重要的CORS響應標頭，它指明了哪些來源可以訪問資源。如果這個標頭的值是請求中Origin標頭的值，或者是一個*，則請求會被允許。
**Access-Control-Allow-Methods**：指明了實際請求中允許使用的HTTP方法（例如GET, POST）。
**Access-Control-Allow-Headers**：在預檢請求（preflight request）的響應中使用，指明了實際請求中可以使用的HTTP標頭列表。
**Access-Control-Allow-Credentials**：指明當瀏覽器的憑證旗標（credentials flag）設置為true時，是否允許請求。這適用於cookies和HTTP認證。

對於某些類型的請求，瀏覽器會先發送一個預檢請求（preflight request），通過OPTIONS方法來檢查服務器是否允許實際的請求。這是一種安全機制，用來確保跨來源請求不會對服務器產生意外的影響。

## 如何解決 No 'Access-Control-Allow-Origin' header
根據剛才的介紹，出現這個問題可能這些原因：
- **伺服器配置不當**：如果伺服器沒有正確設置`Access-Control-Allow-Origin`標頭，當發生跨域請求時，瀏覽器將因為安全政策而阻止請求。
- **請求的使用方式**：有時候即使伺服器配置了 CORS，但是如果我們的請求方式或請求的資源類型不被允許，也會觸發 CORS 錯誤。

### PHP 檔案解決 CORS 問題
如果你只想要讓一個 PHP 檔案允許 CORS，只需要在他的最前面加上：
```php
header("Access-Control-Allow-Origin: *");
```
這個會允許所有網站的請求，如果只想允許特定網站的請求可以這樣：
```php
header("Access-Control-Allow-Origin: https://example.com");
```
這個只會允許 https://example.com 的請求。

但如果你有一堆的 PHP 以及檔案需要允許請求，你可以在 apache server 中直接設定 header，方法如下：

### Apache 伺服器解決 CORS 問題
Apache 伺服器可以通過設置適當的 HTTP 標頭來解決 CORS 問題。以下是具體的步驟和配置方法：

#### 1. 啟用headers模組
首先，確保 Apache 的 `mod_headers` 模組是啟用狀態。
可以透過以下指令啟用他：
```shell
a2enmod headers
```
{% note primary %}
啟用完後記得重啟 apache!!!
{% endnote %}

#### 2. 設置 .htaccess 或 Apache 設置文件
接下來，你需要在 Apache 的 **.htaccess** 文件或 Apache 的配置文件中設置 CORS 相關的 HTTP 標頭。

如果沒有 .htaccess，可以創建一個 .htaccess 放在你 api 的根目錄！記得要在 `apache2.conf`裡面設定 `AllowOverride All`來允許他！詳細設定可以 [參考這個網站](https://phoenixnap.com/kb/how-to-set-up-enable-htaccess-apache)

直接在 .htaccess 裡面加上下方的指令就可以了！

假設你只想允許來自 https://www.example.com 的請求訪問你的 API，你可以這樣設定：
```shell
Header set Access-Control-Allow-Origin "https://www.example.com"
```

假設你想允許所有的網站請求可以這樣設定：
```shell
Header set Access-Control-Allow-Origin "*"
```

## 結論
通過適當配置 Apache 伺服器來處理 CORS 問題，可以讓你的 Web 應用更安全、更可靠。記得按照最佳實踐來設置 CORS 政策，確保你的應用在開放性和安全性之間保持平衡 :D！