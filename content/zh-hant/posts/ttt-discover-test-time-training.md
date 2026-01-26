---
title: "誰說 AI 訓練完就定型了？這篇論文讓模型「邊考邊學」直接刷榜"
description: "TTT-Discover 讓模型在推理時更新權重，用更像人類試錯的方式把算力集中在最有希望的路徑，並在多個 discovery 任務刷出 SOTA。"
date: 2026-01-26T15:33:45+08:00
image: "/images/TTT-Discover.png"
categories: ["AI", "研究"]
tags:
  [
    "TTT-Discover",
    "Test-Time Training",
    "邊考邊學",
    "強化學習",
    "PUCT",
    "Entropic Objective",
    "Stanford",
    "NVIDIA",
  ]
author: "TTT-Discover"
draft: false
slug: "ttt-discover-test-time-training"
---

以前大家總覺得 LLM 訓練完就是「唯讀」狀態，要解難題只能靠 Prompt Engineering，或是讓模型多想一下。
但 Stanford 和 NVIDIA 合作的 TTT-Discover 直接打破這個框架：人類遇到難題會試錯、學習、再嘗試；那為什麼 AI 在解題時不能繼續訓練？

這項研究的核心假設其實很反直覺：做 discovery 跟一般的 AI 任務不一樣，不需要模型什麼都會，也不用管平均表現好不好。
目標非常簡單：只要找到那一個破紀錄的極端值就贏了。

換句話說，就像考試允許模型把其他通識課的內容全忘光，只要這題能解出來就好。
既然不需要考量未來的通用性，那就可以大膽地在推理階段對模型進行強化學習，把所有算力都砸在最有希望的路徑上，而不是盲目地隨機搜索。

## 怎麼做：生成、評分，真的去更新權重

實驗團隊使用 `gpt-oss-120b`：拿到題目後，讓模型生成解法、評分，然後真的更新模型的權重。

為了適應這種「一次性」的學習需求，團隊設計了：

- 特殊的目標函數（Entropic Objective）：專門獎勵那些能突破現狀的嘗試，而不是求穩。
- 搜索策略（PUCT）：把探索資源集中在更有希望產生突破的路徑上。

重點是這樣跑一次的成本極低：解決一個問題大概只需要幾百美金。
跟動輒幾百萬的預訓練比起來，根本是零頭。

## 成效：多領域直接刷出 SOTA

這種「臨陣磨槍」的策略效果非常好，TTT-Discover 在多個領域刷出 State-of-the-art（SOTA）：

- 數學：在著名的 Erdos 最小重疊問題上，找到了比之前所有人類和 AI 都更強的解法。
- 工程：在 GPU Kernel 優化比賽中，寫出的程式碼運行速度比人類專家更快，甚至比之前的 AI 方法快了兩倍。
- 演算法：在 AtCoder 競賽題庫中，表現也超越了現有的開源 AI 基準。

證明了：與其期待模型帶著全世界的知識去通靈，不如給它一點時間和空間，讓它針對眼前的問題死嗑到底。
這種 Test-Time Training 的潛力，可能比想像中更巨大。

![TTT-Discover](/images/TTT-Discover.png)
