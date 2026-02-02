---
title: "SDPO (Self-Distillation Policy Optimization) 讓 LLM 自我檢討"
description: "SDPO 把環境錯誤訊息轉成每一步的密集評分，讓模型自我蒸餾、收斂更快、準度更高。"
date: 2026-02-02T10:00:00+08:00
image: "/images/sdpo-chemistry-accuracy-response.png"
categories: ["AI", "研究"]
tags: ["SDPO", "Self-Distillation", "Policy Optimization", "強化學習", "RLVR", "Dense Credit Assignment", "LiveCodeBench", "GRPO"]
author: "SDPO"
draft: false
slug: "sdpo-self-distillation-policy-optimization"
---

現在訓練 LLM 很流行用強化學習 RL，特別是像 GRPO 這種方法。通常做法是讓模型試著解題，答對給分、答錯扣分（RLVR）。

但這有個大問題：這就像老師改考卷只打分數不寫評語，模型知道自己錯了，但根本不知道錯在哪個步驟。
既然寫程式時編譯器會噴 error log，這些就是超營養的 Rich Feedback，不拿來用太浪費了。

LLM 其實具備看著錯誤訊息修正的能力（In-context Learning）。與其依賴一個更強的外部模型（Teacher）來教，不如讓模型自己教自己。
當模型收到錯誤（例如 Runtime Error）後，它就變身成 Self-Teacher，回頭去審視自己剛剛產出的答案。
有了錯誤訊息，Self-Teacher 就能精準指出哪幾個 Token 導致了錯誤。

這種把單一分數變成每個步驟的詳細評分 Dense Credit Assignment，就是 SDPO 的精髓。

## SDPO 的運作流程

SDPO 的流程不像傳統 RL 需要另外訓練一個 Reward Model，而是直接利用當前的 Policy：

1. 嘗試：模型針對問題生成一個答案（Student）。
2. 回饋：環境回傳結果，比如程式碼的報錯訊息，或是測試結果的輸出。
3. 反思：把題目 + 爛答案 + 錯誤訊息丟回給同一個模型（Self-Teacher）。
4. 蒸餾：Self-Teacher 會根據這些資訊，重新計算原本答案中每個步驟的機率分佈，告訴 Student 哪裡該加強、哪裡該修正。接著透過梯度下降，讓 Student 去模仿 Self-Teacher。

## 效果有多猛？

這套方法在 LiveCodeBench（程式解題）上效果非常顯著：

- 效率暴增：SDPO 的訓練收斂速度比 GRPO 快了 4 倍。
- 準度提升：最終準確率超越了 GRPO，甚至贏過 Claude Sonnet 4。
- 解難題更強：在測試階段，針對那些超難題目，SDPO 發現解法的速度比單純一直重試 Best-of-k 快了 3 倍。

這證明了不用依賴外部大神模型，只要懂得善用環境回饋做「自我蒸餾」，模型就能學得又快又好。
對於那些有明確驗證機制（如寫 Code、算數學）的場景，這絕對是未來的訓練趨勢。

## 開源專案

- https://github.com/lasgroup/SDPO

![SDPO](/images/sdpo-chemistry-accuracy-response.png)
