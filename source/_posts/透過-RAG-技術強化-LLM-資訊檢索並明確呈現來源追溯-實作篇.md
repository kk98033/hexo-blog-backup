---
title: 透過 RAG 技術強化 LLM 資訊檢索並明確呈現來源追溯-實作篇
date: 2024-04-03 10:21:51
tags:
  - LLM
  - RAG
categories:
  - 學習筆記
excerpt: 本篇文章將會使用之前所提到的 RAG 技術，實作一個可以返回資訊來源的 LLM
description: 最近在解決 LLM 回答問題準確度的問題，我找到了一項名為 RAG（Retrieval-Augmented Generation）的技術，這是一種旨在提升大型語言模型回答品質的方法。 RAG 通過先行檢索相關資料，然後基於這些資料生成回答，這種方式不僅可以增強了模型的回答能力，還提供了一種機制來追溯資訊源頭。
toc: true
---

## 透過 RAG 技術強化 LLM 資訊檢索並明確呈現來源追溯-實作篇

本篇文章將會使用上一篇文章所提到的 [RAG](https://blog.iddle.dev/public/2024/04/02/%E9%80%8F%E9%81%8E-RAG-%E6%8A%80%E8%A1%93%E5%BC%B7%E5%8C%96-LLM-%E8%B3%87%E8%A8%8A%E6%AA%A2%E7%B4%A2%E4%B8%A6%E6%98%8E%E7%A2%BA%E5%91%88%E7%8F%BE%E4%BE%86%E6%BA%90%E8%BF%BD%E6%BA%AF/) 技術，實作一個可以返回資訊來源的 LLM

接下來我會參考這篇 [blog](https://zilliz.com/blog/retrieval-augmented-generation-with-citations) 的文章（文章裡面的一些語法跟目前的版本有所差異 -_-），在 Google Colab 使用 [Milvus](https://milvus.io/) 和 [LlamaIndex](https://docs.llamaindex.ai/en/stable/) 搭建一個基於大型語言模型（LLM）的檢索增強生成模型（RAG）查詢引擎。（不使用 Milvus 也沒關係，反而還會比較好做）

此專案會使用到：
1. Milvus：Milvus 是一個開源的向量數據庫，專為提供高效的相似性搜索和向量索引而設計。它能夠處理大規模的向量數據，適用於各種AI應用，如推薦系統、影像識別和自然語言處理等。不使用 Milvus 也沒關係，反而還會比較好做）
2. Milvus Lite：Milvus Lite 是 Milvus 的輕量版本，專為與 Google Colab 和 Jupyter Notebook 無縫配合而設計。
3. LlamaIndex：LlamaIndex 是一個為語言模型設計的索引和檢索框架，它提供了建立、管理和利用索引來增強語言模型檢索性能的工具。
4. OpenAI embeddings：我們使用他語義檢索技術來增強大型語言模型（LLM）的能力，以及利用其生成向量嵌入的能力來改善文檔檢索的精確度和相關性。

## 安裝相關套件
```shell
!pip install milvus # optional
!pip install python-dotenv
!pip install llama-index
!pip install llama-index-vector-stores-milvus # optional
```

## 設定 API KEY
{% note danger %}
注意！ 設定 OpenAI api key 環境變數要放在 import SimpleDirectoryReader 之前，不然之後會讀不到 api key，[參考 Github 上面的問題](https://github.com/run-llama/llama_index/issues/6920)
{% endnote %}

設定 API key，API key 記得放在 Google Colab 的 **Secrets** 裡，並且要把 **Notebook access** 打開。

{% img [class names] https://blog.iddle.dev/public/2024/04/03/%E9%80%8F%E9%81%8E-RAG-%E6%8A%80%E8%A1%93%E5%BC%B7%E5%8C%96-LLM-%E8%B3%87%E8%A8%8A%E6%AA%A2%E7%B4%A2%E4%B8%A6%E6%98%8E%E7%A2%BA%E5%91%88%E7%8F%BE%E4%BE%86%E6%BA%90%E8%BF%BD%E6%BA%AF-%E5%AF%A6%E4%BD%9C%E7%AF%87/key.png 256  '"API KEY" "API KEY"' %}



名字最好取 `OPENAI_API_KEY`

```python
import os
from google.colab import userdata

# 設定OpenAI API密鑰
os.environ["OPENAI_API_KEY"] = userdata.get('OPENAI_API_KEY')
```

## Import 相關套件
{% note info %}
記得要先設定 api key!!
{% endnote %}

```python
from llama_index.llms.openai import OpenAI
from llama_index.core.query_engine import CitationQueryEngine
from llama_index.core.indices.vector_store.base import VectorStoreIndex
from llama_index.core import SimpleDirectoryReader
from llama_index.core import StorageContext
# from llama_index.vector_stores.milvus import MilvusVectorStore
from milvus import default_server

from llama_index.core import Settings
from llama_index.embeddings.openai import OpenAIEmbedding
from llama_index.core.node_parser import SentenceSplitter
```

{% note info %}
如果你不打算使用 Milvus，這段可以跳過（說實在的，這段有點麻煩:| ）
{% endnote %}
這段另外 import，因為 import 時可能會出現問題

```python
from llama_index.vector_stores.milvus import MilvusVectorStore
```

如果你在 import 他時出現：

`ContextualVersionConflict: (grpcio 1.62.1 (/usr/local/lib/python3.10/dist-packages), Requirement.parse('grpcio<=1.60.0,>=1.49.1'), {'pymilvus'})
`

代表你現在的 grpcio 版本不兼容，需要使用以下指令安裝指定的版本

```shell
# 卸載 grpcio，然後安裝特定版本的 grpcio（1.60.0）安裝其他的版本可能會導致衝突
!pip uninstall -y grpcio
!pip install grpcio==1.60.0
```


卸載後可能會叫你 **Restart session**，按下 Restart session 後就可以了，重啟 session 後就可以成功 import 了！

## 從 PDF 文件中讀取文檔數據
先下載我們這次測試的 pdf 資料（可以是任意的資料），這裡提供我們這組為了專題整理的原住民資料來測試。

用 wget 下載他
```shell
!wget 'https://drive.google.com/uc?export=download&id=1kDwfFbMC3nM0K7OTXu88cczYBFm10pQT' -O 原住民資料.pdf
```

接下來我們使用 [SimpleDirectoryReader](https://docs.llamaindex.ai/en/stable/examples/data_connectors/simple_directory_reader_parallel/?h=simpledirectoryreader) 來讀取 pdf 數據。

SimpleDirectoryReader 可以從文件目錄中平行處理和讀取文檔數據。它支援從各種類型的文件中提取文本，包括PDF、Word文檔等，並將其轉換為可用於建立索引和進行檢索操作的格式。

```python
# 從 PDF 文件中讀取文檔數據。
documents = SimpleDirectoryReader(
    input_files=["./原住民資料.pdf"]
  ).load_data()
print(len(documents))
print(documents)
```

回傳結果：

```text
472
[Document(id_='242ddcd6-186e-43e1-92ff-c6364a930f97', embedding=None, metadata={'page_label...
```

{% note info %}
472 代表 pdf 總共有 472 頁
{% endnote %}

## 啟動 Milvus lite 服務
{% note info %}
如果你不打算使用 Milvus，這段可以跳過
{% endnote %}
在讀取資料後，我們可以啟用 [Milvus Lite](https://milvus.io/docs/milvus_lite.md) 的服務，Milvus Lite 是一種輕量級的向量數據庫，適用於不需要高性能生產環境的場景。

```python
# 啟動 Milvus lite 服務。
default_server.start()
vector_dimension = 768

vector_store = MilvusVectorStore(
    collection_name="citations",
    host="127.0.0.1",
    port=default_server.listen_port,
    dim=vector_dimension
)
```

{% note info %}
如果無法開啟伺服器，顯示 Timeout ，可能要把整個筆記本重開再試一次（這就是為什麼我不太想要用他的關係）
{% endnote %}

## 從文檔中創建向量存儲索引
接下來從文檔中創建向量存儲索引，
[VectorStoreIndex.from_documents(documents)](https://docs.llamaindex.ai/en/stable/examples/vector_stores/SimpleIndexDemoLlama-Local/?h=vectorstoreindex)  會將文檔集合作為輸入，並基於這些文檔創建一個向量索引，使得可以在這些文檔中進行高效的相似性搜索或其他類型的向量操作。

```python
# 從文檔中創建向量存儲索引。
index = VectorStoreIndex.from_documents(documents)
```

## 設置 LLaMA 模型與嵌入
處理完伺服器後就可以開始設置使用的模型，在這裡我們使用了 `GPT-3.5 Turbo` ， embedding 使用 `text-embedding-3-small`

[SentenceSplitter](https://docs.llamaindex.ai/en/stable/module_guides/loading/node_parsers/modules/?h=sentencesplitter#sentencesplitter)裡面的
**chunk_size** 應設定為可以包含完整句子或段落的大小，而不至於過大或過小，以保持文本的連貫性和完整性。**chunk_overlap** 應該設定成一個較小的數值，以允許相鄰分塊之間有一定的重疊，這有助於保持句子的完整性，並減少因分塊導致的資訊損失。這裡的分割大小可以自己試試看。

```python
from llama_index.embeddings.openai import OpenAIEmbedding
from llama_index.core.node_parser import SentenceSplitter
from llama_index.llms.openai import OpenAI
from llama_index.core import Settings

# 設置 LLaMA 模型、嵌入模型、節點解析器等。
Settings.llm = OpenAI(model="gpt-3.5-turbo")
Settings.embed_model = OpenAIEmbedding(model="text-embedding-3-small")
Settings.node_parser = SentenceSplitter(chunk_size=256, chunk_overlap=20)
Settings.num_output = 256
Settings.context_window = 3900
```

## 初始化的查詢引擎並且測試在模型中整合 RAG 技術進行增強的效果
設定完後我們就可以測試我們整合 RAG 技術後的效果了，首先我們要先創建一個搜索引擎 [CitationQueryEngine](https://docs.llamaindex.ai/en/stable/examples/query_engine/citation_query_engine/?h=citationqueryengine) ，這裡我們設定搜索的頂部 `K` 值為 3，意味著返回相似性最高的前三個結果。`citation_chunk_size` 被設定為 256

最後使用 `query_engine.query()` 來查找相關的資料。

`query_engine.query()` 會根據先前設定的 LLM、Embedding 和其他設定來整理資料並提供回應。當執行 query_engine.query("阿美族有什麼祭典") 時，系統會執行以下步驟：

1. 文本嵌入：首先，使用設定的 Embedding 模型（在這個例子中是 OpenAIEmbedding 模型）將查詢文本轉換成向量。這個嵌入向量會捕捉查詢文本的語義資訊。
2. 向量相似性比對：然後，系統會在向量存儲（在這個例子中是 Milvus 向量數據庫）中查找與查詢向量最相似的文檔向量。這一步是通過計算向量之間的相似度來完成的，常用的相似度度量包括餘弦相似度。
3. 資料整理與回應：一旦找到最相關的文檔向量，LLM 會根據這些文檔的內容和查詢文本來整理資料。這可能包括生成摘要、提取關鍵資訊或以其他有用的格式回應查詢。
4. 回應輸出：最後，系統會以適當的格式輸出回應，可能包括相關文檔的文本摘要、直接引用或其他形式的資訊聚合。



```python
# 初始化引擎，設置相似性搜索的參數。
query_engine = CitationQueryEngine.from_args(
    index,
    similarity_top_k=3,
    citation_chunk_size=256,
)
```

```python
# response = query_engine.query("魯凱一詞的來源是什麼")
response = query_engine.query("阿美族有什麼祭典")
print(response)
print('=================')
for source in response.source_nodes:
    print(source.node.get_text())
```

## 成果

```text
阿美族有豐年祭和漁撈祭等祭典活動[5]。 豐年祭是阿美族規模最盛大且熱鬧非凡的節慶，具有政治、軍事、經濟、教育、訓練等功能，被認為是一年中最神聖的祭典，也是族人命脈延續根源的重要活動[5]。漁撈祭則包含海祭和河祭，是在每年5、6月間祭祀海神或河川神的活動，族人藉此祈求出海平安或撈捕魚類滿載而歸[1]。
=================
Source 1:
年7 月開始，各部落按照稻米收成時間由南往北安排辦理，祭典時間為期1 ∼ 7 天之間。   豐年祭雖然以豐收為名，但內容包括豐收、謝神、聯誼、社交與年齡階級晉級儀式、軍事訓練驗收儀式等，是具經濟、宗教、社會、政治、文化等多性質的綜合活動。豐年祭的多面向活動，具有多元文化特質的意義，加上參與人數眾多，規模相當盛大。阿美族人即使移居都市，仍然持續辦理豐年祭典，傳承各項傳統文化與觀念，這個祭典也是新一代族人對部落文化認同的重要活動。 2. 漁撈祭 用傳統檳榔葉柄食盒烹煮魚 阿美族的漁撈祭包含海祭與河祭，是在每年5、6 月間祭祀海神或河川神的活動，族人藉此祈求出海平安或撈捕魚類滿載而歸。

Source 2:
漁撈祭典有不同的名稱，在海邊進行的海祭，北部阿美稱為mia’adis，海岸阿美族稱為misacepo’，馬蘭阿美族稱為mikesi’；撈捕淡水魚的河川祭，沿秀姑巒溪兩岸的阿美族人稱為komoris。   而都歷部落於民國70 年（1981） 中斷過海祭， 復於民國100 年（2011）時恢復此祭儀，更名為pafafuy。 傳統領袖帶領族人祭祖 漁撈祭具有敬老尊賢的意義，祭典由青年以魚蟹、米酒祭拜河神或海神後揭開序幕，接著由各年齡階層進入海河溪流中撈捕魚類。近午時分，青年將捕獲的魚類集中、烹煮，並按照年齡階級的輩份順序，將煮熟的漁獲送交長者、耆老品嚐，以表示老者優先，而年長者也會賞賜漁獲

Source 3:
祈求作物豐收，也祈使身體健康，如未婚女子在播種祭時盪鞦韆，便能在婚後早生子女，若是已婚而尚未生育者，則能迅速懷孕。因此既是祈福，又是娛樂。         第二天，由先生媽主持告祖祭儀，祭祀邵族的祖靈，祈求作物能順利成長，以及族人能平安健康。第三天，族人仍休息不工作，依舊飲酒為樂。直到第四天的清晨，族人才上山播種，當播種至一半時須由先生媽再主持一次祭祀儀式，以糯米糕作祭品，續求作物的成長及為族人祈福。至此，播種祭始告全部結束。 參考資料  原住民族委員會→原住民族簡介→邵族→祭典傳說：http://www.apc.gov.tw/portal/docList.html?CID=AC58C79198E1FD34&type=1EE2C9E1BA3440B2D0636733C6861689 阿美族總人口220,067，阿美族人群聚而居，部落規模大、人口多，祭典活動特別盛大，以每年的豐年祭典最具代表性。

Source 4:
阿美族族群簡介 阿美族人群聚而居，部落規模大、人口多，祭典活動特別盛大，以每年的豐年祭典最具代表性。 美麗的家園 阿美族自稱為「pangcah」（邦查），含有「人」、「同族人」的意思，臺東的阿美族人多數住在卑南族的北邊，被卑南族人稱為「Amis」，有北方人、北方民族的意思，後來受到學術界的採用與傳播，成為廣為人知的族群名稱。阿美族的起源神話中，有「創生神話」以及「發祥傳說」兩大類別系統；北部阿美族人傳說祖先是由神降生而來，南部阿美族人認為祖先是由石頭誕生而來。

Source 5:
達西烏拉彎‧畢馬(田哲益)          2001。《臺灣的原住民：阿美族》。臺北市：臺原。  阿美族豐年祭 Malalikit、Malikoda、Ilisin、Kiloma'an 
 豐年祭為阿美族規模最盛大且熱鬧非凡的節慶，它包含了政治、軍事、經濟、教育、訓練等功能，故豐年祭到現在仍在阿美族人心目中認為是一年中最神聖的祭典，也是族人命脈延續根源。         阿美族原為種小米的農業社會，以往每當小米收割之後，各部落分別舉行盛大的慶典活動，以感謝神靈的恩惠，此即為豐年祭的傳統起源。阿美族耆老認為小米的精靈是所有植物中最敏感的，也是最麻煩的農作物，它好似具有人性一樣，有靈眼、靈耳、靈覺，而且禁忌也多，人們一不小心隨時會招來禍患災難。老人回憶說：「在田裡收割小米是最辛苦的工作，不僅講話要小心，動作也不得粗暴，否則會招來禍患」。

Source 6:
「休息」、「完畢」、「回家」等言詞以及放屁、打人等動作都是小米精靈不喜歡的。小米時期的豐年祭形式以原始宗教儀式呈現，內容單純、嚴肅、隆重。         二十世紀初年，阿美族放棄了小米而改種水稻。對阿美族來說，小米和水稻是不同性質的農作物，小米具有激烈的精靈，對人們會明確的善賞惡罰，但水稻是良性溫和的植物，除非在特殊的情況下，不會隨便害人。老人回憶說：「耕作水稻心情比較輕鬆自如，男子犁田時可高聲獨唱，女子們除草時可合唱、談笑。」這些狀況在種小米時期是絕對不可以的。因為作物的改變，使得現今的豐年祭呈現較為熱鬧的場面。         豐年祭又因各地區不同而有不同的稱呼，如北部阿美族普遍稱Malalikit，東海岸阿美族則稱為Malikoda，中部阿美則稱為Ilisin。

```

## 04/19 Update：取得來源的資訊
（詳細程式碼可以看[更新後的版本（沒有用 milvus）](https://colab.research.google.com/drive/11oQ4tnOWZdIid_5jxPI3PvX6v0UMD8Qq?usp=sharing)）
最近在翻 llamindex 的 document 時，看到原來 `query_engine.query()` 回傳的 response 可以得到他的 **metadata**。接下來我會簡單演飾**如何取得資料參考來源的詳細資料**。

我們先再 query 一次，取得 response：
```python
response = query_engine.query("阿美族有什麼祭典，請詳細講解")
print(response)
```

```text
阿美族有豐年祭和漁撈祭兩個主要祭典。豐年祭是一項綜合活動，包括豐收、謝神、聯誼、社交、年齡階級晉級儀式、軍事訓練驗收等多方面內容，具有經濟、宗教、社會、政治、文化等多性質的意義，並且是具有多元文化特質的盛大活動[1]。漁撈祭則包含海祭和河祭兩種形式，是在每年5、6月間祭祀海神或河川神的活動，族人藉此祈求出海平安或撈捕魚類滿載而歸。漁撈祭具有敬老尊賢的意義，祭典由青年以魚蟹、米酒祭拜河神或海神後揭開序幕，並按照年齡階級的輩份順序將煮熟的漁獲送交長者、耆老品嚐，以表示老者優先，並賞賜漁獲給表現良好的年輕人，體現互助共享和重視長者等倫理觀念[2]。
```

根據 [官方 document](https://docs.llamaindex.ai/en/stable/api_reference/schema/?h=textnode#llama_index.core.schema.BaseNode)，我們可以在 node 裡面取得許多資訊：

例如我們可以使用 `get_metadata_str()` 來取得每個 source 的詳細資訊（出自於哪裡、第幾頁、路徑......等）

```python
for source in response.source_nodes:
    print(source.node.get_text())
    print(source.node.get_metadata_str())
    print(source.node.node_id)
    print()
```

```text
Source 1:
年7 月開始，各部落按照稻米收成時間由南往北安排辦理，祭典時間為期1 ∼ 7 天之間。   豐年祭雖然以豐收為名，但內容包括豐收、謝神、聯誼、社交與年齡階級晉級儀式、軍事訓練驗收儀式等，是具經濟、宗教、社會、政治、文化等多性質的綜合活動。豐年祭的多面向活動，具有多元文化特質的意義，加上參與人數眾多，規模相當盛大。阿美族人即使移居都市，仍然持續辦理豐年祭典，傳承各項傳統文化與觀念，這個祭典也是新一代族人對部落文化認同的重要活動。 2. 漁撈祭 用傳統檳榔葉柄食盒烹煮魚 阿美族的漁撈祭包含海祭與河祭，是在每年5、6 月間祭祀海神或河川神的活動，族人藉此祈求出海平安或撈捕魚類滿載而歸。

page_label: 71
file_name: 原住民資料.pdf
file_path: 原住民資料.pdf
file_type: application/pdf
file_size: 5073207
creation_date: 2024-04-19
last_modified_date: 2024-04-03
ef1d43c4-b74d-4e66-b884-d23e341aaf1c

...
```
但是 `get_metadata_str()` 回傳的是字串，如果我們只想要他的其中一些資訊（例如頁數）可以這樣做：

使用 `response.metadata` 來回傳此次回應的 metadata:
```python
print(response.metadata)
```
```text
{'ef1d43c4-b74d-4e66-b884-d23e341aaf1c': {'page_label': '71', 'file_name': '原住民資料.pdf', 'file_path': '原住民資料.pdf', 'file_type': 'application/pdf', 'file_size': 5073207, 'creation_date': '2024-04-19', 'last_modified_date': '2024-04-03'}, '43831d09-7987-440f-b06f-8b02b85768b3': {'page_label': '64', 'file_name': '原住民資料.pdf', 'file_path': '原住民資料.pdf', 'file_type': 'application/pdf', 'file_size': 5073207, 'creation_date': '2024-04-19', 'last_modified_date': '2024-04-03'}, 'e9321498-b697-47e7-88d8-33764283c5bb': {'page_label': '419', 'file_name': '原住民資料.pdf', 'file_path': '原住民資料.pdf', 'file_type': 'application/pdf', 'file_size': 5073207, 'creation_date': '2024-04-19', 'last_modified_date': '2024-04-03'}}
```
因為他是一個字典的形式，我們可以用 `json.dump` 來比較漂亮的輸出他：
```python
import json
print(json.dumps(response.metadata, indent=4, sort_keys=True, ensure_ascii=False))
```
```text
{
    "43831d09-7987-440f-b06f-8b02b85768b3": {
        "creation_date": "2024-04-19",
        "file_name": "原住民資料.pdf",
        "file_path": "原住民資料.pdf",
        "file_size": 5073207,
        "file_type": "application/pdf",
        "last_modified_date": "2024-04-03",
        "page_label": "64"
    },
    "e9321498-b697-47e7-88d8-33764283c5bb": {
        "creation_date": "2024-04-19",
        "file_name": "原住民資料.pdf",
        "file_path": "原住民資料.pdf",
        "file_size": 5073207,
        "file_type": "application/pdf",
        "last_modified_date": "2024-04-03",
        "page_label": "419"
    },
    "ef1d43c4-b74d-4e66-b884-d23e341aaf1c": {
        "creation_date": "2024-04-19",
        "file_name": "原住民資料.pdf",
        "file_path": "原住民資料.pdf",
        "file_size": 5073207,
        "file_type": "application/pdf",
        "last_modified_date": "2024-04-03",
        "page_label": "71"
    }
}
```
我們可以看到，他的 key 對應到了 node 的 id，所以我們可以這樣來取得每個 source 的詳細資訊：

```python
for source in response.source_nodes:
    sourceMetadata = response.metadata[source.node.node_id]
    print(json.dumps(sourceMetadata, indent=4, sort_keys=True, ensure_ascii=False))
    print()
```
```text
{
    "creation_date": "2024-04-19",
    "file_name": "原住民資料.pdf",
    "file_path": "原住民資料.pdf",
    "file_size": 5073207,
    "file_type": "application/pdf",
    "last_modified_date": "2024-04-03",
    "page_label": "71"
}

{
    "creation_date": "2024-04-19",
    "file_name": "原住民資料.pdf",
    "file_path": "原住民資料.pdf",
    "file_size": 5073207,
    "file_type": "application/pdf",
    "last_modified_date": "2024-04-03",
    "page_label": "71"
}

{
    "creation_date": "2024-04-19",
    "file_name": "原住民資料.pdf",
    "file_path": "原住民資料.pdf",
    "file_size": 5073207,
    "file_type": "application/pdf",
    "last_modified_date": "2024-04-03",
    "page_label": "64"
}

{
    "creation_date": "2024-04-19",
    "file_name": "原住民資料.pdf",
    "file_path": "原住民資料.pdf",
    "file_size": 5073207,
    "file_type": "application/pdf",
    "last_modified_date": "2024-04-03",
    "page_label": "64"
}

{
    "creation_date": "2024-04-19",
    "file_name": "原住民資料.pdf",
    "file_path": "原住民資料.pdf",
    "file_size": 5073207,
    "file_type": "application/pdf",
    "last_modified_date": "2024-04-03",
    "page_label": "419"
}

{
    "creation_date": "2024-04-19",
    "file_name": "原住民資料.pdf",
    "file_path": "原住民資料.pdf",
    "file_size": 5073207,
    "file_type": "application/pdf",
    "last_modified_date": "2024-04-03",
    "page_label": "419"
}
```


## 結論
在這篇文章中，我們探索了如何通過 RAG 技術強化 LLM 的資訊檢索能力並明確呈現來源追溯。這項技術剛好符合我最近的專題需求—— *要求 LLM 提供精確且可靠的答案* 。

不過，在目前實作中也遇到了一些問題，當查詢的資訊在資料庫中不存在時，模型似乎無法利用自身的知識庫來提供答案。目前我會先想辦法解決這一個問題，讓模型在缺乏外部資料支持時，能夠更好地運用其預訓練的知識，從而保持回答的品質和可靠性。

## 程式碼
此專案程式碼我都放在 [Google Colab](https://colab.research.google.com/drive/1XfHc2bFtITeB2w8jIC9z5JatfwMn_5fW?usp=sharing) 上，有興趣可以看看：Ｄ

## 04/19 Update
因為我覺得 Milvus 在這裡實在有點麻煩，每次執行都要跑很久，我乾脆就做了一個新的沒有 Milvus 的版本：[link](https://colab.research.google.com/drive/11oQ4tnOWZdIid_5jxPI3PvX6v0UMD8Qq?usp=sharing)

除此之外這個更新過後的版本有新增了 ***取得 source 來源於哪個 pdf 檔案*** 的功能，想要看完整程式碼也是可以參考更新後的版本 :D

最後的最後，對於被問到資料沒有的問題時無法使用模型自己的知識回答的問題已經解決了，只是最近期中考比較忙（加上六日要工讀QQ），可能過幾天後才會放上來。

如果有興趣想要先了解可以去參考這篇：[ReAct Agent with Query Engine (RAG) Tools
](https://docs.llamaindex.ai/en/stable/examples/agent/react_agent_with_query_engine/)

## 參考資料
[Retrieval Augmented Generation with Citations](https://zilliz.com/blog/retrieval-augmented-generation-with-citations)

[Introducing text and code embeddings](https://openai.com/blog/introducing-text-and-code-embeddings)

[Milvus](https://milvus.io/)

[LlamaIndex docs](https://docs.llamaindex.ai/en/stable/)