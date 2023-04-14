---
title: 如何在Google搜尋到我的網站？Google Search Console教學以及SEO優化
tags:
  - 網站開發
categories:
  - [心得筆記]
excerpt: 最近架了一個網站打算分享我平時的學習心得，但一直無法在Google上搜尋到我的文章，在經過各種嘗試過後才成功在Google上搜尋到我的文章。本篇篇文章會以我的經驗來分享如何推送自己的網站到Google以及如何讓自己的排名往前
description: 最近架了一個網站打算分享我平時的學習心得，但一直無法在Google上搜尋到我的文章，在經過各種嘗試過後才成功在Google上搜尋到我的文章。本篇篇文章會以我的經驗來分享如何推送自己的網站到Google以及如何讓自己的排名往前
---

#### 前言
最近架了一個網站打算分享我平時的學習心得，但一直無法在 Google 上搜尋到我的文章，在經過各種嘗試過後才成功在 Google 上搜尋到我的文章。本篇篇文章會以我的經驗來分享如何推送自己的網站到 Google 以及如何讓自己的排名往前。

其實Google是會自動尋找可加入索引的網站，通常不須採取任何行動，只要在網站上張貼資訊，就能夠讓網頁顯示在 Google 的搜尋結果中。但為什麼我新架的網站無法在 Google 上搜尋到呢？


