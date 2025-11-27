---
title: "Data Onboarding Checklist"
description: "A quick checklist before connecting a new source so dashboards stay trustworthy."
date: 2024-09-12T12:00:00Z
image: "/images/image-placeholder.png"
categories: ["Data", "Guides"]
author: "Sam Wilson"
tags: ["etl", "quality"]
draft: false
---

Before wiring a new feed into production, walk through this short list:

1. Confirm ownership and escalation paths for the producer team.
2. Define latency expectations and document them next to the model using the data.
3. Add lightweight anomaly alerts and keep them close to the on-call rotation.
4. Version the schema and ship a contract test that runs in CI.

If a step feels heavy, automate it instead of skipping it.
