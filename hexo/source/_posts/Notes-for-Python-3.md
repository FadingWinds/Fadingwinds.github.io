---
title: 'Notes for Python 3'
date: 2019-10-29 19:35:33
author: FadingWinds
img:
categories: Python
tags: 
	- notes
	- updating
summary: Personal notebook for anything with python 3. For me, even the smallest thing is worth writing down. 
top:
hidden:
password:
---
### Algorithms / Data structures
#### Easy Ways
- Remember to use lambda for simple functions and use `[x for x in statement if statement]` for iterating.
#### Possible Mistakes
- `list.append()` will only return **None**, which indicates it has successfully complete the process. Use `list` to get to the changes.
- Things like a `range(0,n)` do NOT include n but do include 0.


### Machine Learning / Data Science
#### Package
- *"nltk"* is a useful package for natural language processing. The functions include tokenize (divide a sentence into words), strip stop words and punctuations, lemmatize (convert different forms of a word into one, e.g. run, ran, running, runs → run).
- The famous *"sklearn"*.
#### Frequently Encountered
- Do not iterate through rows of a *pandas* dataframe unless really necessary. Usually, just define a function and use `.apply(func)` instead. No parameter needed when calling.
- If want to make changes to an existing *pandas* dataframe, remember to write `inplace = True`.
- Combine *pandas* dataframes: `pandas.concat()`
- `pandas.read_csv()` is good for loading a data file. The "csv" could be replaced with other type of file.
- A *pandas* object could be converted to a numpy array using `to_numpy()`. 

