---
title: "Keeping Hugo Content Organized"
description: "Folder tricks and front matter defaults that keep a growing site maintainable."
date: 2024-05-18T14:00:00Z
image: "/images/image-placeholder.png"
categories: ["Hugo", "Content"]
author: "William Jacob"
tags: ["hugo", "content"]
draft: false
---

Use section folders to scope list templates, and keep shared taxonomies lean. When adding a new content type, start with a dedicated `_index.md` that documents how to contribute.

Hugo will pick up any Markdown file you drop into this folder. Keep filenames kebab-cased, include dates for sorting, and prefer relative image references so local builds mirror production.
