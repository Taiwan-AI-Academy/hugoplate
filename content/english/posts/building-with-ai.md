---
title: "Building with AI"
description: "Notes on keeping small projects shippable while experimenting with AI tooling."
date: 2024-10-01T09:00:00Z
image: "/images/image-placeholder.png"
categories: ["AI", "Product"]
author: "John Doe"
tags: ["workflow", "prompting"]
draft: false
---

AI features are only useful when they actually ship. Keep the scaffolding light, prefer smaller feedback loops, and bias toward automation that is observable.

### What worked

- Treat prompts like code; track them next to the feature they support.
- Ship a small guardrail first, then widen scope with telemetry.
- Make rollback easy so experiments do not stall delivery.

### What hurt

Heavy review gates for prototype prompts. Replace with post-deploy auditing that is easy to run.
