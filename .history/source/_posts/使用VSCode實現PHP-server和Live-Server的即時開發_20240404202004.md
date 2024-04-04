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

## 前置作業
#### 安裝插件
安裝 [Live Server](https://marketplace.visualstudio.com/items?itemName=ritwickdey.LiveServer) 以及 [PHP server](https://marketplace.visualstudio.com/items?itemName=brapifra.phpserver)

#### 設置開發環境
安裝完後可以先創一個資料夾，並且對他點擊右鍵，選擇 `在終端開啟`，開啟後輸入 `code .` 來開啟 vsocde 的開發環境，我們可以先創建一個 `index.php` 來測試。

{% img source 512  '"開啟vsocde開發環境" "開啟vsocde開發環境"' %}

```php
<?php
    echo "Hello World!";
?>
```
*index.php*

如果 Live Server 有安裝成功，此時在 vscode 右下角應該會出現 `Go Live`，點擊他就可以開啟 Live server

{% img source 512  '"開啟 Live server" "開啟 Live server"' %}

{% img source 512  '"開啟 Live server 後" "開啟 Live server 後"' %}
