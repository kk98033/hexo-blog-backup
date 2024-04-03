---
title: 透過 RAG 技術強化 LLM 資訊檢索並明確呈現來源追溯
date: 2024-04-02 23:58:40
tags:
  - LLM
  - RAG
categories:
  - 學習筆記
excerpt: 最近在解決 LLM 回答問題準確度的問題，我找到了一項名為 RAG（Retrieval-Augmented Generation）的技術，這是一種旨在提升大型語言模型回答品質的方法。 RAG 通過先行檢索相關資料，然後基於這些資料生成回答，這種方式不僅可以增強了模型的回答能力，還提供了一種機制來追溯資訊源頭。
description: 最近在解決 LLM 回答問題準確度的問題，我找到了一項名為 RAG（Retrieval-Augmented Generation）的技術，這是一種旨在提升大型語言模型回答品質的方法。 RAG 通過先行檢索相關資料，然後基於這些資料生成回答，這種方式不僅可以增強了模型的回答能力，還提供了一種機制來追溯資訊源頭。
---


# 透過 RAG 技術強化 LLM 資訊檢索並明確呈現來源追溯

最近在解決 LLM 回答問題準確度的問題，我找到了一項名為 **RAG（Retrieval-Augmented Generation）** 的技術，這是一種旨在提升大型語言模型回答品質的方法。 RAG 通過先行檢索相關資料，然後基於這些資料生成回答，這種方式不僅可以增強了模型的回答能力，還提供了一種機制來追溯資訊源頭。

## 什麼是RAG (Retrieval-Augmented Generation)？
RAG 是一種將資訊檢索（Retrieval）與文本生成（Generation）相結合，來強化大型語言模型（LLM）的技術。這種方法允許模型在生成回答之前，先從一個外部知識庫中檢索相關資訊，從而提供更加準確和豐富的回答​

## RAG的運作方式
RAG 通過將用戶查詢轉換成數學向量，並與知識庫中的數據進行比較來檢索最相關的信息。然後，這些檢索到的資訊被整合到一個新的 prompt 中，供 LLM 生成回答

簡單來說，當用戶提出問題時，RAG 首先從知識庫中搜尋與該問題相關的答案，然後將這些檢索到的資訊整合到一個新的 prompt 中，這個 prompt 包括了原始查詢和檢索到的信息，最後由 LLM 生成包含檢索信息的回答。這種方式類似於 **“開卷考試”** ，模型在回答問題時可以參考 “書本” 中的內容，而不是完全依賴於記憶中的事實

{% img [class names] https://blog.iddle.dev/public/2024/04/02/%E9%80%8F%E9%81%8E-RAG-%E6%8A%80%E8%A1%93%E5%BC%B7%E5%8C%96-LLM-%E8%B3%87%E8%A8%8A%E6%AA%A2%E7%B4%A2%E4%B8%A6%E6%98%8E%E7%A2%BA%E5%91%88%E7%8F%BE%E4%BE%86%E6%BA%90%E8%BF%BD%E6%BA%AF/rag.jpeg  512 '"RAG 的運作方式" "RAG 的運作方式"' %}

[圖片來源：AWS 檢索增強生成（RAG）](https://docs.aws.amazon.com/zh_tw/sagemaker/latest/dg/jumpstart-foundation-models-customize-rag.html)

## RAG有哪些優點？
1. 提高回答的準確性：通過引入當前和外部數據，RAG 增加了回答的 **相關性** 和 **時效性​**，可以有效避免模型亂回答的情況​。
2. 擴展知識基礎：RAG允許LLM訪問訓練數據之外的廣泛信息，豐富了回答的深度和廣度​ 
3. 降低了持續對模型進行昂貴的再訓練的需求，節約了計算和財務資源

## RAG 與 Fine-tuning 的比較
RAG與 Fine-tuning 相比，提供了一種有效整合外部知識來增強模型回答的方法。RAG 通過**檢索**和**整合**相關信息來擴展模型的知識庫，從而在不需要大量再訓練的情況下提高回答的準確性和豐富度。

而 Fine-tuning 則著重於通過特定數據集重新調整模型的參數來適應特定任務或內容，這可能需要大量的計算資源和數據。RAG 的方法更加動態，能夠適應新的信息和變化，減少了持續訓練的需求，從而降低成本。

## RAG 應用場景
在客戶服務聊天機器人的應用中，RAG 可以提供比 Fine-tuning 更優的解決方案。RAG 允許聊天機器人動態地從最新的政策文檔或知識庫中檢索信息來回答客戶問題，從而確保回答的準確性和最新性。相比之下，僅依靠 Fine-tuning 的模型可能只能依賴於其訓練數據中的信息，無法適應新的問題或政策變更，除非進行重新訓練，這可能既耗時又昂貴。


透過本文的探討，我們了解到 RAG 技術如何透過結合外部資訊檢索來提高 LLM 的回答品質和來源可追溯性。相較於傳統的 Fine-tuning 方法，RAG在處理動態、多變的資訊時展現了其優勢。剛好這對於我的專題—— *製作一個能提供準確答案並附上來源的LLM* ——來說，提供了寶貴的見解。接下來我會分享我是如何實現這一目標的，希望能對同樣對 AI 與機器學習感興趣的朋友們提供幫助。

## 參考資訊
[What Is Retrieval-Augmented Generation, aka RAG?
](https://blogs.nvidia.com/blog/what-is-retrieval-augmented-generation/)

[IBM Research What is retrieval-augmented generation?
](https://research.ibm.com/blog/retrieval-augmented-generation-RAG)

[Retrieval-Augmented Generation (RAG) Explained: Understanding Key Concepts](https://www.datastax.com/guides/what-is-retrieval-augmented-generation)