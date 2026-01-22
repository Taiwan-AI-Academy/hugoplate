---
title: "RAG 不只要找對答案還要找對「地方」：UniversalRAG"
description: "UniversalRAG 透過路由分流機制，解決多模態檢索中的模態落差與偏食問題。"
date: 2026-01-22T10:57:34+08:00
image: "/images/universalrag-concept.png"
categories: ["AI", "RAG"]
tags: ["UniversalRAG", "多模態", "檢索", "Router", "模態落差"]
author: "UniversalRAG"
draft: false
slug: "universalrag-find-the-right-place"
---

大家對 RAG（檢索增強生成）應該不陌生，現在的痛點在於資料不只有純文字還有圖片、表格跟影片。
目前主流的 RAG 做法，通常是把所有類型的資料全部轉成 Embedding，丟進向量資料庫。
但把不同格式的資料硬湊在同一個空間搜尋，其實效果並不好，很容易出現偏食的狀況。

![UniversalRAG 概念圖](/images/universalrag-concept.png)

這篇研究提出了一個叫做「模態落差（Modality Gap）」的問題。
簡單來說，因為使用者輸入通常是文字，如果在一個含有圖、文、影的向量空間裡搜尋，檢索系統會傾向只抓回文字類型的資料，卻忽略掉其實更相關的圖片或影片。

為了解決這個問題，UniversalRAG 的核心概念是改用「分流」策略。
與其把所有資料壓在同一個空間，不如保留各自的資料庫（Text Corpus, Image Corpus, Video Corpus）。
在檢索前，先透過一個「Router（路由器）」來判斷這個問題到底需要什麼類型的資料。

有趣的是，他們不只分「模態（Modality）」，還分「粒度（Granularity）」。
例如文檔按段落切分，視頻按短片切段，圖像則保持完整。系統可以根據查詢需求動態選擇最相關的知識來源。

研究團隊設計了包含 7 種不同路徑的路由機制（包含文字段落、整份文件、表格、圖片、短影片片段、長影片等），
並使用了 10 個涵蓋不同模態的基準資料集來測試。

在 Router 的實作上，他們測試了兩種方法：
1. Training-based：用像是 Qwen 或 InternVL 這類小一點的模型，專門訓練它來做分類。
2. Training-free：直接寫 Prompt 讓 GPT-5 或 Qwen-VL 這類大模型憑「常識」判斷該去哪裡找資料。

接著將這種分流檢索的結果，與傳統「Unified Embedding」的方法（如 UniRAG, GME 等）進行 PK。

實驗結果顯示，把資料全混在一起的 Unified 方法，在多模態檢索上確實存在嚴重的偏差。
反觀 UniversalRAG 這種「先分類、再搜尋」的策略，在各項指標上都取得了領先。

原本以為多加一個 Router 會變慢，結果因為系統不需要每次都在海量混合資料裡大海撈針，
而是精準地去特定的小資料庫撈資料，隨著資料量變大，UniversalRAG 的檢索速度反而比傳統方法更快。

這說明多模態 RAG，不在於如何把所有向量壓得更像，而在於如何讓系統懂得「挑對工具做對事」。

論文網址: https://universalrag.github.io/
