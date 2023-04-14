---
title: 如何在Google搜尋到我的網站？Google Search Console教學以及SEO優化
tags:
  - 網站開發
categories:
  - [心得筆記]
excerpt: 最近架了一個網站打算分享我平時的學習心得，但一直無法在Google上搜尋到我的文章，在經過各種嘗試過後才成功在Google上搜尋到我的文章。本篇篇文章會以我的經驗來分享如何推送自己的網站到Google以及如何讓自己的排名往前
description: 最近架了一個網站打算分享我平時的學習心得，但一直無法在Google上搜尋到我的文章，在經過各種嘗試過後才成功在Google上搜尋到我的文章。本篇篇文章會以我的經驗來分享如何推送自己的網站到Google以及如何讓自己的排名往前
---

## 前言
最近架了一個網站打算分享我平時的學習心得，但一直無法在 Google 上搜尋到我的文章，在經過各種嘗試過後才成功在 Google 上搜尋到我的文章。本篇篇文章會以我的經驗來分享如何推送自己的網站到 Google 以及如何讓自己的排名往前。

其實Google是會自動尋找可加入索引的網站，通常不須採取任何行動，只要在網站上張貼資訊，就能夠讓網頁顯示在 Google 的搜尋結果中。但為什麼我新架的網站無法在 Google 上搜尋到呢？


#### 為什麼無法在Google搜尋到我的網站？
根據 [Google搜尋中心](https://developers.google.com/search/docs/fundamentals/get-on-google?hl=zh-tw)的說法，很有可能是

 * 網路上沒有其他網站連結到您的網站。請確認您是否能讓別的網站連結到您的網站，但切勿透過付費給對方來建立連結，因為這種行為會被視為違反 Google 指南規定。
 * 您的網站剛上線運作，Google 還來不及進行檢索。Google 可能需要經過幾週才會發現新上線的網站，或注意到現有網站的任何變更。
 * 網站的設計讓 Google 難以有效檢索網站內容。如果您的網站是使用 HTML 之外的專業技術所建置，Google 可能會無法正確檢索網站內容。請務必在網站中納入文字內容，而不要只使用圖片或影片。
 * Google 在嘗試檢索您的網站時收到錯誤訊息。這個問題最常見的原因是您的網站設有登入頁面，或是因故禁止 Google 檢索。請確認您可以在無痕式視窗中存取網站。
 * Google 漏掉您的網站。雖然 Google 可檢索數十億個網頁，但有時還是難免會遺漏少數網站，尤其是規模較小的網站。請稍等一段時間，同時設法讓其他網站連結至您的網站。

大部分無法在 Google 搜尋到自己網站的原因大部分都是因為 **第二點** 的關係，我們可以直接在Chrome上使用`site://example.com`來看網站是否有在 Google 的搜索結果上，以我的主網站 `iddle.dev` 為例：
{% img [class names] site.png [512] [512] '"site://iddle.dev示範" "site://iddle.dev示範"' %}

此指令會列出所有已編入Google搜尋的所有網址


#### 如何將自己的網站推送到 Google 搜尋上
對於剛上線運作的網站，Google 可能不知道有這個網站存在，所以要等到 Google 的爬蟲爬到你的網站會需要很多的時間。
那麼要如何快速的讓 Google 知道你網站的存在呢？我們可以藉助 Google 出的工具：[Google Search Console](https://search.google.com/search-console/about)來手動要求 Google 的爬蟲來爬你的網站

###### Google Search Console 使用教學
在登入[Google Search Console](https://search.google.com/search-console/about)後，會出現以下的選項:
{% img [class names] GSC_example_1.png [512] [512] '"site://iddle.dev示範" "site://iddle.dev示範"' %}