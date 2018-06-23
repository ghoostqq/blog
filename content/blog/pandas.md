---
title: "Pandas"
date: 2018-06-03T01:49:26+09:00
draft: true
---

```python
df = pandas.read_csv('path_to/some.csv')

df.info()
df.describe()
df.describe(include=['O'])
```

##### [Analyze by pivoting features](https://www.kaggle.com/startupsci/titanic-data-science-solutions)
```python
df[['Pclass', 'Survived']].groupby(['Pclass'], as_index=False).mean().sort_values(by='Survived', ascending=False)
```


```python
series.apply(lambda x: x * 2)
for index, row in df.iterrows():
  pass
```
