---
title: "Kaggle Cheat sheet"
date: 2020-03-30T23:57:53+09:00
draft: true
categories:
- kaggle
tags:
- Data science
- kaggle
keywords:
- kaggle
- EDA
---

# Kaggle Cheat Sheet

## 特徴量生成

### ワンホットエンコーディング

```python
train = pd.get_dummies(train, columns=['foo', 'bar'])
```

