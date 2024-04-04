---
title: 使用VSCode實現PHP server和Live Server的即時開發
date: 2024-04-04 19:55:43
tags:
  - PHP
  - 網站開發
categories:
  - 學習筆記
excerpt: 本篇文章將會使用之前所提到的 RAG 技術，實作一個可以返回資訊來源的 LLM
description: 最近在解決 LLM 回答問題準確度的問題，我找到了一項名為 RAG（Retrieval-Augmented Generation）的技術，這是一種旨在提升大型語言模型回答品質的方法。 RAG 通過先行檢索相關資料，然後基於這些資料生成回答，這種方式不僅可以增強了模型的回答能力，還提供了一種機制來追溯資訊源頭。
---

# 使用 VSCode 實現 PHP server 和 Live Server 的即時開發 (Windows系統)

在當今快速變化的技術領域中，開發工具的選擇可以大大影響我們的工作效率和產品質量。[Visual Studio Code（VSCode）](https://code.visualstudio.com/) 憑藉其輕量級設計、強大的功能和豐富的插件生態系統，已經成為全球開發者的首選之一。

[Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) 插件是一個非常受歡迎的 Visual Studio Code（VS Code）插件，它允許開發者快速在本地端電腦開啟一個開發用的伺服器，並且可以即時預覽他們的網頁應用。當你修改和保存文件時，這個插件會自動刷新瀏覽器，讓你立即看到更改的效果。 

由於 Live Server 插件本身不支援 PHP 文件，這就需要我們進行一些特別的設定和調整。在接下來的內容中，我們將一步一步探索如何在 Windows 系統利用 VSCode 運行 [PHP server](https://marketplace.visualstudio.com/items?itemName=brapifra.phpserver)，同時結合 Live Server 插件，來實現 PHP 即時更新功能。

#### 安裝插件
安裝 [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) 以及 [PHP server](https://marketplace.visualstudio.com/items?itemName=brapifra.phpserver)

#### 設置開發環境
安裝完後可以先創一個資料夾，並且對他點擊右鍵，選擇 `在終端開啟`，開啟後輸入 `code .` 來開啟 vsocde 的開發環境，我們可以先創建一個簡單的 `index.php` 來測試。

{% img source 512  '"開啟vsocde開發環境" "開啟vsocde開發環境"' %}

```php
<?php
    echo "Hello World!";
?>
```
*index.php*

如果 Live Server 有安裝成功，此時在 vscode 右下角應該會出現 `Go Live`，點擊他就可以開啟 Live server

{% img source 512  '"開啟 Live server" "開啟 Live server"' %}

開啟完後你應該會發現他把整個目錄的項目都列了出來，因為 Live Server 並不支援 PHP 網頁。

{% img source 512  '"開啟 Live server 後" "開啟 Live server 後"' %}

#### 設置 PHP Server
要實現在 VScode 開啟 PHP 網頁，我們可以用 PHP Server 這個套件，安裝完後應該可以在 vscode 右上角的 `PHP icon` 或者點擊右鍵選擇 `PHP Server: Open file in browser`，來開啟 PHP Server。

{% img source 512  '"4" "4"' %}

如果開啟後出現： **PHP not found**，代表你沒有設定 php.exe 的位置。
{% img source 512  '"5" "5"' %}

#### 設置 PHP 位置
你需要先在 [PHP 官方網站](https://windows.php.net/download#php-8.3) 下載 PHP，下載完解壓縮放在一個資料夾裡。
{% img source 512  '"6" "6"' %}

接下來在 vscode 裡面，點擊 **File->Preferences->settings**
{% img source 512  '"7" "7"' %}

然後在上方搜尋：**php**，在下方找到 `Phpserver: PHP Config Path` 以及 `Phpserver: PHP Path`的地方，在裡面輸入你剛才下載的 php 資料夾內的 `php.exe`路徑

{% img source 512  '"8" "8"' %}

設定完 php.exe 路徑後再次啟動 PHP Server 應該就可以成功啟用了！

{% img source 512  '"9" "9"' %}

#### 安裝 Chrome Live Server Web Extension
為了讓 Live Server 可以跟 PHP Server 連動，這時就要使用 [Live Server Web Extension](https://chromewebstore.google.com/detail/fiegdmejfepffgpnejdinekhfieaogmj) 這個 Chrome 插件，安裝完後應該可以在 Chrome 的插件選項找到並打開它，打開後我們會要設定三個選項：

1. Live Reload：打開它
2. Actual Server Address：使用 PHP Server 架設的伺服器位置，通常為 `http://localhost:3000/`
3. Live Server Address：Live Server 的位置，通常為 `http://localhost:5500/`
設定完後點擊 apply 來確認。

設定如下：
{% img source 512  '"10" "10"' %}

#### 實現 PHP 實時編輯自動更新功能！
最後將 PHP Server 以及 Live Server 都打開就可以實現用 VScode 實時編輯自動更新的功能！
開啟 PHP Server 可以先點右鍵，選擇 **PHP Server: Reload server** 選項，然後再選擇 **PHP Server: Open file in browser**。