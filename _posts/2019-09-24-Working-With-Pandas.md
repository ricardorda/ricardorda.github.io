---
layout: posts
title: "Working with Pandas"
date: 2019-09-24
tags: [pandas, python]
author_profile: true
excerpt: "Pandas commands to manipulate files"
published: false

---

```python
import numpy as np
import pandas as pd

testDict = {
'name': [np.nan, 'John', 'Peter', 'Charles', 'Richard', 'Paul'],
'heights': [1.9, np.nan, 1.8, 1.7, 1.56, 1.81],
'weight': [80, 70, np.nan, 90, 80, 70],
'age': [15, 20, 25, 40, 34, 44]}

df = pd.DataFrame(testDict)
print(df)

# *** WORKING WITH NULL VALUES ***
# Identifying null values
print(df.isnull())

# Identifying the columns with null values
print(df.loc[:, df.isnull().any(axis=0)])

# Identifying the rows with null values
print(df[df.isnull().any(axis=1)])

# Identifying the columns/rows with null values
print(df.loc[df.isnull().any(axis=1), df.isnull().any(axis=0)])

# Delete all rows that contains null values
df.dropna(inplace=True)

# Delete rows if specific columns have null values
df.dropna(subset=['name', 'heights'], inplace=True)

# Delete rows if all columns have null values
df.dropna(how='all')

# Delete rows if at least 2 columns are null
df.dropna(thresh=2)

# Delete columns that have null rows
df.dropna(axis='columns')



```

| Command | Description |
| --- | --- |
| git status | List all new or modified files |
| git diff | Show file differences that haven't been staged |
| git diff 3 | 2 Show file differences that haven't been staged |