#### 為什麼無法在Google搜尋到我的網站？
根據 [Google搜尋中心](https://developers.google.com/search/docs/fundamentals/get-on-google?hl=zh-tw)的說法，很有可能是

 * 網路上沒有其他網站連結到您的網站。請確認您是否能讓別的網站連結到您的網站，但切勿透過付費給對方來建立連結，因為這種行為會被視為違反 Google 指南規定。
 * 您的網站剛上線運作，Google 還來不及進行檢索。Google 可能需要經過幾週才會發現新上線的網站，或注意到現有網站的任何變更。
 * 網站的設計讓 Google 難以有效檢索網站內容。如果您的網站是使用 HTML 之外的專業技術所建置，Google 可能會無法正確檢索網站內容。請務必在網站中納入文字內容，而不要只使用圖片或影片。
 * Google 在嘗試檢索您的網站時收到錯誤訊息。這個問題最常見的原因是您的網站設有登入頁面，或是因故禁止 Google 檢索。請確認您可以在無痕式視窗中存取網站。
 * Google 漏掉您的網站。雖然 Google 可檢索數十億個網頁，但有時還是難免會遺漏少數網站，尤其是規模較小的網站。請稍等一段時間，同時設法讓其他網站連結至您的網站。

大部分無法在 Google 搜尋到自己網站的原因大部分都是因為 **第二點** 的關係，我們可以直接在Chrome上使用`site:example.com`來看網站是否有在 Google 的搜索結果上，以我的主網站 `iddle.dev` 為例：
{% img [class names] https://blog.iddle.dev/public/2023/04/09/%E5%A6%82%E4%BD%95%E5%9C%A8Google%E6%90%9C%E5%B0%8B%E5%88%B0%E6%88%91%E7%9A%84%E7%B6%B2%E7%AB%99%EF%BC%9FGoogle-Search-Console%E6%95%99%E5%AD%B8%E4%BB%A5%E5%8F%8ASEO%E5%84%AA%E5%8C%96/site.png [512] [512] '"site:iddle.dev示範" "site:iddle.dev示範"' %}
![site:iddle.dev示範](https://blog.iddle.dev/public/2023/04/09/%E5%A6%82%E4%BD%95%E5%9C%A8Google%E6%90%9C%E5%B0%8B%E5%88%B0%E6%88%91%E7%9A%84%E7%B6%B2%E7%AB%99%EF%BC%9FGoogle-Search-Console%E6%95%99%E5%AD%B8%E4%BB%A5%E5%8F%8ASEO%E5%84%AA%E5%8C%96/site.png)

{% note primary %}
site: 運算子不一定會傳回在查詢中所指定前置字元下建立索引的所有網址。
{% endnote %}

#### 如何將自己的網站推送到 Google 搜尋上
對於剛上線運作的網站，Google 可能不知道有這個網站存在，所以要等到 Google 的爬蟲爬到你的網站會需要很多的時間。
那麼要如何快速的讓 Google 知道你網站的存在呢？

1. 使用 [Google Search Console](https://search.google.com/search-console/about) 來手動要求 Google 爬蟲來爬你的網站
2. 為你的網站建立 **sitemap**

接下來我會先介紹如何使用 **Google Search Console**

#### Google Search Console 使用教學
在登入[Google Search Console](https://search.google.com/search-console/about)後，會出現以下的選項:
{% img [class names] GSC_example_1.png [512] [512] '"登入Google Search Console後" "登入Google Search Console後"' %}

你可以選擇使用 **網域** 或者是 **網址前置字元**
1. 使用網域：可以查看此網域下的所有資料 ex: example.com
2. 網址前置字元：只查看子網域的資料 ex: https://www.example.com, https://blog.example.com ... 等

我這裡以我的blog為例，所以使用**網址前置字元**。
輸入我blog的網址 https://blog.iddle.dev/public/ ，點繼續之後會出現驗證所有權的頁面：
{% img [class names] GSC_example_2.png [512] [512] '"驗證擁有權" "驗證擁有權"' %}

可以看到有許多可以驗證網站所有權的方式，我這裡示範的是他建議的驗證方法（使用網域和網址前置字元驗證方法都是一樣的）
1. 下載上面的html檔案
2. 上傳到你網站的子目錄(就是把檔案放在你輸入的網址的目錄)
   
上傳完成後按驗證應該會看到 **已驗證所有權** 的提示，按前往資源就可以開始使用 Google Search Console 了！
{% img [class names] GSC_example_3.png [512] [512] '"已驗證擁有權" "已驗證擁有權"' %}

你的頁面應該會長這樣：
{% img [class names] GSC_example_4.png [512] [512] '"Google Search Console 頁面" "Google Search Console 頁面"' %}

我們可以看到左邊有許多的工具，我們可以簡單地查看你的網頁是否已經編入 Google 的索引裡。

首先點擊 **產生索引** 區域的 **網頁** ，此頁面會列出已建立索引的連結的數量
{% img [class names] GSC_example_5.png [512] [512] '"所有已建立索引的連結" "所有已建立索引的連結"' %}

在下方有個 **查看已建立索引網頁的相關資料** ，點進去後可以看到所有已建立索引的連結，這些連結都是可以在 Google 上搜索到的網頁
{% img [class names] GSC_example_6.png [512] [512] '"所有已建立索引的連結" "所有已建立索引的連結"' %}

接下來我會介紹如何手動推送網頁到 Google 上

###### 手動推送網頁
對於新上線的網頁， Google 並不會馬上知道這個網頁，要快速的讓 Google 知道這個網頁，我們可以使用 Google Search Console 來手動推送網頁。

首先點擊 **網址審查** ，輸入你要編入索引的網址
{% img [class names] GSC_example_7.png [512] [512] '"網址審查" "網址審查"' %}

按下enter後， Google 會查看這個網址是否有被編入索引，如果沒有，會出現像這樣的頁面：
{% img [class names] GSC_example_8.png [512] [512] '"網址審查" "網址審查"' %}

我們可以看到下方的 **發現方式** ，有 **sitemap** 和 **參照網頁** 部分，這兩項是 Google 爬蟲如何找到你網頁的方式，我們也可以使用 **sitemap** 讓 Google 爬蟲自動來找到你的網頁，不過這可能需要一點時間，關於sitemap，等等我也會一起介紹

你可以按下 **要求建立索引** 來手動要求 Google 幫你的網頁建立索引，按下後等 Google 測試線上網址是否可以編入索引之後就成功要求建立索引了！
{% img [class names] GSC_example_9.png [512] [512] '"建立索引" "建立索引"' %}

此時 Google 就會把你的網址排入建立索引的隊列，大概過個最多兩三天就可以在 Google 上搜索到你的網頁了！

你可以用上方提到的`site: + 網址連結` 來檢查 Google 是否已經檢索到你的網站

###### 使用sitemap
上方我有提到 **sitemap** 這個東西也可以幫助 Google 找到你的網頁，那麼 sitemap 是什麼呢？

Sitemap 是一中用來提供網站資訊的檔案，你可以在上方列出你網站上的所有網頁，方便 Google 等搜尋引擎找到你的網頁。
使用 Sitemap 的好處是**可以一次處理大量的網址**，所以當你的網址數量龐大，可以考慮使用 Sitemap，不過我個人是兩種方式都用。

Sitemap 對於剛建立的網站很有效，因為 Googlebot 和其他網路檢索器是透過網頁層層連結的方式來檢索網頁，因此，如果沒有其他網站連結到你的網頁，Googlebot 可能就無法找到你的網頁。

關於 Sitemap 的語法，可以參考 [Google 搜尋中心的文章](https://developers.google.com/search/docs/crawling-indexing/sitemaps/build-sitemap?hl=zh-tw) 的教學，我這裡就不介紹了，有興趣可以進去看看。

與其自己寫 sitemap，網路上其實有很多可以自動生成 sitemap 的服務，我就先介紹一個 sitemap 生成器 [Free XML Sitemap](https://www.mysitemapgenerator.com/) 給大家，用法很簡單，打上你網站的根目錄網址，選擇**XML Sitemap**(預設的)，再點擊 **GO TO CREATION**即可，接下來按下 **START** ，就會開始爬你的網站，爬完後點擊下載。

你應該會下載下來一個 **.xml** 檔案，內容大概會長這樣
{% img [class names] GSC_example_10.png [512] [512] '"sitemap" "sitemap"' %}

把 sitemap 下載下來後，將他放在你伺服器的根目錄 * ex: https://www.example.com/sitemap.xml *，確定打上網址後可以在你瀏覽器上看到sitemap後就可以去到 Google Search Console 的 **Sitemap** 欄位提交你的sitemap了
{% img [class names] GSC_example_11.png [512] [512] '"sitemap" "sitemap"' %}

{% note primary %}
通常新架設的網站就算加了sitemap或者是手動添加還是不一定會被馬上找到，可能還是需要一兩天才能讓 Google 檢索到
{% endnote %}


#### SEO 優化教學
SEO 全名為 **Search Engine Optimization (搜索引擎最佳化)**，正如他的名字所說，就是讓搜索引擎更容易發現網站並且幫助你的網站排序提升的程序。

在優化前記得先用`site:`指令來確定你的網站可以出現在 Google 搜索結果中

接下來我會大致的介紹如何優化自己網站的 SEO，讓你的網站排名提升

###### 使用 robots.txt 檔案禁止特定網頁被搜索引擎檢索
在前面的教學裡，我有介紹 Sitemap 是用來告訴搜索引擎網站上有哪些網頁。
而 robots.txt 就是告訴搜索引擎不要檢索這個網址，如果你有網頁不想要出現在搜索結果中，就可以使用 robots.txt 來避免被搜索到。
詳細的 rotots.txt 教學可以參考 Google 的 [robots.txt 簡介](https://developers.google.com/search/docs/crawling-indexing/robots/intro?hl=zh-tw)。

###### 為每個網頁建立獨特的標題以及描述
標題是每個網頁很重要的一部分，他可以幫助使用者以及搜索引擎快速的了解此網頁的主題是什麼，而在搜索結果中，系統大部分都會以網頁中的`<title>`元素作為搜尋結果的標題連結，因此幫每個網頁建立**明確的**、**可以吸引人的**`<title>`元素是很重要的

除了標題，**網頁摘要**也很重要，他**可能會**出現在搜索結果中的標題下方(我說的 **“可能會”** 是因為如果網頁中有文字非常符合使用者的查詢，Google 可能會選擇使用這段文字做為摘要。)，可以幫助使用者了解網站內容，所以一定要為每個網頁提供準確的內容摘要。

關於語法，可以參考下方程式碼：
```html
<head>
    <title>網頁標題</title>
    <meta name="description" content="網頁摘要">
</head>
```

範例：
```html
<head>
    <meta 
      name="description" 
      content="這題跟UVa 10041 - Vito&#39;s Family很類似，也都是要找 中位數 ，只不過這題需要把所有符合的中位數給找出來。 - Python UVa 10057 - A mid-summer night&#39;s dream. 解題心得">
    <title>Python UVa 10057 - A mid-summer night's dream.</title>
</head>
```

Google 搜索結果：
{% img [class names] GSC_example_12.png '"標題以及摘要" "標題以及摘要"' %}

###### 打造富有吸引力的網站
想要提升網站的排名最重要的還是為你的網站提供優質的內容，以及獨特吸引人的標題，並且了解自己網站的目標受眾的需求，只要能吸引越多使用者點進去你的網站，你的網站在搜索結果排名也會自然的提升

###### 善用連結
適當的使用連結可以幫助 Google 和使用者導覽到你的網頁，因此，為你的每個連結都加上明確的文字是很重要的。
另外，當你的網站連結被越多人分享到其他網站， Google 就會認為你的網站具有一定的可信度，這也會一定的影響到你網站的排名。

因此我們常常會在部落格上看到有個部分叫做 **友站連結** ，就是利用這點來互相宣傳對方的網站

當你的網站上有連結會連結到其他的網站，可能會使連結到的網站也獲得利益，為了避免有人在留言板等地方留上他自己的網站連結，好提升他的網站排名，我們可以使用`nofollow`來避免 Google 追蹤那個連結
```html
<a href="https://www.example.com" rel="nofollow">不要讓 Google 追蹤的連結</a>
```

最後，如果想為網頁中所有的連結加上 nofollow 屬性，可以在網頁的 `<head>` 標記中加入 
```html
<meta name="robots" content="nofollow">
```

###### 新增結構化資料標記
在網頁中加入結構料資料，可以使你的網頁在搜索結果中以更加豐富的樣式呈現出來
結構化資料標記支援了許多格式，建議使用 **JSON-LD** 的格式

舉例來說，我想要找蛋糕的食譜，第一個出現的網頁是這個：
{% img [class names] GSC_example_13.png '"蛋糕食譜" "蛋糕食譜"' %}

我們可以看到他上方有各種的資訊，像是評分星星數、票數、分鐘，這些都是使用結構化資料標記的效果

結構化資料標記大部分都會以以下形式出現在`<head>`中
```html
<script type="application/ld+json">
  ...
</script>
```

這是剛才搜索蛋糕食譜出現第一個網址的html，我們可以看見他在head中就有使用了結構化資料標記，在裡面還可以找到搜尋結果數的評分星星數以及投票人數。
{% img [class names] GSC_example_14.png '"蛋糕食譜" "蛋糕食譜"' %}

**那麼該如何使用結構化資料標記呢？**
結構化資料標記有許多的標記可以使用，可以參考 [Google 的所有結構化資料標記](https://developers.google.com/search/docs/appearance/structured-data/search-gallery?hl=zh-tw) 來為你的網頁加上最適合的標記。

如果想要測試你寫的標記是否有效，可以用 Google 出的 [複合式搜尋結果測試工具](https://search.google.com/test/rich-results?hl=zh-tw) 來測試網頁是否成功支援結構化標記。

我已剛才蛋糕食譜網站為例，他就為他的網頁加了一堆結構化資料標記
{% img [class names] GSC_example_15.png '"蛋糕食譜網頁測試" "蛋糕食譜網頁測試"' %}

###### 結構化標記-資料螢光筆標記
如果你想使用結構化標記，但不想手動加上程式碼，可以利用 Google 的 [資料螢光筆](https://www.google.com/webmasters/data-highlighter/?hl=zh-tw) ，只要 **選取一個具有代表性的網頁** ，點擊 **開始標記** ，就可以手動的標記你的網頁了(如果你的網站不是自動生成且每篇文章架構具有相似性的網站記得選 **只標記這個網頁**)。

接下來只需把你要標記的部分選取起來就可以標記了
{% img [class names] GSC_example_16.png '"標記網頁" "標記網頁"' %}

如果系統找到相似的網頁會把他列出來，再來點擊 **建立網頁集**
{% img [class names] GSC_example_17.png '"標記網頁" "標記網頁"' %}

最後檢查系統自動幫你選取的標題、發布日期、類別後就可以發布了！
{% img [class names] GSC_example_18.png '"標記網頁" "標記網頁"' %}

###### 讓網站適合透過行動裝置瀏覽
現在大部分人都是使用行動裝置在瀏覽網頁，因此對於行動裝置的適配性是非常重要的一件事，**強烈建議為你的網頁設計成響應式網頁(RWD)**

可以使用 Google 提供的工具 [行動裝置相容性測試](https://search.google.com/test/mobile-friendly?hl=zh-tw) 來測試你的網頁是否適合在行動裝置上瀏覽。

這大概是我分享的全部內容了，如果你對於 SEO 優化或是 Google 搜尋有興趣的可以參考 [Google 搜尋中心](https://developers.google.com/search/docs?hl=zh-tw) 的文章。

對於文章有任何問題要問歡迎在下方留言問我！